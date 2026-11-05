The files are anonymized examples modeled after the real structure of the workflow.  
1. DailyLogs.xlsx 
- Provides daily environmental conditions inside the lab and daily instrument baseline signals.  
- This file is not part of the rule-based logic. Instead it will be used during the AI prompting to help the model understand whether the abnormality is due to lab temperatures, humidity or environmental instability.  

2. Raw Data (folder) 
- Contains chromatogram output files used for the data ingestion. All examples are normal except for the latest date which contains an abnormality. 
- These files are the primary input for the rule based logic in which Out of Control conditions can be interpreted. The workflow will then output what needs to go through an additional workflow that requires AI Review.  

3. Instrument Component List.xlsx 
- Defines instrument GC Column  
- Required for proper mapping of results 

4. Lab Standard.xlsx 
- Contains the NIST traceable cylinder standards used for the study. The file provides the baseline for Concentration which is used in reference  

Overall Summary 
Raw Data+Instrument Component List+Lab StandardsàPowerBI with metrics and Rule Based Logic 
DailyLog.xlsxàUsed only during AI prompting to provide environmental context.  
Together these files represent the complete set of inputs required for the AI-assisted system.  
