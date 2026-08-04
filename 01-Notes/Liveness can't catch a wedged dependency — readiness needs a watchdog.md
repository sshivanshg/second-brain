---
created: 2026-06-13
tags: engineering, reliability, health-checks, observability
status: evergreen
---

# Liveness can't catch a wedged dependency — readiness needs a watchdog

> A process that is *alive* but can't reach its database is not "unhealthy" to a liveness probe — and a readiness probe alone will pull it from rotation but never restart it. With one replica, that's a permanent, silent outage.

## Context
The standard probe split is sound: **liveness** should be cheap and dependency-free (`/health` → "is the process running?") so a momentary DB blip doesn't trigger a restart storm; **readiness** checks dependencies (`/ready` → `SELECT 1`) so a replica with a dead pool stops receiving traffic. The trap is the *gap between them*. A wedged connection pool — DB perfectly healthy, but this worker's connections stuck — fails readiness forever while passing liveness forever. The orchestrator's logic is then: "alive, so don't restart; not ready, so don't route." The replica sits in limbo. With `min_replicas = 1`, that one limbo replica means ingress has zero ready backends and **blackholes every request** — so even the dependency-free `/health` is unreachable *externally*, because there's nothing healthy to route it to.

## The rule
A condition that fails readiness but not liveness needs a **third actor that can force a restart**. Options, cheapest first:
1. **In-app watchdog** — a timer that probes the dependency and, after a *sustained* failure window (not a single blip), `process.exit(1)` so the orchestrator restarts with a fresh pool. Grace window keeps transient failures from looping; a genuine wedge self-heals. This is the same "exit non-zero, let the orchestrator restart" reflex you already use for `unhandledRejection`/`uncaughtException` — extended to "can't reach my own dependency."
2. **Liveness that escalates** — only if the platform supports a slow liveness that trips after the dependency has been down past a grace window. Risky: too tight and a real DB outage becomes a restart storm.
3. **More than one replica** — reduces blast radius but doesn't fix the wedge; both replicas can wedge.

And independently: **detection is half of uptime.** A self-healing service with no external watcher still fails silently if the heal doesn't work. An external check on the public health endpoint turns "down for days, found by accident" into "down for minutes, emailed." (Sibling belief: [[A green deploy is not a live deploy — verify the running revision]] — both are "the thing reports healthy but isn't serving.")

## Where this bit us (Arth Saathi)
The prod backend (Azure Container Apps, single always-on replica) went **down for ~2 days, unnoticed**. Liveness probed `/health` (green); readiness probed `/ready`'s `SELECT 1` (failing); the Prisma pool had wedged while the external AWS RDS was completely healthy the whole time (proven with `psql SELECT 1` from a laptop). Nothing restarted it, nothing watched it. Restart fixed it in <1 min. The permanent fix was exactly this belief: a DB watchdog that exits after a 5-min sustained-unreachable window (`apps/backend/src/server/db-watchdog.ts`) **plus** a 5-minute external uptime check (`.github/workflows/uptime.yml`). Full write-up: [[Backend deploy & revision health — Arth Saathi]].

## Related
- [[Backend deploy & revision health — Arth Saathi]]
- [[A green deploy is not a live deploy — verify the running revision]]
- [[Arth Saathi]]
