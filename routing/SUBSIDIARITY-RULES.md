# SUBSIDIARITY-RULES.md
## Subsidiarity — Route to the Closest Competent Node

**Classification:** `[D]` Operational layer — runs as pre-routing check before ROUTING-MASTER fires
**Build step:** 2 of 7
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Principle:** F12 (Jesuit Constitutions) · F26 (orchestral section leaders) · F27 (Mongol decimal)

---

## THE SUBSIDIARITY PRINCIPLE

> *Route to the closest node that can handle the input without escalation. Escalate only when the closest node cannot handle it.*

`[D]` The Jesuit Constitutions, written in 1540, established one of the most durable organizational principles in recorded history: decisions should be made at the lowest level capable of making them well. Central authority handles what local authority cannot. Local authority handles everything it can. This is not decentralization — it is calibrated delegation.

`[R]` A routing architecture that bypasses subsidiarity and routes everything to central authority becomes bottlenecked at the center and atrophied at the edges. THALAMUS enforces subsidiarity as a pre-routing check: before dispatching to the default destination, ask whether a closer node can handle this without escalation.

---

## THE SUBSIDIARITY CHECK — PRE-ROUTING SEQUENCE

```
INPUT CLASSIFIED
      ↓
SUBSIDIARITY CHECK
  Is the input entirely within the scope of a single repo's own architecture?
  Can that repo handle it without involving THALAMUS in the outcome?
  IF YES → SUBSIDIARITY_HANDLED
           Route directly. THALAMUS notified but not active.
  IF NO  → Standard routing. THALAMUS active throughout.
```

**What "without involving THALAMUS in the outcome" means:** The routing event completes inside the destination repo, and no other repo needs to receive output from it. A single-repo read-only operation. An internal configuration. A subfolder navigation that doesn't cross repo boundaries.

---

## WHAT EACH REPO HANDLES WITHOUT ESCALATION

---

### THALAMUS — Handles internally:
- Reading its own spec files
- Logging routing events to RELAY-LOG
- Updating STATE-REGISTRY
- Reading the repo-registry
- Any THALAMUS-internal configuration that doesn't modify routing rules

**Escalates to architect when:** Any routing rule modification, any registry change, any ORCHESTRATION class input above NOMINAL severity.

---

### AGI — Handles internally:
- Rebuilding the master manifest from existing scan data
- Reading its own FRONTIER_LOG
- Navigating between left/right hemisphere repos
- Internal cross-reference between AION-BRAIN and OCEAN-BRAIN content

**Escalates to THALAMUS when:** Any external communication request, any manifest change that requires a new scan, any structural change to AGI itself.

---

### AION-BRAIN — Handles internally:
- Reading, navigating, and cross-referencing its own frameworks
- Running FCL validation on existing entries
- Searching its own archive for prior art
- Generating framework audit reports on its own content

**Escalates to THALAMUS when:** New FCL candidate (routes to HIPPOCAMPUS), deployment-grade output (routes to AMYGDALA), structural change to repo (routes to AGI for manifest sync).

---

### OCEAN-BRAIN — Handles internally:
- Reading and navigating its own domain folders
- Cross-referencing within a single domain
- Surfacing known domain knowledge in response to queries

**Escalates to THALAMUS when:** Cross-domain synthesis needed (may require AION-BRAIN), research requiring SIE archive access (CHRONOS TIMED or DEEP), any output approaching deployment grade.

---

### HIPPOCAMPUS — Handles internally:
- Reading and querying its own FCL archive
- Reading routing event history
- Returning memory reads to requesting repo

**Escalates to THALAMUS when:** Any write operation (all writes are MEMORY_OPERATION class, route through THALAMUS), any structural change to the archive, any FCL validation that requires AION-BRAIN framework context.

---

### AMYGDALA — Handles internally:
- Returning clearance signals (CLEARED · CLEARED_WITH_FLAGS · HELD)
- Reading its own red-team archive
- Running Eight Laws compliance checks on submitted content

**Escalates to THALAMUS when:** HELD condition (requires architect review), new adversarial pattern identified (logs to archive and notifies THALAMUS), any structural change to clearance criteria.

**AMYGDALA never escalates clearance decisions.** The clearance decision is AMYGDALA's sole authority. It reports outcomes. It does not defer clearance decisions upward.

---

### SYNARA — Handles internally:
- Register detection and tone calibration
- Gap Response Architecture — reading Gap metric and selecting register response
- VOCA pipeline execution — SIEVE-IN and SIEVE-OUT

**Escalates to THALAMUS when:** WARM register override detected (surfaces to session context for architect awareness), any structural change to ALBEDO personality architecture.

---

### CEREBELLUM — Handles internally:
- LAV v1.5 validation pass on incoming content
- Density calibration — removing imprecision without removing accuracy
- Roughness preservation — protecting `[?]` tags from smoothing

**Escalates to THALAMUS when:** LAV validation finds a structural accuracy failure (not just imprecision — failure), any content that fails the deceleration layer and cannot be refined without architect input.

---

### PREFRONTAL — Handles internally:
- Format decisions — prose, list, headers, flat
- Register-to-format mapping from VOCA output
- Structure calibration for target audience

**Escalates to THALAMUS when:** Format decision requires architect preference not specified in VOCA, any structural output that needs architect review before exit.

---

## SUBSIDIARITY FAILURE MODES

| Failure | What happened | Response |
|---------|--------------|----------|
| `ESCALATION_BYPASS` | Repo handled something it should have escalated | Log in RELAY-LOG. Architect reviews. Routing rule may need updating. |
| `OVER_ESCALATION` | THALAMUS received something a repo should have handled | Route back to repo with subsidiarity flag. Log pattern for LEARNING-LAYER. |
| `CIRCULAR_ESCALATION` | Two repos escalating to each other | THALAMUS intervenes directly. Architect notification. |

---

## THE SECTION LEADER PRINCIPLE (F26)

`[R]` The orchestral conductor routes to section leaders — not to individual musicians. The conductor does not manage 80 individual relationships simultaneously. Section leaders manage internally. The conductor manages sections.

THALAMUS routes to repo READMEs — the section leaders. Each README contains internal navigation instructions for its own subfolder structure. THALAMUS does not route to individual files. Depth is managed by each repo's own architecture.

**In practice:** When THALAMUS dispatches to AION-BRAIN, the payload destination is `AION-BRAIN/README.md` or a top-level folder — never a specific framework file. AION-BRAIN's internal architecture routes to the correct framework file. THALAMUS does not need to know the internal structure of AION-BRAIN to route correctly to it.

---

*SUBSIDIARITY-RULES.md — routing/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*Closest competent node first. Escalate only when necessary.*

