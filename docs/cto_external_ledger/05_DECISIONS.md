# 05_DECISIONS.md — Decision Chain

## DEC-005 — Google Auth Hierarchy
- **Status:** ACTIVE (partially superseded by DEC-028 for credential model)
- **Date:** 2026-04-06
- OpenClaw built-in auth first; Jarvis browser OAuth fallback when needed
- No gog CLI for end users
- No client_secret files at user level

## DEC-026 — Jarvis Google Code Is Reference Only
- **Status:** ACTIVE
- **Date:** 2026-04-07
- Existing Jarvis V5 OS Google code is reference only until conformance proven
- Code existence is not architectural approval

## DEC-027 — OpenClaw-First Google Direction
- **Status:** SUPERSEDED by DEC-028
- **Date:** 2026-04-07
- Original direction: use OpenClaw GOG skill for Google Workspace auth
- Superseded because: OpenClaw has no native Google Workspace auth capability; GOG skill is just a wrapper around the gog CLI binary which requires Developer Console steps and client_secret.json

## DEC-028 — Fresh Jarvis V5 OS Browser-OAuth with AKIOR-Managed Credentials
- **Status:** ACTIVE
- **Date:** 2026-04-07
- Approved direction: fresh Jarvis V5 OS browser-OAuth implementation
- AKIOR-managed server-side credentials (clientId/clientSecret baked into server config, never exposed to user)
- Salvage safe UI/status/helper code only from existing implementation
- Delete credential input model (clientId/clientSecret form fields)
- No end-user secrets; no developer-console burden on user
- Browser auth only: click "Connect Google" → browser → sign in → done
- Server-side managed credentials
- Token storage must not be git-tracked
- google-tokens.json and refreshToken fields must be in .gitignore

## DEC-029 — Option A Selected for Product 1; Option B Deferred to Product 2
- **Status:** ACTIVE — narrows DEC-028 for Product 1 only, does NOT supersede DEC-028
- **Date:** 2026-04-07
- **Source:** CTO decision following G-T06.D1 amended artifact (G-T06.D1-AMEND-1)
- **Decision:** For AKIOR Full (Product 1) Google OAuth implementation, use Option A — personal-install OAuth app. Yosi (CEO) registers one Google Cloud OAuth app, one time, for his personal AKIOR Full install. clientId/clientSecret are provided to the CTO (Claude Chat side), who instructs the terminal engineer (Claude Code) to write data/google-credentials.json. End user click-to-connect flow remains as designed in DEC-028.
- **Reasoning:**
  1. Option A and Option B implementation code paths are identical. Only the credential provisioning step differs. Choosing A now does not lock out B later.
  2. Option B has two non-engineering open questions (Google Cloud project ownership, Google verification for sensitive scopes) that could block Product 1 progress on a Google review queue.
  3. Product 1 has never had a real Google account connect successfully. Getting to a working, testable connection is the immediate critical path.
  4. The Product 2 limitation must be tracked as an explicit blocker (BLK-004) so future planning does not assume inheritance.
- **Scope:** AKIOR Full (Product 1) only.
- **Explicit limitation:** Does NOT satisfy DEC-028 for AKIOR Light (Product 2) or Software 4 All (Product 3). See BLK-004.
- **Code change required:** None. The G-T06 refactor plan (Sections 1-6) is implementation-ready under DEC-029 without any additional code modification beyond what was already specified.

## DEC-030 — Execution Guardrail Lock
- **Status:** ACTIVE
- **Date:** 2026-04-08
- **Source:** CTO review of G-T06.I9-RESET conformance repair
- **Decision:** The terminal engineer (Claude Code) is authorized and required to refuse any task instruction that conflicts with the Non-Technical User Bible, DEC-028, DEC-029, or the approved SSOT. On conflict, the terminal engineer must STOP, report the conflict using the standard correction format (06_EXECUTION_GUARDRAILS.md), propose a Bible-aligned correction, and wait for CTO review. This applies especially to Google credential and OAuth flows, where the previous I9 instruction was rejected for pushing developer-console work onto the CEO.
- **Scope:** All future bounded tasks across all products.
- **Rationale:** The I9 rejection demonstrated that even CTO-issued instructions can accidentally violate locked product rules. A durable guardrail prevents recurrence by making Dev the last line of conformance defense.
- **Reference:** ~/akior/docs/cto_external_ledger/06_EXECUTION_GUARDRAILS.md, ~/projects/akior/forge/jarvis-v5-os/CLAUDE.md

