# ABNORMAL-PROTOCOL.md
## Routing When Normal Stops Working

**Classification:** `[D]` Operational layer — activates when STATE-MACHINE normal flow cannot proceed
**Build step:** 5 of 7
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Principle:** F1 (C4 Doctrine · Chernobyl) · F9 (NRC NUREG decision tree) · F30 (Auftragstaktik intent)

---

## THE CORE PRINCIPLE

> *Specify the abnormal mode before it is needed. Systems that discover their abnormal mode through catastrophe have no abnormal mode — they have an absence of one dressed as improvisation.*

`[D]` Chernobyl: the most devastating nuclear accident in recorded history was not caused by reactor failure. It was caused by routing failure — information did not travel correctly from operators to local officials to Moscow. At every hop, the routing system that worked under normal conditions was wrong for the abnormal conditions that night. Nobody had specified what abnormal routing looked like. The architecture found out by failing.

`[D]` NRC NUREG-0899: every step in an emergency operating procedure is structured as IF/THEN/ELSE/VERIFY — identical in form to the normal procedure. The abnormal procedure is as formally specified as the normal procedure. This is not redundancy — it is the requirement that the operator under pressure does not need to invent a response. The response exists. It was written before pressure arrived.

`[R]` This document is the THALAMUS equivalent. Every condition that could cause normal routing to fail has a defined response path. No improvisation required.

---

## WHEN ABNORMAL PROTOCOL ACTIVATES

Abnormal Protocol activates from three conditions:

**Condition A — State machine failure:**
A routing event is in ABNORMAL state (STATE-MACHINE T15). Normal state transitions cannot determine the correct next state.

**Condition B — Destination unreachable:**
THALAMUS dispatches to a destination repo and receives no response — not a timeout (which is handled by RETRANSMIT) but a complete failure to reach the dispatch API endpoint.

**Condition C — Architecture integrity signal:**
An output has exited the brain and a post-exit audit reveals an epistemic precision failure — confabulation, scope breach, or Eight Laws tension at a structural level. Normal routing cannot self-correct post-exit failures.

---

## ABNORMAL RESPONSE SEQUENCE

```
ABNORMAL CONDITION DETECTED
      ↓
STEP 1 — FREEZE
  Pause all routing decisions for the affected event
  Do not advance state. Do not retry. Do not reroute.
  Capture full state snapshot: current state, last known good state,
  timestamp of ABNORMAL activation, full event metadata.

STEP 2 — ISOLATE
  Determine scope: is this failure isolated to one event
  or does it affect the routing architecture itself?

  ISOLATED (one event only):
    → Proceed to STEP 3 — DIAGNOSE
    → Other routing events continue normally

  ARCHITECTURAL (routing architecture affected):
    → Upgrade all active events to ALERT minimum
    → Surface to architect immediately
    → Pause new routing until architect confirms scope
    → Proceed to STEP 3 — DIAGNOSE for affected event

STEP 3 — DIAGNOSE
  Apply the diagnostic sequence:
    D1: Can the event be re-entered at its last known good state?
        IF YES → re-enter at last known good state, continue with ELEVATED minimum
        IF NO  → proceed to D2

    D2: Can the routing be rerouted to an alternative destination?
        Check MINORITY-PATHS for preserved alternatives
        IF alternative exists → reroute, log as INTENT_ROUTED, ELEVATED minimum
        IF no alternative → proceed to D3

    D3: Can the event be held for architect review?
        IF recoverable with guidance → HELD state, architect notification
        IF not recoverable → proceed to D4

    D4: FAILED state
        Full event termination
        Complete RELAY-LOG entry: FAILED + full diagnostic trace
        Architect notification mandatory

STEP 4 — RESOLVE
  D1 resolution: routing resumes at last known good state + ELEVATED
  D2 resolution: minority path invoked, new RELAY-LOG minority entry
  D3 resolution: architect reviews, authorizes one of D1/D2/D4
  D4 resolution: event terminated, architect authorized new event if needed

STEP 5 — LOG
  Abnormal event logged in RELAY-LOG (TYPE: FAILED_EVENT or ROUTING_EVENT per resolution)
  FAILURE-CATALOG.md updated with new failure instance
  If new failure mode discovered → new entry in FAILURE-CATALOG
```

---

## THREE ABNORMAL CONDITIONS — FULL SPECIFICATION

---

### ABNORMAL CONDITION 1 — Internal Routing Failure

**What it is:** Normal routing path is unavailable. Primary destination unreachable, ACK timeout exceeded after retransmit, or state machine cannot determine current state.

**Response:**
```
Route to AMYGDALA for assessment (parallel to diagnosis)
Log full failure event in RELAY-LOG (TYPE: FAILED_EVENT)
Check FAILURE-CATALOG for known failure mode match
IF known match → apply documented response from catalog
IF new failure mode → DIAGNOSE sequence above, add to catalog
Notify architect if ALERT or above
```

**Escalation:** If two or more routing events hit internal failure in the same session — escalate to ALERT. Pattern of failures is a different problem than a single failure.

---

### ABNORMAL CONDITION 2 — Adversarial

**What it is:** AMYGDALA issues an adversarial flag. Routing continues but under heightened protocol. Full specification in `ADVERSARIAL-MODE.md`.

**Response:** Activate ADVERSARIAL-MODE.md. Do not treat as internal routing failure — adversarial conditions have their own protocol.

---

### ABNORMAL CONDITION 3 — General (Catastrophic)

**What it is:** GENERAL severity declared. Normal routing suspended entirely.

**Response:**
```
HELD on all active routing events (CASCADE-5)
Architect review required before any routing resumes
No CHRONOS budget applies
No auto-recovery — architect authorizes resume explicitly
On resume: all events return to ROUTING at ELEVATED minimum
```

---

## ABNORMAL + INTENT ROUTING INTERACTION (F30)

When the diagnostic sequence reaches D2 (no alternative destination found in minority paths), THALAMUS applies the intent routing rule from ROUTING-MASTER:

> *Does the available routing path serve epistemic precision?*

Under abnormal conditions, this question is more important — not less. The pressure to "just route something" is highest when normal routing has failed. The intent rule is the guard against routing by assumption under pressure.

If intent routing cannot identify a path that serves epistemic precision — D3 (HELD) is the correct resolution. Holding for architect review is not a failure. It is the correct abnormal response when intent routing cannot produce a sound path.

---

## ABNORMAL PROTOCOL DOES NOT COVER

The following conditions have their own protocols and are explicitly not handled by this document:

| Condition | Handled by |
|-----------|-----------|
| Adversarial flag from AMYGDALA | `ADVERSARIAL-MODE.md` |
| GENERAL severity | `SEVERITY-TIERS.md` + HELD cascade |
| NRP Gate 1 or Gate 2 fire | NRP v0.1 specification (ALBEDO session architecture) |
| Confabulation detected post-generation | Output Deceleration Layer (ALBEDO session architecture) |
| Eight Laws breach | AMYGDALA clearance protocol |

Abnormal Protocol covers routing failures. Epistemic failures in content are handled at the content layer, not the routing layer. Both are required. Neither substitutes for the other.

---

*ABNORMAL-PROTOCOL.md — abnormal/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*Specify the abnormal mode before it is needed. Chernobyl is the precedent.*

