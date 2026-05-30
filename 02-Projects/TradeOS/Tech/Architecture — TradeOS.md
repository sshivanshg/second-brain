---
created: 2026-05-30
type: note
tags: tradeos, tech, architecture
---

# Architecture — TradeOS

> How the system is put together as of Phase 1. Source-of-truth for code is the repo; this is the map. See [[Source-of-truth pointers — TradeOS]].

## Layers

```
DATA SOURCES  (yfinance daily OHLCV: split-adjusted close + total-return adj_close, holdings + ^NSEI)
   │  ingestion (Python, idempotent UPSERT)
   ▼
STORAGE       local Homebrew Postgres, db `tradeos`, table `prices(symbol,date,ohlc,adj_close,volume)`
   │  point-in-time reads (date <= as_of)
   ▼
RISK ENGINE   risk.py — pure Python/pandas/numpy (the FACTS layer, deterministic, tested)
   │  structured dict
   ▼
LLM AGENT     risk_agent.py — Claude messages.parse + Pydantic RiskReport (NARRATION only)
   │
   ▼
CLI           tradeos-risk  (--horizon, --as-of, --no-llm)
```

## Repo layout (`/Users/shivanshgupta/projects/tradeos`)
- `src/tradeos/config.py` — env-driven settings, `Position` + portfolio loader, `RISK_LIMITS`.
- `src/tradeos/db.py` — single Postgres connection helper.
- `src/tradeos/ingest.py` — yfinance → UPSERT (`tradeos-ingest`).
- `src/tradeos/check.py` — row-count verifier (`tradeos-check`).
- `src/tradeos/risk.py` — the quant engine (vol, EWMA covariance, component risk, VaR/CVaR, stress, liquidity, limits, horizon scaling).
- `src/tradeos/risk_agent.py` — Claude narration (buy-side risk-manager persona, descriptive-only).
- `src/tradeos/technical.py` — Technical agent: per-stock indicators (SMA/EMA, RSI, MACD, returns, volume) + descriptive dials.
- `src/tradeos/orchestrator.py` — multi-agent orchestrator: runs risk + technical, builds per-stock cards, parallel Claude synthesis (`tradeos-analyze`).
- `src/tradeos/cli.py` — `tradeos-risk` command.
- `tests/test_risk.py` — 18 tests (math validated independently + live invariants).
- `holdings.csv` — the portfolio (symbol, quantity, avg_cost).
- `ROADMAP.md` — the phased learning plan.

## Key design rules
- **LLM never computes, only explains** — facts come from `risk.py`; the model can't invent a number.
- **Descriptive, not advisory** — risk is explained, never "buy/sell/hold". (For my own use, but keeps the boundary clean.)
- **Estimate once, express at any horizon** — see [[Estimate once, express at any horizon]].
- **Point-in-time everything** — `as_of` filters every query; the seed of the Phase 4 eval harness.
- **Earn the infra** — Docker/TimescaleDB/Kafka/Rust deferred until a real need is felt → [[Earn the infra — adopt a tool only when you feel the problem it solves]].

## Stack rationale
- **Local Postgres over Docker/Timescale** (for now): Docker was down, a native PG already ran on 5432, and stock PG lacks the timescaledb extension. Plain table + good indexing covers the query patterns at this scale.
- **Claude `messages.parse` + Pydantic** for structured narration; default model `claude-opus-4-8`, overridable via `CLAUDE_MODEL`.

## Related
- [[TradeOS]] · [[Risk engine methodology — TradeOS]] · [[Working log — TradeOS]]
