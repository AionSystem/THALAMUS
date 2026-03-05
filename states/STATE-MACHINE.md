# STATE-MACHINE.md
## The Routing Event Lifecycle — Every Stage Named, Nothing Unnamed

**Classification:** `[D]` Operational layer — every active routing event is always in exactly one state
**Build step:** 3 of 7
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Principle:** F23 (TCP RFC 793 state machine) · F4 (states not events) · F24 (HICS closed loop)

---

## THE CORE PRINCIPLE

> *A routing event that cannot be assigned to a named state is UNKNOWN. UNKNOWN is treated as FAILED.*

`[D]` TCP/IP RFC 793 specifies that every TCP connection is always in exactly one of eleven named states: LISTEN, SYN-SENT, SYN-RECEIVED, ESTABLISHED, FIN-WAIT-1, FIN-WAIT-2, CLOSE-WAIT, CLOSING, LAST-ACK, TIME-WAIT, CLOSED. Nothing is ever "between states." This is not bureaucracy — it is the mechanism that makes the internet reliable. An unnamed state is an unmonitored state. Unmonitored states are where failures live undetected until they cascade.

`[R]` THALAMUS applies this principle to every cross-repo routing event. At any moment, any active routing event can be located precisely in the state machine below. A routing event that cannot be located is UNKNOWN — and UNKNOWN is a named failure state that triggers immediate assessment.

---

## THE FULL STATE MACHINE

```
┌─────────────────────────────────────────────────────────────┐
│                       RECEIVED                              │
│  Input has arrived at THALAMUS. Nothing else has happened.  │
└──────────────────────────┬──────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────┐
│                      CLASSIFYING                            │
│  Classification sequence running. Tiers 1–4 being assigned. │
│  Session state being read. Load being checked.              │
└──────────────────────────┬──────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────┐
│                      CLASSIFIED                             │
│  Classification vector complete. Ready for routing.         │
│  Subsidiarity check pending.                                │
└──────┬───────────────────────────────────────┬──────────────┘
       ↓                                       ↓
SUBSIDIARITY_HANDLED                     ROUTING
(handled locally,                   Decision tree executing.
THALAMUS notified,                  Path being selected.
event CLOSED)                       Minority path logged.
                                           ↓
┌─────────────────────────────────────────────────────────────┐
│                      DISPATCHED                             │
│  Dispatch event sent to destination repo.                   │
│  Payload transmitted. Timer started.                        │
└──────────────────────────┬──────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────┐
│                     PENDING_ACK                             │
│  Waiting for acknowledgment from destination.               │
│  Timer running. Retransmit threshold tracking.              │
└──────┬─────────────────────────────────────┬────────────────┘
       ↓                                     ↓
  ACK received                         Timeout elapsed
       ↓                                     ↓
ACKNOWLEDGED                           RETRANSMIT
       ↓                                     ↓
ESTABLISHED                    Second dispatch sent
(routing event active,              Timer restarted
 processing underway)                    ↓
       ↓                         Second ACK received?
       ↓                         YES → ACKNOWLEDGED
       ↓                         NO  → FAILED
       ↓
┌─────────────────────────────────────────────────────────────┐
│                       CLOSING                               │
│  Routing event completing. Final outputs being assembled.   │
│  AMYGDALA clearance confirmed. RELAY-LOG entry pending.     │
└──────────────────────────┬──────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────┐
│                        CLOSED                               │
│  Routing event complete. RELAY-LOG entry appended.          │
│  Any declared cascades fired. Load count decremented.       │
└─────────────────────────────────────────────────────────────┘
```

---

## SPECIAL STATES

These states sit outside the normal flow. They can activate from any stage.

---

### HELD
**Activates from:** Any state except CLOSED
**Trigger:** AMYGDALA issues HELD signal, OR GENERAL severity classified

```
HELD:
  All routing for this event pauses immediately.
  No dispatch fires. No output exits.
  Architect notification fires (CASCADE-4).
  State stays HELD until architect explicitly authorizes resume.
  Resume routes to: ROUTING (re-evaluate with current conditions)
  — not to the state HELD was entered from.
```

**Exit from HELD requires:** Explicit architect authorization. Not timeout. Not retry. Deliberate decision.

