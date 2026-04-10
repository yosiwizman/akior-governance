# 01_LEDGER.md — Chronological Action Log
# Newest entries at top. Append new entries above the divider line.

---

## LB-018 — BASELINE-REMEDIATE-01: settings.json scope correction + active queue lock
- **Completed:** 2026-04-10
- **Product anchor:** a4a8206fed48fa67281f045ee965c472d0c2b7e7 (one corrective commit from 6195db4)
- **Corrective actions:**
  - Product commit: corrected apps/server/data/settings.json to contain ONLY the legitimate DEC-033 Gmail/GoogleCalendar credential-model removals; reverted all unintended schema-normalizer drift (alexa/irobot/nest/smartLights/avatar blocks added; elevenLabs/azureTTS/spotify field expansions; localLLM model change; webSearch field additions).
  - Governance commit: enforced the two-item active queue across 07_NEXT_ACTION.md, 08_HANDOFF.md, RESTORE_BLOCK.txt. Removed the out-of-scope third queue item.
- **Active queue (locked):**
  1. 3D-PRINT-QUICK-VERIFY-01 — bounded static verification of the 3D print integration against CEO-burden exit criteria
  2. GMAIL-READ-CAPABILITY-PLAN-01 — bounded written plan for minimal Gmail read capability on top of the proven session lane
- **WhatsApp:** Preserved byte-identical. whatsapp-send.ts untouched.
- **Google:** Still NOT solved as a product capability. Browser-only posture (DEC-033) active.
- **No new feature code.** Corrective-only task.

## LB-017 — BASELINE-COMMIT-01: DEC-033 purge + holomat cleanup committed
- **Completed:** 2026-04-10
- **Product anchor:** 6195db4a73442054f8452bf6d66947b63c0211ba
- **Action:** Committed DEC-033 credential-model purge and holomat cleanup/repair to jarvis-v5-os main as a single baseline commit (18 files: 8 deleted, 10 modified).
- **Lanes committed:**
  - DEC-033 Google credential-model purge (credential-file loading, googleapis routes, OAuth flow deleted)
  - Holomat cleanup (test-email-notifications page, EmailApp, CalendarApp deleted; holomat/page.tsx repaired; CLAUDE.md updated)
- **WhatsApp:** Preserved byte-identical. whatsapp-send.ts untouched. DEC-032 PROVEN status unchanged.
- **Gmail:** DEC-031 browser-session lane PROVEN at session level (GMAIL-VERIFY-01 + GMAIL-CONNECT-PROOF). CEO-facing surface hidden until minimal read capability exists.
- **3D print:** Pending quick verification (3D-PRINT-QUICK-VERIFY-01 queued).
- **Google:** NOT solved. Browser-only posture (DEC-033) active. No credential files. No off-system credential staging.
- **No new feature code.** This is a durability/cleanup commit only.

## LB-016 — DEC-033 Google Credential-Model Purge
- **Completed:** 2026-04-09
- **Source:** CEO directive — remove Google credential model completely
- **Decision added:** DEC-033
- **Actions taken:**
  - Deleted: gmailClient.ts, googleCalendarClient.ts, email-notifications.ts, OAUTH_SETUP_GUIDE.md, AGENT_C_IMPLEMENTATION_PLAN.md
  - Surgically edited: index.ts, settingsContract.ts, integrations.ts (shared), settings/page.tsx, settings.json, .env.example (×2), .gitignore
  - Removed operator artifact: ~/.akior/ops/OPS-CRED-01-HANDOFF.md
  - OPS-CRED-01 / I9-OPS marked ABANDONED
  - All governance active-state files updated to reflect DEC-033
