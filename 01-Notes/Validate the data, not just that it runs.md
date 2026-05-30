---
created: 2026-05-30
type: note
tags: data, quant, atomic
---

# Validate the data, not just that it runs

Code that runs cleanly can still be fed wrong data and produce confident, wrong numbers. Validate the **inputs and outputs** against reality, not just the absence of exceptions.

**Seen in:** TradeOS ingested **raw** (unadjusted) prices, so stock splits/bonuses showed up as fake −45/−50% single-day crashes — which silently poisoned every volatility, drawdown, beta, and VaR number. Everything "ran fine." The bug only surfaced because the numbers *looked wrong for blue-chips* and an audit checked max daily moves. Fix: use **total-return adjusted** prices; worst-21-day went from a bogus −50% to a realistic −11.7%.

**Rule of thumb:** sanity-check magnitudes against domain priors (a large-cap doesn't drop 50% in a day), and write tests/audits that assert on *values*, not just on "no error." Sibling of "trust the code, not an older summary doc."
