This file contains the expected outputs for the sample data. These outputs represent what the rule-based SPC system should produce.  

1. The system should output 
- OOC Flag – rule based logic that compares the data to control points. A flag indicates an abnormal point 
- Trend Flags- rule based logic that indicates a statistical trend in the data that indicates a review. (six data points on one side of the mean) 
- AI Review Required- Rule based logic that is TRUE when an OOC Flag/Trend Flag exist. 
- SPC  Graph- Visual graph of the data and control limit points.  

2. Normal Output:  
- No OOC, no trends, no escalation to AI 
- This is important as this is the expected output every week and FP can waste company time, a FN can be a missed violation.  

3. Expected Output: OOC Run 
- At least one component exceeds limit; escalation to AI prompt 
- This is important as an OOC can be seen as an immediate anomaly that must be investigated for quality 

4. Expected Output: Trend Violation 
- At least one component has a trend violation; escalation to AI prompt 
- This is important because a trend violation indicates a systemic bias that must be corrected.  

The AI only receives prompts where the AI_Review=Yes in which OOC flag or Trend Violation flag are True. If AI_Review=No then the program is only run by Rule-Based Logic and AI is not prompted.  
