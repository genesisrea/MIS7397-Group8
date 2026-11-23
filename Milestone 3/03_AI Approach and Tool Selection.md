# AI Approach and Tool Selection

## Detection Approach

Power BI was selected for SPC detection because out-of-control (OOC) and trend violations are deterministic statistical rules. Rule-based detection is auditable, repeatable, and avoids the risk of AI hallucinations during anomaly identification.

## AI Approach

Generative AI was not used to detect anomalies. Instead, AI Builder was used after a rule-based event was detected.

The purpose of AI is to:

- Collect relevant context
- Summarize evidence
- Explain detected events
- Communicate uncertainty
- Suggest human checks

This approach allows AI to assist with interpretation while keeping detection deterministic.

## Tool Selection

### Power BI

Used to perform SPC rule evaluation, identify OOC events, identify trend violations, and display review queues.

### Power Automate

Used to orchestrate the workflow, retrieve historical context, build AI payloads, and save results.

### AI Builder

Used to generate advisory summaries based on the supplied evidence.

### Excel

Used for synthetic datasets, review logging, test tracking, and AI evaluation.

## Human Oversight

The prototype uses a human-in-the-loop design. AI cannot determine instrument status, approve corrective actions, close investigations, or make compliance decisions. Final decisions remain with laboratory personnel and the lab manager.