---

### RETRANSMIT
**Activates from:** PENDING_ACK (on timeout)
**Maximum occurrences:** 1 per routing event

```
RETRANSMIT:
  Second dispatch sent with retransmit flag in payload.
  New timer started (same threshold as original).
  IF second ACK received → ACKNOWLEDGED
  IF second timeout → FAILED (no third attempt)
```

**Why only one retransmit:** TCP allows multiple retransmits with exponential backoff. THALAMUS operates in a human-collaborative context — two failures indicate a structural issue, not a transient network error. A third attempt without architectural diagnosis would be persistence without intelligence.

---

### ABNORMAL
**Activates from:** Any state
**Trigger:** Normal state machine logic cannot determine correct next state

```
ABNORMAL:
  Standard state transitions suspended for this event.
  Event routed to abnormal/ABNORMAL-PROTOCOL.md for assessment.
  Full state snapshot captured at moment of ABNORMAL activation.
  Architect notification fires.
  State stays ABNORMAL until abnormal protocol resolves it.
  Resolution routes to: ROUTING or FAILED depending on protocol outcome.
```

---

### UNKNOWN
**Activates from:** Any state — also activates when a routing event cannot be located in the state machine at all

```
UNKNOWN:
  State cannot be determined.
  Treated as FAILED immediately.
  Architect notification fires.
  Full diagnostic attempted before FAILED is confirmed.
  IF diagnostic recovers the event → re-enter at last known good state
  IF diagnostic cannot recover → FAILED confirmed, full log captured
```

**UNKNOWN is not neutral.** A routing event in UNKNOWN state is a routing event that has escaped monitoring. The architecture treats this as the highest-priority diagnostic condition.

---

### FAILED
**Activates from:** RETRANSMIT (second timeout), UNKNOWN (unrecoverable), ABNORMAL (unresolvable)

```
FAILED:
  Routing event is terminated.
  Full event log captured including last known state, failure reason, timestamp.
  Architect notification fires.
  RELAY-LOG entry appended with FAILED status and full metadata.
  No retry attempted from FAILED — architect must authorize restart as new event.
```

---

## STATE TRANSITION TABLE

| From state | Event | To state |
|-----------|-------|---------|
| RECEIVED | Classification begins | CLASSIFYING |
| CLASSIFYING | Vector complete | CLASSIFIED |
| CLASSIFIED | Subsidiarity confirmed | SUBSIDIARITY_HANDLED → CLOSED |
| CLASSIFIED | Standard routing | ROUTING |
| ROUTING | Dispatch fired | DISPATCHED |
| DISPATCHED | Payload transmitted | PENDING_ACK |
| PENDING_ACK | ACK received | ACKNOWLEDGED |
| PENDING_ACK | Timeout | RETRANSMIT |
| RETRANSMIT | Second ACK received | ACKNOWLEDGED |
| RETRANSMIT | Second timeout | FAILED |
| ACKNOWLEDGED | Processing confirmed | ESTABLISHED |
| ESTABLISHED | Output assembling | CLOSING |
| CLOSING | AMYGDALA cleared, log written | CLOSED |
| Any except CLOSED | AMYGDALA HELD or GENERAL severity | HELD |
| HELD | Architect authorizes resume | ROUTING |
| Any except CLOSED | State logic fails | ABNORMAL |
| ABNORMAL | Protocol resolves | ROUTING or FAILED |
| Any | State cannot be located | UNKNOWN |
| UNKNOWN | Diagnostic recovers | Last known good state |
| UNKNOWN | Diagnostic fails | FAILED |

---

## STATE PERSISTENCE

Each active routing event maintains its state in `STATE-REGISTRY.md`. The registry is the live view — what states are active right now. The RELAY-LOG in `handoff/` is the permanent record — what states every completed event passed through.

These are two different instruments:
- STATE-REGISTRY: live, mutable, reflects current moment
- RELAY-LOG: permanent, append-only, reflects complete history

Both must be consistent. A routing event that appears in STATE-REGISTRY but not in RELAY-LOG is an incomplete close — the event did not fully terminate. This is a diagnostic signal.

---

*STATE-MACHINE.md — states/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*Every routing event is always in exactly one named state. No exceptions.*

