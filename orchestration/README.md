<div align="center">

# orchestration/

### *The Execution Layer — Where Specifications Become GitHub Actions*

[![Function](https://img.shields.io/badge/FUNCTION-Reusable_Workflow_Library-e94560?style=for-the-badge&labelColor=0d1117)]()
[![Build Step](https://img.shields.io/badge/BUILD_STEP-7_of_7-FFD700?style=for-the-badge&labelColor=0d1117)]()
[![Principle](https://img.shields.io/badge/PRINCIPLE-Code_Is_Last_Specs_Are_First-2d6a4f?style=for-the-badge&labelColor=0d1117)]()
[![Source](https://img.shields.io/badge/SOURCE-NASA_CAPCOM_%7C_ATC_%7C_TCP_%2F_RFC_793-4B0082?style=flat-square&labelColor=0d1117)]()
[![Runtime](https://img.shields.io/badge/RUNTIME-GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white&labelColor=0d1117)]()

</div>

---

## WHAT THIS FOLDER IS

This folder is Step 7 — built last deliberately. The workflows here are the machine-executable implementations of every specification in `classification/`, `routing/`, `states/`, `handoff/`, and `abnormal/`. Code built without its specification beneath it is a liability. Code built after its specification is a deployment.

`[D]` NASA Mission Control established the CAPCOM principle: all communication between ground and spacecraft flows through a single designated controller — the Capsule Communicator. Not because other controllers lack knowledge, but because one channel eliminates ambiguity about who is speaking and who has authority at any moment. The AGI repo is the CAPCOM of the AION brain. All external communication routes through it. The workflows here enforce that architecture programmatically.

`[R]` These five workflows are the reusable components that `thalamus-master.yml` calls. The master workflow is the conductor. These are the section leaders — each governing a specific domain of orchestration, callable independently or as part of the full routing sequence.

---

## WHAT LIVES HERE

| File | Purpose | Build Status |
|------|---------|-------------|
| `WORKFLOW-SPEC.md` | Human-readable spec for all workflows — what each does, when it fires, what it returns | Step 7 |
| `scan-trigger.yml` | Dispatches AION Structure Scanner to all brain repos simultaneously | Step 7 |
| `manifest-sync.yml` | Triggers AGI to rebuild the master manifest after scan completes | Step 7 |
| `amygdala-check.yml` | Routes any output to AMYGDALA for pre-clearance check | Step 7 |
| `memory-write.yml` | Routes FCL candidates and routing events to HIPPOCAMPUS for logging | Step 7 |
| `abnormal-mode.yml` | Activates adversarial routing — slower, fully logged, double-confirmed | Step 7 |
| `README.md` | This file | Complete |

---

## HOW THESE WORKFLOWS RELATE TO THE MASTER

```
thalamus-master.yml  (lives in .github/workflows/)
        ↓
    calls these reusable workflows:
        │
        ├── scan-trigger.yml      ← job: scan-all-repos
        ├── manifest-sync.yml     ← job: manifest-sync
        ├── amygdala-check.yml    ← job: amygdala-clearance
        ├── memory-write.yml      ← job: memory-write
        └── abnormal-mode.yml     ← job: abnormal routing path
```

Each workflow is independently callable via `workflow_call` — meaning other repos in the brain network can invoke them directly without going through the full master routing sequence. Reusable, composable, independently testable.

---

## WORKFLOW ARCHITECTURE PRINCIPLES

**One external channel — AGI is CAPCOM.**
All workflows that produce consolidated output dispatch to AGI, never directly to external systems. AGI synthesizes and surfaces. THALAMUS routes internally.

**ACK before close.**
Every workflow that dispatches to another repo waits for acknowledgment before marking the job complete. No fire-and-forget.

**Full log on adversarial mode.**
`abnormal-mode.yml` produces step-level logging at every decision point — not just job success/failure. When routing is under pressure, the record must be complete.

**Idempotent design.**
Every workflow can be re-run safely. A workflow that cannot be re-run without side effects is a fragile workflow. All dispatch events carry session timestamps that prevent duplicate processing.

---

## REQUIRED SECRETS

| Secret | Scope | Purpose |
|--------|-------|---------|
| `THALAMUS_DISPATCH_TOKEN` | `repo` + `workflow` | Dispatch events to all nine brain repos |

One token. Lives in THALAMUS secrets. Never hardcoded. Rotates on schedule per architect's calendar protocol.

---

## BUILD CONSTRAINT

`[R]` These files are built last. The specifications in `classification/`, `routing/`, `states/`, `handoff/`, and `abnormal/` must be complete and reviewed before any workflow code is written. Code that implements an incomplete specification is not a partial implementation — it is an incorrect implementation that will require full rewrite when the specification solidifies.

The master workflow `thalamus-master.yml` is already built and lives in `.github/workflows/`. These are the reusable components it calls.

---

*orchestration/ — THALAMUS · AION Brain Architecture*
*Part of: Sheldon K. Salmon & ALBEDO · March 2026*

