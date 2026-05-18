# Change 003: Auth (Magic Link)

**Status:** Draft stub
**Priority:** P0
**Phase:** 1 (MVP)

## What

Email-based magic link authentication. User enters email, receives a one-time link, clicks it to authenticate. No password required.

## Why

Magic links reduce friction at sign-up (no password to remember) and are secure by design (link is single-use, time-limited). Preferred for a consumer SaaS where the primary action (upload audio) should be as frictionless as possible.

## Scope

- Email input form
- Magic link generation (signed JWT or opaque token, expires in 15 min)
- Email delivery (transactional email provider TBD)
- Token verification and session creation
- Session persistence (JWT in httpOnly cookie or localStorage)
- "Remember me" (longer session for repeat users)
- Free tier: 10 credits granted on first auth

## Out of Scope

- OAuth (Google, Apple) — future consideration
- Password-based auth — not in scope unless Alice decides otherwise
- Enterprise SSO — future consideration

## Open Questions

- Email provider: Resend vs Postmark vs SendGrid?
- Session length: 7 days default? 30 days with "remember me"?
- Passwordless only, or offer password as fallback?

## Acceptance Criteria

- [ ] User enters email → receives magic link email within 30s
- [ ] Clicking link → authenticated session
- [ ] Link is single-use (second click returns error)
- [ ] Link expires after 15 min
- [ ] Session persists across browser refreshes
- [ ] First-time user receives 10 free credits on activation
- [ ] Logged-in state visible in UI (email shown, credit balance shown)
