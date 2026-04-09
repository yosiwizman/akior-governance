# 06_BLOCKERS.md — Active Blockers

## BLK-001 — WhatsApp Session Conflict
- **Status:** RESOLVED 2026-04-08
- **Severity:** Closed
- **Detail:** Session conflict eliminated by W-T01 (Mac Mini session released via `openclaw channels logout`), W-T03 manual Phase 2 (AI Desktop stale credentials wiped), and W-T04.IMPL Phase 3 (fresh link achieved via AKIOR UI, real QR scan by CEO, creds.json persisted at 1840 bytes, openclaw channels list reports "linked, enabled").

## BLK-002 — Google OAuth Architecture
- **Status:** RESOLVED for Gmail via DEC-031 browser-session lane. OAuth architecture remains available as fallback. Calendar, Drive, Contacts remain under BLK-002 scope for their future slices.
- **Severity:** Low (Gmail resolved), Medium (Calendar/Drive/Contacts pending)
- **Detail:** Gmail is now served via the OpenClaw managed browser session (DEC-031), bypassing the OAuth-client lane entirely for Product 1. The OAuth-client refactor (I1-I8 + I4-AMEND-1) is retained as fallback infrastructure. Calendar, Drive, and Contacts will likely follow the same browser-session pattern but have not been wired yet.
- **Resolution (Gmail):** G-T06 chain complete. DEC-031 operative.
- **Resolution (Calendar/Drive/Contacts):** Future slices — mirror DEC-031 pattern when prioritized.

## BLK-003 — WhatsApp AI Desktop End-to-End Not Tested [CLOSED 2026-04-09]
- **Status:** CLOSED
- **Severity:** Closed
- **Link status:** RESOLVED 2026-04-08. QR link proven via W-T04.IMPL Phase 3.
- **Send status:** RESOLVED 2026-04-09. Phase 4.12B bounded runtime recovery + one controlled send returned HTTP 200, messageId `3EB0F0DD0CB207EF639E1C`. Unique test string `AKIOR-W-T05-P412B-20260409T174414Z-g7U31x` physically received on CEO phone. Gateway remained active and listener remained connected after send. Direction A (OpenClaw gateway RPC) confirmed.
- **Resolution chain:** Phase 4 (device pairing) → Phase 4.5 (audit) → Phase 4.6 (deviceIdentity fix) → Phase 4.7 (scope fix) → Phase 4.8 (fresh process) → Phase 4.9 (scope audit) → Phase 4.10 (identity restore) → Phase 4.11 (listener audit) → Phase 4.12A (runtime sanity) → Phase 4.12B (VERIFIED END-TO-END SUCCESS).

## BLK-004 — Option A Does Not Scale to AKIOR Light (Product 2)
- **Status:** ACTIVE — does NOT block Product 1, blocks Product 2 Google work only
- **Severity:** Medium (no impact on current execution lane)
- **Created:** 2026-04-07 by DEC-029
- **Detail:** DEC-029 selected Option A (personal-install OAuth app) for Product 1. This works for AKIOR Full on the AI Desktop because there is exactly one install. It does NOT work for AKIOR Light because each Mac Mini install would require its own Google Cloud OAuth app registration, which reintroduces the developer-console burden DEC-028 was meant to eliminate.
- **Resolution path:** Before any AKIOR Light Google work begins, Option B (shared pre-provisioned OAuth app, AKIOR-managed at company level) must be planned and resolved. This requires:
  1. CEO decision on Google Cloud project ownership (AKIOR LLC vs Yosi personally)
  2. Research on Google's OAuth verification process for sensitive scopes (gmail.modify, gmail.send) for apps serving more than 100 users
  3. A new planning artifact, likely G-T06.D2 or a Product 2 equivalent
- **Does NOT block:** G-T06 implementation for Product 1, any Product 1 work
- **Does block:** All AKIOR Light Google integration work
