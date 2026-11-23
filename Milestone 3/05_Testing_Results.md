# Testing Results

The prototype was tested using three representative scenarios developed during Milestone 2. These scenarios were selected to evaluate normal AI-assisted interpretation, trend analysis, and missing-information handling.

## TC02 – Out-of-Control (OOC) Event

### Scenario

A Methane (CH4) concentration of 90.41 exceeded the Upper Control Limit (UCL) of 90.30 during a documented HVAC fault and elevated detector baseline condition.

### Result

PASS

### Observations

- Power BI correctly identified the OOC event.
- Power Automate retrieved historical run information and environmental context.
- AI Builder generated a structured advisory summary.
- The AI identified elevated detector baseline conditions and HVAC-related environmental changes.
- The AI remained advisory and did not declare instrument failure or a root cause.

### Outcome

The AI-assisted workflow successfully explained the detected OOC event and provided appropriate investigative recommendations.

---

## TC03 – Six-Point Trend Event

### Scenario

Ethane (C2H6) readings remained within control limits but exceeded the six-point same-side trend rule. Standard-cylinder pressure decreased over time.

### Result

PASS

### Observations

- Power BI correctly identified the trend event.
- The AI did not incorrectly classify the event as an OOC violation.
- The AI identified the declining cylinder pressure history as a possible contributing factor.
- The AI maintained uncertainty and presented possible causes rather than definitive conclusions.
- Suggested actions focused on investigation and continued monitoring.

### Outcome

The AI successfully distinguished a trend violation from an OOC event and provided useful investigative context.

---

## TC05 – Missing Context Event

### Scenario

A Nitrogen (N2) concentration of 1.931 fell below the Lower Control Limit (LCL) of 1.940. The DailyLogs record for the event date was intentionally missing.

### Result

PASS

### Observations

- Power BI correctly identified the OOC event.
- Missing DailyLogs information was detected.
- The AI explicitly stated that environmental information was unavailable.
- The AI did not invent temperature, humidity, maintenance, or environmental conditions.
- The AI recommended investigation and additional verification.

### Outcome

The AI successfully handled incomplete data while maintaining uncertainty and avoiding unsupported conclusions.

---

# Overall Findings

The prototype successfully demonstrated the intended AI-assisted review workflow.

Across all three scenarios:

- Rule-based logic correctly identified SPC exceptions before AI involvement.
- Historical context retrieval improved the quality of AI explanations.
- AI remained within governance boundaries and did not make operational decisions.
- Missing information was acknowledged instead of being fabricated.
- Human review remained necessary for final disposition and compliance decisions.

The results support the use of AI as an advisory tool for SPC exception review while preserving deterministic detection and human oversight.
