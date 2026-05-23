---
created: 2026-05-24
status: active
type: project
tags: project, agency, marketing-site
categories: [frontend]
audience: [startup]
repo: /Users/shivanshgupta/Documents/Brixloop
private_mirror: /Users/shivanshgupta/Brixloop_priv
---

# 🟠 Brixloop

> **One-liner:** "Build and Ship Faster." Brixloop is a digital studio / dev shop marketing site — services, portfolio, team, blogs.

**Repo:** `/Users/shivanshgupta/Documents/Brixloop` (public) · `/Users/shivanshgupta/Brixloop_priv` (private mirror)
**Generator:** v0.app (Next.js + Tailwind boilerplate)

## Pages live
- `/` — landing (HeroSection)
- `/services` — services on offer
- `/portfolio` — case studies
- `/team` — about the people
- `/blogs` — blog index + `[slug]` posts
- `/project` — project detail
- `/api/inquiry` — contact form handler (sends "Thanks for contacting BrixLoop" email)

## Stack
- Next.js (App Router) + TypeScript
- Tailwind CSS
- Email API route for inquiries
- pnpm

## Brand identity

| Token | HEX | Role |
|------|-----|------|
| **Branding orange** | `#E85002` | Primary accent — CTAs, focus rings |
| **Primary black** | `#000000` | Page background |
| **White** | `#F9F9F9` | Main text on dark |
| **Gray** | `#646464` | Tertiary UI |
| **Light gray** | `#A7A7A7` | Secondary copy |
| **Dark gray** | `#333333` | Card borders, inputs |

**Gradient steps (decorative bands only):** `#000000` → `#C10801` → `#F16001` → `#D9C3A8`

**CSS utilities** (in `app/globals.css`): `.bg-brand-gradient`, `.bg-brand-gradient-vertical`, `.text-gradient-brand`

## Design philosophy
- Black field + orange CTA = bold, audacious
- Dark gray cards (#333333) lift surfaces without heavy shadows
- White section ("How it works") inverts for rhythm
- Orange reserved for actions + priority — never decoration

## Sources
- Color palette spec: `/Users/shivanshgupta/Documents/Brixloop/docs/COLOR_PALETTE.md`
- Layout / metadata: `/Users/shivanshgupta/Documents/Brixloop/app/layout.tsx`
- Hero: `/Users/shivanshgupta/Documents/Brixloop/components/landing/hero-section.tsx`

## Connections
- [[Projects MOC]]
