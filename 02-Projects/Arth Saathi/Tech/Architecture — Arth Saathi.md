---
created: 2026-05-23
type: architecture
tags: arth-saathi, tech
---

# Architecture — Arth Saathi

## Stack

| Layer | Choice |
|------|--------|
| **Web framework** | Next.js 14 (App Router) |
| **Mobile** | Expo / React Native (OTA via EAS) |
| **Backend** | Fastify + Nest controllers |
| **DB** | PostgreSQL on Amazon RDS (Mumbai) via Prisma |
| **Auth** | Auth.js v5 — phone OTP + Firebase |
| **AI** | LangGraph agent (confirm-before-write), Claude Opus 4.5 (77.47% BFCL V4) |
| **Payments** | Razorpay X (payouts) |
| **Messaging** | Twilio (SMS / WhatsApp) |
| **Hosting** | Vercel (web/admin/landing), Azure Container Apps (backend), EAS (mobile) |
| **Observability** | Sentry, Winston structured logging, `X-Request-Id` correlation |

## AI architecture — confirm-before-write
1. Owner says *"Ramesh ki haziri laga do, aaj half day"*
2. LangGraph agent resolves intent → calls tool with proposed mutation
3. Tool **returns a `ConfirmationCard`** (no write yet)
4. Owner taps Confirm / Reject
5. Only on Confirm: mutation commits, `AuditLog` row created

This is the architecturally hard part. Competitors that don't do this will burn trust the first time AI marks the wrong attendance.

See [[Confirm before write — AI mutations need human approval]].

## Data model invariants
- **Tenant isolation:** `businessId` on every model
- **Money in paise:** `Int`, never `Float`
- **Soft-delete:** `deletedAt` field, never hard delete
- **Audit by default:** every mutation gets an `AuditLog` row
- **DPDP compliance:** consent + retention + erasure paths built in

## Mobile sync
- Pull/push endpoints at `/api/mobile/{feature}/sync`
- Idempotent mutations
- Local-first queue for offline shops on 3G

## Related
- [[Monorepo layout — Arth Saathi]]
- [[Feature catalog]]
- [[Source-of-truth pointers — Arth Saathi]]
