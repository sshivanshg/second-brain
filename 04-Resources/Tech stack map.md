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
| **TypeScript** | [[Arth Saathi]], [[LexVault]], [[Agora]], [[TinyTales StoryGlow]], [[e-Factory CMS]], [[Brixloop]], [[Kidbee — AI storytelling for kids]], [[DevHelp]] |
| **Python** | [[RAMT — Regime-Adaptive Multimodal Transformer]], [[SpatiaLaw — WiFi CSI presence detection]], [[StP — Single-stock XGBoost prediction]], [[algo_dev — Indian stock market trading algorithm]], [[Qm Quant Research]], [[CNN — CIFAR-10 mini project]] |
| **JavaScript** | [[Devops Ecom]], [[HealthPro — QR code platform]], [[zenova — health tracker backend]], [[College Appointment System]], [[BioPay — microservices payment backend]] |
| **C / C++ / Rust** | OSS forks (Bitcoin Core), `OS/raw_keyboard.c`, [[Claw Code rewrite]] (Rust port) |
| **Go** | OSS forks (Cilium, Kyverno, Antrea, Prometheus Operator) — see [[OSS contributions]] |
| **Swift / Dart** | `fileorganizer` (Swift), `apidash` (Dart fork) |

## Frontend
| Tech | Projects |
|------|----------|
| **Next.js (App Router)** | [[Arth Saathi]], [[LexVault]], [[Agora]], [[TinyTales StoryGlow]], [[Brixloop]], [[Devops Ecom]], [[Portfolio site]] |
| **React (Vite/CRA)** | [[e-Factory CMS]], [[OS File Organizer]], [[Kidbee — AI storytelling for kids]] |
| **React Native / Expo** | [[Arth Saathi]] (mobile app) |
| **Tailwind + shadcn/ui** | [[Arth Saathi]], [[Agora]], [[Brixloop]], most TS frontends |
| **Electron** | [[OS File Organizer]] |

## Backend / APIs
| Tech | Projects |
|------|----------|
| **Express 4** | [[LexVault]], [[TinyTales StoryGlow]], [[HealthPro — QR code platform]], [[zenova — health tracker backend]], [[College Appointment System]] |
| **Fastify + Nest** | [[Arth Saathi]] |
| **Hono** | [[Agora]] |
| **Microservices** | [[BioPay — microservices payment backend]] |

## Data
| Tech | Projects |
|------|----------|
| **PostgreSQL + Prisma** | [[Arth Saathi]], [[TinyTales StoryGlow]], [[BioPay — microservices payment backend]] |
| **Postgres + pgvector** | [[LexVault]], [[Agora]] (RAG / embeddings) |
| **Neon (serverless PG)** | [[LexVault]] |
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
| **Vercel** | [[Arth Saathi]] (web/admin/landing) |
| **Azure Container Apps** | [[Arth Saathi]] (backend) |
| **AWS (S3, CloudFront, EC2, ECR)** | [[e-Factory CMS]], [[TinyTales StoryGlow]] |
| **Cloudflare R2** | [[LexVault]] |
| **Render** | [[Devops Ecom]], [[LexVault]] |
| **Terraform** | [[Devops Ecom]] |
| **Docker / Compose** | [[Arth Saathi]], [[Agora]], [[BioPay — microservices payment backend]], [[RAMT — Regime-Adaptive Multimodal Transformer]], [[LFX Organizations Dashboard]] |
| **EAS (Expo)** | [[Arth Saathi]] mobile |

## Auth / payments / comms
| Tech | Projects |
|------|----------|
| **Auth.js v5 (phone OTP)** | [[Arth Saathi]] |
| **Firebase Auth** | [[Arth Saathi]], [[TinyTales StoryGlow]] |
| **Clerk** | [[TinyTales StoryGlow]] |
| **JWT (RS256)** | [[LexVault]], [[Devops Ecom]], backends |
| **Better Auth** | [[Agora]] |
| **Razorpay / Razorpay X** | [[Arth Saathi]], [[TinyTales StoryGlow]] |
| **Twilio (SMS/WhatsApp)** | [[Arth Saathi]] |
| **Liveblocks (realtime)** | [[LexVault]] |
| **Socket.IO** | [[TinyTales StoryGlow]] |

## Observations about my stack
- **Default modern stack:** Next.js + TS + Tailwind + Postgres/Prisma + Claude. I reach for this reflexively.
- **AI orchestration depth:** LangGraph appears in 2 production apps — a genuine specialty.
- **Polyglot for systems:** Go/Rust/C reserved for OSS + low-level (Bitcoin, harness, CNCF).
- **Quant is Python-only** and shows a clear skill ladder (see [[Quant projects]]).

## How to keep this updated
When you build with a new tech, add a row (or a project to an existing row). Or ask Claude: *"update the tech stack map from my projects."*

## Related
- [[Projects MOC]] · [[_Categories index]] · [[AI ML projects]]
