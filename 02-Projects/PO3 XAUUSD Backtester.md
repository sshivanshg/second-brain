---
created: 2026-08-04
updated: 2026-08-04
status: studying
type: project
tags: project, quant, trading, python, xauusd
categories: [quant, backend]
audience: [personal]
repo: /Users/shivanshgupta/Projects/po3-xauusd-backtester
---

# 🥇 PO3 XAUUSD Backtester

> **One-liner:** Monorepo for algorithmic PO3/FVG backtesting + discretionary replay simulator on XAUUSD M15 — research first, no live alerts yet.

**Repo:** `/Users/shivanshgupta/Projects/po3-xauusd-backtester` · **No git remote** · Progress: `docs/PROGRESS_LOG.md`

## 🧱 Layout
```
apps/frontend     # Vite results viewer
apps/simulator    # Next.js discretionary trading simulator
packages/backtester  # Python package
scripts/ data/ artifacts/ docs/
```

## 📊 Research snapshot (2026-07-21 overnight)
From PROGRESS_LOG on real MT5 export (~99k M15 bars, 2022–2026, tz inferred `Etc/GMT-2`):

| Metric | Baseline |
|--------|----------|
| Trades | 524 |
| Win rate | 21.8% |
| Expectancy | **−0.148 R/trade** |
| Max DD | −92 R |
| Sensitivity | 13/80 cells positive; best islands **overfit-flagged** |

**NEEDS INPUT (still open):**
1. Confirm broker server timezone (GMT-2 vs GMT-3)
2. Confirm live spread/commission vs default 0.30
3. Strategy has no robust edge — revise / filter / abandon before live

## 📝 Log
### 2026-07-21
- Built modules: data_loader, fvg, structure, strategy, backtester, tests
- Causal sim, SL-before-TP, sensitivity heatmap
- Overnight full grid; equity + trades + heatmap artifacts under `output/`
- Later monorepo: simulator + Vite viewer (related history also appeared under Buildy git — separate products)

### 2026-08-04 — vault catch-up
- Documented as studying/research; not production trading system

## ✅ Open tasks
- [ ] Confirm broker tz + spread with human
- [ ] Walk-forward / nested OOS before any claim of edge
- [ ] Optional: discretionary simulator polish

## 🔗 Connections
- [[TradeOS]] · [[Qm Quant Research]] · [[algo_dev — Indian stock market trading algorithm]]
- [[Projects MOC]]
