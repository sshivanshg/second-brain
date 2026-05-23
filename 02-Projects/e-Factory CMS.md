---
created: 2026-05-24
status: active
type: project
tags: project, cms, monorepo
categories: [fullstack, backend]
audience: [freelance]
repo: /Users/shivanshgupta/Documents/CMS
---

# 🏭 e-Factory (CMS)

> **One-liner:** TypeScript monorepo for **content and media workflows** — React web app, Express API, background jobs, object storage, shared schemas. One codebase for authoring, processing, delivery.

**Repo:** `/Users/shivanshgupta/Documents/CMS` · **Mgr:** pnpm + Turborepo

## Stack
- **Frontend:** React (web app)
- **API:** Express
- **Background work:** API + optional worker process (semaphore-based concurrency, not a job queue)
- **Storage:** AWS S3
- **Edge:** CloudFront
- **Hosting:** EC2 + ECR
- **Schemas:** shared package across apps

## Key architectural choice — semaphores, not job queue
Documented in repo: workers gate concurrency via semaphores instead of running a separate Redis/RabbitMQ job queue. Fewer moving parts, simpler ops, sufficient throughput for the content workflow scale.

## Sources
- README: `/Users/shivanshgupta/Documents/CMS/README.md`
- Worker architecture: `/Users/shivanshgupta/Documents/CMS/docs/WORKER-ARCHITECTURE.md`
- AWS deploy: `/Users/shivanshgupta/Documents/CMS/docs/AWS.md`
- Agents: `/Users/shivanshgupta/Documents/CMS/AGENTS.md`
- Decisions log: `/Users/shivanshgupta/Documents/CMS/DECISIONS.md`
- Cursor / Claude / Gemini config: `CLAUDE.md`, `GEMINI.md`

## Connections
- [[Projects MOC]]
