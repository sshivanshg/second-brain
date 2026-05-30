---
created: 2026-05-30
type: note
tags: systems, learning, atomic
---

# Earn the infra — adopt a tool only when you feel the problem it solves

Reaching for heavy infrastructure (Kafka, Rust, K8s, TimescaleDB) before you have the problem it solves is cargo-culting: you learn the tool's "hello world" and none of *why* it exists, and you build impressive plumbing with no value on top.

**The earn-it path:** build the simple version → hit a *real* limitation (too slow, won't scale, need streaming) → then adopt the tool. Now every concept maps to a pain you personally felt, so it sticks — and "I started simple, then re-architected for X" demonstrates engineering *judgment*, which is what actually gets hired.

**Seen in:** TradeOS deferred Docker/TimescaleDB/Kafka/Rust to a deliberate Phase 6, and ran Phase 0–1 on a plain local Postgres. Also why the SvelteKit frontend is deferred until the data model stabilises. → [[TradeOS]]
