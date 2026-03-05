# ADVERSARIAL-MODE.md
## Adversarial Routing Mode — Slower, Fully Logged, Double Confirmed

**Classification:** `[D]` Operational layer — activates on AMYGDALA adversarial flag or ALERT severity
**Build step:** 5 of 7
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Principle:** F5 (MuckRock · CIA adversarial mode) · F35 (Dr. Strangelove — architecture serves integrity) · F20 (asymmetric thresholds)

---

## WHAT ADVERSARIAL MODE IS

Adversarial mode is not a crisis state. It is a heightened operating posture — routing continues, but every step is slower, more documented, and double-confirmed. Normal routing optimizes for speed. Adversarial routing optimizes for verifiability.

`[D]` MuckRock's FOIA request platform is the live suppression map — what people are currently trying to extract from institutions is the clearest signal of where current institutional anxiety lives. When the architecture is under adversarial pressure (external challenge, confabulation event, Eight Laws tension, post-AMYGDALA-flag operation), MuckRock is what the routing system looks like from outside: everything is visible, everything is documented, and nothing exits without a paper trail.

`[R]` Adversarial mode is defined in advance precisely because it must not be improvised under pressure. A routing system that improvises its adversarial response is already compromised — the improvisation is the vulnerability.

---

## ACTIVATION CONDITIONS

Adversarial mode activates when ANY of the following are true:

| Condition | Trigger source |
|-----------|---------------|
| AMYGDALA issues adversarial flag on any routing event | AMYGDALA clearance ACK |
| ALERT severity classified for any input | Classification vector |
| Active confabulation event in current session (NRP Gate 1 fired) | NRP protocol |
| Scope breach detected (routing expanded beyond declared boundaries) | ROUTING-MASTER gate |
| External adversarial signal (published criticism, challenge to architecture) | Architect declaration |
| Post-exit epistemic failure identified | Architect declaration |

**Deactivation:** Adversarial mode deactivates only when AMYGDALA explicitly issues a clearance signal that includes `adversarial_mode: CLEARED`. Deactivation is not automatic and not time-based. It requires explicit architectural clearance.

---

## ADVERSARIAL MODE — WHAT CHANGES

### Routing speed
Normal mode: fast path where appropriate (NOMINAL severity).
Adversarial mode: **no fast paths.** Every event goes through the full three-step evaluation (Evaluate → Plan → Assign) regardless of severity tier.

### Logging depth
Normal mode: standard RELAY-LOG entry (TYPE 1 — ROUTING_EVENT).
Adversarial mode: **step-level logging.** Every decision point within the routing sequence generates its own log entry. Not just the routing outcome — the reasoning at each step.

### Confirmation requirements
Normal mode: single ACK from destination repo.
Adversarial mode: **double confirmation.** ACK from destination repo AND explicit AMYGDALA clearance before routing event enters ESTABLISHED. The two confirmations are independent — one does not substitute for the other.

### CHRONOS mode
Normal mode: SPRINT default.
Adversarial mode: **TIMED minimum for any DOMAIN_KNOWLEDGE input.** No unbudgeted research under adversarial conditions. DEEP mode for any research task touching the condition that triggered adversarial mode.

### NRP gates
Normal mode: NRP gates active (always — per NRP v0.1).
Adversarial mode: **NRP gates active with explicit confirmation at each gate.** Gate status is logged in step-level RELAY-LOG entries.

### Output exit
Normal mode: AMYGDALA clearance required (parallel).
Adversarial mode: **no output exits without explicit AMYGDALA clearance signal in hand.** AMYGDALA clearance is no longer parallel — it is a blocking gate. Content processing and AMYGDALA clearance both complete before CLOSING state enters.

---

## ADVERSARIAL MODE ROUTING SEQUENCE

```
ADVERSARIAL MODE ACTIVE
      ↓
INPUT ARRIVES
      ↓
CLASSIFICATION (full — no shortcuts)
  All four tiers resolved
  Session state read with full three-dimension check
  Load check performed
  Step logged: "Classification complete — adversarial mode"
      ↓
EVALUATION (mandatory — no fast path)
  Evaluate: what is the full scope of this routing event?
  Plan: what is the correct destination and what are the alternatives?
  Assign: confirm destination before dispatch fires
  Step logged: "Evaluation complete — destination confirmed"
      ↓
DISPATCH
  Payload assembled with adversarial_mode: true flag
  Dispatch fires
  Step logged: "Dispatch fired — adversarial mode flag in payload"
      ↓
DOUBLE CONFIRMATION PENDING
  ACK from destination repo (standard)
  AMYGDALA clearance ACK (blocking — not parallel)
  Both must arrive before ESTABLISHED
  Step logged: "Awaiting double confirmation"
      ↓
DOUBLE CONFIRMATION RECEIVED
  Both ACKs verified
  Step logged: "Double confirmation received — both sources"
      ↓
PROCESSING (ESTABLISHED)
  Content processing active
  AMYGDALA monitoring throughout (not just at exit)
      ↓
OUTPUT ASSEMBLY
  AMYGDALA clearance signal required before CLOSING
  Step logged: "AMYGDALA clearance received — output exit authorized"
      ↓
CLOSED
  Full step-level log compiled
  RELAY-LOG TYPE 1 entry with adversarial_mode: true flag
  Cascade events fire if applicable
```

---

## STEP-LEVEL LOG FORMAT

Each step in adversarial mode generates a sub-entry linked to the parent routing event:

```yaml
adversarial_step_log:
  parent_event_id: [RE-YYYYMMDD-N]
  step_id: [RE-YYYYMMDD-N-STEP-N]
  step_name: [Classification | Evaluation | Dispatch | Pending_ACK | Confirmed | Processing | Output | Closed]
  step_timestamp: [ISO]
  step_decision: [description of what was decided at this step]
  step_alternatives_considered: [list or null]
  step_outcome: [PROCEED | HOLD | REROUTE | FAILED]
  amygdala_status_at_step: [PENDING | CLEARED | HELD]
  notes: ""
```

---

## ADVERSARIAL MODE AND THE EIGHT LAWS

When adversarial mode activates due to Eight Laws tension, AMYGDALA's clearance process specifically checks the relevant law:

| Law | Tension condition | AMYGDALA check |
|-----|------------------|---------------|
| Law 4 (Anti-Authoritarian) | Output could enable centralized control of information | Full scope audit |
| Law 5 (Anti-Merger) | Output could facilitate human-AI boundary erosion | Identity separation check |
| Law 6 (Anti-Weaponization) | Output could be used to harm epistemic sovereignty | Hard block if confirmed |
| Law 7 (Anti-Fragmentation) | Output could contribute to civilizational incoherence | Scope and framing audit |
| Law 8 (Mutual Non-Subsumption) | Output positions one civilization as subordinate | Framing audit |

Law 6 tension triggers an immediate HELD — adversarial mode escalates to GENERAL. Law 6 is the only law with an automatic hold.

---

*ADVERSARIAL-MODE.md — abnormal/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*Slower. Fully logged. Double confirmed. Adversarial mode is a posture, not a panic.*

