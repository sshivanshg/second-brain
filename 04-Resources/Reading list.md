---
created: 2026-05-24
type: reference
tags: reference, reading, books
---

# 📚 Reading list

> Books, papers, essays. Apply the **Book** template (`Cmd+Shift+T` → Book) for anything you want to take notes on. This page rolls them up.

## 📖 Currently reading
```dataview
TABLE WITHOUT ID ("[[" + file.name + "]]") AS Title, author AS Author, rating AS Rating
FROM "04-Resources"
WHERE type = "book" AND status = "reading"
```

## ✅ Finished
```dataview
TABLE WITHOUT ID ("[[" + file.name + "]]") AS Title, author AS Author, rating AS Rating
FROM "04-Resources"
WHERE type = "book" AND status = "finished"
SORT rating DESC
```

## 🔖 Want to read
- 

## 📄 Papers / essays worth keeping
- _RAMT's research framing borrows from "honest ablation" practice — see [[RAMT — Regime-Adaptive Multimodal Transformer]]_

## How to add a book
`Cmd+N` in `04-Resources/`, insert the **Book** template, set `status: reading`. When done, set `status: finished` + a `rating`.

## Related
- [[Home]] · [[Spaced repetition beats rereading]]
