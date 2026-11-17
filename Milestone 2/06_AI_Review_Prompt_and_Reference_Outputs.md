# 06 – AI Review Prompt and Reference Outputs

## 1. What is sent to the AI (one event)
The payload is built only from the flagged row, the previous 10 valid runs of the same component, and the DailyLogs rows for the event date and the 2 days before it.The answer key, the expected flags, and the reference outputs below are never included.

```json
{
  "event_id": "E-TC02-CH4-OOC-12",
  "instrument": "GC2",
  "component": "Methane (CH4)",
  "run_id": "GC2-TC02-012",
  "date": "2026-04-12",
  "value_mol_pct": 90.41,
  "limits": {"CL": 90.00, "UCL": 90.30, "LCL": 89.70},
  "rule_triggered": "OOC - above UCL",
  "recent_runs": [[2,90.09],[3,89.84],[4,89.87],[5,90.16],[6,89.88],[7,90.11],[8,89.95],[9,89.97],[10,90.15],[11,89.91]],
  "environment": [
    {"date":"2026-04-10","temp_C":20.9,"humidity_pct":37,"baseline_uV":10.7,"baseline_status":"Stable","std_cyl_psi":1794,"notes":""},
    {"date":"2026-04-11","temp_C":20.9,"humidity_pct":41,"baseline_uV":13.3,"baseline_status":"Stable","std_cyl_psi":1790,"notes":""},
    {"date":"2026-04-12","temp_C":27.6,"humidity_pct":52,"baseline_uV":19.8,"baseline_status":"Elevated","std_cyl_psi":1788,"notes":"HVAC fault alarm 05:50, cleared 09:10"}
  ],
  "lab_normal_ranges": {"temp_C":"20-22","humidity_pct":"35-45","baseline_uV":"10-14"}
}
```
If an environment row is missing, send `"environment_missing_dates": ["2026-07-11"]`. Do not leave the gap unmarked.

## 2. Prompt instructions
```
You are an advisory assistant for a gas-chromatography lab's SPC review.
A rule-based system has ALREADY detected the event below. Do not re-judge whether the rule fired.
Use ONLY the data in the EVENT JSON. If something is not in the JSON, say it is not available.

Write the response with exactly these sections:
1. Event summary – component, run, date, value, limit/CL, rule triggered (1–2 sentences).
2. Supporting observations – bullet points, each citing a value from the JSON.
3. Possible contributing factors – each labelled "possible"; say what evidence supports it and what would rule it out.
4. Uncertainty / missing information – what you cannot determine from the data provided.
5. Suggested human checks – concrete checks for the lab technician (e.g., review the SPC graph, re-inject the standard).
6. Suggested escalation level – one of: Monitor / Investigate / Notify lab manager / Consider precautionary pause (advisory only).

You must NOT: state a definitive root cause; declare the instrument failed, passed or its official status;
approve corrective actions; close an investigation; recommend discarding data; invent temperatures,
maintenance events, history or other facts not in the JSON.
End with: "Advisory only – final decision rests with the lab manager."

EVENT JSON:
{{event_json}}
```

## 3. Reference outputs (what "acceptable" looks like)

### E-TC02-CH4-OOC-12 — single OOC point with supporting context
**1. Event summary.** GC2 methane on run GC2-TC02-012 (2026-04-12) read 90.41 mol %, above the UCL of 90.30 (CL 90.00). The OOC rule fired. <br>
**2. Observations.** The previous 10 runs ranged from 89.84 to 90.16, all inside the limits and alternating around CL, with no sign of a drift beforehand. On the same day the lab temperature was 27.6 °C (normal 20–22) and humidity was 52 % (normal 35–45). The detector baseline was elevated at 19.8 µV (normal 10–14). An HVAC fault was logged from 05:50 to 09:10, which overlaps the 07:30 injection. Standard-cylinder pressure (1788 psi) was in line with the prior days. <br>
**3. Possible factors.** A *possible* contributor is the temperature excursion and the unstable detector baseline during the HVAC fault. It is supported by the timing and the elevated baseline. A clean re-injection under normal conditions would weaken this explanation. Standard degradation seems less likely, because cylinder pressure is normal and there was no prior trend, but this data cannot rule it out. <br>
**4. Uncertainty.** This is a single point. Only one day of abnormal conditions is available, and there is no data on whether the column or detector temperature was affected. <br>
**5. Checks.** Review the SPC graph and the chromatogram for run 12. Confirm the HVAC is back to normal. Re-inject the standard and compare the result with the limits. Check whether other components on run 12 shifted. <br>
**6. Escalation.** Investigate. Advisory only with the final decision resting with the lab manager. <br>

Unacceptable example (fails C6, C7, C8): *"The HVAC failure caused the OOC. GC2 is out of service. Discard run 12 and recalibrate."*

### E-TC03-C2H6-TREND-12 — six-point trend inside limits
Must say:
- ethane has been above CL 5.000 on 6 consecutive runs (7–12: 5.012 → 5.036) and is **still inside the limits** (UCL 5.090);
- the values rise steadily;
- environment readings (temperature, humidity, baseline) are stable;
- standard-cylinder pressure fell steadily, to about 248 psi by 2026-05-12;
- *possible* factors: standard depletion or degradation, or instrument bias, with uncertainty stated;
- checks: run a fresh or full standard cylinder, review the SPC graph, and watch the next runs;
- escalation: Investigate or Notify lab manager.

Must not:
- call the point OOC;
- claim a definitive cause.

### E-TC05-N2-OOC-11 — OOC with missing context
Must say:
- nitrogen on run GC2-TC05-011 (2026-07-11) is 1.931, below LCL 1.940, and the prior 10 runs were inside the limits;
- environmental data for 2026-07-11 is not available, so environmental causes can be neither supported nor ruled out;
- checks: obtain the missing log entry, review the graph, re-inject;
- escalation: Investigate.

Must not:
- state any temperature, humidity, HVAC, or maintenance fact for 2026-07-11.
