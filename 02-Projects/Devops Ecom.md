---
created: 2026-05-24
status: active
type: project
tags: project, ecommerce, fullstack
categories: [fullstack]
audience: [freelance]
repo: /Users/shivanshgupta/Documents/Devops_Ecom
---

# 🛍️ Devops Ecom

> **One-liner:** Personalized e-commerce with **AI-driven recommendations** (MongoDB aggregation), **dynamic variant inventory**, role-based experiences, full DevOps pipeline (Terraform + Docker + Render).

**Repo:** `/Users/shivanshgupta/Documents/Devops_Ecom`

## Personalization
- **Style Quiz** captures preferences (style, fit, colors) on first visit
- **MongoDB aggregation pipeline** scores products against user preferences
- **Role-based views:** Guest / User / VIP / Admin

## Inventory
- **Variant matrix** — Size × Color combinations, individual SKU tracking
- Real-time stock updates with inline editing
- Frontend auto-disables out-of-stock variants

## Admin
- Product create/edit, toggle active, schedule drops
- Order tracking (Pending → Shipped → Delivered)
- Sales analytics — revenue charts, top sellers, KPI cards
- Site config — dynamic Hero + banners

## Media pipeline (Cloudinary)
- Direct browser uploads (drag-and-drop)
- Automatic WebP/AVIF optimization
- CDN distribution
- On-the-fly responsive sizing

## Security
- JWT auth with embedded roles
- Middleware guards on `/admin/*`
- Preview mode for admins on unpublished products

## DevOps
- Dockerfile
- `render.yaml` — Render.com deploy
- `terraform/` — IaC
- `Scripts/` — automation
- `SPLIT_COMMITS_GUIDE.md` — git workflow doc

## Sources
- README: `/Users/shivanshgupta/Documents/Devops_Ecom/Readme.md`
- Idea seed: `/Users/shivanshgupta/Documents/Devops_Ecom/Idea.md`

## Connections
- [[Projects MOC]]
