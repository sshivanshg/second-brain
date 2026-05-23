---
created: 2026-05-24
status: archived
type: project
tags: project, backend, microservices, fintech
categories: [backend, systems]
audience: [college]
github: https://github.com/sshivanshg/biopay-backend
language: JavaScript
visibility: private
---

# 💳 BioPay — Microservices Payment Backend

> **One-liner:** Microservices-based backend for the BioPay app — user, wallet, transaction services.

**GitHub:** [sshivanshg/biopay-backend](https://github.com/sshivanshg/biopay-backend) (private)

## Architecture
```
biopay-backend/
├── src/services/
│   ├── user-service/         # User management
│   ├── wallet-service/       # Wallet management
│   └── transaction-service/  # Transaction processing
├── src/shared/               # Shared utils, middleware
├── prisma/                   # Schema + migrations
└── docker/                   # Compose configs
```

## Stack
- Node.js (v18+)
- Prisma ORM
- PostgreSQL
- Redis (caching / queues)
- Docker + Docker Compose

## Why microservices here (and a note on whether it was right)
The split (user / wallet / transaction) is the textbook decomposition for a payments app. The cost: 3× more deploy infra, cross-service auth, distributed transactions. For an MVP / college project, a modular monolith would likely have shipped faster — but the microservices split is a *useful learning artifact* for understanding bounded contexts.

## Connections
- [[Projects MOC]] · [[College projects MOC]]
- Architectural cousin to: [[zenova — health tracker backend]] (similar Express + Postgres pattern)
