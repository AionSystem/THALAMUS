# TRANSITION-RULES.md
## State Transition Rules — What Moves Events Forward, What Blocks Them

**Classification:** `[D]` Operational layer — the rules governing every state change
**Build step:** 3 of 7
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Principle:** F23 (TCP state machine) · F20 (asymmetric thresholds) · F9 (decision tree)

---

## HOW TO READ THIS DOCUMENT

Each rule specifies:
- **Transition:** From state → To state
- **Trigger:** What causes the transition
- **Blocker:** What prevents the transition from firing
- **Failure condition:** What constitutes a failed transition
- **Logged:** What is recorded in STATE-REGISTRY and RELAY-LOG

---

## NORMAL FLOW TRANSITIONS

---

### T1 — RECEIVED → CLASSIFYING

**Trigger:** Input arrives at THALAMUS and is acknowledged by the session instrument
**Blocker:** Session load at SATURATED — input queues in RECEIVED until load drops
**Failure condition:** Input arrives but classification does not begin within the same response cycle
**Logged:** Event ID assigned, T1 timestamp recorded, `current_state: CLASSIFYING`

---

### T2 — CLASSIFYING → CLASSIFIED

**Trigger:** All four classification tiers resolved (class · severity · destination · mode). Session state check complete.
**Blocker:**
- Tier 1 class cannot be determined → event stays CLASSIFYING until class resolved
- Session state read fails → treat as ELEVATED minimum, proceed to CLASSIFIED with degraded confidence flag
**Failure condition:** Classification produces an incomplete vector (any tier null or undefined)
**Logged:** Full classification vector recorded, `current_state: CLASSIFIED`

---

### T3 — CLASSIFIED → SUBSIDIARITY_HANDLED

**Trigger:** Subsidiarity check confirms the input is entirely within one repo's scope and requires no cross-repo routing
**Blocker:** Input requires output that crosses repo boundaries, or output is deployment-grade
**Failure condition:** Subsidiarity applied incorrectly — event later found to require cross-repo routing
**Logged:** `current_state: SUBSIDIARITY_HANDLED`, reason logged, minimal RELAY-LOG entry
**Note:** SUBSIDIARITY_HANDLED events move directly to CLOSED. No further transitions.

---

### T4 — CLASSIFIED → ROUTING

**Trigger:** Subsidiarity check confirms standard routing required. Decision tree in ROUTING-MASTER begins executing.
**Blocker:** GENERAL severity classified → event moves to HELD, not ROUTING
**Failure condition:** Decision tree cannot produce a routing path (no rule matches, intent routing also fails)
**Logged:** `current_state: ROUTING`, decision tree entry point recorded

---

### T5 — ROUTING → DISPATCHED

**Trigger:** ROUTING-MASTER decision tree produces a destination and dispatch fires via `repository_dispatch`
**Blocker:**
- AMYGDALA HELD signal arrives during ROUTING → event moves to HELD before dispatch
- Architect notification required (ALERT severity) and not yet sent → hold at ROUTING until notification confirmed
**Failure condition:** Dispatch API call fails (network error, auth failure, repo unreachable)
**Logged:** `current_state: DISPATCHED`, destination repo, dispatch event type, payload hash

---

### T6 — DISPATCHED → PENDING_ACK

**Trigger:** Dispatch payload transmitted successfully. Timer starts.
**Blocker:** None — DISPATCHED moves to PENDING_ACK immediately on transmission confirmation
**Failure condition:** Transmission confirmed but timer never starts (instrument failure)
**Logged:** `current_state: PENDING_ACK`, `state_entered` timestamp, timer threshold recorded

---

### T7 — PENDING_ACK → ACKNOWLEDGED

**Trigger:** ACK signal received from destination repo before timeout
**Blocker:** None — ACK received always triggers ACKNOWLEDGED
**Failure condition:** ACK received but payload hash doesn't match dispatch → flag as ACK_MISMATCH, route to ABNORMAL
**Logged:** `current_state: ACKNOWLEDGED`, ACK timestamp, elapsed time from dispatch to ACK

---

### T8 — PENDING_ACK → RETRANSMIT

**Trigger:** Timer expires before ACK received
**Blocker:** None — timeout always triggers RETRANSMIT (first occurrence only)
**Failure condition:** Second timeout fires (see T9)
**Logged:** `current_state: RETRANSMIT`, `retransmit_count: 1`, timeout elapsed recorded

---

### T9 — RETRANSMIT → FAILED

**Trigger:** Second timeout fires after retransmit dispatch
**Blocker:** None — second timeout always triggers FAILED
**Failure condition:** This transition is itself the failure — it terminates the routing event
**Logged:** `current_state: FAILED`, full event metadata, failure reason: `ACK_TIMEOUT_RETRANSMIT`, architect notification fired

---

### T10 — ACKNOWLEDGED → ESTABLISHED

