---
created: 2026-05-29
status: active
type: project
tags: project, personal
categories: [frontend]
audience: [personal]
github:
language: TypeScript
visibility: local
---

# 💀 git wrapped — GitHub commit roast generator

> **One-liner:** A minimal, viral web toy — paste any GitHub username and get brutally (and funnily) roasted by your own commit history. Spotify Wrapped energy, but for a developer's git log. Shareable card + dynamic OG image.

**Local:** `~/projects/git-wrapped` — not yet pushed / deployed.
**Goal of the build:** something cool, quirky, dev-culture, and *shareable* enough to pull millions of footfall on a free Vercel frontend.

## What it does
- Type a GitHub username → ~2s later you get a card with a 0–100 **chaos score**, a **dev-spirit title**, and a stack of savage one-liners computed from your real commits.
- Example burns: *"27 'fix' commits — you don't write software, you write hostage notes to your future self."* / *"82% of everything is 'linux'. You and that repo need to start seeing other people."*
- **Share loop:** every result has a shareable URL (`/?u=username`) with a dynamic OG image, so links unfurl into the card on X / Slack / Discord. Plus share-on-X, download-PNG, copy-link buttons.

## How it works
- Pulls recent commits via GitHub's **commit search API** — the only public endpoint that still returns commit messages *and* author-local timestamps (so "you commit at 2am" is accurate to the dev's own git timezone).
- The roast engine (`lib/roast.ts`) counts patterns — lazy "fix" messages, late-night %, weekend %, repo obsession, one-word commits, reverts, merges, language — and turns them into burns.
- Joke variants are chosen with a **seeded RNG keyed by username**, so a person always gets the same roast *and* the web card matches the share image. Different users pull different lines.
- A "headliner" tier only fires on egregious stats and leads the card with the worst offense.

## Stack
- Next.js 15 (App Router) + React 19, TypeScript
- Tailwind CSS — minimal monochrome theme (near-black, off-white, hairline borders, no gradients)
- `next/og` for dynamic share images · `html-to-image` for PNG export
- Deploys to Vercel with zero config

## Why this matters
A pure shareability experiment: low-friction, no login, output is *about you* so people screenshot and tag friends → every share brings new visitors. Good test of the "minimal + quirky + dev-humor = viral" thesis.

## Open items
- [ ] Add `GITHUB_TOKEN` (raises rate limit 10→30 search/min, 60→5000/hr) — required before public launch
- [ ] `git init` + push to GitHub
- [ ] Deploy to Vercel
- [ ] Optional: KV cache keyed by username to survive a traffic spike
- [ ] Optional: "compare two users" mode

## Connections
- [[Projects MOC]]
- [[Frontend projects]]
