---
created: 2026-05-24
status: active
type: project
tags:
categories: [devtool, systems]
audience: [startup]
repo: /Users/shivanshgupta/devhelp
package: devhelp-cli
version: 0.4.0
---

# 🛠️ DevHelp

> **One-liner:** Clone an OSS repo. Get a working dev environment. Deterministic by default. Honest about what it can't do.

**Repo:** `/Users/shivanshgupta/devhelp` · **Status:** v0.4.0 · **Package:** `devhelp-cli`

## What it does
```bash
devhelp "Set me up to contribute to facebook/react"
```
…and then does what you'd do by hand for the next 45 minutes — *automatically, with a live progress UI*:
- Detects runtimes (Node/Python/Rust/Go), picks the right package manager from the lockfile
- Runs install, copies env files, generates Prisma client, installs Playwright browsers
- Surfaces Docker Compose service deps
- Honors `.devcontainer/devcontainer.json` `postCreateCommand` natively
- Uses your existing **mise/asdf/volta** if present

## Stress-tested accuracy (measured, not claimed)
Same 20 real OSS repos, two runs:

| Ecosystem | Repos | v0.1 clean | v0.2 clean |
|---|---|---|---|
| Node monorepos | 5 | 3/5 | 5/5 |
| Python | 5 | 1/5 | 4/5 |
| Rust | 2 | 2/2 | 2/2 |
| Go | 2 | 0/2 | 2/2 |
| Polyglot / edge | 4 | 0/4 | 1/4 |
| Library-only | 2 | 0/2 | 1/2 |
| **Total** | **20** | **5/20 (25%)** | **15/20 (75%)** |

**Repos outside coverage exit non-zero with a clear `UNSUPPORTED` panel.** Zero silent failures since v0.2.

## Positioning vs adjacent tools

| Tool | Solves | Doesn't |
|------|--------|---------|
| **mise / asdf** | Runtime versions from `.tool-versions` | Package install, env files, codegen, framework hints |
| **volta** | Node version + pm pinning | Non-Node ecosystems, post-install |
| **corepack** | Node package-manager selection | Runtime install |
| **Devbox / devenv.sh** | Declarative Nix dev env | Only ~1% of OSS opted in |
| **devcontainers** | Containerized dev env | Needs Docker; doesn't help native |
| **act** | Run CI workflows locally in Docker | Doesn't set up your laptop |
| **devhelp** | All of the above, glued, on your native laptop | Doesn't replace your runtime manager — uses it |

## Key bug fix (v0.1 → v0.2)
v0.1 always rendered a green `READY` box, even on broken detection. v0.2 returns exit 1 with `UNSUPPORTED` (no recognized stack) or `INCOMPLETE` (critical step errored) — so users actually find out when something went wrong.

## Concrete wins from the stress test
- **calcom/cal.com:** mise would resolve Node + Yarn. devhelp does that *and* runs `pnpm install`, finds `packages/prisma/schema.prisma`, runs `prisma generate`, copies the eight per-app `.env.example` files, surfaces the `docker compose up -d` requirement for Postgres.
- **pydantic/pydantic:** mise picks up `requires-python` (3.9, EOL). devhelp picks 3.14 from the CI matrix max — what maintainers actually develop against.

## Sources
- README: `/Users/shivanshgupta/devhelp/README.md`
- Differentiators: `/Users/shivanshgupta/devhelp/DIFFERENTIATORS.md`
- Stress test: `/Users/shivanshgupta/devhelp/stress-test/results.json`, `/Users/shivanshgupta/devhelp/stress-test/RETEST_RESULTS.md`
- Regression suite: `/Users/shivanshgupta/devhelp/run-regression.sh`
- "Why not mise?": `/Users/shivanshgupta/devhelp/docs/WHY-NOT-MISE.md`
- Overnight log: `/Users/shivanshgupta/devhelp/overnight-log.md`

## Connections
- [[Projects MOC]]
- Composes with [[Daily-active surfaces own monthly-active surfaces]]: devhelp is daily-CLI-frequency for OSS contributors
