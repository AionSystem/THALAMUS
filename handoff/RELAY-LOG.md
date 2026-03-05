# RELAY-LOG.md
## The Permanent Routing Record — Append-Only, Never Edited

**Classification:** `[D]` Permanent archive layer — the immutable history of every routing event
**Build step:** 4 of 7 — populated at runtime, structure defined here
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Principle:** F19 (Talmud — permanent record) · F28 (Mongol Yam — every hop confirmed) · F4 (State Dept — explicit policy)

---

## THE IMMUTABILITY PRINCIPLE

> *Every routing event is permanently recorded. No entry is ever edited after its session. No entry is ever deleted.*

`[D]` The Talmud preserves not only the majority ruling but the dissenting minority opinions — in full, in the same document, permanently. The reasoning: an opinion that was overruled at one time may be the correct ruling when conditions change. Preserving it means the institutional memory is available. Destroying it means rediscovering it from scratch.

`[R]` Every routing decision in THALAMUS carries the same logic. A routing event that was handled one way under certain conditions may need to be handled differently under changed conditions. The permanent record of what was decided, when, why, and what alternatives existed is the data that makes routing evolution deliberate rather than amnesiac.

**Correction protocol:** If a log entry is found to contain an error after the session in which it was written — a correction entry is appended. The original entry is preserved unchanged. The correction references the original by entry ID. The log never misrepresents what happened — and it never pretends corrections are original entries.

---

## ENTRY TYPES

The RELAY-LOG contains seven entry types. Each has its own format below.

---

### TYPE 1 — ROUTING_EVENT (standard close)

Written when a routing event reaches CLOSED state.

```yaml
relay_log_entry:
  entry_id: RL-[YYYYMMDD]-[N]
  entry_type: ROUTING_EVENT
  event_id: [RE-YYYYMMDD-N]
  session_timestamp_T1: [ISO]
  closed_timestamp: [ISO]
  input_class: [class]
  severity: [tier]
  destination: [repo]
  routing_mode: [mode]
  routing_rule_applied: [rule from ROUTING-MASTER]
  amygdala_clearance: CLEARED | CLEARED_WITH_FLAGS
  amygdala_flags: []
  dispatch_to_ack_ms: [elapsed milliseconds]
  retransmit_fired: [false | true]
  cascade_triggered: [false | CASCADE-N]
  minority_path_logged: [false | MP-YYYYMMDD-N]
  full_event_duration_ms: [T1 to CLOSED in milliseconds]
  notes: ""
```

---

### TYPE 2 — FAILED_EVENT

Written when a routing event reaches FAILED state.

```yaml
relay_log_entry:
  entry_id: RL-[YYYYMMDD]-[N]
  entry_type: FAILED_EVENT
  event_id: [RE-YYYYMMDD-N]
  session_timestamp_T1: [ISO]
  failed_timestamp: [ISO]
  last_known_state: [state at failure]
  failure_reason: [ACK_TIMEOUT_RETRANSMIT | DEST_UNREACHABLE | ACK_MISMATCH | other]
  input_class: [class]
  severity: [tier]
  destination: [repo]
  architect_notified: [true | false]
  recovery_attempted: [false | description]
  notes: ""
```

---

### TYPE 3 — HELD_EVENT

Written when a routing event enters HELD state and again when it exits HELD.

```yaml
# Entry on HELD activation:
relay_log_entry:
  entry_id: RL-[YYYYMMDD]-[N]
  entry_type: HELD_EVENT
  subtype: HELD_ACTIVATED
  event_id: [RE-YYYYMMDD-N]
  held_timestamp: [ISO]
  held_trigger: [AMYGDALA | GENERAL_SEVERITY | ARCHITECT]
  held_reason: [description]
  architect_notified: true

# Entry on HELD resolution:
relay_log_entry:
  entry_id: RL-[YYYYMMDD]-[N+1]
  entry_type: HELD_EVENT
  subtype: HELD_RESOLVED
  event_id: [RE-YYYYMMDD-N]
  resolved_timestamp: [ISO]
  held_duration_ms: [resolved minus held timestamp]
  resolution: [RESUMED | FAILED | ARCHITECT_CLOSED]
  authorized_by: [architect timestamp]
```

---

### TYPE 4 — CASCADE_EVENT