## DEC-031 — Gmail via OpenClaw Browser-Session Lane (Product 1)
- **Status:** ACTIVE — operative for Gmail on AKIOR Full Product 1
- **Date:** 2026-04-08
- **Source:** CTO final call per guardrail lock section 6 after G-T06.OPENCLAW-BROWSER-GMAIL-SPIKE, G-T06.OPENCLAW-BROWSER-GMAIL-ACCEPTANCE, and G-T06.UI-CHANNELS-NAV all verified, plus CEO end-user UI acceptance confirmed visually.
- **Decision:** For AKIOR Full (Product 1) Gmail integration, the primary path is the OpenClaw managed browser session (profile at ~/.openclaw/browser/openclaw/user-data/) via the /api/browser/gmail/connect and /api/browser/gmail/status routes. The user clicks "Connect Google" in the Channels UI, the OpenClaw managed browser opens Gmail, and the existing persistent browser session is used. No API keys, no OAuth client, no Google Cloud Console, no client_secret, no terminal bootstrap.
- **Relation to DEC-028 and DEC-029:** DEC-028 and DEC-029 remain active for the OAuth-client architecture, which is retained in the codebase (I1 through I8 plus I4-AMEND-1) as a fallback lane. DEC-031 does NOT supersede DEC-028 or DEC-029; it implements DEC-005's "OpenClaw built-in auth first" clause, which was always ahead of the Jarvis OAuth fallback in the DEC-005 hierarchy. The OAuth lane is preserved as a safety net in case the browser-session lane ever breaks.
- **Scope:** Gmail only. Calendar, Drive, and Contacts are not covered by DEC-031 and remain separate future slices.
- **Proof:** G-T06.OPENCLAW-BROWSER-GMAIL-ACCEPTANCE Phase 1 (Gmail open with inbox title visible), Phase 2 (session persistence across browser restart), CEO visual UI confirmation via screenshot showing "Gmail Active" state, G-T06.UI-CHANNELS-NAV (sidebar entry present and functional).
- **Non-Technical Bible compliance:** Zero end-user secrets, zero Developer Console work, zero terminal commands, zero JSON file editing. The user's only action is clicking "Connect Google" in the Channels UI.

## DEC-032 — WhatsApp Product 1 via Direct Node Import of OpenClaw Core Functions
- **Status:** ACTIVE AND PROVEN — W-T04.IMPL end-to-end acceptance passed 2026-04-08
- **Proof evidence (2026-04-08):** W-T04.IMPL Phases 0-3 all verified. Phase 0 confirmed OpenClaw's internal cleanup path (resetActiveLogin, 3-minute TTL safety net). Phase 1 implemented backend state machine + three routes + glob resolver (zero new typecheck errors in apps/server). Phase 2 wired the AKIOR Channels UI card with five visual states (one-line typecheck patch in GoogleConnectCard during corrective step). Phase 3 baseline captured idle state, CEO performed real QR scan via AKIOR UI, card flipped to 'WhatsApp Connected' within scan window, creds.json persisted at 1840 bytes (818 session files total), openclaw channels list flipped from 'not linked, enabled' to 'linked, enabled'. CEO visual UI acceptance confirmed. No OpenClaw dashboard visible, no redirect, no iframe, no popup.
- **Date:** 2026-04-08
- **Source:** CTO final call after W-T03.6 server-side source inspection verified that OpenClaw ships startWebLoginWithQr and waitForWebLogin as importable ES modules at ~/.npm-global/lib/node_modules/openclaw/dist/login-qr-*.js. Live import test confirmed functions are directly callable from Node.
- **Decision:** For AKIOR Full (Product 1) WhatsApp integration, the primary path is direct Node import of OpenClaw's startWebLoginWithQr and waitForWebLogin functions from the AKIOR backend. The backend imports these functions, calls them in-process, receives qrDataUrl as a base64 PNG data URL, and proxies it to the Channels page UI. No WebSocket RPC client. No CLI subprocess. No OpenClaw dashboard redirect, iframe, or popup.
- **Relation to DEC-031 (Gmail):** DEC-031 and DEC-032 are SIBLINGS, not a supersession. Both implement DEC-005's "OpenClaw built-in auth first" clause. DEC-031 uses OpenClaw's managed browser gateway (port 18791) for Gmail. DEC-032 uses OpenClaw's importable core functions for WhatsApp. Different transports, same UX philosophy: proof-first, UI-first, AKIOR-native, Non-Technical Bible compliant.
- **Unifying principle:** OpenClaw distributes its core functionality as importable ES modules under ~/.npm-global/lib/node_modules/openclaw/dist/. Future Product 1 channel integrations should first check for an importable function path before considering WS-RPC, CLI spawn, or browser-session wrappers.
- **Scope:** WhatsApp only.
- **Risks acknowledged (to be addressed in W-T04):** (1) Hashed filenames change across OpenClaw versions. (2) Function contract with runtime/accountId parameters needs live pre-flight test. (3) Credential directory writes need permission verification. (4) Process lifetime and cleanup on crash/restart need documented contract.
- **Non-Technical Bible compliance:** CEO sees AKIOR only. Click "Connect WhatsApp" → QR renders in AKIOR card → scan with phone → card updates to Connected. No terminal, no dashboard, no redirect, no iframe, no popup, no secrets, no Developer Console.
- **Rejected alternatives:** WS-RPC client (simpler via direct import), CLI spawn (more reliable via direct import), dashboard redirect/iframe/popup (REJECTED — violates Non-Technical Bible).
- **Proof:** W-T04 link proven 2026-04-08. W-T05 send proven 2026-04-09 via Direction A (gateway RPC). Phase 4.12B HTTP 200, messageId `3EB0F0DD0CB207EF639E1C`, CEO physical phone receipt confirmed.

---

**Execution confirmation 2026-04-09:** Direction A (AKIOR → OpenClaw gateway RPC) is now proven end-to-end for WhatsApp Product 1. See 04_TEST_LOG.md Phase 4.12B entry for evidence. DEC-031 (Gmail via OpenClaw managed-browser lane) and DEC-032 (WhatsApp via direct Node import of OpenClaw core functions) remain active. DEC-028 remains active and DEC-027 remains superseded. Google track is unchanged.
