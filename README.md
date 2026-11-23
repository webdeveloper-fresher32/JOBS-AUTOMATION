# Multi-Platform Autonomous Job Application Pipeline

An enterprise-grade, intelligent daily job application pipeline that searches, filters, auto-fills screening questionnaires via AI, and submits applications across **Naukri**, **LinkedIn**, **Foundit** (Monster), **Instahyre**, **Hirist**, **Wellfound** (AngelList), **Uplers**, **Apna**, and **Glassdoor** (Indeed SmartApply).

Designed to run **autonomously 3 times a day** (8:30 AM, 11:30 AM, 2:30 PM) via macOS `launchd`, fully resilient to running on **battery power with the laptop lid closed**, with persistent deduplication, anti-ban pacing, and responsive email status reports delivered to your inbox.

---

## Daily Schedule & Anti-Ban Architecture

```text
               macOS launchd Daemon (8:30 AM, 11:30 AM, 2:30 PM)
                                     │
                                     ▼
                        scripts/scheduled_runner.sh
  ├── Concurrency Lock (/tmp/jobs_automation_scheduled.lock)
  ├── caffeinate -disu (Awake on battery/AC with lid closed)
  ├── Wi-Fi Auto-Wake (networksetup -setairportpower en0 on) + route ping verification
  ├── Start notification email sent via Resend API
  ├── Profile Activity Refresher (8:30 AM & 2:30 PM: resets "Last Active" timestamp)
  │
  ▼
daily_pipeline.py --parallel (9 Concurrent Headless Browser Workers)
  ├── Naukri Worker     --> 17 applications/run (Protected: 50/day cap, <10 first then <100)
  ├── LinkedIn Worker   --> 17 applications/run (Protected: 50/day cap, <10 first then <100, max 75 attempts)
  ├── Foundit Worker    --> 35 applications/run (High-volume driver)
  ├── Instahyre Worker  --> 30 applications/run (High-volume driver)
  ├── Hirist Worker     --> 25 applications/run (Seniority 0–4 yrs)
  ├── Wellfound Worker  --> 20 applications/run (Top 12 target roles)
  ├── Uplers Worker     --> 20 applications/run (Full modal autofill + verified submit)
  ├── Apna Worker       --> 15 applications/run
  └── Glassdoor Worker  --> 3 applications/run (Restricted: 5/day cap)
                                     │
                                     ▼
LinkedIn Recruiter Outreach (Sends 10 connection notes/day to hiring managers)
                                     │
                                     ▼
Email Status Report dispatched to ganeshpirikirala@gmail.com (Resend API with battery telemetry)
```

---

## Core Rules & Eligibility Standards

### 1. Experience & Seniority Rules
- **Candidate Total Experience**: Evaluated as **3 years**.
- **Seniority Window**: `seniority_floor_years: 0` to `seniority_ceiling_years: 4` (Hard ceiling at 4 years).
- **Preference Order**: Postings requiring **$\le$ 3 years** (0–2, 1–3, 2–3 yrs) are **strictly prioritized** over 4-year postings. Postings requiring 5+ years are automatically filtered out.

### 2. Applicant Count Rules (Naukri & LinkedIn)
- **Tier 1 (Priority 1 — Highest)**: Postings with **fewer than 10 applicants** (`< 10`) are evaluated and applied to first.
- **Tier 2 (Priority 2 — Secondary)**: Postings with **fewer than 100 applicants** (`10 to 99`) are evaluated next.
- **Tier 3 (Fallback)**: Postings with 100 to 500 applicants are evaluated only if needed to hit the run quota.
- **Ceiling**: Any posting with **more than 500 applicants** is skipped (`too many applicants: > 500`).
- **Freshness Window**: Postings within the last **7 days** (`job_freshness_days: 7`).

### 3. Application Quotas & Pacing (Anti-Ban Architecture)
To ensure your primary accounts on **Naukri** and **LinkedIn** are never restricted or banned:
- **Paced Runs**: Capped at **17 applications per run** on Naukri and **17 applications per run** on LinkedIn.
- **3-Cycle Distribution**: $17 \times 3 = 51 \approx \mathbf{50\text{ applications/day}}$ total per platform.
- **Hard Daily Safety Ceiling**: Both platforms dynamically check `remaining_today = max(0, daily_ceiling - applied_today)`. Once 50 is reached, they safely pause for the day.
- **Volume Drivers (Foundit, Instahyre, Hirist, Wellfound, Uplers, Apna)**: Configured with higher volume to ensure your total daily submissions reach **100 to 150 applications daily**.
- **Glassdoor**: Restricted and capped at **3 per run (5 daily max)**.

### 4. Company Blacklist
The following companies are automatically excluded across all platforms via substring matching:
- `Jobgether`
- `Jobtogether`
- `Tenacious HR Solutions`
- `Tenacious HR Solution`

