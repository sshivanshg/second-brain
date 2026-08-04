---
created: 2026-05-31
type: product
status: built — backend + web + mobile + employee self-view + AI agent
tags: arth-saathi, features, payroll, fintech
repo_doc: /Users/shivanshgupta/projects/arth/docs/features/SALARY_ADVANCE.md
---

# Salary advance — feature

> **One-liner:** the owner hands a worker ₹2,000 mid-month, taps it into the app, and payroll quietly subtracts it from that month's pay. The everyday "udhaar to the staff" that every kirana and workshop already does on paper — now it just *works*, on every screen.

**Status:** built end-to-end and live in every format — web (owner + employee), the React Native mobile app (owner + employee), the AI agent ("Advance payment to Ravi"), with cap guards, soft delete, and offline-aware UX. Repo truth: `docs/features/SALARY_ADVANCE.md`.

## Why this matters
- Advances are the **most common payroll event in the SMB segment** — far more frequent than bonuses. A worker asking for money before payday is a Tuesday, not an exception. If the tool can't do the everyday thing cleanly, the notebook wins.
- It's the deduction half of payroll truth: salary minus attendance minus *advances* plus bonus = what actually leaves the cash drawer. Without advances, the "net payable" we show is a lie the owner has to mentally correct — and a tool you can't trust on money, you stop trusting.
- It's a quiet on-ramp to the fintech layer. Once the system *knows* the advance pattern of each worker, that's the raw signal for [[Vertical SMB-OS earns most revenue from embedded fintech|earned-wage access and micro-credit]] later. An advance is EWA the owner is funding out of pocket; we're just the ledger — for now.

## What it is (and isn't)
- **Is:** a per-employee **ledger** of advances — many per month, each with an amount, a paid date, and an optional note. Attributed to a month by its date; fully deducted from that month's net.
- **Is NOT:** a bonus (that's one editable entry per month) and **not** an instalment loan. There's no "recover over 3 months" yet — an advance hits the month it was paid, in full. That's the honest MVP; instalments are a deliberate later call.

## The shape of the thing (decisions worth remembering)
- **Two caps, server-enforced, never client-trusted.** A *hard* cap at 3× monthly salary that can't be overridden (a typo'd ₹50,000 advance on a ₹12,000 salary should be impossible, full stop). A *soft* cap when the month's advances cross the salary line — that one the owner *can* clear, but only by consciously confirming ("Advance is more than salary → Pay anyway"). The soft cap is a guardrail, not a wall; the hard cap is a wall. That distinction is the whole UX.
- **Soft delete, not hard delete — and this is load-bearing.** Deleting an advance sets a `deletedAt` tombstone instead of erasing the row. Why it matters: the mobile app is offline-first and only learns a row vanished if the sync *ships it back* marked deleted. A hard delete would leave a ghost advance on the worker's phone forever, still subtracting from a payslip that no longer reflects reality. So "delete" had to mean "tombstone + sync the tombstone + filter it out of every read." The build wasn't the delete button; it was making *every* payroll read agree the row is gone. (See [[Offline-first sync needs tombstones, not hard deletes]].)
- **The form stays open while you confirm.** A small thing: when the soft cap fires, the entry form doesn't collapse — your ₹ and note are still there behind the dialog, so "Pay anyway" doesn't make you re-type. Money entry that loses your input on a warning is money entry people abandon.

## What was actually missing (this pass)
The feature was ~80% there — the math already subtracted advances, add/edit existed on both web and mobile. The gaps that made it *not* trustworthy, now closed: **no way to delete** a mistaken advance anywhere (service supported it; no route, no button); the soft cap returned a dead-end error with **no way to proceed**; and a latent bug where the web edit used `PATCH` against a `PUT`-only route (editing an advance silently 405'd). Completing it meant finishing the CRUD honestly across all four formats, not adding a new screen.

## Roadmap fit
- **Now:** full advance CRUD everywhere, capped and tombstoned.
- **Next:** the same soft-delete discipline should land on **bonus** (it still hard-deletes — same latent mobile-ghost bug). Then the fintech step: an advance the *platform* funds = earned-wage access. See [[Feature catalog]].

## Related
- [[Feature catalog]]
- [[Architecture — Arth Saathi]]
- [[UX principles — Arth Saathi]]
- [[Offline-first sync needs tombstones, not hard deletes]]
- [[Vertical SMB-OS earns most revenue from embedded fintech]]
- [[Arth Saathi]]
