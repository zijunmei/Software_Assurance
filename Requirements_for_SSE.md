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
### 1.3 
## Part 2: Security Review of Elasticsearch
### Configurations issues
The security configuration of ES mainly includes the following points:
- [TLS on the HTTP layer configuration](https://www.elastic.co/guide/en/elasticsearch/reference/current/security-basic-setup.html) 
    TLS on the HTTP layer it provides an additional layer of security to ensure that all communications to and from the cluster are encrypted.
- [User Authentication Configuration](https://www.elastic.co/guide/en/elasticsearch/reference/current/setting-up-authentication.html)
    Verify whether an account is a legitimate account.
- [User Authorization Configuration](https://www.elastic.co/guide/en/elasticsearch/reference/current/authorization.html)
    Define which operations each account can do and which indexes it can access.)
- [User Audit Logging Configuration](https://www.elastic.co/guide/en/elasticsearch/reference/current/enable-audit-logging.html)
    Monitor the clusters for suspicious activity.<br>  
*Authentication Configurations and issues*
ES's x-pack suite provides basic account authentication with a feature called Realm. depending on the payment, the Realm module provides different authentication capabilities. The open source version of Elasticsearch only provides a local account service, which can be configured locally by setting it up in elasticsearch.yml. It can also be set up through the [security api](https://www.elastic.co/guide/en/elasticsearch/reference/current/security-api.html) of ES. The paid version of Elasticsearch provides LDAP/kerbors/SAML/AD based authentication.<br>  
The issues that I found is that the open source version of Elasticsearch does not include LDAP、PKI, and Active Directory authentication features. In other words, the open source version of Elasticsearch only provides encrypted communication, [Naitive user authentication](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/native-realm.html). In this case, an organization that wants to use the open source version of Elasticsearch can only deploy it through the intranet and not provide the service to the public. The open source version of Elasticsearch does not have data protection features, and very easy to lead to online index or data may be accidentally deleted.<br>
*Authorization configurations and issues*

### Installation issues

