---
created: 2026-05-24
type: category-moc
tags: moc, category
category: backend
---

# 🔌 Backend projects

> Server / API / database projects. Auto-populated from `categories:` containing `backend`.

## All backend projects
```dataview
TABLE WITHOUT ID
  file.link AS Project,
  audience AS Audience,
  status AS Status
FROM "02-Projects"
WHERE (type = "project" OR type = "project-moc") AND contains(categories, "backend")
SORT file.name ASC
```

## Common patterns observed
- Express + JWT bearer (HealthPro, zenova, College Appointment, BioPay)
- Microservices split (BioPay)
- Prisma + Postgres (CMS, BioPay)
- Mongoose + MongoDB (College Appointment)

## Related
- [[_Categories index]] · [[Full Stack projects]] · [[Systems projects]]
