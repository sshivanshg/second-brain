---
created: 2026-08-04
updated: 2026-08-04
status: active
type: project
tags: project, telegram, ops, growth, deskzy, python
categories: [backend, systems]
audience: [startup, personal]
repo: /Users/shivanshgupta/Projects/tg-promo
package: tg-promo
---

# 📣 tg-promo

> **One-liner:** Telegram ops + monetized promo pipeline for Deskzy / Linkvertise — multi-link packs, ban-check, hop chains, and group maintenance via Telethon.

**Repo:** `/Users/shivanshgupta/Projects/tg-promo` · **No remote git yet** (local project) · **Depends on:** [[Deskzy]] multi-link API

## 🎯 Goal
Post resilient promo links into Telegram chats on a schedule: when single invites die, multi-link packs still convert. Monetize via Linkvertise hop chain → Deskzy list pages.

## 🧱 Stack
- **Python 3** + venv
- **Telethon** for user sessions (approve, join, schedule, ban-check)
- **Bot token** for live bot + evening scheduler
- **Deskzy API** (`DESKZY_API_KEY`) for multi-link list pages
- **Linkvertise** Full Script API for hop monetization
- State in JSON under `state/` (gitignored); sessions under `sessions/`

## 📁 Layout
```
tg-promo/
├── bot/           # live bot + evening scheduler
├── group/         # approve, clean joins, discover, join, pin
├── promo/         # packs + hop-chain + schedule_week
├── scrape/        # nested link crawler
├── data/links/    # the_list.md source queue
├── data/exports/  # find_groups CSVs
├── sessions/      # Telethon *.session (gitignored)
├── state/         # runtime JSON (gitignored)
├── paths.py
└── tests/         # pack tests
```

## 🏗 Promo architecture
```
the_list.md
    → ban-check active pool (+ dead_urls)
    → rolling pack (~5 invites)
    → Deskzy multi-link POST /api/links {"urls":[...]}
    → Linkvertise → Deskzy → Linkvertise → Deskzy  (hop chain)
    → Telethon schedule into PROMO_CHAT_IDS
```

**Why packs:** single `t.me` invites die from reports/bans; users unlock once and get ~5 channel options.

**Schedule default:** 3 days × 4 slots (03:00 · 09:00 · 14:00 · 20:00 IST).  
**Pack size:** `PROMO_PACK_SIZE` (default 5, min 2).

## ⚙️ Common commands
| Task | Command |
|------|---------|
| Promo dry-run | `python -m promo.schedule_week --dry-run --days 1` |
| Schedule 3 days | `python -m promo.schedule_week` |
| Replace scheduled | `python -m promo.replace_scheduled` |
| Live bot | `python bot/bot.py` |
| Approve joins | `python group/accept_requests.py --yes` |
| Delete join msgs | `python group/delete_joins.py` |
| Find groups | `python group/find_groups.py` |
| Nested scrape | `python scrape/nested_links.py` |

## ⚠️ Safety / ops notes
- Only **one** process per `.session` at a time
- FloodWaits handled with sleep/retry
- Mass-DM members = ban risk — prefer group posts
- Never enable Linkvertise “wait 60 mins” on Full Script API (account-wide)
- Secrets only in `.env` + sessions (never commit)

## 📝 Log
### 2026-08-04 — vault catch-up
- Project last touched Aug 3–4; multi-link pack promo plan under `docs/superpowers/plans/2026-08-03-multi-link-pack-promo.md`.
- Not in vault before; tightly coupled to live [[Deskzy]] multi-link + hop product.

### 2026-08-03
- Multi-link pack architecture live in `promo/` (ban_check, pack, hop_chain, schedule_week, replace_scheduled).
- Tests under `tests/test_pack.py`.

## ✅ Open tasks
- [ ] Optional: init git remote (private) for backup without secrets
- [ ] Keep `the_list.md` topped up; prune `dead_urls` awareness
- [ ] Monitor digest DM to `@NOTIFY_USERNAME` after schedule runs
- [ ] Confirm Linkvertise 60-min wait stays off after any account changes

## 📚 Sources
- `/Users/shivanshgupta/Projects/tg-promo/README.md`
- `/Users/shivanshgupta/Projects/tg-promo/docs/superpowers/plans/2026-08-03-multi-link-pack-promo.md`
- Deskzy API docs in Deskzy README multi-link section

## 🔗 Connections
- [[Deskzy]] — multi-link API + hop landing pages
- [[Projects MOC]] · [[Now]]
- Growth / ops tooling, not end-user product
