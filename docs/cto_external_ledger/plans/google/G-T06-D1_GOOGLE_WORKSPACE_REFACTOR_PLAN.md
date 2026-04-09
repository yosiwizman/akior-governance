# G-T06.D1 — Google Workspace Refactor Plan (Planning Only)

## 0. Provenance
- Date: 2026-04-09
- Product: AKIOR Full, Product 1
- Repo: `jarvis-v5-os`
- HEAD at planning time: `1ed4f45`
- Local-only chain: `1ed4f45` → `7f04059` → `3817ace` → `8f1b188`
- Planning phase: G-T06.D1
- Planning scope: Google Workspace only. WhatsApp lane is VERIFIED END-TO-END SUCCESS and is NOT touched by this plan.
- Governing decisions: DEC-028 active, DEC-029 active, DEC-030 active, DEC-027 superseded, DEC-031 active (Gmail managed-browser lane, proven), DEC-032 active (WhatsApp link + send, proven).
- Non-Technical Bible: binding on every CEO-visible step.

## 1. Locked Constraints (restated for the implementation phase)
- Approved Google direction: fresh Jarvis V5 OS browser-OAuth with AKIOR-managed server-side credentials (DEC-028).
- No end-user secrets.
- No developer-console burden on the CEO.
- No `client_secret.json` at user level.
- Browser auth only: CEO clicks "Connect Google" → browser → sign in → done.
- Server-side managed credentials only: `data/google-credentials.json` populated by CTO/terminal engineer during I9-OPS, never by the CEO.
- Three distinct non-product items that must NOT be collapsed:
  1. **OpenClaw GOG skill** — rejected on current stack (no Workspace auth capability). Not product architecture.
  2. **gog CLI** — forbidden (requires Developer Console + client_secret.json from end user). Not product architecture.
  3. **Claude Code MCP tools** (`mcp__claude_ai_Gmail__*`, `mcp__claude_ai_Google_Calendar__*`) — Anthropic-managed, not AKIOR product architecture. Not for use in product flows.
- End-user-held clientId/clientSecret — closed per DEC-028/DEC-029.
- WhatsApp lane preservation: `apps/server/src/whatsapp-send.ts` must remain byte-identical; `scopes: ["operator.write"]` preserved; `deviceIdentity: null` absent.

## 2. Google Surface Inventory (read-only evidence snapshot)

### Server-side code
| File | Role | Lines | Evidence |
|------|------|-------|----------|
| `apps/server/src/index.ts` | Main server: Google credential loader, OAuth flow routes, Gmail/Calendar API routes | 5561 | Lines 417,518-547,563-564,1485-1808,5518-5600 |
| `apps/server/src/clients/gmailClient.ts` | Gmail API client: token refresh, inbox, send, message read | 593 | Lines 80-110 (token refresh), 137-526 (API calls) |
| `apps/server/src/clients/googleCalendarClient.ts` | Calendar API client | 197 | Full file |
| `apps/server/.env.example` | Config template with GOOGLE_CLIENT_ID/SECRET placeholders | — | Lines 28-30 |
| `apps/server/.gitignore` | Excludes `data/google-credentials.json` and `data/google-tokens.json` | 4 | Lines 2-4 |

### Shared types
| File | Role | Lines | Evidence |
|------|------|-------|----------|
| `packages/shared/src/integrations.ts` | Integration config types — clientId/clientSecret as nullable fields (kept for Spotify/Alexa/Nest) | 391 | Lines 63-64 (Gmail), 84-85 (Calendar), 100-101 (Spotify) |
| `apps/server/src/utils/settingsContract.ts` | Zod schemas — Gmail/Calendar credential fields already removed by I1-I8 | 578 | Commit 331619e |

### UI
| File | Role | Lines | Evidence |
|------|------|-------|----------|
| `apps/web/app/settings/channels/page.tsx` | Channels UI: GoogleConnectCard (DEC-031 browser-session) | 298 | Lines 24-93 |
| `apps/web/app/settings/page.tsx` | Settings page: Spotify/Alexa/Nest clientId fields remain; Gmail/Calendar fields already removed by I1-I8 | 3804 | Lines 2536-3187 (Spotify/Alexa/Nest only) |

