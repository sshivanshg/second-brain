---
created: 2026-05-24
status: completed
type: project
tags: project, backend, health, tracking
categories: [backend]
audience: [college]
github: https://github.com/sshivanshg/zenova-backend
language: JavaScript
visibility: public
---

# 🧘 zenova — Health/Lifestyle Tracker Backend

> **One-liner:** Backend API for a lifestyle tracking app — alcohol logs, habits, etc., with JWT auth.

**GitHub:** [sshivanshg/zenova-backend](https://github.com/sshivanshg/zenova-backend)

## Surface (from API doc)
- All endpoints (except `/auth`) require Bearer token
- `POST /alcoholLog` — log alcohol consumption with date, count, context
- (Likely) other lifestyle logs: water, sleep, mood

## Pattern observed
JWT bearer + user-scoped resources. Per-user log entries with `date`, `count`, `context` fields. Standard CRUD over Mongoose / Prisma.

## Connections
- [[Projects MOC]] · [[College projects MOC]]
- Architectural cousin: [[BioPay — microservices payment backend]]
