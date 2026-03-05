<div align="center">

# classification/

### *The First Gate — Every Routing Decision Begins Here*

[![Function](https://img.shields.io/badge/FUNCTION-Input_Classification-1a6b9a?style=for-the-badge&labelColor=0d1117)]()
[![Build Step](https://img.shields.io/badge/BUILD_STEP-1_of_7-FFD700?style=for-the-badge&labelColor=0d1117)]()
[![Principle](https://img.shields.io/badge/PRINCIPLE-Classify_Before_Routing-2d6a4f?style=for-the-badge&labelColor=0d1117)]()
[![Source](https://img.shields.io/badge/SOURCE-FBI_DIOG_%7C_NARA_ERM_%7C_LCC_%7C_Hajj-4B0082?style=flat-square&labelColor=0d1117)]()

</div>

---

## WHAT THIS FOLDER IS

Classification is the keystone operation of THALAMUS. Nothing routes before it classifies. Get the classification wrong and every downstream decision routes incorrectly — the routing table, the severity tier, the destination, the mode. This folder contains the complete taxonomy and all classification decision instruments.

`[D]` Architectural precedent: The Library of Congress classifies 15 million items using a general-to-specific architecture — broad class first, then subclass, then granular routing. The FBI's Domestic Investigations and Operations Guide puts input classification as the first mandatory step before any investigative action. The Saudi Hajj management system classifies pilgrims by type, time, load, and health simultaneously before assigning routing zones. Every serious routing system in the world puts classification first.

`[R]` Classification is not a lookup. It is a multi-factor decision that takes input type, severity, session state, and load simultaneously and produces a routing vector. A taxonomy that classifies by type alone is incomplete. One that ignores session state routes correctly in normal conditions and incorrectly under pressure — exactly when correct routing matters most.

---

## WHAT LIVES HERE

| File | Purpose | Build Status |
|------|---------|-------------|
| `INPUT-TAXONOMY.md` | Master 4-tier classification system | Step 1 — Build first |
| `SEVERITY-TIERS.md` | 4-tier severity model: Nominal · Elevated · Alert · General | Step 1 |
| `SESSION-STATE-INPUTS.md` | Gap metric, register, load as classification inputs | Step 1 |
| `INTAKE-TIMING.md` | Pacing controls for elevated load — Hajj staggered arrival principle | Step 1 |
| `README.md` | This file | Complete |

---

## THE FOUR-TIER TAXONOMY STRUCTURE

Every input resolves four classification questions in sequence:

```
TIER 1 — Input Class (what kind of input is this?)
  FRAMEWORK_BUILD · DOMAIN_KNOWLEDGE · MEMORY_OPERATION
  SECURITY_CHECK · MANIFEST_SYNC · SCAN_ALL · ABNORMAL

TIER 2 — Severity (how consequential is this routing event?)
  NOMINAL · ELEVATED · ALERT · GENERAL

TIER 3 — Destination (which repo is the closest competent node?)
  AION-BRAIN · OCEAN-BRAIN · HIPPOCAMPUS · AMYGDALA
  SYNARA · CEREBELLUM · PREFRONTAL · AGI · THALAMUS

TIER 4 — Routing Mode (how does it travel?)
  NORMAL · ELEVATED · ADVERSARIAL · SUBSIDIARITY-HANDLED
```

Classification produces a vector — not a single answer but a four-field output that governs everything downstream. The routing table in `routing/ROUTING-MASTER.md` consumes this vector and fires the correct routing chain.

---

## DESIGN PRINCIPLES EMBEDDED HERE

- **General-to-specific** — Library of Congress structure. Broad class before granular destination.
- **Multi-factor** — Hajj principle. Type alone is insufficient. Session state and load are classification inputs.
- **Severity asymmetry** — Sanhedrin principle. Reversible actions need one confirmation. Irreversible actions need a higher threshold and can only reverse toward safety.
- **Classify before everything** — FBI DIOG. The sequence is not optional.

---

*classification/ — THALAMUS · AION Brain Architecture*
*Part of: Sheldon K. Salmon & ALBEDO · March 2026*

