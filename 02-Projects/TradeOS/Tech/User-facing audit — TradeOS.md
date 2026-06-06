---
created: 2026-05-31
type: audit
tags: tradeos, audit, frontend, ux
---

# User-facing audit — TradeOS

> Audit of the **user-facing layer only** — the SvelteKit SPA (`web/`) and the FastAPI read API
> (`api.py`) that feeds it. Scope: what a *user* can see and do, not the quant core (audited
> separately in the working log). Verdict: the dashboard is a clean, honest **read-only viewer of a
> CLI-driven engine** — and that's the core problem. Almost everything that *changes state* (add a
> holding, upload a quarterly result, refresh data, replay a past date, tune an alert) lives only in
> the terminal. The UI can look but it can't touch.

Severity key: **P0** = blocks a normal user from a core job · **P1** = real gap / bug · **P2** = polish.

> **Update (same day) — acted on:** the colour/theme ask (§0) **and** the write seam + Manage page
> (§1 #1 upload quarterly results, #2 holdings management, #3 doc-coverage view, #4 refresh data) are
> **shipped** — Shivansh chose "thin write seam + build both." 123 tests green. Remaining items below
> (as_of/horizon controls, a11y/responsive, shared types, `VITE_API_BASE`) are still open.

---

## 0. Fixed in this pass (the colour/theme ask)

- **Single hardcoded dark theme → black (default) + light, with a toggle.** `app.css` `:root` was one
  fixed dark palette; charts duplicated the same hex in `charts.ts`. Now: themed CSS variables keyed on
  `<html data-theme>` (`black` = true-black/OLED default, `light` = white with darkened accents for
  contrast), persisted to `localStorage`, applied before first paint (no flash, inline script in
  `app.html`), toggled from a sidebar button. `color-scheme` set per theme so native form controls/
  scrollbars follow.
- **Charts are now theme-aware.** `charts.ts` reads the live CSS variables (`getPalette()`) instead of
  baked hex; pages wrap `<Chart>` in `{#key $theme}` so every chart re-renders with the new palette on
  toggle. Removed the second source-of-truth for colour.
- **Two small colour bugs fixed:** `.dial-neutral` (referenced in `format.ts`, never defined in CSS) is
  now defined; button/brand text colour moved to a themed `--on-accent` so it keeps contrast when the
  accent darkens in light mode (was a fixed near-black).
- Verified: `svelte-check` clean, production build clean.

**Still open on colour/a11y** (see §3): status is signalled by colour alone in places (red/green dials &
heatmap), no keyboard focus ring, charts have no text alternative.

---

## 1. Missing features — the UI can't do what the engine can (P0/P1)

The CLI (`main.py`) exposes a whole layer of capability the SPA never surfaces. The biggest gaps:

| # | Capability | In CLI | In API | In UI | Sev |
|---|---|---|---|---|---|
| 1 | **Upload a quarterly result / concall transcript** (`docs add SYM file.pdf --period --filing-date`) — feeds the Fundamental agent's guidance **and** the "Ask the call" RAG box | ✅ | ❌ (no add route) | ❌ | **P0** |
| 2 | **Add / remove / edit a holding** (`add`, `remove`) | ✅ | ❌ | ❌ (sidebar list is read-only) | **P0** |
| 3 | **Document coverage view** (which holdings are MISSING/STALE/UNTAGGED on their latest results) | ✅ (`docs status`) | ✅ `/api/docs/status` | ❌ **endpoint built, no page calls it** | **P1** |
| 4 | **Refresh / re-ingest price data** (`ingest`) | ✅ | ❌ | ❌ (no "refresh" button; staleness invisible) | **P1** |
| 5 | **Point-in-time replay** — `?as_of=YYYY-MM-DD` (the engine's flagship feature) | ✅ | ✅ (all read routes) | ❌ **no date picker — UI is always "now"** | **P1** |
| 6 | **Horizon control** — `?horizon=d/w/m/q/y/Nd` | ✅ | ✅ | ❌ hardcoded `annual` (and `21` on eval) | **P1** |
| 7 | **Extract structured concall guidance** (`extract`) | ✅ | ❌ | ❌ | P2 |
| 8 | **Edit alert rules** — UI says *"alerts on **your** rules"* but rules are hardcoded Python in `briefing.py` (thresholds from `RISK_LIMITS`); no UI/API to tune them | ✅ (code only) | ❌ | ❌ | **P1** (copy over-promises) |
| 9 | **Search / view a non-holding symbol** — only portfolio names are analysable | ✅-ish | ❌ | ❌ | P2 |

The headline: **#1 (upload quarterly results) and #2 (manage holdings) are the two you called out, and
both are genuinely absent from the UI.** "Ask the call" is shipped, but it queries documents a user can
only load from a terminal — so for a non-CLI user that box is permanently empty.

---

## 2. User-input gaps — the app is read-only (P0)

The entire surface is `GET`-only except `POST /api/ask` (a RAG question). There is **no write path from
the browser at all** — no form, no upload, no mutation. This is *consistent with* the architecture note
in `CLAUDE.md` ("the API/frontend are READ layers — no business logic"), so it's a deliberate line, not
an oversight — **but it's the root cause of every gap in §1.** A decision is needed:

- **Option A — keep the bright line.** UI stays read-only; data entry stays in the CLI. Then the UI should
  *say so* (e.g. an empty "Ask the call" should link the `tradeos docs add …` command; a zero-holdings
  state should show the `tradeos add …` command) instead of looking broken.
- **Option B — add a thin, local-only write seam.** A small set of `POST` routes (`/api/holdings`,
  `/api/docs` with file upload, `/api/ingest`) that call the *same* functions the CLI uses
  (`config.add_holding`, `docs.add_document`, `ingest.*`). These are I/O orchestration, not quant logic,
  so they arguably don't violate "no business logic in the read layer" — but they do cross "no writes,"
  which is your call. Given it runs on `127.0.0.1` for a single user, the security surface is small.

Even the **read-only** controls the API *already* supports (`as_of`, `horizon`) aren't wired to any
input — those are pure-win additions that need **zero** backend change (§1 #5, #6).

---

## 3. Colour / theme / accessibility (P1/P2)

- ~~Only one theme, no light option~~ → **fixed** (§0).
- ~~Chart colours hardcoded separately from CSS~~ → **fixed** (§0).
- **Status by colour alone (P1, a11y).** Dials and the correlation heatmap encode meaning purely in
  red/green — invisible to red-green colour-blind users (~8% of men). Dials *do* carry a text label
  (good), so they're partly OK; the **heatmap and the attention chip rely on hue**. Recommend a
  shape/icon or sign prefix.
- **Descriptive directive vs. good/bad colouring (subtle, on-brand concern).** `dialClass` maps states to
  green=good / red=bad — but the whole project is *descriptive, never prescriptive*. Colouring
  `oversold`/`overbought` red quietly editorialises a neutral state as "bad." Consider a value-neutral
  "state" palette (e.g. by category, not valence) to stay honest to Directive #1.
- **No keyboard focus ring (P1, a11y).** Cards are `<a>` (good, focusable) but there's no
  `:focus-visible` style, so keyboard users can't see where they are. Cheap fix.
- **Charts have no text alternative (P2, a11y).** Canvas charts are opaque to screen readers; only the
  eval page has a real `<table>`. Overview risk/sector/correlation have no tabular fallback.
- **Muted-text contrast (P2).** `--muted` on dark panels was ~4.0:1 (borderline for small text); nudged
  lighter in the black theme. Light-theme palette chosen for ≥4.5:1.

---

## 4. UX / robustness (P1/P2)

- **Not responsive on mobile (P1).** The sidebar is a fixed 232px `position:sticky; height:100vh` with no
  collapse/hamburger; on a phone it eats the viewport. `grid2/grid3` collapse at 900px but the shell
  doesn't. Needs a mobile nav.
- **Error states are dead-ends (P1).** `.err` prints the message with no **Retry** button — if the API is
  down at mount, the only recovery is a manual page reload. Same for the per-page loaders.
- **No first-run / empty states in the UI (P1).** Zero holdings → the overview just errors; there's no
  "add your first holding" CTA (the CLI has one). A new user sees a red box.
- **`VITE_API_BASE` defaults to an absolute `http://127.0.0.1:8000` (P1, latent bug).** In the
  single-origin production mode (FastAPI serves the built SPA), the page is loaded from whatever host the
  user hits, but the client still calls `127.0.0.1:8000` — so it **breaks the moment the app is opened
  from any machine other than the server itself.** Default should be same-origin (relative `''`), with
  the dev `.env` setting the absolute base for the cross-origin `:5173 → :8000` case.
- **No data-freshness / auto-refresh (P2).** Server read-cache TTL is 300s but the client only fetches on
  mount; no "as of … · refresh" affordance beyond the `as_of` date string.
- **"Ask the call" polish (P2):** answer isn't markdown-rendered, no clear/reset, citations are raw
  `[n]` with no hover to the source text.
- **Static `<title>` (P2).** Every route is "TradeOS" in the tab; per-page titles (e.g. the stock
  symbol) would help.

---

## 5. Correctness / maintainability (P1/P2)

- **API responses are all `any` (P1).** Every `$state<any>` / `s as any` means a backend field rename
  silently breaks the UI with **no compile error** — `svelte-check` can't protect a contract it can't
  see. Recommend a shared `lib/types.ts` (or generate from the FastAPI OpenAPI schema) for the card /
  risk / eval / briefing payloads. This is the highest-leverage maintainability fix.
- **Dial→colour keyword map can drift from the agent vocab (P2).** `dialClass` hand-mirrors the agents'
  dial strings. Today technical (`uptrend/downtrend/…`, `at highs/near lows/mid-range`,
  `overbought/strong/oversold/weak/neutral`) is covered, but any new dial value falls back to neutral
  silently. A new dial vocabulary is an API change (per `CLAUDE.md`) — the map should be updated in
  lockstep, or derived.
- **No request cancellation on fast nav (P2).** Minor; per-route components isolate state, so the
  practical risk is low.

---

## 6. Recommended order of work

1. **Wire the two zero-backend wins:** `as_of` date picker + `horizon` selector (read-only, API already
   supports them). Surfaces the engine's flagship point-in-time story in the UI immediately. *(P1, cheap)*
2. **Document coverage page** consuming the existing `/api/docs/status` (no backend work). *(P1, cheap)*
3. **Decide §2 Option A vs B.** If B: add the thin write seam, then build **(a) upload quarterly
   results/transcripts** and **(b) holdings add/remove** — the two you flagged. *(P0)*
4. **a11y + responsive batch:** focus rings, mobile nav, retry buttons, empty states, colour-blind-safe
   status. *(P1)*
5. **Shared API types** from the OpenAPI schema. *(P1, maintainability)*
6. Polish: refresh affordance, per-page titles, markdown answers, `VITE_API_BASE` default. *(P2)*