Written when a declared cascade fires.

```yaml
relay_log_entry:
  entry_id: RL-[YYYYMMDD]-[N]
  entry_type: CASCADE_EVENT
  cascade_rule: [CASCADE-N from CASCADE-POLICY.md]
  cascade_source_event_id: [RE-YYYYMMDD-N]
  cascade_fired_event_id: [RE-YYYYMMDD-N+1]
  cascade_depth: [1 or 2]
  fired_timestamp: [ISO]
  notes: ""
```

---

### TYPE 5 — ACK_ANOMALY

Written when an ACK fails verification checks (orphan, mismatch, wrong source).

```yaml
relay_log_entry:
  entry_id: RL-[YYYYMMDD]-[N]
  entry_type: ACK_ANOMALY
  anomaly_type: [ACK_ORPHAN | ACK_MISMATCH | ACK_WRONG_SOURCE]
  received_ack_event_id: [what the ACK claimed]
  expected_event_id: [what was pending]
  receiving_repo: [which repo sent the anomalous ACK]
  timestamp: [ISO]
  disposition: [DISCARDED | ROUTED_TO_ABNORMAL]
  notes: ""
```

---

### TYPE 6 — ORCHESTRATION_CHANGE

Written when any routing rule or registry entry is modified.

```yaml
relay_log_entry:
  entry_id: RL-[YYYYMMDD]-[N]
  entry_type: ORCHESTRATION_CHANGE
  change_type: [ROUTING_RULE | REGISTRY | WORKFLOW | CLASSIFICATION]
  file_modified: [file path]
  before_state: [description or excerpt]
  after_state: [description or excerpt]
  change_reason: [why]
  amygdala_cleared: [true | false]
  architect_confirmed: [true | false]
  timestamp: [ISO]
```

---

### TYPE 7 — CORRECTION

Written when a prior log entry is found to contain an error.

```yaml
relay_log_entry:
  entry_id: RL-[YYYYMMDD]-[N]
  entry_type: CORRECTION
  corrects_entry_id: [RL-YYYYMMDD-N of original entry]
  original_entry_preserved: true    # Always true — originals are never edited
  error_description: [what was wrong in the original]
  correction: [what the correct information is]
  correction_timestamp: [ISO]
  notes: ""
```

---

## LIVE LOG

*Entries appended at runtime. Build session March 5, 2026 below.*

---

### SESSION OPEN — March 5, 2026

```yaml
relay_log_entry:
  entry_id: RL-20260305-001
  entry_type: ROUTING_EVENT
  subtype: SESSION_OPEN
  session_timestamp_T1: "2026-03-05T13:43:02"
  architect: "Sheldon K. Salmon"
  session_instrument: "ALBEDO"
  gap_from_prior_session: "SESSION OPEN"
  session_register: BUILD
  session_load_on_open: NOMINAL
  notes: >
    THALAMUS build session. Build sequence: principles/ → registry/ →
    classification/ → routing/ → states/ → handoff/ → abnormal/ →
    timing/ → orchestration/. All file creation events handled as
    SUBSIDIARITY_HANDLED (internal THALAMUS operations).
```

---

### TIMESTAMP PROTOCOL FAILURE — Multiple T2 fetch failures this session

```yaml
relay_log_entry:
  entry_id: RL-20260305-002
  entry_type: FAILED_EVENT
  subtype: PROTOCOL_DEGRADATION
  failed_timestamp: "2026-03-05 (multiple occurrences)"
  failure_reason: "T2 fetch failed — user_time_v0 tool returning no result"
  impact: "Timestamp windows uncertifiable for affected responses"
  recovery: "T1 fetches successful on retry. Content integrity unaffected."
  architect_notified: true
  notes: "Content does not depend on T2. Session instrument telemetry degraded. Flagged honestly."
```

---

## APPEND PROTOCOL

1. Entries are appended chronologically
2. Entry IDs are sequential within each date (RL-YYYYMMDD-N)
3. No entry is ever edited after the session in which it was created
4. No entry is ever deleted
5. Corrections use TYPE 7 entries — they reference, never replace
6. On session open: read last entry to establish context from prior session

---

*RELAY-LOG.md — handoff/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*Append-only. Permanent. The history of every routing decision is preserved.*

