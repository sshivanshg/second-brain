---
created: 2026-05-29
status: in-progress
type: project
tags: project, personal, frontend, viral
categories: [frontend]
audience: [personal]
local: ~/dev-spirit-animal
language: HTML/CSS/JS (vanilla, single-file)
visibility: private
---

# 🦀 Dev Spirit Animal

> **One-liner:** A 30-second personality quiz for engineers — answer 5 quippy questions, get a shareable "dev spirit animal" card. Built deliberately for virality (high-volume reach).

**Local:** `~/dev-spirit-animal/index.html` · not pushed to GitHub yet · runs by double-click or `python3 -m http.server`.

## What it is
Single self-contained `index.html` — no build step, no backend, no dependencies. Five questions map (via weighted scoring) to one of **8 dev archetypes**, each with emoji, a witty quip, traits, stat bars, and a "rarity %":

🦀 Rust Evangelist · 🦉 3AM Architect · 🦅 Ship-It Visionary · 🦫 Backend Engineer · 🦦 Playful Prototyper · 🐙 Full-Stack Polyglot · 🦝 Stack Overflow Forager · 🐺 Opinionated Senior

## Viral mechanics (the actual point)
- **Frictionless** — no sign-up, instant load, works offline once cached.
- **Shareable card** — result renders to a 1080×1350 PNG on `<canvas>`; downloadable or native-shared (Web Share API on mobile, X intent on desktop).
- **Viral loop** — every result is deep-linked (`#crab`, `#owl`, …). Opening a shared link shows the friend's card *and* prompts the visitor to take the quiz → built-in pull.
- **Scales for ~free** — pure static, sits on a CDN, handles millions at no cost.

## Design language
Started loud (neon gradient mesh). Pivoted to **minimal/editorial** per taste: warm paper background, Fraunces serif headlines + Inter body, hairline borders, one *muted* accent per animal (terracotta, dusty-indigo, clay, muted-teal…) used sparingly on the emoji halo + stat bars. The downloadable card matches.

## Decisions captured
- **Hosting → Vercel first, Cloudflare Pages later if it pops.** Reasoning: no traffic yet, so optimizing for a viral spike is premature; momentum (live in ~60s) beats theoretical scale; it's one static file so migrating to Cloudflare's *unlimited-bandwidth* free tier later is a 10-min job with zero lock-in.
- **Domain → defer until traction.** Availability checked live via RDAP. Available + cheap-renewal options: `devspiritanimal.xyz` (~$2 yr1 / ~$13–15 renewal) and `devspiritanimal.com` (available; cheapest *forever* at Cloudflare ~$11/yr flat vs GoDaddy ~$22). Avoid renewal traps (.fun/.online/.site/.space renew $26–31; .io/.tech/.store $44–52). Taken: devanimal.com, devtotem.com, devspirit.dev, codeanimal.com.
- The card/footer currently hardcode `whatsmydevspiritanimal.com` — **must URL-neutralize before deploy** so the shared PNG doesn't advertise an unowned domain.

## Status / next steps
- [x] Build (quiz + 8 animals + canvas card + share/deep-link)
- [x] Minimal aesthetic redesign
- [ ] URL-neutralize card/footer/OG tags
- [ ] Deploy to Vercel
- [ ] (if traction) buy domain → move to Cloudflare

## Why this matters
A pure distribution play, not a portfolio piece. The whole craft here is the **viral loop + share artifact** — a reusable pattern (deep-linked results, client-rendered share cards, zero-friction entry) worth keeping in the brain for any future "make it spread" build.

## Connections
- [[Projects MOC]] · [[Frontend projects]] · [[Personal projects]]
