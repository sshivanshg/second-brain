---
created: 2026-05-24
type: reference
tags: reference, tech, skills
---

# 🧰 Tech stack map

> Where each technology shows up across my projects. Useful for: "where did I use X?", interview prep, choosing a stack for a new project, and seeing my actual skill surface.

## Languages & runtimes
| Tech | Projects |
|------|----------|
| **TypeScript** | [[Arth Saathi]], [[LexVault]], [[Agora]], [[TinyTales StoryGlow]], [[e-Factory CMS]], [[Brixloop]], [[Kidbee — AI storytelling for kids]], [[DevHelp]], [[Deskzy]], [[Buildy — NewsFeed]], [[Spill]], [[Links Tracker]], [[SubscriptionSpy]], [[QuizDuel]], [[Engram — Industrial Knowledge Intelligence]], [[MyAssistant]], [[Ditch LA Creator Portal]] |
| **Python** | [[RAMT — Regime-Adaptive Multimodal Transformer]], [[SpatiaLaw — WiFi CSI presence detection]], [[StP — Single-stock XGBoost prediction]], [[algo_dev — Indian stock market trading algorithm]], [[Qm Quant Research]], [[CNN — CIFAR-10 mini project]], [[tg-promo]], [[PO3 XAUUSD Backtester]], [[Brixloop Leads]] |
| **Go** | [[MarketMind — Vellum]] (API+River), optional [[Deskzy]] local API; OSS CNCF forks |
| **JavaScript** | [[Devops Ecom]], [[HealthPro — QR code platform]], [[zenova — health tracker backend]], [[College Appointment System]], [[BioPay — microservices payment backend]] |
| **C# / .NET 8** | [[Buildy — NewsFeed]] (Minimal API) |
| **C / C++ / Rust** | OSS forks (Bitcoin Core), `OS/raw_keyboard.c`, [[Claw Code rewrite]] (Rust port) |
| **Go** | [[MarketMind — Vellum]]; optional local API in [[Deskzy]]; OSS forks (Cilium, Kyverno, Antrea, Prometheus Operator) — see [[OSS contributions]] |
| **Swift / Dart** | `fileorganizer` (Swift), `apidash` (Dart fork) |

## Frontend
| Tech | Projects |
|------|----------|
| **Next.js (App Router)** | [[Arth Saathi]], [[LexVault]], [[Agora]], [[TinyTales StoryGlow]], [[Brixloop]], [[Devops Ecom]], [[Portfolio site]], [[Deskzy]] (OpenNext → Cloudflare Workers) |
| **React (Vite/CRA)** | [[e-Factory CMS]], [[OS File Organizer]], [[Kidbee — AI storytelling for kids]], [[Links Tracker]] |
| **React Native / Expo** | [[Arth Saathi]] (mobile), [[Spill]], [[Buildy — NewsFeed]] (universal RN Web + native later) |
| **Tailwind + shadcn/ui** | [[Arth Saathi]], [[Agora]], [[Brixloop]], [[Deskzy]], most TS frontends |
| **Electron** | [[OS File Organizer]] |

## Backend / APIs
| Tech | Projects |
|------|----------|
| **Express 4** | [[LexVault]], [[TinyTales StoryGlow]], [[HealthPro — QR code platform]], [[zenova — health tracker backend]], [[College Appointment System]] |
| **Fastify + Nest** | [[Arth Saathi]] |
| **Hono** | [[Agora]], [[Links Tracker]] (Workers) |
| **.NET Minimal API** | [[Buildy — NewsFeed]] |
| **Telethon / bots** | [[tg-promo]] |
| **Microservices** | [[BioPay — microservices payment backend]] |

## Data
| Tech | Projects |
|------|----------|
| **PostgreSQL + Prisma** | [[Arth Saathi]], [[TinyTales StoryGlow]], [[BioPay — microservices payment backend]] |
| **Postgres + pgvector** | [[LexVault]], [[Agora]] (RAG / embeddings) |
| **Neon (serverless PG)** | [[LexVault]], [[Buildy — NewsFeed]] (planned) |
| **Supabase** | [[Deskzy]] (auth, subscriptions, clicks), [[Spill]] |
| **Cloudflare KV / D1** | [[Deskzy]] (KV short links), [[Links Tracker]] (D1) |
| **MongoDB** | [[Devops Ecom]], [[College Appointment System]] |
| **Drizzle ORM** | [[Agora]] |

