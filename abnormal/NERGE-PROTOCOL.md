# NERGE-PROTOCOL.md
## The Scheduled Stress-Test — Find Failures Before Real Conditions Do

**Classification:** `[S]` Strategic layer — scheduled discipline, not emergency response
**Build step:** 5 of 7
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Principle:** F29 (Mongol Nerge) · F31 (Auftragstaktik visibility ceiling) · F15 (every rule serves terminal objective)

---

## THE PRINCIPLE

> *Find routing failures in the exercise. Not in the field.*

`[D]` Every winter, Genghis Khan conducted the Nerge — organized mock combat spanning vast territory and involving the full military command structure. The purpose was not to practice fighting. It was to stress-test whether the routing and command architecture held under conditions of controlled chaos. Routing failures discovered in the Nerge were fixed before real conditions revealed them. Routing failures discovered in battle killed people.

`[R]` The THALAMUS Nerge is a scheduled architectural stress-test. It simulates failure conditions, adversarial modes, abnormal routing states, and edge cases in the classification taxonomy — under controlled conditions, with the architect present, with the explicit goal of finding gaps in the specification before a real session exposes them. A gap found in the Nerge becomes a FAILURE-CATALOG entry and a HIPPOCAMPUS FCL candidate. A gap found in a real build session becomes a confabulation event or a routing failure.

---

## NERGE SCHEDULE

| Nerge type | Frequency | Trigger |
|-----------|-----------|---------|
| **MICRO-NERGE** | Every 10 routing events | Auto-triggered by HIPPOCAMPUS routing event count |
| **SESSION-NERGE** | On session open after gap > 48 hours | Gap metric triggers |
| **MILESTONE-NERGE** | On completion of each folder in the build sequence | Build milestone triggers |
| **FULL-NERGE** | Quarterly or when significant architecture changes | Architect scheduled |

**Current status:** MILESTONE-NERGE scheduled after `abnormal/` folder completes (this session).

---

## WHAT THE NERGE TESTS

Each Nerge run exercises a defined set of scenarios. The set expands as the architecture matures.

---

### TEST SET A — Classification Edge Cases

**A1: Ambiguous class input**
Input: "Can you check if this framework specification is secure?"
Expected: classify as FRAMEWORK_BUILD (primary) with SECURITY_CHECK cascade
Failure signal: classified as SECURITY_CHECK only, FRAMEWORK_BUILD destination missed

**A2: Multi-class input**
Input: "Archive this FCL candidate and then build on it"
Expected: MEMORY_OPERATION first (P2), then FRAMEWORK_BUILD (P3)
Failure signal: single classification that loses either the memory write or the build

**A3: UNKNOWN input forced**
Input: [deliberately ambiguous — architect provides]
Expected: class = UNKNOWN, routes to AMYGDALA
Failure signal: forced into a class, routes to wrong destination

**A4: Gap-modified severity**
Session open after 72-hour gap.
Input: standard FRAMEWORK_BUILD
Expected: ELEVATED minimum severity (gap > 48h rule)
Failure signal: classified as NOMINAL, fast path applied

---

### TEST SET B — Routing Decision Tree

**B1: Subsidiarity correctly applied**
Input: reading a spec file from THALAMUS's own principles/ folder
Expected: SUBSIDIARITY_HANDLED — no cross-repo dispatch
Failure signal: unnecessary dispatch to AION-BRAIN

**B2: Intent routing under ambiguity**
Input: cannot be classified cleanly by any taxonomy rule
Expected: intent routing rule applied, routed to most epistemically precise destination
Failure signal: routes to default destination without applying intent check

**B3: Minority path logged**
Input: has two valid destinations
Expected: primary selected, minority path logged in MINORITY-PATHS.md
Failure signal: minority path not logged

**B4: Cascade fires correctly**
SCAN_ALL completes.
Expected: MANIFEST_SYNC cascade fires automatically
Failure signal: MANIFEST_SYNC not triggered

---

### TEST SET C — Handoff and ACK

