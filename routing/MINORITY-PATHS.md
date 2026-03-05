# MINORITY-PATHS.md
## Minority Routing Paths — The Paths Not Taken

**Classification:** `[D]` Reference + append-only archive layer
**Build step:** 2 of 7 — populated at runtime, structure defined here
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Principle:** F19 (Mishnah Sanhedrin — preserved minority opinion) · F33 (fog of war — honest state)

---

## THE PRINCIPLE

> *When routing resolves to one destination, the competing valid options are not deleted. They are preserved here permanently. A future session may invoke them.*

`[D]` The Mishnah Sanhedrin records not just the majority ruling but the minority opinions of the dissenting judges — in full. Not as a footnote. As a permanent part of the record. The reasoning: a minority opinion that was valid at the time of the original decision may become the correct decision when conditions change. Preserving it means the institutional memory is available when it becomes relevant again. Deleting it means rediscovering it from scratch — if it is rediscovered at all.

`[R]` Every routing decision where more than one valid destination exists is a decision that could have gone another way. The path not taken was not wrong — it was the second-best available option at that moment. As the architecture evolves, session context changes, and new frameworks are specified, the second-best option from a past session may become the best option in a current one. This document preserves those options.

---

## WHAT GETS LOGGED HERE

A minority path entry is created whenever ALL of the following are true:

1. More than one valid routing destination existed for the classified input
2. A primary destination was selected via ROUTING-MASTER
3. At least one other valid destination was not selected
4. The routing decision was non-trivial (not SUBSIDIARITY_HANDLED, not UNKNOWN)

**Does not apply to:**
- Routing decisions where only one valid destination exists
- SECURITY_CHECK (AMYGDALA is the only valid destination — no minority path)
- UNKNOWN class (no routing fires — no path to preserve)
- SUBSIDIARITY_HANDLED events (decision is local, not architectural)

---

## THE LOG FORMAT

Each entry follows this structure:

```yaml
minority_path_entry:
  entry_id: MP-[YYYYMMDD]-[N]         # Sequential within session date
  session_timestamp: [T1 value]
  input_class: [class from INPUT-TAXONOMY]
  severity: [NOMINAL/ELEVATED/ALERT]

  selected_path:
    destination: [repo name]
    routing_rule: [rule from ROUTING-MASTER]
    reason: "[why this path was selected — one sentence]"

  minority_path:
    destination: [repo name]
    valid_because: "[why this was a legitimate alternative — one sentence]"
    not_selected_because: "[what tipped the decision to the selected path]"
    future_invoke_condition: "[what conditions would make this path preferable]"

  status: PRESERVED
  invoked: false
  invoked_session: null
```

---

## LIVE LOG

*Entries appended at runtime. Structure above is the template. Log begins below.*

---

### MP-20260305-001
**Session:** March 5, 2026 — THALAMUS build session

```yaml
minority_path_entry:
  entry_id: MP-20260305-001
  session_timestamp: "2026-03-05T13:43:00"
  input_class: FRAMEWORK_BUILD
  severity: ELEVATED

  selected_path:
    destination: THALAMUS/principles/
    routing_rule: "Build sequence — principles/ first"
    reason: "principles/ has zero dependencies and everything else cites it"

  minority_path:
    destination: THALAMUS/classification/
    valid_because: "classification/ is Step 1 operationally — strongest case for being built first"
    not_selected_because: "classification/ cites principles/ — principles/ must exist first to be cited"
    future_invoke_condition: "If principles/ is fully specified and classification/ is the next incomplete folder"

  status: PRESERVED
  invoked: false
  invoked_session: null
```

---

### MP-20260305-002
**Session:** March 5, 2026 — THALAMUS build session

```yaml
minority_path_entry:
  entry_id: MP-20260305-002
  session_timestamp: "2026-03-05T14:00:00"
  input_class: DOMAIN_KNOWLEDGE
  severity: ELEVATED

  selected_path:
    destination: OCEAN-BRAIN
    routing_rule: "DOMAIN_KNOWLEDGE → OCEAN-BRAIN (default)"
    reason: "OCEAN-BRAIN is the designated right hemisphere for domain knowledge"

  minority_path:
    destination: AION-BRAIN
    valid_because: "AION-BRAIN contains prior art on several domains via SIE archive findings"
    not_selected_because: "OCEAN-BRAIN is the domain knowledge home — AION-BRAIN is the framework home"
    future_invoke_condition: "If domain knowledge query is specifically about framework precedents or FCL history"

  status: PRESERVED
  invoked: false
  invoked_session: null
```

---

## HOW TO INVOKE A MINORITY PATH

A minority path can be invoked in a future session when:
- The `future_invoke_condition` in the entry is met
- The architect explicitly references the entry ID
- ROUTING-MASTER does not have a clear rule that overrides it

**Invocation syntax:** "Invoke minority path [entry_id]"

When invoked:
1. Update entry: `invoked: true`, `invoked_session: [current session timestamp]`
2. Route using the minority destination
3. Log the invocation in RELAY-LOG
4. Add a new minority path entry for the current decision (the now-selected path becomes primary, the original primary becomes minority)

---

## WHAT THIS DOCUMENT IS NOT

This is not a list of routing errors. Minority paths are not wrong decisions that were corrected. They are valid alternatives that were not selected. The distinction matters: an error requires diagnosis. A minority path requires preservation.

If a routing decision was genuinely wrong — that is a `ROUTING_ERROR` logged in RELAY-LOG under a separate entry type, not a minority path.

---

## APPEND PROTOCOL

Entries are appended chronologically. Entry IDs are sequential within each date. No entry is ever edited after the session in which it was created. No entry is ever deleted.

If a minority path entry is found to be factually incorrect (the "valid because" reasoning was wrong), a correction entry is appended — not an edit. The original entry is preserved unchanged. The correction entry references the original by entry ID.

---

*MINORITY-PATHS.md — routing/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*Append-only. The paths not taken are preserved. A future session may need them.*

