# 03_STATUS.md — Current Project Status

- **Anchor date:** 2026-04-09
- **Active product:** AKIOR Full (Product 1)
- **Current batch:** Batch G — Channels, Skills & Infrastructure
- **Google Gmail status:** Gmail connection card SOLVED via OpenClaw browser-session lane (DEC-031). Gmail read/send NOT solved (no API tokens). Calendar, Drive, Contacts not yet solved. DEC-028 active, DEC-027 superseded.
- **WhatsApp Product 1 status:**
  - **Link:** SOLVED via DEC-032 (QR link proven 2026-04-08, creds persisted).
  - **Send:** NOT SOLVED. W-T05 Phase 3 implementation complete (commit 8f1b188, Direction A gateway RPC). Phase 4 one-send attempt FAILED (device pairing at gateway handshake). Phase 4.5 audit diagnosed and locked fix (Option A: `deviceIdentity: null`). Phase 4.6 is next.
- **Active channel decisions:** DEC-028 (OAuth fallback), DEC-029 (Product 1 Option A), DEC-031 (Gmail browser-session), DEC-032 (WhatsApp link, PROVEN)
- **Superseded:** DEC-027
- **Ledger:** LB-001 → ... → LB-012 → LB-013 (Phase 3) → LB-014 (Phase 4 SEND_FAILED) → LB-015 (Phase 4.5 audit)
- **Next major step:** W-T05 Phase 4.6 — apply `deviceIdentity: null` fix + one real send + require CEO physical phone receipt. WhatsApp send remains NOT SOLVED until physical receipt confirmed.

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
