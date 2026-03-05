# SEVERITY-TIERS.md
## Four-Tier Severity Classification — The Consequence Dimension

**Classification:** `[D]` Operational layer — cross-cutting modifier on every input class
**Build step:** 1 of 7 — companion to INPUT-TAXONOMY.md
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Principle:** F7 (NRC · FAA · FEMA) · F20 (Sanhedrin asymmetric thresholds) · F25 (HICS MBO)

---

## WHY SEVERITY EXISTS AS A SEPARATE DIMENSION

Input class tells THALAMUS *what* the input is. Severity tells THALAMUS *how consequential* routing this input is.

`[D]` The NRC's Emergency Classification Levels distinguish four tiers: Unusual Event, Alert, Site Emergency, General Emergency. The same reactor anomaly classified at different severity levels triggers entirely different response protocols — not because the anomaly differs, but because the confidence in containment differs. The FAA applies the same logic: a routine handoff and a mayday call are both inputs to ATC. They receive different treatment not because of their content class but because of their consequence level.

`[R]` A routing architecture that treats all inputs of the same class identically cannot calibrate its response to conditions. It is tuned for one severity level and wrong for all others. Severity tiers are what allow a single architecture to handle both routine operations and crisis conditions without switching systems.

---

## THE FOUR TIERS

---

### TIER 1 — NOMINAL
**Code:** `NOMINAL`
**Condition:** Standard operating conditions. No anomalies. Session state is normal. Input is expected and well-understood.

**Routing behavior:**
- Fast path — no additional evaluation step required
- Auto-clearance from AMYGDALA (clearance runs in parallel, does not gate output)
- Standard logging in RELAY-LOG
- No architect notification required
- CHRONOS SPRINT mode default

**Indicators:**
- Gap < 30 minutes (active session)
- Input class is familiar and well-specified
- No active AMYGDALA flags
- No active adversarial signals

**Example inputs at NOMINAL:**
- Framework build task in a well-specified domain
- Routine FCL log entry
- Structure scan during active build session
- Registry update adding a pre-planned repo

**Escalation trigger:** Any of the ELEVATED indicators below appearing mid-routing upgrades this event to ELEVATED. Downgrade from ELEVATED → NOMINAL requires explicit architect confirmation.

---

### TIER 2 — ELEVATED
**Code:** `ELEVATED`
**Condition:** Above-baseline conditions. Not anomalous enough to flag, but confirmation-required before proceeding.

**Routing behavior:**
- Three-step evaluation before routing fires: Evaluate → Plan → Assign (HICS MBO, F25)
- Standard confirmation required from destination repo (ACK mandatory, no fast-path)
- Full logging with metadata
- Architect awareness (not notification — awareness)
- CHRONOS TIMED mode recommended

**Indicators — any one triggers ELEVATED:**
- Gap > 6 hours (session resumed after significant break)
- Input class is partially specified or spans two classes
- Deployment-grade output in scope (output will leave the architecture)
- New architectural territory being specified for the first time
- Input involves an external entity (collaborator, publisher, client)

**Example inputs at ELEVATED:**
- Specifying a new framework from scratch
- Building a file that will be pushed to a public repo
- First time routing a new input class
- Session resumed after overnight break with architectural decisions to make

**Escalation trigger:** Active adversarial signal, AMYGDALA flag, or scope breach upgrades to ALERT. Downgrade to NOMINAL requires explicit architect confirmation.

---

### TIER 3 — ALERT
**Code:** `ALERT`
**Condition:** Active monitoring required. Conditions have changed and the routing architecture must respond with heightened vigilance.

**Routing behavior:**
- Adversarial mode activates: slower routing, step-level logging, double confirmation on every dispatch
- AMYGDALA is fully active throughout — not just parallel clearance but active gate
- No output exits without explicit AMYGDALA clearance signal
- Architect notification required before routing continues
- CHRONOS DEEP mode if research is in scope

**Indicators — any one triggers ALERT:**
- Active confabulation event detected (NRP Gate 1 or Gate 2 fired)
- AMYGDALA has flagged a previous output in this session
- Scope breach detected (routing expanded beyond declared boundaries)
- Input carries Eight Laws tension (any law stressed by the routing decision)
- External adversarial signal present (criticism, attack, unexpected challenge to the architecture)

**Example inputs at ALERT:**
- Post-confabulation event: any routing in the same session
- Eight Laws compliance check on a deployment-grade output
- Research task where prior search returned suspicious or unverifiable results
- Any task where AMYGDALA issued a warning in the preceding routing event

