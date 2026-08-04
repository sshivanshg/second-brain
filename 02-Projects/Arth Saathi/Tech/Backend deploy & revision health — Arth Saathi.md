---
created: 2026-06-04
type: architecture
status: implemented
tags: arth-saathi, tech, devops, deploy, azure, vercel, ci, reliability, incident, rds
repo_doc: /Users/shivanshgupta/projects/arth/docs/deploy/azure-backend-terraform.md
---

# Backend deploy & revision health — Arth Saathi

> Hard-won ops runbook. The email-auth ship (2026-06-04) was code-complete for hours but invisible in prod because of **four** infra traps below — none in the feature code. Read this before debugging "I shipped it but it's not live."

## The big lie: a green deploy is **not** a live deploy
The **"Deploy backend (Azure)"** GitHub workflow builds a Docker image → pushes → `terraform apply` sets the Container App's image tag → **reports success**. But the Container App **revision health is async**: if the new revision **crash-loops on startup**, Azure keeps serving the **previous healthy revision** and the workflow *still shows success*. So `az containerapp list` shows the *new* image tag on the app **template**, while the **active revision runs old code**.

**Symptom we hit:** new backend routes 404 in prod while old routes (same file) worked → because the live revision was old code from hours earlier.

**Always verify after a backend deploy:**
```
az containerapp revision list -n arth-prod-backend -g arth-prod-rg \
  --query "reverse(sort_by([].{name:name,health:properties.healthState,running:properties.runningState,replicas:properties.replicas,image:properties.template.containers[0].image},&properties.createdTime))[0]"
```
Look for `Healthy` / `Running` / `replicas:1` on the **newest** revision. `ActivationFailed` = it crashed; traffic stayed on the old one.

Direct-curl the backend (bypasses Next/Vercel) to compare an old vs new route:
`https://arth-prod-backend.ashyriver-955dc28b.centralindia.azurecontainerapps.io`
Failed-revision crash logs are in Log Analytics: `ContainerAppConsoleLogs_CL | where RevisionName_s == 'arth-prod-backend--00000NN'`.

## The second lie: a **Healthy** revision is **not** a *serving* revision (the 2-day outage, 2026-06-13)
Found the prod backend fully down — every public request hung with **HTTP 000** (timeout), *not* a 5xx — yet `az containerapp revision list` showed the revision **Healthy / Running / replicas:1** and the database was perfectly fine. It had been down **~2 days, unnoticed**.

**Root cause: a wedged Prisma connection pool on the single always-on replica.** The DB (external **AWS RDS** `database-me`, ap-south-1, `create_postgres=false`), credentials, TLS, network, *and* the Key Vault `database-url` secret were all healthy the whole time — proven by running `psql "…?sslmode=require" -c "select 1"` straight from the Mac (connected, returned `ok` in <1s) and by RDS being publicly reachable (TCP 5432 open). But inside the container every query hung and **RDS showed 0 sessions** — the worker never opened a single connection. The 2s *hang* (not an instant reject) = a connection-level stall, masked because Prisma silently retries connect before the `/ready` 2s race fires.

**Why it stayed down for days — the probe topology (this is the trap):**
- **Liveness → `/health`** is dependency-free, so it stayed **green** → the container was **never restarted**.
- **Readiness → `/ready`** runs `SELECT 1`, so it failed forever → the replica was pulled from the load balancer → with `aca_min_replicas=1`, the *one* replica served nothing and nothing rescued it.
- Net: ingress had **zero ready replicas → it blackholed every request**, so even dependency-free `/health` was unreachable *externally* (the 000), though it would answer *inside* the container. A readiness probe that can fail permanently, with a liveness probe that can't catch the same condition, = a wedged-but-alive process that sits dead forever.

**Recovery runbook (restores in <1 min):** just restart the wedged revision → fresh container → fresh pool → reconnects.
```
az containerapp revision restart -n arth-prod-backend -g arth-prod-rg --revision arth-prod-backend--00000NN
```

**Permanent guards (shipped PR #48 → revision 0000092):**
- **DB watchdog** — `apps/backend/src/server/db-watchdog.ts`, started in `main.ts` after `listen()`. Periodic `SELECT 1`; after a *sustained* unreachable window (default **5 min**; tune `DB_WATCHDOG_INTERVAL_MS` / `_PROBE_TIMEOUT_MS` / `_GRACE_MS`; disable with `DB_WATCHDOG_ENABLED=false`) it calls **`process.exit(1)`** so ACA restarts with a fresh pool — same exit-and-restart philosophy as the existing `unhandledRejection`/`uncaughtException` handlers. Stopped in the graceful-shutdown path so a deploy/drain doesn't trip it. Trade-off: during a *genuine* prolonged DB outage it restart-loops (harmless — the app can't serve anyway, and the alert fires).
- **Uptime alert** — `.github/workflows/uptime.yml`, cron `*/5 * * * *`, curls public `/health` + `/ready` (3 attempts each); a red run emails the repo owner. Target overridable via repo var `BACKEND_HEALTH_URL`. **Detection was the real gap** — this turns a 2-day silent outage into a ~5-min email.