### 5. Uplers Engine Rules
- **Full Form Autofill**: Explicitly fills `current_location` ("Bengaluru"), `preferred_location` ("Bengaluru"), `joining_period` ("Immediately / 0–15 Days"), `preferred_modes` ("Remote"), `total_experience` months ("0"), and formatted joining date.
- **Accurate Metadata Extraction**: Reads exact title and company from the right detail pane (`.jobDetailSection h1`), avoiding badge text strings (e.g. `"Within 3 days"`).
- **Verified Confirmation**: Awaits modal dismissal or success toast before logging success, preventing unconfirmed applications.

### 6. Hardware & Battery Resilience
- **Lid-Closed Operation**: `caffeinate -disu` keeps macOS awake and running through battery cycles.
- **Wi-Fi Route Restoration**: `scheduled_runner.sh` forces airport power on (`networksetup -setairportpower en0 on`) and verifies internet route before launching.
- **Resend Email Reporting**: Uses exponential backoff (3 retry attempts) and embeds real-time battery status (`🔋 Battery: XX%` vs `⚡ AC Power`) into every status email sent to `ganeshpirikirala@gmail.com`.

---

## Platform Quota Overview

| Platform | Per-Run Target | Daily Hard Cap | Strategy |
| :--- | :---: | :---: | :--- |
| **Naukri** | **17** | **50** | **Anti-Ban Protected** (17 per run $\times$ 3 runs = 50/day) |
| **LinkedIn** | **17** | **50** | **Anti-Ban Protected** (17 per run $\times$ 3 runs = 50/day, max 75 attempts) |
| **Foundit (Monster)** | **35** | **60** | **High Volume Driver** |
| **Instahyre** | **30** | **50** | **High Volume Driver** |
| **Hirist** | **25** | **50** | **High Volume Driver** (0–4 yrs) |
| **Wellfound** | **20** | **35** | **Top 12 Target Roles Sweep** |
| **Uplers** | **20** | **30** | **Direct Form Autofill** |
| **Apna** | **15** | **25** | **Active Match Sweep** |
| **Glassdoor** | **3** | **5** | **Restricted** (Low volume) |
| **Total Daily Target** | — | **150 Jobs** | **Guarantees 100–150 applications/day** |

---

## File Structure

```text
jobs-automation/
├── daily_pipeline.py             # Main orchestrator: runs all 9 platforms in parallel
├── naukri.py                     # Unified CLI entry point
│
├── *_apply.py                    # Direct automation engines:
│   ├── naukri_apply.py           # Naukri engine (2-tier applicant sort, 17/run, 50/day cap)
│   ├── linkedin_apply.py         # LinkedIn Easy Apply engine (2-tier sort, 17/run, max 75 attempts)
│   ├── foundit_apply.py          # Foundit (Monster) automation engine
│   ├── instahyre_apply.py        # Instahyre automation engine
│   ├── hirist_apply.py           # Hirist automation engine
│   ├── wellfound_apply.py        # Wellfound startup engine
│   ├── uplers_apply.py           # Uplers talent portal engine
│   ├── apna_apply.py             # Apna automation engine
│   └── glassdoor_apply.py        # Glassdoor + Indeed SmartApply engine
│
├── profile_activity_refresher.py # Algorithmic profile headline refresher for Naukri & LinkedIn
├── linkedin_recruiter_outreach.py# Automated hiring manager connection request sender
│
├── common/
│   ├── answers.py                # AI screening answer routing & typed profile facts
│   ├── profile.py                # Profile loader and schema validator (profile.yaml)
│   ├── external_tracker.py       # Tracks external ATS career portals (external_jobs.csv)
│   ├── stats_tracker.py          # Real-time discovered/skipped/applied counter
│   ├── stealth.py                # Anti-detection stealth launch & humanized delays
│   └── resume_selector.py        # Selects or generates role-targeted LaTeX/PDF resumes
│
├── scripts/
│   ├── scheduled_runner.sh       # Production runner with battery wake & caffeinate
│   ├── send_status_email.js      # Resend API email reporter with retry & battery metrics
│   └── com.ganesh.jobsautomation.plist # macOS launchd schedule definition
│
├── applications_log.csv          # Permanent application database (4,100+ applied jobs)
├── external_jobs.csv             # Daily queue of external company ATS links
└── profile.yaml                  # Local configuration (gitignored, private)
```

---

## Manual Execution & Testing

```bash
# Run unit tests (127 tests)
./.venv/bin/python -m unittest discover -s tests -v

# Run the complete multi-platform pipeline in parallel
./.venv/bin/python daily_pipeline.py --parallel

# Test individual platforms
./.venv/bin/python naukri_apply.py
./.venv/bin/python linkedin_apply.py
./.venv/bin/python uplers_apply.py

# Send status email manually
node scripts/send_status_email.js
```
