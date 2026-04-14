# 08_HANDOFF.md — Session Handoff State
# Last updated: 2026-04-13

---

## Handoff Anchor 2026-04-14 — CODEQL-NON-CHANNELS-RATE-LIMITING-CLUSTER-03-USER-DATA-CRUD COMPLETE

### Current Posture (read before any new CTO session)

Cluster 03 of the non-channel rate-limit lane is closed. Product anchor advanced from `3931498…` to `3d81f2c5ff3d6b53349ec0ae45c61be9ba226d10` via PR #121 (merge 2026-04-14). Nine CodeQL js/missing-rate-limiting alerts retired on live main (all fixed_at=2026-04-14T18:59:27Z) — #59 memory, #63 settings-GET, #64 settings-POST, #67 file-library-GET, #68 file-library-DELETE, #69 images, #70 tasks, #71 dnd, #72 contacts. Single-file bounded edit in `apps/server/src/index.ts`: rate-limit prelude inserted at the top of each of the nine user-data CRUD handlers; four one-liner handlers (#67/#70/#71/#72) expanded to receive (req, reply). PR #121 landed in two commits: `c7f2bf2` initial preludes, then `db04efa` in-PR bucket fixup after the first CI E2E run hit a 429 cascade on polled GET endpoints (evidence: run id 24416602047). READ endpoints now use inline `{maxAttempts:600, windowMs:60_000, lockoutMs:60_000}` (10 req/sec, 1-min lockout); MUTATING endpoints (POST settings, DELETE file-library) keep `ADMIN_MODERATE`. Same helper, same prelude shape, single-file boundary preserved. Second CI run: 13/13 green. **Scope boundary:** Cumulative non-channel rate-limit lane status = 19/20 retired (C01: 4, C02: 6, C03: 9). Cluster 04 (3D-print alert #54, 1 alert) is locked-lane excluded and is the sole remaining carryover. Do not begin Cluster 04 implementation — the 3D-print lane is LOCKED per RESTORE_BLOCK.txt and requires an explicit new CTO decision to unlock. CTO picks next bounded lane.

---

## Handoff Anchor 2026-04-14 — CODEQL-NON-CHANNELS-RATE-LIMITING-CLUSTER-02-EXTERNAL-API COMPLETE

### Current Posture (read before any new CTO session)

Cluster 02 of the non-channel rate-limit lane is closed. Product anchor advanced from `76aa4fb…` to `3931498d99b3b7ca0e6fedb3e549e1b570b32a74` via PR #120 (merge 2026-04-14). Six CodeQL js/missing-rate-limiting alerts retired on live main (all fixed_at=2026-04-14T17:54:14Z) — #58 ollama/models, #60 elevenlabs/tts, #61 azure-tts/tts, #62 spotify/search, #65 weather/query, #66 openai/text-chat. Single-file bounded edit in `apps/server/src/index.ts` (+84/-0): rate-limit prelude inserted at the top of each of the six external-API handlers, using the same proven reusable `checkRateLimit + RateLimitPresets` helper shape that closed Cluster 01. Paid outbound APIs (elevenlabs/azure-tts/openai) use ADMIN_MODERATE; read-only external/local (ollama/spotify/weather) use ADMIN_LIGHT. No new framework, no plugin, no middleware, no auth-semantic change. 13/13 CI checks green. **Scope boundary:** Cluster 03 (user-data CRUD, 9 alerts) remains carryover. Cluster 04 (3D-print, 1 alert) is locked-lane excluded. Cumulative non-channel lane status: 10/20 alerts retired, 9 carryover, 1 locked. The broader non-channel rate-limit lane is NOT fully retired. CTO picks next bounded lane.

---

## Handoff Anchor 2026-04-14 — CODEQL-NON-CHANNELS-RATE-LIMITING-CLUSTER-01-LLM-AUTH COMPLETE

### Current Posture (read before any new CTO session)

