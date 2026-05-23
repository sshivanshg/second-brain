---
created: 2026-05-24
type: audience-moc
tags: moc, audience
audience: personal
---

# 🏠 Personal projects

> Hobby / craft / portfolio / family. Auto-populated from `audience:` containing `personal`.

## All personal projects
```dataview
TABLE WITHOUT ID
  file.link AS Project,
  categories AS Stack,
  status AS Status
FROM "02-Projects"
WHERE (type = "project" OR type = "project-moc") AND contains(audience, "personal")
SORT file.name ASC
```

## Related
- [[_Categories index]]
