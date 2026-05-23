---
created: 2026-05-24
type: audience-moc
tags: moc, audience
audience: freelance
---

# 💼 Freelance projects (client / paid work)

> Client work — paid, scoped, delivered. Auto-populated from `audience:` containing `freelance`.

## All freelance projects
```dataview
TABLE WITHOUT ID
  file.link AS Project,
  categories AS Stack,
  status AS Status
FROM "02-Projects"
WHERE (type = "project" OR type = "project-moc") AND contains(audience, "freelance")
SORT file.name ASC
```

## Linked to their tech category
| Project | Primary stack | Category MOC |
|---------|--------------|--------------|
| [[e-Factory CMS]] | Full-stack TS monorepo + content workflow | [[Full Stack projects]] |
| [[TinyTales StoryGlow]] | AI + Full-stack (Next.js + Express + Fal.ai) | [[Full Stack projects]] · [[AI ML projects]] |
| [[Devops Ecom]] | E-commerce + DevOps | [[Full Stack projects]] |
| [[Boutique web projects]] | Boutique e-commerce sites | [[Frontend projects]] · [[Full Stack projects]] |
| [[Design Flow Studio]] | Design workflow tool | [[Full Stack projects]] |

## Related
- [[_Categories index]] · [[Startup projects]]
