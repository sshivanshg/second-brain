---
created: 2026-05-24
status: completed
type: project
tags: project, college, quant, ml
categories: [quant, ai-ml]
audience: [college]
github: https://github.com/sshivanshg/StP
language: Python
visibility: public
---

# 📉 StP — Single-Stock Next-Day Return Prediction (XGBoost, Quant Mode)

> **One-liner:** Predicts **next-day return** (not raw price) for one stock using XGBoost. Train once → save → daily predict → backtest in a return-based, quant-style pipeline.

**GitHub:** [sshivanshg/StP](https://github.com/sshivanshg/StP)

## Why return prediction (not price)
Price prediction is a non-stationary problem (trends dominate). Return prediction normalizes — yesterday's 1% return is comparable to today's 1% regardless of absolute price. Standard quant practice.

## Stack
- Python
- XGBoost
- Walk-forward validation (rolling train/test by year)

## Architecture — clean separation

```
train_model.py      # Train + save model + test predictions + metadata
predict_daily.py    # Load model, predict next close, recommended action
evaluate.py         # Evaluate saved model on test set (no retraining)
evaluation.py       # Shared metrics + plotting
walk_forward.py     # Rolling train/test by year
strategy.py         # Predicted return → LONG / SHORT / HOLD
risk.py             # Position size, stop-loss, take-profit
backtest.py         # Equity curve, Sharpe, drawdown
features.py         # Feature engineering (reused everywhere)
config.py           # Symbol, paths, params
models/             # reliance_model.pkl + metadata.json
```

## What's good about this layout
- **Train once, predict many times** — separation of expensive (training) from cheap (inference)
- **Walk-forward validation** is the right discipline for time series (not random k-fold)
- **Risk module** is separate from prediction — predictions don't bake in position sizing
- Reuses `features.py` and `evaluation.py` to avoid train/inference skew

## Sources
- README: github.com/sshivanshg/StP

## Connections
- [[Projects MOC]] · [[College projects MOC]]
- Sister: [[RAMT — Regime-Adaptive Multimodal Transformer]] (multi-stock + foundation model)
- Sister: [[algo_dev — Indian stock market trading algorithm]] (algo trading)
- Sister: [[Qm Quant Research]] (broader quant framework)
