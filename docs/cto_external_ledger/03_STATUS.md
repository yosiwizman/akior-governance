# 03_STATUS.md — Current Project Status

- **Anchor date:** 2026-04-13
- **Active product:** AKIOR Full (Product 1)
- **Current batch:** Batch G — Channels, Skills & Infrastructure
- **3D print lane:** CLOSED. VERIFIED in 3D-PRINT-STRICT-CLOSURE-01 — Playwright 5/5 green on e2e/3dprinters.smoke.spec.ts. No reopening.
- **Gmail read lane:** CLOSED end-to-end. VERIFIED in GMAIL-READ-INBOX-03-E2E-AUTH-BOOTSTRAP — Playwright 5/5 green on e2e/gmail-inbox.smoke.spec.ts. All four endpoints (/status, /accounts, /inbox-summary, /inbox) returned HTTP 200. Auth wall preserved: middleware redirects unauthenticated /settings/** → /login. DEC-033 clean (no client_secret, no google-credentials.json, no refresh_token revival, no off-system credential artifacts). E2E auth bootstrap (env-gated) live in-repo — see 05_DECISIONS.md DEC-034.
- **WhatsApp Product 1 status:** UNCHANGED.
  - **Link:** SOLVED via DEC-032 (QR link proven 2026-04-08, creds persisted).
  - **Send:** VERIFIED END-TO-END SUCCESS (Phase 4.12B, 2026-04-09). HTTP 200, messageId `3EB0F0DD0CB207EF639E1C`, unique test string `AKIOR-W-T05-P412B-20260409T174414Z-g7U31x` physically received on CEO phone. Gateway remained active post-send. Listener remained connected. BLK-003 CLOSED.
- **Active channel decisions:** DEC-031 (Gmail browser-session), DEC-032 (WhatsApp link + send, PROVEN), DEC-033 (Google credential-model purge, browser-only posture), DEC-034 (E2E auth bootstrap, env-gated)
- **Superseded:** DEC-027, DEC-028, DEC-029
- **Ledger:** LB-001 → ... → LB-012 → LB-013 (Phase 3) → LB-014 (Phase 4 SEND_FAILED) → LB-015 (Phase 4.5 audit) → Phase 4.12B (VERIFIED SUCCESS)
- **OPS-CRED-01 / I9-OPS:** ABANDONED / SUPERSEDED by DEC-033. No off-system credential artifacts. No credential-provisioning tracks active.
- **Hard constraints (inviolable):** no client_secret, no google-credentials.json, no refresh_token revival, no off-system credential staging, no CEO developer-console burden. Normal auth wall intact.
- **Product anchor:** `3d81f2c5ff3d6b53349ec0ae45c61be9ba226d10` (CODEQL-NON-CHANNELS-RATE-LIMITING-CLUSTER-03-USER-DATA-CRUD, merge commit for PR #121, 2026-04-14). Nine js/missing-rate-limiting alerts retired on live main (state=fixed, fixed_at=2026-04-14T18:59:27Z): #59 `index.ts:1135` (GET /api/memory), #63 `index.ts:1508` (GET /api/settings), #64 `index.ts:1550` (POST /api/settings), #67 `index.ts:2985` (GET /api/file-library), #68 `index.ts:2987` (DELETE /api/file-library/:name), #69 `index.ts:3544` (GET /api/images), #70 `index.ts:4648` (GET /api/tasks), #71 `index.ts:4668` (GET /api/dnd), #72 `index.ts:4767` (GET /api/contacts). Single-file bounded edit in `apps/server/src/index.ts`. Two commits in PR #121: `c7f2bf2` initial preludes, then `db04efa` in-PR bucket fixup after the first CI E2E run hit a 429 cascade on polled GET endpoints — READ buckets raised to inline `{maxAttempts:600, windowMs:60_000, lockoutMs:60_000}`; mutations kept `ADMIN_MODERATE`. Second CI run: 13/13 green. Prior anchor `3931498d99b3b7ca0e6fedb3e549e1b570b32a74` retained the Cluster 02 external-API retirement. Chain extends with `c7f2bf2` → `db04efa` → `3d81f2c` (PR #121 merge) → LOCKED. **Cumulative non-channel rate-limit lane status: 19 of 20 alerts retired (Cluster 01: 4, Cluster 02: 6, Cluster 03: 9). Cluster 04 (3D-print alert #54, 1 alert) is locked-lane excluded and is the sole remaining carryover.**
- **Canonical Gmail slice proof:** GMAIL-INBOX-LIST-CANONICAL-E2E-01 CLOSED 2026-04-14 on the SAME unchanged product SHA `9adc7fb…` (NO product mutation). Live proof: OpenClaw gateway `running:true`, live Gmail tab at `mail.google.com` signed in as `yosiwizman5638@gmail.com`, `/api/channels/gmail/{accounts,status,inbox-summary,inbox}` all HTTP 200 with live payloads (unread=12, rowCount=62), UI connected-state rendered with populated inbox list, Playwright chromium smoke 5/5 passed. Capability-factory gate proven for one canonical Google/Gmail read slice on the browser-first / inside-system architecture. Calendar/Drive/Contacts NOT implemented — they remain future clone candidates.
- **Next major step:** Google Calendar, Drive, Contacts (future slices). All must use browser-only inside-system auth (DEC-033). No credential entry. No credential files.

## What Works
- Chat text: Verified PASS (local qwen2.5:72b)
- Voice: Verified PASS (OpenAI Realtime WebRTC)
- UI: 12/12 smoke test PASS
- Contacts CRUD: Verified PASS
- DND toggle: Verified PASS
- Scheduled Tasks: Verified PASS
- Yahoo Mail: Connected via himalaya
- GPU roles: v2.7 compliant
- /mnt/models data plane: partially initialized (dirs created, HF cache redirected)
- HRM: operationally ready on GPU 1 (venv healthy, flash_attn working)
- Canonical external ledger: LB-001 → LB-002 → LB-003 → LB-005 → LB-009 → LB-010
- Gmail read: CLOSED end-to-end (GMAIL-READ-INBOX-03-E2E-AUTH-BOOTSTRAP). Playwright 5/5 green on e2e/gmail-inbox.smoke.spec.ts. Endpoints /status /accounts /inbox-summary /inbox all HTTP 200. Auth wall intact. DEC-034 (env-gated E2E auth bootstrap) live in-repo.
- WhatsApp: OpenClaw direct-import lane (DEC-032), verified end-to-end via W-T04.IMPL, creds persisted, card shows Connected in AKIOR Channels UI

## What Does Not Work Yet
- Google Calendar, Drive, Contacts: not yet wired (separate future slices)
- iMessage: not attempted
