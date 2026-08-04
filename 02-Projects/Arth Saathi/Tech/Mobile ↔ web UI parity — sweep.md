---
created: 2026-06-02
type: architecture
status: implemented
tags: arth-saathi, tech, mobile, design-system, dark-mode, ux
repo_doc: /Users/shivanshgupta/projects/arth/docs/audits/MOBILE_WEB_UI_PARITY.md
---

# Mobile ↔ web UI parity — sweep

> Thinking + map. **Repo is truth:** `docs/audits/MOBILE_WEB_UI_PARITY.md` is the canonical audit + plan + outcome; this note is the orientation layer. Builds on the Phase-1 chrome fixes (nav/header/AI-chat/theme plumbing) recorded in the same doc.

## What looked wrong
"Mobile UI doesn't match the web, especially the theme colour." On the surface a vague complaint; underneath, the native app's dark mode kept rendering *light-palette* colours while everything around it flipped — icons, status pills, card surfaces stranded in the wrong scheme. The web phone rendering (the source of truth) looked right; mobile drifted.

## The non-obvious cause
Both apps consume the **same** `@arthsaathi/design-tokens`. So the divergence was never the tokens — it was **how mobile reached for colour**. NativeWind className utilities (`bg-ds-*`, `text-ds-*`) auto-flip via the `.dark` CSS-variable scope, exactly like web's `var(--ds-*)`. But three escape hatches bypass that machinery and freeze the light palette:

1. **Hardcoded hex in RN `color=` / `placeholderTextColor=` props** — lucide icons and inputs take literal strings, so `#0D4A3A` stays `#0D4A3A` in dark mode (**377 occurrences** across ~80 files).
2. **Literal Tailwind palette classes** (`bg-emerald-50`, `text-rose-700`, `bg-amber-50`) instead of the semantic `ds-*` status tokens — they don't flip and they don't match the web element (**246 occurrences**).
3. **`bg-white` on card surfaces** — a white card on a near-black canvas.

The general lesson, worth carrying everywhere: **a colour that can't go through the theme variable must go through a hook that resolves the palette — otherwise dark mode is a lie the moment any JS-side colour prop appears.** → [[JS color props must track the theme, not hardcode hex]]

