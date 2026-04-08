# 00_PROTOCOL.md — CTO External Ledger Protocol

## Purpose
Canonical external ledger for the CTO/Claude-Chat side of the AKIOR project. This directory is the single source of truth for decisions, blockers, status, and handoff state that must persist across Claude Code sessions and Claude Chat advisory sessions.

## Update Rules
- After every completed Claude Code task, the relevant canonical files in this directory are updated on disk.
- `03_STATUS.md` and `07_NEXT_ACTION.md` must always reflect the current state.
- `08_HANDOFF.md` and `RESTORE_BLOCK.txt` must be refreshed whenever a handoff triggers.
- Superseded decisions are preserved in `05_DECISIONS.md`, never deleted.
- No invented progress; code existence does not equal approval; proof-first.

## Canonical Files
| File | Purpose |
|------|---------|
| 00_PROTOCOL.md | This protocol document |
| 01_LEDGER.md | Chronological action log |
| 02_ROADMAP.md | Product roadmap snapshot |
| 03_STATUS.md | Current project status |
| 04_TEST_LOG.md | Test results log |
| 05_DECISIONS.md | Decision chain (append-only, never delete) |
| 06_BLOCKERS.md | Active blockers |
| 07_NEXT_ACTION.md | Exact next instruction for Claude Code |
| 08_HANDOFF.md | Session handoff state |
| NEW_CHAT_STARTER.txt | Instruction for fresh Claude Chat sessions |
| RESTORE_BLOCK.txt | Minimal restore state for any new session |
