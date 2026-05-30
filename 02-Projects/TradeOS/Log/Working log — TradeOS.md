---
created: 2026-05-30
type: log
tags: tradeos, log
---

# Working log — TradeOS

> Append-only log of decisions, blockers, and shipped work. Newest at top.

## 2026-05-30

- **Phase 3 (a): structured Fundamental agent — shipped.** Probed yfinance first (the responsible move): RELIANCE returns ₹-crore quarterlies, **INFY returns USD** (its ADR) — so absolute revenue isn't cross-comparable. Built the agent to compute **ratios only** (currency-invariant): revenue/earnings YoY & QoQ growth, net/operating margins, margin trend → descriptive dials (growth strong/growing/flat/declining; margin expanding/stable/contracting). **Calendar-quarter YoY matching** (robust to yfinance's quarter gaps). Quarterly data ingested into a new `fundamentals` table so `analyze` stays offline/fast. Skipped EPS (yfinance returns 0) and `earnings_dates` (needs lxml) — Net Income growth is the reliable earnings signal. The orchestrator now runs **three agents** (risk + technical + fundamental) per card. Sanity-checked vs reality: INFY net margin ~18%, TCS ~19% — correct. **33 tests, ruff clean.**
  - **Caveat for Phase-4 eval:** fundamentals are filtered by `period_end` but were only *known* ~30–45d later (results-announcement lag) — a look-ahead to handle when back-testing fundamental signals.
  - **Next: Phase 3 (b)** — RAG "ask-the-call" over concall/results PDFs the user supplies.

- **Unified `tradeos` CLI + portfolio is now user-managed (demo data removed).** New `tradeos add / remove / holdings` commands (alongside `ingest / check / risk / analyze`) — type your stocks in the terminal and `add` **auto-fetches that symbol's data**, or edit `holdings.csv` directly (same source of truth, both stay in sync). **Wiped the hardcoded demo holdings**: `holdings.csv` is now empty + gitignored (real portfolio stays private), `holdings.example.csv` is the template. Ingestion is incremental (`ingest_symbols` fetches just what you add). **29 tests** (added portfolio add/remove + a conftest so integration tests run against the example portfolio, not the user's file).
  - **Clarified a design point:** the portfolio was *never* meant to be hardcoded — `holdings.csv` is the user input; the example rows were only build-time seed. Data source stays **yfinance** for now (right tool for daily EOD on a few names) but is isolated in one `fetch_ohlcv` adapter, so swapping to NSE bhavcopy / a paid feed later is a one-file change. → [[Architecture — TradeOS]]

- **Senior-quant model-validation pass + 2 fixes.** Independently recomputed every risk number from raw DB data with *different* methods (full-sample OLS, manual VaR, manual SMA) and diffed vs the engine. VaR/CVaR matched to the basis point, SMA/last-price exact, risk contribution structurally identical, vol coherent (EWMA > realized = current regime). Two genuine issues found and fixed:
  1. **Betas were noise.** They'd inherited the short EWMA window (~32 effective days) meant for volatility → jittery sensitivities (HDFCBANK 1.52, INFY 0.42, book 0.75). Beta is a *structural* exposure, so it now uses the **full sample** with **Bloomberg-style shrinkage toward 1** (β_adj = ⅔·raw + ⅓). Result: stable, believable betas (HDFCBANK 1.01, INFY 0.88, **book 0.89**) that cross-check exactly. Vol stays EWMA — you *want* current-regime vol. → [[Risk engine methodology — TradeOS]]
  2. **Technicals & risk shared one price series but want different adjustments.** Now store BOTH: `close` (split-adjusted price → SMA/RSI/52-wk levels, value, liquidity) and `adj_close` (total-return → returns/vol/VaR/beta) via yfinance `auto_adjust=False`. SMA200 correctly shifted to the price-chart level (1430 → 1436 — the dividend drift we'd been mixing in).
  - **Back-tested (2nd pass):** out-of-sample rolling-250d VaR exceedances 95% = 6.6% / 99% = 1.7% (vs 5% / 1% expected) — within ~1σ, mild conservative lean as expected for historical VaR on fat tails (→ why we also report CVaR). EWMA covariance is positive-definite, condition number 13.6 (well-conditioned at 5 names). No new bugs.
  - **Documented as intentional (not bugs):** vol is *conditional* (EWMA) while historical VaR is *unconditional* (full-2y empirical tail) — complementary; Filtered Historical Simulation is the consistent Phase-4 upgrade. Covariance is unshrunk (Ledoit-Wolf overkill at 5 names). **27 tests** (added a beta-shrinkage unit test), ruff clean. Lesson: passing tests ≠ a desk would sign off — cross-validate the numbers and interrogate which estimator each parameter deserves. → [[Validate the data, not just that it runs]]

- **Shipped: Phase 2 — multi-agent core (orchestrator + Technical agent).** A **hand-built orchestrator** (no framework yet — build-your-own-graph first) runs the portfolio Risk agent + a new per-stock **Technical agent** (SMA 20/50/200, RSI(14) Wilder, MACD, 52-wk position, 1m/3m returns, volume trend → descriptive dials: trend / momentum / level), merges each stock's technical read with its **risk slice** into a per-stock **card ranked by risk contribution**, and synthesises a reasoning trace per card via Claude **in parallel** (`ThreadPoolExecutor`) — `messages.parse` + Pydantic `StockCard`, descriptive-only. Factual cards work with no API key; synthesis is added when one is set. New command `tradeos-analyze` (`--horizon`, `--as-of`, `--no-llm`). **26 tests green** (added indicator + orchestrator tests), ruff clean.
  - **Pattern locked:** every analyzer agent = *pure-Python facts + optional LLM narration*; the orchestrator composes them. This is the template for the earnings / macro / sentiment agents next.
  - **Read today:** all 5 holdings print downtrend / weak momentum (RSI 35–44), most near 52-wk lows — coherent with the adjusted-price data. INFY is both the **top risk contributor (46.6%)** *and* technically weak — exactly the two-dimensional flag the orchestrator exists to surface.

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
