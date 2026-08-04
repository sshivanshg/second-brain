---
created: 2026-08-04
updated: 2026-08-04
status: active
type: project
tags: project, mobile, game, expo, bun
categories: [fullstack, frontend]
audience: [personal]
repo: /Users/shivanshgupta/Projects/quizduel
package: quizduel
---

# ⚔️ QuizDuel

> **One-liner:** Real-time 1v1 quiz battle Android app — built for **zero recurring cost** (SQLite, no Redis, pre-seeded questions, EAS free tier).

**Repo:** `/Users/shivanshgupta/Projects/quizduel` · Spec: `INSTRUCTIONS.md`

## 🧱 Stack
| Layer | Choice |
|-------|--------|
| Mobile | Expo + RN, Expo Router, NativeWind v4, Zustand, Reanimated |
| Server | **Bun + Elysia** |
| DB | SQLite via `bun:sqlite` + drizzle |
| Realtime | Native WebSocket (in-memory rooms) |
| Auth | JWT 90-day in SecureStore |
| Questions | Pre-generated JSON seed — **no runtime AI API** |
| Ship | EAS Build → Play Store |

## Game machine
`WAITING → COUNTDOWN → ACTIVE → FINISHED`  
10 questions × 15s; score `100 + floor(50*(1 - timeMs/15000))` max 150/q.

## Explicit v1 trade-offs
- Single instance (in-memory rooms die on restart)
- Scheme-only deep links `quizduel://join/CODE` (no App Links domain)
- Static question bank gets repetitive
- No push

## 📝 Log
### 2026-08-04 — vault catch-up
- Spec-complete monorepo on disk; no remote git in inventory. Treat as **active build**, not yet public.

## ✅ Open tasks
- [ ] Finish WS state-machine tests (Phase 7 non-negotiable per spec)
- [ ] EAS preview APK + Play listing
- [ ] v2: domain App Links, push, question variety

## 🔗 Connections
- [[Spill]] (Expo consumer app) · [[Dev Spirit Animal]] / [[git wrapped — GitHub commit roast generator]] (viral share-card DNA)
- [[Projects MOC]]
