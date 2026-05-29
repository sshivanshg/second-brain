---
created: 2026-05-24
updated: 2026-05-26
status: active
type: project
tags:
categories: [devtool, systems]
audience: [startup]
repo: /Users/shivanshgupta/devhelp
package: devhelp-cli
version: 0.4.0
next: 0.5.0 (unreleased)
---

# 🛠️ DevHelp

> **One-liner:** Clone an OSS repo. Get a working dev environment — then *prove it actually runs*. Deterministic by default. Honest about what it can't do.

**Repo:** `/Users/shivanshgupta/devhelp` · **Released:** v0.4.0 · **Unreleased:** large v0.5 capability expansion (see [[DevHelp/v0.5 Capabilities & Validation]]) · **Package:** `devhelp-cli`

## What it does
```bash
devhelp "Set me up to contribute to facebook/react"
```
…and then does what you'd do by hand for the next 45 minutes — *automatically, with a live progress UI*:
- Detects runtimes (29 ecosystems), picks the right package manager **from the lockfile**
- Runs install, copies env files, generates Prisma client, installs Playwright browsers
- Surfaces Docker Compose service deps; honors `.devcontainer` `postCreateCommand`
- Uses your existing **mise/asdf/volta** if present
- **New (v0.5):** can *verify* the result, *start services + migrate the DB*, *auto-fix* native-build failures, emit a *reproducible lock*, run *project recipes*, generate *VS Code* configs, populate *secrets*, and run as an *MCP server*

## Commands, flags & subcommands
| | What |
|---|---|
| `devhelp <repo>` | Clone + set up a repo (URL or `owner/repo`) |
| `devhelp doctor` | Read-only diagnosis: detected stack vs. what's installed (flags version mismatches) |
| `devhelp mcp` | Run as an MCP server over stdio (`detect` + `doctor` tools) |
| `--verify` | After setup, run tests + boot the dev server and poll its URL — real pass/fail, non-zero exit on failure |
| `--with-services` | `docker compose up -d --wait` (v1 fallback) + DB migrations (Prisma/Drizzle/Django/Rails) |
| `--fix` | On a recoverable failure, install the missing system dep (apt/dnf/pacman/zypper/apk/brew) and retry once |
| `--write-lock` | Write `.devhelp.lock` pinning resolved runtime versions |
| `--vscode` | Generate `.vscode/launch.json` for the detected stack (won't overwrite) |
| `--secrets` | Populate `.env` from 1Password (`op inject`) or Doppler |
| `--json` | Machine-readable result (status, detected, steps, failures, verify) |
| `.devhelp.yml` | Repo recipe: `postInstall` steps + `dev`/`test`/`build` overrides |

> Native Windows isn't fully supported yet — run under **WSL** or **Git Bash**. devhelp detects a missing bash-compatible shell and says so up front.

## The thesis, now measurable
The product's success metric has always been *"did `pnpm dev` actually work afterwards?"* — `--verify` turns that from a stress-test claim into a **per-run check**: it runs the tests and boots the dev server, polling the real URL for a response, then tears it down. Validated end-to-end on a live app (express installed → tests pass → server answers at :3000).

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

**Repos outside coverage exit non-zero with a clear `UNSUPPORTED` panel.** Zero silent failures since v0.2. v0.5 re-validated all new code against **21 real cloned repos: 0 crashes**, and fixed a real zsh `lts/*` glob bug found in the process.

## Positioning vs adjacent tools

| Tool | Solves | Doesn't |
|------|--------|---------|
| **mise / asdf** | Runtime versions from `.tool-versions` | Package install, env files, codegen, framework hints |
| **volta** | Node version + pm pinning | Non-Node ecosystems, post-install |
| **corepack** | Node package-manager selection | Runtime install |
| **Devbox / devenv.sh** | Declarative Nix dev env | Only ~1% of OSS opted in |
| **devcontainers** | Containerized dev env | Needs Docker; doesn't help native |
| **act** | Run CI workflows locally in Docker | Doesn't set up your laptop |
| **devhelp** | All of the above, glued, on your native laptop — *and verifies it* | Doesn't replace your runtime manager — uses it |

## Concrete wins from the stress test
- **calcom/cal.com:** mise would resolve Node + Yarn. devhelp does that *and* runs `pnpm install`, finds `packages/prisma/schema.prisma`, runs `prisma generate`, copies the eight per-app `.env.example` files, surfaces the `docker compose up -d` requirement for Postgres.
- **pydantic/pydantic:** mise picks up `requires-python` (3.9, EOL). devhelp picks 3.14 from the CI matrix max — what maintainers actually develop against.

## Sources
- README: `/Users/shivanshgupta/devhelp/README.md`
- CHANGELOG (the `[Unreleased]` v0.5 work): `/Users/shivanshgupta/devhelp/CHANGELOG.md`
- Differentiators: `/Users/shivanshgupta/devhelp/DIFFERENTIATORS.md`
- Stress test: `/Users/shivanshgupta/devhelp/stress-test/results.json`, `…/RETEST_RESULTS.md`
- Regression suite: `/Users/shivanshgupta/devhelp/run-regression.sh`
- "Why not mise?": `/Users/shivanshgupta/devhelp/docs/WHY-NOT-MISE.md`

## Connections
- [[Projects MOC]]
- [[DevHelp/v0.5 Capabilities & Validation]] — full feature + flag + architecture + validation detail
- Composes with [[Daily-active surfaces own monthly-active surfaces]]: devhelp is daily-CLI-frequency for OSS contributors
