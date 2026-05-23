---
created: 2026-05-24
type: audience-moc
tags: moc, audience
audience: oss
---

# 🌐 OSS projects

> Open source — either projects I own that are OSS, or contributions to others. Auto-populated from `audience:` containing `oss`.

## Owned OSS
```dataview
TABLE WITHOUT ID
  file.link AS Project,
  categories AS Stack
FROM "02-Projects"
WHERE (type = "project" OR type = "project-moc") AND contains(audience, "oss")
SORT file.name ASC
```

## Contributions to other people's OSS
See **[[OSS contributions]]** — full catalog of forks across CNCF, Bitcoin, GSoC, LFX, openSUSE.

## Related
- [[_Categories index]] · [[OSS contributions]]
