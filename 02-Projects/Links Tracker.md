---
created: 2026-08-04
updated: 2026-08-04
status: studying
type: project
tags: project, cloudflare, interview-prep, learning
categories: [fullstack]
audience: [personal]
repo: /Users/shivanshgupta/Projects/links-tracker
---

# 🔗 Links Tracker

> **One-liner:** Full-stack link tracking + geo routing on Cloudflare (Workers, D1, Durable Objects, Queues) — learning / interview-prep monorepo more than a shipped product.

**Repo:** `/Users/shivanshgupta/Projects/links-tracker` · Last major activity: mid-June 2026 (interview-prep docs + architecture guides)

## 🎯 Goal
Internalize Cloudflare’s full stack by building a real link-analytics platform: create links, geo-route, live click dashboard, Better Auth.

## 🧱 Stack
- **Monorepo:** pnpm workspaces
- **Frontend:** React 19 + TanStack Router/Query + Tailwind + Radix + Vite
- **Backend:** Cloudflare Workers + Hono + D1 + Durable Objects + Queues + Workflows
- **Auth:** Better Auth (evolved from Google OAuth → email/password in history)
- **Realtime:** Socket.io / DO patterns
- **Shared:** `packages/data-ops` (db, auth, zod)

## 📁 Layout
```
apps/user-application   # React dashboard + worker for tRPC
apps/data-service       # Hono API + DOs + queues + workflows
packages/data-ops       # schema, better-auth, zod
interview-prep/         # architecture drawing guides
```

## 📝 Log
### 2026-08-04 — vault catch-up
- Present on disk under Projects; not in vault. Treated as **studying** (interview-prep folder + architecture guide commits), not an active flagship.
- Related *shipped* product for short links is [[Deskzy]], not this repo.

## 🔗 Connections
- [[Deskzy]] — production short-link product (different stack: Next/OpenNext + KV + Supabase)
- [[Projects MOC]]
- [[Experiments and small repos]]
