# Learning Log

> Last updated: 2026-04-12 | Append-only — every learning session gets logged

## Format
```
### YYYY-MM-DD: [Topic]
- **Source:** Where did you learn this? (book, course, article, incident)
- **Time spent:** How long?
- **Key takeaway:** One sentence that captures the main insight
- **How I'll use it:** Concrete application to work/career
```

---

### 2026-04-10: Quarkus Framework (Java Cloud-Native)
- **Source:** Quarkus in Action (Manning MEAP) — Chapters 1-11
- **Time spent:** ~6 hours reading + 2 hours building app
- **Key takeaway:** Quarkus uses build-time processing to create fast-startup, low-memory Java apps perfect for Kubernetes. Dev Services auto-provision test databases.
- **How I'll use it:** Understanding Java microservices deployed on our AKS clusters. Could build CAMARA API gateway in Quarkus.
- **Artifact:** `/quarkus-notes/` (11 chapter summaries), working app on port 8090

### 2026-04-11: Hermes Agent vs OpenClaw Comparison
- **Source:** Hermes Agent repo (Nous Research), hands-on installation
- **Time spent:** ~3 hours
- **Key takeaway:** Hermes' GAPA (General-purpose Agent Performance Architecture) auto-generates lessons from task history. OpenClaw doesn't have this natively — but self-improvement cron jobs close ~90% of the gap.
- **How I'll use it:** Self-improvement cron system already running. GAPA concept inspires future agent development.
- **Artifact:** Hermes v0.8.0 installed, 3 cron jobs created

### 2026-04-11: PPTX Parsing Without python-pptx
- **Source:** Necessity (no pip/python-pptx available)
- **Time spent:** 30 min figuring it out
- **Key takeaway:** PPTX files are ZIP archives containing XML. Unzip + parse `ppt/slides/slide*.xml` directly. No dependencies needed.
- **How I'll use it:** Parse any Office document at work without installing anything.
- **Artifact:** Extracted 62 slides from 2 Lev Selector AI Updates presentations

### 2026-04-11: Multica Architecture (Agent Orchestration)
- **Source:** Building Multica end-to-end (Go backend, PostgreSQL, daemon, CLI)
- **Time spent:** ~8 hours
- **Key takeaway:** Multica is NOT an AI — it's an orchestrator. Task board + daemon that bridges to local agent CLIs (Claude Code, Codex, OpenClaw). The CLIs are the brains.
- **How I'll use it:** Understanding how to build agent orchestration platforms. Relevant to building telecom API intelligence platform.
- **Artifact:** Full working demo with 5 agents, 6 issues, Claude Code runtime executing tasks

### 2026-04-11: Incident Knowledge Graphs for SRE
- **Source:** Building incident-knowledge-graph repo
- **Time spent:** ~4 hours
- **Key takeaway:** Zero-dependency Markdown knowledge graphs can replace complex graph databases for incident management. Copilot reads them directly.
- **How I'll use it:** Template for building knowledge graphs at tier-1 carrier. Manager pitch material.
- **Artifact:** github.com/rohitsalesforce132/incident-knowledge-graph

### 2026-04-12: Knowledge Graph Deep Dive (Brainstorming)
- **Source:** Self-directed thinking (30+ ideas generated)
- **Time spent:** ~3 hours
- **Key takeaway:** The gap in the market is not "better monitoring" — it's "understanding." Nobody has built an infrastructure knowledge graph that understands WHY things are connected, not just THAT they're connected.
- **How I'll use it:** Patent applications, startup vision, career direction
- **Artifact:** 20+ knowledge graph project ideas, 10 CAMARA patent ideas, 7 "world-changing" visions

### 2026-04-12: Karpathy's LLM Wiki Pattern
- **Source:** https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
- **Time spent:** 2 hours implementing
- **Key takeaway:** Instead of RAG (stateless retrieval every time), have the LLM maintain a persistent wiki that compounds. INGEST sources → wiki grows. QUERY wiki → answers get saved back. LINT → stays healthy.
- **How I'll use it:** Built sre-wiki for CAMARA ops. Career wiki for professional growth.
- **Artifact:** github.com/rohitsalesforce132/sre-wiki

---

## Learning Sources to Explore

### Books (Priority Order)
- [ ] **Designing Machine Learning Systems** — Chip Huyen (ML production)
- [ ] **Machine Learning Engineering** — Andriy Burkov (ML fundamentals)
- [ ] **Fundamentals of Data Engineering** — Joe Reis (data pipelines)
- [ ] **Building LLM Apps** — Valentino Gagliardi (LLM applications)
- [ ] **Designing Data-Intensive Applications** — Martin Kleppmann (distributed systems)

### Courses
- [ ] **fast.ai** — Practical deep learning (free, 7 weeks)
- [ ] **Andrew Ng ML Specialization** — Stanford/Coursera (theory foundation)
- [ ] **MLOps Specialization** — Duke/Coursera (production ML)
- [ ] **Azure AI-300 learning path** — Microsoft Learn (cert prep)

### YouTube Channels
- [ ] 3Blue1Brown — Linear Algebra, Neural Networks visual
- [ ] Andrej Karpathy — Neural Networks: Zero to Hero
- [ ] StatQuest — Statistics fundamentals
- [ ] TECH WITH TIM — Python projects

### Certifications (Exam Dates TBD)
| Cert | Target Date | Status |
|------|------------|--------|
| GH-300 (GitHub Copilot) | May 2026 | 📝 Study started |
| AI-300 (MLOps Engineer) | Aug 2026 | 📋 Planned |
| AI-103 (Azure AI Developer) | Nov 2026 | 📋 Planned |
