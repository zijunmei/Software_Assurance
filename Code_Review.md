# Code analysis for Software Security Engineering
## Part 1: Code Review
### 1.1 Code Review Strategy   ----> to be completed by Mustapha 
Here is the description of the Code review Strategy. <br>
Answer Following question(*Required*):
- What challenges did you expect before starting code review?
- How did your code review strategy attempt to address the anticipated challenges?

### 1.2 Automated Scanning  (2 sections to be completed by Mustapha and Zijun)
In this assignment, we used two automatic scanning tools: CodeQL and Fortify.
-  **Fortify Scan to be completed by Mustapha
- **CodeQL**
As a complement to the Fortify scan results, we performed a secondary scan of the code using CodeQL provided by Github. This scan was global in scope and scanned over 2.1M lines of code in total. Finally over 5000 Alerts were found, including 149 Critical Alerts, 311 High Alerts, 17 Medium Alerts, and the rest were warnings and errors.
Here is the link of the [CodeQL Scanning result](https://github.com/zijunmei/elasticsearch/security/code-scanning).

### 1.3 Manual Reviews
By summarizing the Usa/Misuse case, Assurance case, DFDs, we have identified ten CWEs that may be worthy of our attention.

- [CWE-20](https://cwe.mitre.org/data/definitions/20.html): **Improper Input Validation**
   - We chose to manually review this CWE because we noticed a relevant threat in the [Authentication Threat Model](https://htmlpreview.github.io/?https://github.com/zijunmei/Software_Assurance/blob/main/Authentication_Threat_Model_Report.htm) and saw the presence of corresponding Alerts in the CodeQL scan results.
   - 




Document findings from a manual code review of critical security functions identified in misuse cases, assurance cases, and threat models.

## Part 2: Key Findings and Contributions  ---> to be completed by Charlie 2.1 ans 2.2

#### CWE checklist ---> Chardlie to put the whole list here  

### 2.1 Key Findings
Provide a summary of key findings from manual and/or automated scanning. This summary should include mappings to CWEs to describe significant findings and perceive risk in your hypothetical operational environment.
### 2.2 Contributions
Planned or ongoing contributions to the upstream open-source project (documentation, design changes, code changes, communications, etc.) You can discuss planned or in-progress contributions based on any of the prior assignments in the class.

## Team Reflection  ---> to be completed by Mustapha 
Include a reflection of your teamwork for this assignment. What issues occurred? How did you resolve them? What did you plan to change moving forward?
## Links
- [Project Board]()
- [Project Repository]()
