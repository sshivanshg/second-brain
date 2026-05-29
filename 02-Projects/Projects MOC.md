---
created: 2026-05-24
type: moc
tags: moc, projects
---

# 🗺️ Projects MOC — everything I'm building

> Map of every real project. One node per repo / cluster. The repo is source-of-truth for code; this vault is source-of-truth for *thinking*.

## 🚀 Active flagships

| Project | One-liner | Repo |
|---------|-----------|------|
| [[Arth Saathi]] | AI-first OS for India's 5–50 employee shops — payroll, attendance, GST, fintech | `~/Documents/arth` |
| [[DevHelp]] | Clone OSS repo → working dev env. Deterministic, honest. (v0.4) | `~/devhelp` |
| [[LexVault]] | AI contract lifecycle — draft, review, sign, RAG with line-level citations | `~/lexvault` |
| [[TinyTales StoryGlow]] | Kid's photo + theme → personalized illustrated storybook + animated video | `~/Documents/aistorygen` |
| [[Agora]] | Self-hostable OSS multi-agent AI debate platform. BYOK. LangGraph + Hono. | `~/agora` |
| [[Brixloop]] | "Build and Ship Faster" — digital studio marketing site | `~/Documents/Brixloop` |

## 🔬 College — research / capstone

| Project | One-liner |
|---------|-----------|
| [[RAMT — Regime-Adaptive Multimodal Transformer]] | 3rd-year capstone: Chronos-T5 + LoRA + HMM on Indian equities. Honest "HMM is conditional insurance" framing. |
| [[SpatiaLaw — WiFi CSI presence detection]] | 99.4% accuracy WiFi-CSI human-presence classifier. |

## 📈 Quant / ML

| Project | One-liner |
|---------|-----------|
| [[StP — Single-stock XGBoost prediction]] | Walk-forward next-day return prediction |
| [[algo_dev — Indian stock market trading algorithm]] | Earlier algo with TA + ML |
| [[CNN — CIFAR-10 mini project]] | Hand-rolled `SimpleNet` CNN |
| [[Qm Quant Research]] | NIFTY backtest framework |

## 🛠️ Builds + product

| Project | One-liner |
|---------|-----------|
| [[Kidbee — AI storytelling for kids]] | AI storytelling platform (sister to TinyTales) |
| [[HealthPro — QR code platform]] | Dynamic QR + tracking + analytics |
| [[BioPay — microservices payment backend]] | Microservices wallet/transaction backend |
| [[zenova — health tracker backend]] | Health/lifestyle logger backend |
| [[College Appointment System]] | Student-professor appointment booking |
| [[Design Flow Studio]] | Design workflow tool |
| [[e-Factory CMS]] | TS monorepo for content/media. Semaphores not job queue. |
| [[Devops Ecom]] | Personalized e-commerce + DevOps pipeline |
| [[OS File Organizer]] | Electron + Vite + React desktop app |
| [[LFX Organizations Dashboard]] | LF program participation history |
| [[Claw Code rewrite]] | Studying clean-room Python rewrite of Claude Code harness |
| [[Munim]] | Brand + design assets (precursor of Arth Saathi) |

## 👗 Client / personal

| Project | One-liner |
|---------|-----------|
| [[Boutique web projects]] | Client boutique e-commerce sites |
| [[Twitter clone]] | First-year Twitter UI clone |
| [[Mother's Day gift site]] | Family craft project |
| [[Dev Spirit Animal]] | 5-question quiz → shareable "dev spirit animal" card. Built to go viral. |
| [[git wrapped — GitHub commit roast generator]] | Paste a GitHub username → brutally-funny roast of your commit history. Shareable card + OG image. Built to go viral. |
| [[Portfolio site]] | Personal portfolio |

## 🧭 Browse by category / audience
**[[_Categories index]]** — every project tagged by tech + purpose, auto-grouped:
- By tech: [[Full Stack projects]] · [[Backend projects]] · [[Frontend projects]] · [[AI ML projects]] · [[Quant projects]] · [[Systems projects]] · [[Dev Tools projects]]
- By purpose: [[Startup projects]] · [[Freelance projects]] · [[Research projects]] · [[College projects category]] · [[OSS projects]] · [[Personal projects]]

## 📚 Aggregations
- 🎓 **[[College projects MOC]]** — academic project map (incl. RAMT, SpatiaLaw, all coursework)
- 📝 **[[Coursework + lab assignments]]** — the long-tail class repos (Todos, CRUDs, Weather APIs, etc.)
- 🌐 **[[OSS contributions]]** — forks across CNCF, Bitcoin, GSoC, LFX, openSUSE, AI dev tools
- 📦 [[Experiments and small repos]] — scratch & abandoned

## 🧠 Reusable beliefs powering these projects
- [[Bharat shop owner IS the HR department]]
- [[Voice and Hinglish are the UX not a translation]]
- [[Confirm before write — AI mutations need human approval]] — Arth Saathi, LexVault, TinyTales
- [[Every salary slip is a billboard — K-factor design]]
- [[Free + free = no revenue]]
- [[Compliance integration is a structural moat]]
- [[Daily-active surfaces own monthly-active surfaces]]
- [[Vertical SMB-OS earns most revenue from embedded fintech]]

## 📊 Live dataview — all projects sorted by recency
```dataview
TABLE WITHOUT ID file.link AS Project, status, github
FROM "02-Projects"
WHERE type = "project" OR type = "project-moc" OR type = "project-list"
SORT file.mtime DESC
LIMIT 30
```
