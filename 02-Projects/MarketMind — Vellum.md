---
created: 2026-08-04
updated: 2026-08-04
status: active
type: project
tags: project, rag, quant, fintech, go, nextjs
categories: [fullstack, ai-ml, quant, backend]
audience: [startup]
repo: /Users/shivanshgupta/Projects/MarketMind
github: https://github.com/sshivanshg/marketmind
package: marketmind
product_name: Vellum
---

# 📈 MarketMind — Vellum

> **One-liner:** Enterprise-grade RAG for stock/market research — cited, time-aware answers over SEC filings, news, transcripts, and your own notes/PDFs.

**Repo folder:** `MarketMind` · **Product name:** **Vellum** · **GitHub:** [sshivanshg/marketmind](https://github.com/sshivanshg/marketmind)

## 🎯 Goal
Let analysts ask natural questions about a ticker and get answers grounded in filings + news + BYO docs, with evidence/citation UX (Evidence Trace rail).

## 🧱 Stack
| Layer | Choice |
|-------|--------|
| API + workers | **Go** + **River** job queue |
| Vectors | **Qdrant** |
| Data | **Postgres** + **Redis** |
| Embeddings | **Voyage** |
| LLM | **Claude** + **Cohere** rerank |
| Web | **Next.js** (`web/`) — landing, `/app` chat, `/admin`, `/kb` |
| Infra local | docker compose |

## ✨ Capabilities
- **Dual ingest:** Auto (SEC 10-K/10-Q, news, transcripts) vs **My documents** (paste, URL, PDF — text extracted, bytes discarded)
- Corpus filter in chat: All / Uploads / Filings
- Evidence Trace rail (citation sync) — design + implementation Jul 2026
- Admin portal + eval (`make eval`, LLM-as-judge optional)
- Auth: session cookies for chat; `X-Admin-Key` for ingest/admin
- Rate limit: 30 query/min/user

## 📝 Log
### 2026-08-04 — vault catch-up
- Repo active through **2026-07-27** (`landing` commit). Local dirty: `admin_handlers.go`, `server.go`.
- Superpowers specs (2026-07-23): evidence-trace-rail, add-ticker, dual-rag-admin, pdf-knowledge-base.
- Session logs under `.cursor/session-logs`.

### 2026-07-23 → 2026-07-27
- Phase 0–1 skeleton + SEC ingestion
- Evidence Trace rail
- Admin portal, dual RAG, PDF KB, corpus filter
- Marketing landing

## ✅ Open tasks
- [ ] Clean local Go admin server dirty files + push if intentional
- [ ] Harden eval gates for production-quality citation answers
- [ ] Productize pricing / multi-tenant if going beyond demo

## 📚 Sources
- `/Users/shivanshgupta/Projects/MarketMind/README.md`
- `/Users/shivanshgupta/Projects/MarketMind/docs/superpowers/`
- Screenshots in repo root (`screenshot-*.png`)

## 🔗 Connections
- [[TradeOS]] — adjacent quant research surface
- [[Projects MOC]] · [[Tech stack map]]
- Belief: citations/grounding like [[LexVault]] line-level grounding
