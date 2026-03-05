# ORCHESTRATION-GUIDE.md
## GitHub Actions Workflow Architecture — How the Automation Layer Works

**Classification:** `[D]` Operational layer — read before deploying any workflow
**Build step:** 7 of 7 — final folder, built last
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Principle:** F10 (NASA CAPCOM — single external channel) · F36 (Reagan Standard — inevitable architecture)

---

## WHAT THIS FOLDER IS

The `orchestration/` folder contains the GitHub Actions workflows that automate the THALAMUS routing layer at the platform level. These are not the conceptual routing rules — those live in `routing/ROUTING-MASTER.md`. These are the operational automation layer that executes those rules against the actual GitHub repos.

Every workflow in this folder corresponds to a declared operation in the architecture. Nothing automates silently. Nothing fires without a traceable declared rule.

---

## THE FIVE WORKFLOWS

| File | Event type | What it does |
|------|-----------|-------------|
| `thalamus-master.yml` | `repository_dispatch: thalamus-route` | Master dispatcher — receives routing directive, fires correct sub-workflow |
| `scan-trigger.yml` | `repository_dispatch: run-structure-scanner` | Deploys AION Structure Scanner across target repo |
| `manifest-sync.yml` | `repository_dispatch: rebuild-master-manifest` | Triggers AGI manifest rebuild |
| `amygdala-check.yml` | `repository_dispatch: clearance-request` | Sends content to AMYGDALA for clearance assessment |
| `memory-write.yml` | `repository_dispatch: fcl-candidate-incoming` | Writes FCL candidate to HIPPOCAMPUS |

---

## THE CAPCOM PRINCIPLE (F10)

Every external dispatch goes through THALAMUS. No repo dispatches directly to another repo without passing through THALAMUS first.

```
AGI does not dispatch directly to AION-BRAIN.
AION-BRAIN does not dispatch directly to HIPPOCAMPUS.
Every cross-repo event: THALAMUS → destination.
```

This is NASA Mission Control's CAPCOM rule: the single external communication channel. The astronaut hears one voice. The crew hears one channel. All other communication routes through it. This is not bureaucracy — it is the architecture that prevents crossed signals, duplicate instructions, and authority conflicts.

In THALAMUS: every repo is an astronaut. THALAMUS is CAPCOM. All cross-repo traffic routes through the master dispatcher.

---

## AUTHENTICATION

All workflows use `THALAMUS_DISPATCH_TOKEN` — a GitHub Personal Access Token with `repo` and `workflow` scopes.

**Where it lives:** THALAMUS repository secrets. Not in any workflow file. Not committed anywhere.

**Rotation:** 90-day schedule. Architect calendar reminder required.

**Scope:** `repo` scope allows reading and writing to all AionSystem repos. `workflow` scope allows triggering workflow dispatch events.

**Critical:** Never hardcode the token. Never log it. Every workflow references it as `${{ secrets.THALAMUS_DISPATCH_TOKEN }}`.

---

## PAYLOAD ARCHITECTURE

Every dispatch carries a structured payload. Consistent payload structure means any repo can parse a THALAMUS dispatch with the same parser.

**Standard payload fields:**
```json
{
  "event_type": "thalamus-route",
  "client_payload": {
    "event_id": "RE-YYYYMMDD-N",
    "class": "FRAMEWORK_BUILD",
    "severity": "NOMINAL",
    "destination": "AION-BRAIN",
    "mode": "NORMAL",
    "session_timestamp": "2026-03-05T15:51:05-05:00",
    "payload_hash": "[sha256 of content]",
    "adversarial_mode": false,
    "notes": ""
  }
}
```

**Adversarial mode flag:** When adversarial mode is active, `adversarial_mode: true` is included in every payload. Destination repos read this flag and apply heightened logging accordingly.

---

## ACK RETURN ARCHITECTURE

Destination repos return ACKs by dispatching back to THALAMUS:

```json
{
  "event_type": "thalamus-ack",
  "client_payload": {
    "event_id": "RE-YYYYMMDD-N",
    "payload_hash": "[must match dispatch]",
    "received_timestamp": "ISO",
    "ack_timestamp": "ISO",
    "repo": "AION-BRAIN",
    "status": "RECEIVED"
  }
}
```

THALAMUS `thalamus-master.yml` listens for `thalamus-ack` events and processes them through the three-check verification (ACK-SPEC.md).

---

## DEPLOYMENT ORDER

Deploy workflows in this order. Each step depends on the previous:

```
STEP 1: Store THALAMUS_DISPATCH_TOKEN in THALAMUS repo secrets
STEP 2: Deploy thalamus-master.yml to THALAMUS repo
STEP 3: Deploy scan-trigger.yml to THALAMUS repo
STEP 4: Deploy manifest-sync.yml to THALAMUS repo
STEP 5: Deploy amygdala-check.yml to THALAMUS repo
STEP 6: Deploy memory-write.yml to THALAMUS repo
STEP 7: Test with a SCAN_ALL event — verify all 8 repos receive and ACK
STEP 8: Confirm MANIFEST_SYNC cascade fires after SCAN_ALL
STEP 9: Declare orchestration layer ACTIVE in STATE-REGISTRY
```

---

## WORKFLOW FILE LOCATIONS

All workflows live in THALAMUS repo at `.github/workflows/`. The orchestration/ folder in this repo contains the source documents. The deployed versions live in the GitHub Actions location.

```
THALAMUS repo (GitHub):
  .github/
    workflows/
      thalamus-master.yml
      scan-trigger.yml
      manifest-sync.yml
      amygdala-check.yml
      memory-write.yml

THALAMUS repo (this folder):
  orchestration/
    ORCHESTRATION-GUIDE.md   ← this document
    thalamus-master.yml       ← source
    scan-trigger.yml          ← source
    manifest-sync.yml         ← source
    amygdala-check.yml        ← source
    memory-write.yml          ← source
```

---

## FAILURE HANDLING IN WORKFLOWS

Every workflow step that can fail has explicit failure handling. No silent failures.

**Standard pattern:**
```yaml
- name: Dispatch to destination
  id: dispatch
  run: |
    response=$(curl -s -o /dev/null -w "%{http_code}" ...)
    if [ "$response" != "204" ]; then
      echo "DISPATCH FAILED: HTTP $response"
      exit 1
    fi

- name: Log failure to RELAY-LOG
  if: failure()
  run: |
    echo "Dispatch failed at $(date -u +%Y-%m-%dT%H:%M:%SZ)"
    # Log failure event
```

Workflow failures are not silent GitHub Actions red marks — they are routing events that enter FAILED state and generate RELAY-LOG entries.

---

*ORCHESTRATION-GUIDE.md — orchestration/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*Every workflow corresponds to a declared rule. Nothing automates silently.*