## AI / ML
| Tech | Projects |
|------|----------|
| **Claude (Anthropic)** | [[Arth Saathi]], [[LexVault]], [[Agora]], [[Claw Code rewrite]] |
| **LangGraph** | [[Arth Saathi]], [[Agora]] |
| **Vercel AI SDK** | [[Agora]] |
| **Fal.ai (PuLID, Grok Imagine)** | [[TinyTales StoryGlow]] |
| **ElevenLabs (TTS)** | [[TinyTales StoryGlow]] |
| **Chronos-T5 + LoRA** | [[RAMT — Regime-Adaptive Multimodal Transformer]] |
| **XGBoost** | [[StP — Single-stock XGBoost prediction]] |
| **HMM** | [[RAMT — Regime-Adaptive Multimodal Transformer]] |
| **scikit-learn (Random Forest)** | [[SpatiaLaw — WiFi CSI presence detection]] |
| **PyTorch** | [[CNN — CIFAR-10 mini project]], [[RAMT — Regime-Adaptive Multimodal Transformer]] |

## Infra / DevOps
| Tech | Projects |
|------|----------|
| **Vercel** | [[Arth Saathi]] (web/admin/landing), [[Spill]] web |
| **Azure Container Apps** | [[Arth Saathi]] (backend) |
| **AWS (S3, CloudFront, EC2, ECR)** | [[e-Factory CMS]], [[TinyTales StoryGlow]] |
| **Cloudflare Workers / Pages / KV / R2** | [[Deskzy]] (Workers + OpenNext + KV), [[Buildy — NewsFeed]] (Pages), [[LexVault]] (R2), [[Links Tracker]] (Workers/D1/DO) |
| **Render** | [[Devops Ecom]], [[LexVault]], [[Buildy — NewsFeed]] API (ADR-004) |
| **Railway** | scaffold leftovers only — prefer Render for NewsFeed |
| **Terraform** | [[Devops Ecom]] |
| **Docker / Compose** | [[Arth Saathi]], [[Agora]], [[BioPay — microservices payment backend]], [[RAMT — Regime-Adaptive Multimodal Transformer]], [[LFX Organizations Dashboard]], [[Buildy — NewsFeed]] |
| **EAS (Expo)** | [[Arth Saathi]] mobile, [[Spill]], [[Buildy — NewsFeed]] (later) |

## Auth / payments / comms
| Tech | Projects |
|------|----------|
| **Auth.js v5 (phone OTP)** | [[Arth Saathi]] |
| **Firebase Auth** | [[Arth Saathi]], [[TinyTales StoryGlow]] |
| **Clerk** | [[TinyTales StoryGlow]] |
| **Supabase Auth** | [[Deskzy]], [[Spill]] |
| **JWT (RS256)** | [[LexVault]], [[Devops Ecom]], backends |
| **Better Auth** | [[Agora]], [[Links Tracker]] |
| **Razorpay / Razorpay X** | [[Arth Saathi]], [[TinyTales StoryGlow]], [[Deskzy]] (Pro seats) |
| **Twilio (SMS/WhatsApp)** | [[Arth Saathi]] |
| **Liveblocks (realtime)** | [[LexVault]] |
| **Socket.IO** | [[TinyTales StoryGlow]], [[Links Tracker]] |
| **Telegram / Telethon** | [[tg-promo]] |
| **Linkvertise** | [[tg-promo]] hop monetization → [[Deskzy]] |

## Observations about my stack
- **Default modern stack:** Next.js + TS + Tailwind + Postgres/Prisma + Claude. I reach for this reflexively.
- **Edge-first product line emerging:** [[Deskzy]] (Workers + KV) + Cloudflare Pages patterns in [[Buildy — NewsFeed]] / [[Links Tracker]].
- **AI orchestration depth:** LangGraph appears in 2 production apps — a genuine specialty.
- **Polyglot for systems:** Go/Rust/C reserved for OSS + low-level (Bitcoin, harness, CNCF); **C#/.NET** now live in NewsFeed API.
- **Quant is Python-only** and shows a clear skill ladder (see [[Quant projects]]); Python also powers growth ops ([[tg-promo]]).

## How to keep this updated
When you build with a new tech, add a row (or a project to an existing row). Or ask Claude: *"update the tech stack map from my projects."*

## Related
- [[Projects MOC]] · [[_Categories index]] · [[AI ML projects]]
