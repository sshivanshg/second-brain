---
created: 2026-05-24
type: category-moc
tags: moc, category
category: devtool
---

# 🛠️ Dev Tools projects

> Things I built *for other developers*. Auto-populated from `categories:` containing `devtool`.

## All dev tools
```dataview
TABLE WITHOUT ID
  file.link AS Project,
  audience AS Audience,
  status AS Status
FROM "02-Projects"
WHERE (type = "project" OR type = "project-moc") AND contains(categories, "devtool")
SORT file.name ASC
```

## Related
- [[_Categories index]] · [[Systems projects]] · [[OSS contributions]]
