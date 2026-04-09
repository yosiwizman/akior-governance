# 02_ROADMAP.md — Product Roadmap Snapshot
# Last updated: 2026-04-07 (LB-002)

## Active Execution Lane

**Product 1: AKIOR Full** — Personal autonomous AI assistant on AI Desktop

| Step | Status | Notes |
|------|--------|-------|
| Batches A–E (foundation) | Complete | GPU, Git, Ollama, POC |
| Batch F (Jarvis V5 OS on AI Desktop) | Complete | Chat, Voice, Tasks, Contacts, DND all working |
| Batch G (channels, skills, infra) | In progress | WhatsApp send VERIFIED (Phase 4.12B). Google NOT solved. |
| G-T06.D1 (Google refactor planning) | Complete | Plan artifact at `plans/google/G-T06-D1_GOOGLE_WORKSPACE_REFACTOR_PLAN.md` |
| G-T06.D2 (durability + preconditions) | Complete | D1 plan durable, test plan + I9-OPS definition produced, preconditions closed |
| I9-OPS (internal credential provisioning) | Scheduled | Separate bounded CTO task. NOT implementation. |
| G-T06 implementation | After I9-OPS + CTO approval | Bounded refactor per DEC-028 |
| Batch H (Contact Intelligence) | Queued | Contacts Manager, Takeover Mode, DND, Proactive Intelligence |

## Google Direction
- **Active decision:** DEC-028 — fresh Jarvis V5 OS browser-OAuth with AKIOR-managed server-side credentials
- **Superseded:** DEC-027 (OpenClaw GOG — rejected, no Workspace auth capability)
- **Status:** NOT solved. G-T06.D1 plan complete. G-T06.D2 preconditions closed. I9-OPS scheduled. Next: I9-OPS then first bounded implementation slice.

## Secondary Products (not in current execution lane)
- **AKIOR Light** (Product 2) — $500 Mac Mini install. Downstream of Product 1 completion.
- **Software 4 All** (Product 3) — Paperclip orchestration. Downstream of Product 2.
