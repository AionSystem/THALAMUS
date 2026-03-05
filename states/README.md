<div align="center">

# states/

### *The Lifecycle Layer — Routing Events Have States, Not Just Destinations*

[![Function](https://img.shields.io/badge/FUNCTION-Routing_State_Machine-16213e?style=for-the-badge&labelColor=0d1117)]()
[![Build Step](https://img.shields.io/badge/BUILD_STEP-3_of_7-FFD700?style=for-the-badge&labelColor=0d1117)]()
[![Principle](https://img.shields.io/badge/PRINCIPLE-Unnamed_States_Are_Failure_Modes-e94560?style=for-the-badge&labelColor=0d1117)]()
[![Source](https://img.shields.io/badge/SOURCE-TCP_%2F_RFC_793_%7C_HICS_%7C_NRC_EOP-4B0082?style=flat-square&labelColor=0d1117)]()

</div>

---

## WHAT THIS FOLDER IS

A routing event is not a single moment — it is a lifecycle. Input arrives, gets classified, gets routed, gets acknowledged, completes, and closes. Each stage is a named state. An event that moves between stages without naming them is operating in the gap between states — and gaps are where failures live undetected.

`[D]` TCP/IP established the canonical state machine for network communication: LISTEN → SYN-SENT → SYN-RECEIVED → ESTABLISHED → FIN-WAIT → CLOSE-WAIT → CLOSING → LAST-ACK → CLOSED. Every TCP connection is always in one of these named states. Nothing is ever "in between." This is not bureaucracy — it is the mechanism that makes the internet reliable. An unnamed state is not a neutral condition; it is an unmonitored condition.

`[R]` The THALAMUS state machine applies this principle to routing events across the brain architecture. Every cross-repo dispatch is always in a named state. When a state cannot be determined — that is itself a named state: UNKNOWN, which triggers an alert. Nothing in THALAMUS is allowed to be in an unnamed condition.

---

## WHAT LIVES HERE

| File | Purpose | Build Status |
|------|---------|-------------|
| `STATE-MACHINE.md` | Full lifecycle specification — all named states and transitions | Step 3 |
| `STATE-REGISTRY.md` | Live log of active routing states — append-only during session | Step 3 |
| `TRANSITION-RULES.md` | What triggers each transition, what blocks it, what constitutes a failed state | Step 3 |
| `README.md` | This file | Complete |

---

## THE THALAMUS STATE MACHINE

```
RECEIVED       ← Input arrives at THALAMUS
    ↓
CLASSIFYING    ← Classification vector being assembled
    ↓
CLASSIFIED     ← Vector complete — ready for routing decision
    ↓
ROUTING        ← Decision tree executing
    ↓
DISPATCHED     ← Sent to destination repo
    ↓
PENDING_ACK    ← Waiting for acknowledgment from destination
    ↓
ACKNOWLEDGED   ← Destination confirmed receipt
    ↓
ESTABLISHED    ← Routing event is active and processing
    ↓
CLOSING        ← Routing event completing
    ↓
CLOSED         ← Complete — logged to RELAY-LOG
    ↓
FAILED         ← Any unrecoverable error — logged, architect notified
```

**Special states:**
- `HELD` — AMYGDALA issued a hold. Nothing proceeds until cleared.
- `RETRANSMIT` — ACK not received within timeout. TCP principle: retransmit.
- `ABNORMAL` — Normal state machine suspended. See `abnormal/ABNORMAL-PROTOCOL.md`.
- `UNKNOWN` — State cannot be determined. Treated as FAILED until proven otherwise.

---

## DESIGN PRINCIPLES EMBEDDED HERE

- **Named states only** — TCP state machine. Unnamed states are failure modes by definition.
- **Closed-loop feedback** — HICS. Routing evaluation returns to THALAMUS. Open loops drift.
- **HELD is harder to exit than enter** — Sanhedrin. Easier to stop than to condemn; easier to be held than to be cleared. Asymmetric thresholds protect against premature clearance.
- **UNKNOWN is FAILED** — NRC EOP. If the system cannot determine its own state, treat it as a failed state until determined otherwise.

---

*states/ — THALAMUS · AION Brain Architecture*
*Part of: Sheldon K. Salmon & ALBEDO · March 2026*

