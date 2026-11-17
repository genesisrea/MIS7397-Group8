# Milestone 2 – Revision Notes (response to 47/100 feedback)

| Rubric item (score) | Feedback | What changed |
|---|---|---|
| Representative Input Data (10/40) | Data files were described but not included | Added the actual synthetic files in `data/`: 90 Raw Data runs, DailyLogs, Instrument Component List, Lab Standard. Rewrote `01_Representative_Input_Data.md` with schemas, limits, and planted scenarios. |
| Expected Results (15/30) | No reference results tied to records; no example AI outputs | Added the answer key `data/Expected_Results_Reference.xlsx` (450 rows, 3 events). Rewrote `02_Expected_Results.md`. Added `05_Detection_Rules.md` (precise rules) and `06_AI_Review_Prompt_and_Reference_Outputs.md` (payload, prompt, reference and unacceptable outputs). |
| Testing Plan (12/20) | No six-point test; invalid data → AI conflicted with the trigger; FP/FN mislabel; no AI-accuracy checks | `03_Testing_Plan.md` now has 6 cases: normal, OOC, six-point, invalid, missing context, boundaries. Invalid data now gives a DQ flag and **no** AI call. An undetected error is now a **false negative**. Added a 10-item AI scoring checklist, 3 repeats per event, and an alert-only vs alert+AI comparison. |
| Scope (10/10) | — | Small consistency edits: user-triggered button, advisory output, review log, Gemini fallback. |
| Implementation guidance | Confirm tool access first | Added `07_Toolchain_Access_Check.md` for the team to complete. |
| AI KPI Metric | Added Time-to-Review KPI and explicit AI value hypothesis | Updated 01_KPIs.md by clarify why AI exist, seperates AI value from automation value and allows project to support or reject AI using hypothesis |
Rebuild and verify the data: `cd scripts && python generate_synthetic_data.py ../data && python build_answer_key_and_logs.py ../data && python verify_expected_results.py ../data`
