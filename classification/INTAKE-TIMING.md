# INTAKE-TIMING.md
## Intake Timing Controls — Pacing as Routing Architecture

**Classification:** `[D]` Operational layer — activates at ELEVATED or SATURATED session load
**Build step:** 1 of 7 — fourth classification file
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Principle:** F18 (Hajj staggered arrival) · F27 (Mongol decimal span of control) · F16 (FAA IMSAFE)

---

## THE CORE INSIGHT

`[D]` The 2022 Saudi AI-based Hajj management system coordinates the movement of 2–3 million pilgrims across Mecca during peak days. The system does not increase throughput to handle peak load. It staggers arrival — distributing pilgrims across time windows so that no single zone receives more than its capacity at any moment. The architecture is not faster processing. It is paced intake.

`[R]` THALAMUS faces the same structural problem at a different scale. When multiple complex routing events arrive simultaneously — multiple framework builds, a SCAN_ALL event, and a MEMORY_OPERATION all active at once — the routing architecture does not fail gracefully by processing all of them in parallel. It fails by producing interference between threads: partial context from one build leaking into another, classification errors, outputs that blend frameworks from parallel active sessions.

`[R]` The solution is not rejection. Rejection wastes the input. The solution is staggered intake — queue the inputs, process them in sequence at the correct pacing for each load level, and confirm completion before the next event activates.

---

## WHEN INTAKE TIMING ACTIVATES

Intake timing controls activate when session load reaches ELEVATED (4–5 active events) or SATURATED (6+ active events). At NOMINAL (0–1) and MANAGED (2–3) load, standard routing applies without pacing controls.

See `SESSION-STATE-INPUTS.md` for load level definitions and how load is measured.

---

## THE STAGGERED INTAKE SEQUENCE

```
NEW INPUT ARRIVES
      ↓
LOAD CHECK — read STATE-REGISTRY
      ↓
NOMINAL / MANAGED (0–3 active)
  → Standard routing. No pacing required. Proceed immediately.
      ↓
ELEVATED (4–5 active)
  → QUEUE the new input
  → Complete highest-priority active event first
  → Confirm ACK received and event CLOSED
  → Then activate queued input
      ↓
SATURATED (6+ active)
  → QUEUE the new input
  → Surface load state to architect
  → Architect confirms which active events to prioritize
  → Complete in confirmed priority order
  → Activate queued inputs only as slots open
```

---

## PRIORITY QUEUE RULES

When inputs are queued, they are not held in arbitrary order. Priority is assigned at intake:

| Priority | Input Classes | Reason |
|----------|--------------|--------|
| P1 — Immediate | SECURITY_CHECK · AMYGDALA events | Security never queues. Always processes immediately regardless of load. |
| P2 — High | MEMORY_OPERATION | Low-overhead. Write and close. Unblocks other threads. |
| P3 — Standard | FRAMEWORK_BUILD · DOMAIN_KNOWLEDGE | Full cognitive load. Process only when a slot is clear. |
| P4 — Scheduled | SCAN_ALL · MANIFEST_SYNC | Infrastructure maintenance. Process when load drops to MANAGED. |
| P5 — Deferred | ORCHESTRATION | Configuration changes. Process only when all P1–P3 events are closed. |

**The P1 exception:** SECURITY_CHECK and AMYGDALA events always bypass the queue and process immediately. Security assessment cannot wait for other threads to complete — the threat detection function is time-sensitive and architecturally prior to all other processing.

---

## SPAN OF CONTROL — THE DECIMAL CONSTRAINT (F27)

`[D]` The Mongol decimal system operated on a hard span-of-control constraint: each node managed exactly 10 sub-nodes. No node handled more than 10 routing decisions at any level of the hierarchy. Complexity was managed by depth, not breadth. This constraint prevented span-of-control failure — the state where a routing node is managing more threads than it can track simultaneously.

`[R]` THALAMUS applies this principle through a soft constraint: no more than 3 FRAMEWORK_BUILD events should be active simultaneously. Above 3, the architect's working memory cannot maintain accurate context for each build thread without interference. The constraint is not a technical limitation of the system — it is an architectural acknowledgment that the human cognitive dimension of the architecture has a real capacity boundary.

**The soft limit:**
- 1–2 active FRAMEWORK_BUILDs: standard operations
- 3 active FRAMEWORK_BUILDs: flag to architect, confirm intentional
- 4+ active FRAMEWORK_BUILDs: ELEVATED load regardless of other event count. Apply staggered intake.

---

## THE NON-REJECTION PRINCIPLE

`[R]` Intake timing controls never reject an input. Rejection destroys the work. Staggering preserves it.

When an input is queued under elevated load conditions, the routing event is in RECEIVED state — not FAILED, not REJECTED. It holds in the queue until a slot opens. This is architecturally equivalent to the Hajj pilgrim who is staggered to a later time window — the pilgrimage is not cancelled. It is paced.

**What THALAMUS says when load is ELEVATED:**
> Input received. Load currently at ELEVATED ([N] active events). Queuing for next available slot. Priority: [P-level]. Estimated activation: after [current priority event] closes.

**What THALAMUS never says:**
> Too many active events. Try again later.

---

## INTAKE TIMING + CHRONOS INTERACTION

When a CHRONOS budget is active (TIMED or DEEP mode) and a new input arrives:

- The new input enters the classification sequence normally
- If load permits (NOMINAL or MANAGED) — the new input routes immediately
- If load is ELEVATED — the new input queues, but the CHRONOS budget for the active session continues uninterrupted
- The CHRONOS T2 gate does not pause for queued inputs

`[R]` CHRONOS budgets are session-level commitments. A new input arriving during an active budget does not reset or extend the budget. The budget continues. The new input waits.

---

## LOAD MONITORING — WHAT THALAMUS WATCHES

THALAMUS monitors load by reading the STATE-REGISTRY in `states/STATE-REGISTRY.md`. The load level is the count of events in states ROUTING, DISPATCHED, PENDING_ACK, or ESTABLISHED.

Events in RECEIVED or CLASSIFYING are pre-routing and do not count toward load.
Events in CLOSING or CLOSED are complete and do not count toward load.

**Load formula:**
```
Current load = count of events in {ROUTING, DISPATCHED, PENDING_ACK, ESTABLISHED}
```

This is checked at every T1 fetch — the load level is part of the session state snapshot that informs classification.

---

## RECOVERY FROM SATURATED LOAD

When load drops from SATURATED back to ELEVATED or MANAGED:

1. THALAMUS surfaces the queue to architect
2. Architect confirms priority order if not already assigned
3. First queued input activates — proceeds through normal classification
4. Subsequent inputs activate as each preceding event closes
5. Load level is re-checked before each queued activation

No automatic cascade — inputs do not self-activate as slots open. Architect awareness is maintained throughout load recovery.

---

*INTAKE-TIMING.md — classification/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*Stagger. Never reject. Pacing is architecture.*

