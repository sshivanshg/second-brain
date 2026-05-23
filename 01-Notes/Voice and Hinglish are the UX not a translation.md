---
created: 2026-05-23
tags: ux, ai, bharat
status: evergreen
---

# Voice and Hinglish are the UX, not a translation

> "Hindi support" is a settings toggle. Voice-first Hinglish is a different product entirely.

## Context
Most competitors add language support as a translation layer over an English-shaped form UI. The cognitive load of switching between English form labels and Hindi placeholder text is *worse* than English-only. The owner abandons.

The right approach: the owner says *"Ramesh ki haziri laga do, aaj half day"* and the work happens. Forms are the failure mode, not the default.

## Why this works now (and didn't 3 years ago)
- Sarvam Saaras V3 hits **19% WER on Hinglish** (good enough for production)
- Claude Opus 4.5 hits **77% BFCL V4** on tool calling
- Vernacular LLM inference cost collapsed **94.5% in 36 months**

## Implication
- Default surface = chat (text or voice)
- Forms exist only as a confirmation layer ([[Confirm before write — AI mutations need human approval]])
- 57% of urban Indian internet users prefer Indic over English (IAMAI–Kantar 2024)
- 98% of Indian internet users access Indic-language content

## Connections
- Related: [[Bharat shop owner IS the HR department]]
- Related: [[Confirm before write — AI mutations need human approval]]
- Applied in: [[Arth Saathi]]
