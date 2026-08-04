---
created: 2026-05-25
type: architecture
status: implemented
tags: arth-saathi, tech, ai, agent, attendance, ux
repo_doc: /Users/shivanshgupta/projects/arth/docs/AI/AGENT_ATTENDANCE_RELIABILITY.md
---

# Agent attendance reliability — fix

> Thinking + map. **Repo is truth:** `docs/AI/AGENT_ATTENDANCE_RELIABILITY.md` is the canonical spec; this note is the orientation layer. Sibling to [[Agent confirmation flow — fix]].

## What broke
Owner typed **"mark everyone absent"** and the chat replied — four times in one bubble — *"Invalid arguments for mark_attendance_bulk."* Raw, English, technical, and repeated. On the single most common daily action, again. (Same week as the [[Agent confirmation flow — fix]]; attendance is where first impressions are won or lost.)

## The non-obvious cause
There were whole-team tools for **present** and **holiday** (each calls `markDay`, which marks every active employee with no list), but **none for absent**. "Everyone absent" had nowhere to land except `mark_attendance_bulk`, whose schema demands an explicit, resolvable `employees[]`. The model can't conjure that list from the word "everyone", so it sent junk → Zod `safeParse` rejected it → hard error.

Then the loop amplified it: after a failed tool batch the graph routes back to the decision node up to 4×; the model re-emitted the same broken call each time; the fallback response **joins all tool-result summaries**, so four identical failures became one four-times-repeated string.

The general lesson — worth carrying to every future tool: **a tool schema that forces the model to fabricate a value it cannot produce turns a missing capability into a raw Zod error, and the agent loop multiplies it.** A capability gap should fail *gracefully inside the tool*, or better, *not be a gap*.

## The fix — four layers, defence in depth
1. **`mark_all_absent`** — the missing sibling. Marks every active employee absent via `markDay`. The reliable home for the command.
2. **`mark_attendance_bulk` expands whole-team tokens** — "everyone"/"all"/"sabhi"/"sabko"… (`isWholeTeamToken`) delegate to `markDay` instead of failing resolution. Belt-and-suspenders.
3. **Prompt guidance** — whole-team marking → `mark_all_*`; `mark_attendance_bulk` is only for an explicit named subset, never the word "everyone".
4. **Loop guard** — the graph now tracks per-turn tool-call signatures; identical retries aren't re-run, and an all-repeats batch settles with the model's own message. No more duplicate-error bubbles. The *first* genuine failure still shows once so the model can rephrase it in the owner's language.

## Robustness for *every* phrasing, *every* tool
Owners type "aaj sabko absent mark kro", "mark all of them absent today", typos, regional languages — endless variants. Enumerating phrases is whack-a-mole. So robustness is split by responsibility:
- **Intent mapping → the LLM** (a language model is the right tool for "any phrasing"). The prompt now says "everyone" lives in many languages and to *extract intent, not match words*; date words like "aaj" → today. The real lever is **capability completeness** — the tool must exist.
- **No raw error ever reaches the owner** — every tool's failure now carries a *friendly* owner-facing summary while the precise reason stays in `error` (which is what the model reads to self-correct). One systemic change, all tools covered. The owner never sees "Invalid arguments" or an internal tool name again.
- **The model is told to recover** — on ERROR: fix the next call or explain in the owner's language; never repeat the identical call.
- **The token net is a generous fallback** — sab/sabhi/saare/"poori team"/"all of them" + trailing-filler stripping ("sab ko" → "sab").

**Typos too.** The shared `resolveEmployeeFromContext` — behind *every* people-facing tool — now falls back to Optimal String Alignment distance (transposition = 1 edit) when exact/prefix/substring miss: "Rmesh"→Ramesh, "Suresg"→Suresh, "Pirya"→Priya. A clear closest match wins; equally-close names come back as *choices* (owner picks — and the mutation still goes through the Confirm card, so a fuzzy guess is never silently written); unrelated text stays "not found". Claude already auto-corrects names from the roster it sees in-context — this is the defence-in-depth layer for when a typo slips through.

Mental model: **the LLM handles language; the tool layer handles safety; neither leaks plumbing to the owner.**

## Audit takeaway
Swept the employee + payroll + attendance tools: `mark_attendance_bulk` was the **only** fabricate-a-list trap. Everything else resolves people via `resolveEmployeeFromContext` and fails gracefully with suggestions. The bug was a capability hole, not a pattern rot.

## Map
- Tools: `apps/backend/src/agent/tools/attendance.tools.ts` (`markAllAbsentTool`, bulk expansion), `utils.ts` (`isWholeTeamToken`)
- Loop: `graph/nodes.ts` (`executeToolsNode` dedupe, `routeAfterTools`), `graph/state.ts` (`attemptedToolCalls`), `agent.service.ts` (per-turn reset)
- Prompt: `prompts/system.ts`
- Tests: `tools/attendance.tools.test.ts`, `graph/nodes.test.ts` (loop guard) — 151 agent tests green

## Related
- [[Agent confirmation flow — fix]]
- [[Agent capability vision — do any manual task, dead-simple]]
- [[Architecture — Arth Saathi]]
