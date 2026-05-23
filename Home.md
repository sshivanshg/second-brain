---
created: 2026-05-23
type: moc
tags: home, moc, dashboard
---

# 🧠 Command Center

> *Repos are source-of-truth for code. This vault is source-of-truth for thinking.* — Shivansh's second brain.

## 🎯 Right now
![[Now#Focus]]
> Quick links: [[Goals 2026]] · [[Decisions log]] · [[Tasks dashboard]] · [[Tech stack map]] · [[People & Network]]

---

## ⚡ Quick capture
- 📥 New to inbox → `Cmd+N` (defaults to `00-Inbox`)
- 📅 Today's note → `Cmd+Shift+D`
- 🔍 Search → `Cmd+Shift+F`
- 🎲 Random note → `Cmd+Shift+R`

---

## 🔥 Active projects
```dataview
TABLE WITHOUT ID
  ("[[" + file.name + "]]") AS Project,
  categories AS Stack,
  audience AS For
FROM "02-Projects"
WHERE (type = "project" OR type = "project-moc") AND status = "active"
SORT file.mtime DESC
```

## ✅ Open tasks across all projects
```tasks
not done
path does not include Templates
limit 15
short mode
```

## ⚠️ Needs attention — not touched in 45+ days
```dataview
TABLE WITHOUT ID
  ("[[" + file.name + "]]") AS Project,
  status AS Status,
  (date(today) - file.mtime).day + "d ago" AS "Last touched"
FROM "02-Projects"
WHERE (type = "project" OR type = "project-moc") AND status = "active" AND file.mtime < date(today) - dur(45 days)
SORT file.mtime ASC
```

## 🆕 Recently edited
```dataview
TABLE WITHOUT ID
  ("[[" + file.name + "]]") AS Note,
  file.folder AS Where,
  dateformat(file.mtime, "MMM dd") AS Edited
FROM "" 
WHERE file.name != "Home"
SORT file.mtime DESC
LIMIT 8
```

---

## 🗺️ Navigate

### Projects
- **[[Projects MOC]]** — every project, flat list
- **[[_Categories index]]** — browse by tech (full-stack, AI/ML…) or purpose (startup, freelance…)
- **[[College projects MOC]]** · **[[OSS contributions]]** · **[[Coursework + lab assignments]]**

### Knowledge
- **[[How to use this vault]]** · **[[How to keep the vault updated]]**
- **[[Tech stack map]]** — what I use, where
- **[[Reading list]]** — books & resources
- **[[Decisions log]]** — why I chose what I chose

### Life / direction
- **[[Now]]** — current focus
- **[[Goals 2026]]** — annual targets
- **[[People & Network]]** — investors, collaborators, mentors
- **[[Personal profile — Shivansh Gupta]]**

---

## 🧠 Reusable beliefs (the thinking that compounds)
```dataview
LIST
FROM "01-Notes"
WHERE status = "evergreen" AND !contains(file.name, "How to") AND !contains(file.name, "Inbox") AND !contains(file.name, "Decisions")
SORT file.name ASC
```

---

## 📊 Vault stats
```dataview
TABLE WITHOUT ID
  length(rows) AS Count
FROM "02-Projects" OR "01-Notes"
WHERE type = "project" OR type = "project-moc" OR status = "evergreen"
GROUP BY (type = "project" OR type = "project-moc") AS IsProject
```

> The loop: **Capture → Process → Connect → Review.** Weekly review via [[Templates/Weekly-Review]].
