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
- **Product anchor:** `3931498d99b3b7ca0e6fedb3e549e1b570b32a74` (CODEQL-NON-CHANNELS-RATE-LIMITING-CLUSTER-02-EXTERNAL-API, merge commit for PR #120, 2026-04-14). Six js/missing-rate-limiting alerts retired on live main (state=fixed, fixed_at=2026-04-14T17:54:14Z): #58 `index.ts:1064` (ollama/models), #60 `index.ts:1263` (elevenlabs/tts), #61 `index.ts:1326` (azure-tts/tts), #62 `index.ts:1389` (spotify/search), #65 `index.ts:1512` (weather/query), #66 `index.ts:2169` (openai/text-chat). Bounded edits in `apps/server/src/index.ts` only (+84/-0). Prior anchor `76aa4fbdae34454114479a6a3e39aa38847d05c5` retained the Cluster 01 LLM/Auth retirement. Chain extends with `8b6edb2` → `3931498` (PR #120 merge) → LOCKED. **This closes Cluster 02 only (external-API) of the non-channel rate-limit lane. Cluster 01 (LLM/Auth, 4 alerts) and Cluster 02 (external-API, 6 alerts) are now CLOSED — 10 of 20 non-channel alerts retired. Cluster 03 (user-data CRUD, 9 alerts) remains carryover. Cluster 04 (3D-print, 1 alert) is locked-lane and excluded.**
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
