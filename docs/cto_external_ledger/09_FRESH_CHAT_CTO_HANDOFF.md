# 09_FRESH_CHAT_CTO_HANDOFF.md
# Last updated: 2026-04-14 (post Cluster 03 product closure)
# Audience: a fresh AKIOR CTO chat session resuming work mid-stream.
# Style: crisp, evidence-based, no hype.

---

## 1. Purpose

Single source of truth for a brand-new CTO chat to resume work without re-reading the full ledger. Every claim below is anchored to a merged commit, a CodeQL alert state, or a governance file. Anything not anchored is labelled ASSUMED / NOT PROVEN.

---

## 2. Current locked baseline

- **Product main SHA:** `3d81f2c5ff3d6b53349ec0ae45c61be9ba226d10`
- **Tag:** locked by CODEQL-NON-CHANNELS-RATE-LIMITING-CLUSTER-03-USER-DATA-CRUD (PR #121 squash-merge, 2026-04-14)
- **Governance main SHA at last verified roll:** `cefb92a7…` (after Cluster 02 anchor roll, PR #8). Governance follow-through for Cluster 03 is **NOT yet verified in this handoff cycle** — see §6.
- Repos: product `yosiwizman/jarvis-v5-os`, governance `yosiwizman/akior-governance`.

---

## 3. Accepted current reality

- Product anchor advanced from `3931498…` → `3d81f2c5ff3d6b53349ec0ae45c61be9ba226d10`.
- Cluster 03 (user-data CRUD, 9 alerts) is **CLOSED in product**.
- Cluster 03 closure ran with one PR-CI iteration: first run failed E2E because the read-side bucket (`ADMIN_LIGHT` 30/5min) was too tight for polled GET endpoints under Playwright fan-out + Next dev HMR — observed as HTTP 429 cascading into 200-expecting tests in run id `24416602047`. Same PR was corrected in-place by switching the seven READ endpoints to an inline custom bucket `{ maxAttempts: 600, windowMs: 60_000, lockoutMs: 60_000 }` (10 req/sec, 1-min lockout). Mutating endpoints (`POST /api/settings`, `DELETE /api/file-library/:name`) kept `RateLimitPresets.ADMIN_MODERATE`. Second CI run: 13/13 green. Same helper, same prelude shape, single-file boundary preserved.
- Governance has NOT yet been observed to record this exact Cluster 03 closure state in this handoff cycle — verify before starting any new product work.

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

## 5. What is NOT solved / not proven

- **Cluster 04 — 3D-print rate-limit alert** (`apps/server/src/routes/3dprint.routes.ts:403`, alert #54). **Locked-lane excluded by policy** — the 3D-print lane is CLOSED/LOCKED per RESTORE_BLOCK.txt and must not be reopened to add a limiter without a new explicit CTO decision. Status: PERMANENTLY EXCLUDED FROM THIS LANE.
- Google Calendar / Drive / Contacts (Google integration): **NOT IMPLEMENTED**. The Gmail canonical proof established the browser-first / inside-system pattern but no other Google capability has been cloned from it. ASSUMED clonable; NOT PROVEN.
- Auth-middleware gap on `/api/channels/gmail/inbox*` routes: read-only routes do not call `requireAdmin`. Identified during the OpenClaw audit; **NOT YET REMEDIATED**. Out of scope for the current rate-limit lane.
- Nest / Alexa OAuth-adjacent surfaces (`apps/server/src/clients/{nestClient.ts,alexaClient.ts}`): externally-managed credentials present in code; **DORMANT**. Not a DEC-033 violation in current state because Gmail lane is clean and Nest/Alexa are not invoked in Gmail paths. Flagged as future risk.
- Governance follow-through for Cluster 03 closure: **NOT YET VERIFIED in this handoff cycle.** See §6.

---

## 6. Current bounded next task

**The next bounded task is NOT new product implementation.** Cluster 03 product closure is complete; what is missing is governance truth maintenance.

**Default next bounded task: `GOV-ANCHOR-ROLL-AFTER-RATE-LIMIT-CLUSTER-03`**

- **Repo scope:** governance repo (`~/akior/docs`) ONLY.
- **No product repo mutation.**
- Required edits (4 files, minimal):
  - `cto_external_ledger/RESTORE_BLOCK.txt` — roll `jarvis-v5-os HEAD` to `3d81f2c5ff3d6b53349ec0ae45c61be9ba226d10`; extend Chain with the Cluster 03 commits (`c7f2bf2` initial preludes, `db04efa` bucket fixup, `3d81f2c` PR #121 merge); update BASELINE STATE tag.
  - `cto_external_ledger/03_STATUS.md` — rewrite Product anchor line to reflect Cluster 03 closure; cumulative non-channel lane status `19/20 retired, 1 locked-lane excluded`.
  - `cto_external_ledger/07_NEXT_ACTION.md` — append CLOSED entry for `CODEQL-NON-CHANNELS-RATE-LIMITING-CLUSTER-03-USER-DATA-CRUD` with all 9 alert IDs, fixed_at timestamp, the in-PR bucket-fixup nuance, and the explicit fact that PR #121 merged after CI green on the second run.
  - `cto_external_ledger/08_HANDOFF.md` — new Handoff Anchor 2026-04-14 section for Cluster 03 above the Cluster 02 section; Locked Baseline anchors rolled (`3d81f2c` current, `3931498` prior); cumulative status enumerated; Cluster 04 explicitly preserved as locked-lane excluded.
- **Must preserve:**
  - Cluster 04 framed as locked-lane excluded (NOT next-up).
  - No claim that the broader non-channel rate-limit lane is fully retired (it is not — 1 alert remains, even though it is excluded).
  - WhatsApp / DEC-031 / DEC-033 / DEC-034 invariants untouched.
  - 05_DECISIONS.md and 06_BLOCKERS.md unchanged.
- **Do NOT:**
  - begin Cluster 04 implementation (locked lane).
  - mutate the product repo.
  - widen into Calendar / Drive / Contacts / Gmail send.
  - rewrite governance docs broadly.

---

## 7. Product and governance anchors

| Repo | Branch | SHA |
|---|---|---|
| Product (`jarvis-v5-os`) | `main` | `3d81f2c5ff3d6b53349ec0ae45c61be9ba226d10` |
| Governance (`akior-governance`) | `main` | `cefb92a7a293152c88f11ba58056be19ff55957d` (last verified after Cluster 02 anchor roll PR #8; **NOT YET ADVANCED for Cluster 03**) |

If governance HEAD differs from `cefb92a7…` at session start, that means the Cluster 03 anchor roll has already landed — verify by grepping `jarvis-v5-os HEAD` in `cto_external_ledger/RESTORE_BLOCK.txt` on governance main.

---

## 8. Authority & invariants

- DEC-031 ACTIVE (Gmail browser-session lane).
- DEC-032 ACTIVE & PROVEN (WhatsApp link + send, byte-locked — DO NOT TOUCH).
- DEC-033 ACTIVE & UNDEFEATED (Google credential model purged; browser-only inside-system).
- DEC-034 ACTIVE (PLAYWRIGHT_E2E_AUTH test-only; never in Dockerfile/fly.toml).
- DEC-027 / DEC-028 / DEC-029 SUPERSEDED.
- 3D-print lane CLOSED/LOCKED.
- WhatsApp send byte-identical (sha256 `37edb08f…`).

---

## 9. Risk register (current)

| Risk | Severity | Lane |
|---|---|---|
| Cluster 04 alert #54 remains open by lane-lock policy | accepted (locked) | non-channel rate-limit |
| `/api/channels/gmail/inbox*` lacks `requireAdmin` | medium | future channel-auth lane |
| Nest/Alexa OAuth credentials dormant in code | low (currently uninvoked) | future cleanup |
| Calendar / Drive / Contacts not implemented | medium (capability gap) | future Google clone slices |

---

## 10. Recovery / rollback notes

- All recent product PRs (#118 → #121) are squash-merge commits on `main`. Rollback path: `git revert <SHA>` per PR, then push.
- Governance is recoverable from PR #7 / #8 / pending #9.
- No destructive operations performed; OpenClaw managed-browser session and signed-in Gmail tab persist across server stack restarts because OpenClaw daemon is process-independent of `apps/server`.

---

## 11. Open carryover from prior handoff

- `06_BLOCKERS.md` working-tree edit remains in `stash@{0}: On main: wip-blockers` in `~/akior/docs`. Do not stage without explicit CTO decision; it predates this handoff cycle and was deliberately not committed.
- Untracked `docs/plans/` in product repo is a planning sandbox; do not commit.
- Untracked `test-results/gmail-inbox-smoke.png` in product repo is the Gmail canonical proof artifact; do not commit (proof artifact only).

---

## 12. Files the next CTO should read first

In order:

1. `cto_external_ledger/09_FRESH_CHAT_CTO_HANDOFF.md` (this file)
2. `AKIOR_CTO_SUPERVISION_SSOT.md` (governance constitution)
3. `cto_external_ledger/RESTORE_BLOCK.txt` (anchor truth — confirm `jarvis-v5-os HEAD` matches §2)
4. `cto_external_ledger/03_STATUS.md` (Product anchor + lane status)
5. `cto_external_ledger/07_NEXT_ACTION.md` (latest CLOSED entries — confirm Cluster 03 entry exists once governance roll lands)
6. `cto_external_ledger/08_HANDOFF.md` (most-recent Handoff Anchor section)
7. `cto_external_ledger/05_DECISIONS.md` (DEC-031/032/033/034 — DO NOT modify in routine work)
8. `cto_external_ledger/06_BLOCKERS.md` (read-only context; carryover edit is stashed)

Skip during routine resume: 00–02, 04 (test log), broad audits, plans/.

---

## 13. Exact first action for the new CTO

Before any repo work:

1. **Read this handoff** end-to-end.
2. **Read `AKIOR_CTO_SUPERVISION_SSOT.md`** for the supervision constitution.
3. **Restate** the four classifications for the current state in your own words:
   - **PROVEN:** what is in §4 with a live anchor or live CodeQL evidence.
   - **ASSUMED:** what is in §5 marked ASSUMED / NOT PROVEN.
   - **BLOCKED / EXCLUDED:** Cluster 04 (locked-lane).
   - **OUT OF SCOPE:** anything not in §4–§6 of this file.
4. **Confirm exact current product anchor** is `3d81f2c5ff3d6b53349ec0ae45c61be9ba226d10`:
   ```
   cd ~/projects/akior/forge/jarvis-v5-os && git fetch origin main --quiet && git rev-parse origin/main
   ```
5. **Identify stale documents** in the governance repo that still describe Cluster 03 as "next" or "carryover" rather than CLOSED. At minimum scan:
   - `cto_external_ledger/RESTORE_BLOCK.txt` (anchor line)
   - `cto_external_ledger/03_STATUS.md` (Product anchor bullet)
   - `cto_external_ledger/07_NEXT_ACTION.md` (top-most CLOSED entry)
   - `cto_external_ledger/08_HANDOFF.md` (Locked Baseline section)
6. **Verify governance currency:**
   ```
   cd ~/akior/docs && git fetch origin main --quiet && git rev-parse origin/main
   grep "jarvis-v5-os HEAD" cto_external_ledger/RESTORE_BLOCK.txt
   ```
   - If `jarvis-v5-os HEAD` already equals `3d81f2c…` → Cluster 03 governance roll is COMPLETE; pick a different bounded task (consult §5 risk register; CTO chooses).
   - If `jarvis-v5-os HEAD` still equals `3931498…` (or older) → governance is stale; **prepare and run the narrow governance-only prompt for `GOV-ANCHOR-ROLL-AFTER-RATE-LIMIT-CLUSTER-03`** as defined in §6.
7. **Do NOT prepare a Cluster 03 product implementation prompt.** Cluster 03 is CLOSED in product.
8. **Do NOT prepare a Cluster 04 prompt** unless a new explicit CTO decision unlocks the 3D-print lane.

---

*End of handoff.*
