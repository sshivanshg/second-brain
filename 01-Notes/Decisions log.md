---
created: 2026-05-24
type: log
tags: decisions, log
---

# 🧭 Decisions log

> Append-only record of meaningful decisions — technical, strategic, personal. Newest at top. Future-you will thank present-you for writing down the *why*, because the *why* is what you forget.

## Format
```
## YYYY-MM-DD — Short title
**Decision:** what you decided
**Why:** the reasoning / constraints at the time
**Alternatives considered:** what you rejected and why
**Revisit if:** the condition that would change this
```

---

## 2026-05-23 — Built a second brain in Obsidian
**Decision:** Centralize all project knowledge in an Obsidian vault at `~/Documents/SecondBrain`, separate from code repos.
**Why:** Knowledge was scattered across 130+ repos, docs, and my head. Needed one queryable place for *thinking* (strategy, decisions, connections) distinct from *code* (the repos).
**Alternatives considered:** Notion (heavier, less linkable, not local-first), keeping it in repo READMEs (couples thinking to code, doesn't cross-link).
**Revisit if:** vault maintenance becomes a chore that exceeds its value.

## 2026-05-24 — Flat project folder + frontmatter categories (not nested folders)
**Decision:** Keep `02-Projects` mostly flat; categorize via `categories:`/`audience:` frontmatter + auto-updating Dataview MOCs. Only Arth Saathi gets a deep subfolder.
**Why:** A project belongs to multiple categories (e.g. full-stack AND AI/ML AND freelance). Folders force one location; tags allow multi-membership and the MOCs stay current automatically.
**Alternatives considered:** Folder-per-category (breaks on multi-category projects), manual lists (rot immediately).
**Revisit if:** Obsidian Bases matures enough to replace the Dataview MOCs entirely.

---

> Add the next decision above this line.

## Related
- [[Home]] · [[Goals 2026]] · individual project working logs (e.g. [[Working log — Arth Saathi]])
