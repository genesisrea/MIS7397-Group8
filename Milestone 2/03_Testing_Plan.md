The test will use the representative input data prepared in Milestone 2 and evaluate whether the rule‑based outputs and AI escalation logic match the expected results. 

1. Test Case indicating a normal run.  
- No OOC, no trends, AI should not be prompted.  
- An incorrect result would be a false positive 

2. Test Case with an OOC  
- At least one OOC, AI_Review=Yes, and AI prompt is given.  
- An incorrect result would be a false negative 

3. Test Case with invalid data 
- At least one case where the data itself is invalid that causes an error in the results. AI_Review_Yes and AI prompt is given.  
- An incorrect result would be a false positive where a mistake goes unnoticed.  

The results must match the rule based logic.  

AI Review Required must match the escalation rules. (At least one OOC or trend) 
