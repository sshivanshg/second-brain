---
created: 2026-05-24
type: category-moc
tags: moc, category
category: quant
---

# 📈 Quant / finance projects

> Anything touching market data, backtesting, trading, or financial alpha. Auto-populated from `categories:` containing `quant`.

## All quant projects
```dataview
TABLE WITHOUT ID
  file.link AS Project,
  audience AS Audience,
  status AS Status
FROM "02-Projects"
WHERE (type = "project" OR type = "project-moc") AND contains(categories, "quant")
SORT file.name ASC
```

## Lineage (oldest → newest)
1. **[[algo_dev — Indian stock market trading algorithm]]** — first attempt, TA + ML, Alpha Vantage
2. **[[StP — Single-stock XGBoost prediction]]** — disciplined walk-forward
3. **[[Qm Quant Research]]** — research framework, NIFTY backtest
4. **[[RAMT — Regime-Adaptive Multimodal Transformer]]** — capstone, foundation model

Each one is a better-engineered version of the prior — clean walking ladder of skill.

## Related
- [[_Categories index]] · [[AI ML projects]] · [[Research projects]]
