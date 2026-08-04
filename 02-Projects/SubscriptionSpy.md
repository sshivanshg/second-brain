---
created: 2026-08-04
updated: 2026-08-04
status: active
type: project
tags: project, saas, fintech, nextjs
categories: [fullstack, ai-ml]
audience: [startup, personal]
repo: /Users/shivanshgupta/Projects/subscriptionspy
github: https://github.com/sshivanshg/subspy
package: subscriptionspy
---

# 💳 SubscriptionSpy

> **One-liner:** Upload a bank statement CSV → GPT identifies recurring SaaS charges → flags duplicates, forgotten trials, and unused tools in under two minutes.

**Repo:** `/Users/shivanshgupta/Projects/subscriptionspy` · **GitHub:** [sshivanshg/subspy](https://github.com/sshivanshg/subspy)

## 🎯 Goal
Surface wasted SaaS spend so people cancel what they forgot. Waitlist + Pro via Stripe.

## 🧱 Stack
- **Next.js 16** App Router
- **Supabase** Auth (magic links) + Postgres
- **Prisma**
- **OpenAI** GPT-4o-mini (categorization) — public copy reframed away from “AI internals”
- **Stripe** Checkout + webhook for Pro
- **Resend** for waste-detected / digest email
- Deploy: **Vercel** (`/api/upload` 60s timeout)

## ✨ Flow
1. Auth → dashboard  
2. CSV parse (`csv-parser.ts`)  
3. Batched categorization (`ai-categorizer.ts`)  
4. Deterministic waste detector (dupes, trials, unused)  
5. Persist subscriptions; optional Pro upgrade

## 📝 Log
### 2026-08-04 — vault catch-up
- Last commit **2026-06-05**: public copy stripped of GPT/AI internals; product-language framing.
- MVP commit: landing, dashboard, `/coming-soon` waitlist, Supabase auth, OpenAI cat, Stripe, Resend, prod gate.
- Status: `main` clean except local `.claude*` dirs.

## ✅ Open tasks
- [ ] Confirm production domain + Stripe live mode
- [ ] Improve bank CSV format coverage (banks differ)
- [ ] Digest email cadence product decision

## 🔗 Connections
- [[Deskzy]] — another consumer SaaS monetization path
- [[Projects MOC]] · [[Free + free = no revenue]]
