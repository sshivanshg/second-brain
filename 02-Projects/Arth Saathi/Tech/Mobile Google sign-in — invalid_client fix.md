---
created: 2026-06-03
type: architecture
status: implemented
tags: arth-saathi, tech, mobile, auth, oauth
repo_doc: /Users/shivanshgupta/Documents/arth/docs/features/MOBILE_GOOGLE_SIGNIN.md
---

# Mobile Google sign-in — invalid_client fix

> Thinking + map. **Repo is truth:** `docs/features/MOBILE_GOOGLE_SIGNIN.md` is the canonical runbook; this note is the orientation layer.

## What broke
An owner tapped "Continue with Google" on the Android app and hit a Google dead-end: **"Access blocked: The OAuth client was not found · Error 401: invalid_client."** Not a crash, not a server error — a terminal page on `accounts.google.com` that doesn't redirect back, so the only exit is the X. First impression on the marquee one-tap login: broken.

## The non-obvious cause
A Google OAuth client is **bound to one platform and its redirect rules**, and the three types are *not interchangeable* even though the client-ID strings look identical:
- **Web** clients only accept `https://` / `localhost` redirects.
- **iOS / Android** clients accept a **custom-scheme** redirect (bundle/package based) + are pinned to bundle-id (iOS) or package + signing **SHA-1** (Android).

On a native standalone build, `expo-auth-session@7` sends the redirect `in.arthsaathi.app:/oauthredirect` — a custom scheme. A **Web** client *rejects* it. So pointing the native flow at a Web client ID is precisely what produces `invalid_client`.

And that's exactly what the config did. Google sign-in was **enabled** (`EXPO_PUBLIC_GOOGLE_OAUTH_ENABLED=true`) but the only client ID present — `…0div…`, the **Web** client (it pairs with the `GOCSPX-` secret and is what web NextAuth uses) — was pasted into the **iOS** slot, while the Web and Android slots were blank. A code "convenience" made it worse: `google-auth.ts` *silently fell native client IDs back to the web client*, so the button would mount on the phone and then dead-end.

## The fix — make a wrong client *type* un-representable, don't paper over it
One-line model: **the button may only mount where the redirect can actually complete.**
- Removed the cross-platform fallback in `googleClientIds()`. Each platform reads *its own* env var or nothing. A Web client can no longer masquerade as the native client.
- `isGoogleSignInEnabled()` therefore gates per-platform: blank native slot → **button hidden** (phone+password untouched), never a dead-end. Add a native client later → button appears automatically, no code change.
- Re-labelled `.env`: `…0div…` is the **Web** client → moved to `EXPO_PUBLIC_GOOGLE_WEB_CLIENT_ID`; iOS/Android slots left blank with a TODO + doc pointer.
- Added a recoverable Alert in `OwnerGoogleSignIn` for *redirected* OAuth errors (`access_denied`, `redirect_uri_mismatch`, …) instead of swallowing them.

Net today: **web/PWA works** (Web client + https redirect); **native shows nothing until a native client exists** — honest, not broken.

## The hard external dependency I can't code around
Making the button *work on the phone* needs a one-time Google Cloud step that only the account owner can do: create an **Android** OAuth client (package `in.arthsaathi.app` + the build's signing **SHA-1**, from `eas credentials -p android`) and an **iOS** client (bundle `in.arthsaathi.app`), paste their IDs into the env, rebuild. The server already accepts all three as token `aud`, so no backend change. Runbook §6 has the steps.

## The tempting wrong fix I rejected
"Just put the Web client ID in the Android slot too." That *is* the bug — it re-creates the dead-end. There is no repo-only shortcut around the SHA-1 registration; the native client is non-negotiable for the custom-scheme redirect.

## Map
- Resolution + gating: `apps/mobile/lib/google-auth.ts` (`googleClientIds`, `googleClientIdForPlatform`, `isGoogleSignInEnabled`)
- Button + error UX: `apps/mobile/components/auth/OwnerGoogleSignIn.tsx`
- Config: `apps/mobile/.env` + `.env.example` (three platform-typed client IDs)
- Server verify (`aud` ∈ web/ios/android): `apps/backend/src/services/google-mobile-id-token.service.ts`
- Flow into app session: `apps/mobile/lib/auth.ts` → `POST /api/mobile/auth/google`

## Related
- [[Session persistence — mobile PWA stays logged in]]
- [[Mobile ↔ web UI parity — sweep]]
- [[Monorepo layout — Arth Saathi]]
