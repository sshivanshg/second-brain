---
created: 2026-05-24
status: active
type: project
tags: project, quant, finance, research
categories: [quant, ai-ml]
audience: [personal]
repo: /Users/shivanshgupta/Documents/Qm
---

# 📈 Qm — NIFTY Quant Research

> **One-liner:** Research-grade modular strategy framework for **NIFTY / NIFTY50** — backtest, optimize, walkforward, live trade via Kite / SmartAPI.

**Repo:** `/Users/shivanshgupta/Documents/Qm`

## Stack
- pandas
- backtesting.py + vectorbt (both)
- Kite Connect (Zerodha)
- SmartAPI (Angel One)

## Layout
```
configs/strategy_variants.yaml
data/
scripts/
  run_download.py
  run_backtest.py
  run_live.py
src/
  backtest/       (metrics, run_backtest_bt, run_backtest_vbt, run_optimize, walkforward)
  data/           (downloader, loader)
  features/       (indicators)
  live/           (kite_client, smartapi_client, trader)
notebooks/
research/
```

## Sources
- README: `/Users/shivanshgupta/Documents/Qm/README.md`
- Requirements: `/Users/shivanshgupta/Documents/Qm/requirements.txt`

## Connections
- [[Projects MOC]]
