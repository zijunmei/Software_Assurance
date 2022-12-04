# Code analysis for Software Security Engineering
## Part 1: Code Review
### 1.1 Code Review Strategy
Since our System-Of-Interest (Elasticsearch) codebase is very large with a size of around 1GB and having several modules in it, we had to use a code review strategy based on a combination of scenario and weakness-based approach.<br/><br/>
Our project misuse cases, assurance claims, and threat model were all around user authentication and data access. Those scenarios rely on the X-Pack security settings. As discussed in previous assignments, X-Pack module is an Elasticsearch extension that provides the security, alerting, monitoring, reporting, machine learning, and many other capabilities. So, we focused our efforts analyzing and scanning the X-Pack code to discover if the implementation code has any security weaknesses which can be exploited by malicious actors.<br/>

We expected few challenges before starting the code review. The first one, as noted above, was the large size of the codebase to scan. The second challenge was the selection of the automated code-scanning tool. The tool hardware requirements needed to run it and the amount of time to complete a scan.<br/>

Our code strategy helped us to scope just the X-Pack module. Also, instead of scanning locally using limited hardware resources on our machines, we decided to use automated code-scanning services such as Fortify On Demand and Github CodeQL to scan the codebase.

### 1.2 Automated Scanning  (2 sections to be completed by Mustapha and Zijun)
We tried several automated code-scanning tools but we had trouble running them. We first tried both SonarQube and Fortify SCA.
SonarQube scan needed some changes in the Elasticsearch Gradle build tool. Fortify SCA is a licensed static code analyzer software.
Even when we tried to scan just the X-Pack module, Fortify SCA scan was taking a lot of time and hardware resources such CPU and RAM to complete.<br/>

We ended up using two automated code-scanning services: Fortify On Demand and Github CodeQL scanning tools.<br/>

#### Fortify On Demand
Both Fortify SCA and Fortify On Demand are software offerings by the same company MicroFocus. Fortify SCA will run the scan On-premise. However, Fortify On Demand will run it as a service remotely from MicroFocus hardware. Both of them are licensed software. We used a trial version for Fortify On Demand. The Elasticsearch X-Pack codebase was uploaded to Fortify On Demand and run the scan.<br/>

![Fortify On Demand scan](/images/X-Pack-FortifyOnDemandScan.PNG "Fortify On Demand scan")

See the attached image for the details of the codebase scanned (size, loc, run time).

![Fortify On Demand codebase scanned](/images/Scan-Stats.PNG "Fortify On Demand codebase scanned")

