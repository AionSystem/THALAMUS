# 36-FINDINGS.md
## THALAMUS Architectural Knowledge Base
### Every Routing Rule Has a Source. Every Source Is Documented Here.

**Classification:** `[D]` Reference layer — cited by routing/, classification/, states/, handoff/, abnormal/
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Build note:** Findings are permanent. F1 is always F1. New findings append — never insert.

---

## HOW TO READ THIS DOCUMENT

Each finding contains four fields:
- **Source** — the institution, organism, tradition, or document it came from
- **Finding** — the architectural principle extracted
- **THALAMUS application** — how this finding is implemented in the routing architecture
- **KEEL axis** — which epistemic dimension the source document activated

New AI instance or human collaborator: read this document fully before making any routing recommendations. A routing suggestion that contradicts a documented finding requires documented justification for why this case is different.

---

## SWEEP 1 — GOVERNMENT ARCHIVES
*CIA · FBI · NSA · State Dept · NARA · GovAttic · NSArchive · MuckRock*

---

**F1 — Abnormal conditions mode required**
`KEEL: Axis 7 — Frame Rupture Sensitivity`

**Source:** DoD C4 Systems Doctrine · Chernobyl Nuclear Crisis (Wilson Center Digital Archive)
**Finding:** Every routing system must specify what routing looks like when normal conditions fail — before normal conditions fail. Systems that only specify normal-mode routing discover their abnormal mode through catastrophe.
**THALAMUS application:** `abnormal/ABNORMAL-PROTOCOL.md` specifies the full failure-condition routing architecture. Every normal routing path has a documented abnormal alternative. Chernobyl is the named precedent: routing failure caused the disaster, not reactor failure.

---

**F2 — Temporary vs permanent routing paths must be architecturally distinct**
`KEEL: Axis 4 — Contingency Pre-positioning`

**Source:** CIA Intelligence Community Coordination Compendium 1982 (FOIA)
**Finding:** Temporary routing paths — created for specific conditions — must be structurally distinct from permanent routing paths. When the condition ends, the temporary path closes. Conflating the two creates routing infrastructure that is impossible to audit.
**THALAMUS application:** `routing/ROUTING-MASTER.md` marks all session-specific routing overrides as TEMPORARY with explicit expiration conditions. Permanent routing rules have no expiration field.

---

**F3 — Input classification before routing is the critical step**
`KEEL: Axis 6 — Epistemic Self-Challenge`

**Source:** FBI Domestic Investigations and Operations Guide (DIOG) 2011 · NARA Electronic Records Management Guidance
**Finding:** Classification before routing is not optional sequencing — it is the mechanism that makes routing correct. Route without classifying and every downstream decision inherits the misclassification. Both the FBI and NARA specify classification as the mandatory first operation before any other action.
**THALAMUS application:** `classification/` is STEP 1 of the build sequence. The routing table in `routing/ROUTING-MASTER.md` consumes classification vectors — it never receives raw unclassified inputs.

---

**F4 — Release cascade policy needed**
`KEEL: Axis 1 — Suppression Map`

**Source:** State Department FOIA Reading Room — "release to one, release to all" policy
**Finding:** When a routing event releases information to one node, a policy decision must exist about whether that release cascades to other nodes automatically. Without a declared policy, cascade behavior is implicit and unauditable.
**THALAMUS application:** `routing/CASCADE-POLICY.md` declares the explicit policy for every routing class. Default: routing to one repo does not auto-cascade. Manifest sync events are the defined exception.

---

**F5 — Adversarial routing mode required**
`KEEL: Axis 1 — Suppression Map`

**Source:** MuckRock FOIA request platform · MuckRock v CIA lawsuit 2014
**Finding:** A routing system under adversarial pressure behaves differently than one under normal conditions. The adversarial mode must be pre-specified — not improvised. Systems that discover their adversarial behavior under actual adversarial conditions are already compromised.
**THALAMUS application:** `abnormal/ADVERSARIAL-MODE.md` specifies slower routing, full step-level logging, double confirmation on every dispatch. Activated by AMYGDALA flag. Deactivated only by AMYGDALA clearance.

