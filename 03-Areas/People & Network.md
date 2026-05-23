---
created: 2026-05-24
type: moc
tags: moc, people, crm
---

# 👥 People & Network

> Lightweight CRM. One note per person. Tag relationships, log interactions, never lose a follow-up. Especially load-bearing while [[Arth Saathi]] is fundraising.

## ➕ Add a person
`Cmd+N` in `03-Areas/People/`, apply the **Person** template (`Cmd+Shift+T` → Person), fill in `relationship`, `company`, `role`.

## 🤝 Everyone
```dataview
TABLE WITHOUT ID
  ("[[" + file.name + "]]") AS Person,
  role AS Role,
  company AS Company,
  relationship AS Relationship,
  last_contact AS "Last contact"
FROM "03-Areas/People"
WHERE type = "person"
SORT last_contact DESC
```

## 💰 Investors
```dataview
LIST FROM "03-Areas/People"
WHERE type = "person" AND relationship = "investor"
SORT last_contact DESC
```

## 🧑‍💻 Collaborators
```dataview
LIST FROM "03-Areas/People"
WHERE type = "person" AND relationship = "collaborator"
SORT last_contact DESC
```

## ⏰ Follow-ups due (open person tasks)
```tasks
not done
path includes 03-Areas/People
short mode
```

## 🕸️ Stale — no contact in 30+ days
```dataview
TABLE WITHOUT ID
  ("[[" + file.name + "]]") AS Person,
  (date(today) - last_contact).day + "d" AS "Since contact"
FROM "03-Areas/People"
WHERE type = "person" AND last_contact < date(today) - dur(30 days)
SORT last_contact ASC
```

## Related
- [[Investor pipeline]]
- [[Home]] · [[Goals 2026]]
