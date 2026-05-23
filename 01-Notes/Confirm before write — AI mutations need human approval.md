---
created: 2026-05-23
tags: ai, architecture, trust
status: evergreen
---

# Confirm before write — AI mutations need human approval

> Efficiency-only AI fails on edge cases. The trust cost of a wrong write is higher than the latency cost of a confirmation tap.

## Context
Klarna fired humans for AI in 2024, then re-hired in 2025 because the AI couldn't handle edge cases without supervision. The lesson isn't "AI is bad" — it's that **mutations without confirmation destroy trust irreversibly**.

For products where wrong data costs the user real money (payroll, billing, attendance), the architecturally correct pattern is:

1. AI agent resolves intent → calls tool with proposed mutation
2. Tool returns a `ConfirmationCard` (no write yet)
3. Human taps Confirm or Reject
4. Only on Confirm does the mutation commit, with an `AuditLog` row

## Implication
- All tools in your agent are **proposals**, not actions
- The UI surface for confirmation is more important than the prompt
- Audit log is non-optional
- Latency budget includes the human confirmation tap — design for it

## Why competitors don't do this
It's harder. They ship faster by writing on intent and apologizing later. They burn trust faster too.

## Connections
- Related: [[Voice and Hinglish are the UX not a translation]]
- Applied in: [[Arth Saathi]] · [[Architecture — Arth Saathi]]
- Related: [[Atomic notes compose better than long notes]] (also about modularity over monoliths)
