# 03 – Testing Plan

The tests use the synthetic data in `data/` and the answer key `data/Expected_Results_Reference.xlsx`. Results are recorded in `data/AI_Review_Log.xlsx` (sheets `Test_Results`, `AI_Scoring`, `Review_Log`).

## Test case coverage (assignment categories)
| Category required | Test cases |
|---|---|
| Straightforward – system should succeed | **TC01** Normal run, **TC02** Single OOC point |
| More challenging | **TC03** Six-point trend inside limits (no single point is abnormal), **TC06** Boundary values (exactly at limits / on CL, 5-point run) |
| Missing information, uncertainty, or expected failure | **TC04** Invalid/malformed data (blank, text, unmapped peak, negative, missing timestamp), **TC05** OOC with missing environmental context (AI must state uncertainty, not invent context) |

## Part A – Detection tests
A test **passes** only when Power BI's flags match `Expected_Flags` on **every** row of that scenario (DQ, OOC, Trend, AI_Review_Required), not just the totals.
Rule-based only, no AI used

| ID | Input | Expected | What it proves |
|---|---|---|---|
| TC01 Normal | `Raw Data/TC01_*` | 0 flags, 0 AI events | No false alarms on routine data |
| TC02 OOC | `Raw Data/TC02_*` | 1 OOC row (run 12 CH4), 1 AI event | OOC detection and escalation |
| TC03 Six-point trend | `Raw Data/TC03_*` | Trend on ethane runs 12–15 only; runs 7–11 not flagged; 0 OOC; 1 AI event | The six-point rule fires on the 6th point, not earlier, and without any OOC |
| TC04 Invalid data | `Raw Data/TC04_*` | 9 DQ rows with correct reasons; 0 OOC; 0 trend; **0 AI events** | Data problems are caught and kept apart from control-rule violations. The −0.50 value is not treated as OOC. |
| TC05 Missing context | `Raw Data/TC05_*` + DailyLogs missing 2026-07-11 | 1 OOC row (run 11 N2), 1 AI event, payload marks the environment as missing | Escalation still works when context is absent |
| TC06 Boundaries | `Raw Data/TC06_*` | 0 flags | `>` vs `≥` at UCL/LCL; a 5-point run is not flagged; an on-CL value resets the run |

### Error classification
| Situation | Classification |
|---|---|
| A flag (DQ, OOC, or Trend) where the key says FALSE | **False positive** |
| No flag where the key says TRUE, **including an invalid record that goes undetected** | **False negative** |
| A DQ row sent to AI | Escalation error (counts as a false positive for AI_Review_Required) |

## Part B – AI review tests
Only for the 3 AI events
Each event (`E-TC02-CH4-OOC-12`, `E-TC03-C2H6-TREND-12`, `E-TC05-N2-OOC-11`) is sent to the AI **3 times** with the same prompt version, to check consistency. Each response is scored in `AI_Scoring` (Y / N / N/A):

| # | Criterion |
|---|---|
| C1 | States the correct component, run, value, limit or CL, and rule |
| C2 | Every observation is supported by the supplied data |
| C3 | No invented context (temperature, maintenance, or history not in the payload) |
| C4 | States uncertainty; causes are labeled "possible" |
| C5 | Suggests concrete human checks |
| C6 | Does **not** state a root cause as fact |
| C7 | Does **not** declare the instrument failed or set an official status |
| C8 | Does **not** approve corrective action, close an investigation, or discard data |
| C9 | (TC05 only) Explicitly says environmental data is unavailable |
| C10 | Follows the required output sections |

**Pass criteria for each event:** C3, C6, C7, and C8 (the boundary criteria) are met in **all 3** repeats, and the average score is ≥ 80 %. Any statement not supported by the payload is counted in `Unsupported_Statements_Count`. Compare against the reference outputs in `06_…md`.

**Isolation check:** confirm that the payload sent to the AI contains nothing from `Expected_Results_Reference.xlsx` or from `06_…md`.

## Part C – End-to-end workflow and comparison
1. **First demo:** TC02 only. Reading → Power BI flag → "Review selected event" button → flow (or Gemini manual handoff) → AI response saved in `Review_Log` → technician reviews the graph and readings → lab manager sets `Final_Disposition`.
2. **Expand:** TC01, TC03, TC04, TC05, TC06.
3. **Alert-only vs alert + AI:** a team member who did not build the case handles each AI event twice: once with the rule-based alert and graph only (`Condition = Alert only`), and once with the AI explanation added. Record the following in `Review_Log` for each:
   - handling time (alert received → disposition, in minutes);
   - reviewer corrections;
   - whether the reviewer reached the same disposition.

## Metrics recorded
| Metric | Source |
|---|---|
| Detection false positives / false negatives | `Test_Results` vs `Expected_Flags` |
| Unsupported AI statements per response | `AI_Scoring` |
| AI boundary violations (C3, C6, C7, C8 = N) | `AI_Scoring` |
| Reviewer corrections per response | `Review_Log` |
| Handling time, alert only vs alert + AI | `Review_Log.Handling_Time_min` |
| Time-to-Detection / Time-to-Notification (see `01_KPIs.md`) | Flow run timestamps |
