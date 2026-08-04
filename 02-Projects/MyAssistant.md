---
created: 2026-08-04
updated: 2026-08-04
status: active
type: project
tags: project, agent, telegram, personal-os
categories: [fullstack, ai-ml, systems]
audience: [personal]
repo: /Users/shivanshgupta/Projects/MyAssistant
---

# 🤖 MyAssistant

> **One-liner:** Self-owned Telegram operator for real-world tasks — property, freight, research, calendar, email — with approval gates and spend caps.

**Repo:** `/Users/shivanshgupta/Projects/MyAssistant` · Docs: `docs/PRD.md`, `docs/architecture.md`, plan `docs/plans/2026-07-23-myassistant-implementation.md`

## What you get
- Telegram bot (text + voice), allowlist + `/pause`
- OpenAI agent tools: memory, tasks, search, browse, email/calendar
- **Approval-gated** Gmail send + Calendar events
- Playwright worker for page extract
- Postgres memory/tasks/audit + Redis queue
- n8n daily + research digests
- Monthly OpenAI spend cap

## 🧱 Stack
- Monorepo: `apps/` + `packages/` + `infra/` (Docker Compose + Caddy)
- n8n :5678 · API health via Caddy · Postgres host port 55432

## 📝 Log
### 2026-07-23
- Implementation plan documented; ops guides for Telegram webhook, Google OAuth, VPS deploy
- No git remote in inventory — local Docker-first personal system

### 2026-08-04 — vault catch-up
- Added to vault as personal agent OS; related belief: [[Confirm before write — AI mutations need human approval]]

## ✅ Open tasks
- [ ] Full VPS deploy per `docs/ops/vps-deploy.md`
- [ ] Tighten allowlist + spend alerts
- [ ] Optional private git remote

## 🔗 Connections
- [[tg-promo]] (Telegram ops, different purpose)
- [[Claw Code rewrite]] / agent harness interest
- [[Projects MOC]]
