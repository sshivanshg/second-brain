---
created: 2026-05-24
type: category-moc
tags: moc, category
category: systems
---

# 🖥️ Systems / Core CS projects

> OS, desktop, harness, low-level, microservices, signal processing. Things that live close to the metal or to the OS abstraction. Auto-populated from `categories:` containing `systems`.

## All systems projects
```dataview
TABLE WITHOUT ID
  file.link AS Project,
  audience AS Audience,
  status AS Status
FROM "02-Projects"
WHERE (type = "project" OR type = "project-moc") AND contains(categories, "systems")
SORT file.name ASC
```

## What "systems" covers here
- **Desktop / native:** OS File Organizer (Electron)
- **Harness / agent runtime:** Claw Code rewrite study
- **Microservices architecture:** BioPay
- **Signal processing:** SpatiaLaw (WiFi CSI)
- **CLI / dev tooling:** DevHelp

## Related coursework (not in main list)
From [[Coursework + lab assignments]]:
- `230054-midsem` — C midsem
- `OS/raw_keyboard.c` — raw keyboard input in C
- `File-System` (mock test AP)

## Related
- [[_Categories index]] · [[Dev Tools projects]]
