# CYBR 8420 - Requirements for Software Security Engineering
  
## Part 1: Use/Misuse Case Analysis
### 1.1 User Login
#### *Use case*
User story: The hospital employee wants to be able to login to their Elasticsearch accounts so that they can use the function of Elasticsearch. 
#### *Misuse case*
In this case, the threats come from the *information thief* who try to gain the profit by selling the information of Hospital includes patient's information(ID, medical history, etc), hospital employee personal inforamtion(ID, address, insurrance, etc), and some other informations.<br>
#### *Diagram*
![The Diagram of User Login](/images/authentication_use_case——02.drawio.png)
#### *Assessment* 
The thief may try to crack the password of the employee, who has been stolen the username or ID, so that the thief can ilegally login the system with authorized account.  <br>
  
In the process, the thief will try to crack the user's password by trying various passwords. In ElasticSearch, passwords are stored in the database after hash encryption. Whenever a password is written to disk, it is not allowed to be plaintext at any time. When the user logs in, the encrypted password is retrieved from the database and compared with the hashed user input. In order to break a password that has been hash-encrypted, information thieves may use RainTable which is a pre-computed table for the inverse of the cryptographic hash function, commonly used to break encrypted cryptographic hashes. In this case, the Elasticsearch provides a stronger [password encryption](https://www.elastic.co/guide/en/elasticsearch/reference/current/security-settings.html) algorithms to mitigates the thief's behavior. <br>