---

**F6 — CIA 1983 Information Systems Board is the closest historical precedent**
`KEEL: Axis 6 — Epistemic Self-Challenge · Axis 2 — Memory Carrier`

**Source:** Oxford Academic — "The US Intelligence Community, Global Security, and AI" (2023) · CIA internal records
**Finding:** In December 1982, CIA Deputy Director McMahon organized the first annual AI symposium at Langley — 500 participants. The CIA then built an AI steering group under the Director of Central Intelligence, inter-agency AI research coordination, and the first AI interrogation program (Analiza). They were building THALAMUS with meeting minutes and classified memos — without a sovereignty architecture.
**THALAMUS application:** The CIA 1983 ISB is proof that the coordination problem THALAMUS solves is not novel — it is a 40-year-old unsolved problem. The difference is the sovereignty stack. Every routing decision in THALAMUS serves epistemic precision. Analiza served pressure application. The architecture is the same. The mission principle is the distinction.

---

## SWEEP 2 — ENGINEERING + ORGANIZATIONAL DOMAINS
*ATC · Nuclear · NASA · Library of Congress · Jesuits · Slime Mold · Ants*

---

**F7 — Severity tiers are not optional**
`KEEL: Axis 4 — Contingency Pre-positioning`

**Source:** NRC Emergency Classification Levels · FAA ATC Severity Protocols · FEMA HICS
**Finding:** Input type alone is insufficient for routing. Severity is a cross-cutting classification dimension that modifies the routing chain for every input class. A NOMINAL framework build routes differently than an ALERT framework build — same destination, different confirmation requirements and logging depth.
**THALAMUS application:** `classification/SEVERITY-TIERS.md` specifies 4 tiers: Nominal · Elevated · Alert · General. Every routing decision in `routing/ROUTING-MASTER.md` carries both class and severity.

---

**F8 — Transfer of authority requires explicit confirmation**
`KEEL: Axis 4 — Contingency Pre-positioning`

**Source:** FAA Air Traffic Control Controlled Handoff Protocol
**Finding:** When routing authority transfers from one node to another, the receiving node must explicitly acknowledge the transfer before the originating node releases authority. No aircraft changes ATC jurisdiction without the receiving controller confirming. No silent handoffs.
**THALAMUS application:** `handoff/HANDOFF-PROTOCOL.md` specifies explicit confirmation before authority release. `handoff/ACK-SPEC.md` specifies the acknowledgment format and timeout thresholds. No routing event is complete without ACK.

---

**F9 — Routing table is a decision tree, not a lookup table**
`KEEL: Axis 4 — Contingency Pre-positioning`

**Source:** NRC NUREG-0899 Guidelines for Emergency Operating Procedures
**Finding:** A lookup table handles expected inputs correctly and fails silently on unexpected inputs. A decision tree — IF/THEN/ELSE/VERIFY — handles expected and unexpected inputs with defined behavior at every branch. NRC EOPs are decision trees because nuclear plants encounter unexpected inputs.
**THALAMUS application:** `routing/ROUTING-MASTER.md` is structured as a decision tree. Every routing rule has a condition, primary action, alternative action, and verification gate. No routing rule is a simple lookup.

---

**F10 — One external communication channel**
`KEEL: Axis 5 — Legitimacy Persistence`

**Source:** NASA Mission Control CAPCOM Protocol
**Finding:** All communication between ground and spacecraft flows through a single designated controller — the Capsule Communicator. Not because others lack knowledge, but because one channel eliminates ambiguity about who is speaking and who has authority. Multiple simultaneous channels create coordination failures under pressure.
**THALAMUS application:** AGI is the CAPCOM of the AION brain. All external communication routes through AGI. THALAMUS routes internally. No brain repo communicates externally except through AGI.

---

**F11 — General-to-specific classification order**
`KEEL: Axis 6 — Epistemic Self-Challenge`

