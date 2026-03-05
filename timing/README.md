<div align="center">

# timing/

### *The Temporal Layer — Three Clocks, One Session, Full Telemetry*

[![Function](https://img.shields.io/badge/FUNCTION-Session_Telemetry_%26_Time_Architecture-1a6b9a?style=for-the-badge&labelColor=0d1117)]()
[![Build Step](https://img.shields.io/badge/BUILD_STEP-No_Dependency-4a4a4a?style=for-the-badge&labelColor=0d1117)]()
[![Principle](https://img.shields.io/badge/PRINCIPLE-Confabulated_Timestamps_Are_Prohibited-e94560?style=for-the-badge&labelColor=0d1117)]()
[![Clocks](https://img.shields.io/badge/CLOCKS-Gregorian_%7C_13_Moon_%7C_Hebrew-9b59b6?style=for-the-badge&labelColor=0d1117)]()
[![Validation](https://img.shields.io/badge/VALIDATED-Live_Fetch_Only_%5BD%5D-2d6a4f?style=flat-square&labelColor=0d1117)]()

</div>

---

## WHAT THIS FOLDER IS

Every response THALAMUS produces carries a temporal signature. Not as decoration — as a transparency artifact. The timestamp travels with the output, making the instrument visible. The session window, the gap between sessions, the processing overhead — all of it named, measured, and displayed. Nothing estimated. Nothing confabulated.

`[D]` Three independent timekeeping systems are used simultaneously. Not redundancy for its own sake — three clocks with different computational bases provide independent verification of the same moment. Gregorian is the universal reference anchor. The 13 Moon Dreamspell is a continuous-count lunisolar system that doesn't reset with calendar politics. The Hebrew lunisolar calendar is an independent ancient continuous-count system. Agreement across three independent systems is a stronger temporal claim than any single system alone.

`[R]` The measurement is inside the thing being measured. T1 and T2 fetches are part of the response window they measure. This is named, held, and not hidden. The window is honest about the total response act including telemetry overhead. This constraint has a name — the Watchmaker Constraint — because acknowledging it is more rigorous than pretending it doesn't exist.

---

## WHAT LIVES HERE

| File | Purpose | Build Status |
|------|---------|-------------|
| `CLOCK-SPEC.md` | Full specification of all three timekeeping systems and their roles | Ready to fill |
| `TIMESTAMP-PROTOCOL.md` | T1/T2 specification — fetch sequence, Gap calculation, Window calculation, failure formats | Ready to fill |
| `GAP-RESPONSE.md` | Session rhythm documentation — Gap tiers, register responses, Watchmaker constraint | Ready to fill |
| `README.md` | This file | Complete |

---

## THE THREE-LINE TEMPORAL SIGNATURE

Every THALAMUS-governed response carries these three lines at the bottom:

```
⏱ Gap: [ELAPSED SINCE LAST T2] | Processed: [T1] [TZ] → Output: [T2] [TZ] | Window: [N]s
🌑 13 Moon (Dreamspell): Day [N], [MOON NAME] Moon [N/13] | Hebrew: [DAY] [MONTH] [YEAR] [D]
📅 Gregorian: [MONTH] [DAY], [YEAR]
```

**Line 1 — Time telemetry.** What the session is doing and how fast.
**Line 2 — Continuous-count systems.** Two ancient independent timekeeping systems for cross-verification.
**Line 3 — Gregorian anchor.** The universal reference point, explicit and clean.

All values are live-fetched. Hebrew date is fetched from `hebcal.com` on every response and tagged `[D]` — directly measured, never derived. A failed fetch produces a named failure output, never a silent gap or estimate.

---

## THE GAP METRIC

The Gap is not a processing speed measurement. It is a session rhythm measurement — how long between the last output and this input arriving. A 30-second gap is a build sprint. A 9-hour gap is a resumed session with new context. A 3-day gap tells a different story than a 3-minute gap, and the architecture responds accordingly.

Gap tiers and their register responses are documented in `GAP-RESPONSE.md`. The response scales from no acknowledgment (under 5 minutes — in flow) to grounded re-engagement (over a year — "I'm still here. Let's not waste it.").

---

## THE CHRONOS INTEGRATION

`timing/` is also the home of CHRONOS v0.1 — the Time-Budgeted Research Protocol. CHRONOS upgrades the T2 check from a measurement into a conditional gate for research tasks: output only exits when the budget is earned. The CHRONOS specification lives in `CHRONOS-v0.1-SPEC.md` in the root of THALAMUS.

---

## DESIGN PRINCIPLES EMBEDDED HERE

- **Live fetch only** — no estimated, derived, or carried-forward timestamps. Failure is named, never silent.
- **Three-clock verification** — Gregorian + 13 Moon + Hebrew. Three independent systems cross-verifying the same moment.
- **Watchmaker Constraint acknowledged** — the measurement includes itself. This is documented, not hidden.
- **Gap as session intelligence** — elapsed time between sessions is a signal about how the collaboration is moving, not just how fast processing occurs.

---

*timing/ — THALAMUS · AION Brain Architecture*
*Part of: Sheldon K. Salmon & ALBEDO · March 2026*

