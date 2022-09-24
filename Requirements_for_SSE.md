# CYBR 8420 - Requirements for Software Security Engineering
  
## Part 1: Use/Misuse Case Analysis
### 1.1 User Login
#### *Use case*
User story: The hospital employee wants to be able to login to their Elasticsearch accounts so that they can use the function of Elasticsearch. 
#### *Misuse case*
In this case, the threats come from the *information thief* who try to gain the profit by selling the information of Hospital includes patient's information(ID, medical history, etc), hospital employee personal inforamtion(ID, address, insurrance, etc), and some other informations.<br>
  
The thief may try to crack the password of the employee, who has been stolen the username or ID, so that the thief can ilegally login the system with authorized account.  <br>
  
In the process, the thief will try to crack the user's password by trying various passwords. In ElasticSearch, passwords are stored in the database after hash encryption. Whenever a password is written to disk, it is not allowed to be plaintext at any time. When the user logs in, the encrypted password is retrieved from the database and compared with the hashed user input. In order to break a password that has been hash-encrypted, information thieves may use RainTable which is a pre-computed table for the inverse of the cryptographic hash function, commonly used to break encrypted cryptographic hashes. In this case, the Elasticsearch provides a stronger [password encryption](https://www.elastic.co/guide/en/elasticsearch/reference/current/security-settings.html) algorithms to mitigates the thief's behavior. <br>

Once the information thief has successfully obtained the login password, the [multi-factor authentication](https://www.elastic.co/guide/en/cloud/current/ec-account-user-settings.html#ec-account-security-mfa) is  an option for the user to secure their. The Elasticsearch provides two approaches: Google authenticator and text messages to realize the two factor authentication. However, the information thief may use man-in-the-middle attack to bypass the authentication. For preventing this, the elastisearch forces use [HTTPS](https://www.elastic.co/guide/en/elasticsearch/reference/master/modules-network.html) which can be used to securely communicate over HTTP using public-private key exchange. This prevents an attacker from having any use of the data he may be sniffing. The [IP filter](https://www.elastic.co/guide/en/elasticsearch/reference/current/ip-filtering.html) feature of Elasticsearch can also mitigates the attack is from the outside of hospital. 
#### *Diagram*
[The Diagram of User Login](images\authentication_use_case——02.drawio.png)
## Part 2: 

