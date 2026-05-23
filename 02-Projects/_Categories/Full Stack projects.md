---
created: 2026-05-24
type: category-moc
tags: moc, category
category: fullstack
---

# 🧱 Full Stack projects

> Projects where you own both UI and server. Auto-populated from any project with `categories:` containing `fullstack`.

## All full-stack projects
```dataview
TABLE WITHOUT ID
  file.link AS Project,
  audience AS Audience,
  status AS Status
FROM "02-Projects"
WHERE (type = "project" OR type = "project-moc") AND contains(categories, "fullstack")
SORT file.name ASC
```

## Just the freelance ones
```dataview
LIST FROM "02-Projects"
WHERE (type = "project" OR type = "project-moc") AND contains(categories, "fullstack") AND contains(audience, "freelance")
SORT file.name ASC
```

## Just the startups
```dataview
LIST FROM "02-Projects"
WHERE (type = "project" OR type = "project-moc") AND contains(categories, "fullstack") AND contains(audience, "startup")
SORT file.name ASC
```

## Related
- [[_Categories index]]
- Other categories: [[Backend projects]] · [[Frontend projects]] · [[AI ML projects]]
