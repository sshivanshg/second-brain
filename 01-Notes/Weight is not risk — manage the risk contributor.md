---
created: 2026-05-30
type: note
tags: quant, risk, atomic
---

# Weight is not risk — manage the risk contributor

A position's **share of capital** (weight) is not its **share of risk**. Portfolio risk lives in the covariance structure, so a name's true contribution depends on its volatility *and* how it correlates with everything else you hold.

- **Marginal contribution to risk (MCTR)** = how much portfolio vol rises if you add a bit more of name *i* = `(Σw)ᵢ / σ_p`.
- **Component contribution** = `weightᵢ × MCTRᵢ`; by Euler's theorem these sum to total portfolio vol → as %, they sum to **100%**.

**Consequence:** to cut book risk most efficiently, trim the biggest **risk contributor**, not the biggest **position** — they're often different names. A high-vol, correlated name can dominate risk far beyond its weight; an uncorrelated name can carry less risk than its weight suggests (and sometimes *reduce* total risk).

**Seen in:** TradeOS — INFY was 31.7% of capital but 46.6% of portfolio risk. → [[Risk engine methodology — TradeOS]]
