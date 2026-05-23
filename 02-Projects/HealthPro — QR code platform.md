---
created: 2026-05-24
status: completed
type: project
tags: project, backend, qr-codes, analytics
categories: [backend]
audience: [college]
github: https://github.com/sshivanshg/healthpro
language: JavaScript
visibility: public
---

# 🩺 HealthPro — QR Code Management Platform

> **One-liner:** Backend for **Dynamic QR Codes + Event Tracking + Analytics** — generate, update, manage QR codes; track interactions; analyze performance.

**GitHub:** [sshivanshg/healthpro](https://github.com/sshivanshg/healthpro)

## Features
- User auth + authorization (JWT)
- **Static + Dynamic QR codes** — dynamic ones redirect through tracker
- Event tracking on every scan
- Analytics on interaction patterns

## Why dynamic QR matters
Static QR = encoded URL, immutable. Dynamic QR = encoded short URL that 302-redirects to current target. Lets you:
- Change destination after printing
- Track per-scan analytics (where, when, who)
- A/B test destinations

## Stack
- Node.js + Express
- JWT auth
- Database (likely Mongo or Postgres)
- Postman collection for API testing

## Connections
- [[Projects MOC]] · [[College projects MOC]]
