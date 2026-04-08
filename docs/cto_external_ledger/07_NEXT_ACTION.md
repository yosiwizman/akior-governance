# 07_NEXT_ACTION.md — Exact Next Instruction
# This file always contains the copy-paste ready instruction for Claude Code.
# CEO pastes the INSTRUCTION BLOCK directly into Claude Code terminal.

---

## CURRENT TASK

**Task ID:** LB-010 complete. WhatsApp slice closed.
**Status:** NO QUEUED TASK
**Notes:** AKIOR Full Product 1 now has two solved channels: Gmail (DEC-031) and WhatsApp (DEC-032). Both verified end-to-end with CEO UI acceptance. No feature task is currently queued for Claude Code. Future slices and the workspace hygiene pass are recorded below for visibility only.

Future possible tasks (not queued, not approved, recorded for visibility only):
- G-T06.BROWSER-CALENDAR — mirror DEC-031 or DEC-032 pattern for Google Calendar
- G-T06.BROWSER-DRIVE — mirror DEC-031 or DEC-032 pattern for Google Drive
- G-T06.BROWSER-CONTACTS — mirror DEC-031 or DEC-032 pattern for Google Contacts
- iMessage channel — separate future slice, not yet scoped
- Workspace hygiene pass — commit the full G-T06 Gmail chain and W-T01-W-T04 WhatsApp chain, audit the 5 pre-existing typecheck errors (setup.tsx, JarvisAssistant, demo module imports), review pre-existing dirty files in git status, clean up what can be cleaned
- Batch H — Contact Intelligence tasks per roadmap
- Product 2 (AKIOR Light) Google and WhatsApp planning — evaluate whether DEC-031 and DEC-032 patterns scale to Mac Mini, revisit BLK-004

---

## ACTIVE DECISIONS
- DEC-005: ACTIVE — OpenClaw built-in auth first
- DEC-028: ACTIVE — OAuth-client architecture (fallback)
- DEC-029: ACTIVE — Option A for Product 1 (OAuth fallback lane)
- DEC-030: ACTIVE — Execution guardrail lock
- DEC-031: ACTIVE AND PROVEN — Gmail via OpenClaw browser-session lane (Product 1)
- DEC-032: ACTIVE AND PROVEN — WhatsApp via direct Node import of OpenClaw core functions (Product 1)
- DEC-027: SUPERSEDED

## ACTIVE BLOCKERS
- BLK-001: RESOLVED 2026-04-08
- BLK-002: RESOLVED for Gmail (DEC-031). Calendar/Drive/Contacts pending.
- BLK-003: RESOLVED 2026-04-08
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
Result: DEC-032 PROVEN. WhatsApp Product 1 solved. BLK-001 and BLK-003 RESOLVED.
