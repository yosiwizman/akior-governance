# G-T06-D2 — I9-OPS Task Definition

## 1. Scope
I9-OPS is an internal AKIOR operations task executed by the CTO / terminal engineer. It provisions server-side Google OAuth credentials for AKIOR Full Product 1 under DEC-028. It is NOT a CEO-facing task. It is NOT an implementation phase of Product 1. It is a separate bounded internal task.

## 2. Non-Technical Bible Binding
The CEO does not participate in I9-OPS. The CEO does not see any Developer Console, OAuth client, clientId, clientSecret, or JSON file as part of I9-OPS. If any step of I9-OPS would land on the CEO, the step is out of scope and must be rewritten.

## 3. Forbidden Paths
Three distinct non-product items that must NOT be collapsed:
1. **gog CLI** — forbidden (requires Developer Console + client_secret.json from end user)
2. **OpenClaw GOG skill** — rejected on current stack (no Workspace auth capability)
3. **Claude Code MCP tools** (`mcp__claude_ai_Gmail__*`, etc.) — not AKIOR product architecture

Additional forbidden:
- End-user-held clientId/clientSecret (closed per DEC-028/DEC-029)
- CEO opening Google Cloud Console (Non-Technical Bible violation)
- CEO creating/selecting a Google Cloud project (violation)
- CEO enabling Google APIs (violation)
- CEO configuring an OAuth consent screen (violation)
- CEO creating OAuth client credentials (violation)
- CEO copying/pasting clientId or clientSecret (violation)
- CEO handling `client_secret.json` or any credential file (violation)
- CEO writing/editing `data/google-credentials.json` (violation)
- CEO running terminal commands for Google setup (violation)
- CEO needing to know what OAuth is (violation)

## 4. Inputs (internal to CTO/ops)
- Access to an AKIOR-owned Google Cloud project (ownership is AKIOR-internal, CEO never touches it)
- Authority to create an OAuth client in that project
- Access to the server host's `data/` directory
- Secure channel to transmit the credentials file to the server host (e.g. direct write via Claude Code on the AI Desktop)

## 5. Outputs (on server host only)
- Populated `data/google-credentials.json` containing `clientId` and `clientSecret` (no per-user secrets, no tokens)
- Correct file permissions: readable only by the server process user (600 or 640)
- Verified `.gitignore` exclusion (already confirmed: `apps/server/.gitignore` line 4)
- Written confirmation entry in `04_TEST_LOG.md` stating I9-OPS completed on date X

## 6. Preconditions Before I9-OPS May Be Scheduled
- DEC-028 active
- DEC-029 active
- DEC-030 active
- G-T06.D1 planning artifact durable in governance
- G-T06.D2 REWRITE test plan durable in governance
- `.gitignore` audit confirms `data/google-credentials.json` and `data/google-tokens.json` excluded (CONFIRMED: `apps/server/.gitignore` lines 3-4)
- No CEO-facing credential entry UI remains for Gmail/Calendar (CONFIRMED: I1-I8 chain, commit 331619e removed those fields)

## 7. Postconditions Before the First Google Implementation Slice
- I9-OPS confirmation entry exists in `04_TEST_LOG.md`
- `loadGoogleCredentials()` can be invoked server-side and returns a valid `GoogleCredentials` object in a read-only smoke test (does not call Google)
- G-T06.D2 verdict is `PRECONDITIONS_CLOSED_READY_FOR_IMPLEMENTATION` (or equivalent)

## 8. Explicit Non-Goals
- I9-OPS does NOT start an OAuth flow
- I9-OPS does NOT obtain a refresh token
- I9-OPS does NOT open any browser for consent
- I9-OPS does NOT test against Google
- I9-OPS is credential provisioning only
- The first real Google connection happens in a later bounded implementation phase, not in I9-OPS

## 9. Scheduling
I9-OPS is SCHEDULED AS A SEPARATE BOUNDED INTERNAL CTO TASK. It is not this phase. It is not the next implementation slice. It is a sibling bounded task. This scheduling statement alone closes Precondition #2 for G-T06.D2, provided this artifact is durable in governance.

## 10. Stop Gate
I9-OPS must NOT be executed as part of G-T06.D2 or any planning phase. If any phase accidentally enters an I9-OPS step, STOP.
