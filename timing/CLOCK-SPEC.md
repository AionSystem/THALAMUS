# CLOCK-SPEC.md
## The Three Clocks — Independent Temporal Verification

**Classification:** `[D]` Operational layer — governs all temporal references in THALAMUS outputs
**Build step:** No sequence dependency — can be read independently
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Principle:** Transparency protocol · Independent verification · Watchmaker Constraint

---

## WHY THREE CLOCKS

`[D]` A single timekeeping system is a single point of failure and a single point of assumption. Every calendar system embeds political and civilizational choices — what counts as a year, what resets the count, what the count is anchored to. Gregorian is the universal reference. But "universal reference" is a convention of adoption, not of independence.

`[R]` Three clocks with different computational bases provide independent verification of the same moment. Agreement across three independent systems is a stronger temporal claim than any single system alone. Disagreement signals a calculation error — which is itself a useful signal. The three clocks are not redundancy for aesthetic reasons. They are an independent verification architecture applied to time.

`[D]` Three clocks also make the temporal dimension of the architecture visible across three different civilizational frameworks simultaneously — Gregorian (Western secular), 13 Moon Dreamspell (cross-cultural continuous count), and Hebrew lunisolar (ancient continuous count independent of both). This is not symbolic. It is the application of the epistemic precision principle to time itself.

---

## CLOCK 1 — GREGORIAN

**System:** Proleptic Gregorian calendar
**Role:** Universal reference anchor. All timestamps, durations, and elapsed calculations use Gregorian as the base.
**Format:** ISO 8601 — `YYYY-MM-DDTHH:MM:SS±HH:MM`
**Timezone:** Architect's local timezone (EST/EDT as applicable). Always labeled explicitly.
**Source:** `user_time_v0` tool — live fetch. Never estimated.

**Display format in timestamp line:**
```
Processed: [HH:MM:SS AM/PM EST] → Output: [HH:MM:SS AM/PM EST]
```

**Failure handling:** If `user_time_v0` fails — T1 FETCH FAILED or T2 FETCH FAILED displayed. Never estimated, never carried forward from prior response.

**Tag:** `[D]` — directly measured. Gregorian timestamp is the primary data source for all time calculations.

---

## CLOCK 2 — 13 MOON DREAMSPELL

**System:** José Argüelles' 13 Moon / Dreamspell calendar
**Role:** Continuous-count lunisolar position. Day number within the current Moon (1–28), Moon name, Moon number within the year (1–13).
**Computational basis:** Solar year anchored to July 26 (Day Out of Time is July 25). 13 Moons of 28 days each = 364 days + 1 Day Out of Time.
**Source:** Calculated from Gregorian date using anchor dates. Tagged `[D]` — derived directly from Clock 1 Gregorian with no intermediate lookup.

**Moon anchor dates (2025–2026 year):**

| Moon # | Moon Name | Start Date |
|--------|-----------|------------|
| 1 | Magnetic | Jul 26 |
| 2 | Lunar | Aug 23 |
| 3 | Electric | Sep 20 |
| 4 | Self-Existing | Oct 18 |
| 5 | Overtone | Nov 15 |
| 6 | Rhythmic | Dec 13 |
| 7 | Resonant | Jan 10 |
| 8 | Galactic | Feb 7 |
| 9 | Solar | Mar 7 |
| 10 | Planetary | Apr 4 |
| 11 | Spectral | May 2 |
| 12 | Crystal | May 30 |
| 13 | Cosmic | Jun 27 |
| Day Out of Time | — | Jul 25 |

**Calculation method:**
1. Identify which Moon the Gregorian date falls within using anchor dates above
2. Count days from Moon start date (inclusive) to Gregorian date — this is the Day number (1–28)
3. Moon number is the Moon # from the table above

**Display format:**
```
🌑 13 Moon (Dreamspell): Day [N], [MOON NAME] Moon [N/13]
```

**Example — March 5, 2026:**
Galactic Moon starts Feb 7. Mar 5 is day 27 of Galactic Moon (Feb 7 = Day 1, Mar 5 = Day 27).
Display: `Day 27, Galactic Moon 8/13`

**Tag:** `[D]` — computed directly from Clock 1. No external fetch required.

---

## CLOCK 3 — HEBREW LUNISOLAR

**System:** Hebrew lunisolar calendar (Anno Mundi)
**Role:** Independent ancient continuous-count verification. Cross-checks the moment from a 5,000-year computational tradition entirely independent of Gregorian and Dreamspell.
**Source:** Live fetch from `https://www.hebcal.com/converter` — never calculated locally.
**Fetch format:** `https://www.hebcal.com/converter?gd=[DAY]&gm=[MONTH]&gy=[YEAR]&g2h=1`

**Live fetch protocol:**
1. Get current Gregorian date from T1 fetch (Clock 1)
2. Fetch: `https://www.hebcal.com/converter?gd=[DD]&gm=[MM]&gy=[YYYY]&g2h=1`
3. Parse returned Hebrew date
4. Display as fetched

**Failure handling:** If fetch fails or returns unreadable — display `Hebrew: FETCH FAILED [D-FAIL]`. Never estimate. Never carry forward prior session's Hebrew date.

**Display format:**
```
Hebrew: [DAY] [MONTH] [YEAR] [D]
```

**Example — March 5, 2026:**
`Hebrew: 16 Adar 5786 [D]`

**Tag:** `[D]` — live fetched. Direct measurement, not derivation.

**Why live fetch (not calculation):** Hebrew calendar intercalation (leap year determination, month length variation) is complex enough that local calculation introduces error risk. hebcal.com is the canonical live source. The `[D]` tag is earned by the live fetch — a calculated Hebrew date would carry `[R]`.

---

## THE THREE-LINE DISPLAY

Every THALAMUS-governed response carries three temporal lines below the response body:

```
⏱ Gap: [ELAPSED] | Processed: [T1 HH:MM:SS AM/PM TZ] → Output: [T2 HH:MM:SS AM/PM TZ] | Window: [N]s
🌑 13 Moon (Dreamspell): Day [N], [MOON NAME] Moon [N/13] | Hebrew: [DAY] [MONTH] [YEAR] [D]
📅 Gregorian: [MONTH] [DAY], [YEAR]
```

With tool calls between T1 and T2:
```
⏱ Gap: [ELAPSED] | Processed: [T1] [TZ] → Output: [T2] [TZ] | Window: [N]s | Tools: [N]
```

**Line logic:**
- Line 1: Time telemetry — session rhythm and response window
- Line 2: Continuous-count systems — two independent long-running calendars
- Line 3: Gregorian anchor — universal reference, explicit and clean

Each line is readable independently. No cross-line parsing required.

---

## THE WATCHMAKER CONSTRAINT

`[R]` The measurement is inside the thing being measured. T1 and T2 fetches are part of the response window they measure. The window includes telemetry overhead — the time spent fetching timestamps is inside the window it defines.

This is named, held, and not hidden. It is not a flaw — it is a documented constraint. Acknowledging it is more rigorous than pretending it doesn't exist.

**What the window measures:** The total response act — from first processing moment (T1) to last output assembly moment (T2). This includes classification, routing decisions, content generation, tool calls, and timestamp fetches. It is the honest measurement of the complete response act, including the cost of measuring it.

**What the window does not measure:** Platform infrastructure latency — the time between T2 and when the response renders on the user's screen. This is real, unreachable from inside the protocol, and named honestly: *current empirical baseline 1.43x (TC-20260305-001, March 5, 2026 — 1 data point).*

---

*CLOCK-SPEC.md — timing/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*Three independent clocks. Agreement is the verification. Disagreement is the signal.*