## The fix — one hook, a rubric, and fan-out
- **`lib/use-theme-colors.ts`** (from Phase 1) is the single home for JS-side colour: it returns `ds` in light and `dsDark` in dark, so icon/`style` colours track the resolved scheme exactly like `var(--ds-*)` does on web. The whole sweep is "route literals through this hook, or swap to a `ds-*` class."
- **A precise rubric** kept judgment consistent: preserve `#FFFFFF` on filled-brand buttons (that *is* `text.onDark`) and brand `shadowColor` greens (shadows don't flip); never use `/opacity` on a `ds-*` CSS-var colour in NativeWind; match the **web element's** token for each status pill rather than blanket-replacing.
- **11 parallel domain fix-agents**, each owning a disjoint file lane (dashboard, billing×2, inventory, attendance, salary, khata, employees, employee-portal, profile, auth) + a central shared-chrome pass. Disjoint lanes meant no edit conflicts; edits land on disk incrementally, so a dropped agent connection never lost work.

Result: hardcoded hex **377 → 77**, literal palette classes **246 → 13** (the remainder all intentional — white-on-brand, brand logo, fixed light auth chrome, dark-hero on-card tints that mirror web). `tsc` 0 errors, `expo lint` 0/0.

## Two foot-guns found mid-sweep
- **Undefined tokens that render colourless:** `text-ds-text-strong` and `text-ds-error` were used in several files but **don't exist** — the only valid text tokens are `ds-text` / `ds-text-body` / `ds-text-muted`, and danger text is `ds-danger-text`. Swept app-wide.
- **Structural gaps, not just colour:** the parity ask surfaced real missing UI — the entire **`/inventory/reports` screen didn't exist on mobile** (built it), the inventory search box was a dead `<Text>` placeholder, the dashboard's "still to mark" primary action had decayed into a tiny text line, billing list rows lost their leading icon badges. Colour was the loudest symptom; the audit caught the silent structural drift too.

## Deliberately deferred (not regressions)
Web's `rounded-[28px]` **sage gradient + soft-shadow** card chrome was matched with flat token cards (correct radius + `bg-ds-surface`/`border-ds-border`), not the literal gradient — that needs `expo-linear-gradient` (a native rebuild), declined for the v1 release. Also: `BillingUsageChip`, per-employee "mark as paid" in the payroll row (needs a contract field), and the ~19 pre-existing `/opacity`-on-`ds-*` files that ship today.

## Follow-on (2026-06-03) — component geometry & typography
The colour sweep above fixed *theme flipping*. A second pass closed the
**component-level** gaps that survived it — same lesson, different layer: shared
tokens, but each primitive *composed* them differently from web's phone scale.
- **What drifted:** buttons `text-base/font-semibold` → web `text-sm/font-medium` (+ `sm`/`lg` sizes, `secondary` palette, `link` variant, `shadow-sm`); **Tabs pills → segmented control** (web `bg-muted rounded-xl` + `rounded-lg` triggers with active `bg-background shadow`); `StatCard` `rounded-xl`→`rounded-2xl` + shadow + squircle icon + `font-heading` value; `Badge` `py-1`/`font-medium` → `py-0.5`/`font-semibold`; `Select` trigger 24px→`rounded-[14px]`; `Skeleton` base radius; `StatusPill` `not-marked` stale grey → web's action-required amber; `BottomNav` AI icon 26→24, shadow params, strokeWidths, + the missing active-tab underline dot.
- **The residual dark-mode lie:** `Input`/`Textarea` hardcoded `placeholderTextColor="#9CA3AF"` — frozen light grey on dark. Missed by the 06-02 grep because it's a `placeholderTextColor` prop, not `color=`. → routed through `useThemeColors().textMuted`. Reinforces [[JS color props must track the theme, not hardcode hex]]: the bypass audit must cover **every** colour-bearing RN prop.
- **Direction call:** web vs mobile disagreed stylistically (button text size, pill-vs-segmented tabs). The standing rule is "mobile aesthetic is sacred"; this task said "pixel-faithful to web." Surfaced the conflict explicitly → user chose **strict-literal, web = truth**. So this pass bends mobile to web; the sacred-aesthetic rule still governs *how* (reuse tokens, no invented colours, no broken layouts).
- **Deferred (documented, not regressions):** mobile renders **synthetic bold** — Inter is registered as discrete family names so `font-bold`/`font-semibold` only set `fontWeight`; true parity needs a weight→family map (load `Inter_700Bold`). `Card` base keeps its load-bearing built-in `p-4` (web product cards override the shadcn `p-6` base anyway). `NotificationBar` stays a multi-variant alert (its `variant` prop is API the web promo-bar lacks — styles-only scope). `mini-calendar` is a **different widget** per platform (web week-strip vs mobile month-grid), not a parity pair.
- **Repo doc:** `docs/audits/MOBILE_WEB_UI_PARITY_AUDIT.md` (this follow-on; the original sweep is `MOBILE_WEB_UI_PARITY.md`).

## Map
- Hook: `apps/mobile/lib/use-theme-colors.ts` · tokens: `packages/design-tokens/src/index.ts`
- New screen: `apps/mobile/app/(dashboard)/inventory/reports.tsx`
- Audit + plan + outcome: `docs/audits/MOBILE_WEB_UI_PARITY.md`
- ~92 files changed under `apps/mobile`; no web files touched (web = source of truth)

## Related
- [[JS color props must track the theme, not hardcode hex]]
- [[Architecture — Arth Saathi]]
- [[Monorepo layout — Arth Saathi]]
- [[First-run walkthrough — feature]]
