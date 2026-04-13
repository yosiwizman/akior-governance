# 07_NEXT_ACTION.md — Exact Next Instruction
# This file always contains the copy-paste ready instruction for Claude Code.
# CEO pastes the INSTRUCTION BLOCK directly into Claude Code terminal.

---

## CURRENT TASK

**Task ID:** PRODUCT-ANCHOR-ADVANCE-01
**Status:** CLOSED — 2026-04-13 (post-PR#114-merge update). Product anchor advanced to `e9f285186f2cd1cc474bff93cca0249e064e4307` (merge commit for PR #114). Chain: `bc10fe3 (3D-PRINT-CLOSURE) → cf04d99 (GMAIL-READ) → 5d8f704 (AUTH-BOOTSTRAP DEC-034) → d49d3e2 (TYPECHECK-REPAIR-01) → e1093d1 (SETTINGS-CONTRACT-RETIRE-01) → 2db8ab1 (SMOKE-PROBES-RETIRE-01) → e9f2851 (merge commit) → LOCKED`. Governance ratified in parallel.
**Current queue:** EMPTY. Next bounded lane to be chosen by CTO.
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
