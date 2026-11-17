The goal is to create a small, working prototype of our AI‑assisted SPC system using the sample data from Milestone 2. 

1. SPC (rule based logic) in PowerBI 
- OOC Flags, Trend Flags, AI_Review Required queue 
This part is rule‑based and uses the sample GC2 data, Lab Standards, and instrument metadata. 

2. AI Escalation Flow 
- A **"Review selected event"** button (Power Automate visual) in the Power BI report starts the flow. The reviewer triggers it; it does not run unattended.
- The flow first checks that the event qualifies (AI Review Required = Yes). DQ-flagged rows never qualify.
- It sends the event payload (component, run, value, limits, rule, previous 10 runs, DailyLogs context) to AI Builder "Run a prompt" (see `06_AI_Review_Prompt_and_Reference_Outputs.md`).
- It returns an **advisory** response: event summary, supporting observations, uncertainty, and suggested human checks. It does not return a corrective-action decision.
- It saves the response to the `ReviewLog` table in `data/AI_Review_Log.xlsx` (Excel Online (Business) → "Add a row into a table").
- If AI Review Required = No, the AI receives nothing.
- Fallback if AI Builder is not licensed: export the event from Power BI → Gemini web app → paste the response into the same review log. This manual step is labeled as such in the demo (see `07_Toolchain_Access_Check.md`). 

3. Adjustments From Milestone 1 
We narrowed the project to: 
- One GC2instrument 
- One SPC workflow 
- One AI escalation path 
- A small set of sample files 

This keeps the prototype realistic for the course timeline. 

We also want to identify the rule based logic as the main component of the workflow but use the AI component for when an anomaly is detected.  

4. Summary 
The Milestone 3 prototype will show: 
- Rule‑based SPC detection 
- Conditional AI escalation 
- Advisory AI explanation, saved for human review
- Human review and lab-manager disposition recorded in the review log 

This is the complete scope for our prototype.
