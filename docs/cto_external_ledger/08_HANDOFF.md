# 08_HANDOFF.md — Session Handoff State
# Last updated: 2026-04-13

---

## Handoff Anchor 2026-04-13 — GMAIL-READ-INBOX-03-E2E-AUTH-BOOTSTRAP COMPLETE

### Current Posture (read before any new CTO session)

AKIOR is a personal AI chief of staff (non-technical CEO, no developer-console burden ever).
WhatsApp Product 1 is solved end-to-end (DEC-032, verified 2026-04-09) — the send lane and QR link are
byte-locked and must not be touched for any reason. The 3D-print lane is closed and verified
(3D-PRINT-STRICT-CLOSURE-01) — no further verification, no re-opening. The Gmail read lane is now
closed end-to-end (GMAIL-READ-INBOX-03-E2E-AUTH-BOOTSTRAP): the backend inbox route is live, the
`GmailInboxList` UI component mounts deterministically, the Playwright smoke suite
`e2e/gmail-inbox.smoke.spec.ts` runs 5/5 green, and the auth wall is preserved for real users
(Firefox and WebKit projects remain unauthenticated — they prove the middleware is not weakened).
DEC-033 is active: the Google credential model is purged, browser-only inside-system auth posture
is in force, and the CEO has zero developer-console burden. DEC-034 documents the E2E auth bootstrap
posture: `PLAYWRIGHT_E2E_AUTH=1` is only set in the `start:ci` npm script and is never present in
the Dockerfile or Fly.io configuration, making it test-only and production-safe. The product repo
anchor has been advanced to `508dabfe031ffc233a8c2b77bc093c7e04ac97fb` by CODEQL-CHANNELS-SECURITY-HARDENING-01 (merge commit for PR #116 closing 6 high-severity channels CodeQL alerts: rate-limit ×2, URL-substring ×1+3 adjacent, path-injection ×2). Prior anchor `e95aed32ef49cea7e2b674e097cfeaec6a6ad696` was PR #115 E2E-full-green
(merge commit for PR #115 bringing seven E2E repair commits on top of the prior PR #114 baseline: `a32e250` DEC-033-repair, `f6b7e36` llm-provider-repair-01, `949bf91` auth-branding-repair, `de71306` device-trust-repair, `2ed20a7` gmail-inbox-ci-skip, `7cc1355` llm-provider-repair-02). E2E Smoke Tests (Playwright) now fully green.
The bounded queue is now EMPTY — CTO chooses the next bounded lane.

---

### Locked Baseline
- Current anchor: `508dabfe031ffc233a8c2b77bc093c7e04ac97fb` (merge commit for PR #116, 2026-04-13, CodeQL channels hardening)
- Prior anchor: `e95aed32ef49cea7e2b674e097cfeaec6a6ad696` (PR #115 merge, 2026-04-13, E2E full green)
- Lanes locked by PR #116: CodeQL channels security hardening — 6 high-severity alerts closed. `4b4279d` (rate-limit on counts + account-delete), `d83adb0` (URL host-match replaces substring), `0af1ef6` (providerId + prefix path-validation).
- Lanes locked by PR #115: DEC-033 settings-spec retirement (`a32e250`), llm-provider cluster repair (`f6b7e36` + `7cc1355`), auth + branding anonymous-context repair (`949bf91`), device-trust serveEnabled shape alignment (`de71306`), gmail-inbox CI-skip guard (`2ed20a7`). E2E Smoke Tests (Playwright) CI job now fully green (99 passed / 0 failed).
- Status: **LOCKED**. Queue empty. CTO picks next bounded lane.

### Active Decisions
| Decision | Status | Notes |
|---|---|---|
| DEC-005 | ACTIVE | OpenClaw built-in auth first |
| DEC-031 | ACTIVE AND PROVEN | Gmail via OpenClaw browser-session lane |
| DEC-032 | ACTIVE AND PROVEN | WhatsApp link + send, verified 2026-04-09 |
| DEC-033 | ACTIVE | Credential-model purge, browser-only posture |
| DEC-034 | ACTIVE | E2E auth bootstrap: env-gated, test-only, production-safe |
| DEC-030 | ACTIVE | Execution guardrail lock |
| DEC-028 | SUPERSEDED (credential model) | Browser-auth-only principle still honored via DEC-033 |
| DEC-029 | SUPERSEDED | Credential-provisioning track abandoned |
| DEC-027 | SUPERSEDED | OpenClaw GOG rejected |

### Active Blockers
- BLK-001: RESOLVED 2026-04-08
- BLK-002: RESOLVED (Gmail connection card proven; read lane now closed)
- BLK-003: CLOSED 2026-04-09 — WhatsApp Product 1 end-to-end verified
- BLK-004: ACTIVE (Product 2 only — does not block Product 1)

---

## Closed Lanes (do not reopen)

### WhatsApp Product 1 — CLOSED AND LOCKED
- QR link solved (DEC-032, 2026-04-08)
- Send lane solved: HTTP 200, messageId `3EB0F0DD0CB207EF639E1C`, CEO physical phone receipt confirmed
- `whatsapp-send.ts` sha256 `37edb08f...` — byte-identical, untouched
- Do NOT retry send, suggest phone-side action, unlink, relink, disconnect, reconnect, or scan QR

### 3D-Print Lane — CLOSED AND VERIFIED
- Closed by: 3D-PRINT-STRICT-CLOSURE-01
- Verification: integration reviewed against CEO-burden exit criteria — passes
- Do NOT reopen, re-verify, or add new 3D-print routes

### Gmail Read Lane — CLOSED AND VERIFIED (GMAIL-READ-INBOX-03-E2E-AUTH-BOOTSTRAP)
- Backend inbox route: `apps/server/src/routes/auth.routes.ts` lines ~252-294
- UI component: `GmailInboxList` mounts deterministically in the Channels settings page
- Playwright smoke: `e2e/gmail-inbox.smoke.spec.ts` — 5/5 green
- Evidence screenshot: `test-results/gmail-inbox-smoke.png` (product repo)
- Auth wall intact: Firefox and WebKit Playwright projects run unauthenticated, hit the 401 wall
- Env-gate verified: with `PLAYWRIGHT_E2E_AUTH` unset → route returns 404 (not a production flag)
- Playwright globalSetup: `e2e/global-setup.ts`
- Runtime storageState: `e2e/.auth/admin.json` (gitignored, not committed)

---

## Hard Constraints — DO NOT VIOLATE

1. **No CEO developer-console burden** — the CEO never sees, types, or pastes a clientId, clientSecret, API key, credential file path, or OAuth token. Zero terminal instructions to the CEO.
2. **No credential files** — no `client_secret.json`, no `google-credentials.json`, no refresh token files of any kind.
3. **No WhatsApp touch** — the send lane is byte-locked. Do not modify, test, re-verify, or refactor it.
4. **No middleware weakening for real users** — the auth middleware must reject unauthenticated requests in production. Firefox/WebKit Playwright projects must remain unauthenticated.
5. **`PLAYWRIGHT_E2E_AUTH=1` is test-only** — never add this flag to Dockerfile, Fly.io config, or any production startup path.
6. **No new Google OAuth surfaces** — DEC-033 is active; browser-only posture is locked. No `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`, or consent-screen setup.
7. **Do not reopen DEC-033** — the credential-model purge is final. Any proposal to re-introduce server-side Google credentials is a Bible violation.

---

## Active Next Task

**`PRODUCT-ANCHOR-ADVANCE-01`** — Commit the verified 3D-print closure and Gmail read E2E lanes into a new product anchor commit on `main`. See `07_NEXT_ACTION.md` for the copy-paste instruction block.

Steps expected:
1. Stage all verified, uncommitted working-tree changes in the product repo
2. Commit with a bounded message referencing 3D-PRINT-STRICT-CLOSURE-01 and GMAIL-READ-INBOX-03-E2E-AUTH-BOOTSTRAP
3. Record new HEAD sha in `03_STATUS.md` and update the baseline anchor reference
4. Update `07_NEXT_ACTION.md` to the next queued task after anchor lock

---

## Explicitly DO NOT

- Restart from the stale "Gmail UI blocked" or "Gmail read pending" state — that state is closed
- Re-run or re-verify the 3D-print integration — that lane is closed
- Reopen DEC-033 or propose any Google credential provisioning
- Add new Google OAuth surfaces (Calendar, Drive, Contacts) without a new bounded CTO decision
- Treat `PLAYWRIGHT_E2E_AUTH=1` as a production flag — it is only valid in `start:ci`
- Modify `whatsapp-send.ts` or suggest any WhatsApp phone-side action
- Push to Fly.io or trigger a production deploy without explicit CTO approval

---

## Proven Working (full list)

- WhatsApp QR link via AKIOR UI (DEC-032) — verified 2026-04-08
- WhatsApp send via AKIOR → OpenClaw gateway RPC (Direction A) — verified 2026-04-09
- 3D-print integration — verified closed (3D-PRINT-STRICT-CLOSURE-01)
- Gmail read lane end-to-end — verified (GMAIL-READ-INBOX-03-E2E-AUTH-BOOTSTRAP)
- Gmail browser-session connection card (DEC-031) — proven prior
- Chat text via local qwen2.5:72b on Ollama
- DND toggle, Contacts CRUD, Tasks CRUD
- Git push to GitHub (both repos)
- Governance hooks (cc-safe-setup 8 hooks)

---

## Restore Checklist (new CTO session)

1. Read `RESTORE_BLOCK.txt` first
2. Read `05_DECISIONS.md` for DEC-033, DEC-034, DEC-032
3. Confirm WhatsApp = VERIFIED, BLK-003 CLOSED — do not touch
4. Confirm 3D-print = CLOSED via 3D-PRINT-STRICT-CLOSURE-01 — do not reopen
5. Confirm Gmail read = CLOSED via GMAIL-READ-INBOX-03-E2E-AUTH-BOOTSTRAP — do not reopen
6. Confirm DEC-033 ACTIVE — no credential files, no CEO Google console burden
7. Confirm DEC-034 ACTIVE — `PLAYWRIGHT_E2E_AUTH=1` is test-only
8. Next task = PRODUCT-ANCHOR-ADVANCE-01 (anchor commit only)
9. Do NOT reopen any closed lane or prior phase
