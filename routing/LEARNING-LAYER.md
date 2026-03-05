# LEARNING-LAYER.md
## Route Reinforcement and Decay — PLACEHOLDER

**Classification:** `[?]` Unspecified — awaiting dependency
**Build step:** 6 of 7 — requires HIPPOCAMPUS event logging to be live
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Principle:** F13 (slime mold reinforcement) · F14 (ant stigmergy decay)

---

## STATUS

⚠ **PLACEHOLDER — DO NOT FILL UNTIL HIPPOCAMPUS EVENT LOGGING IS LIVE**

This file is a declared dependency placeholder. The LEARNING-LAYER cannot be specified without live routing event data from HIPPOCAMPUS. Building it now would produce a specification with no data source — a design without an instrument.

This is an honest architectural state, not a missing spec. The file exists to make the dependency visible, not to hide the gap.

---

## WHAT WILL LIVE HERE

When HIPPOCAMPUS event logging is live and has accumulated sufficient routing history (minimum 30 routing events recommended), this file will contain:

**Route Reinforcement Rules**
- How successful routing paths (primary destination reached, ACK received, AMYGDALA cleared) accumulate weight
- Threshold at which a reinforced path becomes the default for its classification vector
- Reinforcement decay rate — how quickly a path loses weight without use

**Trace Evaporation Rules** (F14 — ant stigmergy)
- How long an unused routing path retains its weight before decaying
- The evaporation schedule — rapid decay for high-confidence paths that stop being used (signals conditions have changed), slower decay for lower-confidence paths
- What triggers immediate evaporation vs scheduled decay

**Learning Loop Integration**
- How LEARNING-LAYER reads from HIPPOCAMPUS
- How LEARNING-LAYER writes updated weights to ROUTING-MASTER
- Frequency of weight recalculation
- Architect review gate before any weight change modifies live routing behavior

**Anti-Lock Rules**
- Preventing the routing table from converging permanently on a single path regardless of conditions
- Minimum traffic floor for minority paths — paths below this floor are preserved but their weight is frozen, not further decayed
- The condition under which a minority path is reactivated despite low weight

---

## THE DEPENDENCY CHAIN

```
HIPPOCAMPUS event logging → live (Step 6 prerequisite)
      ↓
30+ routing events accumulated in HIPPOCAMPUS
      ↓
LEARNING-LAYER specified (this file filled)
      ↓
ROUTING-MASTER weights updated on schedule
      ↓
THALAMUS routes differently — the loop closes (F24 — HICS closed loop)
```

---

## COUPLING NOTE IN REGISTRY

`repo-registry.yml` documents this dependency:
```yaml
notes: "COUPLED TO THALAMUS LEARNING-LAYER. HIPPOCAMPUS event logging must be live
        before LEARNING-LAYER.md in THALAMUS routing/ can be filled."
```

---

## DO NOT:

- Fill this file with placeholder rules that have no data source
- Assume routing weights based on intuition rather than logged event data
- Build the reinforcement mechanism before the logging mechanism that feeds it

**The confabulation risk here is structural:** a LEARNING-LAYER built without live data is a learning layer that learns from its own assumptions — a closed loop with no external input. That is not learning. That is drift that looks like learning.

---

*LEARNING-LAYER.md — routing/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*PLACEHOLDER. Dependency: HIPPOCAMPUS event logging live. Do not fill until dependency is met.*

