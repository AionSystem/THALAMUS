# ACK-SPEC.md
## Acknowledgment Specification — Format, Thresholds, Retransmit Rules

**Classification:** `[D]` Operational layer — the technical specification for all ACK events
**Build step:** 4 of 7
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Principle:** F22 (TCP RFC 793) · F8 (FAA ATC) · F7 (severity tiers modify thresholds)

---

## THE ACK CONTRACT

Every dispatch from THALAMUS creates an ACK contract: the destination repo commits to returning a confirmation signal within the defined timeout threshold. THALAMUS commits to retransmitting once if the threshold is exceeded. Both sides of the contract are specified here.

---

## ACK PAYLOAD FORMAT

Every ACK returned from a destination repo must contain these fields:

```yaml
ack_payload:
  event_id: [RE-YYYYMMDD-N]          # Matches the dispatched event ID exactly
  payload_hash: [hash]                # Must match the dispatch payload hash
  received_timestamp: [ISO timestamp] # When the destination received the dispatch
  ack_timestamp: [ISO timestamp]      # When the ACK was generated
  repo: [repo name]                   # Which repo is returning the ACK
  status: RECEIVED                    # Always RECEIVED on ACK — not PROCESSED
  notes: ""                           # Optional — any flags or conditions
```

**`status: RECEIVED` not `status: PROCESSED`:** The ACK confirms the destination received the dispatch. It does not confirm processing is complete. Processing completion is signaled separately when the event enters CLOSING state. Conflating receipt with processing is a handoff failure.

---

## TIMEOUT THRESHOLDS BY REPO AND SEVERITY

Thresholds reflect the operational characteristics of each repo — some operations are fast (HIPPOCAMPUS memory write), some are slower (AION-BRAIN framework build).

| Repo | NOMINAL timeout | ELEVATED timeout | ALERT timeout |
|------|----------------|-----------------|---------------|
| AGI | 30s | 60s | 120s |
| AION-BRAIN | 60s | 120s | 300s |
| OCEAN-BRAIN | 60s | 120s | 300s |
| HIPPOCAMPUS | 15s | 30s | 60s |
| AMYGDALA | 30s | 60s | 120s |
| SYNARA | 20s | 40s | 90s |
| CEREBELLUM | 20s | 40s | 90s |
| PREFRONTAL | 20s | 40s | 90s |

**GENERAL severity:** No timeout threshold — HELD state, architect review. Clock doesn't run under GENERAL.

**Timeout rationale:**
- HIPPOCAMPUS is fastest (15s NOMINAL) — memory writes are low-complexity, near-instantaneous
- AION-BRAIN and OCEAN-BRAIN are slowest (60s NOMINAL) — framework builds and research synthesis have genuine depth requirements
- ALERT timeouts are 4–5x NOMINAL — adversarial mode slows everything deliberately (F5)

---

## RETRANSMIT RULES

**One retransmit per event. Maximum.**

```
TIMEOUT FIRES
      ↓
IF retransmit_count = 0:
  Retransmit dispatch fired (same payload, retransmit flag added)
  retransmit_count set to 1
  Same timeout threshold restarts
  IF ACK received within threshold → ACKNOWLEDGED
  IF second timeout → FAILED (no third attempt)

IF retransmit_count = 1:
  Second timeout → FAILED immediately
  No third attempt authorized without architect review
```

**Why one retransmit:** Two consecutive ACK failures indicate a structural problem — not a transient one. A third attempt without architectural diagnosis is persistence in the face of evidence. The architecture stops and reports rather than continuing to attempt.

**Retransmit payload modification:**
```yaml
# Added to original payload on retransmit:
retransmit: true
retransmit_timestamp: [ISO timestamp]
original_dispatch_timestamp: [original T1]
```

The retransmit flag allows the destination repo to distinguish a first dispatch from a retransmit — preventing duplicate processing if the original dispatch was received but the ACK was lost.

---

## THE IDEMPOTENCY REQUIREMENT

All destination repo dispatch handlers must be idempotent: receiving the same event_id twice must produce the same result as receiving it once.

If a dispatch was received but the ACK was lost in transit, the retransmit will deliver the same event_id again. The destination repo must:
1. Detect the duplicate by checking event_id against recent received events
2. Return a new ACK for the duplicate
3. Not reprocess the payload

This prevents the retransmit mechanism from causing duplicate processing while still ensuring the ACK contract is honored.

---

## ACK VERIFICATION AT THALAMUS

When THALAMUS receives an ACK, it runs three checks before accepting it:

```
CHECK 1 — EVENT ID MATCH
  Does ack_payload.event_id match the pending dispatch event_id?
  IF NO → discard ACK, log as ACK_ORPHAN, continue waiting

CHECK 2 — PAYLOAD HASH MATCH
  Does ack_payload.payload_hash match the dispatch payload_hash?
  IF NO → route to ABNORMAL (ACK_MISMATCH), do not transfer authority

CHECK 3 — REPO MATCH
  Does ack_payload.repo match the intended destination in routing vector?
  IF NO → discard ACK, log as ACK_WRONG_SOURCE, continue waiting
```

All three checks must pass. A partial match is not an ACK — it is an anomaly requiring diagnosis.

---

## AMYGDALA ACK — SPECIAL HANDLING

AMYGDALA ACK returns one of three status values (not just RECEIVED):

```yaml
amygdala_ack:
  event_id: [RE-YYYYMMDD-N]
  payload_hash: [hash]
  received_timestamp: [ISO]
  ack_timestamp: [ISO]
  repo: AMYGDALA
  clearance_status: CLEARED | CLEARED_WITH_FLAGS | HELD
  flags: []          # Populated if clearance_status = CLEARED_WITH_FLAGS
  held_reason: ""    # Populated if clearance_status = HELD
```

AMYGDALA is the only repo whose ACK carries a clearance decision. All other repos return `status: RECEIVED` only — they confirm receipt, not approval. AMYGDALA confirms receipt AND issues a clearance ruling in a single ACK.

**Why AMYGDALA combines receipt and clearance:** Separating them would require two round-trips and create a window where THALAMUS knows the content was received but doesn't know the clearance status — an ambiguous state. Combined ACK eliminates the ambiguity.

---

## RELAY-LOG ACK RECORD

Every successful ACK generates a RELAY-LOG entry:

```yaml
relay_log_ack:
  entry_type: ACK_RECEIVED
  event_id: [RE-YYYYMMDD-N]
  repo: [destination]
  dispatch_timestamp: [when dispatch fired]
  ack_timestamp: [when ACK received]
  elapsed_ms: [ack_timestamp minus dispatch_timestamp in milliseconds]
  payload_hash_verified: true
  retransmit: [false or true]
  amygdala_clearance: [if applicable]
```

The `elapsed_ms` field feeds LEARNING-LAYER when it becomes active — faster ACK times on a routing path are a reinforcement signal.

---

*ACK-SPEC.md — handoff/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*One retransmit maximum. Two failures is a structural signal. Stop and diagnose.*

