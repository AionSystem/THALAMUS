<div align="center">

# routing/

### *The Decision Tree — Where Classification Becomes Direction*

[![Function](https://img.shields.io/badge/FUNCTION-Routing_Architecture-e94560?style=for-the-badge&labelColor=0d1117)]()
[![Build Step](https://img.shields.io/badge/BUILD_STEP-2_of_7-FFD700?style=for-the-badge&labelColor=0d1117)]()
[![Principle](https://img.shields.io/badge/PRINCIPLE-IF_%2F_THEN_%2F_ELSE_%2F_VERIFY-2d6a4f?style=for-the-badge&labelColor=0d1117)]()
[![Source](https://img.shields.io/badge/SOURCE-NRC_EOP_%7C_Jesuits_%7C_Slime_Mold_%7C_TCP%2FIP-4B0082?style=flat-square&labelColor=0d1117)]()

</div>

---

## WHAT THIS FOLDER IS

Routing is the core decision layer of THALAMUS. It receives the classification vector from `classification/` and converts it into a directive: where does this go, how does it travel, and what are the alternatives if the primary path fails.

`[D]` Routing is not a lookup table. The NRC's Emergency Operating Procedures established the standard: every routing step is an IF/THEN/ELSE/VERIFY structure. IF condition is met, THEN this action fires. IF NOT, this alternative fires. At every step, a verification gate confirms the action completed before the next step begins. A lookup table handles expected inputs. A decision tree handles everything.

`[R]` The routing table learns. Physarum polycephalum — the slime mold — finds optimal network paths by strengthening tubes that successfully connect to food sources and allowing unused tubes to retract. The LEARNING-LAYER.md in this folder implements that principle: successful routing paths accumulate weight, unused paths decay. The routing table is not static — it is shaped by the history of what worked. This file is left as a placeholder until HIPPOCAMPUS event logging is live to feed it.

---

## WHAT LIVES HERE

| File | Purpose | Build Status |
|------|---------|-------------|
| `ROUTING-MASTER.md` | Master decision tree — IF/THEN/ELSE/VERIFY per input class and severity | Step 2 |
| `SUBSIDIARITY-RULES.md` | What each repo handles before escalating upstream | Step 2 |
| `CASCADE-POLICY.md` | Whether one routing event triggers additional events | Step 2 |
| `MINORITY-PATHS.md` | Competing routing options preserved in record — never deleted | Step 2 |
| `LEARNING-LAYER.md` | Route reinforcement and decay — **placeholder until HIPPOCAMPUS live** | Step 6 |
| `README.md` | This file | Complete |

---

## THE ROUTING DECISION STRUCTURE

```
INPUT VECTOR ARRIVES (from classification/)
  ↓
SUBSIDIARITY CHECK
  Can the closest competent node handle this without escalation?
  YES → route to that node, do not escalate
  NO  → continue to full routing decision
  ↓
ROUTING-MASTER DECISION TREE
  IF [class] + [severity] + [session state]
  THEN [primary destination]
  ELSE [alternative destination]
  VERIFY [ACK received before closing]
  ↓
CASCADE CHECK
  Does this routing event trigger additional routing events?
  Policy: see CASCADE-POLICY.md
  ↓
MINORITY PATH LOG
  Record the alternative paths not taken
  Available for future session invocation
  ↓
RELAY-LOG ENTRY (in handoff/)
```

---

## DESIGN PRINCIPLES EMBEDDED HERE

- **Decision tree, not lookup** — NRC EOP. Every step has a condition, action, alternative, and verification gate.
- **Subsidiarity** — Jesuit Constitutions. Route to the closest competent node. Escalate only when necessary.
- **Routes learn** — Slime mold. Successful paths strengthen. Unused paths decay. The table is shaped by history.
- **Minority paths preserved** — Talmud. Competing routing options are logged, not deleted. A future session may invoke them.
- **No silent failures** — TCP/RFC 793. Every routing event requires acknowledgment. No confirmation = retransmit.

---

## COUPLING NOTE

`LEARNING-LAYER.md` requires HIPPOCAMPUS event logging to be operational before it can be filled. Create the file now as a placeholder. Fill it in Step 6 of the build sequence.

---

*routing/ — THALAMUS · AION Brain Architecture*
*Part of: Sheldon K. Salmon & ALBEDO · March 2026*

