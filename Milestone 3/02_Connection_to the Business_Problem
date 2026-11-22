## How the Prototype Addresses the Business Problem

> **Core Operational Challenge:** In routine gas chromatography (`GC2`) quality control, analysts spend excessive time manually reviewing passing runs or miss subtle, early-stage instrument drift—while introducing generative AI directly into lab operations risks false alarms, hallucinated root causes, and compliance violations.

By intentionally scoping the prototype to **one instrument (`GC2`)**, **one SPC workflow**, and **one conditional AI-review path**, our focused two-step architecture solves this problem across three areas:

### 1. Eliminating Alert Fatigue & Missed Drift
* **Solution:** Deterministic math rules in **Power BI** screen every run against fixed lab-standard limits before any AI is invoked.
* **Impact:** Routine runs pass silently with zero alerts, while genuine **Out-of-Control (OOC)** spikes and subtle **six-point trends** are caught early. Malformed records are isolated via a `DQ_Flag` so broken data feeds never trigger false investigations.

### 2. Accelerating Investigation Lookups
* **Solution:** A user-triggered **Power Automate** flow packages the flagged run, the prior 10 valid runs, and 3 days of environmental telemetry (`DailyLogs.xlsx`) into one structured payload.
* **Impact:** Eliminates manual cross-referencing across spreadsheets and chromatograms during an investigation while enforcing strict data isolation.

### 3. Preventing AI Hallucinations & Protecting Compliance
* **Solution:** System prompts restrict the AI to strictly *advisory* explanations grounded solely in the supplied JSON payload.
* **Impact:** Explicitly flags missing environmental logs (e.g., `TC05`) rather than inventing context, and routes concrete checks to `AI_Review_Log.xlsx`—keeping physical verification, batch holds, and final dispositions firmly under human and lab-manager authority.
