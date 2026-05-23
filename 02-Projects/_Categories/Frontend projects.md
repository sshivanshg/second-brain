---
created: 2026-05-24
type: category-moc
tags: moc, category
category: frontend
---

# 🎨 Frontend projects

> UI-only / marketing / static-site projects. Auto-populated from `categories:` containing `frontend`.

## All frontend projects
```dataview
TABLE WITHOUT ID
  file.link AS Project,
  audience AS Audience,
  status AS Status
FROM "02-Projects"
WHERE (type = "project" OR type = "project-moc") AND contains(categories, "frontend")
SORT file.name ASC
```

## Related
- [[_Categories index]] · [[Full Stack projects]]