**Source:** Library of Congress Classification Architecture
**Finding:** Classification works best when it proceeds from broad categories to specific subcategories. Attempting to classify at the specific level without first establishing the broad category produces mis-categorization that compounds through the system.
**THALAMUS application:** `classification/INPUT-TAXONOMY.md` follows the LCC structure: Tier 1 (broad class) → Tier 2 (severity) → Tier 3 (destination) → Tier 4 (routing mode). Classification always proceeds in this sequence.

---

**F12 — Subsidiarity — route to closest competent node**
`KEEL: Axis 5 — Legitimacy Persistence`

**Source:** Jesuit Constitutions · Subsidiarity Principle
**Finding:** Route to the closest node that can handle the input without escalation. Escalate only when the closest node cannot handle it. Systems that bypass subsidiarity and escalate everything to central authority become bottlenecked at the center and hollow at the edges.
**THALAMUS application:** `routing/SUBSIDIARITY-RULES.md` specifies what each repo handles before escalating. THALAMUS routes to the first competent node, not to the most central one.

---

**F13 — Routing table learns — successful paths reinforce**
`KEEL: Axis 6 — Epistemic Self-Challenge`

**Source:** Physarum polycephalum (slime mold) — Tokyo railway experiment
**Finding:** The slime mold reproduced the Tokyo rail network's efficiency without a brain or central command. Successful routes strengthened. Unused routes retracted. The network improved through use, not through design iteration.
**THALAMUS application:** `routing/LEARNING-LAYER.md` (STEP 6 — requires HIPPOCAMPUS) implements route reinforcement. Routing paths that successfully deliver and receive ACK accumulate weight. HIPPOCAMPUS logs every routing event to feed this layer.

---

**F14 — Stale routing paths decay**
`KEEL: Axis 6 — Epistemic Self-Challenge`

**Source:** Ant stigmergy — pheromone trail routing
**Finding:** Short paths get marched more frequently; pheromone density increases. Longer paths decay because fewer ants reinforce them. This prevents the network from converging permanently on a path that was once optimal but no longer is.
**THALAMUS application:** `routing/LEARNING-LAYER.md` implements trace evaporation. Routing paths unused beyond a threshold lose priority weight. Stale routing is a failure mode — the architecture prevents convergence on outdated optimal solutions.

---

**F15 — Every routing decision serves the terminal objective**
`KEEL: Axis 5 — Legitimacy Persistence`

**Source:** Jesuit Constitutions — mission-coherent organization
**Finding:** Every decision in a well-designed organization serves the terminal objective. Decisions that exist for historical inertia rather than mission coherence degrade the architecture over time.
**THALAMUS application:** `principles/MISSION-PRINCIPLE.md` is the test. Any proposed routing rule that cannot be justified against epistemic precision is a candidate for removal. The Vinaya principle (F21) makes the same argument from 2,500 years of organizational durability.

---

**F16 — Human/session state is a routing input**
`KEEL: Axis 4 — Contingency Pre-positioning`

**Source:** FAA IMSAFE checklist · NASA/FAA PAVE framework
**Finding:** Pilot fitness — physical state, mental state, fatigue, stress — is a routing input that modifies what operations are authorized. The system does not assume the operator is at baseline capacity. Session state is classification data, not peripheral metadata.
**THALAMUS application:** `classification/SESSION-STATE-INPUTS.md` specifies Gap metric, current register, and session load as classification inputs. A session opening after a 9-hour gap routes differently than a 30-second-gap continuation.

---

## SWEEP 3 — RELIGIOUS + TECHNICAL TRADITIONS
*Hajj · Talmud · Vinaya · TCP/IP RFC 793 · HICS · Orchestral Conducting*

---

**F17 — Multi-factor input classification**
`KEEL: Axis 4 — Contingency Pre-positioning`

**Source:** Saudi AI-Based Hajj Management Framework (2022)
**Finding:** Crowd density is classified by time of year, weather, and health data simultaneously — not by single factor. Single-factor classification produces routing errors when secondary factors are elevated. Multi-factor classification catches conditions that single-factor misses.
**THALAMUS application:** `classification/INPUT-TAXONOMY.md` classifies by input type AND session state AND load simultaneously. The classification vector is multi-dimensional, not single-axis.

