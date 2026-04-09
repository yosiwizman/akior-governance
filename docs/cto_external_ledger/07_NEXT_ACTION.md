# 07_NEXT_ACTION.md — Exact Next Instruction
# This file always contains the copy-paste ready instruction for Claude Code.
# CEO pastes the INSTRUCTION BLOCK directly into Claude Code terminal.

---

## CURRENT TASK

**Task ID:** W-T05 Phase 4.6 — WhatsApp send fix + verify
**Status:** QUEUED — approved prompt ready to paste
**Notes:** Phase 4.6 is a combined fix+verify task. It applies the accepted one-line fix (`deviceIdentity: null` in `apps/server/src/whatsapp-send.ts`), then executes one real send to phone +***8490, then requires CEO physical phone receipt before WhatsApp send can be marked SOLVED.

**Prerequisites before pasting the Phase 4.6 prompt:**
1. jarvis-v5-os at commit 8f1b188 pushed to origin/main (confirmed by LB-013 durability)
2. LB-013 / LB-014 / LB-015 written and pushed (this governance update)
3. OpenClaw gateway running and WhatsApp linked (confirmed in Phase 4.5)
4. AKIOR server running on port 3002 with tsx watch (confirmed in Phase 4)

**Action:** CEO pastes the already-approved Phase 4.6 prompt into Claude Code. No regeneration required.

**After Phase 4.6 execution:**
- If HTTP 200 with messageId returned: CEO must check phone for the unique test string
- Only after CEO confirms physical receipt back in CTO chat may WhatsApp send be marked SOLVED
- If send fails again: STOP and escalate to CTO for diagnosis

---

## ACTIVE DECISIONS
- DEC-005: ACTIVE — OpenClaw built-in auth first
- DEC-028: ACTIVE — OAuth-client architecture (fallback)
- DEC-029: ACTIVE — Option A for Product 1 (OAuth fallback lane)
- DEC-030: ACTIVE — Execution guardrail lock
- DEC-031: ACTIVE AND PROVEN — Gmail via OpenClaw browser-session lane (Product 1)
- DEC-032: ACTIVE AND PROVEN — WhatsApp LINK via direct Node import (Product 1)
- DEC-027: SUPERSEDED

## ACTIVE BLOCKERS
- BLK-001: RESOLVED 2026-04-08
- BLK-002: RESOLVED for Gmail connection card. Calendar/Drive/Contacts pending.
- BLK-003: PARTIALLY RESOLVED — link proven, send NOT proven. Device pairing fix pending Phase 4.6.
- BLK-004: ACTIVE (Product 2 only, does not block Product 1)

---

## ARCHIVED CHAINS

### G-T06 Gmail Chain (COMPLETE)
Implementation: I1→I2→I3→I4→I4-AMEND-1→I5→I6→I7→I8→SPIKE→ACCEPTANCE→UI-CHANNELS-NAV
Key artifacts: g-t06-d1-refactor-plan.md, g-t06-impl-breakdown.md, g-t06-i9-reset-conformance-plan.md

### WhatsApp Discovery Chain (COMPLETE)
Discovery: W-T01→W-T02→W-T03→W-T03.5(partial)→W-T03.6→LB-009

### WhatsApp Implementation Chain (COMPLETE)
Implementation: W-T04.PREFLIGHT→W-T04.IMPL Phase 0→Phase 1→Phase 2→Phase 2 corrective→Phase 2 patch→Phase 3→LB-010
Result: DEC-032 PROVEN (link only). WhatsApp QR link solved. BLK-001 and BLK-003 link portion RESOLVED.

### WhatsApp Send Chain (IN PROGRESS)
W-T05: Phase 0 audit→Phase 1 feasibility→Phase 1.5 gateway RPC audit→Phase 2 strategy→Phase 3 implementation (8f1b188)→Phase 4 SEND_FAILED→Phase 4.5 device pairing audit→Phase 4.6 QUEUED
Result so far: Direction A locked, implementation durable at 8f1b188, blocker diagnosed as device pairing. Fix locked: `deviceIdentity: null`.
