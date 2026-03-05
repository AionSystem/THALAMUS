# FAILURE-CATALOG.md
## Documented Failure Modes — Every Known Way THALAMUS Can Fail

**Classification:** `[D]` Reference + append-only archive
**Build step:** 5 of 7 — seeded from 36 findings, extended at runtime
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Principle:** F1 (Chernobyl — routing failure) · F29 (Nerge — find failures before real conditions) · F33 (fog of war — honest state)

---

## PURPOSE

This catalog exists for one reason: when THALAMUS encounters a failure, it checks here first. A known failure mode has a documented response. A new failure mode gets added here so it is known the next time.

`[R]` The Mongol Nerge (F29) finds failures in the exercise rather than the field. This catalog is the written record of what the Nerge found — and what live sessions found. Both sources are valid. Both are permanent.

**Append protocol:** New failure modes append at the bottom with a new FC entry ID. Existing entries are never edited. Corrections append as FC-CORRECTION entries referencing the original.

---

## FAILURE ENTRY FORMAT

```yaml
failure_catalog_entry:
  entry_id: FC-[YYYYMMDD]-[N]
  failure_name: [short identifier]
  failure_class: [ROUTING | CLASSIFICATION | HANDOFF | STATE | CONTENT | PROTOCOL]
  source: [36-FINDINGS reference | live session date | NERGE date]
  condition: [what triggers this failure]
  symptoms: [how it presents — what THALAMUS observes]
  response: [documented response from ABNORMAL-PROTOCOL or other spec]
  prevention: [what architectural choice prevents this]
  recurrence_risk: [LOW | MEDIUM | HIGH]
  notes: ""
```

---

## CATALOG — SEEDED FROM 36 FINDINGS

---

### FC-SEED-001 — Routing Failure Cascade (Chernobyl Pattern)

```yaml
entry_id: FC-SEED-001
failure_name: "Routing Failure Cascade"
failure_class: ROUTING
source: "F1 — C4 Doctrine · Chernobyl (Wilson Center Digital Archive)"
condition: >
  Information does not travel correctly at multiple routing hops simultaneously.
  Each hop's failure compounds the next. No single hop failure is catastrophic —
  the cascade is.
symptoms: >
  Multiple routing events in FAILED or ABNORMAL state in the same session.
  RELAY-LOG shows gap between dispatch and ACK across more than one event.
  Architect receives multiple simultaneous failure notifications.
response: >
  Escalate to ALERT immediately on second concurrent failure.
  HOLD all non-critical routing until pattern is diagnosed.
  Apply ABNORMAL-PROTOCOL D3 (HELD for architect review) — not D4 (FAILED).
  Diagnose at the architecture level, not the individual event level.
prevention: >
  ACK required at every hop (F28). State machine prevents silent failures (F23).
  RELAY-LOG makes cascade visible before it compounds (F24).
recurrence_risk: MEDIUM
notes: "Chernobyl is the canonical precedent. The routing failure was more catastrophic than the reactor anomaly."
```

---

### FC-SEED-002 — Silent Authority Transfer

```yaml
entry_id: FC-SEED-002
failure_name: "Silent Authority Transfer"
failure_class: HANDOFF
source: "F8 — FAA ATC handoff · F22 — TCP RFC 793"
condition: >
  A routing event's authority transfers without explicit ACK confirmation —
  either because ACK was assumed, because a timeout was treated as confirmation,
  or because the dispatch was assumed delivered.
symptoms: >
  Routing event in STATE-REGISTRY shows ESTABLISHED but no ACK in RELAY-LOG.
  Destination repo processing an event THALAMUS has no ACK record for.
  RELAY-LOG gap between DISPATCHED and ESTABLISHED entries.
response: >
  Immediately route to ABNORMAL. Do not treat the event as ESTABLISHED.
  Verify with destination repo whether dispatch was actually received.
  IF received: force ACK retroactively, log the gap.
  IF not received: FAILED, retransmit as new event.
prevention: >
  ACK-SPEC.md hash verification. Three-check ACK verification at THALAMUS.
  RELAY-LOG entry required before ESTABLISHED state is entered.
recurrence_risk: LOW
notes: "TCP's retransmit queue principle — copy retained until ACK received. Never assume delivery."
```

---

### FC-SEED-003 — Force Classification