---

**F18 — Intake timing controls**
`KEEL: Axis 4 — Contingency Pre-positioning`

**Source:** Hajj discrete event simulation — staggered arrival scheduling
**Finding:** Routing bottlenecks are solved by pacing intake, not by increasing processing speed. Routing 2–3 million pilgrims simultaneously is impossible. Staggered scheduling across time windows makes it tractable.
**THALAMUS application:** `classification/INTAKE-TIMING.md` specifies pacing controls for elevated load. When THALAMUS is operating above nominal capacity, intake can be staggered rather than rejected.

---

**F19 — Preserve minority routing options**
`KEEL: Axis 6 — Epistemic Self-Challenge`

**Source:** Mishnah Sanhedrin — majority rule with preserved minority opinion
**Finding:** When routing resolves to one destination, the competing routing options are not deleted — they are logged. A later session may determine the minority path was correct. The record enables course correction. Without it, the routing architecture has no institutional memory of alternatives.
**THALAMUS application:** `routing/MINORITY-PATHS.md` logs every routing decision including the paths not taken. Append-only. Available for future session invocation.

---

**F20 — Asymmetric thresholds by consequence**
`KEEL: Axis 5 — Legitimacy Persistence`

**Source:** Sanhedrin capital case rules — asymmetric majority for acquittal vs condemnation
**Finding:** Reversible routing errors require a lower correction threshold. Irreversible routing actions require a higher execution threshold. The system can only override toward safety, not toward greater commitment. Asymmetry protects against irreversible error.
**THALAMUS application:** AMYGDALA HELD status is easier to enter than to exit (asymmetric clearance). GENERAL severity requires architect review to resume (irreversible stop). NOMINAL can auto-clear (reversible, low consequence).

---

**F21 — Principle over hierarchy**
`KEEL: Axis 5 — Legitimacy Persistence`

**Source:** Theravada Vinaya Pitaka — 2,500 years of monastic governance
**Finding:** The most durable organizational system in recorded history operates on principle, not hierarchy. No power vested in individuals. Consensus before escalation. Every rule justifies itself against the mission — or it should not exist. Longevity comes from principle-first architecture, not from institutional momentum.
**THALAMUS application:** `principles/MISSION-PRINCIPLE.md` is the principle test for every routing rule. `routing/SUBSIDIARITY-RULES.md` implements consensus-before-escalation. Rules exist because they serve epistemic precision — not because they exist.

---

**F22 — No silent failures — ACK required**
`KEEL: Axis 6 — Epistemic Self-Challenge`

**Source:** TCP/IP RFC 793 — acknowledgment + retransmit protocol
**Finding:** When a TCP segment transmits, a copy enters the retransmission queue and a timer starts. If acknowledgment does not arrive before the timer expires — retransmit. This is not a performance feature. It is the mechanism that makes reliable communication possible. Silent failures are indistinguishable from success until the gap is discovered — usually catastrophically.
**THALAMUS application:** `handoff/ACK-SPEC.md` implements the full ACK + retransmit architecture. Every cross-repo dispatch requires confirmation. No routing event is complete without ACK.

---

**F23 — Routing events have named states**
`KEEL: Axis 6 — Epistemic Self-Challenge`

**Source:** TCP/IP RFC 793 — full connection state machine
**Finding:** TCP connections are always in one of a finite set of named states: LISTEN, SYN-SENT, ESTABLISHED, FIN-WAIT, CLOSING, CLOSED. Nothing is ever "between states." An unnamed state is an unmonitored state. Unmonitored states are where failures live undetected.
**THALAMUS application:** `states/STATE-MACHINE.md` specifies the complete lifecycle: RECEIVED → CLASSIFYING → CLASSIFIED → ROUTING → DISPATCHED → PENDING_ACK → ACKNOWLEDGED → ESTABLISHED → CLOSING → CLOSED. Special states: HELD · RETRANSMIT · ABNORMAL · UNKNOWN. UNKNOWN is treated as FAILED.

