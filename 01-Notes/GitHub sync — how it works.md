---
created: 2026-05-24
tags: meta, automation, guide
status: evergreen
---

# 🔄 GitHub sync — how it works

> Keeps the vault honest against your GitHub. Run it whenever you've been away; it tells you which repos need notes.

## Run it
```bash
cd ~/Documents/SecondBrain
python3 scripts/sync-github.py
```
Requires `gh` CLI authenticated (you already are, as `sshivanshg`).

**It only reports — never modifies.** Output:
- 🆕 **NEW repos** without a note
- 🍴 **New forks** (candidates for [[OSS contributions]])
- ✅ "in sync" when every non-trivial repo is documented

## Then document new ones
The script can't *understand* a repo — that's the AI's job. Tell Claude in this folder:
> "Add a project note for my repo `<name>`"

Claude fetches the README via `gh api repos/sshivanshg/<name>/readme`, writes a note with the right `categories:`/`audience:` frontmatter, and links it into the MOCs.

## How matching works (so you can tune it)
The script (`scripts/sync-github.py`) considers a repo "documented" if any vault note references it via:
1. A `github.com/sshivanshg/<repo>` URL
2. A `repo:` / `mirror:` local path whose basename matches
3. The `ALIASES` map (for renamed repos, e.g. `projectalpha` → Arth Saathi)

Repos matching `IGNORE_SUBSTRINGS` (class assignments, scratch, portfolio variants) are skipped. **Edit those two lists at the top of the script** when you rename a repo or want to start/stop ignoring something.

## Optional: schedule it
Two ways to make it automatic:
- **In Claude Code:** use `/schedule` to run a weekly routine — *"run the github sync and draft notes for any new repos."*
- **Cron:** add a weekly cron that runs the script and emails/logs the report.

## Related
- [[How to keep the vault updated]] · [[OSS contributions]] · [[Projects MOC]]
