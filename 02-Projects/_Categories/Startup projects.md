---
created: 2026-05-24
type: audience-moc
tags: moc, audience
audience: startup
---

# 🚀 Startup projects (my own ventures)

> My own products with real users, fundraising, or open-source distribution intent. Auto-populated from `audience:` containing `startup`.

## All startup projects
```dataview
TABLE WITHOUT ID
  file.link AS Project,
  categories AS Stack,
  status AS Status
FROM "02-Projects"
WHERE (type = "project" OR type = "project-moc") AND contains(audience, "startup")
SORT file.name ASC
```

## Active fundraise
- **[[Arth Saathi]]** — USD 1.5M pre-seed open ([[Pre-seed ask — USD 1.5M]])

## Related
- [[_Categories index]] · [[Freelance projects]]
