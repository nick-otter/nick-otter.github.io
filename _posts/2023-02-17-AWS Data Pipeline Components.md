---
layout: post
title:  "AWS Data Pipeline Components"
categories: aws_datapipelines
---

# AWS Data Pipeline Components
{: style="text-align: center"}

| Category | Use cases | Service | Descri­ption |
| --- | --- | --- | --- |
| **Analytics** | Intera­ctive analytics | Athena | Athena is an intera­ctive query service that makes it easy to analyze data in S3 using standard SQL. 
| | Big data processing | EMR | EMR is the indust­ry-­leading cloud big data platform for processing vast amounts of data using open source tools such as Apache Spark, Hive, HBase,­Flink, Hudi, & Presto. EMR
| | Data wareho­using | Redshift | The most popular & fastest cloud data warehouse. |
| |	Real-time analytics |Kinesis | Kinesis makes it easy to collect, process, & analyze real-time, streaming data so one can get timely insights. |
| | Operat­ional analytics | Elasti­csearch Service | Elasti­csearch Service is a fully managed service that makes it easy to deploy, secure, & run Elasti­csearch cost effect­ively at scale.|
| | Dashboards & visual­iza­tions |Quicksight | QuickSight is a fast, cloud-­powered business intell­igence service that makes it easy to deliver insights to everyone in organi­zation. |
| **Data movement** | Real-time data movement | 1) Amazon Managed Streaming for Apache Kafka (MSK) 2) Kinesis Data Streams 3) Kinesis Data Firehose 4) Kinesis Data Analytics 5) Kinesis Video Streams 6) Glue | MSK is a fully managed service that makes it easy to build & run applic­ations that use Apache Kafka to process streaming data. | 
| **Data lake** | Object storage | 1) S3 2) Lake Formation | Lake Formation is a service that makes it easy to set up a secure data lake in days. A data lake is a centra­lized, curated, & secured repository that stores all data, both in its original form & prepared for analysis. S3 LakeFo­rmation |	
| | Backup & archive | 1) S3 Glacier 2) Backup | S3 Glacier & S3 Glacier Deep Archive are a secure, durable, & extremely low-cost S3 cloud storage classes for data archiving & long-term backup.|
| | Data catalog | 1) Glue 2)) Lake Formation |
| | Third-­party data | Data Exchange | Data Exchange makes it easy to find, subscribe to, & use third-­party data in the cloud. |
| **Predictive analytics && machine learning** | Frameworks & interfaces | Deep Learning AMIs | Deep Learning AMIs provide machine learning practi­tioners & resear­chers with the infras­tru­cture & tools to accelerate deep learning in the cloud, at any scale.| 
| | Platform services |SageMaker | SageMaker is a fully managed service that provides every developer & data scientist with the ability to build, train, & deploy machine learning (ML) models quickly.|


# Data Lake on AWS

The code configures a suite of AWS Lambda microservices (functions), Amazon OpenSearch Service for robust search capabilities, Amazon Cognito for user authentication, AWS Glue for data transformation, and Amazon Athena for analysis.

Data Lake on AWS leverages the security, durability, and scalability of Amazon S3 to manage a persistent catalog of organizational datasets, and Amazon DynamoDB to manage corresponding metadata. Once a dataset is cataloged, its attributes and descriptive tags are available to search on. Users can search and browse available datasets in the console, and create a list of data they require access to. It keeps track of the datasets a user selects and generates a manifest file with secure access links to the desired content when the user checks out.

![](/assets/data_l_on_aws.png)

# Lake Formation on AWS

1. **User personas**: As first step, the DMO team identified three user personas for the Audit Surveillance system on AWS.
- **Data service compliance users** who would like to consume audit surveillance data from the data lake into their respective applications through an enterprise data service.
- **Business users** who would like to create business intelligence dashboards using a BI tool to audit data for compliance needs.
- **Compliance IT users** who would like to perform ad-hoc queries on the data lake to perform analytics using an interactive query tool.
2. **Data ingestion**: Data is ingested into Amazon Simple Storage Service (S3) from different on-premises data sources by using AWS Lake Formation blueprints. AWS Lake Formation provides workflows that define the data source and schedule to import data into the data lake. It is a container for AWS Glue crawlers, jobs, and triggers that are used to orchestrate the process to load and update the data lake.
3. **Data storage**: Parametric used Amazon S3 as a data storage to build an Audit Surveillance data lake, as it has unmatched 11 nines of durability and 99.99% availability. The existing Hadoop storage was replaced with Amazon S3. The DMO team created a drop zone (raw), an analytics zone (transformed), and curated (enriched) storage layers for their data lake on AWS.
4. **Data cataloging**: AWS Glue Data Catalog was the central catalog used to store and manage metadata for all datasets hosted in the Audit Surveillance data lake. The existing Hadoop metadata store was replaced with AWS Glue Data Catalog. AWS services such as AWS Glue, Amazon EMR, and Amazon Athena, natively integrate with AWS Glue Data Catalog.
5. **Data processing**: Amazon EMR and AWS Glue process the raw data and places it into analytics zones (transformed) and curated zones (enriched) S3 buckets. Amazon EMR was used for big data processing and AWS Glue for standard ETL processes. AWS Lambda and AWS Step Functions were used to initiate monitoring and ETL processes.
6. **Data consumption**: After Audit Surveillance data was transformed and enriched, the data was consumed by various personas within the firm as follows:
- AWS Lambda and Amazon API Gateway were used to support consumption for data service compliance users.
- Amazon QuickSight was used to create business intelligence dashboards for compliance business users.
- Amazon Athena was used to query transformed and enriched data for compliance IT users.
7. **Security**: AWS Key Management Service (KMS) customer managed keys were used for encryption at rest, and TLS for encryption at transition. Access to the encryption keys is controlled using AWS Identity and Access Management (IAM) and is monitored through detailed audit trails in AWS CloudTrail. Amazon CloudWatch was used for monitoring, and thresholds were created to determine when to send alerts.
8. **Governance**: AWS IAM roles were attached to compliance users that permitted the administrator to grant access. This was only given to approved users or programs that went through authentication and authorization through AWS SSO. Access is logged and permissions can be granted or denied by the administrator. AWS Lake Formation is used for fine-grained access controls to grant/revoke permissions at the database, table, or column-level access.

![](/assets/data_lake_formation.png)
--