# 09_FRESH_CHAT_CTO_HANDOFF.md
# Last updated: 2026-04-14 (post Cluster 03 governance anchor-roll merged)
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

- **Product main SHA:** `3d81f2c5ff3d6b53349ec0ae45c61be9ba226d10`
- **Product tag:** locked by CODEQL-NON-CHANNELS-RATE-LIMITING-CLUSTER-03-USER-DATA-CRUD (PR #121 squash-merge, 2026-04-14).
- **Governance main SHA:** `7918b5e457bd673a57bb66b7b4ce16b17d6de06e`
- **Governance tag:** locked by GOV-ANCHOR-ROLL-AFTER-RATE-LIMIT-CLUSTER-03 (PR #9 squash-merge, 2026-04-14).
- Governance follow-through for Cluster 03 is **CURRENT** on tracked main. If on session start you observe a newer product SHA that does not match `3d81f2c…`, re-verify §2 before acting.
- Repos: product `yosiwizman/jarvis-v5-os`, governance `yosiwizman/akior-governance`.

---

## 3. Accepted current reality

- Product anchor advanced from `3931498…` → `3d81f2c5ff3d6b53349ec0ae45c61be9ba226d10` via PR #121.
- Cluster 03 (user-data CRUD, 9 alerts) is **CLOSED in product** and **RECORDED in governance**.
- Cluster 03 closure required an in-PR bucket fixup: first CI run failed E2E Smoke Tests (Playwright) because the `ADMIN_LIGHT` (30/5min) bucket was too tight for polled GET endpoints under Playwright fan-out + Next dev HMR — observed as HTTP 429 cascading into 200-expecting tests in run id `24416602047`. Same PR was corrected in-place by switching the seven READ endpoints to an inline custom bucket `{ maxAttempts: 600, windowMs: 60_000, lockoutMs: 60_000 }` (10 req/sec, 1-min lockout). Mutating endpoints (`POST /api/settings`, `DELETE /api/file-library/:name`) kept `RateLimitPresets.ADMIN_MODERATE`. Second CI run: 13/13 green. Same helper, same prelude shape, single-file boundary preserved.
- Governance has ratified this closure in 4 edited ledger files + 1 new tracked file (`09_FRESH_CHAT_CTO_HANDOFF.md`).

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

**Cumulative non-channel rate-limit lane status:** **19 / 20 alerts retired.** Remaining: 1 (Cluster 04, locked-lane excluded — see §5).

---

## 5. What is NOT solved / not proven / excluded

### Excluded by policy (locked lane)
- **Cluster 04 — 3D-print rate-limit alert** (`apps/server/src/routes/3dprint.routes.ts:403`, alert #54). Locked-lane excluded. The 3D-print lane is CLOSED/LOCKED per RESTORE_BLOCK.txt and must not be reopened to add a limiter without an **explicit new CTO decision** to unlock. Cluster 04 is the sole remaining carryover of the non-channel rate-limit lane. It is **NOT** the automatic next implementation task.

### Not proven / future work
- **AKIOR Light / AKIOR Cloud:** no proven pattern inheritance yet; both depend on AKIOR Full patterns that are proven. Full is currently the only proven lane.
- **WhatsApp:** link + send both VERIFIED (DEC-032, messageId `3EB0F0DD0CB207EF639E1C`, CEO physical phone receipt). This is "send proven end-to-end", stronger than "linked only". It is **NOT** proof of whole-system robustness — only of the one proven Direction-A send path. Do not generalise.
- **Gmail read (browser-session lane):** one canonical inbox-list slice proven end-to-end on `9adc7fb…`. This is stronger than idea-only or code-exists. It is **NOT** proof that Gmail read/send is globally solved — only the single canonical slice. Gmail send/compose/reply is explicitly MISSING by registry design (`capabilities.send=false`).
- **Google Calendar / Drive / Contacts:** NOT IMPLEMENTED. ASSUMED clonable from the Gmail canonical pattern; NOT PROVEN.
- **Auth-middleware gap on `/api/channels/gmail/inbox*`:** read-only routes do not call `requireAdmin`. Identified during the OpenClaw audit; NOT YET REMEDIATED. Separate future channel-auth lane.
- **Nest / Alexa OAuth-adjacent surfaces** (`apps/server/src/clients/{nestClient.ts,alexaClient.ts}`): externally-managed credentials present in code; DORMANT. Not a DEC-033 violation because Gmail lane is clean and Nest/Alexa are not invoked in Gmail paths. Flagged as future risk.

### Hard rules for reading this section
- UI presence is **NOT** backend proof.
- Code presence is **NOT** closure proof.
- Historical green CI is **NOT** current proof.
- "Linked" is **NOT** "send verified".
- "One slice proven" is **NOT** "capability solved".
- Contradictions are surfaced above, not smoothed over.

---

## 6. Current bounded next task

Governance is **current through Cluster 03**. The anchor matches product main at `3d81f2c…`. Therefore the prior-cycle default (`GOV-ANCHOR-ROLL-AFTER-RATE-LIMIT-CLUSTER-03`) is **already CLOSED** and must not be re-run.

The smallest correct next bounded task is:

> **OPEN — CTO must choose.** There is no automatic next implementation task. Cluster 04 (the only remaining non-channel rate-limit alert) is locked-lane excluded and requires an explicit CTO decision to unlock. No other lane is currently in-queue.

Candidate lanes for the CTO to weigh (not ranked, not authorised here):
1. **Auth-middleware gap on `/api/channels/gmail/inbox*`** — add `requireAdmin` to Gmail read routes. Small surface; product-only.
2. **Google Calendar canonical slice** — clone the Gmail browser-session pattern for one canonical read capability. Larger surface; product-only.
3. **Cluster 04 unlock decision** — explicit CTO decision to open the 3D-print lane for a single limiter insertion. Governance-level decision; do not execute without authorisation.
4. **Light / Cloud pattern inheritance scoping** — read-only analysis to identify which Full patterns are ready to clone into Light / Cloud. Scoping-only; governance-level read.

**Rules for whichever is picked:**
- Governance repo only OR product repo only per the task — never both in one task.
- One bounded task per chat. No widening.
- Follow the established proof path: baseline → target → minimal bounded edit → local validation → PR → green CI → merge → live evidence → governance follow-through.
- If in doubt: do nothing. Return to CTO for explicit scoping.

---

## 7. Product and governance anchors

| Repo | Branch | SHA |
|---|---|---|
| Product (`jarvis-v5-os`) | `main` | `3d81f2c5ff3d6b53349ec0ae45c61be9ba226d10` |
| Governance (`akior-governance`) | `main` | `7918b5e457bd673a57bb66b7b4ce16b17d6de06e` |

Verification commands (run first in any new session):
```
cd ~/projects/akior/forge/jarvis-v5-os && git fetch origin main --quiet && git rev-parse origin/main
cd ~/akior/docs && git fetch origin main --quiet && git rev-parse origin/main
grep "jarvis-v5-os HEAD" ~/akior/docs/cto_external_ledger/RESTORE_BLOCK.txt
```
Expected: product `3d81f2c…`, governance `7918b5e4…`, anchor line shows `3d81f2c…`.

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
| `/api/channels/gmail/inbox*` lacks `requireAdmin` | medium | future channel-auth lane |
| Nest/Alexa OAuth credentials dormant in code | low (currently uninvoked) | future cleanup |
| Calendar / Drive / Contacts not implemented | medium (capability gap) | future Google clone slices |
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
- product `3d81f2c5ff3d6b53349ec0ae45c61be9ba226d10`
- governance `7918b5e457bd673a57bb66b7b4ce16b17d6de06e`
- anchor line: `- jarvis-v5-os HEAD: 3d81f2c5ff3d6b53349ec0ae45c61be9ba226d10`

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
- Do **not** assume "linked" = "send verified" (that only applies to WhatsApp, and only because §4 has the receipt).
- Do **not** assume "one Gmail slice proven" = "Gmail capability solved".
- Do **not** mutate the product repo in a governance-only task.
- Do **not** mutate governance in a product-only task.
- Do **not** widen a bounded task mid-flight; stop and return to CTO if scope grows.
- Do **not** count historical green CI as current proof.
- Do **not** count code presence as closure proof.
- Do **not** skip the intake gate above.

---

*End of handoff.*
