---
created: 2026-05-30
type: note
tags: quant, risk, atomic
---

# Estimate once, express at any horizon

Annualising volatility (×√252) is a **unit choice, not a calculation**. Risk is horizon-agnostic; the year is just a convenient yardstick. So estimate once from daily data and scale to any horizon with the **√time rule**:

$$\sigma_T = \sigma_{\text{daily}} \times \sqrt{T}$$

- **Scales with √time:** volatility, VaR, CVaR (they carry time units).
- **Horizon-invariant (ratios — never scale):** correlation, beta, risk-contribution %, concentration.

So one daily estimate gives weekly/monthly/yearly views for free, and the "where is my risk / how market-sensitive am I" answers are the *same* at every horizon.

**Caveat:** √time assumes i.i.d. returns (no autocorrelation, stable vol). Accurate short-term; weakens over long horizons where mean-reversion, regime shifts, and fat-tail compounding bite. For long horizons, resample directly if you have the data.

**Seen in:** TradeOS `--horizon` knob — book vol reads 1.09% daily / 2.44% weekly / 17.34% annual; beta and correlation don't move. → [[Risk engine methodology — TradeOS]]
