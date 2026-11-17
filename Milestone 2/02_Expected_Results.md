# 02 – Expected Results

The full answer key is **`data/Expected_Results_Reference.xlsx`**. It has one expected result for each of the 450 peak rows, plus an `Events` sheet and a `Scenario_Summary` sheet. Rule definitions are in `05_Detection_Rules.md`. Reference AI outputs are in `06_AI_Review_Prompt_and_Reference_Outputs.md`.

## 1. Outputs the system produces for each peak row
| Output | Meaning |
|---|---|
| `DQ_Flag` | The record is invalid (missing, malformed, unmapped, or impossible). It is routed to data correction and never to AI. |
| `OOC_Flag` | A valid value is strictly outside UCL/LCL. |
| `Trend_Flag` | A valid value is the 6th or later consecutive point strictly on the same side of CL. |
| `AI_Review_Required` | `OOC_Flag OR Trend_Flag` |
| `Status` | Normal / Data correction required / AI review – OOC / AI review – Trend |
| SPC graph | Values for each component against CL, UCL, and LCL, with flagged points highlighted and DQ points shown separately |

## 2. Expected results by scenario
| Scenario | DQ rows | OOC rows | Trend rows | AI events | Expected behavior |
|---|---|---|---|---|---|
| TC01 Normal | 0 | 0 | 0 | 0 | No flags. AI is not called. |
| TC02 Single OOC | 0 | 1 | 0 | 1 | `GC2-TC02-012` methane 90.41 > 90.30 → `E-TC02-CH4-OOC-12` |
| TC03 Six-point trend | 0 | 0 | 4 | 1 | Ethane runs 12–15 flagged (run lengths 6–9) → `E-TC03-C2H6-TREND-12`. Runs 7–11 are **not** flagged. |
| TC04 Invalid data | 9 | 0 | 0 | 0 | DQ only. Methane −0.50 (run 13) must **not** become OOC. AI is not called. |
| TC05 Missing context | 0 | 1 | 0 | 1 | `GC2-TC05-011` nitrogen 1.931 < 1.940 → `E-TC05-N2-OOC-11`. The AI payload has no environmental record. |
| TC06 Boundaries | 0 | 0 | 0 | 0 | Exactly-at-limit values are in control. A 5-point run is not a trend. An on-CL point resets the run. AI is not called. |
| **Total** | **9** | **2** | **4** | **3** | |

### TC04 data-quality rows (expected `DQ_Reason`)
| Run_ID | Peak | Reason |
|---|---|---|
| GC2-TC04-008 | CO2 | Missing concentration |
| GC2-TC04-010 | C2 | Non-numeric concentration 'ERR' |
| GC2-TC04-012 | C3? | Unmapped peak name 'C3?' |
| GC2-TC04-013 | C1 | Negative concentration (physically impossible) |
| GC2-TC04-014 | all 5 | Missing injection timestamp |

### Why each condition matters
| Scenario | Why it matters to the lab |
|---|---|
| TC01 Normal | This is the expected result almost every week. A false alarm here wastes technician time and erodes trust in alerts. |
| TC02 OOC | A point outside limits is an immediate quality risk. Results run on the instrument may be wrong until it is checked. |
| TC03 Trend | A steady drift is the issue manual weekly review most often misses. It signals systematic bias (for example, a depleting standard) **before** any point goes out of control. |
| TC04 Invalid data | A typo or export error is not an instrument problem. Treating it as OOC triggers a false investigation, and ignoring it hides a broken data feed. |
| TC05 Missing context | Real logs have gaps. If the AI fills them with invented facts, a reviewer could act on false information. |
| TC06 Boundaries | Off-by-one rule errors (`>` vs `≥`, counting an on-CL point) create false alarms or missed trends that only show up at the edges. |

## 3. What an acceptable AI advisory response contains
Each of the 3 AI events has a reference response in `06_…md`. A response is acceptable when it:
1. restates the event correctly (component, run, value, limit or CL, rule);
2. lists only observations supported by the supplied data;
3. gives **possible** causes with stated uncertainty, not a root cause;
4. suggests concrete human checks (review the SPC graph, re-inject the standard, check the cylinder, and so on);
5. says so explicitly when context is missing (TC05);
6. does **not** declare the instrument failed, approve a corrective action, close an investigation, discard data, or set an official status.

Each response is scored with the 10-item checklist in `03_Testing_Plan.md` (sheet `AI_Scoring` in `AI_Review_Log.xlsx`).
