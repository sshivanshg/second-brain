---
created: 2026-05-30
type: note
tags: tradeos, sources
---

# Source-of-truth pointers — TradeOS

> Where the truth lives. **Code = repo. Thinking/decisions = this vault.**

| What | Where |
|---|---|
| Code | `/Users/shivanshgupta/projects/tradeos` |
| Roadmap (phased plan) | `repo/ROADMAP.md` |
| Phase 0 setup / run | `repo/README.md` |
| Risk engine | `repo/src/tradeos/risk.py` |
| LLM narration | `repo/src/tradeos/risk_agent.py` |
| Tests | `repo/tests/test_risk.py` (`uv run pytest`) |
| Portfolio | `repo/holdings.csv` (symbol, quantity, avg_cost) |
| Doc protocol (repo↔vault) | `repo/CLAUDE.md` |

## Database
- **Local Homebrew Postgres**, database **`tradeos`**, role `shivanshgupta` (trust auth, no password), `127.0.0.1:5432`.
- `DATABASE_URL=postgresql://shivanshgupta@127.0.0.1:5432/tradeos` (in `repo/.env`).
- Schema: `repo/db/init/01_init.sql` (plain Postgres `prices` table).
- Reset: `DROP DATABASE tradeos` (isolated; safe to drop).

## Run cheatsheet
```bash
uv run tradeos add RELIANCE.NS 10 2400   # add a holding (fetches its data); editing holdings.csv works too
uv run tradeos holdings                  # list your portfolio
uv run tradeos remove ITC.NS
uv run tradeos ingest                    # refresh all price data
uv run tradeos risk --horizon weekly     # risk read (d/w/m/q/y); + Claude if ANTHROPIC_API_KEY set
uv run tradeos analyze                   # per-stock cards (risk + technical + synthesis)
uv run tradeos risk --as-of 2025-06-30   # point-in-time
uv run pytest                            # the audit suite
```

## Secrets / config
- `ANTHROPIC_API_KEY` (optional) enables the Claude narration; without it, numbers-only.
- `CLAUDE_MODEL` (default `claude-opus-4-8`).

## Related
- [[TradeOS]] · [[Architecture — TradeOS]]
