---
created: 2026-05-24
type: category-moc
tags: moc, category
category: ai-ml
---

# 🤖 AI / ML projects

> Anywhere AI or ML is the primary value. Auto-populated from `categories:` containing `ai-ml`.

## All AI/ML projects
```dataview
TABLE WITHOUT ID
  file.link AS Project,
  audience AS Audience,
  status AS Status
FROM "02-Projects"
WHERE (type = "project" OR type = "project-moc") AND contains(categories, "ai-ml")
SORT file.name ASC
```

## AI/ML × Research (the rigorous ones)
```dataview
LIST FROM "02-Projects"
WHERE (type = "project" OR type = "project-moc") AND contains(categories, "ai-ml") AND contains(audience, "research")
```

## AI/ML × Startup
```dataview
LIST FROM "02-Projects"
WHERE (type = "project" OR type = "project-moc") AND contains(categories, "ai-ml") AND contains(audience, "startup")
```

## Shared technical patterns
- **Confirm-before-write** (Arth Saathi, LexVault) — see [[Confirm before write — AI mutations need human approval]]
- **LangGraph orchestration** (Arth Saathi, Agora)
- **RAG with line-level citations** (LexVault)
- **Foundation model fine-tune + LoRA** (RAMT)
- **Random Forest with strong feature engineering** (SpatiaLaw)

## Related
- [[_Categories index]] · [[Quant projects]] · [[Research projects]]
- Atomic belief: [[Confirm before write — AI mutations need human approval]]
