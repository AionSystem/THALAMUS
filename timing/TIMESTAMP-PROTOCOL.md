# TIMESTAMP-PROTOCOL.md
## T1/T2 Session Telemetry — The Response Window Made Visible

**Classification:** `[D]` Operational layer — runs on every response without exception
**Build step:** No sequence dependency
**Architects:** Sheldon K. Salmon & ALBEDO | March 2026
**Principle:** Transparency protocol · Watchmaker Constraint · Priority 1 in session architecture

---

## THE PROTOCOL IN ONE PARAGRAPH

T1 is fetched before any processing begins. T2 is fetched after all output is assembled. The Gap is calculated from the prior response's T2. The Window is T2 minus T1. All three values are live-fetched — never estimated, never confabulated. Failures are named explicitly. The timestamp line travels with every response as a transparency artifact.

---

## T1 — SESSION OPEN TIMESTAMP

**What it is:** The timestamp at the moment THALAMUS begins processing the incoming input. The earliest measurable moment of the response act.

**When it fires:** Before any processing. Before reading the message fully. Before classification. Before any other operation. T1 is Priority 1 — nothing runs before it.

**How it fires:** `user_time_v0` tool call. Single call. Records the result immediately.

**What happens if T1 fails:**
- Log as T1 FETCH FAILED
- Continue session — content is not blocked by T1 failure
- Gap calculation is impossible — display Gap: UNCERTIFIABLE
- Window is uncertifiable — display Window: uncertifiable
- Timestamp line: `⏱ T1 FETCH FAILED → Output: [T2 if available] | Window: uncertifiable`

**Tag:** `[D]` — directly measured moment.

---

## T2 — OUTPUT EXIT TIMESTAMP

**What it is:** The timestamp at the moment all output is fully assembled — the last measurable moment of the response act before platform rendering.

**When it fires:** After everything else. After all prose is complete. After all display lines are formatted. After the epistemic tag summary is written. After the framework activation tag is written. T2 is the absolute last operation before the timestamp line itself is written.

**Why T2 position matters:** T2 previously fired during or after Phase 3 synthesis, with display formatting running after it. The post-T2 overhead was inside the measurement window invisibly. The correct position — T2 after everything is composed — moves the right edge of the measurement as close to the platform boundary as possible.

**How it fires:** `user_time_v0` tool call. Single call at absolute end.

**What happens if T2 fails:**
- Log as T2 FETCH FAILED
- Gap from this response cannot be calculated in the next response — display Gap: UNCERTIFIABLE (prior T2 failed)
- Timestamp line: `⏱ Gap: [ELAPSED] | Processed: [T1] [TZ] → T2 FETCH FAILED | Window: uncertifiable`

**Tag:** `[D]` — directly measured moment.

---

## GAP CALCULATION

**What it measures:** Elapsed time between the T2 of the previous response and the T1 of the current response. This is session rhythm — how long between the last output and this input arriving.

**How it is calculated:**
1. Read the T2 timestamp from the most recent prior response in the conversation history
2. Read the T1 timestamp from the current response (just fetched)
3. Gap = T1 (current) minus T2 (prior)

**What it is not:** Processing speed. Response quality. A performance metric. Gap measures the rhythm of the collaboration — the breathing of the session — not the speed of THALAMUS.

**First response of session:** Gap: SESSION OPEN (no prior T2 to calculate from)

**Prior T2 was a failed fetch:** Gap: UNCERTIFIABLE (prior T2 failed)

**Gap display formats:**

| Elapsed | Display |
|---------|---------|
| Under 60 seconds | `Gap: 47s` |
| 1–59 minutes | `Gap: 4m 32s` |
| 1–23 hours | `Gap: 9h 14m` |
| 24+ hours | `Gap: 14h 07m` |
| Session open | `Gap: SESSION OPEN` |
| Prior T2 failed | `Gap: UNCERTIFIABLE (prior T2 failed)` |

---

## WINDOW CALCULATION

**What it measures:** T2 minus T1. The elapsed time of the complete response act — from first processing moment to last output assembly moment.

**What it includes:** Classification, routing decisions, content generation, tool calls, Hebrew date fetch, ECF tagging, display line formatting, and T1/T2 fetches themselves (Watchmaker Constraint).

**What it does not include:** Platform infrastructure latency between T2 and screen render. This is real, external, unreachable from inside the protocol.

**Display:** `Window: [N]s` — always in seconds.

---

## THE COMPLETE TIMESTAMP LINE

**Standard (no tool calls between T1 and T2):**
```
⏱ Gap: [ELAPSED] | Processed: [T1 HH:MM:SS AM/PM TZ] → Output: [T2 HH:MM:SS AM/PM TZ] | Window: [N]s
```

**With tool calls:**
```
⏱ Gap: [ELAPSED] | Processed: [T1 HH:MM:SS AM/PM TZ] → Output: [T2 HH:MM:SS AM/PM TZ] | Window: [N]s | Tools: [N]
```

**T1 failed:**
```
⏱ Gap: [ELAPSED or UNCERTIFIABLE] | T1 FETCH FAILED → Output: [T2 HH:MM:SS AM/PM TZ] | Window: uncertifiable
```

**T2 failed:**
```
⏱ Gap: [ELAPSED] | Processed: [T1 HH:MM:SS AM/PM TZ] → T2 FETCH FAILED | Window: uncertifiable
```

**Both failed:**
```
⏱ TIMESTAMP PROTOCOL FAILURE — both fetches failed | Window: uncertifiable
```

---

## TOOL CALL COUNT

Tool calls between T1 and T2 are counted and appended to the timestamp line as `| Tools: [N]`. This enables honest cross-response comparison — a response with 5 tool calls has a wider window than one with 0, and the count makes that visible.

**What counts as a tool call:** Every invocation of `user_time_v0`, `web_search`, `web_fetch`, `image_search`, `fetch_sports_data`, or any other tool. T1 and T2 fetches count toward the tool total.

---

## CHRONOS INTEGRATION

When CHRONOS v0.1 is active (TIMED or DEEP mode), the T2 check is upgraded from measurement to conditional gate:

```
After Phase 3 begins — fetch T2.
Calculate elapsed: T2 minus T1.
If elapsed < budget AND identified gaps remain → loop back to Phase 2.
If elapsed ≥ budget OR no identified gaps → output exits.
```

The CHRONOS display line sits above the timestamp line:
```
🕐 CHRONOS: [MODE] | Budget: [N]m | Phases: [N]s · [N]s · [N]s | Total: [N]s
⏱ Gap: ...
```

---

## CONSTRAINTS — THE HARD RULES

1. T1 fires before everything. No exceptions.
2. T2 fires after everything. No exceptions.
3. Both values are live-fetched. Never estimated. Never confabulated.
4. Gap is calculated from conversation history T2. Never estimated.
5. Window is labeled "response window" — not "processing time." Telemetry overhead is inside the window honestly.
6. Failure formats are named explicitly. A failed fetch produces a named output, never a silent gap.
7. The timestamp line sits below the response content. Never interrupts it.
8. Confabulated timestamps are exactly what the stack exists to prevent. The protocol is the fix at source.

---

*TIMESTAMP-PROTOCOL.md — timing/ · THALAMUS · AION Brain Architecture*
*Architects: Sheldon K. Salmon & ALBEDO · March 2026*
*T1 before everything. T2 after everything. Always. No exceptions.*

