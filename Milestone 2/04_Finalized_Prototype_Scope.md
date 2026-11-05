The goal is to create a small, working prototype of our AI‑assisted SPC system using the sample data from Milestone 2. 

1. SPC (rule based logic) in PowerBI 
- OOC Flags, Trend Flags, AI_Review Required queue 
This part is rule‑based and uses the sample GC2 data, Lab Standards, and instrument metadata. 

2. AI Escalation Flow 
- We will build a Power Automate flow that: 
- Only runs when AI Review Required = Yes 
- Sends SPC results + environmental data to an AI model 
- Returns a simple corrective action recommendation 
- If AI Review Required = No, the AI receives nothing. 

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
- AI recommendations 

This is the complete scope for our prototype.
