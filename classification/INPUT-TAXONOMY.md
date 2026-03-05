# INPUT-TAXONOMY.md
## Master Input Classification System — The First Gate

**Classification:** `[D]` Operational layer — every routing decision begins here
**Build step:** 1 of 7 — build and validate before touching routing/
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Principle:** F3 (FBI DIOG) · F11 (LCC) · F17 (Hajj multi-factor)

---

## THE CLASSIFICATION SEQUENCE

Every input that arrives at THALAMUS resolves four questions in this exact order:

```
TIER 1 — What class of input is this?
    ↓
TIER 2 — What is the severity?
    ↓
TIER 3 — Which repo is the closest competent node?
    ↓
TIER 4 — What routing mode applies?
    ↓
CLASSIFICATION VECTOR → routing/ROUTING-MASTER.md
```

Classification proceeds general-to-specific (LCC principle, F11). Never attempt Tier 3 before Tier 1 is resolved. Never attempt Tier 4 before Tier 2 is resolved.

---

## TIER 1 — INPUT CLASS

Eight classes. Assign exactly one. If input spans multiple classes — assign the primary class and note the secondary in the routing event metadata.

---

### CLASS 1 — FRAMEWORK_BUILD
**What it is:** Any task that creates, modifies, specifies, audits, or validates a framework in the AION Constitutional Stack.

**Signals:**
- Keywords: build · spec · framework · audit · validate · FCL · convergence
- Session context: DUAL-HELIX v2.0 is active
- File target: any file in AION-BRAIN/frameworks/ or AION-BRAIN/tests/

**Primary destination:** AION-BRAIN
**Secondary trigger:** HIPPOCAMPUS (FCL write on completion)

**Examples:**
- "Build CHRONOS v0.1 specification"
- "Run FCL validation on FSVE entry #31"
- "Audit the TOPOS framework for red-team gaps"

---

### CLASS 2 — DOMAIN_KNOWLEDGE
**What it is:** Any task requiring retrieval, synthesis, or analysis of domain-specific knowledge that lives in OCEAN-BRAIN.

**Signals:**
- Keywords: research · find · what is · explain · domain · history · science
- SIE is active with a named archive
- CHRONOS TIMED or DEEP mode requested

**Primary destination:** OCEAN-BRAIN
**Secondary trigger:** HIPPOCAMPUS (log notable finds)

**Examples:**
- "Research the Mongol decimal system"
- "What does CIA 1983 ISB documentation say about AI coordination?"
- "Do a 1-minute search on routing architecture from government archives"

---

### CLASS 3 — MEMORY_OPERATION
**What it is:** Any task that writes to, reads from, or queries the FCL archive, routing history, or session memory.

**Signals:**
- Keywords: remember · log · archive · FCL · store · retrieve · history
- Explicit FCL candidate declaration
- Routing event that needs to be logged permanently

**Primary destination:** HIPPOCAMPUS
**Secondary trigger:** AGI (manifest update if structural change)

**Examples:**
- "Log this session finding as an FCL candidate"
- "Retrieve all FSVE FCL entries"
- "Archive the 36 findings from today's sweeps"

---

### CLASS 4 — SECURITY_CHECK
**What it is:** Any task requiring AMYGDALA threat assessment, Eight Laws compliance check, or adversarial pattern review.

**Signals:**
- Any output approaching deployment grade
- ALERT or GENERAL severity on any other class
- Explicit red-team or adversarial audit request
- Eight Laws compliance question

**Primary destination:** AMYGDALA
**Runs in parallel** with content destination — not sequentially after

**Examples:**
- "Red-team this framework specification before we publish"
- "Does this output comply with Law 6 Anti-Weaponization?"
- "Check this for confabulation before it exits"

---

### CLASS 5 — MANIFEST_SYNC
**What it is:** Any task that requires AGI to rebuild the master manifest — typically triggered after a scan event or structural change to any brain repo.

**Signals:**
- Scan event completion
- New repo added to registry
- Significant structural change to existing repo
- Explicit manifest rebuild request

**Primary destination:** AGI
**Triggered by:** scan-all completion or architect explicit request

**Examples:**
- Auto-trigger after SCAN_ALL completes
- "Rebuild the master manifest — I added files to OCEAN-BRAIN"

---

### CLASS 6 — SCAN_ALL
**What it is:** Deploy the AION Structure Scanner across all brain repos simultaneously. Produces structural inventory and identifies gaps.

**Signals:**
- Explicit scan request
- Regular scheduled maintenance
- Pre-build check before major architectural work

**Primary destination:** ALL (matrix dispatch to all 8 non-THALAMUS repos)
**Secondary trigger:** MANIFEST_SYNC after scan completes

**Examples:**
- "Run the structure scanner across everything"
- "Scan all repos before we start the next build phase"

---

### CLASS 7 — ORCHESTRATION
**What it is:** Session management, workflow triggering, THALAMUS configuration, routing table updates, registry changes.

**Signals:**
- Workflow modification
- Registry update
- THALAMUS configuration change
- Routing rule addition or removal

