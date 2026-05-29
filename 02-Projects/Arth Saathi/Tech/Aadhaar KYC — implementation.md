---
created: 2026-05-24
type: architecture
status: implemented (mock provider) — real provider config-gated
tags: arth-saathi, tech, compliance, kyc
repo_doc: /Users/shivanshgupta/Documents/arth/docs/AADHAAR_KYC.md
---

# Aadhaar KYC — implementation

> Thinking + map. **Repo is truth:** `docs/AADHAAR_KYC.md` is the canonical spec; this note is the orientation layer. Product side: [[Aadhaar eKYC — feature]].

## The one decision everything hangs on
We are **not** a UIDAI AUA/KUA and never call UIDAI directly. Verification is delegated to a **licensed sub-AUA/KUA provider** behind a pluggable adapter. This keeps us legal without a UIDAI licence and lets us swap vendors by config. Aadhaar is used for **KYC only**, never login (not a permitted purpose).

## Provider abstraction
```
/api/kyc/aadhaar/{status,initiate,verify}  (Fastify, employee-self)
        │
        ▼
  aadhaar-kyc.service.ts   ← ALL policy: validate, dedup, consent,
        │                     rate-limit, OTP TTL/attempts, persist
        ▼
  AadhaarKycProvider (interface)
    ├── mock-provider.ts      default; full flow, zero creds (OTP 123456)
    └── cashfree-provider.ts  licensed sub-KUA; real, withRetry + breaker
```
Selected by `AADHAAR_PROVIDER` env. Adding a vendor = implement the interface + register in the factory. Mirrors our [[Confirm before write — AI mutations need human approval|service-owns-policy]] discipline — the provider is dumb transport.

## Data handling — never store the raw Aadhaar
Consistent with the [[Architecture — Arth Saathi]] invariants (tenant isolation, audit-by-default, DPDP). The raw 12-digit number is used transiently to send the OTP and compute derivatives, then discarded. Persisted on `Employee`:
- `aadhaarMasked` — `XXXX XXXX 1234`, display only
- `aadhaarHash` — salted HMAC-SHA256; the `(businessId, aadhaarHash)` **unique index** enforces *one Aadhaar per business* atomically
- `aadhaarRefId` — provider/UIDAI reference token
- `aadhaarVerified`, `aadhaarKycStatus`, `aadhaarVerifiedAt`, `aadhaarVerifiedName`

Every attempt → append-only `AadhaarKycVerification` row (actor, masked number, provider, outcome, IP). Consent → `EmployeeConsent` (`AADHAAR_KYC`). Logs only ever carry the masked form.

## Availability — "coming soon" in prod, live in dev
Single backend flag `config.aadhaar.available`: **true in dev/test** (mock makes it work out of the box), but in **production only once a real provider + creds are wired** — otherwise the UI greys it to "Coming soon" and the routes return `503 COMING_SOON`. Rationale: the mock verifies anyone with the test OTP, so it must never serve real users in prod (enforced twice — service gate + factory refusing mock in prod). Frontends read `available` from the status endpoint.

## State machine
`NOT_STARTED → OTP_SENT → VERIFIED | FAILED`. Guards: Verhoeff checksum pre-check, per-tenant dedup, OTP send rate-limit (5/hr), OTP TTL (10 min) + attempt cap (5), and a race-safe verify (unique-index violation → `AADHAAR_IN_USE`).

## Surfaces
- **Web** (`apps/web`): `useAadhaarKyc` hook → 4-step wizard → `/employee/kyc`, profile badge. Reaches backend through the Next `/api/[...path]` proxy.
- **Mobile** (`apps/mobile`): mirrored screen + hooks, calls backend directly with the Bearer JWT.
- Both reuse the shared pure helpers in repo `lib/aadhaar.ts` (Verhoeff/mask/format).

## Config (see repo `.env.example`)
`AADHAAR_PROVIDER` (mock|cashfree) · `AADHAAR_KYC_ENABLED` · `AADHAAR_HASH_SECRET` (dedicated in prod) · `AADHAAR_PROVIDER_CLIENT_ID/_SECRET` · `AADHAAR_OTP_TTL_MINUTES` · `AADHAAR_MAX_OTP_ATTEMPTS` · `AADHAAR_MAX_INITIATE_PER_HOUR`.

## Go-live checklist
- [x] Run migration on prod DB (`prisma migrate deploy` — `20260524160000_aadhaar_kyc`)
- [ ] Choose provider; set `AADHAAR_PROVIDER` + creds
- [ ] **Confirm Cashfree request/response field names against live docs** (adapter reads defensively but vendor payloads drift)
- [ ] Set a dedicated `AADHAAR_HASH_SECRET` (rotating it invalidates dedup)

## Tests
`lib/aadhaar.test.ts` (Verhoeff/mask), `aadhaar-kyc.schema.test.ts` (consent gate), `aadhaar-kyc.service.test.ts` (happy path, dedup, rate-limit, cross-tenant, wrong-OTP/burn, expiry, race). 29 tests green.

## Related
- [[Aadhaar eKYC — feature]]
- [[Architecture — Arth Saathi]]
- [[Monorepo layout — Arth Saathi]]
- [[Compliance integration is a structural moat]]
- [[Source-of-truth pointers — Arth Saathi]]
