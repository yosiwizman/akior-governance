# 07_NEXT_ACTION.md — Exact Next Instruction
# This file always contains the copy-paste ready instruction for Claude Code.
# CEO pastes the INSTRUCTION BLOCK directly into Claude Code terminal.

---

## CURRENT TASK

**Task ID:** PRODUCT-ANCHOR-ADVANCE-01
**Status:** CLOSED — 2026-04-13 (post-PR#114-merge update). Product anchor advanced to `e9f285186f2cd1cc474bff93cca0249e064e4307` (merge commit for PR #114). Chain: `bc10fe3 (3D-PRINT-CLOSURE) → cf04d99 (GMAIL-READ) → 5d8f704 (AUTH-BOOTSTRAP DEC-034) → d49d3e2 (TYPECHECK-REPAIR-01) → e1093d1 (SETTINGS-CONTRACT-RETIRE-01) → 2db8ab1 (SMOKE-PROBES-RETIRE-01) → e9f2851 (merge commit) → LOCKED`. Governance ratified in parallel.
**Current queue:** EMPTY. Next bounded lane to be chosen by CTO.
---
**Task ID:** PR-115-E2E-FULL-GREEN-AND-ANCHOR-ROLL-01
**Status:** CLOSED — 2026-04-13. Product anchor advanced to `e95aed32ef49cea7e2b674e097cfeaec6a6ad696` (merge commit for PR #115). E2E Smoke Tests (Playwright) full green (99 passed / 0 failed). All 14 CI checks pass. Governance ratified in parallel (sync/anchor-roll-after-pr115-01).
**Current queue:** EMPTY. CTO picks next bounded lane.
---
**Task ID:** CODEQL-CHANNELS-SECURITY-HARDENING-01
**Status:** CLOSED — 2026-04-13. Product anchor advanced to `508dabfe031ffc233a8c2b77bc093c7e04ac97fb` (merge commit for PR #116). 6 high-severity channels CodeQL alerts closed (rate-limit ×2, URL substring ×1+3 adjacent, path-injection ×2). 13 CI checks green. Out-of-scope carryover: WhatsApp rate-limit alert at index.ts:5160 + 22 non-channels rate-limit alerts + 3dprint/storage/log-injection findings.
**Current queue:** EMPTY. CTO picks next bounded lane.
---
**Task ID:** CODEQL-CHANNELS-PATH-INJECTION-CLOSURE-03
**Status:** CLOSED — 2026-04-14. Product anchor advanced to `7654a9a3668f1f2475fbf27d537377196c68c708` (merge commit for PR #117). Path-injection alerts #6 and #7 on `apps/server/src/channels/providers/browserSession.ts:206` retired on live main (state=fixed, fixed_at=2026-04-14T01:53:49Z). Carryover blocker `js/insecure-randomness` at `accountsIndex.ts:179` was deferred to a dedicated bounded task and is now CLOSED — see next entry.
**Current queue:** EMPTY. CTO picks next bounded lane.
---
**Task ID:** CODEQL-CHANNELS-INSECURE-RANDOMNESS-IDENTITY-TRUTH-04B
**Status:** CLOSED — 2026-04-14. Product anchor advanced to `9adc7fb13e35293ff866ea7717fed288933de965` (merge commit for PR #118). Exact historical alert recovered as alert #1 (js/insecure-randomness at `apps/server/src/channels/accountsIndex.ts:179`), proven OPEN on live main pre-fix via explicit state-scoped queries. Minimal channels-only fix in `mintAccount`: replaced `Math.random().toString(36).slice(2, 8)` with `randomBytes(4).toString("hex").slice(0, 6)` (+2 / -1, 1 file). All 13 CI checks green on PR #118 head. Post-merge live CodeQL re-scan confirms alert #1 state=fixed, fixed_at=2026-04-14T11:52:23Z. All channels-surface CodeQL high-severity findings are now retired. Non-channel `js/missing-rate-limiting` lane is now AUTHORIZABLE as the next bounded task (CTO picks).
**Current queue:** EMPTY. CTO picks next bounded lane.
**Notes:** Prior frozen queue (3D-PRINT-QUICK-VERIFY-01, GMAIL-READ-CAPABILITY-PLAN-01) RETIRED — both CLOSED. GMAIL-READ-INBOX-01/02/03 chain CLOSED / VERIFIED (see 03_STATUS.md). Product repo carries uncommitted changes across three verified lanes (3D-print closure, Gmail read implementation, E2E auth bootstrap). This task locks those changes into a new baseline product anchor.

### INSTRUCTION BLOCK

```
Bounded task: PRODUCT-ANCHOR-ADVANCE-01
Date: 2026-04-13

Scope:
1. Review the full uncommitted diff in ~/projects/akior/forge/jarvis-v5-os.
2. Commit changes in small, governance-disciplined commits — one per verified lane:
   - Lane A: 3D-print closure
   - Lane B: Gmail read implementation
   - Lane C: E2E auth bootstrap
   Do NOT create a single mega-commit.
3. Any uncommitted file that does not map to a verified lane must be either
   ratified (committed with explicit lane assignment) or reverted explicitly.
   No silent adoption.
4. After all lane commits land, record the new HEAD SHA as the product anchor
   in RESTORE_BLOCK.txt and 03_STATUS.md.
5. Do NOT add new features. Do NOT re-run verification. Verification is
   already binary-closed for all three lanes.

Out of scope:
- WhatsApp, 3D-print behavior changes, Gmail read behavior changes (all closed)
- Google credential revival
- Cloud / Light product expansion
- New external integrations

Success criteria:
- Product repo HEAD advances to a single locked SHA
- RESTORE_BLOCK.txt and 03_STATUS.md updated with the new anchor SHA
- No new product code introduced in this lock task
- Every committed file maps to a named verified lane
```

---

## ACTIVE DECISIONS
- DEC-005: ACTIVE — OpenClaw built-in auth first
- DEC-028: Credential-model portion SUPERSEDED by DEC-033; browser-auth-only principle still active
- DEC-029: Credential-provisioning portion SUPERSEDED by DEC-033
- DEC-033: ACTIVE — Google credential-model purge, browser-only inside-system auth posture
- DEC-030: ACTIVE — Execution guardrail lock
- DEC-031: ACTIVE AND PROVEN — Gmail via OpenClaw browser-session lane (Product 1)
- DEC-032: ACTIVE AND PROVEN — WhatsApp link + send (Product 1). Send verified end-to-end 2026-04-09.
- DEC-027: SUPERSEDED

## ACTIVE BLOCKERS
- BLK-001: RESOLVED 2026-04-08
- BLK-002: RESOLVED for Gmail connection card. Calendar/Drive/Contacts pending.
- BLK-003: CLOSED 2026-04-09 — WhatsApp Product 1 send lane verified end-to-end.
- BLK-004: ACTIVE (Product 2 only, does not block Product 1)

---

## AFTER THIS TASK

Queue is empty after PRODUCT-ANCHOR-ADVANCE-01 completes. CTO chooses the next bounded lane. Candidates (CTO picks — not Claude): Gmail inbox polish, AKIOR Light scoping, holomat follow-ups.

---

## ARCHIVED CHAINS

### Gmail Read Chain (COMPLETE — VERIFIED)
GMAIL-READ-INBOX-01 (backend route) → GMAIL-READ-INBOX-02 (UI integration) → GMAIL-READ-INBOX-03 (E2E auth bootstrap) — all CLOSED.

### G-T06 Gmail Channel Chain (COMPLETE)
Implementation: I1→I2→I3→I4→I4-AMEND-1→I5→I6→I7→I8→SPIKE→ACCEPTANCE→UI-CHANNELS-NAV

### WhatsApp Discovery Chain (COMPLETE)
Discovery: W-T01→W-T02→W-T03→W-T03.5(partial)→W-T03.6→LB-009

### WhatsApp Link Chain (COMPLETE)
Implementation: W-T04.PREFLIGHT→W-T04.IMPL Phase 0→Phase 1→Phase 2→Phase 3→LB-010
Result: DEC-032 PROVEN (link). WhatsApp QR link solved.

### WhatsApp Send Chain (COMPLETE — VERIFIED END-TO-END SUCCESS)
W-T05: Phase 0 audit→Phase 1 feasibility→Phase 1.5 gateway RPC audit→Phase 2 strategy→Phase 3 implementation (8f1b188)→Phase 4 SEND_FAILED→Phase 4.5 device pairing audit→Phase 4.6 (F1)→Phase 4.7 (F2)→Phase 4.8 (fresh process)→Phase 4.9 (scope audit)→Phase 4.10 (F3)→Phase 4.11 (listener audit)→Phase 4.12A (runtime sanity)→Phase 4.12B (VERIFIED END-TO-END SUCCESS)
Result: Direction A confirmed. HTTP 200, messageId `3EB0F0DD0CB207EF639E1C`. CEO physical phone receipt confirmed. BLK-003 CLOSED.