Cluster 01 of the non-channel rate-limit lane is closed. Product anchor advanced from `9adc7fb…` to `76aa4fbdae34454114479a6a3e39aa38847d05c5` via PR #119 (merge 2026-04-14). Four CodeQL js/missing-rate-limiting alerts retired on live main (all fixed_at=2026-04-14T15:58:44Z): #53 `auth.routes.ts:163` (pin-login), #55 `llm.routes.ts:91`, #56 `llm.routes.ts:110`, #57 `llm.routes.ts:242`. Bounded edits in two files only (+40/-10): extended/reordered the existing `checkRateLimit + RateLimitPresets` helper so the rate-limit prelude runs BEFORE any authorization check. No new framework, no plugin registration, no middleware refactor. 13/13 CI checks green. **Scope boundary:** Cluster 02 (external-API, 6 alerts) and Cluster 03 (user-data CRUD, 9 alerts) remain carryover — not implemented here. Cluster 04 (3D-print, 1 alert) is locked-lane excluded. The broader non-channel rate-limit lane is NOT fully retired; only Cluster 01 is closed. CTO picks next bounded lane.

---

## Handoff Anchor 2026-04-14 — GMAIL-INBOX-LIST-CANONICAL-E2E-01 COMPLETE

### Current Posture (read before any new CTO session)

Capability-factory gate proven: one canonical Google/Gmail read slice is now end-to-end verified on the approved browser-first / inside-system architecture (DEC-031 managed-browser lane, DEC-033 credential-purge posture). The proof was achieved with **zero product code changes**. Product anchor is **RETAINED** at `9adc7fb13e35293ff866ea7717fed288933de965`. Live evidence: OpenClaw gateway `running:true pid:3209842`, signed-in Gmail tab at `mail.google.com` (identity `yosiwizman5638@gmail.com`), Gmail API endpoints `/accounts /status /inbox-summary /inbox` all HTTP 200 with live payloads (unread=12, rowCount=62, 3 real messages), UI `/settings/channels/email` renders connected-state card with populated `GmailInboxList`, Playwright chromium `e2e/gmail-inbox.smoke.spec.ts` 5/5 passed (incl. the live-session-required inbox-list spec that skips in CI). Screenshot: `test-results/gmail-inbox-smoke.png`. This proof **does not mean** Calendar/Drive/Contacts are implemented — those remain future clone candidates of this proven pattern, not executed here. Bounded queue remains EMPTY. CTO picks next bounded lane.

---

