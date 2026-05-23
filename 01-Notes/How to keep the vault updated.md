---
created: 2026-05-24
tags: meta, guide, maintenance
status: evergreen
---

# How to keep the vault updated

> Read this once. The vault stays useful only if updating it is *cheaper than the value it gives back*. Everything below is designed to take under 2 minutes per action.

---

## 🟢 The 3 things you'll actually do

### 1. Add a NEW project
When you start or discover a project that deserves a note:

1. `Cmd+N` → name it (e.g. `My New App`)
2. Paste this frontmatter at the top and fill it in:
```yaml
---
created: 2026-05-24
status: active          # active | completed | archived | studying
type: project
tags: project
categories: [fullstack] # see legend in [[_Categories index]]
audience: [startup]     # startup | freelance | research | college | oss | personal
repo: /path/to/repo     # or github: https://github.com/...
---
```
3. Write a one-liner `> blockquote` summary, then a few sections (Stack, Features, Sources, Connections).
4. **That's it.** It auto-appears in [[Projects MOC]] and the right [[_Categories index]] MOC. No manual list editing.

> 💡 The `categories` and `audience` fields are what power every category MOC. Get them right and the wiring is automatic.

### 2. Re-categorize an existing project
Just edit its `categories:` or `audience:` line in the frontmatter. The Dataview MOCs re-query live. Example: when a side project becomes a real startup, change `audience: [personal]` → `audience: [startup]`.

### 3. Capture a thought / decision
- **Daily:** open today's note (`Cmd+Shift+D`), dump it under "Notes & captures."
- **Project decision:** add a dated entry to that project's working log (Arth Saathi has [[Working log — Arth Saathi]]; copy that pattern for others).
- **Reusable insight:** make an atomic note in `01-Notes/` titled as a claim, link it to 2+ notes.

---

## 🔄 Keeping project notes in sync with the code

The repo is **source of truth for code**; the vault is **source of truth for thinking**. They drift. To re-sync:

### Quick manual refresh
When a project changes materially (new feature, pivot, shipped milestone):
1. Open the project note
2. Update the one-liner + Stack/Features if they changed
3. Add a working-log entry with the date

### Bulk refresh from GitHub (when you've been away a while)
Ask me (Claude) in this project directory:
> "Re-scan my GitHub repos and tell me which project notes are stale or which new repos need notes."

I can run `gh repo list` + diff against existing notes. I can't *know* what changed semantically — but I can flag new repos, renamed repos, and repos pushed-to since the note's `created` date.

### What NOT to mirror into the vault
- Source code (it's in the repo)
- Git history (use `git log`)
- Exhaustive API specs (link to the repo doc instead)
- Anything that rots fast — link to it, don't copy it

---

## 🗓️ The rhythm (what to do when)

| Cadence | Action |
|---------|--------|
| **Daily** | Open daily note, capture. 30 seconds. |
| **Weekly** | Run [[Inbox processing checklist]] + [[Templates/Weekly-Review]]. Empty inbox, update active project statuses. |
| **Monthly** | Skim [[Projects MOC]]. Mark finished projects `status: archived`. Promote any inbox idea that became real. |
| **When away 1+ month** | Ask me to re-scan GitHub for new/changed repos. |

---

## 🧹 Hygiene rules

1. **Status discipline.** Keep `status:` honest: `active` → `completed` → `archived`. Dead projects with `active` status pollute every view.
2. **One idea per atomic note.** If a note in `01-Notes/` grows two distinct ideas, split it.
3. **Every note links to 2+ others.** Orphan notes are invisible. The graph view (`Cmd+Shift+G`) shows orphans — fix them.
4. **Don't create folders per project** (except deep ones like Arth Saathi). Flat + frontmatter categories scales better than nested folders.
5. **Re-categorize freely.** Frontmatter edits are cheap; the MOCs follow.

---

## ⚡ Cheat sheet — common updates

| I want to… | Do this |
|------------|---------|
| Add a project | `Cmd+N`, paste frontmatter, set `categories`+`audience` |
| Move a project to freelance | Edit `audience: [freelance]` |
| Mark a project done | Edit `status: completed` |
| Find all my AI projects | Open [[AI ML projects]] |
| Find all client work | Open [[Freelance projects]] |
| See everything | Open [[Projects MOC]] |
| Log a decision | Add dated bullet to the project's working log |
| Capture a fleeting idea | Today's daily note → "Ideas" |
| Refresh from GitHub | Ask Claude to re-scan repos |

---

## 🤖 What Claude can do for you later

In this directory, you can ask me to:
- "Add a note for my new repo `xyz`" → I'll fetch its README and write the note
- "Re-scan GitHub, flag stale notes" → I'll diff repos vs notes
- "Deepen the [[LexVault]] note" → I'll read the repo and expand it
- "Make an atomic note from this idea: …" → I'll write + link it
- "What projects haven't I touched in 3 months?" → I'll check `status` + git activity

## Related
- [[How to use this vault]] — the basics
- [[_Categories index]] — category + audience legend
- [[Inbox processing checklist]] — the weekly ritual
- [[Projects MOC]] — the master list
