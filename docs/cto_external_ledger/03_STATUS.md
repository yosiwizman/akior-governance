# 03_STATUS.md — Current Project Status

- **Anchor date:** 2026-04-15
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
- **Product anchor:** `fa3c4ffd9e85d1e0c237d7d443baf2228d54ed66` (GOOGLE-CONTACTS-MINIMAL-LIST-CANONICAL-SLICE-01, merge commit for PR #133, 2026-04-15). Latest closure in the Google Workspace bundle chain. Prior anchor `3d81f2c5ff3d6b53349ec0ae45c61be9ba226d10` (PR #121, non-channel rate-limit Cluster 03). Chain extended through PRs #122–#133 (Gmail inbox auth + channels-auth cluster + Calendar descriptor + Calendar events read + Calendar signed-in predicate fix + Drive descriptor + Contacts descriptor + Drive minimal browse + Contacts minimal list). Cluster 04 (3D-print alert #54) remains locked-lane excluded and is the sole remaining carryover of the non-channel rate-limit lane.
- **Google Workspace browser-first bundle:** **CLOSED as a suite** (GOOGLE-WORKSPACE-BUNDLE-CLOSURE-AUDIT-01, 2026-04-15, verdict CLOSED). All four providers proven end-to-end on the managed-browser-gateway lane: Gmail live read, Google Calendar live read, Google Drive minimal browse, Google Contacts minimal list. Every admin-guarded read endpoint returns HTTP 200 with a truthful envelope; `/api/channels/counts` unauth carve-out returns HTTP 200 with all 6 category keys; auth guards return 401 unauth on every protected read; all 4 settings pages render without errors. Known non-blocking note: the `[data-id]` CDP selector used by Drive and Contacts picks up some Google WIZ-data script entries alongside truthful rows — carved-out future scope, not an active blocker.
- **Next bounded task:** OPEN — CTO chooses. Queue is EMPTY. There is no automatic next step. The Google Workspace bundle is closed; Cluster 04 (3D-print, alert #54) remains locked-lane excluded and is NOT the automatic next task. Any future Google write/compose/send/upload/delete/share/search/preview slice must use browser-only inside-system auth per DEC-033 (no credential entry, no credential files).

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
- Google Calendar live read: CLOSED end-to-end (PR #127 + fix PR #129). GET /api/channels/google-calendar/events returns HTTP 200 with real events against live calendar.google.com tab.
- Google Drive minimal browse: CLOSED end-to-end (PR #132). GET /api/channels/google-drive/files returns HTTP 200 with real folder/file entries against live drive.google.com tab.
- Google Contacts minimal list: CLOSED end-to-end (PR #133). GET /api/channels/google-contacts/contacts returns HTTP 200 with truthful envelope against live contacts.google.com tab.
- Google Workspace browser-first bundle: operationally CLOSED as a suite.
- WhatsApp: OpenClaw direct-import lane (DEC-032), verified end-to-end via W-T04.IMPL, creds persisted, card shows Connected in AKIOR Channels UI

## What Does Not Work Yet
- Google write/compose/send/upload/delete/share/search/preview (any provider): not implemented; carved-out future scope
- Drive/Contacts `[data-id]` CDP selector fidelity refinement: carved-out future scope (not an active blocker — endpoints return truthful envelopes)
- iMessage: not attempted
