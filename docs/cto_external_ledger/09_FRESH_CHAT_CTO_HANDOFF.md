# 09_FRESH_CHAT_CTO_HANDOFF.md
# Last updated: 2026-04-15 (Google Workspace browser-first bundle CLOSED as a suite — GOOGLE-WORKSPACE-BUNDLE-CLOSURE-AUDIT-01)
# Audience: a fresh AKIOR CTO chat session resuming work mid-stream.
# Style: crisp, evidence-based, no hype. No soft summaries.

---

## 1. Purpose

Single source of truth for a brand-new CTO chat to resume work without re-reading the full ledger. Every claim below is anchored to a merged commit, a live CodeQL alert state, or a tracked governance file. Anything not anchored is labelled ASSUMED / NOT PROVEN.

**Product inheritance rule (non-negotiable):**
- AKIOR Full is built and proven first.
- AKIOR Light and AKIOR Cloud inherit only from proven Full patterns — never the reverse.
- A proven pattern = merged to product main + live evidence (CodeQL fixed / Playwright green / live HTTP receipts), not "exists in the repo".

---

## 2. Current locked baseline (verified on tracked main)

- **Product main SHA:** `fa3c4ffd9e85d1e0c237d7d443baf2228d54ed66`
- **Product tag:** locked by GOOGLE-CONTACTS-MINIMAL-LIST-CANONICAL-SLICE-01 (PR #133 squash-merge, 2026-04-15 — final slice of the Google Workspace browser-first bundle).
- **Bundle status:** Google Workspace browser-first bundle is operationally **CLOSED as a suite** (GOOGLE-WORKSPACE-BUNDLE-CLOSURE-AUDIT-01, 2026-04-15, verdict CLOSED).
- **Governance main SHA:** synced this session via GOV-GOOGLE-BUNDLE-CLOSURE-SYNC-01.
- **Prior baseline:** Cluster 03 closure PR #121 → `3d81f2c…` (2026-04-14). Chain since: #122 (GMAIL-INBOX-AUTH-GAP) → #123–#125 (channels-auth cluster) → #126 (Calendar descriptor + shared `ensureGatewayDefaultAccount` parameterization) → #127 (Calendar events read) → #129 (Calendar `title-not-signin` predicate fix) → #130 (Drive descriptor) → #131 (Contacts descriptor) → #132 (Drive minimal browse) → #133 (Contacts minimal list).
- Governance follow-through for the Google bundle is **CURRENT** on tracked main. If on session start you observe a newer product SHA that does not match `fa3c4ff…`, re-verify §2 before acting.
- Repos: product `yosiwizman/jarvis-v5-os`, governance `yosiwizman/akior-governance`.

---

## 3. Accepted current reality

- Product anchor advanced from `3d81f2c…` (PR #121, Cluster 03 closure) → `fa3c4ffd9e85d1e0c237d7d443baf2228d54ed66` (PR #133, Google Contacts minimal list).
- **Google Workspace browser-first bundle is CLOSED as a suite** (GOOGLE-WORKSPACE-BUNDLE-CLOSURE-AUDIT-01, 2026-04-15). All four providers (Gmail, Calendar, Drive, Contacts) proven end-to-end on the managed-browser-gateway lane.
- Cluster 03 (user-data CRUD, 9 alerts) remains **CLOSED in product** and **RECORDED in governance** from PR #121. No lane has reopened.
- Governance has ratified the bundle closure in this session's `GOV-GOOGLE-BUNDLE-CLOSURE-SYNC-01` governance sync.

---

## 4. What is CLOSED (proven)

| Lane | Anchor PR | Evidence |
|---|---|---|
| Channels CodeQL hardening (rate-limit + URL-host + path-injection) | PR #116 → `508dabf` | governance ratified |
| Channels path-injection alerts #6 + #7 | PR #117 → `7654a9a` | live CodeQL `state=fixed` |
| Channels insecure-randomness alert #1 (`accountsIndex.ts:179`) | PR #118 → `9adc7fb` | live CodeQL `state=fixed`, fixed_at=2026-04-14T11:52:23Z |
| Gmail canonical inbox-list E2E (`GMAIL-INBOX-LIST-CANONICAL-E2E-01`) | proof task, anchor RETAINED at `9adc7fb` | OpenClaw gateway running, signed-in Gmail tab, /api/channels/gmail/{accounts,status,inbox-summary,inbox} all 200 with live payloads, Playwright chromium 5/5 |
| Cluster 01 LLM/Auth rate-limit (#53/#55/#56/#57) | PR #119 → `76aa4fb` | live CodeQL `state=fixed`, fixed_at=2026-04-14T15:58:44Z |
| Cluster 02 external-API rate-limit (#58/#60/#61/#62/#65/#66) | PR #120 → `3931498` | live CodeQL `state=fixed`, fixed_at=2026-04-14T17:54:14Z |
| **Cluster 03 user-data CRUD rate-limit (#59/#63/#64/#67/#68/#69/#70/#71/#72)** | **PR #121 → `3d81f2c`** | **live CodeQL `state=fixed`, fixed_at=2026-04-14T18:59:27Z** |
| GMAIL-INBOX-AUTH-GAP-01 (requireAdmin on `/api/channels/gmail/inbox*`) | PR #122 | local admin-guard receipts |
| CHANNELS-AUTH-CLUSTER (parameterized `:providerId/{accounts,status,connect,disconnect}` guarded) | PR #123–#125 | audit + local receipts |
| GOOGLE-CALENDAR-DESCRIPTOR-REGISTRATION (connect-only, new `"calendar"` category) | PR #126 → `318076a` | live `channelState:"connected"` receipts |
| GOOGLE-CALENDAR-READ-EVENTS (`/api/channels/google-calendar/events`) | PR #127 → `aac9366` | live 10-event HTTP 200 receipt |
| GOOGLE-CALENDAR-SIGNEDIN-PREDICATE-FIX (one-line `title-not-signin`) | PR #129 → `59666ec` | live connected status + events |
| GOOGLE-DRIVE-DESCRIPTOR-REGISTRATION (connect-only, new `"files"` category) | PR #130 → `c8649e4` | live `channelState:"connected"` |
| GOOGLE-CONTACTS-DESCRIPTOR-REGISTRATION (connect-only, new `"contacts"` category) | PR #131 → `4fc6bde` | live `channelState:"connected"` |
| GOOGLE-DRIVE-MINIMAL-BROWSE (`/api/channels/google-drive/files`) | PR #132 → `fffe29d` | live 10-file HTTP 200 (includes real folder "drone pilot school") |
| **GOOGLE-CONTACTS-MINIMAL-LIST (`/api/channels/google-contacts/contacts`)** | **PR #133 → `fa3c4ff`** | **live HTTP 200 truthful envelope** |
| **GOOGLE-WORKSPACE-BUNDLE-CLOSURE-AUDIT-01** | **read-only audit, anchor RETAINED at `fa3c4ff`** | **bundle CLOSED as a suite; 12 curl probes + 4 settings-page screenshots** |

**Cumulative non-channel rate-limit lane status:** **19 / 20 alerts retired.** Remaining: 1 (Cluster 04, locked-lane excluded — see §5).
**Google Workspace browser-first bundle:** **4/4 providers CLOSED as a suite.**

---

## 5. What is NOT solved / not proven / excluded

### Excluded by policy (locked lane)
- **Cluster 04 — 3D-print rate-limit alert** (`apps/server/src/routes/3dprint.routes.ts:403`, alert #54). Locked-lane excluded. The 3D-print lane is CLOSED/LOCKED per RESTORE_BLOCK.txt and must not be reopened to add a limiter without an **explicit new CTO decision** to unlock. Cluster 04 is the sole remaining carryover of the non-channel rate-limit lane. It is **NOT** the automatic next implementation task.

### Not proven / future work
- **AKIOR Light / AKIOR Cloud:** no proven pattern inheritance yet; both depend on AKIOR Full patterns that are proven. Full is currently the only proven lane.
- **WhatsApp:** link + send both VERIFIED (DEC-032, messageId `3EB0F0DD0CB207EF639E1C`, CEO physical phone receipt). This is "send proven end-to-end", stronger than "linked only". It is **NOT** proof of whole-system robustness — only of the one proven Direction-A send path. Do not generalise.
- **Gmail read (browser-session lane):** one canonical inbox-list slice proven end-to-end. Stronger than idea-only or code-exists. **NOT** proof that Gmail send/compose/reply is solved — those remain MISSING by registry design (`capabilities.send=false`).
- **Google Calendar / Drive / Contacts browser-session lanes:** all three are now **CLOSED as read-only slices** — Calendar live events, Drive minimal browse, Contacts minimal list. Known non-blocking note: the `[data-id]` CDP selector used by Drive and Contacts catches Google WIZ-data script entries alongside truthful file/contact rows. Carved-out future scope; endpoints still return truthful envelopes and UIs render correctly. Write/compose/upload/delete/share/search/preview/detail/edit/merge for Calendar/Drive/Contacts are **NOT** implemented.
- **Auth-middleware cluster on `/api/channels/*`:** the auth-gap on `/api/channels/gmail/inbox*` and all parameterized siblings (`accounts`, `status`, `connect`, `disconnect`, `accounts/:accountId`) is now **CLOSED** across PRs #122–#125. `/api/channels/counts` remains intentionally unauthenticated as a proven carve-out. WhatsApp dedicated handlers are out of scope by policy. Audit `CHANNELS-AUTH-NON-WHATSAPP-CLOSURE-AUDIT-01` returned NO_CHANGE_NEEDED.
- **Nest / Alexa OAuth-adjacent surfaces** (`apps/server/src/clients/{nestClient.ts,alexaClient.ts}`): externally-managed credentials present in code; DORMANT. Not a DEC-033 violation because Gmail/Calendar/Drive/Contacts lanes are clean and Nest/Alexa are not invoked in Google paths. Flagged as future risk.

### Hard rules for reading this section
- UI presence is **NOT** backend proof.
- Code presence is **NOT** closure proof.
- Historical green CI is **NOT** current proof.
- "Linked" is **NOT** "send verified".
- "One slice proven" is **NOT** "capability solved".
- Contradictions are surfaced above, not smoothed over.

---

## 6. Current bounded next task

Governance is **current through the Google Workspace bundle closure**. The anchor matches product main at `fa3c4ff…`. Prior-cycle defaults (`GOV-ANCHOR-ROLL-AFTER-RATE-LIMIT-CLUSTER-03`, `GOOGLE-WORKSPACE-BUNDLE-CLOSURE-AUDIT-01`) are **already CLOSED** and must not be re-run.

The smallest correct next bounded task is:

> **OPEN — CTO must choose.** There is no automatic next implementation task. The Google Workspace browser-first bundle is CLOSED as a suite. Cluster 04 (the only remaining non-channel rate-limit alert) is locked-lane excluded and requires an explicit CTO decision to unlock. No other lane is currently in-queue.

Candidate lanes for the CTO to weigh (not ranked, not authorised here):
1. **Cluster 04 unlock decision** — explicit CTO decision to open the 3D-print lane for a single limiter insertion. Governance-level decision; do not execute without authorisation.
2. **Light / Cloud pattern inheritance scoping** — read-only analysis to identify which Full patterns (now including the full Google bundle) are ready to clone into Light / Cloud. Scoping-only; governance-level read.
3. **Drive/Contacts `[data-id]` CDP selector-fidelity refinement** — carved-out future scope; endpoints already return truthful envelopes. Small product surface; product-only; not currently blocking.
4. **Any Google write/compose/send/upload/share/delete/search/preview slice** — must be browser-only inside-system per DEC-033. Product-only; new bounded task required per capability.
5. **iMessage lane** — not attempted; CTO-level scoping first.

**Rules for whichever is picked:**
- Governance repo only OR product repo only per the task — never both in one task.
- One bounded task per chat. No widening.
- Follow the established proof path: baseline → target → minimal bounded edit → local validation → PR → green CI → merge → live evidence → governance follow-through.
- If in doubt: do nothing. Return to CTO for explicit scoping.

---

## 7. Product and governance anchors

| Repo | Branch | SHA |
|---|---|---|
| Product (`jarvis-v5-os`) | `main` | `fa3c4ffd9e85d1e0c237d7d443baf2228d54ed66` |
| Governance (`akior-governance`) | `main` | synced via GOV-GOOGLE-BUNDLE-CLOSURE-SYNC-01 (2026-04-15) |

Verification commands (run first in any new session):
```
cd ~/projects/akior/forge/jarvis-v5-os && git fetch origin main --quiet && git rev-parse origin/main
cd ~/akior/docs && git fetch origin main --quiet && git rev-parse origin/main
grep "jarvis-v5-os HEAD" ~/akior/docs/cto_external_ledger/RESTORE_BLOCK.txt
```
Expected: product `fa3c4ff…`, anchor line shows `fa3c4ffd9e85d1e0c237d7d443baf2228d54ed66`.

---

## 8. Authority & invariants

- DEC-031 ACTIVE (Gmail browser-session lane).
- DEC-032 ACTIVE & PROVEN (WhatsApp link + send, byte-locked — DO NOT TOUCH).
- DEC-033 ACTIVE & UNDEFEATED (Google credential model purged; browser-only inside-system).
- DEC-034 ACTIVE (PLAYWRIGHT_E2E_AUTH test-only; never in Dockerfile/fly.toml).
- DEC-027 / DEC-028 / DEC-029 SUPERSEDED.
- 3D-print lane CLOSED / LOCKED.
- WhatsApp send byte-identical (sha256 `37edb08f…`).

---

## 9. Risk register (current)

| Risk | Severity | Lane |
|---|---|---|
| Cluster 04 alert #54 remains open by lane-lock policy | accepted (locked) | non-channel rate-limit |
| Nest/Alexa OAuth credentials dormant in code | low (currently uninvoked) | future cleanup |
| Drive/Contacts `[data-id]` CDP selector picks up WIZ-data script entries alongside truthful rows | low (carved-out; endpoints return truthful envelopes) | future selector-fidelity refinement |
| Google write/compose/send/upload/delete/share/search/preview not implemented for any provider | medium (capability gap, not a blocker for current suite) | future Google write slices |
| `AKIOR_CTO_SUPERVISION_SSOT.md` referenced but missing from governance repo | medium (governance doc gap) | separate governance authorship task |

---

## 10. Recovery / rollback notes

- All recent product PRs (#118 → #121) are squash-merge commits on product `main`. Rollback path: `git revert <SHA>` per PR.
- Governance is recoverable from PR #7 / #8 / #9.
- No destructive operations performed. OpenClaw managed-browser session and signed-in Gmail tab persist across server stack restarts because the OpenClaw daemon is process-independent of `apps/server`.

---

## 11. Open carryover from prior handoff

- `06_BLOCKERS.md` working-tree edit remains in `stash@{0}: On main: wip-blockers` in `~/akior/docs`. Do not stage without explicit CTO decision; it predates this handoff cycle and was deliberately not committed.
- Untracked `docs/plans/` in product repo is a planning sandbox; do not commit.
- Untracked `test-results/gmail-inbox-smoke.png` in product repo is the Gmail canonical proof artifact; do not commit (proof artifact only).
- Untracked `.omc/` outside the governance working directory is unrelated; do not touch.

---

## 12. Files the next CTO should read first

In order:

1. `cto_external_ledger/09_FRESH_CHAT_CTO_HANDOFF.md` (this file)
2. `cto_external_ledger/RESTORE_BLOCK.txt` — confirm anchor matches §7
3. `cto_external_ledger/03_STATUS.md` — Product anchor + lane status
4. `cto_external_ledger/07_NEXT_ACTION.md` — latest CLOSED entries (top entry should be Cluster 03)
5. `cto_external_ledger/08_HANDOFF.md` — most-recent Handoff Anchor section (should be Cluster 03)
6. `cto_external_ledger/05_DECISIONS.md` — DEC-031 / 032 / 033 / 034 (DO NOT modify in routine work)
7. `cto_external_ledger/06_BLOCKERS.md` — read-only context

Skip during routine resume: 00–02, 04 (test log), broad audits, `plans/`.

`AKIOR_CTO_SUPERVISION_SSOT.md` is **referenced but missing from governance repo** — treat its absence as a known gap (§9), not as a file to read.

---

## 13. Exact first action for the new CTO (paste-ready)

Before any repo work, the new CTO must run this intake sequence:

```
# 1. Verify product anchor
cd ~/projects/akior/forge/jarvis-v5-os && git fetch origin main --quiet && git rev-parse origin/main

# 2. Verify governance anchor
cd ~/akior/docs && git fetch origin main --quiet && git rev-parse origin/main

# 3. Verify anchor line in governance
grep "jarvis-v5-os HEAD" ~/akior/docs/cto_external_ledger/RESTORE_BLOCK.txt
```

Expected results:
- product `fa3c4ffd9e85d1e0c237d7d443baf2228d54ed66`
- governance synced by GOV-GOOGLE-BUNDLE-CLOSURE-SYNC-01 (2026-04-15)
- anchor line: `- jarvis-v5-os HEAD: fa3c4ffd9e85d1e0c237d7d443baf2228d54ed66`

If any of the three differs from the above, §2 and §7 of this handoff are stale — re-read both repos' ledgers before proceeding.

If all three match, the CTO restates the four classifications for the current state in their own words:
- **PROVEN:** items from §4.
- **ASSUMED / NOT PROVEN:** items from §5 flagged ASSUMED.
- **BLOCKED / EXCLUDED:** Cluster 04 (locked-lane).
- **OUT OF SCOPE:** anything not in §4–§6 of this file.

Then the CTO picks exactly one lane from §6 and writes a new bounded-task prompt for it. **Do not start work without a bounded-task prompt.**

### Forbidden behaviours that cause drift

- Do **not** prepare a Cluster 03 implementation prompt (already CLOSED).
- Do **not** prepare a Cluster 04 prompt unless an explicit new CTO decision unlocks the 3D-print lane.
- Do **not** prepare any Google Workspace bundle closure prompt — the bundle is already CLOSED as a suite.
- Do **not** prepare Calendar/Drive/Contacts read-capability prompts — those slices are CLOSED.
- Do **not** assume "linked" = "send verified" (that only applies to WhatsApp, and only because §4 has the receipt).
- Do **not** assume "bundle CLOSED" = "Google write/compose/send/upload/share/delete/search/preview are solved" — those remain unimplemented.
- Do **not** mutate the product repo in a governance-only task.
- Do **not** mutate governance in a product-only task.
- Do **not** widen a bounded task mid-flight; stop and return to CTO if scope grows.
- Do **not** count historical green CI as current proof.
- Do **not** count code presence as closure proof.
- Do **not** skip the intake gate above.

---

*End of handoff.*
