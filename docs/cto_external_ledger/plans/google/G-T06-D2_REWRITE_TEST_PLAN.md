# G-T06-D2 — REWRITE Test Plan

## Top Matter
- Date: 2026-04-09
- D1 source sha256: `ef66430f6886dda5f3139e6f21ecb4f19f873a05cbeb54e2f97a7044f63f35d8`
- REWRITE items covered: 2 (Gmail/Calendar route handlers integration testing; I9-OPS credential provisioning)
- Non-Technical Bible: binding on every CEO-visible step referenced in this test plan.
- Forbidden paths (three distinct non-product items):
  1. **gog CLI** — forbidden
  2. **OpenClaw GOG skill** — rejected on current stack
  3. **Claude Code MCP tools** — not AKIOR product architecture
- End-user-held clientId/clientSecret: closed per DEC-028/DEC-029.

---

## REWRITE-G-01: Gmail + Calendar API Route Handlers Integration Testing

1. **name:** REWRITE-G-01
2. **file:** `apps/server/src/index.ts` lines 1485-1808 (Gmail routes: test, inbox, message, send; Calendar routes: sync-events, sync-reminders, test)
3. **invariant:** All Gmail/Calendar API routes must use only server-side credentials from `loadGoogleCredentials()` reading `data/google-credentials.json`, never from user input. (DEC-028, DEC-029, Non-Technical Bible)
4. **preconditions:**
   - `data/google-credentials.json` provisioned server-side via I9-OPS
   - `.gitignore` confirms `data/google-credentials.json` and `data/google-tokens.json` excluded
   - No CEO-facing credential entry fields remain in UI for Gmail/Calendar (verified by I1-I8 chain, commit 331619e)
5. **stub fixtures:** FAKE credentials file `test/fixtures/fake-google-credentials.json` with `clientId: "FAKE_CLIENT_ID_FOR_TESTING"`, `clientSecret: "FAKE_SECRET_FOR_TESTING"`. Clearly labeled FAKE. Must NOT be real secrets. Implementation phase creates these; this phase only names them.
6. **unit-level assertions:**
   - `loadGoogleCredentials()` throws a typed error when `data/google-credentials.json` is missing
   - `loadGoogleCredentials()` returns `{ clientId, clientSecret, redirectUri }` when file exists and is valid
   - `loadGoogleCredentials()` does NOT read from any user-facing input, request body, or query parameter
   - `loadGoogleTokens()` returns null when `data/google-tokens.json` is missing (not an error)
   - Token loader never logs token contents (grep test: no `console.log` of `access_token` or `refresh_token`)
   - Token loader redacts tokens in error messages
7. **integration-level assertions (without real Google connection):**
   - OAuth start route (`/api/auth/google/start`) constructs a valid Google consent URL using only server-side credentials
   - OAuth start route returns 503 with typed error when credentials file missing
   - OAuth callback route (`/api/auth/google/callback`) returns 503 when credentials file missing
   - Gmail inbox route (`/api/integrations/gmail/inbox`) returns 503 when credentials missing
   - Gmail send route (`/api/integrations/gmail/send`) returns 503 when credentials missing
   - All routes that call `loadGoogleCredentials()` handle the typed error and return 503 (not 500)
8. **non-conformance tripwires (must FAIL):**
   - Any path that reads a clientSecret from request body
   - Any path that reads clientId from a user-facing form field
   - Any path that writes a token file outside `data/` directory
   - Any path that logs a token value to stdout or a log file
   - Any path that exposes clientSecret in an HTTP response body
9. **out-of-scope:**
   - Real end-to-end Google OAuth (requires live credentials + CEO consent flow)
   - Calendar/Drive/Contacts beyond the route-level 503 check
   - I9-OPS provisioning itself
   - WhatsApp routes (preserved, not tested here)
10. **verdict gate:** All unit assertions pass + all integration assertions pass + all tripwires trip on non-conformant code + no real secret present in any fixture file.

---

## REWRITE-G-02: I9-OPS Credential Provisioning Verification

1. **name:** REWRITE-G-02
2. **file:** `data/google-credentials.json` (does not exist yet; created by I9-OPS)
3. **invariant:** After I9-OPS, a valid `data/google-credentials.json` must exist server-side with `clientId` and `clientSecret`, readable only by the server process user, excluded from git. (DEC-028)
4. **preconditions:**
   - I9-OPS has been executed by CTO/terminal engineer
   - `.gitignore` excludes `data/google-credentials.json`
5. **stub fixtures:** None for this REWRITE item; it verifies a real provisioned file.
6. **unit-level assertions:**
   - `data/google-credentials.json` exists and has `size > 10` bytes
   - JSON parses without error
   - Contains `clientId` as a non-empty string
   - Contains `clientSecret` as a non-empty string
   - File permissions are `600` or `640` (not world-readable)
   - `git status --short data/google-credentials.json` returns empty (gitignored, not tracked)
7. **integration-level assertions:**
   - `loadGoogleCredentials()` returns valid `{ clientId, clientSecret, redirectUri }` when called server-side
   - The returned `redirectUri` matches the expected OAuth callback path
8. **non-conformance tripwires:**
   - File is world-readable (permissions `644` or broader)
   - File contains a `refresh_token` or `access_token` (should be credentials only, not tokens)
   - File is tracked by git (appears in `git ls-files`)
9. **out-of-scope:**
   - The OAuth flow itself (separate implementation phase)
   - Token persistence (separate REWRITE-G-01 concern)
10. **verdict gate:** File exists + JSON valid + fields present + permissions correct + gitignored + `loadGoogleCredentials()` returns valid object.
