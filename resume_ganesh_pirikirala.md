# Ganesh Pirikirala
**Full Stack & Distributed Systems Engineer**  
Hyderabad, India | +91-7659869814 | [ganesh.pkl08@gmail.com](mailto:ganesh.pkl08@gmail.com)  
[GitHub: webdeveloper-fresher32](https://github.com/webdeveloper-fresher32) | [LinkedIn: ganesh-p](https://www.linkedin.com/in/ganesh-p-b711a924a) | [Portfolio](https://ganesh-full-stack-developer-portfolio.vercel.app/)

---

## 🎯 Professional Summary
Software Engineer with 3 years of experience specializing in **high-concurrency backend architecture, distributed systems, and AI-enabled platforms**. Proven expertise across **Node.js, TypeScript, Java (Spring Boot), Python (FastAPI), React.js, and Next.js**. Empirically scaled single-host microservices to **20,000 concurrent Virtual Users (VUs) sustaining 7,319.07 RPS with 1.42ms p(95) tail latency and 0.00% error rate** via a two-tier caching paradigm and OS kernel socket tuning. Strong background in **PostgreSQL, Redis, MongoDB, Qdrant Vector DB, Docker, CI/CD, and Local RAG/MCP AI agents**.

---

## 🛠️ Technical Skills Matrix

- **Languages:** TypeScript, JavaScript (ES6+), Java (21 / 17), Python, Kotlin, SQL, HTML5, CSS3
- **Backend & APIs:** Node.js, Express.js, Spring Boot, Spring AI, FastAPI, RESTful APIs, WebSockets, Microservices, gRPC
- **Frontend & Mobile:** React.js, Next.js 16 (App Router), Android SDK, Jetpack Compose (Material 3), Tailwind CSS, Material UI, Redux Toolkit
- **Databases & Caching:** PostgreSQL, MongoDB, Redis (L2 Caching, Atomic Lua, Pub/Sub, SETNX Distributed Locks), Qdrant Vector DB, SQLite / Room Database, HikariCP
- **Systems & DevOps:** Linux/macOS Kernel Socket Tuning (`somaxconn`, `ulimit`), Docker, Docker Compose, Git, GitHub Actions, CI/CD, AWS, Azure, Grafana k6 Load Testing
- **AI & Architecture:** Local RAG Pipelines, Ollama, LangChain, Model Context Protocol (MCP), System Design (LLD / HLD), SOLID Principles, Clean Architecture

---

## 💼 Professional Experience

### **Cognitivo** — Full-Stack Software Developer  
*Aug 2025 – Present | Hyderabad, India*
- **SaaS Subscription Billing:** Architected multi-tenant SaaS backend microservices with **Stripe subscription billing**, engineering strictly idempotent webhook handlers with event ID deduplication and PostgreSQL transaction locks to eliminate payment processing race conditions.
- **AI Platform Assistant:** Developed an in-app AI conversational assistant using **LangChain and Retrieval-Augmented Generation (RAG)**, enabling users to schedule meetings, query platform documentation, and execute platform workflows via natural language.
- **Asynchronous Meeting Intelligence:** Engineered an asynchronous **Meeting Intelligence Pipeline** with **Python and FastAPI** to ingest Zoom and Teams recordings, automate speech-to-text transcription, and connect extracted insights to a dynamic **Knowledge Graph**.
- **Developer Observability & Cloud Deployment:** Built a developer KPI dashboard in React, TypeScript, and FastAPI integrating GitHub webhooks for engineering telemetry, and deployed containerized microservices to **Azure** via automated CI/CD pipelines.

### **Contenterra Software Private Limited** — Associate Software Developer  
*Jul 2024 – Jun 2025 | Hyderabad, India*  
*(Relieving Letter confirmed: 9th Sep 2024 – 30th Jun 2025)*
- **Enterprise Portal Engineering:** Engineered responsive claim-processing interfaces and onboarding workflows for an enterprise insurance platform using **Next.js, TypeScript, and Material UI**, significantly reducing form drop-off rates.
- **High-Throughput Payments & Microservices:** Developed and scaled **Spring Boot** backend REST APIs and MongoDB schemas integrated with **Razorpay**, supporting **10,000+ concurrent active users** with transactional data consistency.
- **Dashboard Optimization:** Built analytics dashboards integrating **DND-kit** and **Nivo Charts**, designing custom memoized React hooks and route-level code splitting to eliminate redundant DOM re-renders.

### **Northgaze Inc** — Software Development Engineer (Freelance)  
*Jan 2025 – Aug 2025 | Remote*
- **Visual Workflow Builder:** Engineered an interactive canvas workflow builder using **React Flow, Next.js, and TypeScript**, orchestrating real-time node topology state over **WebSockets** with backend streaming endpoints, reducing client latency by **30%**.

---

## 🚀 Featured Engineering Projects

### **1. High-Concurrency Distributed Backend — Node.js, Redis, MongoDB**  
[GitHub Repository](https://github.com/webdeveloper-fresher32/high-concurrency-nodejs-backend)
- **Empirical Scale to 20,000 VUs:** Engineered an empirical benchmark scaling Node.js from a single-core bottleneck to **20,000 concurrent Virtual Users (VUs)** sustaining **7,319.07 RPS**, **0.00% error rate**, and **1.42ms p(95) tail latency** across **2.87 Million requests** on a single machine.
- **Two-Tier Caching Paradigm:** Implemented an **L1 In-Process V8 Heap Cache** (5-second micro-TTL, 98% hit rate at 1 microsecond zero-network latency) and **L2 Shared Redis Cache** backed by distributed mutex locks (`SETNX` + atomic Lua scripts) to eliminate cache stampedes and thundering herds.
- **OS Kernel Socket & Cluster Tuning:** Configured macOS/Linux kernel network limits (`somaxconn: 4096`, `ulimit: 65536`) with Node.js cluster master-worker round-robin socket balancing and dual-layer covered compound MongoDB indexing (`{ status: 1, category: 1, price: 1, _id: 1 }`), eliminating `COLLSCAN` CPU bottlenecks.

### **2. DocsRAG — Local AI Knowledge Assistant & MCP Platform**  
[GitHub Repository](https://github.com/webdeveloper-fresher32/AI-PDF-Knowledge-Assistant)
- **Local RAG Architecture:** Built an enterprise local RAG document intelligence engine in **Java 21, Spring Boot 4.1.1, Spring AI 2.0.1, and Qdrant Vector DB**, parsing documents via Apache PDFBox and generating 768-dimensional embeddings offline using **Ollama** (`nomic-embed-text`).
- **MCP Tool Calling & Token Streaming:** Engineered token-streaming answers via **Server-Sent Events (SSE) / Reactor Flux** and integrated 3 **Model Context Protocol (MCP)** tools (Calculator, Filesystem Inspector, Weather) for dynamic tool-calling with context citations in a **Next.js 16 (App Router + Tailwind v4)** interface.

### **3. FocusLock — Anti-Distraction Screen Time Blocker (Android)**  
[GitHub Repository](https://github.com/webdeveloper-fresher32/Focus-Lock) | [Download APK](https://github.com/webdeveloper-fresher32/Focus-Lock/releases/latest)
- **Active App Interception:** Engineered an offline-first Android productivity app in **Kotlin 2.0.0, Jetpack Compose (Material 3), and Clean Architecture/MVVM**, actively intercepting unauthorized app launches in real-time via an Android **AccessibilityService** and full-screen blocking overlay.
- **Resilient Epoch Timers & Local Analytics:** Implemented persistent target-epoch session timers and deflection analytics backed by **Room SQLite** and **UsageStatsManager**, surviving process termination, aggressive OS background killers, and device reboots (`BOOT_COMPLETED`).

### **4. Jobs-Automation — Autonomous Multi-Portal Application Engine**  
[GitHub Repository](https://github.com/webdeveloper-fresher32/JOBS-AUTOMATION)
- **Parallel Browser Orchestration:** Designed an asynchronous pipeline across 9 job portals (Naukri, LinkedIn, Foundit, Hirist, Instahyre, Wellfound, Glassdoor, Apna, Uplers) featuring a **6-worker parallel Chromium cluster** and anti-detection stealth spoofing.
- **AI Screening & Telemetry:** Integrated LLM dynamic form reasoning (Gemini/Groq) for automated questionnaire completion, automated `launchd` background execution with lid-closed caffeinate support, and Resend API HTML reports with battery telemetry.

### **5. SkillVault — Developer Learning Operating System**  
[GitHub Repository](https://github.com/webdeveloper-fresher32/skillvault-website) | [Live Demo](https://dev-skillvault.vercel.app/)
- **Curricula Ingestion Engine:** Engineered a full-stack learning platform using **Spring Boot, Next.js 16, PostgreSQL, and Docker**, indexing 30+ engineering curricula via a Git-backed reactive markdown ingestion engine with sub-100ms catalog REST APIs via tuned **HikariCP** connection pooling.
- **JVM Telemetry:** Designed an operational JVM telemetry dashboard monitoring connection pool health, query latency (3ms), and memory heap allocation.

### **6. Production-Grade Custom API Gateway**  
[GitHub Repository](https://github.com/webdeveloper-fresher32/Custom-Api-Gateway)
- **Radix Router & Rate Limiting:** Built a high-throughput API gateway from scratch in **Node.js & TypeScript** featuring a custom **Trie-based Radix router**, sliding window and token bucket rate limiters, reverse proxy streaming, and a self-healing **Circuit Breaker**.

---

## 🎓 Education
- **Bachelor of Technology (B.Tech) in Electronics and Communication Engineering (ECE)**  
  *Chaitanya Bharathi Institute of Technology (CBIT), Hyderabad* | **2020 – 2024** | **CGPA: 8.0 / 10**
- **Higher Secondary Certificate (MPC)**  
  *Sri Gayatri Junior College, Hyderabad* | **2017 – 2019** | **Score: 910 / 1000**
