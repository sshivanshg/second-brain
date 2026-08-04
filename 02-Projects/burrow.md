---
created: 2026-05-29
updated: 2026-08-04
status: active
type: project
tags: project, cli, bun, devtool
categories: [devtool, systems]
audience: [personal]
repo: /Users/shivanshgupta/Projects/burrow
github: https://github.com/sshivanshg/burrowed
package: burrowed
version: 0.3.1
---

# 🐹 burrow → burrowed

> **One-liner:** Mole-inspired macOS cleaner CLI — reclaim disk from caches, DerivedData, duplicates, dormant apps; animated mole TUI. **Shipped as `burrowed` v0.3.1** via Homebrew.

**Repo:** `/Users/shivanshgupta/Projects/burrow` · **GitHub:** [sshivanshg/burrowed](https://github.com/sshivanshg/burrowed) · **Install:** `brew tap sshivanshg/burrowed && brew install burrowed`

## 📝 Log
### 2026-06-04
- Renamed project burrow → **burrowed**; release pipeline + Homebrew formula
- v0.3.0 → **v0.3.1** (menu ghost-frame fix, score badge spacing)
### 2026-08-04
- Local dirty: `docker.ts`, `dormant-apps.ts` cleaners — uncommitted polish

## 🎯 Goal
A one-command way to find and delete reclaimable dev junk across my projects, without ever risking real source/data.

## ⚙️ How it works
`scan → measure (du -sk) → interactive multi-select → confirm → delete`, with the running total of reclaimable space shown up front. `--list` and `--dry-run` let me look without touching anything.

## 🧱 Stack
- **Runtime:** Bun + TypeScript (single file, `index.ts`)
- **UI:** `@clack/prompts` (multi-select, spinner, notes)
- **Colors:** `picocolors`
- **Sizing:** `du -sk`, batched
- **Distribution:** `bun build --compile` → standalone binary copied to `~/.local/bin`

## 🔒 Safety design
- Pre-checks only low-risk items; `dist`/`build`/`target`/`venv` start **unchecked** (opt-in)
- Guardrails refuse to delete the scan root, `$HOME`, `/`, anything outside the root, or any non-junk folder name
- Skips `Library`/`.git`/`.Trash`, never follows symlinks, always confirms before deleting

## 📁 Repo structure
- `index.ts` — the whole tool (scan, size, TUI, delete) with exported helpers behind `import.meta.main`
- `burrow.test.ts` — Bun tests for scanning + deletion guardrails (4 passing)
- `package.json` · `README.md`

## ⌨️ Commands
```bash
burrow                 # scan current folder (interactive)
burrow ~/Projects -l   # list findings + total, no prompts
burrow . --min 50      # only junk ≥ 50 MB
burrow ~/Projects -n   # dry run
```

## 📝 Log
### 2026-05-29
- Built v0.1 — scanning, sizing, interactive cleaner, `--list`/`--dry-run`, deletion guardrails, 4 passing tests, compiled binary installed to PATH.
- Live test found **388 MB** reclaimable in `~/Projects`.

## 💡 Ideas / Next
- Live system dashboard (CPU/mem/disk IO) TUI
- App uninstaller (hunt down leftover `Application Support`/prefs/Launch Agents)
- `--json` output mode · push to GitHub (`sshivanshg`) · Homebrew tap

## 📚 Sources
- `/Users/shivanshgupta/Projects/burrow/README.md`
- `/Users/shivanshgupta/Projects/burrow/index.ts`

## 🔗 Connections
- [[Projects MOC]] · [[Dev Tools projects]] · [[OS File Organizer]]