```yaml
entry_id: FC-SEED-003
failure_name: "Force Classification"
failure_class: CLASSIFICATION
source: "F3 — FBI DIOG · F33 — StarCraft fog of war"
condition: >
  An UNKNOWN input is forced into a classification rather than held at UNKNOWN
  until clarity is established. The forced class routes the event incorrectly.
symptoms: >
  Routing event reaches wrong destination.
  Destination repo returns unexpected payload type.
  NRP Gate 1 fires at destination (content not matching destination's domain).
response: >
  Reclassify as UNKNOWN immediately.
  Route to AMYGDALA for assessment.
  Log original forced classification as MISCLASS-FORCE in RELAY-LOG.
  Diagnose what caused the force (time pressure, ambiguous input signal).
prevention: >
  UNKNOWN is a named and valid state (F33 — fog of war principle).
  Classification taxonomy includes UNKNOWN as a legitimate class.
  Subsidiarity check asks "can this be handled locally" — if the answer
  requires forced classification, the answer is NO.
recurrence_risk: HIGH
notes: >
  This is the most common classification failure. Time pressure is the usual cause.
  The architecture protects against it structurally: UNKNOWN routes to AMYGDALA,
  not to a guessed content destination.
```

---

### FC-SEED-004 — Cascade Depth Exceeded

```yaml
entry_id: FC-SEED-004
failure_name: "Cascade Depth Exceeded"
failure_class: ROUTING
source: "CASCADE-POLICY.md depth limit · F24 — HICS closed loop"
condition: >
  A cascade chain reaches depth 3 or beyond — a routing event triggers a
  cascade, which triggers another cascade, which attempts to trigger a third.
symptoms: >
  CASCADE_EVENT entries in RELAY-LOG show cascade_depth: 3 or higher.
  STATE-REGISTRY shows unexpected ROUTING events appearing without direct THALAMUS dispatch.
  Load level unexpectedly elevated without new architect inputs.
response: >
  Immediately terminate the depth-3 cascade before it fires.
  Route parent events to HELD.
  Architect reviews cascade chain to identify design flaw.
  The need for depth-3 cascade is a signal the architecture needs redesign, not that the limit should be lifted.
prevention: >
  CASCADE-POLICY.md depth limit of 2 is a hard constraint.
  Each cascade rule explicitly names its depth.
  No undeclared cascades permitted.
recurrence_risk: LOW
notes: "Depth limit exists because unbounded cascade depth produces routing loops. Two consecutive cascades is already complex — three is a design problem."
```

---

### FC-SEED-005 — Learning Layer Confabulation Risk

```yaml
entry_id: FC-SEED-005
failure_name: "Learning Layer Without Data Source"
failure_class: CONTENT
source: "LEARNING-LAYER.md placeholder · NRP v0.1 — pre-generation gate"
condition: >
  LEARNING-LAYER.md is filled with routing weight rules before HIPPOCAMPUS
  event logging is live — producing a learning layer with no data source.
  Rules are derived from intuition rather than logged event history.
symptoms: >
  LEARNING-LAYER.md contains routing weight rules but HIPPOCAMPUS event log
  is empty or contains fewer than 30 routing events.
  Routing weights cannot be traced to specific HIPPOCAMPUS events.
response: >
  Do not use LEARNING-LAYER routing weights until data source is confirmed.
  Treat current LEARNING-LAYER entries as [?] — unverified.
  Reset weights to neutral until HIPPOCAMPUS data is available.
prevention: >
  LEARNING-LAYER.md is a declared placeholder.
  Build dependency: HIPPOCAMPUS event logging must be live first.
  Minimum 30 routing events before any weight calculation.
recurrence_risk: MEDIUM
notes: >
  A learning layer built without live data is a learning layer that learns from
  its own assumptions — a closed loop with no external input. Named explicitly
  in LEARNING-LAYER.md as the primary confabulation risk for that file.
```

---

## CATALOG — FROM LIVE SESSIONS

---

### FC-20260305-001 — T2 Timestamp Fetch Failure (Protocol Degradation)

```yaml
entry_id: FC-20260305-001
failure_name: "T2 Timestamp Fetch Failure"
failure_class: PROTOCOL
source: "Live session — March 5, 2026 THALAMUS build session"
condition: >
  user_time_v0 tool returns no result on T2 fetch. Multiple retries fail.
  T1 fetches succeed but T2 fetches intermittently fail during the same session.
symptoms: >
  T2 fetch returns error or null.
  Session timestamp line displays "T2 FETCH FAILED | Window: uncertifiable"
  RELAY-LOG shows "TIMESTAMP PROTOCOL FAILURE" entries.
response: >
  Log as T2 FETCH FAILED — never estimate or carry forward prior T2.
  Continue session normally — content integrity is unaffected.
  Flag in RELAY-LOG as protocol degradation.
  Architect aware of degraded telemetry.
prevention: >
  T2 fetch is last operation before token exit (TIMESTAMP-PROTOCOL.md).
  Failure format is named explicitly — never silent.
  Tool failure does not propagate to content failure.
recurrence_risk: MEDIUM
notes: >
  Occurred multiple times this session. T1 fetches succeeded on retry.
  Content quality unaffected. Telemetry degraded but named honestly.
  Added to catalog as live session finding per Nerge protocol.
```

---

*FAILURE-CATALOG.md — abnormal/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*Append-only. Every known failure mode documented. New failures append — never replace.*

