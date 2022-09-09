# Software_Assurance 

The following are some ideas we can use in the project proposal:

## List Team members and their roles in the project:
From my side, I can be Team Lead / Technical

## Operation environment will be health care provider (hospital).
I thought it will be a good idea to build our security cases around PII/HIPAA rules: 
PII(Personally identifiable information)/PHI(Protected health information) to protect patient privacy data
and system be HIPAA compliant.

## Open-source software:  Elastic Stack

## Elasticsearch Architecture:
I will provide the diagram of System Engineering View later.<br/>
We will include the following Elasticsearch clusters, nodes, Beats, Logstash , Elasticsearch and Kibana

### Kibana is the UI of Elasticsearch to visualize Elasticsearch query results
For example we can say that we will have different dashboards (Doctors Dashboard / Nurses Dashboard / Patient Care Services Dashboard / SysAdmin Dashboard)

### Elasticsearch cluster/nodes  used to store/search & analyze

### Beats and Logstash to Ingest data

### Enabling System
Jenkins (build, test, and deploy)<br/>
Jira (report and track issues resolution)<br/>
IDE / Code Compiler<br/>
Unit and Integration Tests<br/>

### Other Systems
Billing System<br/>
Patient Care Web Portal<br/>
System Admin Monitoring Web App<br/>

## The threats perceived by users of the software
Malicious Data breach from Elasticsearch data clusters<br/>
Unauthorized access to patient PII/PHI data from health care personnel<br/>
Elasticsearch resiliency issues<br/>
Data loss due to system failure, disaster or humans intentional/unintential errors<br/>
Elasticsearch security is not enabled by default. If an Elasticsearch installation left without security set then it can be exploited to access data<br/>