### Documentation (non-code)
| File | Role |
|------|------|
| `CLAUDE.md` | Repo execution contract — references `data/google-credentials.json` as server-side file |
| `docs/OAUTH_SETUP_GUIDE.md` | OAuth setup guide (pre-existing, may contain CEO-facing dev console instructions) |
| `JARVIS_V5_RELEASE_NOTES_v5.8.0.md` | Gmail integration release notes |
| `JARVIS_V5_RELEASE_NOTES_v5.9.0.md` | Calendar integration release notes |
| `AGENT_C_IMPLEMENTATION_PLAN.md` | Legacy planning doc with OAuth code snippets |

### Data (not committed, gitignored)
| Path | Status |
|------|--------|
| `apps/server/data/google-credentials.json` | DOES NOT EXIST (not yet provisioned) |
| `apps/server/data/google-tokens.json` | DOES NOT EXIST (no OAuth flow has completed) |

## 3. Classification

### 3.1 SALVAGE (keep as-is or with trivial changes)

| File | Reason | Evidence |
|------|--------|----------|
| `apps/server/src/index.ts` lines 527-547 (`loadGoogleCredentials`) | Already reads from `data/google-credentials.json` with env var fallback. Conforms to DEC-028 server-side model. | Phase 2c window |
| `apps/server/src/index.ts` lines 518-525 (`loadGoogleTokens`) | Reads from `data/google-tokens.json`. Token persistence path is correct. | Phase 2c window |
| `apps/server/src/index.ts` lines 5518-5600 (OAuth `/api/auth/google/*` routes) | Server-initiated OAuth redirect, code exchange, token storage. The flow itself is DEC-028 conformant — server holds credentials, CEO only clicks a button. | Phase 2f window |
| `apps/web/app/settings/channels/page.tsx` lines 24-93 (`GoogleConnectCard`) | DEC-031 browser-session card. Does NOT ask CEO for credentials. Connects via OpenClaw managed browser. Already proven. | Phase 2e window |
| `apps/server/.gitignore` lines 2-4 | Already excludes `data/google-tokens.json` and `data/google-credentials.json`. | Phase 2d |
| `apps/server/src/clients/gmailClient.ts` | Gmail API calls using server-side tokens. Does not ask CEO for credentials. | Phase 2b lines 80-526 |
| `apps/server/src/clients/googleCalendarClient.ts` | Calendar API calls. Same pattern as Gmail client. | Full file |
| `packages/shared/src/integrations.ts` | Nullable clientId/clientSecret fields still exist but are unused for Gmail/Calendar (removed from UI in I1-I8). Kept for Spotify/Alexa/Nest. No behavioral change needed. | Phase 2g |

### 3.2 DELETE (must be removed in later implementation phase)

| File/Location | Violation | Evidence |
|---------------|-----------|----------|
| `docs/OAUTH_SETUP_GUIDE.md` | If this doc instructs the CEO to open Google Cloud Console or create OAuth credentials, it violates the Non-Technical Bible. Must be deleted or rewritten as an internal CTO operations guide. | Path discovered in Phase 2a; content inspection deferred to implementation phase — ASSUMPTION: likely contains CEO-facing dev console instructions based on the filename pattern. Implementation phase must verify before deleting. |
| `apps/server/.env.example` lines 28-30 (`GOOGLE_CLIENT_ID=`, `GOOGLE_CLIENT_SECRET=`) | Suggests credentials come from environment variables set by the user. DEC-028 says credentials come from `data/google-credentials.json` managed by CTO. The env var fallback in `loadGoogleCredentials` at line 541-544 should be kept as an internal operations convenience but the `.env.example` should NOT surface it to the CEO. | Phase 2d, Phase 2c line 541-544 |

### 3.3 REWRITE (replace with DEC-028-conformant implementation in later phase)

