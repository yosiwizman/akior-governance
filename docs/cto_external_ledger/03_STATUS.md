# 03_STATUS.md — Current Project Status

- **Anchor date:** 2026-04-07
- **Active product:** AKIOR Full (Product 1)
- **Current batch:** Batch G — Channels, Skills & Infrastructure
- **Google Gmail status:** SOLVED via OpenClaw browser-session lane (DEC-031). Calendar, Drive, Contacts not yet solved.
- **WhatsApp Product 1 status:** SOLVED via DEC-032 (direct Node import of OpenClaw core functions). End-to-end acceptance confirmed 2026-04-08.
- **Active channel decisions:** DEC-028 (OAuth fallback), DEC-029 (Product 1 Option A), DEC-031 (Gmail browser-session), DEC-032 (WhatsApp direct-import, PROVEN)
- **Superseded:** DEC-027
- **Ledger:** LB-001 → LB-002 → LB-003 → LB-005 → LB-009 → LB-010 (WhatsApp slice closure)
- **Next major step:** LB-010 complete. No queued feature tasks. Future slices (Calendar, Drive, Contacts, iMessage) pending CEO prioritization. Workspace hygiene pass pending (5 pre-existing typecheck errors, uncommitted chain work).

## What Works
- Chat text: Verified PASS (local qwen2.5:72b)
- Voice: Verified PASS (OpenAI Realtime WebRTC)
- UI: 12/12 smoke test PASS
- Contacts CRUD: Verified PASS
- DND toggle: Verified PASS
- Scheduled Tasks: Verified PASS
- Yahoo Mail: Connected via himalaya
- GPU roles: v2.7 compliant
- /mnt/models data plane: partially initialized (dirs created, HF cache redirected)
- HRM: operationally ready on GPU 1 (venv healthy, flash_attn working)
- Canonical external ledger: LB-001 → LB-002 → LB-003 → LB-005 → LB-009 → LB-010
- Gmail: OpenClaw browser-session lane (DEC-031), verified end-to-end
- WhatsApp: OpenClaw direct-import lane (DEC-032), verified end-to-end via W-T04.IMPL, creds persisted, card shows Connected in AKIOR Channels UI

## What Does Not Work Yet
- Google Calendar, Drive, Contacts: not yet wired (separate future slices)
- iMessage: not attempted
