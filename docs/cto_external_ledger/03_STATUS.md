# 03_STATUS.md — Current Project Status

- **Anchor date:** 2026-04-09
- **Active product:** AKIOR Full (Product 1)
- **Current batch:** Batch G — Channels, Skills & Infrastructure
- **Google status:** Gmail browser-session lane (DEC-031) STRUCTURALLY INTACT but IDLE — managed browser not running, no active Gmail session (GMAIL-VERIFY-01, 2026-04-10). Gmail read/send NOT solved. Calendar, Drive, Contacts not yet solved. DEC-033 active: credential model purged, browser-only auth posture. DEC-028 credential-model superseded by DEC-033. DEC-027 superseded.
- **WhatsApp Product 1 status:**
  - **Link:** SOLVED via DEC-032 (QR link proven 2026-04-08, creds persisted).
  - **Send:** VERIFIED END-TO-END SUCCESS (Phase 4.12B, 2026-04-09). HTTP 200, messageId `3EB0F0DD0CB207EF639E1C`, unique test string `AKIOR-W-T05-P412B-20260409T174414Z-g7U31x` physically received on CEO phone. Gateway remained active post-send. Listener remained connected. BLK-003 CLOSED.
- **Active channel decisions:** DEC-031 (Gmail browser-session), DEC-032 (WhatsApp link + send, PROVEN), DEC-033 (Google credential-model purge, browser-only posture)
- **Superseded:** DEC-027
- **Ledger:** LB-001 → ... → LB-012 → LB-013 (Phase 3) → LB-014 (Phase 4 SEND_FAILED) → LB-015 (Phase 4.5 audit) → Phase 4.12B (VERIFIED SUCCESS)
- **OPS-CRED-01 / I9-OPS:** ABANDONED / SUPERSEDED by DEC-033. No off-system credential artifacts. No credential-provisioning tracks active.
- **Next major step:** Future Google work must use browser-only inside-system auth (DEC-033). No credential entry. No credential files. Browser-facing shells preserved.

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