| File/Location | Non-conformance | Required Invariant |
|---------------|----------------|-------------------|
| `apps/server/src/index.ts` lines 1485-1808 (Gmail + Calendar API route handlers) | These routes call `loadGoogleCredentials()` which throws 503 when `data/google-credentials.json` is missing. The routes themselves are structurally correct, but they have never been tested end-to-end because no credentials file exists yet. They will need integration testing once I9-OPS provisions credentials. | Must work end-to-end with real Google API tokens obtained via the OAuth flow. The 503 fallback when credentials are missing is correct behavior. |
| I9-OPS credential provisioning (NEW — does not exist yet) | No internal operational procedure exists to create the Google Cloud project, register the OAuth client, and write `data/google-credentials.json` server-side. | CTO/terminal engineer creates the Cloud project, registers OAuth client, extracts clientId+clientSecret, writes to `data/google-credentials.json`. CEO is NEVER involved. This is a new bounded operational task. |

### 3.4 LEAVE UNTOUCHED

| File | Reason |
|------|--------|
| `apps/server/src/whatsapp-send.ts` | WhatsApp lane. SHA256: `37edb08fd597c6be93eeb39032bfaf2dd949c69d1fb7d24a79616c28360a9f99`. Must remain byte-identical. |
| `apps/server/src/index.ts` WhatsApp routes (lines 460-506, 5329-5440, 5454+) | WhatsApp link + send routes. Not part of Google refactor. |
| `apps/web/app/settings/page.tsx` Spotify/Alexa/Nest sections | These integrations have their own clientId/clientSecret fields. Not part of Google refactor scope. |
| `e2e/auth.smoke.spec.ts` | Auth E2E test. Not Google-specific. |
| All governance/ledger files under `~/akior/docs/cto_external_ledger/` | Phase 4.13 already owns governance state. |

## 4. Credential Storage Design

**Path:** `apps/server/data/google-credentials.json` (per DEC-028, CLAUDE.md line 56)
**Gitignored:** YES — `apps/server/.gitignore` line 4 already excludes it.
**Ownership:** Server-process-owned, not world-readable.
**NOT created by the CEO.** Created by CTO/terminal engineer during I9-OPS (a separate bounded operational task).

**Contents:**
```json
{
  "clientId": "<AKIOR-managed OAuth client ID>",
  "clientSecret": "<AKIOR-managed OAuth client secret>",
  "redirectUri": "http://localhost:3002/api/auth/google/callback"
}
```
No per-user secrets. No refresh/access tokens. No CEO-supplied values.

**Loader:** `loadGoogleCredentials()` at `apps/server/src/index.ts:527-547` — SALVAGE. Already reads from the correct path with env var fallback. Returns `{ clientId, clientSecret, redirectUri }`. Throws typed error if missing.

**UI contract (Non-Technical Bible):**
1. CEO opens AKIOR → Settings → Channels
2. CEO clicks "Connect Google" button
3. Server-initiated OAuth redirect opens in system browser
4. CEO signs into their Google account and grants consent
5. Browser redirects back to AKIOR
6. Done. Zero terminal. Zero JSON. Zero API keys.

**Forbidden in credential design:**
- gog CLI (forbidden per DEC chain)
- OpenClaw GOG skill as Workspace auth (rejected on current stack)
- Claude Code MCP tools as product architecture (not product)
- End-user-held clientId/clientSecret (closed per DEC-028)
- CEO opening Google Cloud Console (Non-Technical Bible violation)
- CEO creating OAuth clients, handling client_secret.json, pasting credentials (all Non-Technical Bible violations)

## 5. Token Storage Design

**Path:** `apps/server/data/google-tokens.json` (per existing code at `index.ts:417`)
**Gitignored:** YES — `apps/server/.gitignore` line 3 already excludes it.
**NOT git-tracked.** No real tokens in any committed file.

**Contents (per existing `loadGoogleTokens` + OAuth callback code at index.ts:5553-5600):**
```json
{
  "access_token": "<short-lived>",
  "refresh_token": "<long-lived>",
  "token_type": "Bearer",
  "expiry_date": 1234567890000,
  "scope": "https://www.googleapis.com/auth/gmail.modify ...",
  "email": "ceo@example.com"
}
```

**Lifecycle:**
- Created by the OAuth callback handler (`/api/auth/google/callback`) after successful consent flow.
- `access_token` refreshed server-side by `gmailClient.ts:80-110` using credentials from Section 4 + stored `refresh_token`.
- If refresh fails (revoked/expired): token file invalidated, UI shows "Reconnect Google" (browser flow).
- Tokens never transmitted to browser, never logged, never in cookies/localStorage.
- No token persistence in env vars.

