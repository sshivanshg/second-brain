---
created: 2026-05-24
type: pipeline
tags: arth-saathi, fundraising, pipeline
---

# 💸 Investor pipeline — Arth Saathi

> Track every investor conversation through stages. Round: **USD 1.5M pre-seed** (see [[Pre-seed ask — USD 1.5M]]).

## Stages
`Researching → Intro requested → First meeting → Diligence → Term sheet → Closed / Passed`

## Pipeline table
> Add each investor as a [[People & Network|person note]] with `relationship: investor` and a `stage:` field, or track inline here.

| Investor | Firm | Stage | Check size | Last touch | Next step |
|----------|------|-------|-----------|-----------|-----------|
| [[Artem Luko]] | Angel (solo) | Researching · track/no-ask | $25k–$3M | 2026-06-04 | Send soft-open DM + comment (no deck, no ask) |

## Pipeline by stage (from person notes)
```dataview
TABLE WITHOUT ID
  ("[[" + file.name + "]]") AS Investor,
  company AS Firm,
  stage AS Stage,
  last_contact AS "Last touch"
FROM "03-Areas/People"
WHERE type = "person" AND relationship = "investor"
SORT last_contact DESC
```

## Open fundraising tasks
```tasks
not done
path includes Arth Saathi/Fundraising
short mode
```

## The pitch in one breath
> AI-first, Hindi/Hinglish OS for India's 5–50 employee shops. Wedge = daily attendance; valuation engine = embedded fintech. ₹0 entry, monetize capability. The compliance integration is the moat LLMs can't eat.

Supporting notes: [[Market — TAM SAM SOM and growth]] · [[Business model — subscription + embedded fintech]] · [[Inflection triggers — why now]] · [[Competitors — landscape and gaps]]

## Materials
- Deck: `/Users/shivanshgupta/projects/arth/Arth_Saathi_Pitch_Deck.pptx`
- Master context: `/Users/shivanshgupta/projects/arth/ARTH_SAATHI_CONTEXT.md`

## Related
- [[Pre-seed ask — USD 1.5M]] · [[People & Network]] · [[Arth Saathi]]
