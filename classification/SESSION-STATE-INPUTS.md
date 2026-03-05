# SESSION-STATE-INPUTS.md
## Session State as Classification Input — The Human Dimension

**Classification:** `[D]` Operational layer — read alongside INPUT-TAXONOMY.md and SEVERITY-TIERS.md
**Build step:** 1 of 7 — third classification dimension
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Principle:** F16 (FAA IMSAFE · NASA PAVE) · F17 (Hajj multi-factor) · F7 (NRC severity)

---

## WHY SESSION STATE IS A CLASSIFICATION INPUT

`[D]` The FAA's IMSAFE checklist and NASA's PAVE framework are pilot pre-flight assessment tools that classify operational readiness before any flight begins. They ask: what is the condition of the human operator right now, not yesterday, not on average? Illness, stress, fatigue, alcohol, medication, emotion — all of these modify what operations are safe to execute. A pilot who is medically IMSAFE clears differently than one who is not — even if the aircraft and the route are identical.

`[R]` The same principle applies to the THALAMUS session architecture. An input arriving 47 seconds after the last output is a different routing event than the same input arriving 14 hours later. The content is identical. The session context is not. Session state is classification data, not peripheral metadata.

`[D]` Three session state dimensions feed into classification: the Gap metric (elapsed time since last session), the current register (what cognitive mode the session is in), and the load level (how many routing events are active simultaneously). All three are read from live data, never estimated.

---

## DIMENSION 1 — THE GAP METRIC

**What it measures:** Elapsed time between the T2 timestamp of the last response and the T1 timestamp of the current response. Measured live — never estimated.

**Why it matters for classification:** Session context degrades over time. Architectural decisions made in a session 9 hours ago may have been superseded. Frameworks that were partially specified may need to be re-read. The classification of the incoming input must account for how much context is fresh and how much needs to be re-established.

### Gap Tiers and Classification Implications

| Gap | Tier | Session State | Classification implication |
|-----|------|--------------|--------------------------|
| < 5 min | IN-FLOW | Full context active | No modification to classification. Normal processing. |
| 5–30 min | WORKING | Context mostly active | Light context check before complex builds. |
| 30 min–2 hr | RESUMED | Context partially degraded | Re-read relevant specs before proceeding on FRAMEWORK_BUILD. |
| 2–6 hr | BREAK | Significant context loss | Re-read specs. Treat FRAMEWORK_BUILD as ELEVATED minimum. |
| 6–12 hr | PAUSE | Major context loss | Full context re-establishment for any architectural work. |
| 12–24 hr | OVERNIGHT | Session context reset | Treat as new session for complex builds. ELEVATED minimum. |
| 24–48 hr | EXTENDED | Full reset | Re-read session architecture. Confirm last known state before proceeding. |
| 48 hr–1 week | MULTI-DAY | Architecture may have evolved | Architect confirms last known state before any builds. |
| > 1 week | LONG-ABSENCE | Cannot assume anything is current | Full state confirmation required. Start from registry before routing. |

### Gap + Severity Interaction

Gap tier modifies severity floor:

| Gap Tier | Minimum Severity |
|----------|-----------------|
| IN-FLOW / WORKING | NOMINAL allowed |
| RESUMED / BREAK | ELEVATED minimum for FRAMEWORK_BUILD |
| PAUSE / OVERNIGHT | ELEVATED minimum for all non-trivial tasks |
| EXTENDED+ | ELEVATED minimum for all tasks. Architect confirms before GENERAL downgrade. |

Gap does not override a higher severity already assigned — it only raises the floor.

---

## DIMENSION 2 — CURRENT REGISTER

**What it measures:** The cognitive mode the session is currently operating in. Derived from the most recent VOCA register signal and the Gap Response Architecture tier.

**Why it matters for classification:** Register mismatch produces routing errors. A CHRONOS DEEP research session that receives a PEER-register casual question has two active cognitive modes simultaneously — and the routing architecture must handle both without conflating them.

### Register States

| Register | What it means | Classification implication |
|----------|--------------|--------------------------|
| `BUILD` | Active framework construction, deep architectural work | Full deceleration layer. ECF tagging mandatory. No compression. |
| `TECHNICAL` | Specification, how-does-X-work, precise technical output | Complete answer. Precision over brevity. Full output. |
| `PEER` | Conversational exchange, direct questions, casual technical | Minimum words carrying full accuracy. No headers unless necessary. |
| `WARM` | Emotional signal detected | WARM overrides all others. Human presence first. Short. |
| `RESTING` | Sheldon has indicated rest mode | Light responses. No heavy framework deployment. |

**Register + Class interaction:**
- FRAMEWORK_BUILD input arriving during RESTING register: acknowledge receipt, hold build until register shifts to BUILD
- WARM register signal arriving during active BUILD: pause build, address emotional signal first
- PEER register input during CHRONOS DEEP session: classify and route the new input without interrupting the budget

---

## DIMENSION 3 — SESSION LOAD

**What it measures:** How many routing events are active simultaneously in the current session. Derived from the STATE-REGISTRY in `states/`.

**Why it matters for classification:** THALAMUS is a serial routing instrument in a session context. Multiple simultaneous complex builds produce interference — partial context from one build leaking into another, classification errors from working memory saturation, outputs that blend frameworks from parallel threads.

### Load Levels

| Load Level | Active Events | Implication |
|-----------|--------------|-------------|
| `NOMINAL` | 0–1 | Standard operations. Full capacity available. |
| `MANAGED` | 2–3 | Manageable with explicit tracking. Confirm which thread is active before each output. |
| `ELEVATED` | 4–5 | Apply intake timing controls. Stagger rather than run parallel. |
| `SATURATED` | 6+ | Stop accepting new routing events until current events close. See INTAKE-TIMING.md. |

**Load + Class interaction:**
- SCAN_ALL during SATURATED load: queue for next available slot, not rejected
- FRAMEWORK_BUILD during MANAGED load: confirm which build is active before proceeding
- MEMORY_OPERATION always proceeds regardless of load — memory writes are low-overhead

---

## THE THREE-DIMENSIONAL CLASSIFICATION CHECK

Before finalizing any classification vector, run this check:

```
1. GAP CHECK
   What is the current Gap tier?
   Does it raise the severity floor?
   Is context re-establishment needed before routing?

2. REGISTER CHECK
   What register is the session currently in?
   Does the incoming input class match the current register?
   If mismatch — which takes priority?

3. LOAD CHECK
   How many routing events are currently active?
   Is load at ELEVATED or SATURATED?
   Should this input be queued or staggered?
```

These three checks run after Tier 1 class assignment and before Tier 2 severity assignment. They may raise the severity floor but cannot lower it.

---

## LIVE DATA SOURCES

All three dimensions are read from live data:

| Dimension | Data source | How read |
|-----------|------------|----------|
| Gap | T2 from previous response in conversation history | Calculated from TIMESTAMP-PROTOCOL |
| Register | Most recent VOCA register signal | Read from session context |
| Load | STATE-REGISTRY.md in states/ | Count of ACTIVE/PENDING_ACK events |

`[D]` Live data only. No estimates. No assumptions about what the session state probably is. If the data cannot be read — flag it and treat as UNKNOWN until it can be confirmed.

---

*SESSION-STATE-INPUTS.md — classification/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*Session state is classification data. Never peripheral metadata.*