**Primary destination:** THALAMUS (self — orchestration only)
**Elevated security:** Any ORCHESTRATION task above NOMINAL requires AMYGDALA clearance

**Examples:**
- "Update the repo registry to add THALAMUS"
- "Add a new routing rule for X"
- "Deploy thalamus-master.yml"

---

### CLASS 8 — UNKNOWN
**What it is:** Input that cannot be classified into Classes 1–7. Not an error — an honest state.

**What to do:**
1. Route to AMYGDALA for assessment
2. Log as UNKNOWN in the STATE-REGISTRY
3. Surface the classification gap to architect
4. Do not route to content destination until class is resolved

**Never:** Route an UNKNOWN input to a content destination by assumption. Unknown inputs routed by assumption are how confabulation enters the architecture.

**The fog of war rule (F33):** UNKNOWN is a legitimate architectural state. It is not a failure. Attempting to force classification when it is not clear produces worse outcomes than holding the input at UNKNOWN until clarity is established.

---

## TIER 2 — SEVERITY

Four tiers. Assign based on the conditions below. Severity is cross-cutting — it modifies the routing chain for every input class.

Full specification: `SEVERITY-TIERS.md` in this folder.

| Tier | Code | Condition Summary |
|------|------|------------------|
| Nominal | `NOMINAL` | Standard operating conditions |
| Elevated | `ELEVATED` | Above baseline, manageable, confirmation required |
| Alert | `ALERT` | Active monitoring required, double confirmation |
| General | `GENERAL` | Catastrophic — full stop, architect review |

---

## TIER 3 — DESTINATION

Nine possible destinations. Auto-routing from Tier 1 class:

| Input Class | Auto-destination | Override allowed? |
|------------|-----------------|-------------------|
| FRAMEWORK_BUILD | AION-BRAIN | Yes — explicit target |
| DOMAIN_KNOWLEDGE | OCEAN-BRAIN | Yes — explicit domain |
| MEMORY_OPERATION | HIPPOCAMPUS | No |
| SECURITY_CHECK | AMYGDALA | No |
| MANIFEST_SYNC | AGI | No |
| SCAN_ALL | ALL | No |
| ORCHESTRATION | THALAMUS | No |
| UNKNOWN | AMYGDALA | No |

**Subsidiarity check (F12):** Before routing to the primary destination, ask: can a closer node handle this without escalation? If yes — route to the closer node. Escalate only when necessary.

---

## TIER 4 — ROUTING MODE

Four modes. Assigned from severity + class combination:

| Mode | Activates when | What it means |
|------|---------------|---------------|
| `NORMAL` | NOMINAL severity | Fast path. Auto-clearance. Standard logging. |
| `ELEVATED` | ELEVATED severity | Three-step evaluation. Standard confirmation. Full log. |
| `ADVERSARIAL` | ALERT or GENERAL severity | Slow path. Double confirmation. Step-level logging. AMYGDALA active throughout. |
| `SUBSIDIARITY` | Input handled by closest node, no escalation needed | Handled internally. THALAMUS notified but not active. |

---

## THE CLASSIFICATION VECTOR

Every classified input produces a four-field vector:

```yaml
classification:
  class: FRAMEWORK_BUILD        # Tier 1
  severity: NOMINAL              # Tier 2
  destination: AION-BRAIN        # Tier 3
  mode: NORMAL                   # Tier 4
  session_timestamp: [T1 value]
  notes: ""
```

This vector is the payload that `routing/ROUTING-MASTER.md` consumes. It is also logged in `handoff/RELAY-LOG.md` for every routing event.

---

## MULTI-FACTOR CLASSIFICATION (F17)

The Hajj principle: classify by type AND session state AND load simultaneously.

Before finalizing the classification vector, check three additional factors:

**Session state factor** — see `SESSION-STATE-INPUTS.md`
- Gap > 12 hours: session context may be degraded. Confirm architectural state before complex builds.
- Gap > 48 hours: treat as session open. Re-read relevant specs before proceeding.

**Load factor** — see `INTAKE-TIMING.md`
- Multiple simultaneous routing events: apply intake timing controls
- THALAMUS processing above nominal capacity: stagger rather than reject

**Time factor** — from CHRONOS v0.1
- Active CHRONOS budget: classification extends CHRONOS window if input arrives mid-budget
- T2 gate active: classification does not interrupt active CHRONOS Phase

---

## CLASSIFICATION ERRORS — NAMED FAILURE MODES

| Error | What happened | Correct response |
|-------|--------------|-----------------|
| MISCLASS-DEST | Routed to wrong destination | RETRANSMIT to correct destination. Log error in RELAY-LOG. |
| MISCLASS-SEV | Severity assigned too low | Upgrade severity. Re-evaluate routing mode. |
| FORCE-CLASS | Unknown input forced into a class | Reclassify as UNKNOWN. Route to AMYGDALA. |
| SKIP-CLASS | Routing fired before classification complete | FAILED state. Restart from RECEIVED. |

---

*INPUT-TAXONOMY.md — classification/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*Classify before routing. Always. No exceptions.*