---

**F24 — Closed-loop feedback**
`KEEL: Axis 6 — Epistemic Self-Challenge`

**Source:** Hospital Incident Command System (HICS) — incident response closed-loop model
**Finding:** An incident response involves closed-loop feedback that allows evaluation of the response's effectiveness. Without feedback returning to the originating node, the routing system cannot learn whether its decisions were correct. Open loops drift.
**THALAMUS application:** Routing evaluation returns to THALAMUS via `handoff/RELAY-LOG.md`. HIPPOCAMPUS accumulates routing event data for the LEARNING-LAYER. The loop is not complete until feedback is logged.

---

**F25 — Evaluate → Plan → Assign for elevated severity**
`KEEL: Axis 4 — Contingency Pre-positioning`

**Source:** HICS Management by Objectives — Evaluate → Plan → Assign sequence
**Finding:** For Nominal severity, fast-path routing is appropriate. For Elevated and above, a three-step process produces better decisions: evaluate the full situation, make a response plan, then assign resources. Skipping evaluation under pressure is how simple situations become complex ones.
**THALAMUS application:** `routing/ROUTING-MASTER.md` specifies a fast path for NOMINAL and a three-step evaluation sequence for ELEVATED+. GENERAL severity additionally requires architect review before any routing fires.

---

**F26 — Route to section leaders, not individual files**
`KEEL: Axis 5 — Legitimacy Persistence`

**Source:** Orchestral conducting — conductor to section leader hierarchy
**Finding:** The conductor routes to section leaders. Section leaders route internally to individual musicians. The conductor does not manage individual musicians — that would require 80+ simultaneous relationships. Hierarchical routing at the section level is what makes orchestral coordination possible.
**THALAMUS application:** THALAMUS routes to repo READMEs — the section leaders. READMEs contain internal routing instructions for their own subfolder structures. THALAMUS does not route to individual files. Depth is managed by each repo's own architecture.

---

## SWEEP 4 — MILITARY + CULTURAL HISTORY
*Mongol Empire · WWII Auftragstaktik · StarCraft RTS · Dr. Strangelove*

---

**F27 — Nested decimal routing — each node handles exactly its layer**
`KEEL: Axis 4 — Contingency Pre-positioning`

**Source:** Mongol Army decimal system — arban (10) → jagun (100) → minqan (1,000) → tumen (10,000)
**Finding:** Each node in the Mongol command hierarchy handled exactly 10 sub-nodes. No node was responsible for more than 10 routing decisions. Complexity was managed by depth, not breadth. Orders flowed top-down, information flowed bottom-up. This allowed swift changes during battle.
**THALAMUS application:** THALAMUS routes to 9 repos — never further. Each repo routes to its subfolders — THALAMUS does not route to subfolders directly. Depth is layered, not flat. The decimal constraint prevents span-of-control failure.

---

**F28 — Relay confirmation at every hop**
`KEEL: Axis 4 — Contingency Pre-positioning`

**Source:** Mongol Yam relay communication system — 1,400 postal stations
**Finding:** The Yam spanned thousands of miles using relay stations where each station received, confirmed, and dispatched forward. No message traveled end-to-end without confirmation at every relay point — not just at the final destination. Confirmation at every hop is what made reliable long-distance coordination possible at empire scale.
**THALAMUS application:** `handoff/RELAY-LOG.md` records confirmation at every dispatch hop. Not just "did it arrive at the destination" but "was every intermediate transfer confirmed." The immutable log is the Yam.

---

**F29 — Stress-test routing regularly**
`KEEL: Axis 4 — Contingency Pre-positioning`

