<div align="center">

# handoff/

### *The Confirmation Architecture — No Silent Transfers of Authority*

[![Function](https://img.shields.io/badge/FUNCTION-Handoff_%26_Acknowledgment-2d6a4f?style=for-the-badge&labelColor=0d1117)]()
[![Build Step](https://img.shields.io/badge/BUILD_STEP-4_of_7-FFD700?style=for-the-badge&labelColor=0d1117)]()
[![Principle](https://img.shields.io/badge/PRINCIPLE-ACK_Required_At_Every_Hop-1a6b9a?style=for-the-badge&labelColor=0d1117)]()
[![Source](https://img.shields.io/badge/SOURCE-FAA_ATC_%7C_TCP_RFC_793_%7C_Mongol_Yam_%7C_NASA-4B0082?style=flat-square&labelColor=0d1117)]()

</div>

---

## WHAT THIS FOLDER IS

Every routing event transfers responsibility from one node to another. That transfer is not complete until the receiving node has confirmed it. A dispatch without acknowledgment is not a handoff — it is a dispatch into the unknown. This folder contains the protocols that make every transfer of routing authority explicit, confirmed, and logged.

`[D]` The FAA Air Traffic Control handoff protocol: when one controller passes an aircraft to the next sector, the receiving controller must explicitly acknowledge the handoff before the originating controller can release authority. No aircraft changes jurisdiction silently. No "it probably received it." Explicit confirmation or the routing event did not complete.

`[D]` TCP/RFC 793: when a TCP segment is transmitted, a copy enters the retransmission queue and a timer starts. When acknowledgment arrives, the copy is deleted from the queue. If acknowledgment does not arrive before the timer expires — retransmit. This is not a performance feature. It is the mechanism that makes reliable communication possible at all.

`[D]` The Mongol Yam relay system: the largest land empire in history was coordinated through a chain of relay stations spanning thousands of miles. Each station received, confirmed, and dispatched forward. No message traveled end-to-end without relay confirmation at every hop. Confirmation was not optional at scale — it was the architecture.

`[R]` These three systems — air traffic, internet, empire — converge on the same architectural requirement: confirmation at every transfer point, not just at final delivery. THALAMUS implements this principle for every cross-repo dispatch.

---

## WHAT LIVES HERE

| File | Purpose | Build Status |
|------|---------|-------------|
| `HANDOFF-PROTOCOL.md` | Full specification for transfer of routing authority | Step 4 |
| `ACK-SPEC.md` | Acknowledgment format, timeout thresholds, retransmit rules per repo | Step 4 |
| `RELAY-LOG.md` | Append-only record of all handoffs and confirmations | Step 4 |
| `README.md` | This file | Complete |

---

## THE HANDOFF SEQUENCE

```
THALAMUS dispatches to destination repo
    ↓
Dispatch payload sent with session timestamp + routing metadata
    ↓
THALAMUS enters PENDING_ACK state (see states/)
    ↓
Timer starts — timeout threshold per ACK-SPEC.md
    ↓
    ACK RECEIVED before timeout
    → THALAMUS enters ACKNOWLEDGED state
    → Handoff record logged in RELAY-LOG.md
    → Routing event continues to ESTABLISHED
    ↓
    OR: No ACK before timeout
    → RETRANSMIT triggered (TCP principle)
    → Second dispatch with retransmit flag
    → If second ACK fails → FAILED state, architect notification
```

---

## RELAY-LOG — THE IMMUTABLE RECORD

`RELAY-LOG.md` is append-only. Every handoff event writes one record. No record is ever edited or deleted. The Talmud principle applies: every routing decision is preserved in full, including alternative paths not taken.

What each record contains: session timestamp, source, destination, routing class, severity, ACK status, elapsed time to ACK, and the session timestamp of the dispatching THALAMUS response.

This is the audit trail that makes THALAMUS credible — not just operational.

---

## DESIGN PRINCIPLES EMBEDDED HERE

- **Explicit confirmation before release** — FAA ATC. No silent handoffs.
- **Retransmit on timeout** — TCP. No silent failures. Unacknowledged events are retransmitted.
- **Relay confirmation at every hop** — Yam. Not just at final delivery — at every transfer point.
- **Go/No-Go** — NASA. Any node can call a halt. HELD in AMYGDALA stops the handoff chain entirely.
- **Immutable record** — RELAY-LOG. The history of every handoff is permanent and uneditable.

---

*handoff/ — THALAMUS · AION Brain Architecture*
*Part of: Sheldon K. Salmon & ALBEDO · March 2026*

