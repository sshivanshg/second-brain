---
created: 2026-05-30
type: note
tags: tradeos, tech, quant, risk
---

# Risk engine methodology — TradeOS

> What risk we actually compute, and how. The mental model: **risk = the distribution of the portfolio's future P&L.** Every metric answers one of three questions — *how wide?* (vol), *how bad is the left tail?* (VaR/CVaR/stress), *where does it come from?* (risk contribution / beta / concentration).

## The chain
1. **Prices → returns.** Total-return adjusted closes. **Log returns** for vol/covariance (additive); **simple returns** for VaR/stress (real P&L).
2. **Volatility (per name)** = stdev of daily returns, scaled by √time. Estimated with **EWMA (λ=0.94)** so recent regime is weighted more — vol clusters.
3. **Covariance / correlation** = how names move *together*. The covariance matrix Σ (via `_ewma_cov`) is the central object. Diversification (corr < 1) is the only free lunch.
4. **Portfolio vol (ex-ante):** `σ_p = √(wᵀ Σ w)`. Credits diversification. (Book vol 17.3% vs ~25.4% if perfectly correlated — ~8 pts erased by a 0.31 avg correlation.)
5. **Concentration:** HHI = Σ wᵢ²; **effective holdings** = 1/HHI (4.58 of 5).
6. **Risk contribution (the headline):** MCTRᵢ = (Σw)ᵢ/σ_p; componentᵢ = wᵢ·MCTRᵢ; these sum to σ_p (Euler), so **%CTR sums to 100%.** → [[Weight is not risk — manage the risk contributor]].
7. **Beta:** βᵢ = cov(i, NIFTY)/var(NIFTY). Splits market risk from idiosyncratic. Portfolio β 0.75 = defensive.
8. **VaR** = loss threshold (historical 5th/1st percentile of portfolio returns). **CVaR / Expected Shortfall** = average loss *beyond* VaR (always ≥ VaR; coherent, tail-aware).
9. **Stress** = worst realised rolling 1/5/10/21-day windows for the current weights (empirical, no distribution assumption).
10. **Liquidity:** days-to-liquidate = position value / (20% × recent daily traded value).
11. **Limits:** compare to a risk budget (`RISK_LIMITS`) → breaches. Checked at **natural units** (annual vol, 1-day VaR), horizon-independent.

## Horizon
Estimate daily, express at any horizon via **σ_T = σ_daily·√T**. Vol/VaR/CVaR scale; **correlation, beta, %CTR are horizon-invariant**. `--horizon d/w/m/q/y|Nd`. → [[Estimate once, express at any horizon]].

## Honest limitations (know when NOT to trust it)
- **Stationarity** — estimated from history; stale in regime shifts (EWMA mitigates, doesn't cure).
- **Window misses disasters** — 2y of data has no 2008/2020, so historical VaR/CVaR *understate* true tails. (5y + EVT later.)
- **Correlations →1 in crises** — the 0.31 avg is a calm-market number; diversification evaporates exactly when needed.
- **Constant current weights** — replays today's weights over history, ignoring rebalancing.

## Related
- [[Architecture — TradeOS]] · [[TradeOS]] · [[Working log — TradeOS]]
