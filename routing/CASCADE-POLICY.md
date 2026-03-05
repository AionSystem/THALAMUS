# CASCADE-POLICY.md
## Cascade Policy — When One Routing Event Triggers Another

**Classification:** `[D]` Operational layer — governs secondary routing event propagation
**Build step:** 2 of 7
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Principle:** F4 (State Dept FOIA cascade) · F24 (HICS closed loop) · F15 (every rule serves terminal objective)

---

## THE POLICY STATEMENT

> *Cascade is explicit. Default is no cascade. Every secondary routing event is declared in this document or it does not fire.*

`[D]` The State Department's "release to one, release to all" FOIA policy is the canonical cascade rule: when one requester receives a document, all requesters receive it simultaneously. This policy exists because implicit cascade — "it probably applies to these other cases too" — produces inconsistent outcomes that are impossible to audit. The cascade rule is declared in policy, not assumed from context.

`[R]` THALAMUS applies the same principle. A routing event that implicitly triggers a secondary routing event without declaration is a routing event that cannot be audited, cannot be debugged, and cannot be modified without unexpected side effects. All cascades are declared here. Nothing cascades silently.

---

## DECLARED CASCADE RULES

---

### CASCADE 1 — SCAN_ALL → MANIFEST_SYNC (automatic)

```
TRIGGER:  SCAN_ALL routing event reaches CLOSED state (all 8 ACKs received)
FIRES:    MANIFEST_SYNC routing event to AGI
PAYLOAD:  {source: SCAN_ALL, session_timestamp, scan_results_summary}
DELAY:    Immediate — no wait
CONDITION: Always. MANIFEST_SYNC fires after every completed SCAN_ALL.
```

**Why:** AGI holds the master manifest. Every scan produces structural data. The manifest must reflect current structure. Allowing scans to complete without manifest rebuild produces a manifest that diverges from reality over time.

**Exception:** If AGI is unreachable at cascade time, MANIFEST_SYNC queues as P4 (Scheduled) and fires when AGI becomes available. Scan completion does not wait for manifest rebuild.

---

### CASCADE 2 — FRAMEWORK_BUILD (FCL candidate) → MEMORY_OPERATION

```
TRIGGER:  FRAMEWORK_BUILD routing event produces a declared FCL candidate
FIRES:    MEMORY_OPERATION to HIPPOCAMPUS (event: fcl-candidate-incoming)
PAYLOAD:  {candidate_content, source_framework, session_timestamp, convergence_state}
DELAY:    On FCL candidate declaration — not on FRAMEWORK_BUILD close
CONDITION: Only when FCL candidate is explicitly declared during build.
           Not triggered by every FRAMEWORK_BUILD.
```

**Why:** FCL candidates are time-sensitive. They represent a moment of epistemic clarity that should be preserved while context is fresh. Waiting until FRAMEWORK_BUILD fully closes risks losing detail.

**The explicit declaration gate:** FCL cascade only fires when the architect or ALBEDO explicitly declares "FCL candidate." It does not fire on every framework build. Implicit FCL candidates do not cascade.

---

### CASCADE 3 — ORCHESTRATION (routing rule change) → RELAY-LOG write

```
TRIGGER:  Any ORCHESTRATION event that modifies a routing rule in ROUTING-MASTER.md
FIRES:    RELAY-LOG write with full before/after state of modified rule
PAYLOAD:  {rule_id, before_state, after_state, reason, session_timestamp}
DELAY:    Simultaneous with rule change — before and after states captured in same event
CONDITION: Every routing rule modification. No exceptions.
```

**Why:** Routing rules that change without a permanent record produce a routing architecture that cannot be audited. The RELAY-LOG entry is the permanent record of what changed, when, and why.

---

### CASCADE 4 — AMYGDALA HELD → architect notification

```
TRIGGER:  AMYGDALA returns HELD signal on any clearance request
FIRES:    Architect notification (surface to session context immediately)
PAYLOAD:  {held_content_reference, held_reason, session_timestamp}
DELAY:    Immediate — no wait
CONDITION: Every HELD signal. No exceptions. HELD never silences.
```

