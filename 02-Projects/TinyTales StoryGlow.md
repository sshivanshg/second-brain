---
created: 2026-05-24
status: active
type: project
tags: project, ai, consumer, content
categories: [fullstack, ai-ml]
audience: [freelance]
repo: /Users/shivanshgupta/Documents/aistorygen
---

# 🎬 TinyTales / StoryGlow

> **One-liner:** End-to-end platform that turns a child's photo and a theme into a **beautifully illustrated storybook + animated video with voiceover narration**.

**Repo:** `/Users/shivanshgupta/Documents/aistorygen` · **Type:** monorepo

## Stack

| Layer | Tech |
|------|------|
| Frontend | Next.js 15 (App Router), React 18, Tailwind |
| State | Zustand, TanStack Query |
| Backend | Express 4, TypeScript, Prisma ORM |
| Database | PostgreSQL |
| Storage | AWS S3 |
| Auth | Firebase (client) + Clerk (server) |
| Payments | Razorpay |
| AI — Images | Fal.ai (PuLID face swap) |
| AI — Video | Grok Imagine (Fal.ai) |
| AI — TTS | ElevenLabs |
| Real-time | Socket.IO (WebSocket + polling fallback) |
| Email | Resend |
| Video assembly | FFmpeg (subtitles, splicing) |

## What's special
- **PuLID face swap** — the kid's face is rendered consistently across every illustration
- **End-to-end orchestration** — photo + theme → storybook + animated video + voiceover, all from one prompt
- **GPU server** — referenced in `gpu/` and `gpu.pub` (separate GPU box for video work)
- **Magic moment:** the parent gets a polished, personalized animated story without touching a creative tool

## Repo notable files
- `AGENTS.md` — agent orchestration docs
- `amplify.yml` — AWS Amplify deploy
- `ecosystem.config.cjs` — PM2 process config
- `docker-compose.yml` + `docker-compose.test.yml`
- `infra/` — IaC
- `intro_outro/` — branded video bumpers
- `imagesforhero/` — landing hero assets
- `apps/` — monorepo apps
- `backend.md` — backend overview

## Sources
- README: `/Users/shivanshgupta/Documents/aistorygen/README.md`
- Backend doc: `/Users/shivanshgupta/Documents/aistorygen/backend.md`

## Connections
- [[Projects MOC]]
- Uses [[Confirm before write — AI mutations need human approval]] pattern? — verify
