---
created: 2026-05-25
type: architecture
status: implemented
tags: arth-saathi, tech, ai, agent, ux
repo_doc: /Users/shivanshgupta/Documents/arth/docs/AI/AGENT_CONFIRMATION_FLOW.md
---

# Agent confirmation flow — fix

> Thinking + map. **Repo is truth:** `docs/AI/AGENT_CONFIRMATION_FLOW.md` is the canonical spec; this note is the orientation layer. Reinforces [[Confirm before write — AI mutations need human approval]].

## What broke
An owner asked the agent to mark attendance and got stuck in a loop: "show today's attendance" → "mark all present" → "yes mark them" → "yes", and *every* reply was the same plain sentence — **"Would you like to mark attendance for them now?"** — while nothing was ever marked. It reads as the agent ignoring the owner. Bad first impression on the single most common daily action.

## The non-obvious cause
The Confirm/Reject card UI was **never the problem** — it was built, correct, and waiting. The agent just never triggered it. The LLM was replying with `finishReason="clarify"` and a *rhetorical* confirmation question instead of emitting the `mark_all_present` tool call. No tool call → `shouldConfirm` never runs → no LangGraph `interrupt` → backend never returns `confirmationRequired` → client never shows the card. A typed "yes" then has no pending confirmation to attach to, so it's sent as a fresh message, and the model produces the identical question again. Infinite loop.

So the agent had quietly grown a **second, prose-based confirmation layer** that duplicated and then short-circuited the real structural one.

## The fix — push the discipline into the prompt, not a code hack
One-line mental model: **the agent proposes by acting; the owner approves by tapping.** The model must commit to the tool call; confirmation is the card, never a sentence.

Changes in `prompts/system.ts`:
- Forbid rhetorical confirmation questions ("Would you like…?", "Shall I…?").
- Clear command → emit the tool call immediately, even on first message.
- Affirmative reply (incl. Hinglish: "haan", "kar do") to an offered action → emit the call now, never re-ask.
- Default attendance date to *today*; `clarify` only for a truly missing value, never for permission.

## The tempting wrong fix I rejected
Broadening the client's typed-"yes" regex to auto-confirm phrases like "yes mark them". That **false-confirms money-moving / destructive actions** from loose text — exactly what [[Confirm before write — AI mutations need human approval]] exists to prevent. The explicit tap-to-Confirm card stays the safe primary path; typed affirmatives stay narrow.

## Map
- Decision: `apps/backend/src/agent/graph/nodes.ts` (`agentDecisionNode`, `reviewToolCallsNode`)
- Gate: tool `shouldConfirm` → `interrupt` → `agent.service.ts` returns `messageType:"CONFIRMATION"`
- UI: `ai-assistant-card.tsx` "Action Required" card + `ChatConfirmCards.tsx`
- Tests: `prompts/system.test.ts` → "confirmation & action behaviour"

## Related
- [[Confirm before write — AI mutations need human approval]]
- [[Architecture — Arth Saathi]]
- [[Voice and Hinglish are the UX not a translation]]
