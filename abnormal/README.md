<div align="center">

# abnormal/

### *The Failure Architecture — Routing When Normal Stops Working*

[![Function](https://img.shields.io/badge/FUNCTION-Abnormal_%26_Adversarial_Protocols-e94560?style=for-the-badge&labelColor=0d1117)]()
[![Build Step](https://img.shields.io/badge/BUILD_STEP-5_of_7-FFD700?style=for-the-badge&labelColor=0d1117)]()
[![Principle](https://img.shields.io/badge/PRINCIPLE-Design_Normal_First_Then_Abnormal-9b59b6?style=for-the-badge&labelColor=0d1117)]()
[![Source](https://img.shields.io/badge/SOURCE-NRC_NUREG_%7C_C4_Doctrine_%7C_Chernobyl_%7C_Nerge-4B0082?style=flat-square&labelColor=0d1117)]()

</div>

---

## WHAT THIS FOLDER IS

Every routing system has a normal mode and an abnormal mode. Most systems only specify the normal mode and discover the abnormal mode when reality provides it — usually catastrophically. This folder specifies the abnormal mode deliberately, before it is needed, so that when normal routing fails the system has a defined path rather than an undefined one.

`[D]` Chernobyl: the most devastating nuclear accident in history was not caused by reactor failure. It was caused by routing failure — information did not travel correctly from plant operators to local party officials to Moscow. At every hop, the routing system that worked in normal conditions was wrong for abnormal conditions. No one had specified what abnormal routing looked like. The system found out by failing.

`[D]` The NRC's NUREG-0899 guidelines for Emergency Operating Procedures require that every step be written as an IF/THEN/ELSE/VERIFY decision with explicit alternatives. The abnormal procedure is as formally specified as the normal procedure — because under pressure, informal fallbacks are not fallbacks. They are the absence of fallbacks dressed as improvisation.

`[D]` The Mongol Nerge: every winter, Genghis Khan conducted organized mock combat to stress-test the entire command and routing system under conditions of controlled chaos. Not to practice fighting — to find routing failures before real conditions revealed them. The system that fails in the Nerge gets fixed. The system that fails in battle kills people.

`[R]` Abnormal protocols built after normal protocols are proven are stronger than normal protocols that assume abnormal conditions won't occur.

---

## WHAT LIVES HERE

| File | Purpose | Build Status |
|------|---------|-------------|
| `ABNORMAL-PROTOCOL.md` | Complete routing specification for failure conditions | Step 5 |
| `ADVERSARIAL-MODE.md` | Slower, fully logged routing under external threat signal | Step 5 |
| `FAILURE-CATALOG.md` | Documented failure modes indexed from 36 architectural findings | Step 5 |
| `NERGE-PROTOCOL.md` | Scheduled stress-test specification — find failures before real conditions do | Step 5 |
| `README.md` | This file | Complete |

---

## THREE ABNORMAL CONDITIONS

**Abnormal — internal routing failure:**
Normal routing path is unavailable. Primary destination unreachable, ACK timeout exceeded after retransmit, or state machine cannot determine current state. Protocol: route to AMYGDALA for assessment, log full failure event, notify architect if ALERT or above.

**Adversarial — external threat signal:**
AMYGDALA issues an adversarial flag. Routing continues but every step is logged in full detail. Slower. More documented. More verified. Nothing proceeds without double confirmation. Activated by AMYGDALA, deactivated only by AMYGDALA.

**General — catastrophic:**
GENERAL severity declared. Normal routing suspended entirely. AMYGDALA holds all events. Architect review required before any routing resumes. This is the NRC's General Emergency tier — the highest severity, reserved for conditions where normal operations are no longer possible.

---

## THE NERGE SCHEDULE

The Nerge is not a one-time audit. It is a scheduled discipline. THALAMUS runs a structured stress-test at regular intervals — simulating routing failures, abnormal conditions, and adversarial modes — to confirm the failure protocols work before real conditions require them.

Frequency: defined in `NERGE-PROTOCOL.md`.
Results: logged in AMYGDALA's red-team archive.
Findings: routed to HIPPOCAMPUS as FCL candidates if they reveal architectural gaps.

---

## DESIGN PRINCIPLES EMBEDDED HERE

- **Specify abnormal before it's needed** — NRC NUREG. Informal fallbacks under pressure are not fallbacks.
- **Every path has an alternative** — C4 Doctrine. If the primary path fails, the alternative is defined in advance.
- **Stress-test regularly** — Mongol Nerge. Find failures in the exercise, not in the field.
- **Adversarial mode is a defined state, not a panic response** — MuckRock/CIA principle. When routing is under pressure, slow down, log everything, verify twice.
- **GENERAL severity halts everything** — NRC. The highest tier is a full stop, not a faster routing mode.

---

*abnormal/ — THALAMUS · AION Brain Architecture*
*Part of: Sheldon K. Salmon & ALBEDO · March 2026*

