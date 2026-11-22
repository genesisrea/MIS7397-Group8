## Validation Approach & Data Isolation

To ensure an unbiased evaluation in Milestone 3, we tested the prototype directly on the synthetic dataset and scenarios built in Milestone 2 while keeping all ground-truth answer keys strictly isolated from the system.

### 1. Replicating Lab Conditions with Milestone 2 Data
We loaded the full set of simulated chromatogram exports (`data/Raw Data/`), calibration reference tables (`Instrument Component List.xlsx`, `Lab Standard.xlsx`), and daily room logs (`DailyLogs.xlsx`) into the prototype to cover three operational conditions:
* **Routine & Boundary Runs:** Healthy baseline runs alongside edge cases (such as readings landing directly on control limits) to confirm zero false alarms during normal operations.
* **Process Anomalies:** Sudden out-of-control spikes and gradual multi-day drifts inside control limits that signal early equipment or calibration-gas issues.
* **Data Messiness & Missing Context:** Malformed test records (missing timestamps, text errors, negative readings) and intentional gaps in `DailyLogs.xlsx` to verify error isolation and uncertainty handling.

### 2. Strict Isolation of Answer Keys
We enforced **zero data leakage** between our ground-truth validation files and the active prototype:
* **Operational Inputs Only:** During execution, Power BI only saw raw instrument runs and fixed limits, and Power Automate bundled only the flagged run, prior baseline history, and surrounding daily logs. Validation files—such as `Expected_Results_Reference.xlsx` and sample reference responses—were never exposed to the AI.
* **Post-Test Evaluation:** Only *after* the prototype finished running did we use those isolated answer keys to grade results in `AI_Review_Log.xlsx`—verifying row-for-row detection accuracy and scoring AI responses to ensure they stayed grounded in the data, flagged missing logs rather than guessing, and respected advisory-only boundaries.
