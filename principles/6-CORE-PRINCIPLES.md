# 6-CORE-PRINCIPLES.md
## The Distilled Architecture — Six Principles From 36 Findings

**Classification:** `[R]` Derived layer — synthesized from 36-FINDINGS.md
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Usage:** Every routing rule in THALAMUS must trace to at least one of these six. A rule that cannot trace to one is a candidate for removal.

---

## THE JUSTIFICATION TEST

Before adding any routing rule, modifying any existing rule, or removing any rule from the THALAMUS architecture — apply this test:

> *Does this rule serve epistemic precision? Which of the six principles does it implement? If it implements none — it should not exist.*

This is the Vinaya principle (F21) applied as an operational gate. The six principles are the constitutional layer. Individual routing rules are implementations of these principles — not independent constructs.

---

## PRINCIPLE 1 — CLASSIFY FIRST

> *Before any routing fires, classify the input.*

**What it means:** No routing decision is made on an unclassified input. Classification is not a preliminary step that can be skipped under time pressure — it is the mechanism that makes routing correct. Every routing error in every system traces back to either no classification or incorrect classification.

**What it prevents:** Routing by assumption. A system that routes without classifying is guessing — it happens to work until conditions change, and then it fails in ways that are difficult to diagnose because the failure occurred before the routing decision.

**Where it lives in THALAMUS:**
- `classification/INPUT-TAXONOMY.md` — the full taxonomy
- `classification/SEVERITY-TIERS.md` — the severity dimension
- `classification/SESSION-STATE-INPUTS.md` — the session dimension
- `routing/ROUTING-MASTER.md` — never receives unclassified input

**Findings that establish this principle:** F3 (FBI DIOG · NARA), F11 (Library of Congress), F17 (Hajj multi-factor)

---

## PRINCIPLE 2 — SEVERITY TIERS

> *Input type alone is insufficient. Severity modifies every routing chain.*

**What it means:** The same input type at different severity levels routes differently — different confirmation requirements, different logging depth, different escalation thresholds. A NOMINAL framework build and an ALERT framework build both route to AION-BRAIN. They do not receive the same treatment when they get there.

**What it prevents:** Uniform routing that treats a routine operation identically to a crisis operation. Systems that apply the same routing protocol regardless of severity cannot respond appropriately when conditions change — they are calibrated for one condition and wrong for all others.

**The four tiers:**

| Tier | Condition | Routing behavior |
|------|-----------|-----------------|
| NOMINAL | Standard operating conditions | Fast path. Auto-clearance. |
| ELEVATED | Conditions above baseline but manageable | Standard confirmation required. |
| ALERT | Conditions requiring active monitoring | Adversarial mode flags active. Double confirmation. |
| GENERAL | Catastrophic or architect-level conditions | Full stop. No routing until architect review. |

**Findings that establish this principle:** F7 (NRC · FAA · FEMA), F20 (Sanhedrin asymmetric thresholds), F25 (HICS MBO evaluate → plan → assign)

---

## PRINCIPLE 3 — CONFIRM EVERY HOP

> *No routing event is complete until the receiving node confirms receipt.*

**What it means:** Dispatch is not delivery. A routing event is in PENDING_ACK state until the destination repo confirms receipt. If confirmation does not arrive within the timeout threshold — retransmit. If second attempt fails — FAILED state, architect notification. Nothing is ever silently assumed to have arrived.

**What it prevents:** Silent failures. The most dangerous failure mode in any routing system is a failure that looks like success. A dispatch that was never received looks identical to a dispatch that was successfully processed — until the downstream gap is discovered, usually much later, under worse conditions.

**The confirmation sequence:**
```
DISPATCH → PENDING_ACK (timer starts)
  ↓
ACK received → ACKNOWLEDGED → ESTABLISHED → proceed
  ↓
No ACK (timeout) → RETRANSMIT (second attempt)
  ↓
Second ACK failure → FAILED → architect notification
```

**Findings that establish this principle:** F8 (FAA ATC handoff), F22 (TCP RFC 793 ACK), F28 (Mongol Yam relay)

---

## PRINCIPLE 4 — STATES NOT EVENTS

> *Routing has a lifecycle. Every stage is named. Unnamed stages are failure modes.*