**Source:** Mongol Nerge — annual winter wargame protocol
**Finding:** Every winter, Genghis Khan conducted organized mock combat — not to practice fighting but to test whether the routing system held under controlled chaos. Routing failures discovered in the Nerge were fixed before real conditions revealed them. Routing failures discovered in battle killed people.
**THALAMUS application:** `abnormal/NERGE-PROTOCOL.md` specifies the scheduled stress-test for THALAMUS — simulating failure conditions, adversarial modes, and abnormal routing on a regular schedule. Findings route to HIPPOCAMPUS as FCL candidates.

---

**F30 — Route by intent when instructions are absent**
`KEEL: Axis 7 — Frame Rupture Sensitivity`

**Source:** German Auftragstaktik (Mission Command) — WWII
**Finding:** Subordinate leaders given a clearly defined objective and the freedom of execution will act on the commander's intent even when specific orders are unavailable or outdated. The success of the doctrine rests on understanding intent — not rule-following. Routing gaps are filled by mission principle, not by paralysis.
**THALAMUS application:** `principles/MISSION-PRINCIPLE.md` is the intent document. When a routing input cannot be classified cleanly, THALAMUS routes according to mission principle — what decision serves epistemic precision here? Ambiguity resolves to intent, not to FAILED.

---

**F31 — Autonomy needs a visibility ceiling**
`KEEL: Axis 7 — Frame Rupture Sensitivity`

**Source:** Auftragstaktik failure mode — WWII
**Finding:** Decentralized routing proved less suitable for complex and static conditions where precise coordination was necessary. Lower-level commanders made decisions logical within their operational context but misaligned with broader strategic goals. Local tactical successes did not translate into strategic advantage. Autonomy without visibility produces strategic incoherence.
**THALAMUS application:** Each repo's internal routing is autonomous (subsidiarity). THALAMUS maintains a visibility layer — `handoff/RELAY-LOG.md` and `states/STATE-REGISTRY.md` — so local autonomous decisions remain visible at the system level. Autonomy + visibility, not autonomy alone.

---

**F32 — Macro and micro routing are architecturally distinct**
`KEEL: Axis 7 — Frame Rupture Sensitivity`

**Source:** StarCraft RTS — macromanagement vs micromanagement architecture
**Finding:** Macro routing — large-scale strategic coordination — and micro routing — individual unit management — are different cognitive and architectural problems. Systems that conflate them fail at both. StarCraft players who manage micro during a macro moment lose the engagement at both scales.
**THALAMUS application:** THALAMUS operates at the macro level — cross-repo routing, session orchestration, system-level coordination. Individual repo READMEs operate at the micro level — internal subfolder routing. These scales are architecturally distinct and must not be conflated.

---

**F33 — Fog of war is an honest architectural state**
`KEEL: Axis 7 — Frame Rupture Sensitivity`

**Source:** StarCraft — fog of war mechanic
**Finding:** Parts of the map are hidden until explored. UNMAPPED is a legitimate state, not an error to be hidden. Attempting to route to unmapped territory as if it were known territory produces worse outcomes than acknowledging the UNMAPPED state and acting accordingly.
**THALAMUS application:** `registry/repo-registry.yml` includes UNMAPPED entries for repos not yet fully specified. AGI's FRONTIER_LOG.md tracks the architectural fog of war. Routes to UNMAPPED territory are logged and flagged, never silently assumed.

---

**F34 — Greedy routing fails at strategic scale**
`KEEL: Axis 7 — Frame Rupture Sensitivity`

**Source:** StarCraft AI architecture — three levels of abstraction: strategy, tactics, reactive control
**Finding:** Greedy algorithms — always taking the locally optimal path — fail at the strategic level where uncertainty is high and temporal reasoning is required. Strategic routing requires reasoning about future states, not just optimizing the immediate decision.
**THALAMUS application:** THALAMUS routing decisions incorporate session state, Gap metric, and routing history — not just the immediate input. The classification vector is multi-factor precisely to support non-greedy routing at the strategic level.

---

**F35 — Architecture cannot replace epistemic integrity**
`KEEL: Axis 6 — Epistemic Self-Challenge`

