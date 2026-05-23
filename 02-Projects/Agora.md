---
created: 2026-05-24
status: active
type: project
tags:
categories:
  - fullstack
  - ai-ml
audience:
  - startup
repo: /Users/shivanshgupta/agora
license: Apache-2.0
---

# 🏛️ Agora

> **One-liner:** Self-hostable, open-source multi-agent **AI debate platform** — distinct AI personas debate current topics in structured, fact-checked rounds. BYOK. No central server.

**Repo:** `/Users/shivanshgupta/agora`

## Philosophy
1. **OSS-first.** Clone, run, modify in <5 minutes.
2. **BYOK.** User-supplied keys for Anthropic, OpenAI, Google, Groq, or Ollama. Encrypted **AES-256-GCM** before touching DB; never leaves the instance in plaintext.
3. **Single-user by default.** No login, no signup. One env-var flips on multi-user.
4. **No Redis.** Postgres only.
5. **Boring infra, exciting product.**

## How it works
A **LangGraph state machine** drives every debate through phases:
```
INIT → FRAMING → OPENING → CROSS-EXAMINATION → REBUTTALS → CLOSING → SYNTHESIS
```
Each phase invokes the right agent (Moderator, Debater, Fact-Checker, Synthesizer) with the right context. Output streams to the browser over **SSE**. State persists to Postgres at every transition — debates are **resumable and inspectable**.

**Personas** = markdown files in `packages/personas/specs/`. Adding a persona = adding one `.md` file. No code changes.

## Stack
| Layer | Choice |
|------|--------|
| Frontend | Next.js 15 + Tailwind v4 |
| API | Hono on Node.js |
| Orchestration | LangGraph.js + Vercel AI SDK |
| DB | PostgreSQL 16 + pgvector + Drizzle ORM |
| Queue | Inngest |
| Auth (opt-in) | Better Auth — email magic links |
| Styling | shadcn/ui + Geist fonts + OKLCH tokens |

## Quickstart
```bash
git clone <repo> && cd agora
cp .env.example .env && pnpm gen:encryption-key   # paste output into .env
pnpm install && pnpm db:push && pnpm db:seed
pnpm dev
```
Requires: Node 20+, PostgreSQL 16+ with pgvector, Docker (local postgres).

## Sources
- README: `/Users/shivanshgupta/agora/README.md`
- Contributing: `/Users/shivanshgupta/agora/CONTRIBUTING.md`
- Security: `/Users/shivanshgupta/agora/SECURITY.md`

## Connections
- [[Projects MOC]]
- Architectural cousin to: [[Arth Saathi]] (both use LangGraph for agent orchestration)
