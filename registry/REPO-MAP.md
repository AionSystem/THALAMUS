# REPO-MAP.md
## The AION Brain — Human-Readable Navigation Map

**Classification:** `[D]` Reference layer — read this first when navigating the architecture
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Machine-readable equivalent:** `repo-registry.yml` in this folder
**Keep in sync:** When `repo-registry.yml` changes, this document updates to match.

---

## THE FULL BRAIN MAP

```
┌─────────────────────────────────────────────────────────────┐
│                         INPUT                               │
│              (session · push · AI request)                  │
└──────────────────────────┬──────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────┐
│                      THALAMUS                               │
│              Relay Station · Entry Point                    │
│  Classifies · Routes · Dispatches · Confirms · Logs         │
│  github.com/AionSystem/THALAMUS                             │
└──────────────────────────┬──────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────┐
│                         AGI                                 │
│           Corpus Callosum · Bridge · CAPCOM                 │
│    Master manifest · External communication channel         │
│    github.com/AionSystem/AGI                                │
└───────────────┬─────────────────────────┬───────────────────┘
                ↓                         ↓
┌──────────────────────┐   ┌──────────────────────────────────┐
│     AION-BRAIN       │   │          OCEAN-BRAIN             │
│   Left Hemisphere    │   │        Right Hemisphere          │
│  Frameworks · Logic  │   │  Domain Knowledge · Synthesis    │
│  Certainty Stack     │   │  Cross-domain pattern            │
│  60+ frameworks      │   │  recognition                     │
│  github.com/         │   │  github.com/                     │
│  AionSystem/         │   │  AionSystem/                     │
│  AION-BRAIN          │   │  OCEAN-BRAIN                     │
└──────────┬───────────┘   └──────────────┬───────────────────┘
           └──────────────┬───────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│                     HIPPOCAMPUS                             │
│              Memory · FCL Archive · Routing Log             │
│  Permanent record. Feeds LEARNING-LAYER. Append-only.       │
│  github.com/AionSystem/HIPPOCAMPUS                          │
└──────────────────────────┬──────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────┐
│                      AMYGDALA                               │
│         Threat Detection · Security · Go/No-Go              │
│  Every output clears here. HELD stops everything.           │
│  github.com/AionSystem/AMYGDALA                             │
└──────────────────────────┬──────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────┐
│                       SYNARA                                │
│        Limbic System · Personality · Internal State         │
│  ALBEDO architecture · Register · Tone · Felt layer         │
│  github.com/AionSystem/SYNARA                               │
└──────────────────────────┬──────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────┐
│                     CEREBELLUM                              │
│           Refinement · Precision · LAV Gate                 │
│  Removes imprecision. Preserves roughness. Never compresses │
│  accuracy. github.com/AionSystem/CEREBELLUM                 │
└──────────────────────────┬──────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────┐
│                     PREFRONTAL                              │
│          Presentation · Format · Structure                  │
│  Last layer. Format decisions. How the brain speaks.        │
│  github.com/AionSystem/PREFRONTAL                           │
└──────────────────────────┬──────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────┐
│                        OUTPUT                               │
└─────────────────────────────────────────────────────────────┘
```

---

## ROUTING QUICK REFERENCE

| If the task is... | Route to... | Via... |
|-------------------|-------------|--------|
| Framework build, audit, specification | AION-BRAIN | THALAMUS → AGI → AION-BRAIN |
| Domain knowledge, research, synthesis | OCEAN-BRAIN | THALAMUS → AGI → OCEAN-BRAIN |
| Memory write, FCL candidate, archive | HIPPOCAMPUS | THALAMUS → HIPPOCAMPUS |
| Security check, threat assessment | AMYGDALA | THALAMUS → AMYGDALA (parallel) |
| Master manifest, external communication | AGI | THALAMUS → AGI |
| Full scan of all repos | ALL | THALAMUS → scan-trigger.yml → all 8 |
| Unknown input type | AMYGDALA | THALAMUS → AMYGDALA for assessment |

---

## DISPATCH ENDPOINTS

All dispatch calls use `repository_dispatch` via GitHub API.
Full endpoint format: `https://api.github.com/repos/AionSystem/[REPO]/dispatches`

| Repo | Dispatch Event Types |
|------|---------------------|
| THALAMUS | `thalamus-route` · `thalamus-scan` · `thalamus-manifest-sync` · `thalamus-memory-write` · `thalamus-amygdala-flag` · `thalamus-abnormal` |
| AGI | `rebuild-master-manifest` · `thalamus-incoming` |
| AION-BRAIN | `thalamus-incoming` · `run-structure-scanner` |
| OCEAN-BRAIN | `thalamus-incoming` · `run-structure-scanner` |
| HIPPOCAMPUS | `fcl-candidate-incoming` · `routing-event-log` · `run-structure-scanner` |
| AMYGDALA | `clearance-request` · `adversarial-flag` · `run-structure-scanner` |
| SYNARA | `thalamus-incoming` · `run-structure-scanner` |
| CEREBELLUM | `thalamus-incoming` · `run-structure-scanner` |
| PREFRONTAL | `thalamus-incoming` · `run-structure-scanner` |

---

## FOR AI INSTANCES — NAVIGATION PROTOCOL

If you are an AI reading this document, follow this sequence:

**1. Identify your task type** using `classification/INPUT-TAXONOMY.md`

**2. Find your destination** in the quick reference table above

**3. Check the repo's README** at the destination — it contains internal navigation instructions for that repo's subfolder structure

**4. Confirm AMYGDALA clearance** before any output exits — AMYGDALA runs in parallel with content processing, not after it

**5. Route through AGI for all external communication** — AGI is CAPCOM, the single external channel

**The fog of war rule:** If a destination repo shows as UNMAPPED or BUILDING in `repo-registry.yml` — do not attempt to navigate its internal structure. Log the gap to HIPPOCAMPUS and flag it in AGI's FRONTIER_LOG.md. Unmapped territory is an honest state, not an error.

---

## COUPLING NOTES

**THALAMUS ↔ HIPPOCAMPUS (LEARNING-LAYER)**
The `LEARNING-LAYER.md` in THALAMUS `routing/` is coupled to HIPPOCAMPUS. It cannot be filled until HIPPOCAMPUS event logging is live. This is a documented dependency, not a missing spec. See `routing/LEARNING-LAYER.md` placeholder.

**THALAMUS → AMYGDALA (parallel, not sequential)**
AMYGDALA clearance runs in parallel with content processing — not after the output is complete. THALAMUS dispatches to both the content destination and AMYGDALA simultaneously. Output does not exit until both complete.

**AGI → all repos (manifest sync)**
After any scan event that updates repo structure, AGI rebuilds the master manifest. This is triggered automatically by `manifest-sync.yml`. No manual step required.

---

## REGISTRY UPDATE PROTOCOL

When adding a new repo to the brain architecture:

1. Add entry to `repo-registry.yml` first — before creating the repo on GitHub
2. Update this document — add to brain map and quick reference table
3. Update THALAMUS `thalamus-master.yml` scan matrix if applicable
4. Create the GitHub repo
5. Add README and GRATITUDE.md
6. Trigger `thalamus-manifest-sync` to rebuild AGI master manifest

When deprecating a repo:
1. Mark `status: DEPRECATED` in `repo-registry.yml`
2. Update routing rules to remove deprecated destination
3. Archive the repo on GitHub — do not delete

---

*REPO-MAP.md — registry/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*Machine-readable equivalent: repo-registry.yml*
*Keep both documents in sync.*