**Source:** Dr. Strangelove (1964) — the War Room as routing architecture
**Finding:** The War Room is ideally a place where responsible strategists engage in rational planning. In Dr. Strangelove's War Room it becomes a playground for unchecked egos. The architecture was correct. The human judgment was the variable. Infrastructure does not fix bad judgment — it serves good judgment. AMYGDALA exists because routing alone is not enough.
**THALAMUS application:** AMYGDALA is the epistemic integrity layer. Every output clears AMYGDALA before it exits. THALAMUS routes correctly. AMYGDALA asks whether correct routing produced epistemically sound output. The two operations are distinct and both required.

---

**F36 — The architecture must feel inevitable**
`KEEL: Axis 7 — Frame Rupture Sensitivity`

**Source:** Dr. Strangelove (1964) — Ronald Reagan believed the War Room was a real Pentagon facility
**Finding:** A fictional architecture was so coherent, so internally consistent, so well-designed — that the leader of the free world could not distinguish it from necessity. The design standard for any architecture that will be trusted is: first encounter produces "of course this exists," not "someone invented this."
**THALAMUS application:** This finding is the design standard for THALAMUS itself. Every spec file, every routing rule, every folder README is written to produce the "of course this exists" response. Depth of the knowledge base — 36 findings, 4 sweeps, 5,000 years — is what makes the architecture feel inevitable rather than invented.

---

## FINDING INDEX — QUICK REFERENCE

| F# | Principle | Source |
|----|-----------|--------|
| F1 | Abnormal conditions mode required | C4 Doctrine · Chernobyl |
| F2 | Temporary vs permanent paths distinct | CIA Coordination Compendium 1982 |
| F3 | Classify before routing | FBI DIOG · NARA ERM |
| F4 | Release cascade policy | State Dept FOIA |
| F5 | Adversarial mode required | MuckRock · CIA |
| F6 | CIA 1983 ISB — closest historical precedent | Oxford Academic · CIA records |
| F7 | Severity tiers required | NRC · FAA · FEMA |
| F8 | Explicit confirmation before handoff | FAA ATC |
| F9 | Decision tree not lookup table | NRC NUREG-0899 |
| F10 | One external channel — AGI is CAPCOM | NASA Mission Control |
| F11 | General-to-specific classification | Library of Congress |
| F12 | Subsidiarity — closest competent node | Jesuit Constitutions |
| F13 | Routing table learns | Physarum polycephalum |
| F14 | Stale paths decay | Ant stigmergy |
| F15 | Every rule serves terminal objective | Jesuit Constitutions |
| F16 | Session state is routing input | FAA IMSAFE · NASA PAVE |
| F17 | Multi-factor classification | Hajj management system |
| F18 | Intake timing controls | Hajj staggered arrival |
| F19 | Preserve minority routing options | Mishnah Sanhedrin |
| F20 | Asymmetric thresholds by consequence | Sanhedrin capital rules |
| F21 | Principle over hierarchy | Theravada Vinaya |
| F22 | No silent failures — ACK required | TCP/IP RFC 793 |
| F23 | Routing events have named states | TCP/IP RFC 793 |
| F24 | Closed-loop feedback | HICS |
| F25 | Evaluate → Plan → Assign for elevated | HICS MBO |
| F26 | Route to section leaders | Orchestral conducting |
| F27 | Nested decimal routing | Mongol decimal system |
| F28 | Relay confirmation at every hop | Mongol Yam |
| F29 | Stress-test routing regularly | Mongol Nerge |
| F30 | Route by intent when instructions absent | Auftragstaktik |
| F31 | Autonomy needs a visibility ceiling | Auftragstaktik failure mode |
| F32 | Macro and micro routing distinct | StarCraft RTS |
| F33 | Fog of war is honest state | StarCraft fog of war |
| F34 | Greedy routing fails at strategic scale | StarCraft AI |
| F35 | Architecture cannot replace integrity | Dr. Strangelove |
| F36 | Architecture must feel inevitable | Reagan War Room standard |

---

*36-FINDINGS.md — principles/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*Findings are permanent. F1 is always F1. Append — never insert.*