- **Preserved:** Browser-session Gmail routes (/api/browser/gmail/*), browser-only UI shells
- **Posture:** Browser-only inside-system auth. No credential files. No off-system credential staging. No clientId/clientSecret entry flow.

## LB-015 — W-T05 Phase 4.5 audit recharacterized the blocker as device pairing at gateway handshake
- **Completed:** 2026-04-09
- **Repo under audit:** OpenClaw gateway runtime / local environment
- **Verdict:** AUDIT_COMPLETE
- **Summary:** Phase 4.5 performed a read-only OpenClaw pairing policy audit and established that the prior Phase 4 failure was NOT DM/contact pairing. The true blocker was device pairing at the WebSocket handshake level.
- **Accepted diagnosis:** Phase 4 failed before the RPC send method was actually dispatched. AKIOR's GatewayClient auto-loaded a device identity because `deviceIdentity` was not explicitly set. The gateway rejected that unpaired device during connection handshake and returned `pairing required`.
- **Evidence anchors:**
  - Device pairing gate: `gateway-cli-CWpalJNJ.js:26944` — `if (device && devicePublicKey && !skipPairing)`
  - Rejection path: `gateway-cli-CWpalJNJ.js:27048-27054` — emits NOT_PAIRED / "pairing required", closes with code 1008
  - GatewayClient auto-load: `method-scopes-DNlWj6m4.js:2030` — `deviceIdentity: opts.deviceIdentity === null ? void 0 : opts.deviceIdentity ?? loadOrCreateDeviceIdentity()`
- **Accepted fix direction:** Option A locked — set `deviceIdentity: null` so no device identity is sent and the pairing check is structurally skipped.
- **Rejected alternatives:** Option B (dmPolicy config change — wrong layer), Option C (inbound CEO message — wrong diagnosis), Option D (approve AKIOR as paired device — viable but unnecessary).
- **Guardrail note:** WhatsApp send remained NOT SOLVED after the audit. Phase 4.6 is next.

## LB-014 — W-T05 Phase 4 verification attempted one real send and failed cleanly
- **Completed:** 2026-04-08
- **Repo:** `~/projects/akior/forge/jarvis-v5-os` (main, commit 8f1b188)
- **Verification type:** One real send attempt
- **Verdict:** SEND_FAILED
- **Summary:** Phase 4 executed one real AKIOR-side send attempt to phone +\*\*\*8490 through the accepted WhatsApp route. The send did not deliver. The response was a gateway RPC failure with detail `pairing required`.
- **Observed result:** HTTP 502, error `gateway_rpc_error`, detail `pairing required`, response time ~77ms. No message delivered to phone.
- **What this proved:** The AKIOR route executed; the AKIOR-to-gateway path was live enough to return a semantic failure; WhatsApp send remained blocked.
- **Historical note:** At the time of Phase 4 review, the failure was initially interpreted as DM/contact pairing. That interpretation was later corrected by LB-015 (device pairing at gateway handshake).
- **Guardrail note:** Gmail smoke remained protected. No WhatsApp success was claimed. The workstream remained NOT SOLVED.

## LB-013 — W-T05 Phase 3 implementation committed and made ready for verification
- **Completed:** 2026-04-08
- **Repo:** `~/projects/akior/forge/jarvis-v5-os` (main, commit 8f1b188)
- **Verdict:** COMPLETE
- **Summary:** Phase 3 implemented Direction A on the AKIOR side by wiring WhatsApp send through the OpenClaw gateway RPC path rather than a local Baileys socket. The accepted implementation point is commit `8f1b188`.
- **What was established:** `POST /api/channels/whatsapp/send` exists on the AKIOR server side. Gateway runtime import path resolved. Verdict A import direction accepted: `openclaw/plugin-sdk/gateway-runtime` exposing `GatewayClient`. Helper implemented in `apps/server/src/whatsapp-send.ts`. Validation discipline passed. 5-error web baseline unchanged.
- **Evidence anchors:** Commit `8f1b188`, route `POST /api/channels/whatsapp/send`, helper `apps/server/src/whatsapp-send.ts`.
- **Notes:** This entry records implementation durability only. It does NOT mean WhatsApp send was proven end-to-end. End-to-end proof was deferred to Phase 4 verification.

## LB-012 — Remote Durability Pass Closure
- **Completed:** 2026-04-08
- **Action:** Both project repos brought under offsite version control on GitHub.
- **Scope:** 4-phase gated process — Phase 0 audit + verification spike, Phase 1 strategy, Phase 2 execution with intra-phase STOP gates, Phase 3 final verification.
- **jarvis-v5-os:** Pushed 10 local commits to github.com/yosiwizman/jarvis-v5-os via HTTPS. origin/main advanced 40f919b → 409e4ed. 4 hygiene-pass commits (331619e, 2d773bf, 87f53d6, 409e4ed) plus 6 pre-existing commits (85dc65a, 65b901c, 6005472, 568bf65, 35bb77e, 1a4e838). Post-push fetch confirmed 0 ahead / 0 behind.
- **akior-governance:** Initialized new private GitHub repo at github.com/yosiwizman/akior-governance. Pushed 2 commits (7b29480 initial snapshot, 70dbc62 LB-011 entry) via `git push -u origin main` to establish upstream tracking. Post-push fetch confirmed 0 ahead / 0 behind.
- **Auth:** HTTPS via `gh` credential helper. Initial Phase 2 Step 1 fetch failed with exit 128 ("could not read Username") because no credential helper was configured. Resolved by CEO running `gh auth login` (web browser flow). Phase 2 re-issued cleanly after credential helper verified.
- **Safety posture:** Every network operation wrapped with `set -o pipefail` + sed redaction pipeline to prevent credential leakage through transport-layer messages and to preserve the git exit status through the pipe. No force operations, no destructive ops, no merges, no rebases, no branch protection bypass attempts from Claude Code.
- **Governance note:** GitHub reported "Bypassed rule violations" on the jarvis-v5-os push — the yosiwizman admin account has bypass privilege on the repo's 6 required CI status checks on main. This is server-side permission, not a Claude Code action, but it means direct pushes to main from admin accounts skip CI validation on this repo. Recorded here for future decision on whether to tighten the bypass rule or accept it as the current trust model.
- **Status deltas:**
  - jarvis-v5-os filesystem-loss risk: RESOLVED (local commits + offsite remote).
  - Ledger filesystem-loss risk: RESOLVED (local commits + offsite remote).
  - "10 commits ahead of origin/main" on jarvis-v5-os: RESOLVED (0 ahead / 0 behind).
  - LB-011 drift: RESOLVED (committed locally in LB-011 commit, pushed to remote in this pass).
  - WhatsApp active message flow: latent partial, unchanged.
  - 5 pre-existing typecheck errors in jarvis-v5-os: scope-separate debt, unchanged.
  - New: admin branch protection bypass on jarvis-v5-os, noted but not remediated.
- **This is the first ledger update made under full version control.** Future ledger writes follow this workflow: edit 01_LEDGER.md, commit locally, push to origin/main on akior-governance.

## LB-011 — Workspace Hygiene Pass + Ledger Durability Pass
- **Completed:** 2026-04-08
- **Action:** Two scope items closed in one ledger entry.
- **Scope A — Workspace hygiene pass:** Captured the G-T06 Gmail chain and W-T01 through W-T04 WhatsApp chain into local git at ~/projects/akior/forge/jarvis-v5-os/ via a 4-phase gated process (Phase 0 audit, Phase 1 strategy, Phase 2 execution, Phase 3 verification). Four local commits created on main:
  - 331619e: G-T06 Gmail OAuth refactor (4 files, +3/-82). Refs DEC-005, DEC-026, DEC-028, DEC-029.
  - 2d773bf: WhatsApp DEC-032 + Gmail DEC-031 dual-chain (3 files, +605/-291). Refs DEC-005, DEC-031, DEC-032. Dual-chain files committed COMBINED (apps/server/src/index.ts, apps/web/app/settings/channels/page.tsx, apps/web/app/layout.tsx) because 554+ interleaved lines made hunk-split infeasible.
  - 87f53d6: DEC-030 CLAUDE.md governance (1 file, +71/-0). Refs DEC-030.
  - 409e4ed: Root .gitignore *.tar.gz guard (1 file, +2/-0) — prevents accidental commit of the 460MB Go 1.26.1 tarball sitting untracked in the repo root.
  Typecheck verdict: PASS. Server clean. Web 5 pre-existing errors unchanged (app/setup/page.tsx, JarvisAssistant.tsx, AkiorDemo.tsx, AkiorMicDemo.tsx x2). Zero new errors introduced. Pre-existing dirt left in working tree. 10 commits ahead of origin/main, unpushed.
- **Scope B — Ledger durability pass:** Initialized a dedicated git repo at ~/akior/ to version the canonical external ledger. Initial snapshot committed faithfully (12 canonical files + defensive .gitignore, no content edits). This LB-011 entry is the first normal ledger update under version control. Remote push deferred to a future task.
- **Status deltas:**
  - Uncommitted-chain filesystem-loss risk: RESOLVED (local commits on main).
  - Ledger durability risk: RESOLVED locally (~/akior/ is now a git repo). Remote push still OPEN as a future task.
  - WhatsApp message flow: latent partial, unchanged by this task.
- **No feature code changed by the hygiene pass or by this ledger entry.**
- **No remote push in either scope.**

## LB-010 — WhatsApp Slice Closure via W-T04.IMPL
- **Completed:** 2026-04-08
- **Action:** Recorded DEC-032 as PROVEN (end-to-end acceptance achieved). Updated 03_STATUS.md to reflect WhatsApp solved. Updated 06_BLOCKERS.md to mark BLK-001 and BLK-003 as RESOLVED. Recorded the W-T04.IMPL chain as the implementation that proved DEC-032.
- **Chain summary:**
  - W-T04.PREFLIGHT: throwaway script verified all four DEC-032 risks. Glob resolver found one candidate. startWebLoginWithQr({}) returned valid qrDataUrl in 801ms. Process exited cleanly.
  - W-T04.IMPL Phase 0: read-only source inspection identified cleanup contract (resetActiveLogin on success, force:true on cancel, 3-min TTL on timeout).
  - W-T04.IMPL Phase 1: backend state machine + three routes (connect, status, cancel) + glob resolver. Typecheck clean.
  - W-T04.IMPL Phase 2: UI card wired with five visual states. Phase 2 corrective ran first-ever web typecheck, found one new error (Boolean coercion fix). Phase 2 patch applied one-line fix.
  - W-T04.IMPL Phase 3: baseline captured, CEO scanned QR via AKIOR UI, card flipped to Connected, creds.json persisted at 1840 bytes (818 files), openclaw channels list confirmed "linked, enabled".
- **Slice result:** WhatsApp Product 1 SOLVED. BLK-001 and BLK-003 RESOLVED. DEC-032 PROVEN.
- **No feature code changed by this ledger task.**
- **No files outside ~/akior/docs/cto_external_ledger/ modified.**

## LB-009 — WhatsApp Discovery Chain Ledger Catch-up
- **Completed:** 2026-04-08
- **Action:** Recorded DEC-032 (WhatsApp via direct Node import). Updated 03_STATUS.md to reflect discovery chain complete and W-T04 queued. Updated 06_BLOCKERS.md to mark BLK-001 and BLK-003 as PROGRESSING. Recorded W-T01 through W-T03.6 as the discovery chain that produced DEC-032.
- **Chain summary:**
  - W-T01.MACMINI-WHATSAPP-DISCONNECT (2026-04-08): released the Mac Mini Baileys WhatsApp session via `openclaw channels logout --channel whatsapp` over SSH. Yahoo himalaya lane preserved. Phone's linked-device slot freed on the Mac Mini side.
  - W-T02.AIDESKTOP-WHATSAPP-AUDIT (2026-04-08): read-only audit confirmed OpenClaw installed and running on AI Desktop, WhatsApp channel registered, `channels list` falsely reporting "linked, enabled" due to stale Apr 6 Mac Mini credential copy (835 + 837 files in two directories). UI card present but partial (no connect logic, no status polling). Architectural note: WhatsApp uses Baileys channel pattern, NOT DEC-031 browser-session pattern.
  - W-T03.AIDESKTOP-WHATSAPP-GATEWAY-QR-DISCOVERY (2026-04-08): Phase 1 probed HTTP endpoints on port 18789, found no REST surface, confirmed WebSocket RPC architecture. Identified createWhatsAppLoginTool and startWebLoginWithQr as the QR generation path returning base64 PNG data URL. Phase 2 creds wipe blocked by Claude Code's destructive-guard.sh safety hook (working as designed). CEO manually executed the wipe in a real terminal: stale default/ and whatsapp/default/ directories removed, whatsapp-pairing.json removed. Post-wipe verification: `channels list` now honestly reports "not linked, enabled".
  - W-T03.5.OPENCLAW-SPA-RPC-DISCOVERY (2026-04-08, PARTIAL): attempted to extract WS-RPC invocation pattern from the OpenClaw SPA bundle at dist/control-ui/. SPA was heavily minified and discovery hit a wall. Claude Code recommended a fallback of redirecting the CEO into the OpenClaw dashboard SPA as a shortcut. **CTO rejected this recommendation** because it violates the Non-Technical Bible — routing the CEO into a second tool's dashboard (redirect, iframe, or popup) breaks the "AKIOR is the only app you see" rule. Drift event recorded: reaching for a UX shortcut when a technical discovery path hits a wall is a pattern future CTOs must watch for. The correct response to a wall is to find a different technical path, not to degrade the UX.
  - W-T03.6.OPENCLAW-SERVER-SOURCE-RPC-DISCOVERY (2026-04-08): re-pointed discovery at server-side Node source at dist/*.js (siblings of the minified dist/control-ui/). Server source was confirmed readable (non-minified, region comments preserved). Found login-qr-BPsUaGFE.js exporting startWebLoginWithQr and waitForWebLogin as importable functions. Live import test via `node -e "import(...)"` confirmed both functions are directly callable. This eliminated the need for WS-RPC client, CLI spawn, or any dashboard routing. Result: DEC-032 architectural direction locked.
- **CTO notes:** The W-T03.5 rejected recommendation is the single most important drift-prevention moment in this chain. The technically-clever shortcut (reuse the SPA that already works) was UX-wrong, and a future CTO instance reading only the happy-path summary might miss that. The rejection is recorded in full so it cannot be silently reversed. The W-T03.6 result is the inverse: a clean technical discovery that unlocks the simplest possible implementation path. Both events together prove that the right response to "the obvious path is minified" is "look at the server-side source," not "route the user somewhere else."
- **No feature code changed by this ledger task.**
- **No files outside ~/akior/docs/cto_external_ledger/ modified.**

## LB-005 — G-T06 Gmail Slice Closure
- **Completed:** 2026-04-08
- **Action:** Recorded DEC-031 as the operative decision for Gmail on Product 1. Updated 03_STATUS.md to reflect Gmail solved. Updated 06_BLOCKERS.md to mark BLK-002 resolved for Gmail only. Updated 07_NEXT_ACTION.md to reflect no queued G-T06 Gmail tasks.
- **Source:** CTO final call per guardrail lock section 6 after G-T06.OPENCLAW-BROWSER-GMAIL-SPIKE + G-T06.OPENCLAW-BROWSER-GMAIL-ACCEPTANCE + G-T06.UI-CHANNELS-NAV + CEO visual UI acceptance.
- **Chain summary:** G-T06.I1 through G-T06.I8 + G-T06.I4-AMEND-1 implemented the OAuth fallback lane (server-side refactor, UI credential field removal, shared type and schema cleanup). G-T06.OPENCLAW-BROWSER-GMAIL-SPIKE discovered and implemented the OpenClaw browser-session primary lane. G-T06.OPENCLAW-BROWSER-GMAIL-ACCEPTANCE proved the primary lane end-to-end. G-T06.UI-CHANNELS-NAV added the sidebar entry point.
- **CTO notes:** Significant portion of the chain was spent analyzing the OAuth lane as if it were the only option. DEC-005's "OpenClaw built-in auth first" clause was the operative permission from the start but was not cashed until the spike task. The OAuth-lane work (I1 through I8 plus amendment) is retained as a documented fallback and is not wasted. Two CTO drift events occurred during the chain (original I9 definition and Phase A Developer Console walkthrough); both were caught by the guardrail lock and retracted. The CEO's persistence in pushing back against the stuck CTO lane was what surfaced the correct path.
- **No feature code changed by this ledger task.**
- **No files outside ~/akior/docs/cto_external_ledger/ were modified.**

## LB-003 — Ledger Update for DEC-029 and BLK-004
- **Completed:** 2026-04-07
- **Action:** Recorded DEC-029 (CTO selection of Option A for Product 1) in 05_DECISIONS.md. Recorded BLK-004 (Option A does not scale to Product 2) in 06_BLOCKERS.md. Updated 03_STATUS.md to reflect both. Updated 07_NEXT_ACTION.md to reflect that G-T06.D1 is complete and G-T06 implementation breakdown is now the next task.
- **Source:** CTO re-review of G-T06.D1-AMEND-1 amended artifact
- **No feature code changed.**
- **No files outside ~/akior/docs/cto_external_ledger/ were modified.**

## LB-002 — Canonical Ledger Normalization
- **Completed:** 2026-04-07
- **Action:** Normalized all LB-001 placeholder files into real canonical format. Replaced stubs in 01_LEDGER.md, 02_ROADMAP.md, 03_STATUS.md, 04_TEST_LOG.md. Wrote full copy-paste-ready G-T06.D1 instruction block into 07_NEXT_ACTION.md. Updated 08_HANDOFF.md for consistency.
- **No feature code changed.**
- **No files outside ~/akior/docs/cto_external_ledger/ were modified.**

## LB-001 — Canonical External Ledger Bootstrap
- **Completed:** 2026-04-07T01:28:17Z
- **Action:** Created canonical external ledger directory at ~/akior/docs/cto_external_ledger/ and seeded 11 plain-name files with initial content per CTO specification.
- **No feature code changed.**
- **No files outside ~/akior/docs/cto_external_ledger/ were created or modified.**

---

**Note:** Pre-bootstrap historical entries (Batches A–F, compliance audits, etc.) have not been imported into this canonical ledger. They are intentionally omitted rather than fabricated. Historical truth lives in ~/LIVE_STATE.md, ~/EXECUTION_QUEUE.md, and git log. Future import may be done as a dedicated backfill task if needed.
