# ROUTING-MASTER.md
## The Decision Tree — THALAMUS Master Routing Table

**Classification:** `[D]` Operational layer — consumes classification vectors, produces dispatch directives
**Build step:** 2 of 7 — built after classification/ is complete
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Principle:** F9 (NRC NUREG decision tree) · F3 (classify first) · F12 (subsidiarity) · F30 (route by intent)

---

## HOW TO READ THIS DOCUMENT

Every routing rule is structured as:

```
IF   [classification condition]
THEN [primary routing action]
ELSE [alternative routing action]
VERIFY [confirmation required before closing]
```

This is not a lookup table. A lookup table handles expected inputs and fails silently on unexpected ones. This decision tree handles expected and unexpected inputs with defined behavior at every branch. (F9 — NRC NUREG-0899)

**Inputs to this document:** Classification vector from `classification/INPUT-TAXONOMY.md`
**Outputs from this document:** Dispatch directive to `handoff/HANDOFF-PROTOCOL.md`

---

## PRE-ROUTING SEQUENCE

Before any routing rule fires, three checks run in this order:

```
STEP 1 — SUBSIDIARITY CHECK
  Can the closest competent node handle this without escalation?
  IF YES → SUBSIDIARITY_HANDLED mode. Route to that node. Do not escalate.
  IF NO  → Continue to Step 2.

STEP 2 — AMYGDALA PARALLEL DISPATCH
  Dispatch SECURITY_CHECK to AMYGDALA simultaneously with content routing.
  AMYGDALA runs in parallel — it does not gate content routing start.
  AMYGDALA does gate content routing EXIT — output waits for clearance.

STEP 3 — SEVERITY GATE
  IF GENERAL → HELD. No routing fires. Architect review required.
  IF ALERT   → Adversarial mode. Proceed with ALERT routing rules below.
  IF ELEVATED → Three-step evaluation. Proceed with ELEVATED routing rules.
  IF NOMINAL  → Fast path. Proceed with NOMINAL routing rules.
```

---

## ROUTING RULES BY CLASS

---

### CLASS 1 — FRAMEWORK_BUILD

**NOMINAL:**
```
IF   class=FRAMEWORK_BUILD, severity=NOMINAL
THEN route to AION-BRAIN via repository_dispatch (event: thalamus-incoming)
     dispatch payload: {class, severity, mode, session_timestamp, notes}
ELSE IF AION-BRAIN unreachable
     → RETRANSMIT once after timeout
     → IF second failure → FAILED state, architect notification
VERIFY ACK from AION-BRAIN within timeout threshold (see ACK-SPEC.md)
```

**ELEVATED:**
```
IF   class=FRAMEWORK_BUILD, severity=ELEVATED
THEN Evaluate → Plan → Assign before dispatch:
     1. EVALUATE: Read relevant framework spec before routing
     2. PLAN: Confirm destination subfolder in AION-BRAIN
     3. ASSIGN: Dispatch to AION-BRAIN with ELEVATED flag in payload
ELSE IF no clear plan established after evaluation
     → Surface to architect before dispatching
VERIFY ACK + confirmation that AION-BRAIN received ELEVATED flag
```

**ALERT:**
```
IF   class=FRAMEWORK_BUILD, severity=ALERT
THEN Adversarial mode activates (see abnormal/ADVERSARIAL-MODE.md)
     Notify architect before dispatch
     Route to AION-BRAIN with ALERT flag
     AMYGDALA clearance required before output exits AION-BRAIN
ELSE IF architect not reachable for notification
     → Hold in PENDING state. Do not dispatch without architect awareness.
VERIFY ACK + AMYGDALA clearance signal
```

**Secondary trigger — all FRAMEWORK_BUILD severities:**
```
ON COMPLETION of FRAMEWORK_BUILD routing event:
IF   FCL candidate identified during build
THEN dispatch MEMORY_OPERATION to HIPPOCAMPUS (fcl-candidate-incoming)
ELSE no secondary trigger
```

---

### CLASS 2 — DOMAIN_KNOWLEDGE

