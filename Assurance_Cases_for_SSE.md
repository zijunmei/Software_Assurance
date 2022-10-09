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
The machine IP from where the malicious query is running is rejected and denied access to run queries. <br/><br/>

#### *2.3.2. Unavailable/Insufficient Evidence*
***E3 - Static code analysis report*** \
Elasticsearch documentation doesn't include any information about code scanning.
We believe that it is being done internally at Elasticsearch and not shared.
Elasticsearch code scanning results need to be available to provide as evidence that the software is free from any security vulnerabilities
to build trusts and confidence required by its users. 

***E4 - Elasticsearch performance load test results*** \
This evidence needs to be provided by the system-of-interest (Elasticsearch) admin since it will depend on the environment of operation. 
Elasticsearch offers application performance monitoring (APM) apps and [Kibana dashboard monitoring](https://www.elastic.co/guide/en/kibana/current/elasticsearch-metrics.html) to observe performance load testing metrics. 

## 4. Teamwork Reflection


