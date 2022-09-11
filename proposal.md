# CYBR 8420 - Project Proposal
# Open-Source Project: Elasticsearch  
## 0. Project members and resource links
### Members
- Mustapha (@sammy-uno): Team leader, Technical
- Zijun (@zijunmei): Project management, Technical
- Charlie (@cbotdorf): Writing/Documenting, Reviewer
- Jeremy (@oreb379): Writing/Documenting, Reviewer
### Links
* [Team project repository](https://github.com/zijunmei/Software_Assurance)  
  
* [Project Board](https://github.com/users/zijunmei/projects/2)
      
* [Open source software repository (Elasticsearch)](https://github.com/elastic/elasticsearch)
## 1. Hypothetical Operational Environment (Zijun)

Data is an important company asset, and different viewers should only be able to search for data that meets his permission level. Enterprise-level search supports comprehensive permission management configuration of data sources to meet enterprises' data security browsing needs.  

Let's say a hospital is seeking a technology capable of enterprise-level search engines for building their system based on PII/HIPAA rules. They require the search engine to be able to adjust the search results based on the visitor's search privileges. For example, a visitor with security level 1 will not be able to access information with a security level higher than 1. So the search engine would only return information for level 1 and obscure the higher level information and rank it by relevance. This need effectively protects the patient's personally identifiable information as well as other important information(such as Protected health information) from being easily compromised.  

Elastic Search provides permission and access control feature which just satisfy the reqiurement described above. Elasticsearch restricts search behavior by predetermining access attributes (including levels) for files, ensuring that files and information are presented only to visitors of the appropriate level.

### 1.1 System Engineering View (Mustapha) 
  ![System Engineering View](images/SystemEngineeringView.jpg "System Engineering View") 
    
### 1.2 Perceived Threats (Mustapha)
- Malicious patients data breach from Elasticsearch data clusters
- Unauthorized access to patient PII/PHI data from health care personnel
- Elasticsearch resiliency issues
- Data loss due to system failure, disaster or intentional/unintential human errors
- Elasticsearch security is not enabled by default. If an Elasticsearch installation left without security set then it can be exploited to access data 
  
### 1.3 List of Security Features (Zijun)
- Permisson and access control
    - Ensure proper visibility results and information based on pre-determined access attributes known as document-level permissions
- Preventing unauthorized access
    - Provide a standalone authentication mechanism that enables users to quickly password-protect their cluster.
- Preserving the integrity of user's data
    - TLS is used to protect the integrity of users' data from tampering and also to provide confidentiality by encrypting communications within the cluster.
- Maintaining an audit trail
    - A feature is provided to log access information and design audit levels to help monitor system conditions as well as help diagnose operational problems.

  

## 2. Motivation of the Project (Zijun)
The process of selecting open source projects is tough. During the planning phase of the project, our team agreed after discussion that we needed to ensure that our project was acceptable to each of us. That is, the selected project should ideally meet our technical preferences, interest preferences, and knowledge background. The reality was that after careful research we found that it is difficult to find a project which could meet the preferences of all members. Therefore, after the first meeting we decided that everyone should look for two or three projects that they were interested in and vote on the final project at the second meeting.

At the second meeting we proposed 8 projects, and after discussion we eliminated 5 of them. We finally decided to ask for Dr. Gandhi's advice with these three projects.From his advice we understood that we had to find a complete software and that could give the user certain security features. In the end we chose Elastic Search as our open source software. First of all, this open source software has a wide variety of application scenarios as a search engine. Correspondingly, it also has a very developed open source community. The developer documentation is also complete, and the security features are clear. Although we are not familiar with this software, we can easily find information about this software on the web (tutorials, technical principles, architecture, etc.). Elasticsearch as the  topic of our analysis, its main programming language is java (99%). All of our team has some experience in java programming, and this will help us to start the rest of the project. Our goal is to analyze the security features of Elasticsearch and to analyze and test it for possible vulnerabilities(Such as NoSQL injection,etc).

## 3. Description of the Open-Source Project (Jeremy)

Elasticsearch is an open-source software that is primarily used in data analytics as a powerful and highspeed search engine. This application is used to search enterprise databases with large quantities of data and then organize the output in a usable form of information to assist the user in a decision-making process. Elasticsearch is comprised of three main components, which are commonly referred to as the Elastic Stack. The stack consists of Elasticsearch, Logstash, and Kibana. At its core, Elasticsearch is a search engine and highly usable database. Kibana has enhanced their core offerings by adding the visualization aspect to the data retrieval. By adding a dashboard tool like Kibana for visual management of data, Elasticsearch has strengthened its marketability. The distributed architecture coupled with a clustering approach allows the software to be a highly scalable solution.  

Elasticsearch is developed in Java and built on Apache Lucene. Apache Lucene is also an open-source Java based software that is supported by the Apache Software Foundation. The documentation source of Elasticsearch is generally saved on the original JSON document. Elasticsearch is innovative from the standpoint of compatibility and scalability.  

Elasticsearch has over 63,000 commits executed with over 1,800 participants during its lifespan. Elasticsearch has over 2,000,000 lines of code. Most of its development happened in the few months after it launched in 2018 and has been consistently updated to the present day. Elasticsearch is used by large and small companies. Some of the most popular companies that utilize this software include but are not limited to Walmart, Netflix, Uber, Microsoft, and Ebay. This has made Elasticsearch one of the most popular open-source tools available for searching and managing bigdata. Elasticsearch can be deployed with Amazon Web services, Google Cloud Platform, and Microsoft Azure. Because they’re compatible with major entities in the cloud, database management and security space their exposure as one of the best tools at the enterprise level has gradually increased.     

## 4. Licensing Information of Elastic Search (Jeremy)

Elasticsearch has updated their licensing options to provide users with a dual-licensing agreement. This license will work with users making updates to Elasticsearch and Kibana. “We are moving our Apache 2.0-licensed source code in Elasticsearch and Cabana to be dual licensed under the Elastic License and Server Side Public License (SSPL), giving users the choice of which license to apply” (FAQ License Change, elastic.co).  
Elasticsearch has a detailed process to allow people the opportunity to make contributions to the community. The Elastic Contributor Program was created in 2020. An individual would need to create an account with their personal information. This is important so that their contributions can be recognized as well as providing a level of accountability for what's published under a specific user account. Once the profile is created for a user, they will be able to make contributions as well as validate the contributions of other users. 
Elasticsearch requires contributors to sign a contributor license agreement (CLA). This agreement between the user and Elasticsearch was built in order to assign credit for the contribution as well as designation ownership of the intellectual property that was created. Elasticsearch does not take ownership of contributed code, but it will provide the user with the option to designation themselves or their company as the intellectual property owner. 
 
FAQ on 2021 License Change. https://www.elastic.co/pricing/faq/licensing 
 
    
## 5. Contribution and Agreements (Charlie)

The Elasticsearch community is a very open and welcoming space that provides individuals with many different options for contributing to the project. They accept a variety of different contribution types including documentation, bug reports, feature requests, and code changes. They are also beginner friendly and include the labels `help wanted` in open issues where the community is looking for contributions, and `good first issue` in issues that they think would be great for new contributors. They even have developed the [Elastic Contributor Program](https://www.elastic.co/community/contributor) that rewards people for making contributions to the project, and gives the contributors a change to win prizes. However, because this is such a large project there are many guidelines on how project contributions should be made.

### Documents:
[Github CONTRIBUTING file](https://github.com/elastic/elasticsearch/blob/main/CONTRIBUTING.md)<br/>
[Elastic Contributor Agreement](https://www.elastic.co/contributor-agreement/)<br/>
[Elastic Contributor Program](https://www.elastic.co/community/contributor)

### Guidelines:
-	Bug reporting: Before opening a bug, it should be tested with the newest version of Elasticsearch to verify it hasn’t already been addressed. If the bug is still prevalent search for similar issues already opened. Then, open an issue with a test case to confirm the bug’s existence with as much information provided as possible.
-	Feature requests: Search for any related feature requests and open an issue describing the requested feature and why it should be added.
-	Contributing: Before contributing to any open issues the contributor should discuss their ideas in the issue first, to cut back on overlapping work or confusion.
-	Java formatting: Java files should adhere to the Java Language Formatting Guidelines provided.
-	Javadoc: All code should have Javadoc that explains why the code was used and adds value to the documentation.
-	License headers: All contributed Java files are required to have the provided license header, files covered by the Elastic license should be in the x-pack directory and should use the alternative license header.
-	Testing: Before changes are submitted it is required to test the changes by running the test suite `./gradlew check`.
-	Class contributions: Contributions as part of a class are accepted but contributing should not be mandatory as part of a class. This could cause an influx of activity and PR’s will not be rushed for class deadlines.
-	Contributor Agreement: All contributors must sign a [Contributor License Agreement](https://www.elastic.co/contributor-agreement/) so Elasticsearch can distribute the contributions without restriction. Contributors still retain copyright ownership, but permission cannot be withdrawn. Contributors can either sign an Individual CLA or a Corporate CLA depending on within what scope the contribution was originally developed.


## 6. Security Related History of Elastic Search (Charlie)
Throughout Elasticsearch’s lifetime there have been many security changes implemented. The [release notes]( https://www.elastic.co/guide/en/elasticsearch/reference/current/es-release-notes.html) of each Elasticsearch version detail any changes to authentication, authorization, and security that take place. Recently, one of the most notable changes has been automatically enabling and configuring security when the software is first run, which was rolled out in Elasticsearch 8.0. Whereas in previous versions security was disabled by default and had to be enabled manually. The current version of Elasticsearch is 8.4.1 and although it may not have implemented any security changes, 8.4.0 addressed several.

Some of these security changes are instigated by vulnerabilities discovered that require remediation. Elastic maintains a list of these [security issues]( https://www.elastic.co/community/security) on their website. These issues not only include Elasticsearch vulnerabilities, but they also include vulnerabilities from other applications that could affect Elasticsearch. Most recently listed was a vulnerability discovered with Java JDK 15. Elastic also informs users of these issues through announcements. These announcements include the security issue’s summary and steps for remediation, which are posted in a [Security Announcements](https://discuss.elastic.co/c/announcements/security-announcements/31) forum.

Elastic also has a [HackerOne bug bounty program](https://hackerone.com/elastic?type=team) which rewards user’s for discovering and reporting vulnerabilities that affect one of Elastic’s products. Or alternatively security issues can be reported to security@elastic.co.

### Vulnerabilities:
Elasticsearch has had 23 [vulnerabilities](https://www.cvedetails.com/vulnerability-list/vendor_id-16559/product_id-51138/Elastic-Elasticsearch.html) discovered since it was first launched in 2010. The first vulnerability was discovered in 2015 involving remote code execution. Since then, there have been a steady stream of vulnerabilities discovered every year. Currently there have been 2 vulnerabilities this year.  

| CVE ID         | Publish Date | Score | Summary | Remediation                    |
|:--------------:|:------------:|:-----:| ------- | ------------------------------ |
| CVE-2022-23712 | 2022-06-06   | 5.0   | A Denial of Service flaw was discovered in Elasticsearch. Using this vulnerability, an unauthenticated attacker could forcibly shut down an Elasticsearch node with a specifically formatted network request. | The issue is resolved in 8.2.1 |
| CVE-2022-23708 | 2022-03-03   | 4.0   | A flaw was discovered in Elasticsearch 7.17.0’s upgrade assistant, in which upgrading from version 6.x to 7.x would disable the in-built protections on the security index, allowing authenticated users with * index permissions access to this index. | Elasticsearch 8.2.1 and 7.17.4 are packaged with OpenJDK 18.0.1 which resolves this issue. See for details. |

  
## 7. Reflection (Mustapha)
At the beginning, communication between team members was not as expected.
But since then the team started using Discord mobile app for our team's group chat to get the attention of everyone.
We had in-person meeting one time and zoom meetings to talk about the project proposal and split up the work.
Unfortunately, one team member had to drop from the class and we had to reassign some of his tasks between the other team members.<br/>

The team had few options on the open-source software to use for this class project.
We ended up selecting Elasticsearch for the reaons mentioned before.  

The team starts to be more organized in splitting up the tasks using Github project board.
We are also using the feature branch workflow in our repository to collaborate on source files check in.
Next project assignments will have more team collaboration and everything will run smoothly.
  
    
