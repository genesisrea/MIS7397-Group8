# 05 – Detection Rules (precise definitions)

The rules run in this order for each peak row. They are deterministic and use no AI. Power BI must reproduce `data/Expected_Results_Reference.xlsx → Expected_Flags` exactly.

### Step 1 – Data-quality check (`DQ_Flag`)
`DQ_Flag = TRUE` if **any** of these is true:
1. `Injection_DateTime` is blank or cannot be read as a date.
2. `Peak_Name` has no match in `Instrument Component List.xlsx` (unmapped peak).
3. `Concentration_mol_pct` is blank.
4. `Concentration_mol_pct` is not numeric (for example `ERR`).
5. `Concentration_mol_pct` < 0 (physically impossible).

Rows with `DQ_Flag = TRUE`:
- are **excluded** from Steps 2–3 (not plotted as valid points, not counted in runs);
- set `Status = "Data correction required"` and go to the technician;
- **never trigger AI review**. A data problem is not a process signal. Sending it to the AI would ask the AI to explain a number that is not a real measurement.

### Step 2 – Out-of-control (`OOC_Flag`)
For valid rows: `OOC_Flag = value > UCL OR value < LCL`.
Strict inequality: a value **exactly equal** to UCL or LCL is in control.

### Step 3 – Six-point trend (`Trend_Flag`)
For each scenario and component, put the valid rows in run order (`Run_Seq`) and compare each value with **CL** from `Lab Standard.xlsx`. CL is a fixed value, not a rolling mean.
- side = +1 if value > CL, −1 if value < CL, 0 if value = CL
- run length = previous run length + 1 when the side equals the previous side and is not 0; = 1 when the side changes; = 0 when side = 0
- `Trend_Flag = run length ≥ 6`. The **6th** consecutive point on the same side is the first flagged point, and every later point that extends the run is also flagged.
- A point **exactly on CL resets** the count. DQ rows are **skipped**: they neither extend nor reset a run.

### Step 4 – Escalation (`AI_Review_Required`)
`AI_Review_Required = OOC_Flag OR Trend_Flag` (valid rows only).

### Step 5 – Event grouping
Consecutive flagged rows with the same component and the same rule form **one event** (`Event_ID = E-<Scenario>-<Component>-<Rule>-<first run>`). The reviewer requests one AI review per event, from the first flagged row.

### Status values
`Normal` · `Data correction required (no AI)` · `AI review - OOC` · `AI review - Trend`

### Expected totals (all 450 rows)
9 DQ rows · 2 OOC rows · 4 trend rows · 6 AI-review rows · **3 AI events**: `E-TC02-CH4-OOC-12`, `E-TC03-C2H6-TREND-12`, `E-TC05-N2-OOC-11`

### Implementation hints (Power BI)
- Steps 1–2 are simple calculated columns, after a merge with the component list and the limits.
- Step 3 is easiest in **Power Query**: sort by scenario, component, and run; add an index; then compute the side and the running count. In DAX, rank the runs and find the last point whose side differed. Either way, check the result against the TC03 and TC06 rows in the answer key before trusting it.
- AI may help write the M or DAX. The team must confirm that the output matches `Expected_Flags` row for row (see `03_Testing_Plan.md`).
