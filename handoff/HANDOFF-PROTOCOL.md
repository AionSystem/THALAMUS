# HANDOFF-PROTOCOL.md
## Transfer of Routing Authority — No Silent Handoffs

**Classification:** `[D]` Operational layer — governs every cross-repo transfer of routing authority
**Build step:** 4 of 7
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Principle:** F8 (FAA ATC handoff) · F22 (TCP ACK) · F28 (Mongol Yam relay) · F10 (NASA CAPCOM)

---

## THE CORE RULE

> *A routing event does not transfer authority until the receiving node explicitly confirms receipt. Dispatch is not delivery. Transmission is not transfer.*

`[D]` FAA Air Traffic Control handoff protocol: when one controller passes an aircraft to the next sector, the originating controller retains authority until the receiving controller explicitly acknowledges the handoff. The aircraft does not change jurisdiction on transmission — it changes jurisdiction on confirmation. No aircraft is ever in an ambiguous jurisdictional state. The handoff is a discrete event with a confirmed completion moment.

`[D]` TCP RFC 793: when a segment transmits, a copy enters the retransmission queue and a timer starts. The copy is not deleted until acknowledgment arrives. The originating node does not consider the transfer complete until confirmation. If confirmation does not arrive — retransmit. The segment is never "probably received."

`[R]` THALAMUS implements both principles simultaneously. Every cross-repo dispatch retains a pending record until ACK is received. Authority transfers at ACK — not at dispatch. RELAY-LOG entry is written at authority transfer — not at dispatch.

---

## THE HANDOFF SEQUENCE

```
STEP 1 — PRE-HANDOFF CONFIRMATION
  THALAMUS verifies destination repo is in registry (repo-registry.yml)
  THALAMUS verifies destination repo status is ACTIVE
  THALAMUS verifies AMYGDALA parallel dispatch is active
  IF any check fails → do not dispatch, route to ABNORMAL

STEP 2 — DISPATCH
  Payload assembled: {classification_vector, session_timestamp, routing_rule, payload_hash}
  Dispatch event fired via repository_dispatch API
  Payload copy retained in THALAMUS pending queue
  Timer starts (see ACK-SPEC.md for timeout thresholds)
  Routing event enters DISPATCHED state

STEP 3 — PENDING
  THALAMUS holds routing authority
  Destination repo has received dispatch but authority has not transferred
  Timer tracking active
  No output exits the brain during this window

STEP 4 — ACKNOWLEDGMENT
  Destination repo fires ACK event back to THALAMUS
  ACK payload includes: {event_id, payload_hash, received_timestamp}
  THALAMUS verifies payload_hash matches dispatch payload_hash
  IF hash matches → authority transfers, routing event enters ACKNOWLEDGED
  IF hash mismatch → route to ABNORMAL (ACK_MISMATCH)
  IF timeout → RETRANSMIT (see ACK-SPEC.md)

STEP 5 — AUTHORITY TRANSFER
  Pending payload copy deleted from THALAMUS queue
  Routing event enters ESTABLISHED
  RELAY-LOG entry written: authority transferred at [ACK timestamp]
  Destination repo now holds routing authority for this event

STEP 6 — RETURN
  When destination repo completes processing:
  Destination repo signals CLOSING to THALAMUS
  AMYGDALA clearance confirmed
  RELAY-LOG entry updated: event CLOSED
  Authority returns to THALAMUS (event complete)
```

---

## WHAT "AUTHORITY" MEANS

Routing authority is not a technical permission — it is an architectural accountability assignment. At any moment, exactly one node holds authority for each active routing event. That node is accountable for the event's current state.

| State | Authority holder |
|-------|-----------------|
| RECEIVED → DISPATCHED | THALAMUS |
| PENDING_ACK | THALAMUS (pending transfer) |
| ACKNOWLEDGED → CLOSING | Destination repo |
| CLOSING confirmation | AMYGDALA (clearance gate) |
| CLOSED | THALAMUS (event complete, in RELAY-LOG) |
| HELD | Architect |
| FAILED | No authority — event terminated, architect notification |

Authority is never shared. It is held by exactly one node and transfers discretely at the ACK moment.

---

## MULTI-HOP HANDOFFS

Some routing events involve more than one intermediate node before reaching the final destination. The Mongol Yam principle (F28) applies: confirmation at every hop, not just at final delivery.

Example: FRAMEWORK_BUILD → AION-BRAIN triggers FCL → HIPPOCAMPUS

```
HOP 1: THALAMUS → AION-BRAIN
  ACK from AION-BRAIN → authority transfers
  RELAY-LOG: Hop 1 confirmed

HOP 2 (cascade): AION-BRAIN → HIPPOCAMPUS (FCL candidate)
  THALAMUS dispatches second event simultaneously
  ACK from HIPPOCAMPUS → authority transfers for second event
  RELAY-LOG: Hop 2 confirmed

Both hops confirmed independently. Neither hop's confirmation substitutes for the other.
```

Multi-hop events generate one RELAY-LOG entry per hop — not one entry for the full chain. The chain is visible in the log by tracing entries with the same `cascade_source` field.

---

## THE NON-SILENT RULE

Every transfer of routing authority produces:
1. A RELAY-LOG entry with ACK timestamp
2. A STATE-REGISTRY update
3. A payload hash verification

If any of these three outputs cannot be produced — the handoff is incomplete. An incomplete handoff leaves the routing event in DISPATCHED state with no confirmed authority holder. This is treated as a diagnostic condition, not a normal operating state.

**Silent handoffs are categorically prohibited.** A handoff that appears to complete without a RELAY-LOG entry has not completed. It has disappeared — which is worse than failing.

---

## HANDOFF FAILURES AND RESPONSES

| Failure | Condition | Response |
|---------|-----------|----------|
| `DEST_UNREACHABLE` | Dispatch API call fails | Do not enter DISPATCHED. Route to ABNORMAL. |
| `ACK_TIMEOUT` | Timer expires before ACK | RETRANSMIT once. Second timeout → FAILED. |
| `ACK_MISMATCH` | Hash in ACK doesn't match dispatch | Route to ABNORMAL. Do not transfer authority. |
| `RELAY_LOG_FAILURE` | RELAY-LOG write fails | Hold in CLOSING. Retry log write. Do not enter CLOSED without log entry. |
| `AMYGDALA_UNAVAILABLE` | AMYGDALA clearance unreachable | Hold in ESTABLISHED. Do not enter CLOSING. Architect notification. |

---

*HANDOFF-PROTOCOL.md — handoff/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*Dispatch is not delivery. Authority transfers at ACK — never before.*

