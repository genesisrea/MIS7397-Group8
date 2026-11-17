# 01 – Representative Input Data

All files are in `data/`. Every file is synthetic and labeled `SYNTHETIC` in a `Data_Source` column or a README sheet. The structure copies the real GC2 workflow, but no values come from a real instrument, lab, or customer.

## Files

| File | What it contains | Used by | Rows |
|---|---|---|---|
| `Raw Data/` (folder) | One `.xlsx` chromatogram export per standard injection. Six subfolders, one per test scenario (TC01–TC06), with 15 runs each. | Detection rules (primary input) | 90 files × 5 peaks = 450 peak rows |
| `Instrument Component List.xlsx` | Maps each instrument `Peak_Name` (C1, C2, C3, N2, CO2) to a `ComponentID`, plus GC column, detector, and expected retention time. | Detection rules (mapping and the unmapped-peak check) | 5 |
| `Lab Standard.xlsx` | Certified concentrations of the standard (lot `SYN-STD-2026-01`) and the **fixed control limits** (CL, UCL, LCL) for each component. | Detection rules (limits) | 5 |
| `DailyLogs.xlsx` | Daily lab temperature, humidity, detector baseline, carrier and standard-cylinder pressure, and maintenance notes. | **AI context only.** Never used by the detection rules. | 89 (one date intentionally missing) |
| `Expected_Results_Reference.xlsx` | Answer key: the expected flags for every peak row, plus events and scenario counts. | Testing only. **Never sent to the AI.** | 450 |
| `AI_Review_Log.xlsx` | Human-review log, test-results sheet, and AI scoring sheet (templates to fill in). | Human review and evaluation | — |

## Raw Data columns (`PeakTable`)
`Data_Source`, `Instrument`, `Run_ID`, `Injection_DateTime`,` Sample_Type`, `Standard_Lot`, `Peak_Name`, `Retention_Time_min`, `Peak_Area`, `Concentration_mol_pct`

`Run_ID` format: `GC2-<Scenario>-<Run#>`.

## Control limits (from `Lab Standard.xlsx`)

| Component | CL (certified, mol %) | UCL | LCL | σ |
|---|---|---|---|---|
| Methane (CH4 / C1) | 90.00 | 90.30 | 89.70 | 0.10 |
| Ethane (C2H6 / C2) | 5.000 | 5.090 | 4.910 | 0.030 |
| Propane (C3H8 / C3) | 2.000 | 2.045 | 1.955 | 0.015 |
| Nitrogen (N2) | 2.000 | 2.060 | 1.940 | 0.020 |
| Carbon dioxide (CO2) | 1.000 | 1.030 | 0.970 | 0.010 |

The limits are fixed (CL ± 3σ) so that the expected flags never change.

## Scenarios built into the data

| Folder | Dates | What is planted |
|---|---|---|
| TC01_Normal_run | 2026-03-02 → 03-16 | Nothing. Every value is within limits, and no same-side run is longer than 3. |
| TC02_Single_OOC_point | 2026-04-01 → 04-15 | Run 12 methane = **90.41** (> UCL 90.30). That day's log shows an HVAC fault, 27.6 °C, and an elevated baseline. |
| TC03_Six-point_trend | 2026-05-01 → 05-15 | Ethane runs 7–15 are all above CL and inside the limits, rising from 5.012 to 5.049. Standard-cylinder pressure falls from 623 to 145 psi. |
| TC04_Invalid_malformed_data | 2026-06-01 → 06-15 | Run 8 CO2 blank · run 10 ethane `"ERR"` · run 12 peak `"C3?"` (unmapped) · run 13 methane **−0.50** · run 14 missing timestamps (all 5 peaks) |
| TC05_OOC_with_missing_context | 2026-07-01 → 07-15 | Run 11 nitrogen = **1.931** (< LCL 1.940). The DailyLogs row for **2026-07-11 is missing**. |
| TC06_Boundary_cases | 2026-08-03 → 08-17 | Propane run 5 = 2.045 (**exactly UCL**) · CO2 run 9 = 0.970 (**exactly LCL**) · methane runs 2–6 above CL (**5-point run**) · ethane run 11 = 5.000 (**exactly CL**), between two 3-point runs above CL |


## Data flow
```
Raw Data + Instrument Component List + Lab Standard ─► Power BI (DQ check → OOC → Trend → AI_Review_Required)
DailyLogs ─► added to the AI payload only for flagged events
Expected_Results_Reference ─► used only to check Power BI output and never shown to AI
```
