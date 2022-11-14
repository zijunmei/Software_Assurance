# Designing For Software Security Engineering

## Part 1: Threat Model and Report
### Authentication DFD
![Authentication DFD](/images/Authentication_DFD.PNG)
### [Authentication Threat Model HTML Report](https://htmlpreview.github.io/?https://github.com/zijunmei/Software_Assurance/blob/main/Authentication_Threat_Model_Report.htm)<br/><br/>
### Data Access DFD
![image](https://user-images.githubusercontent.com/112530627/201564041-7add9458-5cee-4732-a3e8-89c5da128ae3.png)
### [Data Access Threat Model Report](https://htmlpreview.github.io/?https://github.com/zijunmei/Software_Assurance/blob/main/DFD_Model2.htm)<br/><br/>



## Part 2: Observations

### Threats Migitated

|STRIDE|Count|
|------|-----|
|Spoofing|9|
|Tampering|2|
|Repudiation|8|
|Information Disclosure|4|
|Denial of Service|9|
|Elevation of Privilege|9|


### Needs Investigation

|STRIDE|Count|
|------|-----|
|Spoofing|7|
|Tampering|0|
|Repudiation|0|
|Information Disclosure|0|
|Denial of Service|4|
|Elevation of Privilege|4|

### Authentication Threat model Analysis
The two main threats of the Authentication Threat model are Spoofing (9 of them) and Denial of Service (10). Those 2 threats are well mitigated within Elasticsearch architecture. Any data flows between external interactor (Hospital employee), data store and our system of interest (Elasticsearch) are all encrypted using TLS. The hospital employee also has to log in in Elasticsearch using User Id/Password authentication before accessing any data. This will mitigate any spoofing or tampering threats. 

The denial of service threats are also well mitigated by using back up nodes and replicating data to them in case of primary nodes crash or slowness due to denial of service attacks. A secondary cluster can be setup for redundancy or disaster recovery if needed. So, availability and resiliency is built-in in Elasticsearch architecture and well documented. 

Threat detection and prevention are also available in Elasticsearch by using monitoring tool such as Kibana dashboards to react to any suspicious or malicious activities.

One threat in the User Authentication DFD will need more investigation. It is related to Cross-site request forgery(CSRF) attack. Since we didn't analyze the external interactor application used by the Hospital employee to access Elasticsearch (our System of Interest), details how to manage CSRF attacks will need to be investigated. Elasticsearch front end application Kibana requires the use of [custom request headers](https://www.elastic.co/guide/en/kibana/current/api.html#api-request-headers) for Elasticsearch API endpoints. The same mechanism can be used by the Hospital employee application. 

### Data Access Threat model Analysis
The Data Access DFD maps the flow of data when a user uses the search function of Elasticsearch, to retrieve data stored in an Elasticsearch cluster. The user first provides search parameters to Elasticsearch, which determines what data will be returned to the user. These parameters are passed to the coordinating node, which will route the search request to the data node that is storing the requested data. That data node will then retrieve the requested data from a data directory, stored internally, and send it back to the coordinating node. Once the coordinating node has this data, it will verify the user has the correct privileges to access these documents by checking the user role config file, it will then return the data to the user. If, however, the data is stored on a remote cluster the coordinating node will route the search request to the remote-eligible node, which will act like a client and request the data from the correct remote cluster. Once the remote cluster has returned the correct data the remote-eligible node will send the data back to the coordinating cluster, which will again verify the user roles and return the requested documents.

According to this threat model report, one of the primary sources of threats is the inability to verify the authenticity of the data flow source. Verifying the data flow reliability is our main challenge while researching Elasticsearch and makes up 7 spoofing threats that we determined requires additional investigation. For example, in Threat Model Report 2, Threat #30, "Spoofing of Source Data Store User Role Config file," we were unable to determine if there was a mechanism for the Coordinating (client) node to verify the authenticity of the User Role Config file cannot be verified. According to the official documentation of Elasticsearch, we know that the coordinating node mainly serves the user's search requests. Therefore, it mostly plays the role of a query coordinator, assigning query tasks to the appropriate node and returning query results to users. We have yet to find evidence that the Coordinating node has any authentication mechanisms for the data flow source. The same threat also occurs when data nodes access data directory files that contain the requested documents and are stored inside node. In both cases the node does not verify the source of the data being retrieved and could be accessing data from a spoofed data source. There is a trust boundary between both the Remote-eligible Node and Remote Cluster processes. Establishing the authentication mechanism is an important consideration to ensure the system's security. However, we did not find any evidence to prove the existence of an authentication mechanism for cross-cluster data requests.

An ideal solution uses “[the host-based authentication mechanism (HBA)](https://www.ibm.com/docs/en/rsct/3.2?topic=cssac-understanding-cluster-security-services-privatepublic-key-based-mechanisms)” or “[the enhanced host-based authentication mechanism (HBA2)](https://www.ibm.com/docs/en/rsct/3.2?topic=cssac-understanding-cluster-security-services-privatepublic-key-based-mechanisms)”. The main principle of these two mechanisms is to create a unique private/public key pair for each node in the cluster. Only the node with the corresponding key pair can be considered trustworthy. 

Using this threat report we also determined that there is a significant number of denial-of-service threats that need further investigation. This is due to the fact that although Elasticsearch enables audit logging, which would detect a denial-of-service attack, it is unclear if there are any mechanisms enabled that can protect against external users interrupting data flow. After searching through Elasticsearch’s offered security features, and related issues opened it Elasticsearch’s Github, we were unable to find any information pointing to Elasticsearch’s ability to protect against external agents interrupting the data flow outside of clusters. 

Although the threats we determined require additional investigation were spread out across different threat types, they all revolve around a similar problem, how do nodes inside a cluster interact with resources. There is plenty of information regarding Elasticsearch and its basic security features, however we were unable to find much information on node communication, besides the occasional mention of nodes accessing a file or what type of nodes they communicate with. This causes a problem because without that information it is unclear whether Elasticsearch adequately protects against these threats or if there are optional security features available that can be enabled to further protect the system.

### Summary
Analyzing the threats from the 2 DFDs provided gave us an opportunity to review more of the Elasticsearch security configurations.
We can say that Elasticsearch has a rich set of security features. But at the same time, all the available security features may create some confusions for Elasticsearch customers. The different integration solutions offered may leave some security loopholes to be remediated.

Also, another observation is the fact that not all the Elasticserarch security features are available to all customers. The new version of Elasticsearch (8.5.0) has many different subscriptions (software licensing). Elasticsearch offers 3 types of licensing: the free and open Basic, Platinum and Enterprise versions. We found for example Authentication audit logging is not available in the free and open version.


## GitHub Project Board
- [Project Board](https://github.com/users/zijunmei/projects/2/views/1?filterQuery=Threat+Model)

## Teamwork Reflection
The team started late working on this assignment. The team members had questions what to deliver in the data flow diagram and how much details were needed. There were some confusions about the data flow vs control flow. The weekly team check-in with Dr. Gandi helped to clarify the deliverable. First drafts of DFDs were developed and reviewed during Zoom meetings and tasks were defined and assigned to the team members to complete the assignment. The team kept revising the DFDs to focus on the important use cases of our System-Of-Interest (Elasticsearch). 

Overall, our team worked hard to finalize the deliverable of this assignment and submit it on time. 

As always, the team would like to start the assignment early.


 
