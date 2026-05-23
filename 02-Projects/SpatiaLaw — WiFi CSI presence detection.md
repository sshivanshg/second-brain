---
created: 2026-05-24
status: completed
type: project
tags: project, college, ml, research, signal-processing
categories: [ai-ml, systems]
audience: [research]
github: https://github.com/sshivanshg/spatialaw
language: Python
visibility: public
---

# 📡 SpatiaLaw — Spatial Awareness: Human Presence Detection using WiFi CSI

> **One-liner:** *Turn your WiFi signals into an invisible motion sensor.* A privacy-preserving alternative to cameras using Channel State Information (CSI) from standard WiFi.

**GitHub:** [sshivanshg/spatialaw](https://github.com/sshivanshg/spatialaw) · **Headline result:** **99.40% test accuracy** (Random Forest Classifier)

## The idea
Instead of cameras (privacy-invasive, dark-blind), use the way human bodies distort WiFi signal **Phase + Amplitude**. Train a classifier to distinguish *Empty Room* vs *Human Activity*.

## Why this is non-trivial
- Raw CSI from Intel 5300 cards is noisy (electrical interference, multipath)
- Feature engineering matters more than model capacity here: **Variance, Entropy, Doppler** features were the unlock
- Real-time inference path (Streamlit dashboard) vs batch training (Random Forest)

## Key features
- **High accuracy:** 99.40% test accuracy with Random Forest
- **Real-time:** Live Streamlit dashboard for instant visualization
- **Robust:** Feature engineering filters out electrical noise
- **Privacy-first:** No video/audio — only signal physics

## Stack
- Python 3.8+
- Streamlit (real-time dashboard)
- scikit-learn (Random Forest)
- Data: Intel 5300 `.dat` files (WiAR dataset)

## Repo structure
```
app.py                      # Streamlit dashboard entry
model_tools/
  train_random_forest.py    # 99.4% model trainer
models/                     # Saved .joblib + .json artifacts
data/raw/WiAR/              # Intel 5300 .dat files
paper/                      # LaTeX paper sources (.texmf)
_archive/                   # Legacy experiments
```

## What I'd change if redone
- Try transformer over raw CSI sequences (RAMT-style) — RF still wins because dataset is small and features are strong
- Multi-class beyond empty/active (count people, classify activity)
- Generalize across hardware (Intel 5300 → Atheros, commodity routers)

## Sources
- README: github.com/sshivanshg/spatialaw
- Paper: in `paper/` subdir (LaTeX)

## Connections
- [[Projects MOC]] · [[College projects MOC]]
- Methodology cousin to: [[RAMT — Regime-Adaptive Multimodal Transformer]] (also a research capstone with honest framing)