**Why:** A HELD signal is not a background event. It is a routing halt that requires architect decision. Silent HELD signals would allow routing to appear to continue when it has actually stopped.

---

### CASCADE 5 — GENERAL severity → HELD on all active events

```
TRIGGER:  Any routing event classified at GENERAL severity
FIRES:    HELD status on ALL currently active routing events (all non-CLOSED states)
PAYLOAD:  {general_trigger, session_timestamp, held_event_count}
DELAY:    Immediate
CONDITION: Any GENERAL severity classification. Architect cannot override cascade timing.
           Architect can only authorize resume, not prevent the cascade.
```

**Why:** GENERAL severity means normal operations are no longer possible. Routing events that were already in flight under normal assumptions cannot continue under GENERAL conditions — they were classified under different premises.

---

### CASCADE 6 — New repo added to registry → MANIFEST_SYNC

```
TRIGGER:  repo-registry.yml updated with new repo entry, status: ACTIVE
FIRES:    MANIFEST_SYNC to AGI
PAYLOAD:  {new_repo_name, registry_timestamp, session_timestamp}
DELAY:    On registry file commit
CONDITION: Any new ACTIVE repo added. DEPRECATED status changes do not trigger.
```

**Why:** The master manifest must reflect all active repos. Adding a repo without triggering manifest rebuild produces a manifest that doesn't include the new repo — navigation failures for any AI instance using the manifest.

---

## NO-CASCADE DECLARATIONS

The following events do **not** trigger secondary routing events. This is explicit — not assumed.

| Event | Does NOT cascade to | Why |
|-------|--------------------|----|
| FRAMEWORK_BUILD (no FCL) | HIPPOCAMPUS | FCL cascade requires explicit declaration |
| DOMAIN_KNOWLEDGE | MANIFEST_SYNC | Knowledge retrieval doesn't change architecture |
| MEMORY_OPERATION (read) | Any secondary | Read operations are complete in themselves |
| SECURITY_CHECK (CLEARED) | Any secondary | Clean clearance ends the routing event |
| SECURITY_CHECK (CLEARED_WITH_FLAGS) | Architect notification | Flags are surfaced but don't trigger new routing |
| SUBSIDIARITY_HANDLED | RELAY-LOG detailed write | Subsidiarity events log minimally — not full cascade |
| NOMINAL severity routing | ELEVATED+ protocols | Severity upgrade requires new condition, not cascade |

---

## CASCADE DEPTH LIMIT

No cascade chain may exceed depth 2 from the originating event.

```
Depth 0: Original routing event (e.g., SCAN_ALL)
Depth 1: First cascade (e.g., MANIFEST_SYNC)
Depth 2: Second cascade (e.g., if MANIFEST_SYNC triggered something)
Depth 3+: NOT PERMITTED — architect must authorize any depth-3 cascade explicitly
```

`[R]` Unbounded cascade depth produces routing loops — a class of failure that is extremely difficult to detect and diagnose. The depth-2 limit is a hard architectural constraint, not a recommendation. Any routing scenario requiring depth-3 cascade is a signal that the architecture needs redesign, not that the depth limit should be lifted.

---

## AUDITING CASCADE EVENTS

All cascade events are logged in RELAY-LOG with a `cascade_depth` field:

```yaml
relay_log_entry:
  event_type: CASCADE
  cascade_source: SCAN_ALL
  cascade_rule: CASCADE-1
  cascade_depth: 1
  fired_event: MANIFEST_SYNC
  session_timestamp: [T1]
  status: CLOSED
```

The `cascade_rule` field references the specific rule in this document. If a cascade fires that cannot reference a rule in this document — it is an undeclared cascade and is treated as a routing error.

---

*CASCADE-POLICY.md — routing/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*Cascade is explicit. Default is no cascade. Nothing propagates silently.*

