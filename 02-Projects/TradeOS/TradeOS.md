---
created: 2026-05-30
status: active
type: project-moc
tags: project, quant, ai-ml, tradeos, personal
categories: [quant, ai-ml, backend]
audience: [personal, college]
repo: /Users/shivanshgupta/projects/tradeos
---

# 📈 TradeOS

> **One-liner:** a personal, multi-agent **portfolio-intelligence** system — you feed it your real holdings, a swarm of analyzer agents reads risk/technicals/earnings/macro, and it gives you a *scored, fully-traced* read of your book. Built to be **useful to me**, to **learn agentic LLM + quant systems**, and as a **hireable showcase** — explicitly *not* a product.

**Status:** Active build · Phase 0 (data) ✓ · Phase 1 (Risk Agent, quant engine) ✓
**Repo:** `/Users/shivanshgupta/projects/tradeos`
**Stack:** Python + `uv` · local Homebrew Postgres · Claude API (`messages.parse` + Pydantic) · pytest

## 🧭 Quick nav

### 🧱 Tech
- [[Architecture — TradeOS]]
- [[Risk engine methodology — TradeOS]]

### 📚 Sources
- [[Source-of-truth pointers — TradeOS]]

### 📓 Log
- [[Working log — TradeOS]]

## 🧠 Framing (decided — do not relitigate)
- **Personal learning project, NOT a product.** The SaaS framing was killed deliberately: SEBI regulates stock-specific buy/sell advice, the combined signals likely have no real alpha, and the market (Trendlyne/Sensibull/Tickertape) is saturated.
- For my own use, verdicts are fine — but agents stay **descriptive** (explain risk, never command). I make the call.
- Goals, in order: useful to me · learn agentic LLM systems · learn infra · hireable showcase.

## 🗺️ Roadmap (in `repo/ROADMAP.md`)
- **Phase 0 — Data foundation** ✓ (prices in my own DB)
- **Phase 1 — Risk Agent** ✓ (quant engine + Claude narration)
- **Phase 2 — Multi-agent core** (technical / earnings / macro agents + orchestrator)
- **Phase 3 — RAG** (concall/filing intelligence)
- **Phase 4 — Honest eval harness** (does any signal actually predict?)
- **Phase 5 — SvelteKit frontend** (deferred on purpose — build once, against a stable schema)
- **Phase 6 — Infra, deliberately earned** (Rust / Kafka / K8s / TimescaleDB)

## 🧠 Load-bearing beliefs (atomic — reusable)
- [[Weight is not risk — manage the risk contributor]]
- [[Estimate once, express at any horizon]]
- [[Validate the data, not just that it runs]]
- [[Earn the infra — adopt a tool only when you feel the problem it solves]]

## 📊 Live dataview — recent TradeOS notes
```dataview
LIST FROM "02-Projects/TradeOS"
SORT file.mtime DESC
LIMIT 15
```
