---
created: 2026-05-24
status: active
type: project
tags: project, oss, dashboard
categories: [fullstack]
audience: [oss]
repo: /Users/shivanshgupta/Documents/lfxorganizations
---

# 📊 LFX Organizations Dashboard

> **One-liner:** Dashboard to view past records of organizations that have participated in **Linux Foundation LFX programs** — helps contributors discover and decide where to contribute.

**Repo:** `/Users/shivanshgupta/Documents/lfxorganizations`

## Stack
- Node 22
- Docker + Docker Compose
- `service/` — backend
- `ui/` — frontend

## Run
```bash
docker compose build --no-cache
docker compose up        # backend
cd ui && cp .env.example .env && npm i && npm run dev   # ui
```

## Sources
- README: `/Users/shivanshgupta/Documents/lfxorganizations/README.md`
- docker-compose.yml at repo root

## Connections
- [[Projects MOC]]
