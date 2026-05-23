---
created: 2026-05-24
status: completed
type: project
tags: project, college, ml, research, quant, capstone
categories: [ai-ml, quant]
audience: [research]
github: https://github.com/sshivanshg/regime-adaptive-transformer
language: Python
visibility: public
---

# 📈 RAMT — Regime-Adaptive Multimodal Transformer

> **3rd-year capstone on Indian equity alpha.** Research traveled from a transformer-centric ranking hypothesis (RAMT) to a foundation-model fine-tune (Chronos-T5 + LoRA), with an HMM regime detector layered as conditional risk control.

**GitHub:** [sshivanshg/regime-adaptive-transformer](https://github.com/sshivanshg/regime-adaptive-transformer)

## Honest headline finding

Tested three interaction modes between ML components (HMM regime + 21-day momentum ranking) and DL component (Chronos-T5 with LoRA adapters):

| Variant | Sharpe (net of 0.22% friction) | CAGR | Max DD |
|---------|-------------------------------|------|--------|
| Momentum + HMM (Phase 2 production) | 0.83 | 13.5% | −18.7% |
| RAMT transformer (Phase 2, **failed**) | 0.49 | n/a | −6.4% |
| **Chronos-LoRA Foundation-Only** (Phase 3) | **1.34** | 23.5% | −16.0% |
| Simple Hybrid (50/50) | 0.91 | 22.8% | −11.1% |
| Triple-Expert (regime-gated) | 0.54 | 8.2% | −13.8% |

**Foundation-Only won on 2024–2026 Sharpe.** But across historical ablation windows:
- **2008:** HMM gating cut drawdown by 9.4pp (−43.2% vs flat −52.7%)
- **2010–2012:** HMM turned flat-sizing Sharpe of −3.0 into +0.79
- **2024–2026 bull:** HMM cost upside (0.66 vs flat 1.35)

## The real architectural claim
**HMM is conditional insurance, not always-on alpha.** Future work = regime-conditional gate that defers to Foundation-Only in calm regimes and shifts to HMM-protected sizing in high-volatility / bear regimes.

**Current production:** Momentum + HMM (Sharpe 0.83) — drawdown profile is best understood across regimes. Chronos-LoRA Foundation-Only is the best out-of-sample Sharpe but validated on only one window.

## Three-phase architecture

| Phase | What | Status |
|-------|------|--------|
| **Phase 1** — Foundational ML baselines | XGBoost + LSTM daily | ✅ |
| **Phase 2** — Custom RAMT transformer | Built from scratch | ❌ Underperformed baselines |
| **Phase 3** — Chronos-T5 + LoRA + HMM hybrid | Foundation model fine-tune | ✅ Best Sharpe |

## Stack
- Python, PyTorch
- Chronos-T5 (foundation model for time series, Amazon)
- LoRA adapters
- HMM (3-state: Bull / High-Vol / Bear)
- Streamlit (dashboard)
- Docker (containerized runs)

## Position sizing (production strategy)
3-state HMM → regime-based allocation:
- Bull regime → 100% sized
- High-Vol regime → 50% sized
- Bear regime → 20% sized

Backtest applies **0.22% friction + stop-losses** across NIFTY 200.

## Repo highlights
- `report/report.pdf` — IEEE LaTeX, 10 pages, canonical write-up
- `PHASE_INDEX.md` — phase-by-phase file map
- `PROJECT_FACTS.md` — fact sheet
- `dashboard/app.py` — Streamlit dashboard
- `models/lora_experiment/chronos_lora_adapter.pt` — trained adapter
- `results/ablation_summary.json` — full ablation results
- `Dockerfile` + `docker-compose.yml` — containerized
- `demo_walkthrough.sh` — one-shot demo

## What the failure of RAMT taught me
**Custom transformer architectures on small financial datasets get smoked by foundation models with even modest fine-tuning.** Don't build sequence models from scratch when Chronos / TimeGPT exist. The compute and data scale favor pre-trained.

## Sources
- README: github.com/sshivanshg/regime-adaptive-transformer
- Report: `report/report.pdf`
- Phase index: `PHASE_INDEX.md`

## Connections
- [[Projects MOC]] · [[College projects MOC]]
- Sister quant project: [[Qm Quant Research]] (NIFTY backtest framework — likely the data pipeline ancestor)
- Sister single-stock prediction: [[StP — Single-stock XGBoost prediction]]
- Methodology cousin: [[SpatiaLaw — WiFi CSI presence detection]] (honest research framing)
