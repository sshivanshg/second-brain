---
created: 2026-08-04
updated: 2026-08-04
status: completed
type: project
tags: project, hackathon, knowledge-graph, ai
categories: [fullstack, ai-ml]
audience: [college, personal]
repo: /Users/shivanshgupta/Projects/ET-AI-Hackathon-2.0
github: https://github.com/sshivanshg/ET-AI-Hackathon-2.0
package: engram
---

# 🏭 Engram — Industrial Knowledge Intelligence

> **One-liner:** Institutional memory for Indian heavy industry — ingest plant docs → knowledge graph (Assets, People, Incidents, Procedures, Parts) → cited Q&A + **Knowledge Risk Radar** for single-point-of-failure experts.

**Repo:** `ET-AI-Hackathon-2.0` · **Hackathon:** ET AI Hackathon 2.0 PS #8 · **GitHub:** [sshivanshg/ET-AI-Hackathon-2.0](https://github.com/sshivanshg/ET-AI-Hackathon-2.0)

**Product line:** *Don't let your best engineers take institutional knowledge with them.*

## 🧱 Stack
| Layer | Tech |
|-------|------|
| Frontend | Next.js 15 — Copilot chat + React Flow graph + Risk Radar |
| Backend | Express + TS — 5-agent tool loop (Ingestor, Linker, Jargon, Traversal, Risk) |
| Graph | graphology on plant docs in **MongoDB** |
| Docs | pdf-parse, mammoth, xlsx |
| LLM | Claude Haiku tool-use over SSE |

## Demo path
1. Create Plant → Load Unit 3 demo (Bharat Engineering Works, Pune)  
2. Graph / Risk Radar  
3. Ask: *"If Ramesh retires tomorrow, which machines lose their only expert?"*

## 📝 Log
### 2026-07-20
- Google Drive picker + URL ingest for plant/assistant docs (`feature/google-drive-ingest`)
- Engram intelligence suite + simplified plant graph UX
- UX shell reused from prior compliance product; VC sanctions surface removed

### 2026-08-04 — vault catch-up
- Branch `feature/google-drive-ingest` tracking origin; minor sample-data dirty files
- Related design dump folder: `~/Projects/Something` (Engram PRD/theme notes)

## Deferred (explicit)
Hono rewrite, Neo4j, pgvector, Docling OCR, full RBAC

## 🔗 Connections
- [[Projects MOC]] · [[College projects MOC]]
- Graph-of-knowledge pattern vs vector-only RAG ([[MarketMind — Vellum]], [[LexVault]])
