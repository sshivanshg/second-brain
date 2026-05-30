---
created: 2026-05-30
type: log
tags: tradeos, log
---

# Working log — TradeOS

> Append-only log of decisions, blockers, and shipped work. Newest at top.

## 2026-05-30

- **Connected the project to the vault** — created this TradeOS space (hub + log + Tech/Sources notes + atomic beliefs) and a repo↔vault documentation protocol in `repo/CLAUDE.md` so future sessions keep this log updated. Mirrors the Arth Saathi convention.

- **Shipped: horizon generalisation (`--horizon`)** — vol and VaR/CVaR can now be expressed over any horizon (d/w/m/q/y or `Nd`). → [[Estimate once, express at any horizon]]
  - **Insight:** annualising (×√252) is just a *unit*, not a calculation — the underlying risk is horizon-agnostic. So we estimate once (daily) and scale by **√time** (σ_T = σ_daily·√T).
  - **What scales vs what's invariant:** vol/VaR/CVaR scale by √horizon; **correlation, beta, and risk-contribution % are ratios → identical at every horizon.** Verified: book vol reads 1.09% daily / 2.44% weekly / 5.0% monthly / 17.34% annual, while beta 0.75, avg corr 0.31, effective holdings 4.58 don't move.
  - **Limits stay pinned to natural units** (annual vol, 1-day VaR) regardless of the display horizon, so the risk budget keeps its meaning. Caveat documented: √time assumes i.i.d. returns — fine short-term, weakens over long horizons.

- **Audited the whole engine** — `ruff` clean, **18 pytest tests** (math validated against independent calcs: EWMA cov reduces to numpy population cov at λ=1; CVaR ≥ VaR; risk contributions sum to 100%; no look-ahead), data integrity (0 nulls/non-positive, max daily move ≤10% confirms adjusted prices clean), edge cases (single holding, point-in-time). **0 correctness bugs, 0 data issues.** Test suite now guards every future change.

- **Rebuilt Risk Agent → v2 quant engine** (thinking like a buy-side desk). → [[Risk engine methodology — TradeOS]], [[Weight is not risk — manage the risk contributor]]
  - **Headline reframe:** v1 showed weights + standalone vol; v2 leads with **component risk contribution** — INFY is 31.7% of capital but **46.6% of risk**. You manage the risk *contributor*, not the biggest position.
  - Added: **EWMA(λ=0.94) covariance** (regime-aware, replaces flat stdev), **MCTR/%CTR** (Euler decomposition, sums to 100%), **historical VaR & CVaR** (95/99, fat-tail-honest), **correlation matrix + avg pairwise**, **days-to-liquidate**, **stress** (worst 1/5/10/21-day windows), and **risk-limit breach flags**.
  - LLM layer upgraded to a buy-side risk-manager persona; still **descriptive-only** (no buy/sell), still never computes a number.

- **Caught + fixed a data bug via the rebuild** — ingestion used raw (`auto_adjust=False`) prices, so splits/bonuses showed up as fake −45/−50% drawdowns and poisoned every vol/drawdown number. Switched to **total-return adjusted** (`auto_adjust=True`) and re-ingested. Worst-21-day went from a bogus −50% to a realistic −11.7%. → [[Validate the data, not just that it runs]]

- **Shipped: Phase 1 — Risk Agent v1** — pure-Python risk (concentration, vol, beta, drawdown) + a thin Claude narration layer (`messages.parse` + Pydantic `RiskReport`, descriptive). Evolved `holdings.txt` → `holdings.csv` (symbol, quantity, avg_cost) and added a NIFTY benchmark (`^NSEI`) for beta.

- **Shipped: Phase 0 — Data foundation** — Python/`uv` project, `yfinance` ingestion → idempotent UPSERT, a `tradeos-check` verifier, 2 years of daily prices for the book + benchmark. **2,985 rows.**
  - **Pivoted the data layer** — planned Docker + TimescaleDB, but Docker Desktop was down and a **native Homebrew Postgres already held port 5432**. Used that instead (dedicated `tradeos` DB, trust auth). Stock Postgres has no TimescaleDB extension → hypertables deferred to Phase 6. → [[Earn the infra — adopt a tool only when you feel the problem it solves]]

- **Wrote the learning-first roadmap** (`repo/ROADMAP.md`) — each phase ships something usable *and* teaches a skill; frontend deferred to Phase 5, heavy infra to Phase 6 (earn it, don't cargo-cult).

- **Reframed the whole idea: product → personal learning project.** Killed the SaaS framing after an honest review — SEBI regulates stock-specific advice, the combined signals likely lack real alpha, and the market is saturated. As a personal tool + portfolio piece the constraints lift and the project gets *better*. Decided to build it on my own holdings.

## Template entry
```
## YYYY-MM-DD
- What I shipped:
- What I decided:
- What's blocked:
- Next:
```

## Related
- [[TradeOS]]