**Backward-compat:** The token file does not currently exist (no OAuth flow has completed). The REWRITE is the I9-OPS provisioning step + first real OAuth flow. Once credentials are provisioned and the CEO completes the browser flow, the token file will be created automatically by the existing callback handler.

## 6. UI / CEO Flow Plan

| Step | CEO Action | System Action | Bible Test |
|------|-----------|---------------|------------|
| 1 | Opens AKIOR in browser | — | PASS (clicking in browser) |
| 2 | Navigates to Settings → Channels | — | PASS (clicking in browser) |
| 3 | Clicks "Connect Google" button | Server reads `data/google-credentials.json`, builds OAuth URL, redirects browser to `accounts.google.com` | PASS (one click) |
| 4 | Signs into Google, clicks "Allow" | Google redirects to `/api/auth/google/callback` with auth code | PASS (browser sign-in) |
| 5 | Sees "Google Connected" status | Server exchanges code for tokens, stores in `data/google-tokens.json`, redirects to settings page | PASS (automatic) |

**Current DELETE items that violate this flow:**
- `docs/OAUTH_SETUP_GUIDE.md` — if it instructs CEO to open Google Cloud Console (ASSUMPTION; verify in implementation phase).
- `apps/server/.env.example` lines 28-30 — suggests CEO sets env vars. Should be rewritten as internal-ops-only documentation.

## 7. Implementation-Phase Preconditions

| # | Precondition | Status |
|---|-------------|--------|
| 1 | G-T06.D1 plan artifact committed to governance | TO BE ESTABLISHED (this phase produces it; CTO review gates it) |
| 2 | `data/google-credentials.json` provisioning accepted as I9-OPS | TO BE ESTABLISHED (CTO schedules I9-OPS as separate bounded task) |
| 3 | `.gitignore` excludes `data/google-credentials.json` and `data/google-tokens.json` | MET — `apps/server/.gitignore` lines 3-4 |
| 4 | All DELETE items verified to have zero incoming SALVAGE/REWRITE references | TO BE ESTABLISHED (implementation phase must verify before deleting) |
| 5 | All REWRITE items have named test plan | TO BE ESTABLISHED (implementation phase defines tests) |
| 6 | No CEO-facing step violates Non-Technical Bible 60-second test | MET (Section 6 flow passes all steps) |
| 7 | `apps/server/src/whatsapp-send.ts` sha256 = `37edb08fd597c6be93eeb39032bfaf2dd949c69d1fb7d24a79616c28360a9f99` | MET |
| 8 | HRM follow-up list (Section 8) is empty or has scheduled slots | SEE SECTION 8 |

## 8. Ambiguities and HRM Follow-Up

**The AMBIGUOUS list is EMPTY.** No non-obvious credential or token-storage tradeoff surfaced during this planning phase that requires HRM escalation.

Rationale: The credential storage design (Section 4) follows a single unambiguous path established by DEC-028/DEC-029: server-side `data/google-credentials.json` populated by CTO during I9-OPS, OAuth browser flow for the CEO, token persistence in `data/google-tokens.json`. The existing code already implements this pattern (SALVAGE items). The only new work is (a) the I9-OPS provisioning step (operational, not architectural) and (b) end-to-end integration testing once credentials exist. Neither involves a tradeoff requiring HRM.

## 9. Out-of-Scope for This Plan
- Any WhatsApp work (lane verified and closed, BLK-003 closed)
- Any Gmail DEC-031 managed-browser changes (already proven)
- Any Calendar / Drive / Contacts work not covered by the above sections
- I9-OPS internal credential provisioning (a later bounded CTO task, not implementation)
- Any schema change to the governance ledger (Phase 4.13 already owns it)
- Spotify / Alexa / Nest integration refactoring (separate scope)

## 10. Stop Gate
This is a planning artifact only. No implementation was performed. No code edits, no commits, no push, no OAuth flow, no Google connection, no token generation. Any implementation must be bounded into its own later phases (I1..In + I9-OPS + I10), each gated by the preconditions in Section 7. Section 8 is empty, so the CTO may proceed to implementation-phase preconditions review without an HRM detour.