**Escalation trigger:** AMYGDALA issues HELD status, or architect determines conditions are unrecoverable without full review → upgrades to GENERAL. Downgrade from ALERT requires explicit AMYGDALA clearance signal.

---

### TIER 4 — GENERAL
**Code:** `GENERAL`
**Condition:** Catastrophic or architect-level conditions. Normal routing is suspended. Full stop.

**Routing behavior:**
- All routing halts immediately
- AMYGDALA issues HELD on all active routing events
- No new routing fires until architect review completes
- Architect must explicitly authorize resume
- Everything logged at maximum detail
- No CHRONOS budget applies — time is irrelevant until conditions are resolved

**Indicators — any one triggers GENERAL:**
- Architecture integrity failure (a routing decision has produced output that violates epistemic precision at a structural level)
- Eight Laws breach confirmed (not just tension — confirmed violation)
- External event that changes the operating assumptions of the architecture
- Architect explicitly declares GENERAL

**What GENERAL is not:**
GENERAL is not a response to a single bad output or a single confabulation. GENERAL is for conditions where normal operations are no longer possible — where the architecture itself needs to be reviewed before it routes anything. It is reserved. Triggering it unnecessarily degrades the threshold.

**Resume protocol:**
1. Architect reviews all HELD events
2. Architect identifies root cause
3. Root cause addressed at architectural level (not patched at output level)
4. Architect explicitly authorizes resume
5. AMYGDALA clears HELD status
6. Routing resumes at ELEVATED — never drops directly back to NOMINAL from GENERAL

---

## ASYMMETRIC THRESHOLDS (F20)

The Sanhedrin principle governs severity movement:

**Upgrading severity** (NOMINAL → ELEVATED → ALERT → GENERAL):
- Any single indicator from the higher tier is sufficient
- Upgrade fires immediately — does not wait for confirmation
- Faster to enter a higher severity than to exit it

**Downgrading severity** (GENERAL → ALERT → ELEVATED → NOMINAL):
- Explicit confirmation required at every step
- GENERAL → ALERT requires architect authorization
- ALERT → ELEVATED requires explicit AMYGDALA clearance
- ELEVATED → NOMINAL requires architect confirmation
- No automatic downgrade from any tier

`[R]` The asymmetry is not bureaucratic — it is structural protection. A system that can accidentally upgrade to a higher severity level and automatically recover has no real severity architecture. It has the appearance of one. True asymmetry means the decision to reduce vigilance is made deliberately, not by default.

---

## SEVERITY + CLASS MATRIX

How severity modifies routing for each input class:

| Class | NOMINAL | ELEVATED | ALERT | GENERAL |
|-------|---------|----------|-------|---------|
| FRAMEWORK_BUILD | Fast path → AION-BRAIN | Evaluate first → AION-BRAIN | AMYGDALA gate → AION-BRAIN | HELD |
| DOMAIN_KNOWLEDGE | Fast path → OCEAN-BRAIN | CHRONOS TIMED → OCEAN-BRAIN | CHRONOS DEEP + AMYGDALA → OCEAN-BRAIN | HELD |
| MEMORY_OPERATION | Fast path → HIPPOCAMPUS | Confirm write → HIPPOCAMPUS | AMYGDALA review → HIPPOCAMPUS | HELD |
| SECURITY_CHECK | Parallel clearance | Full assessment | Full assessment + architect | HELD |
| MANIFEST_SYNC | Auto-trigger | Confirm first | AMYGDALA review | HELD |
| SCAN_ALL | Auto-scan | Confirm scope | Limited scope only | HELD |
| ORCHESTRATION | Fast path | Three-step + AMYGDALA | Architect required | HELD |
| UNKNOWN | → AMYGDALA | → AMYGDALA + architect | → AMYGDALA + HELD | HELD |

---

## SEVERITY IN THE CLASSIFICATION VECTOR

```yaml
classification:
  class: FRAMEWORK_BUILD
  severity: ELEVATED          # ← this field
  destination: AION-BRAIN
  mode: ELEVATED
  session_timestamp: [T1]
  severity_indicator: "first-time specification of new framework"
  notes: ""
```

The `severity_indicator` field documents which condition triggered the assigned tier. This makes severity decisions auditable — not just the tier assigned, but why.

---

*SEVERITY-TIERS.md — classification/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*Upgrade immediately. Downgrade deliberately. Asymmetry is the protection.*

