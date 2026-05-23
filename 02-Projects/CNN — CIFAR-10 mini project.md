---
created: 2026-05-24
status: completed
type: project
tags: project, college, ml, deep-learning
categories: [ai-ml]
audience: [college, coursework]
github: https://github.com/sshivanshg/CNN
language: Python
visibility: public
---

# 🧠 CNN — CIFAR-10 Mini Project

> **One-liner:** Classic CIFAR-10 image classification with a hand-rolled CNN (`SimpleNet`). College ML coursework.

**GitHub:** [sshivanshg/CNN](https://github.com/sshivanshg/CNN) · Sister repo: `cnnbaseline`

## `SimpleNet` architecture
1. **Conv Layer 1:** 3 RGB channels → 6 feature maps
2. **Max Pooling:** reduces image size, speeds calc
3. **Conv Layer 2:** 6 → 16 maps (depth increases for more complex features)
4. **Fully Connected:** flatten → 10 outputs (one per class)

## Stack
- Python
- PyTorch (assumed from typical CIFAR-10 work)
- `requirements.txt`

## Connections
- [[Projects MOC]] · [[College projects MOC]]
- Foundation for: [[RAMT — Regime-Adaptive Multimodal Transformer]] (used DL fundamentals from this)
