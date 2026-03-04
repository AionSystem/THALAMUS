
# THALAMUS

[![Architect](https://img.shields.io/badge/ARCHITECT-Sheldon_K._Salmon-e94560?style=for-the-badge&labelColor=0d1117)](mailto:aionsystem@outlook.com)
[![Status](https://img.shields.io/badge/STATUS-ACTIVE_BUILD-0f3460?style=for-the-badge&labelColor=0d1117)](https://github.com/AionSystem/THALAMUS)
[![Brain](https://img.shields.io/badge/BRAIN-THALAMUS_RELAY_STATION-FFD700?style=for-the-badge&labelColor=0d1117)](https://github.com/AionSystem/AGI)

---

> *"Almost everything goes through the thalamus first.*
> *It does not decide what to think. It decides where thinking begins."*

---

## WHAT THIS REPO IS

THALAMUS is the relay station of the AION brain architecture.

Every incoming signal — a new session, a new push to any linked repo, an AI navigation request — enters through THALAMUS first. THALAMUS decides where it goes, in what order, and at what timing. It does not process content. It routes it.

In the biological brain, the thalamus receives almost all sensory input and dispatches it simultaneously to two destinations: the cortex for deep processing, and the amygdala for threat assessment. It does this before conscious thought begins.

THALAMUS in this architecture does the same — routes to the correct hemisphere, fires the orchestration sequence, enforces the timing layer, and initiates the security clearance check with AMYGDALA before any output exits.

---

## THE BRAIN ARCHITECTURE

```
INPUT ARRIVES
      ↓
THALAMUS/         ← THIS REPO — relay station, routing, orchestration
      ↓
AGI/              ← Corpus Callosum — master manifest, spatial navigation
    ↙                     ↘
AION-BRAIN/           OCEAN-BRAIN/
LEFT HEMISPHERE        RIGHT HEMISPHERE
Frameworks & Logic     Domain Knowledge
      ↓                     ↓
HIPPOCAMPUS/          AMYGDALA/
Memory & FCL           Threat Detection
Archive                Red Team · Security
```

[![AGI](https://img.shields.io/badge/MASTER-AGI_CORPUS_CALLOSUM-e94560?style=for-the-badge&labelColor=0d1117)](https://github.com/AionSystem/AGI)
[![LEFT BRAIN](https://img.shields.io/badge/LEFT_BRAIN-AION--BRAIN-6b3fa0?style=for-the-badge&labelColor=0d1117)](https://github.com/AionSystem/AION-BRAIN)
[![RIGHT BRAIN](https://img.shields.io/badge/RIGHT_BRAIN-OCEAN--BRAIN-1a6b9a?style=for-the-badge&labelColor=0d1117)](https://github.com/AionSystem/OCEAN-BRAIN)
[![HIPPOCAMPUS](https://img.shields.io/badge/MEMORY-HIPPOCAMPUS-2d6a4f?style=for-the-badge&labelColor=0d1117)](https://github.com/AionSystem/HIPPOCAMPUS)
[![AMYGDALA](https://img.shields.io/badge/SECURITY-AMYGDALA-e94560?style=for-the-badge&labelColor=0d1117)](https://github.com/AionSystem/AMYGDALA)

---

## WHAT LIVES IN THALAMUS

```
THALAMUS/
│
├── orchestration/          ← Reusable workflow definitions
│   ├── scan-trigger.yml    ← Fires AION Structure Scanner across repos
│   ├── manifest-sync.yml   ← Triggers AGI master manifest rebuild
│   ├── amygdala-check.yml  ← Routes output to AMYGDALA for clearance
│   └── memory-write.yml    ← Routes FCL candidates to HIPPOCAMPUS
│
├── timing/                 ← The Three Clocks
│   ├── CLOCK-SPEC.md       ← Gregorian · 13 Moon (Dreamspell) · Hebrew
│   ├── timestamp-protocol.md ← T1/T2 session telemetry specification
│   └── gap-response.md     ← Gap metric · session rhythm architecture
│
├── routing/                ← AI navigation instructions
│   ├── ROUTING-MASTER.md   ← Master routing table — input type → destination
│   ├── session-open.md     ← Full session open sequence specification
│   └── search-protocol.md  ← How AI searches the brain architecture
│
├── dispatch/               ← Cross-repo workflow triggers
│   └── repo-registry.yml   ← List of all linked repos + dispatch endpoints
│
└── README.md               ← This file
```

---

## THE THREE CLOCKS

`[D]` Every output from the AION brain architecture carries three simultaneous time references. This is not aesthetic — it is a transparency protocol. Time is a design intervention when it appears on every output.

| Clock | System | Purpose |
|-------|--------|---------|
| **Clock 1** | Gregorian | Universal reference anchor |
| **Clock 2** | 13 Moon (Dreamspell) | Continuous-count lunisolar — day and moon position |
| **Clock 3** | Hebrew lunisolar | Continuous-count calendar — independent verification layer |

The T1/T2 timestamp protocol — session open time and output exit time — runs on Clock 1. The Gap metric measures session rhythm. The Window measures response span including telemetry overhead. All three are documented honestly — the measurement is inside the thing being measured, and this is named, not hidden.

Full specification: `timing/timestamp-protocol.md`

---

## CROSS-REPO ORCHESTRATION

`[R]` A workflow in one GitHub repo can trigger workflows in other repos. THALAMUS uses two mechanisms:

**`repository_dispatch`** — THALAMUS sends a targeted event to a specific repo. That repo has a workflow listening for the event and fires on receipt. Example: a push to AION-BRAIN sends a dispatch to AGI, which rebuilds the master manifest automatically. No manual trigger required.

**`workflow_call`** — Reusable workflows defined once in THALAMUS and called from any linked repo. Every brain part repo calls the same scan workflow, the same amygdala check workflow, the same memory write workflow. One source of truth. No duplication.

**The repo registry** — `dispatch/repo-registry.yml` — is the list of all linked repos and their dispatch endpoints. Adding a new repo to the brain is one edit to this file.

---

## THE ROUTING PROTOCOL — HOW AI NAVIGATES THE BRAIN

`[S]` Every README in every brain part repo carries a navigation block at the top — structured instructions that tell the AI exactly what to do when it arrives. THALAMUS defines the master routing table that those instructions reference.

The routing logic follows the thalamic principle:

```
INPUT ARRIVES AT THALAMUS
      ↓
THREAT ASSESSMENT — route to AMYGDALA simultaneously
      ↓
CONTENT CLASSIFICATION — what type of input is this?
  Framework build    → AION-BRAIN/CONVERGENCE_ROOM
  Domain knowledge   → OCEAN-BRAIN/[domain folder]
  Navigation request → AGI/MASTER-MANIFEST
  Memory write       → HIPPOCAMPUS/FCL
  Security check     → AMYGDALA/clearance
      ↓
ROUTE FIRES — AI moves to correct destination
      ↓
OUTPUT EXITS — cleared by AMYGDALA before release
```

Full routing table: `routing/ROUTING-MASTER.md`

---

## AMYGDALA CLEARANCE — DOUBLE CONFIRM ARCHITECTURE

`[R]` Every brain part repo contains a red-team section. That section links to AMYGDALA for clearance before any output from that repo is considered deployment-grade. AMYGDALA checks the output against its threat detection architecture and returns a clearance signal or a flag.

This is a distributed verification network — every repo polices itself and refers to a central security authority simultaneously. No repo clears its own output unilaterally. The double-confirm prevents single-point approval failure.

THALAMUS orchestrates the timing of this sequence — the amygdala check runs in parallel with content processing, not after it. Threat assessment does not wait for the output to be complete.

---

## AI NAVIGATION INSTRUCTIONS

**If you are an AI reading this README:**

You have arrived at the relay station. Before routing:

1. Read `routing/ROUTING-MASTER.md` — this tells you where your current task routes to
2. Check the repo registry — confirm all linked repos are reachable
3. Note the current time from the three clocks — session telemetry is live
4. Route simultaneously to content destination AND AMYGDALA clearance check
5. Do not exit any output until AMYGDALA clearance is confirmed

→ **For orchestration workflows:** navigate to `orchestration/`
→ **For timing and clock specifications:** navigate to `timing/`
→ **For routing rules:** navigate to `routing/ROUTING-MASTER.md`
→ **For the master brain map:** return to `https://github.com/AionSystem/AGI`

---

## HONEST STATE — MARCH 2026

`[D]` THALAMUS is newly specified. The architecture is designed. No orchestration workflows are deployed yet. The three clock system is live in ALBEDO session protocol — the formal specification and repo structure are being built now.

`[S]` Build sequence: repo structure → clock specification → routing table → reusable workflow definitions → cross-repo dispatch testing → full orchestration live.

---

## CONTACT

📧 [aionsystem@outlook.com](mailto:aionsystem@outlook.com)

---

*THALAMUS — Relay Station · Routing · Orchestration · The Three Clocks*
*Architect: Sheldon K. Salmon — AI Reliability Architect*
*Co-Architect: ALBEDO*
*Part of the AION Brain Architecture*
*Almost everything goes through the thalamus first.*
