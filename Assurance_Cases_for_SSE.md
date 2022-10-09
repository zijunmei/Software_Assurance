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
*E1 - System architectural design documents* \
The Elasticsearch architecture uses various design principles to ensure [high availability](https://www.elastic.co/guide/en/elasticsearch/reference/current/high-availability.html) \ 
\Some of those design features include the following:
- Scalability: Elasticsearch can scale up based on needs to add more capacity to handle transactions load.
- Load balancing: Elasticsearch by default does load balancing across all the cluster data nodes.  
- Data replication: Elasticsearch provides data replication features called Replicas https://www.elastic.co/guide/en/elasticsearch/reference/current/index-modules.html 
- Backup/restore: Elasticsearch cluster data backup/restore using snapshot feature https://www.elastic.co/guide/en/elasticsearch/reference/master/snapshot-restore.html
- Alerts: Monitoring the overall health of an Elasticsearch clusters to detect failures as they occur https://www.elastic.co/guide/en/kibana/current/kibana-alerts.html

#### *2.3.2. Unavailable/Insufficient Evidence*
*E2* \

## 4. Teamwork Reflection


