# 07_NEXT_ACTION.md — Exact Next Instruction
# This file always contains the copy-paste ready instruction for Claude Code.
# CEO pastes the INSTRUCTION BLOCK directly into Claude Code terminal.

---

## CURRENT TASK

**Task ID:** OPS-CRED-01 rerun (after asset delivery) → then I9-OPS
**Status:** BLOCKED — OPS-CRED-01 completed 2026-04-09 with verdict `OPS_CRED_SOURCE_BLOCKED_MISSING_AKIOR_OWNED_GOOGLE_OAUTH_CLIENT`. Handoff artifact at `~/.akior/ops/OPS-CRED-01-HANDOFF.md`.
**Notes:** Next major task: deliver the missing AKIOR-internal Google OAuth client credentials via one of the acceptable non-interactive operator-only channels named in `~/.akior/ops/OPS-CRED-01-HANDOFF.md`, then re-run OPS-CRED-01. I9-OPS is NOT scheduled until OPS-CRED-01 succeeds. Do not begin any Google implementation work. Do not start any OAuth flow. Do not involve the CEO. Google remains NOT solved. DEC-028 active, DEC-027 superseded. Do not collapse OpenClaw GOG skill / gog CLI / Claude Code MCP. WhatsApp send lane is closed (VERIFIED END-TO-END SUCCESS, BLK-003 CLOSED).

---

## ACTIVE DECISIONS
- DEC-005: ACTIVE — OpenClaw built-in auth first
- DEC-028: ACTIVE — OAuth-client architecture (fallback)
- DEC-029: ACTIVE — Option A for Product 1 (OAuth fallback lane)
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

## ARCHIVED CHAINS

### G-T06 Gmail Chain (COMPLETE)
Implementation: I1→I2→I3→I4→I4-AMEND-1→I5→I6→I7→I8→SPIKE→ACCEPTANCE→UI-CHANNELS-NAV

### WhatsApp Discovery Chain (COMPLETE)
Discovery: W-T01→W-T02→W-T03→W-T03.5(partial)→W-T03.6→LB-009

### WhatsApp Link Chain (COMPLETE)
Implementation: W-T04.PREFLIGHT→W-T04.IMPL Phase 0→Phase 1→Phase 2→Phase 3→LB-010
Result: DEC-032 PROVEN (link). WhatsApp QR link solved.

### WhatsApp Send Chain (COMPLETE — VERIFIED END-TO-END SUCCESS)
W-T05: Phase 0 audit→Phase 1 feasibility→Phase 1.5 gateway RPC audit→Phase 2 strategy→Phase 3 implementation (8f1b188)→Phase 4 SEND_FAILED→Phase 4.5 device pairing audit→Phase 4.6 (F1)→Phase 4.7 (F2)→Phase 4.8 (fresh process)→Phase 4.9 (scope audit)→Phase 4.10 (F3)→Phase 4.11 (listener audit)→Phase 4.12A (runtime sanity)→Phase 4.12B (VERIFIED END-TO-END SUCCESS)
Result: Direction A confirmed. HTTP 200, messageId `3EB0F0DD0CB207EF639E1C`. CEO physical phone receipt confirmed. BLK-003 CLOSED.
