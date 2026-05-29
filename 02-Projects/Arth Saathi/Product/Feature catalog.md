---
created: 2026-05-23
type: product
tags: arth-saathi, features
---

# Feature catalog

> Top-level surface area. See repo `docs/ops/features.md` for the every-feature catalog (EOC).

## Core (live)

### Attendance
- Daily mark-in (GPS optional)
- Full-calendar view
- Bulk holiday tagging
- Weekly-off auto-apply

### Leaves
- Request → approval flow
- Paid / unpaid balance accrual
- Integrated salary impact

### Payroll
- Monthly salary calc (daily-rate divisor per shop setting)
- Advances + bonus + net payable
- Salary-slip PDF (Growth+ tier)
- WhatsApp delivery (Growth+)

### Operations
- **GST billing & invoicing** (F-3.1, atomic FY-scoped counter) — see `docs/architecture/gst-billing.md`
- Inventory + barcode
- Khata (vendor/customer ledger)
- e-invoicing
- e-way bill

### AI
- Conversational chat (Claude Opus 4.5)
- Confirm-before-write on mutations (`ConfirmationCard`)
- WhatsApp bot
- Voice agent (in development)
- Anomaly detection
- Cash-flow forecast
- Ask-anything analytics

### Employee app
- Self-attendance
- Leave requests
- Payslip view
- Advance requests
- Salary-on-demand
- Verified work-history QR
- Reference letter requests

## Roadmap

### Q1 — Compliance + fintech entry
- PF auto-calc + ECR filing
- ESI auto-calc
- UPI salary disbursement
- **Aadhaar eKYC — ✅ built** (mock provider live, real provider config-gated) → [[Aadhaar eKYC — feature]]

### Q3 — Fintech expansion
- Earned Wage Access (EWA)
- Employee micro-loans (referral)
- Group insurance
- Owner working-capital loans

### Q4 — Verticals
- Construction pack
- Restaurant pack
- Salon/spa pack
- Coaching pack
- Factory pack

### Q6 — Platform
- Tally / Zoho Books export
- Open API for CAs
- CA marketplace
- Plugin marketplace

## Compliance roadmap (Q1–Q4)
PF · ESI · Professional Tax (state-wise slabs) · TDS · Form 24Q · Form 16 issuance · Statutory bonus + gratuity · F&F settlement · Minimum-wage alerts · Labour Codes readiness checklist · Shops & Establishment helper

## Related
- [[Architecture — Arth Saathi]]
- [[UX principles — Arth Saathi]]
- [[Aadhaar eKYC — feature]]
- [[Source-of-truth pointers — Arth Saathi]]
