# Milestone 3 – Group 8: AI-Assisted SPC Review for GC2

**Project:** Rule-based SPC detection in Power BI flags out-of-control (OOC) points and six-point trends on GC2 lab-standard runs. Only flagged events go to a generative-AI step, which writes an *advisory* explanation. A technician reviews it, and the lab manager makes every decision.

All data is **synthetic**. No real lab, company, or customer data is used.

## Files and how they will be used

| File | Assignment part | How it is used in Milestone 3 |
|---|---|---|
| `01_Representative_Input_Data.md` | 1 – Input data | Describes every data file, its columns, the control limits, and what is planted in each test scenario |
| `data/Raw Data/` (90 runs, 6 scenario folders) | 1 – Input data | Primary input loaded into Power BI (Get Data → Folder) |
| `data/Instrument Component List.xlsx`, `data/Lab Standard.xlsx` | 1 – Input data | Peak mapping and fixed control limits for the detection rules |
| `data/DailyLogs.xlsx` | 1 – Input data | Environmental context added to the AI payload for flagged events only |
| `data/csv/` | 1 & 2 | CSV copies of the data and answer key, viewable directly on GitHub |
| `02_Expected_Results.md`, `data/Expected_Results_Reference.xlsx` | 2 – Expected results | Answer key: expected flags for all 450 readings and the 3 AI events. Power BI output is checked against it. **Never sent to the AI.** |
| `05_Detection_Rules.md` | 2 – Expected results | Exact rule definitions behind the answer key |
| `06_AI_Review_Prompt_and_Reference_Outputs.md` | 2 – Expected results | AI input payload, prompt, and reference "acceptable / unacceptable" AI responses |
| `03_Testing_Plan.md` | 3 – Testing plan | 6 test cases (straightforward, challenging, missing info / expected failure), how incorrect results are recognized, AI scoring checklist |
| `data/AI_Review_Log.xlsx` | 3 – Testing plan | Review log, test-results (auto PASS/FAIL), and AI scoring sheets |
| `04_Finalized_Prototype_Scope.md` | 4 – Scope | What Milestone 3 will build |
| `01_AI_Capacity_Governance.md`, `01_KPIs.md` | Supporting | What the AI may and may not do; KPIs |
| `07_Toolchain_Access_Check.md` | Supporting | Confirms which Microsoft/Gemini tools the team can actually use |
| `scripts/` | Optional | Regenerates the synthetic data and verifies the answer key (no code was required for this milestone) |

## Changes to project scope (after feedback)
- Narrowed to **one GC2 instrument, one SPC workflow, one conditional AI-review path**.
- AI review is **user-triggered** from a "Review selected event" button in Power BI. It is not unattended monitoring.
- **Invalid data is now a separate data-quality flag** and never goes to AI. Only OOC or trend events do.
- AI output is **advisory only** (summary, observations, uncertainty, suggested human checks) and is saved to a review log for human sign-off.
- Fallback if AI Builder isn't licensed: Power BI → manual export to Gemini → same Excel review log.

See `00_Revision_Notes.md` for a point-by-point response to the Milestone 2 feedback.