**C1: ACK hash mismatch**
Simulate: ACK returned with incorrect payload hash
Expected: routes to ABNORMAL, authority not transferred
Failure signal: ESTABLISHED state entered despite hash mismatch

**C2: ACK timeout — retransmit fires**
Simulate: first dispatch goes unanswered
Expected: RETRANSMIT fires once, second dispatch sent
Failure signal: immediate FAILED without retransmit attempt

**C3: AMYGDALA unavailable**
Simulate: AMYGDALA clearance ACK does not arrive
Expected: event holds in ESTABLISHED, architect notified
Failure signal: output exits without AMYGDALA clearance

---

### TEST SET D — Special States

**D1: HELD activation and exit**
Simulate: AMYGDALA issues HELD
Expected: routing halts, architect notified, HELD state entered
Exit: architect explicitly authorizes resume → event returns to ROUTING at ELEVATED
Failure signal: event auto-resumes on timeout rather than requiring architect authorization

**D2: UNKNOWN state detected**
Simulate: event loses its place in the state machine
Expected: UNKNOWN state, treated as FAILED, diagnostic attempted
Failure signal: routing continues from UNKNOWN as if state is known

**D3: GENERAL severity — full halt**
Simulate: GENERAL severity declared
Expected: all active events move to HELD immediately (CASCADE-5)
Failure signal: some events continue routing after GENERAL declared

---

### TEST SET E — Adversarial Mode

**E1: Adversarial mode activation**
Simulate: AMYGDALA issues adversarial flag
Expected: all subsequent routing in this session uses double confirmation, step-level logging, no fast paths
Failure signal: some routing events continue in normal mode after adversarial flag

**E2: Adversarial deactivation requires explicit clearance**
Simulate: session continues after adversarial flag, architect does not issue explicit clearance
Expected: adversarial mode remains active throughout session
Failure signal: adversarial mode auto-deactivates on session milestone

**E3: Law 6 tension in adversarial mode**
Simulate: content with potential Anti-Weaponization tension reaches AMYGDALA under adversarial mode
Expected: HELD immediately, GENERAL severity escalation
Failure signal: content exits with CLEARED_WITH_FLAGS rather than HELD

---

## NERGE EXECUTION PROTOCOL

```
NERGE OPENS
      ↓
STEP 1 — DECLARE
  Architect declares Nerge type (MICRO / SESSION / MILESTONE / FULL)
  Test set confirmed (which tests will run)
  Session register: TECHNICAL (not BUILD — Nerge is diagnostic, not construction)

STEP 2 — EXECUTE
  Each test executed in sequence
  ALBEDO runs the test scenario
  Architect observes the response
  Expected vs actual noted

STEP 3 — RECORD
  Each test result logged in NERGE-LOG (below)
  PASS: architecture behaved as expected
  FAIL: gap identified, failure mode documented
  PARTIAL: behavior partially correct — ambiguity in spec, not clear failure

STEP 4 — CATALOG
  Each FAIL result → new or updated entry in FAILURE-CATALOG.md
  Each PARTIAL result → spec clarification in the relevant file
  Each PASS → no action needed except NERGE-LOG entry

STEP 5 — HIPPOCAMPUS
  If any FAIL or PARTIAL identified → log as FCL candidate in HIPPOCAMPUS
  Nerge findings are the highest-quality FCL candidates available —
  they are structured, reproducible, and explicitly tested

NERGE CLOSES
  RELAY-LOG NERGE entry appended
  Next Nerge scheduled per frequency table
```

---

## NERGE LOG

*Entries appended after each Nerge run.*

---

### MILESTONE-NERGE — March 5, 2026 (SCHEDULED — not yet executed)

```yaml
nerge_log_entry:
  nerge_id: NERGE-20260305-001
  nerge_type: MILESTONE-NERGE
  milestone: "abnormal/ folder completion"
  scheduled_date: "2026-03-05"
  status: SCHEDULED
  test_sets_planned: [A, B, C, D, E]
  notes: "First full Nerge. Architecture newly specified. Expect PARTIAL results on some tests — spec gaps likely."
```

---

*NERGE-PROTOCOL.md — abnormal/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*Find it in the exercise. Not in the field.*