The scanning report is located here 
[X-Pack Fortify On Demand Scan Report](https://htmlpreview.github.io/?https://github.com/zijunmei/Software_Assurance/blob/main/Elasticsearch_8.5.2-x-pack-scan-FortifyOnDemand.html)<br/><br/>


#### CodeQL
As a complement to the Fortify scan results, we performed a secondary scan of the code using CodeQL provided by Github. This scan was global in scope and scanned over 2.1M lines of code in total. Finally over 5000 Alerts were found, including 149 Critical Alerts, 311 High Alerts, 17 Medium Alerts, and the rest were warnings and errors.
Here is the link of the [CodeQL Scanning result](https://github.com/zijunmei/elasticsearch/security/code-scanning).

### 1.3 Manual Reviews
By summarizing the Usa/Misuse case, Assurance case, DFDs, we have identified 8 CWEs that may be worthy of our attention.

- [CWE-20](https://cwe.mitre.org/data/definitions/20.html): **Improper Input Validation**
   - We chose to manually review CWE-20 because we noticed a relevant threat in the [Authentication Threat Model](https://htmlpreview.github.io/?https://github.com/zijunmei/Software_Assurance/blob/main/Authentication_Threat_Model_Report.htm) and saw the presence of corresponding [Alerts](https://github.com/zijunmei/elasticsearch/security/code-scanning?query=is%3Aopen+branch%3Amain+Overly) in the CodeQL scan results.
   - Through Manually review the source code, we found that those Alerts mainly casued by overlaping of regular expression. The actual range of characters defined by the programmer probably larger than the range he intended to define. This may causes the program to accept input that would otherwise be invalid.
- [CWE-117](https://cwe.mitre.org/data/definitions/117.html): **Improper Output Neutralization for Logs**
    - We chose to manually review CWE-117 because we noticed a relevant threat in the assurance case(Audit log Part) and saw the presence of corresponding [Alerts](https://github.com/zijunmei/elasticsearch/security/code-scanning/330) in the CodeQL scan results.
    - Through Manually review the source code, we found that the programmer does not sanitize the response from outside input. 
    The line of code *tracer.trace(requestLine + '\n' + responseLine);* depend on a user provide value. This is probably a concern about the untrusted data pasted into JSON and cause Audit Log Manipulation, Web Server Logs Tampering, and Log Injection-Tampering-Forging attack.
- [CWE-290](https://cwe.mitre.org/data/definitions/290.html): **Authentication Bypass by Spoofing**	
    - We chose to manually review CWE-290 because we noticed a relevant threat in the DFDs case and saw the presence of corresponding [Alerts](https://github.com/zijunmei/elasticsearch/security/code-scanning?query=is%3Aopen+branch%3Amain+bypass) in the CodeQL scan results.
    - Through Manually review the source code, we found that the Alerts caused by having conditional statements(if else) which contained an important authentication or login code. And the user can make the decision about whether to execute this code because it is based on user-controlled data. Therefore, these methods are sensitive and it may be possible for an attacker to bypass security systems by preventing this code from executing. In [line 113 of ServerTransportFilter.java](https://github.com/zijunmei/elasticsearch/security/code-scanning/540) class, the statement can be control by user provide data(authentication  == NULL or other), and this may cause authzService.authorize() method never be excuated.
- [CWE-295](https://cwe.mitre.org/data/definitions/295.html): **Improper Certificate Validation**	
    - We chose to manually review CWE-295 because we noticed a relevant threat in the DFDs case and saw the presence of corresponding [Alerts](https://github.com/zijunmei/elasticsearch/security/code-scanning?query=is%3Aopen+branch%3Amain+TrustManager) in the CodeQL scan results.
    - Through Manually review the source code, we found that the Alerts caused by the configuration of TrustManager, In the class [TrustEverythingConfig.java](https://github.com/zijunmei/elasticsearch/blob/ce057fc9475b5d863b47c91b6ea9c56afc0c8aa1/libs/ssl-config/src/main/java/org/elasticsearch/common/ssl/TrustEverythingConfig.java#L37-L60), all methods are implemented as a no-op and do not throw exceptions regardless of the certificate presented. If the checkServerTrusted method in TrustManager never throws a CertificateException, it trusts every certificate. The vulnerable program accepts the certificate and proceeds with the connection since the TrustManager implicitly trusted it by not throwing an exception. The security of TLS will be broken.
- [CWE-326](https://cwe.mitre.org/data/definitions/326.html): **Inadequate Encryption Strength** 
    - We chose to manually review CWE-326 because we noticed a relevant threat in the assurance case (data encryption part of authentication case) and saw the presence of corresponding [Alerts](https://github.com/zijunmei/elasticsearch/security/code-scanning/308) in the CodeQL scan results.
    - Through Manually review the source code, we found that in the line 610 of code in the class [PemUtils.java](https://github.com/zijunmei/elasticsearch/blob/ce057fc9475b5d863b47c91b6ea9c56afc0c8aa1/libs/ssl-config/src/main/java/org/elasticsearch/common/ssl/PemUtils.java#L610-L610), the [nameddCurve](https://www.rfc-editor.org/rfc/rfc5480#section-2.1.1.1) was defined as secp192r1 which is a 192-bit prime field Weierstrass curve. However, the key size should be [at least 256 bits](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html#algorithms) for elliptic-curve cryptography (ECC) for considering it is adequated Encryption Strength. In this case, secp256r1 which is a 256-bit prime field Weierstrass curve will be the better choice for encryption.
- [CWE-328](https://cwe.mitre.org/data/definitions/328.html): **Use of Weak Hash**
    - We chose to manually review CWE-328 because we noticed a relevant threat in the assurance case (data encryption part) and saw the presence of corresponding [Alerts](https://github.com/zijunmei/elasticsearch/security/code-scanning?query=is%3Aopen+branch%3Amain+broken+) in the CodeQL scan results.
    - Through Manually review the source code, we found that the Elasticsearch is still setting MD5 and SHA1 as options of hash algorithms. MD5 algorithm is a type of Hash algorithm called message digest algorithm. By digest, it is literally understood as an approximation of the content. In the MD5 algorithm, this digest refers to the mapping of arbitrary data into a 128-bit long digest message. However, MD5 was essentially "cryptographically broken and unsuitable for further use" because its weaknesses have been exploited in the field. We believe that using broken or weak cryptographic algorithms can leave data vulnerable to being decrypted. Furthermore, in the line [512](https://github.com/zijunmei/elasticsearch/security/code-scanning/310) of code in the class PemUtils.java, programmer used DESede encrption mode as a option which is also be considered as weak encryption mode because it is vulnerable to replay and other attacks.
- [CWE-798](https://cwe.mitre.org/data/definitions/798.html): **Use of Hard-coded Credentials**
    - We chose to manually review CWE-798 because it is critical part of the [Alerts](https://github.com/zijunmei/elasticsearch/security/code-scanning?query=is%3Aopen+branch%3Amain+Hard) in the CodeQL scan results.
    - Through Manually review the source code, we found that the [toString()](https://github.com/zijunmei/elasticsearch/security/code-scanning/198) method in DataUtil.java class directly return a NULL value when the input parameter is NULL. However, the ToString() method on an object should always return a string. Returning null instead contravenes the method’s implicit contract. This also related with [CWE-476](https://cwe.mitre.org/data/definitions/476) - NULL Pointer Dereference. In the [line 52](https://github.com/elastic/elasticsearch/blob/main/x-pack/plugin/core/src/main/java/org/elasticsearch/license/CryptUtils.java) of CryptUtils.java class, the DEFAULT_PASS_PHRASE = "elasticsearch-license" inferred that the default pass phrase key was stored in hard-code. 
- [CWE-918](https://cwe.mitre.org/data/definitions/918.html): **Server-Side Request Forgery (SSRF)**
    - We chose to manually review CWE-918 because it is critical part of the [Alerts](https://github.com/zijunmei/elasticsearch/security/code-scanning?query=is%3Aopen+branch%3Amain+forgery) in the CodeQL scan results.
    - Through Manually review the source code, we found that in the [line 277](https://github.com/zijunmei/elasticsearch/security/code-scanning/101) of the class JwtUtil.java, the programmer does not set a verify mechanism for checking validation of uri which is provided by user or the input should be limited such as must include a restrictive URL prefix. This may cause server-side request forgery (SSRF) attacks. It uses an insecure server in the domain as a proxy, which is similar to cross-site request forgery(CSRF) attacks using web clients.



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