**NOMINAL:**
```
IF   class=DOMAIN_KNOWLEDGE, severity=NOMINAL
THEN route to OCEAN-BRAIN via repository_dispatch (event: thalamus-incoming)
     CHRONOS SPRINT mode: no budget, exits when complete
ELSE IF OCEAN-BRAIN unreachable
     → Check if AION-BRAIN holds the domain knowledge (cross-hemisphere check)
     → IF yes: route to AION-BRAIN with domain flag
     → IF no: FAILED state, architect notification
VERIFY ACK from OCEAN-BRAIN
```

**ELEVATED:**
```
IF   class=DOMAIN_KNOWLEDGE, severity=ELEVATED
THEN CHRONOS TIMED mode: declare budget (1 min default)
     Route to OCEAN-BRAIN with TIMED flag and budget in payload
     SIE archive doors loaded if research involves government sources
ELSE IF budget elapsed before Phase 3 completes
     → Proceed to Phase 3 synthesis with current findings
VERIFY ACK + Phase 3 synthesis complete before output exits
```

**ALERT:**
```
IF   class=DOMAIN_KNOWLEDGE, severity=ALERT
THEN CHRONOS DEEP mode: 2-minute hard cap
     NRP gates active: no search gap filling from training data
     AMYGDALA clearance on all retrieved content before synthesis
ELSE IF NRP Gate 1 fires (thin search results)
     → Surface null return to architect
     → Await architect decision before proceeding
VERIFY ACK + NRP gates confirmed active + AMYGDALA clearance
```

---

### CLASS 3 — MEMORY_OPERATION

**All severities (MEMORY_OPERATION bypasses severity routing — always direct):**
```
IF   class=MEMORY_OPERATION
THEN route to HIPPOCAMPUS via repository_dispatch (event: fcl-candidate-incoming OR routing-event-log)
     Payload includes: operation_type, content, session_timestamp, source_class
ELSE IF HIPPOCAMPUS unreachable
     → Queue memory write locally in THALAMUS RELAY-LOG
     → Retry on next session open
     → Log as PENDING_MEMORY in STATE-REGISTRY
VERIFY ACK from HIPPOCAMPUS
     IF no ACK: retain in THALAMUS queue — do not discard
```

**Priority:** MEMORY_OPERATION is P2 (High) — processes ahead of FRAMEWORK_BUILD and DOMAIN_KNOWLEDGE in queue but behind SECURITY_CHECK.

---

### CLASS 4 — SECURITY_CHECK

**All severities (SECURITY_CHECK always runs in parallel — never queued):**
```
IF   class=SECURITY_CHECK
THEN dispatch to AMYGDALA via repository_dispatch (event: clearance-request)
     Payload includes: content_hash, source_class, severity, output_type
     Timer starts: AMYGDALA SLA = [defined in ACK-SPEC.md]
ELSE IF AMYGDALA unreachable
     → Do not release output. Hold in PENDING_ACK.
     → Architect notification: AMYGDALA unreachable, output held
     → Retry after [timeout defined in ACK-SPEC.md]
VERIFY AMYGDALA clearance signal before any output exits
     Clearance types: CLEARED · CLEARED_WITH_FLAGS · HELD
     IF HELD: routing halts, architect review required
```

**SECURITY_CHECK is never queued. Never staggered. Bypasses all load controls.**

---

### CLASS 5 — MANIFEST_SYNC

```
IF   class=MANIFEST_SYNC
THEN dispatch to AGI via repository_dispatch (event: rebuild-master-manifest)
     Payload includes: trigger_source, changed_repos, session_timestamp
ELSE IF AGI unreachable
     → Queue manifest sync for next available window
     → Log as PENDING_MANIFEST in STATE-REGISTRY
     → Architect notification if AGI unreachable > 1 hour
VERIFY ACK from AGI
     Confirm manifest rebuild event logged in AGI
```

**Auto-trigger:** MANIFEST_SYNC fires automatically after any SCAN_ALL completion. No manual trigger required.

---

### CLASS 6 — SCAN_ALL