**What it means:** A routing event is not a single moment — it is a process that moves through named states from RECEIVED to CLOSED. At any moment, every active routing event is in exactly one named state. A routing event that cannot be assigned to a named state is UNKNOWN, and UNKNOWN is treated as FAILED.

**What it prevents:** Unmonitored conditions. When routing is understood as a single event — "it was sent" — there is no mechanism to detect partial failure. A routing event that was dispatched but never acknowledged, or acknowledged but never established, is invisible without a state machine. The state machine makes every stage of the lifecycle visible.

**The state machine:**
```
RECEIVED → CLASSIFYING → CLASSIFIED → ROUTING → DISPATCHED
→ PENDING_ACK → ACKNOWLEDGED → ESTABLISHED → CLOSING → CLOSED

Special states:
HELD      — AMYGDALA hold. Nothing proceeds.
RETRANSMIT — ACK timeout. Second attempt.
ABNORMAL  — Normal state machine suspended.
UNKNOWN   — State cannot be determined. Treated as FAILED.
```

**Findings that establish this principle:** F23 (TCP state machine), F24 (HICS closed loop)

---

## PRINCIPLE 5 — CLOSED LOOP

> *Routing evaluation returns to THALAMUS. Open loops drift.*

**What it means:** Every routing event produces feedback — did the routing achieve its objective? That feedback returns to THALAMUS via RELAY-LOG and eventually informs the LEARNING-LAYER. A routing architecture without feedback cannot improve. It can only repeat its patterns until conditions change and the patterns fail.

**What it prevents:** Routing drift. A system that routes without feedback accumulates routing decisions that were once optimal and are no longer — stale paths that nobody has reviewed because there is no mechanism that makes the staleness visible.

**The feedback flow:**
```
ROUTING EVENT CLOSES
    ↓
RELAY-LOG entry appended (handoff/)
    ↓
HIPPOCAMPUS accumulates routing history
    ↓
LEARNING-LAYER reads reinforcement/decay signals
    ↓
ROUTING-MASTER weights updated
    ↓
THALAMUS routes differently — the loop closed
```

**Findings that establish this principle:** F13 (slime mold reinforcement), F14 (ant stigmergy decay), F24 (HICS closed loop), F31 (Auftragstaktik visibility ceiling)

---

## PRINCIPLE 6 — PRINCIPLE OVER RULES

> *Every routing rule justifies itself against epistemic precision. Rules that cannot — should not exist.*

**What it means:** The Theravada Vinaya is the most durable organizational system in recorded history — 2,500 years of continuous operation across cultures and civilizations. It operates on principle, not hierarchy. No power vested in individuals. Every rule exists because it serves the mission. Rules that no longer serve the mission are removed. This is not flexibility — it is structural integrity.

**What it prevents:** Rule accumulation. Every organization accumulates rules over time. Rules added for specific conditions remain after the conditions pass. Rules added by individuals with authority persist after those individuals leave. A rule layer without a justification gate becomes a constraint layer that no longer serves the architecture — only its own inertia.

**The test for any routing rule:**
1. Which of the six principles does this implement?
2. How does it serve epistemic precision specifically?
3. What failure mode does it prevent?
4. If removed — what would break?

If questions 1–3 cannot be answered clearly, the rule is a candidate for removal. If question 4 cannot be answered — the rule is decorative, not structural.

**Findings that establish this principle:** F15 (Jesuit Constitutions), F21 (Theravada Vinaya), F35 (Dr. Strangelove — architecture serves integrity), F36 (Reagan standard — inevitable not invented)

---

## THE SIX IN ONE VIEW

| # | Principle | Prevents | Key Findings |
|---|-----------|----------|-------------|
| 1 | **Classify First** | Routing by assumption | F3 · F11 · F17 |
| 2 | **Severity Tiers** | Uniform treatment of unequal conditions | F7 · F20 · F25 |
| 3 | **Confirm Every Hop** | Silent failures | F8 · F22 · F28 |
| 4 | **States Not Events** | Unmonitored conditions | F23 · F24 |
| 5 | **Closed Loop** | Routing drift | F13 · F14 · F24 · F31 |
| 6 | **Principle Over Rules** | Rule accumulation | F15 · F21 · F35 · F36 |

---

*6-CORE-PRINCIPLES.md — principles/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*Every routing rule traces to one of these six. If it cannot — it should not exist.*

