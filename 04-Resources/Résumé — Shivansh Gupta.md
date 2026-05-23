---
created: 2026-05-24
type: resume
tags: resume, output
generated_from: vault project notes
---

# Shivansh Gupta

> Founder & full-stack / AI engineer. Building [[Arth Saathi]] — an AI-first operating system for India's small businesses. Strong across product, applied ML/AI, and systems.

**GitHub:** github.com/sshivanshg · **Portfolio:** see [[Portfolio site]]

> 🤖 *This résumé is generated from the vault. To refresh it, tell Claude: "regenerate my résumé from the project notes." Edit contact details below by hand.*

---

## Summary
Full-stack and AI engineer who ships end-to-end — from vertical SaaS with real users and a fundraise underway, to honest, rigorous ML research. Default stack: Next.js + TypeScript + PostgreSQL/Prisma + Claude/LangGraph. Comfortable in Python for ML/quant and Go/Rust/C for systems & OSS.

---

## Flagship work

### Arth Saathi — Founder *(2025–present)*
AI-first, Hindi/Hinglish operating system for India's 5–50 employee shops (attendance, payroll, GST, embedded fintech). Live pilot; pre-seed fundraising (USD 1.5M).
- Built a pnpm monorepo: Next.js 14 web, Expo/React Native mobile, Fastify/Nest backend, Prisma/Postgres on RDS.
- Designed a **confirm-before-write** LangGraph agent architecture (every AI mutation gated by human approval) — Claude Opus tool-calling.
- Authored full PRD, GTM brief, and investor deck; thesis built on embedded-fintech monetization over subscription.

### DevHelp — Creator *(OSS CLI, v0.4)*
"Clone an OSS repo, get a working dev environment." Deterministic dev-env automation.
- Detects runtimes, picks package manager from lockfile, runs install, copies env files, generates Prisma client.
- Stress-tested on 20 real OSS repos: improved clean-setup rate from 25% → 75% across Node/Python/Rust/Go.

### LexVault — Builder
AI contract lifecycle management with **line-level citation grounding**. Next.js 14 + Express + Neon Postgres (pgvector) + Claude. Hybrid RAG, AI review, real-time co-editing (Liveblocks), DAG workflows.

### Agora — Creator *(OSS)*
Self-hostable multi-agent AI debate platform. LangGraph state machine, Hono API, Postgres + pgvector, BYOK with AES-256-GCM key encryption.

---

## Research

### RAMT — Regime-Adaptive Multimodal Transformer *(3rd-year capstone)*
Indian equity alpha via foundation-model fine-tuning. Compared custom transformer vs **Chronos-T5 + LoRA** vs HMM regime gating across multiple market windows.
- Finding: Foundation-Only Chronos-LoRA hit Sharpe 1.34 (2024–26); HMM gating cut 2008 drawdown by 9.4pp — *"HMM is conditional insurance, not always-on alpha."*
- IEEE-style 10-page report; full ablation; Dockerized + Streamlit dashboard.

### SpatiaLaw — WiFi CSI Human Presence Detection
Privacy-preserving alternative to cameras using WiFi Channel State Information.
- **99.4% test accuracy** (Random Forest) via Variance/Entropy/Doppler feature engineering on Intel 5300 CSI. Real-time Streamlit dashboard.

---

## Selected projects
- **TinyTales/StoryGlow** — photo + theme → personalized illustrated storybook + animated video (Fal.ai PuLID, Grok Imagine, ElevenLabs, FFmpeg).
- **e-Factory CMS** — TS + Turborepo content/media platform; semaphore-based concurrency over a job queue.
- **Quant ladder** — algo_dev → StP (XGBoost, walk-forward) → Qm (NIFTY framework) → RAMT.
- See the full catalog in [[Projects MOC]] and [[_Categories index]].

---

## Open source
Contributor / studied across **CNCF** (Cilium, Kyverno, Antrea, Prometheus Operator), **Bitcoin Core** (BOSS 2026 challenges — fuzzing, signet wallet), **GSoC/LFX**, and openSUSE. Details in [[OSS contributions]].

---

## Technical skills
*(auto-derived from [[Tech stack map]])*
- **Languages:** TypeScript, Python, JavaScript, Go, Rust, C/C++, Swift, Dart
- **Frontend:** Next.js, React, React Native/Expo, Tailwind, shadcn/ui, Electron
- **Backend:** Fastify/Nest, Express, Hono, microservices
- **Data:** PostgreSQL, Prisma, pgvector, Neon, MongoDB, Drizzle
- **AI/ML:** Claude/Anthropic, LangGraph, Vercel AI SDK, RAG, Chronos-T5 + LoRA, XGBoost, HMM, PyTorch, scikit-learn
- **Infra:** Vercel, AWS, Azure Container Apps, Cloudflare R2, Docker, Terraform, EAS

---

## Education
*(fill in — degree, institution, graduation year)*

## Contact
*(fill in — email, phone, LinkedIn)*

---
*Sources: every project note in this vault. Regenerate after major milestones.*
