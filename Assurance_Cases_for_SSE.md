## Part 1: Assurance Cases
### 1.1 Assurance Case 1
**Assurance Case:** Elasticsearch acceptably minimizes the risk of unauthorized access.

![Assurance Case 1](/images/Assurance%20Case%20final.png) 

### 1.2 Assurance Case 2
**Assurance Case:** Stored data is acceptably protected from unauthorized changes.

![Stored Data Assurance Case](https://user-images.githubusercontent.com/112530627/194796085-de64da12-61d2-40b1-af85-a542088f7a11.png)


### 1.3 Assurance Case 3
**Assurance Case:** Patient data is available in timely manner.

![Availability Assurance Case](/images/Availability_Assurance_Case.jpg)


## Part 2: Evidence Alignment
### 2.1. Assurance Case 1

#### *2.1.1. Available Evidence*
***E1 - Manual source code review*** \
In the [security setting documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/security-settings.html#password-hashing-algorithms), Elasticsearch provides a series of [password hashing algorithms](https://github.com/elastic/elasticsearch/blob/be7c7415627377a1b795400fb8dfcc6cbdf0e322/docs/reference/settings/security-hash-settings.asciidoc) for encrypting the user's password before storage. Based on the result of a manual check of the [source code](https://github.com/elastic/elasticsearch/blob/be7c7415627377a1b795400fb8dfcc6cbdf0e322/x-pack/plugin/core/src/main/java/org/elasticsearch/xpack/core/security/authc/support/Hasher.java), We believe that Elasticsearch does provide strong encryption algorithm support for user password storage, as described in the documentation in the documentation.<br><br>

***E7 - Elasticsearch's two-factor authentication policy*** \
In [issue #84784](https://github.com/elastic/kibana/issues/84784), the developer of Elasticsearch indicates that they encourage users needing more sophisticated setups to adopt an external authentication provider such as [SAML](https://www.elastic.co/guide/en/elasticsearch/reference/current/saml-realm.html) or [OIDC](https://www.elastic.co/guide/en/elasticsearch/reference/8.4/security-api-oidc-authenticate.html). Therefore, Elasticsearch itself does not provide 2FA authentication, but it supports an external authentication provider to do it. Here are more issues about 2FA in Elasticsearch: [issue #39414](https://github.com/elastic/kibana/issues/39414) and [issue #31773](https://github.com/elastic/kibana/issues/31773). <br><br>

#### *2.1.2. Unavailable/Insufficient Evidence*

***E2 - Review of TLS configuration setting*** \
A review of the TLS configuration setting of Elasticsearch should be completed by the system-of-interest (Elasticsearch) admin. 
This evidence proves that Elasticsearch applies strong TLS for secure communication between users and clusters. TLS versions can be enabled and disabled within Elasticsearch via the [ssl.supported_protocols](https://www.elastic.co/guide/en/elasticsearch/reference/8.4/security-settings.html#ssl-tls-settings). However, Elasticsearch will only support the TLS versions that the underlying JDK enables. In this case, the newest TLS v1.3 is supported on JDK11 and later, and JDK8 builds newer than 8u261. There is a description [here](https://www.elastic.co/guide/en/elasticsearch/reference/8.4/jdk-tls-versions.html#jdk-enable-tls-protocol). Therefore, keeping the JDK version up to date is also a security guarantee.<br><br>

***E3 - Elasticsearch password policy*** \
Unfortunately, the password policy is unavailable in Elasticsearch. 
Contributors of Elasticsearch have demanded a password policy in [issue #29913](https://github.com/elastic/elasticsearch/issues/29913). However, in [issue #84784](https://github.com/elastic/kibana/issues/84784), the developer of Elasticsearch mentioned that "we've historically tried not to make Elasticsearch a full-fledged authentication/identity provider." They encouraged users needing more sophisticated setups to adopt an external identity provider. Therefore, we believe there is a wide *gap* between the evidence for the assurance case and the actual one.<br><br>

***E4 - Testing results of multiple incorrect passoword attempts*** \
This evidence is unavailable. However, we found that, in [issue #18491](https://github.com/elastic/kibana/issues/18491) and [issue #84784](https://github.com/elastic/kibana/issues/84784), the developer of Elasticsearch so far is not apt to add an account lockout protection functionality to reduce the risk of [Brute-force attack](https://attack.mitre.org/techniques/T1110/003/). Furthermore, developers believe they need a holistic, stack-wide solution to reduce the threat of brute force cracking.<br><br>

***E5 - Audit logging configuration report*** \
This evidence is unavailable, and we believe it needs to be completed by the system-of-interest (Elasticsearch) security auditors. This evidence proves that the audit logging function has been correctly [enabled](https://www.elastic.co/guide/en/elasticsearch/reference/current/enable-audit-logging.html) and [configured](https://www.elastic.co/guide/en/elasticsearch/reference/current/auditing-settings.html). Any misconfiguration will cause a series of issues.
<br><br>

***E6 - Audit logging test results*** \
This evidence is unavailable, and we believe it needs to be completed by the system-of-interest (Elasticsearch) security auditors. This evidence proves that the audit logging function is working correctly and can guarantee the detection of abnormal access. The testing should be based on the [audit events](https://www.elastic.co/guide/en/elasticsearch/reference/current/audit-event-types.html) of Elasticsearch.
<br><br>

### 2.2. Assurance Case 2

#### *2.2.1. Available Evidence*
***E1 - Script security measures review*** \
Elasticsearch does provide the opportunity for [scripting](https://www.elastic.co/guide/en/elasticsearch/reference/master/modules-scripting.html) and includes documentation that describes some of the security restrictions in place for scripts. In the documentation there are three security measure described: Elasticsearch???s main scripting language, Painless, includes an allow list and anything not in the allow list will cause a compilation error, Java Security Manager does not allow scripts to write to files, and script setting that can be configured to restrict certain script types and contexts.<br><br>

***E3 - Role mapping access restrictions*** \
In Elasticsearch???s [Mapping users and groups to roles](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-roles.html) documentation Elasticsearch describes how roles are mapped to users through either an API or a stored YAML file. The [create or update role mappings API](https://www.elastic.co/guide/en/elasticsearch/reference/current/security-api-put-role-mapping.html) document further explains what level privileges are necessary to access the API to map users to roles. However, there does not appear to be any documentation on what level privileges are required to change the role mapping YAML file.<br><br>

***E5 - Users API documentation review*** \
The [REST APIs documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/rest-apis.html) lists multiple APIs that could be used to identify other users and their privileges including the [get user API](https://www.elastic.co/guide/en/elasticsearch/reference/current/security-api-get-user.html) and the [has privileges API](https://www.elastic.co/guide/en/elasticsearch/reference/current/security-api-has-privileges.html). The documentation for each of these APIs includes the prerequisite privileges required by the user.<br><br>

***E6 - Unauthorized access Assurance Case results*** \
The unauthorized access Assurance Case above describes the protections in place that can help prevent a valid user account from being compromised. It also includes evidence supporting these claims which in turn is evidence that supports the claim that user accounts are protected against compromise.<br><br>

***E7 - Input sanitation documentation review*** \
Elasticsearch does not appear to provide XSS protections by default, however Elastic does provide a [sanitization guide](https://www.elastic.co/guide/en/app-search/current/sanitization-guide.html) which describes some of the XSS vulnerabilities, and explains a protection implemented by Elastic that can be set by the user to prevent these vulnerabilities.<br><br>

#### *2.2.2. Unavailable/Insufficient Evidence*
***E2 - Results of script capability tests*** \
Currently there are no test results provided by Elasticsearch for the script security measures available. This would generally fall to the admin in charge of implementing Elasticsearch in the production environment and would be relatively easy to test since there is adequate documentation regarding scripts security settings.<br><br>

***E4 - Role mapping test results*** \
There are no test results provided by Elasticsearch proving that role mapping is restricted to high privilege users. Again, this would most likely be completed by the admin implementing Elasticsearch in the production environment and should be easily run because of the provided documentation from Elasticsearch.<br><br>

***E8 - XSS pentest results*** \
Elasticsearch has not performed a pentest on their XSS protections or published the results. This would most likely not be done by Elasticsearch but would fall under a third-party application security team.<br><br>

### 2.3. Assurance Case 3

#### *2.3.1. Available Evidence*
***E1 - System architectural design documents*** \
The Elasticsearch architecture uses various design principles to ensure [high availability](https://www.elastic.co/guide/en/elasticsearch/reference/current/high-availability.html). <br/>
Some of those design features include the following:
- Scalability: Elasticsearch scales up based on needs to add more capacity to handle transaction load.
- Load balancing: Elasticsearch, by default, does load balancing across all the cluster data nodes.  
- Data replication: Elasticsearch provides data replication features called [Replicas](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-modules.html). 
- Backup/restore: Elasticsearch cluster data backup/restore using [snapshot feature](https://www.elastic.co/guide/en/elasticsearch/reference/master/snapshot-restore.html).
- Alerts: Elasticsearch is properly instrumented to [monitor](https://www.elastic.co/guide/en/kibana/current/kibana-alerts.html) the overall health of Elasticsearch clusters to detect failures as they occur. <br/><br/>

***E2 - Manual code review at code check-in*** \
Elasticsearch is open-source software, and anyone can contribute to its codebase.
Best practices guidelines are listed on the Elasticsearch GitHub project explaining how to submit a code change.
Pull requests are needed to merge code in the main branch after the code review is completed by the reviewer.  <br/><br/>

***E5 - Circuit breaker error when running query with large dataset*** \
Elasticsearch [circuit breakers](https://www.elastic.co/guide/en/elasticsearch/reference/current/circuit-breaker.html) are used to limit the memory usage by the queries to prevent JVM heap out-of-memory errors. This is the case of a query returning a large dataset result. <br/><br/>

***E6 - Timeout error for long running query*** \
Query timeout is set on the [Elasticsearch JDBC driver](https://www.elastic.co/guide/en/elasticsearch/reference/8.4/sql-jdbc.html#sql-jdbc-installation).
These settings set the maximum amount of time waiting for a query to return. <br/><br/>

***E7 - Elasticsearch  monitoring alert notification*** \
An Elasticsearch search offers different options to monitor Elasticsearch traffic. 
At the cluster level, [cluster health API](https://www.elastic.co/guide/en/elasticsearch/reference/8.4/cluster-health.html)
are available to monitor the health of Elasticsearch cluster. Also, as mentioned above, Elasticsearch Kibana has an extensive set of monitoring dashboards to visualize Elasticsearch clusters, nodes, and user activities. [Watchers](https://www.elastic.co/guide/en/kibana/current/watcher-ui.html) are used to create actions based on system conditions.<br/><br/>

***E8 - Elasticsearch IP traffic filtering report*** \
The Elasticsearch security features contain an access control feature called [IP filtering](https://www.elastic.co/guide/en/elasticsearch/reference/current/ip-filtering.html) which allows or rejects hosts IP addresses.
The source IP of the malicious query is rejected and denied access to run queries. <br/><br/>

#### *2.3.2. Unavailable/Insufficient Evidence*
***E3 - Static code analysis report*** \
Elasticsearch documentation doesn't include any information about code scanning.
We believe that it is being done internally at Elasticsearch and not shared.
Elasticsearch code scanning results need to be available to provide as evidence that the software is free from any security vulnerabilities
to build trust and confidence required by its users. <br/><br/>

***E4 - Elasticsearch performance load test results*** \
***E9 - Elasticsearch IP monitoring test results*** \
***E10 - Disaster recovery exercise report*** \
***E11 - Risk management document*** \
All the evidences above need to be provided by the system-of-interest (Elasticsearch) technical team since it will depend on the environment of operation.<br/><br/>
Load test and IP monitoring test results are available from Elasticsearch such as application performance monitoring (APM) apps and [Kibana dashboard monitoring](https://www.elastic.co/guide/en/kibana/current/elasticsearch-metrics.html) to observe the performance load testing metrics and the submitted query source IP addresses. The tests need to be run and results provided to stakeholders as assurance cases for the Elasticsearch availability claim. <br/><br/>
Disaster recovery readiness and risk management documents both need to be prepared and conducted by knowledgeable stakeholders and provided to those involved in the certification, regulation, acquisition, or audit of the system.

## 3. Project Board Link
- [Project Board](https://github.com/users/zijunmei/projects/2/views/1?filterQuery=Assurance+Case+Task)
## 4. Teamwork Reflection
The team had some chalenges understanding and defining assurance cases related 
to our System-Of-Interest. We had a few very long Zoom meetings talking about the deliverable. <br/>  
First drafts of assurance cases were developed and reviewed. The team kept revising 
them to make the assurance claims arguments useful and related to critical properties of our system. 
Feedbacks from Dr. Gandhi also helped to make the corrections needed. <br/>

Overall, our teamwork worked hard to finalize the deliverable of this assignment. 
Going forward, the team would like to start the assignment early and have a more organized short team meetings. 