## Handoff Anchor 2026-04-14 — CODEQL-CHANNELS-INSECURE-RANDOMNESS-IDENTITY-TRUTH-04B COMPLETE

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
anchor has been advanced to `9adc7fb13e35293ff866ea7717fed288933de965` by CODEQL-CHANNELS-INSECURE-RANDOMNESS-IDENTITY-TRUTH-04B (merge commit for PR #118 retiring js/insecure-randomness alert #1 at accountsIndex.ts:179 by replacing Math.random() with crypto.randomBytes() in mintAccount). Prior anchor `7654a9a3668f1f2475fbf27d537377196c68c708` was from CODEQL-CHANNELS-PATH-INJECTION-CLOSURE-03 (merge commit for PR #117 retiring path-injection alerts #6 and #7 on browserSession.ts via canonical path.basename() sanitizer at sink). Prior anchor `508dabfe031ffc233a8c2b77bc093c7e04ac97fb` came from CODEQL-CHANNELS-SECURITY-HARDENING-01 (merge commit for PR #116 closing 6 high-severity channels CodeQL alerts: rate-limit ×2, URL-substring ×1+3 adjacent, path-injection ×2). Prior anchor `e95aed32ef49cea7e2b674e097cfeaec6a6ad696` was PR #115 E2E-full-green
(merge commit for PR #115 bringing seven E2E repair commits on top of the prior PR #114 baseline: `a32e250` DEC-033-repair, `f6b7e36` llm-provider-repair-01, `949bf91` auth-branding-repair, `de71306` device-trust-repair, `2ed20a7` gmail-inbox-ci-skip, `7cc1355` llm-provider-repair-02). E2E Smoke Tests (Playwright) now fully green.
The bounded queue is now EMPTY — CTO chooses the next bounded lane.

---

### Locked Baseline
- Current anchor: `3d81f2c5ff3d6b53349ec0ae45c61be9ba226d10` (merge commit for PR #121, 2026-04-14, non-channel rate-limit Cluster 03 user-data CRUD closure)
- Prior anchor: `3931498d99b3b7ca0e6fedb3e549e1b570b32a74` (PR #120 merge, 2026-04-14, non-channel rate-limit Cluster 02 external-API closure)
- Lanes locked by PR #121: 9 CodeQL js/missing-rate-limiting alerts closed (#59, #63, #64, #67, #68, #69, #70, #71, #72) by commits `c7f2bf2` (initial preludes) + `db04efa` (in-PR bucket fixup: READ buckets raised to 600/min/60s-lockout after first CI E2E 429 cascade). Post-merge live CodeQL re-scan confirms all nine `state=fixed, fixed_at=2026-04-14T18:59:27Z`.
- Cumulative non-channel rate-limit lane status: **19/20 alerts retired** (Cluster 01: 4, Cluster 02: 6, Cluster 03: 9). Cluster 04 (3D-print, alert #54, 1 alert) is locked-lane excluded and is the sole remaining carryover — the 3D-print lane is LOCKED and requires an explicit new CTO decision to unlock before any limiter can be added there.
- Lanes locked by PR #119: 4 CodeQL js/missing-rate-limiting alerts closed (#53, #55, #56, #57). Commit `24abc8b` added rate-limit prelude before authorization in `auth.routes.ts` pin-login (guard tightened) and in the three admin llm handlers. Post-merge live CodeQL re-scan confirms all four alerts `state=fixed, fixed_at=2026-04-14T15:58:44Z`.
- Canonical capability proof 2026-04-14: GMAIL-INBOX-LIST-CANONICAL-E2E-01 CLOSED against the prior anchor `9adc7fb…`. Screenshot artifact: `test-results/gmail-inbox-smoke.png` in the product repo working tree (not committed; proof artifact only).
- Carryover for the non-channel rate-limit lane: Cluster 02 (external-API, 6 alerts, index.ts), Cluster 03 (user-data CRUD, 9 alerts, index.ts). Cluster 04 (3D-print, 1 alert) locked-lane excluded.
- Lanes locked by PR #118: CodeQL js/insecure-randomness alert #1 closed at `apps/server/src/channels/accountsIndex.ts:179`. Commit `09e22df` replaces `Math.random().toString(36).slice(2, 8)` with `randomBytes(4).toString("hex").slice(0, 6)` inside `mintAccount` (+2 / -1, 1 file). Post-merge live CodeQL re-scan confirms alert #1 state=fixed, fixed_at=2026-04-14T11:52:23Z. All channels-surface CodeQL high-severity alerts retired. Non-channel `js/missing-rate-limiting` lane is now authorizable for a dedicated future bounded task.
- Lanes locked by PR #117: CodeQL js/path-injection alerts #6 + #7 closed at browserSession.ts:206 via `path.basename()` canonical sanitizer at sink. Commits: `fe87a2f` (initial containment-check attempt; CodeQL did not recognize the pattern), `7a96d82` (switched to `path.basename()` which IS CodeQL-canonical). Post-merge live CodeQL re-scan (2026-04-14T01:53:49Z) confirms both alerts state=fixed.
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

**None queued.** Queue is EMPTY. CTO chooses the next bounded lane — there is no automatic next step. Cluster 04 (3D-print rate-limit, alert #54) remains locked-lane excluded and is NOT the automatic next task; unlocking requires an explicit new CTO decision. Prior `PRODUCT-ANCHOR-ADVANCE-01` framing is CLOSED (2026-04-13) and its INSTRUCTION BLOCK in `07_NEXT_ACTION.md` is archived — do not re-run. Current product anchor is `3d81f2c5ff3d6b53349ec0ae45c61be9ba226d10`; governance anchor matches.

Candidate future work (CTO picks, not Claude):
- auth-middleware gap on `/api/channels/gmail/inbox*`
- Google Calendar canonical slice (clone of the proven Gmail browser-session pattern) — NOT automatic; requires explicit CTO task prompt
- AKIOR Light / Cloud pattern-inheritance scoping
- Cluster 04 unlock decision

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
8. Next task = OPEN. Queue is EMPTY. CTO picks the next bounded lane. Cluster 04 is locked-lane excluded and NOT automatic. Prior `PRODUCT-ANCHOR-ADVANCE-01` is CLOSED — do not re-run.
9. Do NOT reopen any closed lane or prior phase
