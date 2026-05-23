---
created: 2026-05-23
type: architecture
tags: arth-saathi, tech
---

# Monorepo layout — Arth Saathi

**Repo:** `/Users/shivanshgupta/Documents/arth` · **Manager:** pnpm workspaces

## Apps

| Package | Path | Role | Deploy |
|---------|------|------|--------|
| `@arthsaathi/landing` | `apps/landing` | Marketing site + demo (port 3200) | branch `landing` → arthsaathi.co.in |
| `@arthsaathi/web` | `apps/web` | Next.js 14 — owner dashboard, employee portal, `app/api` routes, Auth.js | `development` / `app.*` |
| `@arthsaathi/admin` | `apps/admin` | Internal admin panel (port 3100) | branch `admin` |
| `@arthsaathi/mobile` | `apps/mobile` | Expo / React Native owner + employee app | EAS |
| `@arthsaathi/backend` | `apps/backend` | Controllers, services, validators, schemas, Prisma | Azure Container Apps |

## Shared packages

| Path | Role |
|------|------|
| `lib/` | Auth, Prisma, env helpers |
| `types/` | Shared TS types |
| `packages/contracts` | API contracts |
| `packages/api-client` | Generated API client |
| `packages/design-tokens` | Tailwind / shadcn tokens |
| `packages/analytics-web` | Analytics SDK |
| `packages/i18n` | Localization |

## Key paths
- **Schema:** `apps/backend/prisma/schema.prisma`
- **AI entry:** `docs/AI/START.md` — authority ladder + task router
- **Architecture overview:** `docs/PROJECT_OVERVIEW.md`
- **EOC features catalog:** `docs/ops/features.md`
- **GST billing deep-dive:** `docs/architecture/gst-billing.md`

## Tooling
- **Test:** Vitest (`vitest.config.ts`, `vitest.server-only.ts`)
- **Local infra:** `docker-compose.yml`
- **Agent rule:** `.agents/skills/arth-saathi-monorepo-backend/SKILL.md`
- **Cursor rule:** `.cursor/rules/arth-saathi-monorepo-backend.mdc`

## Related
- [[Architecture — Arth Saathi]]
- [[Source-of-truth pointers — Arth Saathi]]
