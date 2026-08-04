---
created: 2026-06-11
status: active
type: project
tags: project, mobile, expo, react-native, supabase, nextjs, social
categories: [mobile, social]
audience: [personal, public]
repo: /Users/shivanshgupta/projects/spill
package: spill
github: https://github.com/sshivanshg/spill
---

# 💧 Spill

> **One-liner:** A campus-scoped anonymous-message inbox app — share your link, collect anonymous "spills," built for mass Play Store installs.

**Repo:** `/Users/shivanshgupta/projects/spill` · **GitHub:** [sshivanshg/spill](https://github.com/sshivanshg/spill) · **Version:** 1.0.0 (versionCode 10)

## 🎯 Goal
A polished, viral anonymous-messaging app aimed at college campuses: every spill is from a real person (no fake/bot messages), with a verified-student badge, campus leaderboards, daily prompts, and collectible gradient "skins" as the flex/identity hook.

## 🧱 Stack
- **Mobile app (`app/`):** Expo SDK 56 / React Native 0.85, `expo-router` 56, TypeScript
- **Backend/auth:** Supabase (`@supabase/supabase-js` 2.x)
- **Landing + send page (`web/`):** Next.js
- **Design language:** "playful-premium" — near-black indigo canvas, signature violet→pink→coral gradient, Unbounded display + Plus Jakarta Sans grotesk
- **Distribution:** EAS preview / APK download flow; targeting Play Store

## 📁 Repo structure
- `app/` — Expo mobile app
  - `src/app/` — expo-router routes: `(tabs)/` (index/inbox, campus, settings/profile), `message/`, `send/`, `onboarding`, `sign-in`, `verify`
  - `src/components/` — `ui.tsx` (design system: Screen, Button, GradientButton, Card, typography, Toast, SpillButton, etc.), `ShareCard`, `Background`
  - `src/lib/` — `theme.ts` (colors/font/space/glow tokens), `auth`, `api`, `supabase`, `skin`/`skins`, `prompts`, `share`, `haptics`, `push`, `versionCheck`, `config`
  - `AGENTS.md` — ⚠️ "Expo HAS CHANGED — read the v56 docs before writing any code"
- `web/` — Next.js landing page + `/m/<user>` and `/send` pages (Play-compliance pages, APK download flow)

## ✨ Key features
- Anonymous inbox with unread counts, daily "drop" prompt, reactions, day streaks
- Campus leaderboard ("most spilled-on" ranking) with opt-out
- Verified-student badge via college email (`verify` flow)
- Collectible gradient **skins** (Spill, Sunset, Mint, Cyber, Ember, Mono, Frost·seasonal) that re-theme the whole app
- Pause inbox, share link / share-as-story cards, in-app update flow

## 📝 Log
### 2026-06-11
- **UI fix:** Profile/Settings "Share" button was floating over the "SKIN" card with a pink glow halo. Root cause: `GradientButton` defaults `glowEnabled=true` → `glow()` applies Android `elevation: 10`, which lifts the button (and its shadow) above sibling cards regardless of layout bounds. Fixed in commit `9249b72` (settings.tsx: `glowEnabled={false}` + matched `height: 46`). The buggy screenshot was from a stale v10 APK built before the fix.
- Fixed the **same unguarded instance** on the Inbox tab ("Get more messages" card, `index.tsx:96`) — added `glowEnabled={false}` so its glow no longer bleeds onto the inbox list.
- **Lesson:** Android `elevation` floats a view above later-painted siblings — use `glowEnabled={false}` on any inline/secondary `GradientButton` placed near other content; keep glow only on standalone hero CTAs.
- **OTA set up (EAS Update):** Added `expo-updates ~56.0.19`; `app.json` now has `runtimeVersion: {policy:"appVersion"}` (runtime keyed off version `1.0.0`) + `updates.url` (EAS projectId `436e9ddc…`); `eas.json` build profiles got `channel: preview/production`. New `components/OtaReloader.tsx` silently checks/downloads OTA on launch+foreground and reloads on the next foreground (non-interrupting); `<UpdatePrompt />` still handles native/APK updates. Bumped `versionCode` 10→11 (first OTA-capable build) and `web/lib/app-version.ts` latestVersionCode→11.
- **Release model now:** JS/asset-only fix → `eas update --branch preview` (live in seconds, runtime `1.0.0`). Native/config change → bump `version` to `1.0.1` + new EAS build + existing APK version-gate flow. ⚠️ v10 users can't receive OTA (their build has no `expo-updates`) — they get v11 once via the APK prompt, then OTA from v11 on.

### 2026-06-12
- **Shipped v11 live.** EAS preview build `4812f016` finished; APK synced to `web/public/spill.apk` and deployed to Vercel prod (`spillget.vercel.app`). Verified live: `/api/app-version` → `latestVersionCode: 11`, `/spill.apk` md5 matches the v11 artifact. v10 users now get the in-app update prompt.
- **Gotcha:** `scripts/sync-apk.sh` prefers a local `app/build-outputs/preview.apk` over the EAS artifact — it had a stale v10 file (Jun 11) and synced the wrong APK. Fixed by copying the verified EAS artifact into both `public/spill.apk` and `build-outputs/preview.apk`. **Always md5-verify the synced APK against the EAS artifact before deploying**, or clear `build-outputs/` first.
- Commit `8ef1ccf` on branch `backend-foundation` (local; not yet pushed to origin).
- Removed the word "students" from the website (meta description, download counters, privacy/child-safety pages → "teens and adults"); deployed + verified live.
- **Instagram shareable cards (growth engine) — built.** New `components/sharecards/` system: `StoryCard` renders a true 1080×1920 story-native card with IG safe zones, skin gradient, and a baked-in handle/link lockup; 4 surfaces (invite / received-message / reply-reveal / milestone-streak) × 4 templates (Bold/Quote/Gradient/Minimal, swappable in a new `ShareSheet` with live preview). `lib/storyShare.ts` captures at 1080×1920 and posts via `react-native-share` `INSTAGRAM_STORIES` deep-link, falling back to the OS share sheet then text. Wired into inbox/profile/message screens; old 340×604 `ShareCard` removed. Spec: `docs/superpowers/specs/2026-06-12-instagram-shareable-cards-design.md`.
- **IG link reality (important):** Instagram does NOT allow auto-attaching a tappable link sticker via the API (verified vs Meta docs). The loop relies on the link being baked visibly into the card + copied to clipboard for a manual Link sticker.
- **Pending for cards:** a free Meta/Facebook App ID (create at **developers.facebook.com/apps**, NOT developers.meta.com/horizon which is Quest/VR) → set `app.json` `extra.facebookAppId` → new native **v12** EAS build (react-native-share is native, not OTA-able). Until the App ID is set, share falls back to the OS sheet.
- **Gotcha logged:** `react-native-share`'s Expo plugin needs `expo-build-properties` installed or prebuild fails; and `git add -A` swept a 92MB `build-outputs/preview.apk` into history (now gitignored + untracked).

## 💡 Ideas / Next
- Rebuild EAS preview / APK so installed app picks up the glow fixes
- Audit remaining `GradientButton` uses for the elevation-bleed pattern
- Push-notification polish · richer share cards · more seasonal skins

## 📚 Sources
- `/Users/shivanshgupta/projects/spill/app/src/app/(tabs)/settings.tsx`
- `/Users/shivanshgupta/projects/spill/app/src/components/ui.tsx`
- `/Users/shivanshgupta/projects/spill/app/src/lib/theme.ts`
- `/Users/shivanshgupta/projects/spill/web/README.md`

## 🔗 Connections
- [[Projects MOC]]
