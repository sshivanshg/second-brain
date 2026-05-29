---
created: 2026-05-29
status: active
type: project
tags: project, cli, bun, devtool
categories: [devtool, systems]
audience: [personal]
repo: /Users/shivanshgupta/Projects/burrow
package: burrow
---

# 🐹 burrow

> **One-liner:** A [Mole](https://github.com/tw93/Mole)-inspired CLI that scans for dev junk (`node_modules`, `.next`, caches, build artifacts) and reclaims disk space — interactive and safe-by-default.

**Repo:** `/Users/shivanshgupta/Projects/burrow` · **Package:** `burrow` · **Binary:** `~/.local/bin/burrow` (on PATH)

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
