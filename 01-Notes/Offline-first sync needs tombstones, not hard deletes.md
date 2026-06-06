---
created: 2026-05-31
tags: engineering, offline-first, sync, data-modeling
status: evergreen
---

# Offline-first sync needs tombstones, not hard deletes

> If a device can hold its own copy of a row, "delete" has to be a thing you can *sync*, not a thing that makes the row vanish.

## Context
Delta sync works by asking the server "what changed since timestamp T?" — the server returns rows whose `updatedAt > T`. This is efficient and obvious. It also has a silent failure mode: **a hard-deleted row has no `updatedAt` to return.** It's just gone. So a device that synced the row yesterday and goes offline today will *never* be told the row was deleted. It keeps a ghost copy forever — and if that row feeds a computation (a payroll deduction, a cart total, an unread count), the device shows a number that the server no longer believes.

## The rule
Soft delete: set `deletedAt = now()` and let `updatedAt` bump. Now the delete is a *change* like any other — the next delta pull returns the row with `deletedAt` set, and the client turns that into a local destroy (a tombstone). Two consequences that are easy to forget:
1. **Every read must filter `deletedAt: null`** — listing, aggregates, derived math. A soft-deleted row that still gets summed is worse than no soft delete at all, because now it's invisible *and* still counted.
2. **The sync pull is the one read that must NOT filter** — it has to return tombstones so clients learn about them. So exactly one query in the system reads deleted rows on purpose. Document why, or someone will "fix" it.

The build is never the delete button. It's making every other read agree the row is gone.

## Where this bit us (Arth Saathi)
The [[Salary advance — feature]] delete originally hard-deleted. The mobile app's salary delta-sync (`destroyPermanently()` on a `deletedAt` tombstone) had no way to learn about it — every offline phone would keep deducting a deleted advance from a worker's payslip. Fixing "delete" meant: soft-delete the row, ship the tombstone, and add `deletedAt: null` to *six* reads (list, the per-month cap aggregate, three payroll-math reads, the dashboard metric, global search) — leaving only the sync pull unfiltered. **Bonus still hard-deletes and carries the same latent bug.**

## Related
- [[Salary advance — feature]]
