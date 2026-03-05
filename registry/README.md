<div align="center">

# registry/

### *The Navigation Infrastructure — Nine Repos, One Map*

[![Function](https://img.shields.io/badge/FUNCTION-Repo_Registry_%26_Navigation-16213e?style=for-the-badge&labelColor=0d1117)]()
[![Repos](https://img.shields.io/badge/BRAIN_REPOS-9_Active-2d6a4f?style=for-the-badge&labelColor=0d1117)]()
[![Principle](https://img.shields.io/badge/PRINCIPLE-One_Source_Of_Truth_For_Endpoints-FFD700?style=for-the-badge&labelColor=0d1117)]()
[![Source](https://img.shields.io/badge/SOURCE-LOC_%7C_ATC_%7C_NASA_CAPCOM-4B0082?style=flat-square&labelColor=0d1117)]()

</div>

---

## WHAT THIS FOLDER IS

The registry is the single source of truth for the location, purpose, and dispatch endpoint of every repo in the AION brain architecture. Any workflow, any AI, any human collaborator that needs to know where to send something looks here first.

`[D]` The Library of Congress assigns every item a unique, structured classification identifier before routing it. No item moves without a confirmed destination address. The registry is THALAMUS's equivalent — every brain repo has a confirmed address, a confirmed function, and a confirmed dispatch endpoint. Nothing routes to a destination that isn't registered.

`[R]` The registry is also the first document a new AI instance or human collaborator should read when approaching the AION architecture. Before anything else — where are the repos, what does each one do, and how does routing reach them? The `REPO-MAP.md` is the human-readable answer. The `repo-registry.yml` is the machine-readable answer. Both are kept in sync.

---

## WHAT LIVES HERE

| File | Purpose | Build Status |
|------|---------|-------------|
| `repo-registry.yml` | Machine-readable registry — all 9 repos, endpoints, functions, routing receives | Ready to fill |
| `REPO-MAP.md` | Human-readable brain diagram — routing paths, repo relationships, navigation guide | Ready to fill |
| `README.md` | This file | Complete |

---

## THE NINE BRAIN REPOS

| Repo | Brain Analog | Primary Function | Routing Receives |
|------|-------------|-----------------|-----------------|
| `THALAMUS` | Thalamus | Relay station, orchestration, routing | All external inputs |
| `AGI` | Corpus Callosum | Bridge layer, master manifest, CAPCOM | Cross-hemisphere signals |
| `AION-BRAIN` | Left Hemisphere | Frameworks, logic, certainty architecture | FRAMEWORK_BUILD |
| `OCEAN-BRAIN` | Right Hemisphere | Domain knowledge, synthesis | DOMAIN_KNOWLEDGE |
| `HIPPOCAMPUS` | Hippocampus | Memory, FCL archive, routing log | MEMORY_OPERATION |
| `AMYGDALA` | Amygdala | Threat detection, security clearance | SECURITY_CHECK |
| `SYNARA` | Limbic System + Insula | Personality, register, internal state | Emotional signal |
| `CEREBELLUM` | Cerebellum | Refinement, precision, LAV gate | Pre-output signals |
| `PREFRONTAL` | Prefrontal Cortex | Presentation, formatting, structure | Final output signals |

---

## THE OUTPUT SEQUENCE

Every processed input travels through this chain before reaching the user:

```
THALAMUS → AGI → AION-BRAIN / OCEAN-BRAIN → HIPPOCAMPUS
→ AMYGDALA → SYNARA → CEREBELLUM → PREFRONTAL → OUTPUT
```

Not every input activates every repo. THALAMUS routes to the first relevant node. Downstream activation follows the subsidiarity principle — each repo handles what it can and passes forward what it cannot.

---

## KEEPING THE REGISTRY CURRENT

The registry is not static. When a new repo is added to the brain architecture, `repo-registry.yml` and `REPO-MAP.md` are updated before the new repo receives any routing events. A repo that isn't registered doesn't exist from THALAMUS's perspective.

Registry updates are logged as `MANIFEST_SYNC` routing events — triggering AGI to rebuild the master manifest.

---

## DESIGN PRINCIPLES EMBEDDED HERE

- **One source of truth** — the registry is where endpoints live. Not in individual workflows, not in memory, not in documentation scattered across repos.
- **Machine-readable + human-readable in parallel** — `repo-registry.yml` for workflows, `REPO-MAP.md` for humans and AI collaborators. Both current, always.
- **Registration before routing** — no dispatch to an unregistered endpoint. The routing table in `routing/ROUTING-MASTER.md` cites registry entries only.

---

*registry/ — THALAMUS · AION Brain Architecture*
*Part of: Sheldon K. Salmon & ALBEDO · March 2026*