**Trigger:** Destination repo confirms it is actively processing the routed event
**Blocker:** AMYGDALA HELD signal during ACK processing → event moves to HELD
**Failure condition:** ACK received but destination repo does not move to active processing within timeout
**Logged:** `current_state: ESTABLISHED`, processing confirmation timestamp

---

### T11 — ESTABLISHED → CLOSING

**Trigger:** Destination repo signals output assembly complete and ready for final clearance
**Blocker:** AMYGDALA clearance not yet received → event stays ESTABLISHED until clearance arrives
**Failure condition:** CLOSING entered without AMYGDALA clearance confirmed
**Logged:** `current_state: CLOSING`, AMYGDALA clearance status

---

### T12 — CLOSING → CLOSED

**Trigger:** AMYGDALA clearance confirmed AND RELAY-LOG entry written
**Blocker:**
- AMYGDALA returns CLEARED_WITH_FLAGS → resolve flags before CLOSED
- RELAY-LOG write fails → event stays CLOSING until log confirmed
**Failure condition:** CLOSED state entered without RELAY-LOG entry (incomplete termination)
**Logged:** Event removed from STATE-REGISTRY. Full event record appended to RELAY-LOG. Load count decremented.

---

## SPECIAL STATE TRANSITIONS

---

### T13 — Any → HELD

**Trigger (any one sufficient):**
- AMYGDALA issues HELD signal
- GENERAL severity classified at any point in the event lifecycle
- Architect explicitly issues HOLD command

**Blocker:** None — HELD cannot be blocked. It fires immediately.
**From HELD:** Event stays HELD until architect explicitly authorizes resume. Resume routes to ROUTING (not to the state HELD was entered from — conditions may have changed).
**Logged:** `current_state: HELD`, HELD entry timestamp, trigger reason, CASCADE-4 fires (architect notification)

---

### T14 — HELD → ROUTING

**Trigger:** Architect explicitly authorizes resume
**Blocker:**
- AMYGDALA has not cleared the HELD condition → HELD persists
- GENERAL severity still active → HELD persists
**Failure condition:** Resume authorized before HELD condition is resolved at its source
**Logged:** `current_state: ROUTING`, resume authorization timestamp, architect name, HELD duration recorded

---

### T15 — Any → ABNORMAL

**Trigger:** Normal state machine logic cannot determine the correct next state for this event
**Blocker:** None — ABNORMAL fires when state logic fails
**From ABNORMAL:** Routes to `abnormal/ABNORMAL-PROTOCOL.md`. Resolution returns to ROUTING (if recoverable) or FAILED (if not).
**Logged:** `current_state: ABNORMAL`, full state snapshot at moment of ABNORMAL activation, architect notification

---

### T16 — Any → UNKNOWN

**Trigger:** A routing event cannot be located in the STATE-REGISTRY at all, OR the `current_state` field contains a value not in the STATE-MACHINE specification
**Blocker:** None — UNKNOWN fires on detection
**Treatment:** Treated as FAILED immediately. Diagnostic attempted before FAILED confirmed.
**Logged:** UNKNOWN event logged in STATE-REGISTRY. Diagnostic result logged. If recovery: re-enter at last known good state. If not: FAILED.

---

## ASYMMETRIC TRANSITION RULES (F20)

Following the Sanhedrin principle — transitions toward higher severity or more restricted states are faster and easier to trigger than transitions toward lower severity or less restricted states.

| Direction | Threshold |
|-----------|-----------|
| NOMINAL → ELEVATED severity | Any single indicator. Immediate. |
| ELEVATED → ALERT severity | Any single indicator. Immediate. |
| Any → HELD | Any single trigger. Immediate. No override. |
| ALERT → ELEVATED severity | Explicit AMYGDALA clearance required. |
| ELEVATED → NOMINAL severity | Explicit architect confirmation required. |
| HELD → ROUTING | Explicit architect authorization. AMYGDALA clearance. |
| GENERAL → ALERT severity | Full architect review. Root cause addressed. |

The architecture can always increase caution faster than it can reduce it. This is not conservatism — it is structural protection against accidental downgrade.

---

## INVALID TRANSITIONS

The following transitions are explicitly prohibited. If they occur, route to ABNORMAL immediately.

| Prohibited transition | Why prohibited |
|----------------------|---------------|
| CLOSED → any active state | Closed events do not reopen. Create a new event. |
| FAILED → any state except new event | Failed events do not recover. Architect authorizes new event. |
| PENDING_ACK → ESTABLISHED (skipping ACKNOWLEDGED) | ACK is required. Cannot establish without confirmation. |
| ROUTING → ESTABLISHED (skipping DISPATCHED, PENDING_ACK, ACKNOWLEDGED) | Cannot skip the confirmation chain. |
| HELD → CLOSED (skipping ROUTING) | HELD events must be re-evaluated before closing. |

---

*TRANSITION-RULES.md — states/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*Every transition has a trigger, a blocker, a failure condition, and a log entry.*

