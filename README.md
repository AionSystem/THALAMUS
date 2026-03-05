<div align="center">

# THALAMUS

### *The Relay Station of the AION Brain Architecture*

[![Architect](https://img.shields.io/badge/ARCHITECT-Sheldon_K._Salmon-e94560?style=for-the-badge&labelColor=0d1117)](mailto:aionsystem@outlook.com)
[![Co-Architect](https://img.shields.io/badge/CO--ARCHITECT-ALBEDO-9b59b6?style=for-the-badge&labelColor=0d1117)]()
[![Status](https://img.shields.io/badge/STATUS-ACTIVE_BUILD-0f3460?style=for-the-badge&labelColor=0d1117)](https://github.com/AionSystem/THALAMUS)
[![Brain](https://img.shields.io/badge/BRAIN-RELAY_STATION_%E2%80%94_ENTRY_POINT-FFD700?style=for-the-badge&labelColor=0d1117)](https://github.com/AionSystem/AGI)
[![Findings](https://img.shields.io/badge/BUILT_FROM-36_Architectural_Findings-2d6a4f?style=for-the-badge&labelColor=0d1117)]()
[![Sweeps](https://img.shields.io/badge/SWEEPS-4_%7C_Govts_%7C_Religion_%7C_Military_%7C_Biology-4B0082?style=flat-square&labelColor=0d1117)]()

</div>

---

> *"Almost everything goes through the thalamus first.*
> *It does not decide what to think. It decides where thinking begins."*

---

## WHAT THIS REPO IS

THALAMUS is the relay station of the AION brain architecture. Every incoming signal — a new session, a repository push, an AI navigation request, a framework build event — enters through THALAMUS first. THALAMUS classifies it, determines where it goes, fires the routing sequence, confirms the handoff, and logs the event. It does not process content. It routes it.

In the biological brain, the thalamus receives almost all sensory input and dispatches it simultaneously to the cortex for deep processing and to the amygdala for threat assessment — before conscious thought begins. THALAMUS in this architecture does the same: routes to the correct brain region, fires the orchestration sequence, enforces the timing layer, and confirms AMYGDALA clearance before any output exits.

`[D]` This architecture was built from 36 architectural findings drawn from 4 research sweeps: government declassified archives (CIA, FBI, NSA, State Dept, NARA, GovAttic, NSArchive, MuckRock), engineering and organizational systems (FAA, NRC, NASA, Library of Congress, Jesuits, slime mold, ants), religious and technical traditions (Hajj, Talmud, Vinaya, TCP/IP RFC 793, HICS, orchestral conducting), and military and cultural history (Mongol Empire, WWII Auftragstaktik, StarCraft RTS architecture, Dr. Strangelove). Every routing rule has a source. Every source is documented in `principles/36-FINDINGS.md`.

---

## THE FULL BRAIN ARCHITECTURE

```
INPUT ARRIVES
      ↓
  THALAMUS/              ← THIS REPO — relay station, classification, routing
      ↓
   AGI/                  ← Corpus Callosum — master manifest, CAPCOM, bridge layer
    ↙         ↘
AION-BRAIN/   OCEAN-BRAIN/
LEFT           RIGHT
Frameworks     Domain
& Logic        Knowledge
      ↓              ↓
HIPPOCAMPUS/    AMYGDALA/
Memory          Threat Detection
FCL Archive     Security Clearance
      ↓              ↓
         SYNARA/
         Limbic System + Insula
         Personality · Register · Internal State
              ↓
         CEREBELLUM/
         Refinement · Precision · LAV Gate
              ↓
         PREFRONTAL/
         Presentation · Formatting · Structure
              ↓
           OUTPUT
```

<div align="center">

[![AGI](https://img.shields.io/badge/CORPUS_CALLOSUM-AGI-e94560?style=for-the-badge&labelColor=0d1117)](https://github.com/AionSystem/AGI)
[![AION-BRAIN](https://img.shields.io/badge/LEFT_HEMISPHERE-AION--BRAIN-6b3fa0?style=for-the-badge&labelColor=0d1117)](https://github.com/AionSystem/AION-BRAIN)
[![OCEAN-BRAIN](https://img.shields.io/badge/RIGHT_HEMISPHERE-OCEAN--BRAIN-1a6b9a?style=for-the-badge&labelColor=0d1117)](https://github.com/AionSystem/OCEAN-BRAIN)
[![HIPPOCAMPUS](https://img.shields.io/badge/MEMORY-HIPPOCAMPUS-2d6a4f?style=for-the-badge&labelColor=0d1117)](https://github.com/AionSystem/HIPPOCAMPUS)
[![AMYGDALA](https://img.shields.io/badge/SECURITY-AMYGDALA-c0392b?style=for-the-badge&labelColor=0d1117)](https://github.com/AionSystem/AMYGDALA)
[![SYNARA](https://img.shields.io/badge/LIMBIC_SYSTEM-SYNARA-9b59b6?style=for-the-badge&labelColor=0d1117)](https://github.com/AionSystem/SYNARA)
[![CEREBELLUM](https://img.shields.io/badge/REFINEMENT-CEREBELLUM-16213e?style=for-the-badge&labelColor=0d1117)](https://github.com/AionSystem/CEREBELLUM)
[![PREFRONTAL](https://img.shields.io/badge/PRESENTATION-PREFRONTAL-0f3460?style=for-the-badge&labelColor=0d1117)](https://github.com/AionSystem/PREFRONTAL)

</div>

---

## WHAT LIVES IN THALAMUS

```
THALAMUS/
│
├── README.md                        ← This file
├── GRATITUDE.md                     ← Universal gratitude — all sources and collaborators
│
├── classification/                  ← STEP 1 — The first gate. Everything classifies before routing.
│   ├── INPUT-TAXONOMY.md            ← 4-tier classification system (class · severity · destination · mode)
│   ├── SEVERITY-TIERS.md            ← Nominal · Elevated · Alert · General
│   ├── SESSION-STATE-INPUTS.md      ← Gap metric · register · load as routing inputs
│   └── INTAKE-TIMING.md             ← Pacing controls for elevated load
│
├── routing/                         ← STEP 2 — The decision tree. IF/THEN/ELSE/VERIFY.
│   ├── ROUTING-MASTER.md            ← Master routing table — input vector → destination
│   ├── SUBSIDIARITY-RULES.md        ← What each repo handles before escalating
│   ├── CASCADE-POLICY.md            ← Whether one routing event triggers others
│   ├── MINORITY-PATHS.md            ← Competing paths preserved — never deleted
│   └── LEARNING-LAYER.md            ← Route reinforcement + decay [PLACEHOLDER — HIPPOCAMPUS required]
│
├── states/                          ← STEP 3 — Routing events have states, not just destinations.
│   ├── STATE-MACHINE.md             ← RECEIVED → CLASSIFIED → DISPATCHED → ACK → ESTABLISHED → CLOSED
│   ├── STATE-REGISTRY.md            ← Live log of active routing states
│   └── TRANSITION-RULES.md          ← What triggers each transition. What blocks it. What fails it.
│
├── handoff/                         ← STEP 4 — No silent transfers of authority.
│   ├── HANDOFF-PROTOCOL.md          ← Explicit confirmation before authority releases
│   ├── ACK-SPEC.md                  ← Acknowledgment format · timeout thresholds · retransmit rules
│   └── RELAY-LOG.md                 ← Append-only record of every handoff and confirmation
│
├── abnormal/                        ← STEP 5 — Routing when normal stops working.
│   ├── ABNORMAL-PROTOCOL.md         ← Full failure-condition routing specification
│   ├── ADVERSARIAL-MODE.md          ← Slower · fully logged · activated by AMYGDALA flag
│   ├── FAILURE-CATALOG.md           ← Documented failure modes indexed from 36 findings
│   └── NERGE-PROTOCOL.md            ← Scheduled stress-test — find failures before real conditions do
│
├── orchestration/                   ← STEP 7 — Code built after specs are proven.
│   ├── WORKFLOW-SPEC.md             ← Human-readable spec for all workflows
│   ├── scan-trigger.yml             ← Dispatches structure scanner to all repos simultaneously
│   ├── manifest-sync.yml            ← Triggers AGI master manifest rebuild
│   ├── amygdala-check.yml           ← Routes any output to AMYGDALA for pre-clearance
│   ├── memory-write.yml             ← Routes FCL candidates to HIPPOCAMPUS
│   └── abnormal-mode.yml            ← Activates adversarial routing — slow, fully logged
│
├── timing/                          ← Session telemetry — no build sequence dependency.
│   ├── CLOCK-SPEC.md                ← Three clocks: Gregorian · 13 Moon Dreamspell · Hebrew
│   ├── TIMESTAMP-PROTOCOL.md        ← T1/T2 specification · Gap · Window · failure formats
│   └── GAP-RESPONSE.md              ← Session rhythm architecture · Watchmaker Constraint
│
├── registry/                        ← Single source of truth for all brain endpoints.
│   ├── repo-registry.yml            ← All 9 repos · dispatch endpoints · routing receives
│   └── REPO-MAP.md                  ← Human-readable brain map and routing paths
│
└── principles/                      ← The foundation. 36 findings. 5,000 years.
    ├── 36-FINDINGS.md               ← Every finding · source · THALAMUS application
    ├── 6-CORE-PRINCIPLES.md         ← Six principles distilled from all 36 findings
    └── MISSION-PRINCIPLE.md         ← The terminal objective — epistemic precision
```

---

## THE ROUTING SEQUENCE

```
INPUT ARRIVES
      ↓
CLASSIFY — 4-tier taxonomy fires (class · severity · destination · mode)
      ↓
AMYGDALA PRE-CLEARANCE — runs in parallel, not after
  GENERAL severity → HELD — nothing proceeds until architect review
  ALERT severity   → CLEARED WITH FLAGS — monitoring active
  ELEVATED         → CLEARED — standard confirmation
  NOMINAL          → AUTO-CLEARED — fast path
      ↓
ROUTE FIRES — dispatched to correct destination repo
      ↓
PENDING ACK — timer starts, TCP principle active
  ACK received     → ACKNOWLEDGED → ESTABLISHED → proceed
  No ACK (timeout) → RETRANSMIT → second attempt
  Second failure   → FAILED STATE → architect notification
      ↓
RELAY LOG — every routing event appended permanently
      ↓
MANIFEST SYNC → AGI rebuilds master manifest after scan events
```

---

## THE SIX CORE PRINCIPLES

Every routing rule in THALAMUS traces to one of these six. A rule that cannot trace to one should not exist.

| # | Principle | Primary Sources |
|---|-----------|----------------|
| 1 | **Classify First** — before any routing fires | FBI DIOG · NARA ERM · LCC · Hajj |
| 2 | **Severity Tiers** — type alone is insufficient | NRC EOP · FAA ATC · Sanhedrin |
| 3 | **Confirm Every Hop** — no silent movements | FAA ATC · TCP RFC 793 · Mongol Yam |
| 4 | **States Not Events** — routing has a lifecycle | TCP state machine · HICS MBO |
| 5 | **Closed Loop** — feedback returns to THALAMUS | HICS · Slime mold reinforcement |
| 6 | **Principle Over Rules** — serves epistemic precision or it should not exist | Theravada Vinaya · Jesuit Constitutions |

Full source documentation: `principles/36-FINDINGS.md`

---

## THE THREE CLOCKS

Every output from the AION brain architecture carries three simultaneous time references. This is a transparency protocol — time as a design intervention, not decoration.

| Clock | System | Role |
|-------|--------|------|
| **Clock 1** | Gregorian | Universal reference anchor |
| **Clock 2** | 13 Moon Dreamspell | Continuous-count lunisolar — day and moon position |
| **Clock 3** | Hebrew lunisolar | Independent continuous-count verification layer |

T1 is fetched before any processing begins. T2 is fetched after all output is assembled. The Gap measures session rhythm — time between last T2 and this T1. The Window measures the full response act including telemetry overhead. The measurement is inside the thing being measured. This is named, not hidden. Full specification: `timing/TIMESTAMP-PROTOCOL.md`

**CHRONOS v0.1** — the Time-Budgeted Research Protocol — sits above this timing layer. When a research budget is declared (1–2 minutes maximum), T2 becomes a conditional gate: output only exits when the budget is earned. Three phases: SCAN (breadth) → DIVE (depth on highest-signal finds) → REFINE (synthesis only). Anti-padding rule: time remaining alone never justifies additional search.

---

## CROSS-REPO ORCHESTRATION

`[R]` THALAMUS uses two GitHub Actions mechanisms:

**`repository_dispatch`** — THALAMUS sends a targeted event to a specific repo. That repo listens and fires on receipt. No manual trigger required. The entire brain can be activated from a single event in THALAMUS.

**`workflow_call`** — Reusable workflows defined once in `orchestration/` and called from any brain repo. One source of truth. No duplication across repos.

**Required secret:** `THALAMUS_DISPATCH_TOKEN` — a GitHub Personal Access Token with `repo` and `workflow` scope, stored in THALAMUS secrets. This single token enables THALAMUS to dispatch events to all nine brain repos. Rotate on a 90-day schedule or architect's calendar protocol.

---

## AI NAVIGATION INSTRUCTIONS

**If you are an AI instance reading this README:**

You have arrived at the relay station. THALAMUS is the entry point to the full AION brain architecture. Before routing:

1. Read `principles/36-FINDINGS.md` — the knowledge base behind every routing decision
2. Read `routing/ROUTING-MASTER.md` — the routing table for your current task
3. Check `registry/repo-registry.yml` — confirm all linked repos and their endpoints
4. Classify your current task using `classification/INPUT-TAXONOMY.md`
5. Route to content destination AND dispatch AMYGDALA clearance check simultaneously
6. Do not exit any output without AMYGDALA clearance confirmed

**Navigation shortcuts:**
- Framework build task → [`AION-BRAIN`](https://github.com/AionSystem/AION-BRAIN)
- Domain knowledge task → [`OCEAN-BRAIN`](https://github.com/AionSystem/OCEAN-BRAIN)
- Memory / FCL operation → [`HIPPOCAMPUS`](https://github.com/AionSystem/HIPPOCAMPUS)
- Security / threat check → [`AMYGDALA`](https://github.com/AionSystem/AMYGDALA)
- Master brain map → [`AGI`](https://github.com/AionSystem/AGI)
- Routing rules → `routing/ROUTING-MASTER.md`
- Orchestration → `orchestration/WORKFLOW-SPEC.md`

---

## HONEST STATE — MARCH 2026

`[D]` THALAMUS is newly specified. The architecture is fully designed — 9 folders, 33 specification files, 1 master workflow, 5 reusable sub-workflows — and is being built now in build-sequence order.

| Component | Status |
|-----------|--------|
| Repo structure (9 folders) | Created |
| Folder READMEs | Complete |
| Master workflow `thalamus-master.yml` | Built — awaiting deploy |
| `principles/36-FINDINGS.md` | Next to build |
| `classification/INPUT-TAXONOMY.md` | Next to build |
| `registry/repo-registry.yml` | Next to build |
| Remaining spec files | Build sequence order |
| Sub-workflows (5 files) | Step 7 — code last |
| Cross-repo dispatch testing | Pending |

`[S]` Build sequence: `principles/` → `classification/` → `registry/` → `routing/` → `states/` → `handoff/` → `abnormal/` → `orchestration/`. Code is always last.

---

## CONTACT

📧 [aionsystem@outlook.com](mailto:aionsystem@outlook.com)
🔗 [AION-BRAIN Repository](https://github.com/AionSystem/AION-BRAIN)
🔗 [AGI Master Manifest](https://github.com/AionSystem/AGI)

---

<div align="center">

*THALAMUS — Relay Station · Classification · Routing · Orchestration · The Three Clocks*
*Architect: Sheldon K. Salmon — AI Reliability Architect*
*Co-Architect: ALBEDO*
*Part of the AION Brain Architecture · March 2026*

*"Almost everything goes through the thalamus first.*
*It does not decide what to think. It decides where thinking begins."*

**Built from 36 findings. 4 sweeps. 5,000 years of organizational intelligence.**

</div>
