## Part 1: Assurance Cases
### 1.1 Assurance Case 1
**Assurance Case:** Elasticsearch acceptably minimizes the risk of unauthorized access.

![Assurance Case 1](/images/Assurance%20Case%20final.png) 

### 1.2 Assurance Case 2
**Assurance Case:** 

### 1.3 Assurance Case 3
**Assurance Case:** Patient data is available in timely manner.

![Availability Assurance Case](/images/Availability_Assurance_Case.jpg)


## Part 2: Evidence Alignment
### 2.1. Assurance Case 1

#### *2.1.1. Available Evidence*
***E1 - Manual source code review*** \
In the [security setting documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/security-settings.html#password-hashing-algorithms), Elasticsearch provides a series of [password hashing algorithms](https://github.com/elastic/elasticsearch/blob/be7c7415627377a1b795400fb8dfcc6cbdf0e322/docs/reference/settings/security-hash-settings.asciidoc) for encrypting the user's password before storage. Based on the result of a manual check of the [source code](https://github.com/elastic/elasticsearch/blob/be7c7415627377a1b795400fb8dfcc6cbdf0e322/x-pack/plugin/core/src/main/java/org/elasticsearch/xpack/core/security/authc/support/Hasher.java), We believe that Elasticsearch does provide strong encryption algorithm support for user password storage, as the description in the documentation.<br><br>

***E7 - Elasticsearch's two-factor authentication policy*** \
In the [issue#84784](https://github.com/elastic/kibana/issues/84784), the developer of Elasticsearch indicates that they encourage users needing more sophisticated setups to adopt an external autentication provider such as [SAML](https://www.elastic.co/guide/en/elasticsearch/reference/current/saml-realm.html) or [OIDC](https://www.elastic.co/guide/en/elasticsearch/reference/8.4/security-api-oidc-authenticate.html). Therefore, Elasticsearch itself does not provide 2FA-authentication, but it support external autentication provider to do it. Here are more issues about 2FA in Elasticsearch: [issue#39414](https://github.com/elastic/kibana/issues/39414) and [issue#31773](https://github.com/elastic/kibana/issues/31773). <br><br>

#### *2.1.2. Unavailable/Insufficient Evidence*

***E2 - Review of TLS configuration setting*** \
Reviewing of TLS configuration setting of Elasticsearch should be completed by the system-of-interest (Elasticsearch) admin. 
This evidence is to prove that Elasticsearch applies strong TLS for secure communication between users and clusters. TLS versions can be enabled and disabled within Elasticsearch via the [ssl.supported_protocols](https://www.elastic.co/guide/en/elasticsearch/reference/8.4/security-settings.html#ssl-tls-settings). However, the Elasticsearch will only support the TLS versions that are enabled by the underlying JDK. In this case, newest TLS v1.3 is supported on JDK11 and later, and JDK8 builds newer than 8u261.It's description in [here](https://www.elastic.co/guide/en/elasticsearch/reference/8.4/jdk-tls-versions.html#jdk-enable-tls-protocol). Therefore, keeping the JDK version up to date is also a guarantee of security.<br><br>

***E3 - Elasticsearch password policy*** \
Unfortunately, the password policy is unavailable in Elasticsearch. 
In fact, contributors of Elasticsearch have demanded the password policy in [iusse#29913](https://github.com/elastic/elasticsearch/issues/29913). However, in [issue#84784](https://github.com/elastic/kibana/issues/84784), the developer of Elastcsearch mentioned that "we've historically tried not to make Elasticsearch a full-fledged authentication/identity provider". They encouraged users needing more sophisticated setups to adopt an external identity provider. Therefore, we believe that there is a wide *gap* between the evidence for assurance case and the actual<br><br>

***E4 - Testing results of multiple incorrect passoword attempts*** \
This evidence is unavailable. However, we found that, in the [issue#18491](https://github.com/elastic/kibana/issues/18491) and [issue##84784](https://github.com/elastic/kibana/issues/84784), the developer of Elasticsearch so far is not apt to add an account lockout protection functionality to reduce the risk of [Brute-force attack](https://attack.mitre.org/techniques/T1110/003/). Furthermore, Developers believe they need a holistic, stack-wide solution to reducing the threat from brute force cracking.<br><br>

***E5 - Audit logging configuration report*** \
This evidence is unavailable, and we believe it needs to be completed by the system-of-interest (Elasticsearch) security auditors. This evidence is to prove that the audit logging function has been correctly [enabled](https://www.elastic.co/guide/en/elasticsearch/reference/current/enable-audit-logging.html) and [configured](https://www.elastic.co/guide/en/elasticsearch/reference/current/auditing-settings.html). Any misconfiguration will cause a serise of issues.
<br><br>

***E6 - Audit logging test results*** \
This evidence is unavailable, and we believe it needs to be completed by the system-of-interest (Elasticsearch) security auditors. This evidence is to prove that the audit logging function is working correctly and that it can guarantee the detection of abnormal access. The testing should based on the [aduit events](https://www.elastic.co/guide/en/elasticsearch/reference/current/audit-event-types.html).
<br><br>

### 2.2. Assurance Case 2

#### *2.2.1. Available Evidence*
*E1* \

#### *2.2.2. Unavailable/Insufficient Evidence*
*E2* \

### 2.3. Assurance Case 3

#### *2.3.1. Available Evidence*
***E1 - System architectural design documents*** \
The Elasticsearch architecture uses various design principles to ensure [high availability](https://www.elastic.co/guide/en/elasticsearch/reference/current/high-availability.html) <br/>
Some of those design features include the following:
- Scalability: Elasticsearch scales up based on needs to add more capacity to handle transactions load.
- Load balancing: Elasticsearch by default does load balancing across all the cluster data nodes.  
- Data replication: Elasticsearch provides data replication features called [Replicas](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-modules.html). 
- Backup/restore: Elasticsearch cluster data backup/restore using [snapshot feature](https://www.elastic.co/guide/en/elasticsearch/reference/master/snapshot-restore.html).
- Alerts: Elasticsearch is properly instrumented to [monitor](https://www.elastic.co/guide/en/kibana/current/kibana-alerts.html) the overall health of an Elasticsearch clusters to detect failures as they occur. <br/><br/>

***E2 - Manual code review at code check-in*** \
Elasticsearch is an open source software and anyone can contribute to its codebase.
Best practices guidelines are listed on the Elasticsearch github project explaining how to submit a code change.
Pull requests are needed to merge code in the main branch after code review is completed by the reviewer.  <br/><br/>

***E5 - Circuit breaker error when running query with large dataset*** \
Elasticsearch [circuit breakers](https://www.elastic.co/guide/en/elasticsearch/reference/current/circuit-breaker.html) are used to limit the memory usage by the queries to prevent JVM heap out of memory errors. This is case of query returning a large dataset result. <br/><br/>

***E6 - Timeout error for long running query*** \
Query timeout is set on the [Elasticsearch JDBC driver](https://www.elastic.co/guide/en/elasticsearch/reference/8.4/sql-jdbc.html#sql-jdbc-installation).
This settings set the maximum amount of time waiting for a query to return. <br/><br/>

***E7 - Elasticsearch  monitoring alert notification*** \
An Elasticsearch search offers different options to monitor Elasticsearch traffic. 
At the cluster level, [cluster health API](https://www.elastic.co/guide/en/elasticsearch/reference/8.4/cluster-health.html)
are available to monitor the health of Elasticsearch cluster. Also, as mentioned above, Elasticsearch Kibana has an extensive set of monitoring dashboards to visualize Elasticsearch clusters, nodes and user activities. [Watchers](https://www.elastic.co/guide/en/kibana/current/watcher-ui.html) are used to create actions based on system conditions.<br/><br/>

***E8 - Elasticsearch IP traffic filtering report*** \
The Elasticsearch security features contain an access control feature called [IP filtering](https://www.elastic.co/guide/en/elasticsearch/reference/current/ip-filtering.html) which allows or rejects hosts IP addresses.
The source IP of the malicious query is rejected and denied access to run queries. <br/><br/>

#### *2.3.2. Unavailable/Insufficient Evidence*
***E3 - Static code analysis report*** \
Elasticsearch documentation doesn't include any information about code scanning.
We believe that it is being done internally at Elasticsearch and not shared.
Elasticsearch code scanning results need to be available to provide as evidence that the software is free from any security vulnerabilities
to build trusts and confidence required by its users. <br/><br/>

***E4 - Elasticsearch performance load test results*** \
***E9 - Elasticsearch IP monitoring test results*** \
***E10 - Disaster recovery exercise report*** \
***E11 - Risk management document*** \
All the evidences above need to be provided by the system-of-interest (Elasticsearch) technical team since it will depend on the environment of operation.<br/><br/>
Load test and IP monitoring test results are available from Elasticsearch such as application performance monitoring (APM) apps and [Kibana dashboard monitoring](https://www.elastic.co/guide/en/kibana/current/elasticsearch-metrics.html) to observe the performance load testing metrics and the submitted query source IP addresses. The tests need to be run and results provided to stakeholders as assurance cases for the Elasticsearch availablity claim. <br/><br/>
Disaster recovery readiness and risk management documents both need to be prepared and conducted by knowledgeable stakeholders and provided to those involved in certification, regulation, acquisition, or audit of the system.

## 3. Project Board Link
- [Project Board](https://github.com/users/zijunmei/projects/2/views/1?filterQuery=Assurance+Case+Task)
## 4. Teamwork Reflection


