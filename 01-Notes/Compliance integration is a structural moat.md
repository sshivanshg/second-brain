---
created: 2026-05-23
tags: moat, compliance, b2b
status: evergreen
---

# Compliance integration is a structural moat

> The work LLMs can't do is the work that defends the business.

## Context
"AI will eat all software" is the standard fear in B2B SaaS. The counter: AI can't log into the EPFO Unified Shram Suvidha Portal. It can't reconcile state-specific Professional Tax slabs. It can't generate a valid ECR file. It can't allot a UAN.

That kind of compliance integration:
- Requires multi-month engineering per regulatory regime
- Requires custodianship of credentials (Aadhaar-linked UAN, ESIC ID, TAN)
- Requires per-state knowledge (PT slabs differ across 28+ states)
- Requires the regulator's trust (which takes years to build)

## Why this beats feature competition
Once a shop owner files even one PF / ESI / TDS return through your product, **switching vendors means re-onboarding every regulatory identity**. That's months of pain. The lock-in is structural, not contractual.

This is the opposite of horizontal AI plays (Meta WhatsApp Business AI, OpenAI Operator). They have plumbing; they don't have the regulator relationships.

## How to build the moat deliberately
1. Pick a regulatory regime that's mandatory for your ICP
2. Build the integration end-to-end (not just the UI)
3. Take custodianship of the credentials with consent
4. File once → user is locked in

## Implication
B2B AI products that don't build at this layer are easy to displace. The compliance integration is the *substrate* for the AI features, not a bolt-on.

## Connections
- Applied in: [[Arth Saathi]] · [[Ideology — what we believe that competitors don't]]
- Related: [[Inflection triggers — why now]] (Labour Codes 2020 going live in April 2026)