```
IF   class=SCAN_ALL, load=NOMINAL or MANAGED
THEN dispatch to all 8 non-THALAMUS repos simultaneously via matrix dispatch
     Event: run-structure-scanner
     Repos: AGI · AION-BRAIN · OCEAN-BRAIN · HIPPOCAMPUS · AMYGDALA · SYNARA · CEREBELLUM · PREFRONTAL
ELSE IF load=ELEVATED or SATURATED
     → Queue SCAN_ALL at P4 (Scheduled priority)
     → Activate when load drops to MANAGED
IF   all 8 ACKs received
THEN trigger MANIFEST_SYNC automatically
ELSE IF any repo fails to ACK
     → Log gap repos in RELAY-LOG
     → Notify architect of incomplete scan
     → MANIFEST_SYNC proceeds with available data, flags missing repos
VERIFY all 8 ACKs OR documented gap with architect notification
```

---

### CLASS 7 — ORCHESTRATION

**NOMINAL:**
```
IF   class=ORCHESTRATION, severity=NOMINAL
THEN execute configuration change within THALAMUS
     Log in RELAY-LOG with full change metadata
ELSE IF change affects routing rules
     → Re-read affected routing rules after change
     → Confirm no circular routing introduced
VERIFY change applied + RELAY-LOG entry confirmed
```

**ELEVATED+:**
```
IF   class=ORCHESTRATION, severity=ELEVATED or above
THEN AMYGDALA clearance required before any configuration change executes
     Architect confirmation required
     Full before/after state logged
VERIFY AMYGDALA clearance + architect confirmation + RELAY-LOG entry
```

---

### CLASS 8 — UNKNOWN

```
IF   class=UNKNOWN
THEN route to AMYGDALA for assessment (event: clearance-request with UNKNOWN flag)
     Log as UNKNOWN in STATE-REGISTRY
     Surface classification gap to architect: "Input received, class=UNKNOWN. Awaiting assessment."
ELSE IF AMYGDALA assessment returns a class
     → Re-classify with AMYGDALA-assessed class
     → Re-route as ELEVATED minimum
ELSE IF AMYGDALA cannot classify
     → Hold in UNKNOWN state
     → Architect provides classification manually
VERIFY Classification resolved before any content routing fires
```

**UNKNOWN inputs never route to content destinations by assumption. (NRP principle)**

---

## THE INTENT ROUTING RULE (F30)

When no routing rule covers the incoming classification vector exactly:

```
APPLY MISSION-PRINCIPLE:
  Does the available routing path serve epistemic precision?
  IF YES → route to the closest serving destination, log as INTENT_ROUTED
  IF UNCERTAIN → route to AMYGDALA for assessment, log as INTENT_DEFERRED
  IF NO → hold, surface to architect, log as INTENT_BLOCKED
```

Intent routing is logged explicitly — never silently. The RELAY-LOG entry includes the intent rule invoked and the reasoning.

---

## THE MINORITY PATH LOG

At every routing decision where more than one valid destination exists, the non-selected path is logged in `MINORITY-PATHS.md`. (F19 — Sanhedrin)

Format:
```
Routing event: [session_timestamp]
Selected path: [destination] — reason: [why selected]
Minority path: [destination] — preserved: [why valid but not selected]
```

Minority paths are never deleted. A future session may invoke them.

---

## ROUTING FAILURE STATES

| Failure | What happened | Response |
|---------|--------------|----------|
| `DEST_UNREACHABLE` | Primary destination not responding | RETRANSMIT once. If second failure → FAILED + architect notification. |
| `ACK_TIMEOUT` | ACK not received within threshold | RETRANSMIT. See ACK-SPEC.md for timeout values. |
| `AMYGDALA_HELD` | AMYGDALA issued HELD | All routing halts. Architect review before resume. |
| `INTENT_BLOCKED` | No route serves epistemic precision | Hold. Surface to architect. Do not route by assumption. |
| `CLASSIFICATION_INCOMPLETE` | Vector missing a tier | Return to classification/. Complete vector before routing. |

---

*ROUTING-MASTER.md — routing/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*Every rule has a condition. Every condition has an alternative. Every action has a verification.*

