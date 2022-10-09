## Part 1: Assurance Cases
### 1.1 Assurance Case 1
**Assurance Case:** 

### 1.2 Assurance Case 2
**Assurance Case:** 

### 1.3 Assurance Case 3
**Assurance Case:** Patient data is available in timely manner.

![Availability Assurance Case](/images/Availability_Assurance_Case.jpg)


## Part 2: Evidence Alignment
### 2.1. Assurance Case 1

#### *2.1.1. Available Evidence*
*E1* \
 ...........

#### *2.1.2. Unavailable/Insufficient Evidence*
*E2* \

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



#### *2.3.2. Unavailable/Insufficient Evidence*
***E3 - Static code analysis report*** \
Elasticsearch documentation doesn't include any information about code scanning.
We bieleve that it is being done internally at Elasticsearch and not shared.
Elasticsearch code scanning results need to be available to provide as evidence that the software is free from any security vulnerabilities
to build trusts and confidence required by its users. 

## 4. Teamwork Reflection