Once the information thief has successfully obtained the login password, the [multi-factor authentication](https://www.elastic.co/guide/en/cloud/current/ec-account-user-settings.html#ec-account-security-mfa) is  an option for the user to secure their account. The Elasticsearch provides two approaches: Google authenticator and text messages to realize the two factor authentication. However, the information thief may use man-in-the-middle attack to bypass the authentication. For preventing this, the elastisearch forces to use [HTTPS](https://www.elastic.co/guide/en/elasticsearch/reference/master/modules-network.html) which can be used to securely communicate over HTTP using public-private key exchange. This prevents an attacker from having any use of the data he may be sniffing. The [IP filter](https://www.elastic.co/guide/en/elasticsearch/reference/current/ip-filtering.html) feature of Elasticsearch can also mitigates the attack is from the outside of hospital. <br>
  
Based on the above analysis of the Elasticsearch development documentation, I noticed that the Elastisearch use [FIPS 140-2](https://www.elastic.co/guide/en/elasticsearch/reference/current/fips-140-compliance.html) as requirements for Cryptographic Modules. I believe that Elasticsearch basically satisfy the user requirements for secure login. There are sufficient countermeasures for threats from information thieves.

### 1.2 
### 1.3 Access Patient Data in a timely manner 
#### *Use case*
User story: The hospital customer service representative needs to access patient data stored in Elasticsearch nodes such as medical records,
future doctor appointments, lab results and pharmacy prescriptions in a timely manner.
#### *Misuse case*
The threat in this case is the availabity of the data. A denial-of-service attack or slow running Elasticsearch queries 
by internal employees will impact the availability of the data needed for the customer service representative to help patients
calling in. The health information provided to patients are critical for their well-being and need to be 
searched and retrieved fast from Elasticsearch data.
#### *Diagram*
#### *Assessment*
Jack (the extortionist) launched a DOS attack on the hospital network to overload the Elasticsearch cluster nodes
by repetitive malicious queries searches. His goal was to bring the hospital patient Elasticsearch data down.
If Jack succeeds then he will be in force to demand a ransome from the hospital to stop his attack.

Another threat was from Sam (the billing system new hire) with just few weeks on the job and with little training on the Elasticsearch stack.
Sam's Elasticsearch queries were so complex and returning a large dataset. 
His queries disrupted the whole customer service department since slow query responses were noticed.
Customer service representatives were not able to help patients in a timely manner.

Both threats were running queries which took too much time to complete and also a lot of Elasticsearch 
nodes resources (CPU, memory). Since the hospital depends on the availablity of patients records, to prevent and detect
the vulnerabilities observed, a system security review was needed.   

Elasticsearch security features provide some security settings to build a system resilient to attacks mentioned above.
The following security measures can be implemented to prevent such attacks:<br/><br/>
**Config Elasticsearch query timeout**<br/>
Query timeout set by default and can be adjusted as needed. 
The link below gives the different JDBC settings available
[SQL JDBC Settings](https://www.elastic.co/guide/en/elasticsearch/reference/8.4/sql-jdbc.html#sql-jdbc-installation)<br/>

**Setup Elasticsearch circuit breaker**<br/>
The circuit breaker are set to prevent the nodes from running out of JVM heap memory.
This is to prevent queries with a large dataset result 
[Circuit breakers Settings](https://www.elastic.co/guide/en/elasticsearch/reference/8.4/circuit-breaker-errors.html)<br/>

**Elasticsearch Monitoring dashboard**<br/>
An Elasticsearch search offers different options to monitor Elasticsearch traffic.
At the cluster level, Rest API are available to monitor the health of Elasticsearch cluster 
[Cluster health API](https://www.elastic.co/guide/en/elasticsearch/reference/8.4/cluster-health.html)<br/>

The production cluster can be configured to collect data to send it to monitoring dashboard 
[Cluster monitoring](https://www.elastic.co/guide/en/elasticsearch/reference/current/monitoring-production.html)<br/>

Tools from Elasticsearch stack (ELK) like Kibana has built in dashboard to send alerts to Elasticsearch Admin
[XPACK alerts](https://www.elastic.co/guide/en/elasticsearch/reference/8.4/xpack-alerting.html)<br/>

**Set up a cluster for high availability**<br/>
Elasticsearch offers some features to set up high availability clusters in case of failures or to route traffic to other
clusters in case of denial-of-service attacks for example
[High availability clusters](https://www.elastic.co/guide/en/elasticsearch/reference/8.4/high-availability.html)<br/>

Based on the Elasticsearch security features above and the other ones available, we can say 
that Elasticsearch offers a rich set of security measures to protect the data and prevents availability attacks like the ones used in 
our use case. 
Elasticsearch is built with security in mind and it is evolving to adapt and implement new features to remediate new security risks findings
Latest release [Elasticsearch latest release](https://www.elastic.co/guide/en/elasticsearch///reference/master/release-notes-8.4.0.html)
 

## Part 2: Security Review of Elasticsearch
### 2.1 Configurations issues
The security configuration of Elasticsearch mainly includes the following points:
- [Communication Encryption Configuration](https://www.elastic.co/guide/en/elasticsearch/reference/current/security-basic-setup.html) 
    - TLS on the HTTP layer it provides an additional layer of security to ensure that all communications to and from the cluster are encrypted.
- [User Authentication Configuration](https://www.elastic.co/guide/en/elasticsearch/reference/current/setting-up-authentication.html)
    - Verify whether an account is a legitimate account.
- [User Authorization Configuration](https://www.elastic.co/guide/en/elasticsearch/reference/current/authorization.html)
    - Define which operations each account can do and which indexes it can access.)
- [User Audit Logging Configuration](https://www.elastic.co/guide/en/elasticsearch/reference/current/enable-audit-logging.html)
    - Monitor the clusters for suspicious activity.<br>  

**Communication encryption configuration**  
Communication encryption includes internode communication and HTTP client communication.[Here](https://www.elastic.co/guide/en/elasticsearch/reference/7.16/configuring-tls.html#node-certificates) is the description.<br>
- Internodes communication
Encryption for internodes communication needs to be done by configuring the Elasticsearch. The encryption is done by configuring certificates and using ssl. The main purpose of internode communication Encryprtion is to 1. prevent illegal Elasticsearch nodes from joining the cluster and 2. prevent communication traffic from being listened to.<br>
- HTTP client communication
Elasticsearch itself provides an http-based REST interface to the outside world, and the communication of this interface needs to be encrypted, which needs to be configured in elasticsearch.yml.<br>
Transport Protocol is the name of the protocol that Elasticsearch nodes use to communicate with one another. This name is specific to Elasticsearch and distinguishes the transport port (default 9300) from the HTTP port (default 9200). Nodes communicate with one another using the transport port, and REST clients communicate with Elasticsearch using the HTTP port.<br>

**Authentication Configurations**  
Elasticsearch's x-pack suite provides basic account authentication with a feature called Realm. Depending on the version of Elasticsearch, the Realm module provides different authentication capabilities. The open source version of Elasticsearch only provides a local account service, which can be configured locally by setting it up in elasticsearch.yml. It can also be set up through the [security api](https://www.elastic.co/guide/en/elasticsearch/reference/current/security-api.html) of Elasticsearch. The paid version of Elasticsearch provides LDAP/kerbors/SAML/AD based authentication.<br>  

**Authorization configurations**  
The Authorization capability of Elasticsearch uses a role-based access control approach (RBAC). Elasticsearch provides two catagories of security privileges, with more fine-grained permissions under these two categories. This is described [here](https://www.elastic.co/guide/en/elasticsearch/reference/7.16/security-privileges.html). <br>
- Cluster Privileges  
    - Various cluster management capabilities are provided.
- Indic Privileges  
    - Provides access control to a field level, as well as index level operations.  
The run_as permission enables an authenticated user to submit requests on behalf of another user. The value can be a user name or a comma-separated list of user names. It is worthwhile to continue to observe whether this permission will lead to security risks.<br>  

**Audit Logging Configuration**  
To enable Elasticsearch audit logging, you need to add a configuration to the Elasticsearch configuration file elasticsearch.yml.  <br>
    <mark> **xpack.security.audit.enabled:true** <mark>  <br>
After completing the configuration, the node needs to be restarted to take effect. [Here](https://www.elastic.co/guide/en/elasticsearch/reference/master/enable-audit-logging.html) is the descirption. After the audit logging function is turned on, there will be a file called "xxx_audit.json" in the log directory of the corresponding node, which will have relevant security events recorded in it.

### 2.2 Installation
Before [installing Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/install-elasticsearch.html), a newer version of Java(at least Java 8) is required. Then we can get the latest version of Elasticsearch from [Elastic's official website](elastic.co/downloads/elasticsearch). Elasticsearch can be used on multiple platforms, so it has a variety of different installation packages to meet the needs of different platforms. The installation instruction of each platform as following:
- [Linux and MacOS](https://www.elastic.co/guide/en/elasticsearch/reference/current/targz.html)
- [Windows .zip archive](https://www.elastic.co/guide/en/elasticsearch/reference/current/zip-windows.html)
- [deb](https://www.elastic.co/guide/en/elasticsearch/reference/current/deb.html)
- [rpm](https://www.elastic.co/guide/en/elasticsearch/reference/current/rpm.html)
- [docker](https://www.elastic.co/guide/en/elasticsearch/reference/8.4/docker.html)

### 2.3 Issues 
The issues we found after reviewing the security documents of the Elasticsearch is that the open source version of Elasticsearch does not include LDAP、PKI, SAML, and Active Directory(AD) authentication features. In other words, the open source version of Elasticsearch only provides encrypted communication and [Naitive user authentication](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/native-realm.html)features. In this case, once an organization wants to use the open source version of Elasticsearch, then they can only deploy it through the intranet and not provide the service to the public. Otherwise, They have to looking for a third-party security certification scheme for their system. The open source version of Elasticsearch does not have data protection features, and very easy to lead to online index or data may be accidentally deleted.<br>  
Overall, our review of security documents identified several potentially dangerous behaviors leading to safety incidents.
- No setting up Elasticsearch cluster security permissions. 
- Elasticsearch server exposed to the pubilc network.
- Port 9200 mapped to pubilc network.
