---
created: 2026-05-24
type: audience-moc
tags: moc, audience
audience: research
---

# 🔬 Research projects

> Academic / capstone / publishable work. Auto-populated from `audience:` containing `research`.

## All research projects
```dataview
TABLE WITHOUT ID
  file.link AS Project,
  categories AS Stack,
  status AS Status
FROM "02-Projects"
WHERE (type = "project" OR type = "project-moc") AND contains(audience, "research")
SORT file.name ASC
```

## What good research looks like (my pattern)
Both [[RAMT — Regime-Adaptive Multimodal Transformer]] and [[SpatiaLaw — WiFi CSI presence detection]] share an **honest framing** discipline:
- Headline the actual finding (not the best-case)
- Show the failed variant alongside the successful one (RAMT's Phase 2 transformer)
- Quote exact metrics with the test window
- Explain *why* the finding generalizes (or doesn't)

This is rarer than it should be. Use it as the template for any future research write-up.

## Related
- [[_Categories index]] · [[AI ML projects]] · [[College projects MOC]]
