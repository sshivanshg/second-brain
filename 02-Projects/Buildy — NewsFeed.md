---
created: 2026-08-04
updated: 2026-08-04
status: active
type: project
tags: project, news, expo, dotnet, mvp, india
categories: [fullstack, frontend, backend]
audience: [startup]
repo: /Users/shivanshgupta/Projects/Buildy
github: https://github.com/sshivanshg/Buildy
package: newsfeed
---

# 📰 Buildy — NewsFeed

> **One-liner:** Localized news digest for India — short city-level summaries, 40+ readability, WhatsApp share, QR-flyer acquisition. Monorepo placeholder name **NewsFeed** / folder **Buildy**.

**Repo:** `/Users/shivanshgupta/Projects/Buildy` · **GitHub:** [sshivanshg/Buildy](https://github.com/sshivanshg/Buildy) · **Scaffold date:** 2026-08-03

> Product name not finalized — `NewsFeed` is a code placeholder. Find-and-replace when brand lands.

## 🎯 Goal
Validate hyperlocal short-news for readers 40+ in tier-2/3 cities: city select → digest feed → WhatsApp share → return visits. Acquisition starts offline (QR flyers), not app-store discovery alone.

## 🧱 Stack
| Layer | Choice | Notes |
|-------|--------|--------|
| Client | **Expo** (RN + RN Web) | One UI → web MVP now, native later (ADR-003) |
| API | **.NET 8** Minimal API | `apps/api` |
| DB | **Postgres** local Docker; **Neon** staging/prod | EF Core migrations |
| Shared types | OpenAPI → TS via NSwag | `packages/shared-types` |
| Web host | **Cloudflare Pages** | `expo export -p web` |
| API host | **Render** Docker (ADR-004) | Supersedes Railway in scaffold README |
| UI theme | Black text on white only | No brand accents until name/theme locked |

## 📁 Layout
```
Buildy/
├── apps/app           # Expo universal client
├── apps/api           # .NET 8 API
├── apps/api.Tests     # xUnit + WebApplicationFactory
├── packages/shared-types
├── infra/docker + migrations
├── docs/adr           # ADRs 001–004
└── docs/PRD.md
```

Client routes (early): `(tabs)`, `feed`, `city`, `article`.  
API endpoints so far: `ArticlesEndpoints`, `CitiesEndpoints`, health/OpenAPI.

## 📜 Key decisions (ADRs)
1. **Monorepo** — single PR for API + types + Expo (2-person team).
2. **No auth MVP** — validate feed/share/QR loop; rate-limit + CORS instead.
3. **Expo universal from day one** — no separate Vite web app.
4. **Render + Cloudflare Pages + Neon** — edge-cache feed GETs (~60s) to cut Neon cost.

## 📋 MVP scope (from PRD)
**In:** mobile web feed, 3–4 pilot cities, RSS → original short summaries + source link, city select, WhatsApp share, Local/State/National tags, QR landing attribution, admin review queue.  
**Out (phase 1):** login, ML recs, comments, push, paywall, native stores (phase 2 same Expo app).

## 📝 Log
### 2026-08-04 — vault catch-up
- New monorepo found under `~/Projects/Buildy`; not previously in vault.
- Git: `main` **ahead 1** of origin (`initial setup` / NewsFeed scaffold). Older history in same repo includes an **XAUUSD trading simulator** branch of work — unrelated product buried under same git history; current tree is NewsFeed.
- Docs strong: PRD + 4 ADRs + superpowers docs; UI deliberately unbranded B&W.

### 2026-08-03
- Scaffold: Expo client, .NET API, docker-compose Postgres, Railway→Render hosting decision, no-auth MVP ADR.

## ✅ Open tasks
- [ ] Push local commit(s) so origin matches scaffold
- [ ] Lock product name / brand (replace `NewsFeed`)
- [ ] M1: cities + articles API + Expo feed reading from API
- [ ] M2: RSS/summary pipeline + admin review queue
- [ ] M3: WhatsApp share + QR landing attribution
- [ ] Wire Render + Cloudflare Pages + Neon per ADR-004
- [ ] Align README deploy section with ADR-004 (still mentions Railway in places)

## 💡 Ideas / next
- Pilot city selection + flyer batch UTMs
- Readability mode (large type, high contrast) as a product differentiator
- Later: native via EAS on same `apps/app`

## 📚 Sources
- `/Users/shivanshgupta/Projects/Buildy/README.md`
- `/Users/shivanshgupta/Projects/Buildy/docs/PRD.md`
- `/Users/shivanshgupta/Projects/Buildy/docs/adr/001-monorepo.md` … `004-render-cloudflare-neon-hosting.md`

## 🔗 Connections
- [[Projects MOC]] · [[Now]]
- GTM pattern echoes [[HealthPro — QR code platform]] (QR as acquisition)
- Share loop: WhatsApp-native redistribution (India news behavior in PRD)