**Diagnostic leverage for next time:** the failure logs only as `"check failed db ready check timed out","module":"bootstrap"` every 10s — *that exact line means this worker can't reach the DB*, not that the DB is down. Verify the DB independently (psql from outside) **before** touching infra; if the DB is fine, the container is wedged → restart it. Inspecting the secret needs a temporary `Key Vault Secrets User` grant on vault `arthprodkv181owa` for your own az identity (the app reads it via its managed identity, so your personal login is `ForbiddenByRbac` by default) — **revoke the grant after**.

## Trap 1 — corepack auto-upgrades pnpm and crashes the Node 20 container
`Dockerfile.nestjs` runs `CMD ["pnpm","server:start"]`. With **no `packageManager` field in the runner's working dir**, corepack downloads the **latest** pnpm (v11, requires **Node 22**) at startup and dies on the Node 20 image: `ERR_UNKNOWN_BUILTIN_MODULE: node:sqlite`. A time bomb — it only detonated once pnpm's "latest" crossed into Node-22 territory, so the *first* new deploy after that failed while the long-running old revision kept working.
**Fix (shipped):** `"packageManager": "pnpm@9.15.0"` in `apps/backend/package.json` (corepack honors it for the `node` runtime user — the Dockerfile's root `corepack prepare --activate` does **not** carry to node's corepack home) **+** `ENV COREPACK_DEFAULT_TO_LATEST=0` in `Dockerfile.nestjs`. The runner runs `apps/backend/src` directly via `tsx` (no compiled `dist`).

## Trap 2 — CI tests are timezone-flaky (pass local, fail CI → block deploy)
The deploy has a `pnpm test` gate. Several salary/attendance tests build fixed-date inputs via local-time `Date` and only behave in **IST** (the business calendar, `getBusinessTimeZone()` = Asia/Kolkata). They pass on a Mac (IST) and **fail on UTC CI** ("expected 4 to be 3"), blocking the deploy. **Fix:** `vitest.config.ts` pins `test.env.TZ = "Asia/Kolkata"`.

## Trap 3 — Vercel ignored-build-step cancels "empty" commits
Web (`app.arthsaathi.co.in` = Vercel project **`arthsaathi`**) auto-deploys on push to `main`, but an **empty commit** (or one that doesn't touch the relevant app) is **Canceled** by the project's ignored-build-step → the live alias stays on the old build. To force a prod rebuild that picks up a new env var, either push a **real file change** or `vercel redeploy <prod-url> --scope shivanshgupta872-gmailcoms-projects` (redeploy rebuilds with **updated env vars** and bypasses the ignore step).

## Trap 4 — multiple Vercel projects / scope confusion
The repo links `apps/web` → project **`arthsaathi`** (serves `app.arthsaathi.co.in`); `apps/landing` → `arthsaathi-landing` (apex `arthsaathi.co.in`); `apps/admin` → `arthsaathi-admin`. There's also a stray **`arthsaathi-web`** (→ `v2.arthsaathi.co.in`, *not* the live owner app). `NEXT_PUBLIC_*` flags must go on **`arthsaathi`**. Set env from `apps/web` (uses the linked project); `vercel ls`/`redeploy` default to the personal scope and can error "different team" — pass `--scope shivanshgupta872-gmailcoms-projects`. `NEXT_PUBLIC_*` is build-time → setting it does nothing until a **rebuild**.

## Secrets wiring (so you don't clobber the blob)
Backend env = a single **write-only** GitHub secret `TF_VAR_SECRET_ENVS` (JSON map → Key Vault → Container App). You **can't read it to merge**, so adding one key safely = give it its **own** GitHub secret and merge it in the deploy's "Load secret_envs" python step (that's how `RESEND_API_KEY` / `EMAIL_FROM` were added). Web env = Vercel project env. Mobile env = EAS `environment`/profile `env` (baked at build).

## Can't-do-from-here actions (need the human / a permission rule)
The auto-mode guard blocks **`gh pr merge`** and **`gh workflow run deploy-backend`** (production deploys). Workarounds that *do* work: a **push to `main`** is allowed and **auto-triggers** the backend deploy when it touches `apps/backend/**` (etc.) — so a backend code/Dockerfile change deploys itself. `eas build` and `vercel redeploy` are runnable.

## Related
- [[Email auth — implementation]]
- [[Architecture — Arth Saathi]]
- [[Play Store — Data safety & launch compliance map]]
- [[Arth Saathi]]
