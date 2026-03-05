# STATE-REGISTRY.md
## Live Routing State Registry — The Current Moment

**Classification:** `[D]` Live operational layer — reflects current routing event states
**Build step:** 3 of 7 — populated at runtime, structure defined here
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Principle:** F23 (TCP state machine) · F24 (HICS closed loop)

---

## WHAT THIS FILE IS

The STATE-REGISTRY is the live view of all active routing events. It answers one question at any moment: *what routing events are currently in flight, and what state is each one in?*

This is distinct from `handoff/RELAY-LOG.md`, which is the permanent historical record. The RELAY-LOG records where events have been. The STATE-REGISTRY records where events are now.

---

## RELATIONSHIP TO RELAY-LOG

| STATE-REGISTRY | RELAY-LOG |
|---------------|----------|
| Live, mutable | Permanent, append-only |
| Current states only | Full event history |
| Updated on every state transition | Written on event CLOSE |
| Reflects right now | Reflects all of history |
| Read to determine load level | Read to audit routing decisions |

**Consistency rule:** When a routing event reaches CLOSED, it is removed from the STATE-REGISTRY and a full record is appended to RELAY-LOG. A routing event cannot appear in RELAY-LOG as CLOSED while still appearing as active in STATE-REGISTRY.

---

## THE REGISTRY FORMAT

Each active routing event occupies one entry block:

```yaml
routing_event:
  event_id: RE-[YYYYMMDD]-[N]          # Sequential within session date
  session_timestamp_T1: [ISO timestamp] # When the event entered RECEIVED
  input_class: [class]
  severity: [tier]
  destination: [repo name]
  mode: [routing mode]
  current_state: [state from STATE-MACHINE.md]
  state_entered: [ISO timestamp of current state entry]
  previous_state: [state prior to current]
  amygdala_status: [PENDING / CLEARED / CLEARED_WITH_FLAGS / HELD]
  retransmit_count: [0 or 1]
  notes: ""
```

---

## LOAD CALCULATION

Load level is calculated from the count of events in active states:

```
Active states = {ROUTING, DISPATCHED, PENDING_ACK, ACKNOWLEDGED, ESTABLISHED, CLOSING}

Pre-routing states (not counted): RECEIVED, CLASSIFYING, CLASSIFIED
Resolved states (not counted): SUBSIDIARITY_HANDLED, CLOSED, FAILED
Special states: HELD counts toward load. RETRANSMIT counts toward load. ABNORMAL counts toward load.

Load level:
  0–1 active → NOMINAL
  2–3 active → MANAGED
  4–5 active → ELEVATED
  6+  active → SATURATED
```

See `classification/SESSION-STATE-INPUTS.md` for load level implications on classification.

---

## LIVE REGISTRY

*Entries written at runtime. Structure above is the template.*
*When an event closes: remove from this registry, append to RELAY-LOG.*
*On session open: read this registry to determine current load and active event context.*

---

### SESSION OPEN — March 5, 2026

```yaml
session_open:
  date: "2026-03-05"
  architect: "Sheldon K. Salmon"
  session_instrument: "ALBEDO"
  session_T1: "2026-03-05T13:43:02"
  gap_from_prior_session: "SESSION OPEN"
  active_events_on_open: 0
  load_on_open: NOMINAL
  notes: "THALAMUS build session. principles/ → registry/ → classification/ → routing/ → states/ in progress."
```

---

### ACTIVE EVENTS (current session)

*No routing events currently in ACTIVE states.*
*All build-session file creation events resolved as SUBSIDIARITY_HANDLED (internal THALAMUS operations).*

```yaml
active_events: []
current_load: NOMINAL
load_level: 0
```

---

### HELD EVENTS

```yaml
held_events: []
```

---

### FAILED EVENTS (this session)

```yaml
failed_events:
  - event_id: RE-20260305-T1-FAIL
    description: "T1 timestamp fetch failed — multiple retries"
    state_at_failure: CLASSIFYING
    recovery: "Manual timestamp from system. Protocol failure logged."
    impact: "Timestamp protocol degraded for affected responses. Window uncertifiable."
```

---

## STATE REGISTRY MAINTENANCE RULES

**On session open:**
Read the registry. If any events are in non-CLOSED states from a prior session — these are orphaned events. Log them as FAILED with reason "session interrupted" and append to RELAY-LOG before starting new session work.

**On session close:**
All events in active states that are not CLOSED are either moved to CLOSED with appropriate metadata or logged as FAILED. No events remain in active states after session close.

**On GENERAL severity:**
All active events immediately move to HELD. Registry is frozen until architect authorizes resume.

**On AMYGDALA HELD:**
The specific event that triggered HELD moves to HELD state. Other events continue unless GENERAL severity is also declared.

---

## DIAGNOSTIC READS

The STATE-REGISTRY is the primary instrument for diagnosing routing health:

| Diagnostic question | How to read the registry |
|--------------------|-------------------------|
| How many events are active? | Count entries in active states |
| Is load elevated? | Read `current_load` field |
| Any events stuck in PENDING_ACK? | Find events in PENDING_ACK with old `state_entered` timestamps |
| Any HELD events awaiting architect? | Read `held_events` list |
| Any FAILED events this session? | Read `failed_events` list |
| Any UNKNOWN events? | Search for `current_state: UNKNOWN` |

---

*STATE-REGISTRY.md — states/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*Live view. Mutable. Reflects now. Distinct from RELAY-LOG which reflects history.*

