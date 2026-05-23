---
created: 2026-05-24
type: audience-moc
tags: moc, audience
audience: college
---

# 🎓 College projects (audience view)

> Anything built in academic context — capstone, course project, lab. Auto-populated from `audience:` containing `college`.

## All college projects
```dataview
TABLE WITHOUT ID
  file.link AS Project,
  categories AS Stack,
  status AS Status
FROM "02-Projects"
WHERE (type = "project" OR type = "project-moc") AND contains(audience, "college")
SORT file.name ASC
```

## See also
- **[[College projects MOC]]** — narrative view (by research/quant/product/etc.)
- **[[Coursework + lab assignments]]** — the long-tail class repos

## Related
- [[_Categories index]] · [[Research projects]]
