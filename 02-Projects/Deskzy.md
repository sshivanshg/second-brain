---
created: 2026-08-04
updated: 2026-08-04
status: active
type: project
tags: project, saas, tools, cloudflare, nextjs, monetization
categories: [fullstack, frontend]
audience: [startup]
repo: /Users/shivanshgupta/ToolEXP
github: https://github.com/sshivanshg/deskzy
site: https://deskzy.xyz
preview: https://deskzy.sshivanshg.workers.dev
package: deskzy
---

# 🧰 Deskzy

> **One-liner:** Global file toolkit (PDF / image / text / media) + short-link platform on Cloudflare edge — most tools run in-browser; links, auth, billing, and analytics hit the server.

**Repo folder:** `~/ToolEXP` (product name **Deskzy**) · **GitHub:** [sshivanshg/deskzy](https://github.com/sshivanshg/deskzy) · **Live:** [deskzy.xyz](https://deskzy.xyz) · **Preview:** [deskzy.sshivanshg.workers.dev](https://deskzy.sshivanshg.workers.dev)

## 🎯 Goal
Ship a privacy-first tool suite people use daily, monetize via Pro short-link analytics + API + unlimited processing, and grow traffic through SEO tools + multi-link / hop-chain demand (incl. [[tg-promo]]).

## 🧱 Stack
| Layer | Choice |
|-------|--------|
| Web | Next.js 15 (App Router) on **Cloudflare Workers** via OpenNext |
| Short links | Cloudflare **KV** (`LINKS`, ~12mo TTL) |
| Auth / billing data | **Supabase** (Auth, Postgres, subscriptions, usage, clicks) |
| Payments | **Razorpay** subscriptions + webhooks |
| Optional local API | Go + Chi under `services/api` (**not** production) |
| Client tools | pdf-lib, pdfjs, ffmpeg.wasm — files stay on device |

## 📁 Repo layout
```
ToolEXP/   # local folder name; npm package "deskzy"
├── apps/web          # product surface + App Router API
├── apps/redirect-worker
├── packages/
├── services/api      # optional Go short-link API (local only)
├── supabase/
└── docs/superpowers/ # plans + design specs
```

## ✨ Tools shipped
**PDF:** merge, split, compress, reorder, PDF→images  
**Image:** compress, resize, convert, WebP→PNG  
**Text:** JSON, Base64, hash, UUID, encode, word count, case, markdown, password  
**Links:** URL shortener, multi-link list (`link-list` / multilink), QR, UTM, WhatsApp link, bio link  
**Media:** Media Converter, Video→MP3/WAV, Audio Converter (ffmpeg.wasm)

## 💰 Monetization
| Tier | Price (INR) | Highlights |
|------|-------------|------------|
| Free | ₹0 | Core tools, unlimited free shorts, daily PDF/image limits |
| Pro | ₹399/mo or ₹2699/yr (~₹225/mo) | Unlimited processing, custom slugs, click analytics, API keys, seats (1–25) |
| Business | Custom | SSO, higher rate limits, dedicated AM |

API for scripted shortens (Pro/Business): `POST /api/links` with `Authorization: Bearer dz_…` — single `url` or multi `urls[]` → `kind: list` short page.

## 🏗 Architecture (thinking snapshot)
- Browser tools never upload files.
- Shorten → KV write; resolve → `GET /r/{code}` hop UI + click bump; Pro also writes Supabase `link_clicks`.
- DNS cutover: registrar NS → Cloudflare (`adrian` / `leonidas`); until fully cut over, workers.dev preview works.
- Related growth surface: [[tg-promo]] builds Deskzy multi-link packs + Linkvertise hop chains for Telegram promo.

## 📝 Log
### 2026-08-04 — vault catch-up (+ full project inventory)
- Discovered live product at `deskzy.xyz` under local folder `ToolEXP`; vault had no note.
- Recent shipped work (from git history): multi-link shortening, AdSense/Monetag hooks, Pro analytics teaser + dedicated stats pages, bio-link builder, Cobe geo globe, edge traffic / API keys / business landing (WIP in working tree).
- Local status: `main` tracks origin; dirty tree with API-key routes, business landing, HilltopAds, edge-traffic, pricing/header edits — **not fully committed**.
- Linked growth ops: [[tg-promo]] multi-link packs; full vault sync of all Projects/* same day.

### 2026-07-28 → 2026-08-03 (repo plans)
- Specs/plans: mobile home fold, image prepare presets, SEO guides, subscription billing (Razorpay).
- Theme + SSR icon typing fixes for production OpenNext build.
- SEO audit artifact: `seo-audit-deskzy.xyz.md` in repo root.

## ✅ Open tasks
- [ ] Commit or stash WIP (API keys, business landing, edge-traffic, ads) and push clean main
- [ ] Finish DNS NS cutover at registrar → Cloudflare if still pending
- [ ] Verify Razorpay live checkout + webhook → Supabase entitlements end-to-end
- [ ] Pro analytics: conversion from free shorten → upgrade funnel
- [ ] Keep [[tg-promo]] pack scheduler healthy (ban-check + hop chain)

## 💡 Ideas / next
- SEO guide content flywheel (docs/superpowers SEO plans)
- API rate tiers for Business
- Deeper geo analytics (Cobe already teasing geography)

## 📚 Sources
- README: `/Users/shivanshgupta/ToolEXP/README.md`
- Pricing: `/Users/shivanshgupta/ToolEXP/apps/web/src/lib/pricing.ts`
- Tool registry: `/Users/shivanshgupta/ToolEXP/apps/web/src/lib/tools/registry.ts`
- Plans: `/Users/shivanshgupta/ToolEXP/docs/superpowers/`

## 🔗 Connections
- [[Projects MOC]] · [[Now]] · [[tg-promo]] (consumer of multi-link API)
- Stack map: [[Tech stack map]]
- Thesis: [[Daily-active surfaces own monthly-active surfaces]] · [[Free + free = no revenue]]
- Privacy angle: browser-local tools = trust moat for file utilities
