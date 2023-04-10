---
layout: post
title:  "AWS 2020 for DevOps"
categories: aws
---

# Distributed Load Testing, Copilot, AppStream 2.0; A Brief Overview Of AWS 2020 For DevOps
{: style="text-align: center"}

Written by Nick Otter. 

# Contents

- [**Overview of AWS 2020 for DevOps**](#overview-ofaws-2020-for-devops)<br>
     - [And the rest...](#and-the-rest...)<br>
- [**Number of updates per AWS product in 2020 list**](#number-of-updates-per-aws-product-in-2020-list)<br>

# Overview of AWS 2020 for DevOps

Let's take a look over 2020 in AWS. Some great new products arrived for your estate.

!NEW **Distributed Load Testing** | [Distributed Load Testing on AWS](https://aws.amazon.com/solutions/implementations/distributed-load-testing-on-aws/)
* This is an **AWS Solutions Implementation**. A **CloudFormation** template that deploys a distributed load tester API, which uses the **Amazon API Gateway** to invoke microservices (**AWS Lambda** functions). Cost is tied to the Lambda requests.

!NEW **AWS Copilot** | [Get started with AWS Copilot by deploying an Amazon ECS application](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/getting-started-aws-copilot-cli.html)<br>
* **AWS Copilot** is a **CLI** for container management and deployment for ECS. This might be helpful for spiking. A quick look at the flow, it won't be great as an **IaC** solution for a microservice build step.

!NEW **AppStream 2.0** | [Amazon AppStream 2.0](https://aws.amazon.com/appstream2/?blog-posts-cards.sort-by=item.additionalFields.createdDate&blog-posts-cards.sort-order=desc), [Cloud Application Streaming whitepaper](https://d1.awsstatic.com/product-marketing/AppStream2.0/Accelerate%20Cloud%20Adoption%20With%20Application%20Streaming.pdf)<br>
* This isn't new to 2020, but it was one of the most updated products in 2020 AWS, with 10 new patches. A fully managed non-persistent application and desktop streaming service. There's a nice comparison to Citrix workspace [here](https://www.cloudhesive.com/blog-posts/comparing-appstream-2-0-and-citrix-for-workplace-app-streaming/) and here's a helpful [reddit thread](https://www.reddit.com/r/aws/comments/8wxnsx/is_anyone_actually_using_appstream/), 

!NEW **CloudWatch Lambda Insights** | [Using Lambda Insights in Amazon CloudWatch](https://docs.aws.amazon.com/lambda/latest/dg/monitoring-insights.html)<br>
* A monitoring and troubleshooting solution for serverless applications running on AWS Lambda. Hooray! System-level metrics: CPU time, memory, disk/network usage, cold starts and Lambda worker shutdowns. Great.

!NEW **SQL Server Reporting Services** | [Configuring Microsoft SQL Server Reporting Services on Amazon RDS for SQL Server](https://aws.amazon.com/blogs/database/configuring-microsoft-sql-server-reporting-services-on-amazon-rds-for-sql-server/)<br>
* **Amazon RDS for SQL Server** now supports **SSRS**, giving you the ability to host the report server web portal on the same Amazon RDS DB instance as your SQL Server database. 

Other interesting products that might be relevant:

* New **AWS Public Datasets** were made available from [Ford, NASA and NREL](https://aws.amazon.com/about-aws/whats-new/2020/01/new-aws-public-datasets-available-from-ford-nasa-nrel/). 

* Some new articles were added to the great **Amazon Builders' Library** that you can visit [here](https://aws.amazon.com/builders-library/?cards-body.sort-by=item.additionalFields.customSort&cards-body.sort-order=asc) 

* New [Training and Certifcation](https://aws.amazon.com/training/) courses were added.

# And the rest...

As a bit of an overview let's take a look at the other developments. In the [AWS product log](https://aws.amazon.com/about-aws/whats-new/2020/) for 2020, the list has over 2000 entries. Let's take a look at how many updates there were per product to get an overview of all products that were updated.

This should do it.

```bash
$ lynx -dump -nolist https://aws.amazon.com/about-aws/whats-new/2020/ | 
grep -B 2 "Posted On" |
tr -d "[:space:]" | 
grep -wo '[[:alnum:]]\+' | 
sort | 
uniq -cd | 
sort -k1,1n
```

This is a pretty rudimentary search. It isolates the product names in the headers and the dates of all entries on the [AWS product log](https://aws.amazon.com/about-aws/whats-new/2020/) page for 2020.

**`The results`**

The three most updated AWS products in 2020 were: 

* **AWS Solutions Consulting Offers** (mentioned `23` times on the [AWS product log](https://aws.amazon.com/about-aws/whats-new/2020/) page)
* **Amazon Document DB (with MongoDB compatibility)** (`13`)
* **Amazon App Stream 2** (`10`)

The number of mentions here can be synonymous with product updates. So there were `23` updates to the **AWS Solutions Consulting Offers** product in 2020 and `13` for the **Amazon DocumentDB (with MongoDB compatibility)**.

The most mentioned regions were **US** (`93`), **Milan** (`59`), **CapeTown** (`47`). With the most product updates on **Dec 1** (`47`), **Dec 15** (`32`), **Nov 24** (`45`). Can you feel that end of year squeeze.

# Number of updates per product in 2020 list

Here's the list. For anything else, just drop me a message.

## Compute
* **EC2** (`3`) Amazon Elastic Compute Cloud (Amazon EC2) is a web service that provides secure, resizable compute capacity in the cloud.
* **EC2 C5** (`2`) Designed for compute-intensive workloads with cost-effective high performance. For running advanced compute-intensive workloads.
* **AMIs** (`3`) An Amazon Machine Image (AMI) provides the information required to launch an instance.
* **M5** (`2`) RDS instance type.
* **M5d** (`2`) RDS instance type. Large memory capacity.
* **NETCore3** (`2`) You can now develop AWS Lambda functions using .NET Core 3.1.
* **Graviton2 processors** (`2`) AWS Graviton processors are custom built by Amazon Web Services using 64-bit Arm Neoverse cores to deliver the best price performance for your cloud workloads.
* **r5** (`2`) Memory optimized instance type to process large data sets.
* **R5n** (`2`) Memory optimized instance type to process large data sets.

## Storage
* **FSx** (`2`) Fully managed third-party file systems optimized for a variety of workloads.
* **EBS** (`2`)
Amazon Elastic Block Store is easy to use, high performance block storage at any scale.
* **EBS crash** (`2`)
Amazon Elastic Block Store feature. Take point-in-time, crash-consistent snapshot across multiple EBS volumes.
* **EBS Fast Snapshot Restore (FSR)** (`2`)
Amazon Elastic Block Store feature. Fast Snapshot Restore can be enabled on a snapshot even while the snapshot is being created. If you create nightly backup snapshots, enabling them for FSR will allow you to do fast restores the following day regardless of the size of the volume or the snapshot
* **Storage Gateway** (`2`) Service connecting an on-prem software appliance with cloud-based storage to provide seamless and secure integration between an organization's on-prem IT environment and AWS's storage infrastructure. 
* **Elastic File System** (`7`) Managed file storage for EC2.
* **AWS Backup** (`2`) Define Backup plans, schedule backups, automate backup retention management, centrally monitor backup activity, and restore backups across AWS services.

## Database
* **RDS for Oracle** (`4`) Set up, operate, and scale an Oracle database in the cloud.
* **RDS for PostgreSQL** (`2`) Set up, operate, and scale a relational database (PSQL) in the cloud.
* **RDS for MySQL** (`4`) Set up, operate, and scale a relational database (MySQL) in the cloud.
* **RDS Performance Insights Supports SQL** (`3`) Analyze and tune Amazon RDS database performance.
* **SSRS** (`2`) Amazon RDS for SQL Server supports SQL Server Reporting Services (SSRS), giving you the ability to host the report server web portal on the same Amazon RDS DB instance as your SQL Server database. 
* **PSU** (`4`) Amazon RDS for Oracle now supports the January 2020 Patch Set Updates (PSU) for Oracle Database 11.2 and 12.1. Oracle PSUs contain bug fixes and other critical security updates. 
* **DocumentDB (with MongoDB compatibility)** (`13`) Fully managed NoSQL document database service that supports MongoDB workloads.
* **DynamoDB** (`2`) Key-value NoSQL and document database service.
* **DAX** (`2`)
Amazon DynamoDB Accelerator (DAX) is a fully managed, highly available, in-memory cache for Amazon DynamoDB.
* **Amazon Managed Cassandra Service** (`3`) 
Amazon Managed Apache Cassandra Service (MCS), a scalable, highly available, and managed Apache Cassandra-compatible database service. Amazon MCS is serverless.
* **Amazon Aurora** (`3`) MySQL and PostgreSQL-compatible relational database built for the cloud.

## Migration & Transfer
* **AWS Database Migration Service** (`2`) Homogenous or heterogenous migrations. 
* **AWS Transfer Family** (`2`) Simple and seamless file transfer to Amazon S3 and Amazon EFS using SFTP, FTPS, and FTP.

## Networking & Content Delivery
* **VPC** (`9`)
Isolated cloud resources.
* **API Gateway** (`2`)
Build, Deploy and manage APIs.
* **AWS App Mesh** (`2`)
Easily monitor and control microservices.
* **AWS Transit Gateway** (`2`)
AWS Transit Gateway connects VPCs and on-premises networks through a central hub. It acts as a cloud router – each new connection is only made once and puts an end to complex peering relationships.
* **Distributed Load Testing v1** (`2`) New CloudFormation template for 2020 to use as a load testing tool.

## Customer Enablement 
* **AWS Managed Services** (`5`) AWS Managed Services automates common activities, such as change requests, monitoring, patch management, security, and backup services, and provides full-lifecycle services to provision, run, and support your infrastructure.

## Blockchain
* **Amazon Managed Blockchain** (`5`) 
Easily create and manage scalable blockchain networks.

## Management & Governance
* **CloudWatch Logs Insights** (`2`) Interactively search and analyze your log data in Amazon CloudWatch Logs.  
* **CloudWatch Lambda Insights** (`2`) Amazon CloudWatch Lambda Insights collects and aggregates Lambda function runtime performance metrics and logs for your serverless applications. 
* **AWS Well-Architected Tool** (`4`) AWS Well-Architected helps cloud architects build secure, high-performing, resilient, and efficient infrastructure for their applications and workloads. 
* **AWS Systems Manager Explorer** (`4`) Customizable operations dashboard that reports information about your AWS resources.

## Machine Learning 
* **AmazonText** (`2`)
Easily extract data from virtually any document.
* **PyTorch1** (`3`)
PyTorch is an open source deep learning framework that makes it easy to develop machine learning models and deploy them to production.
* **AWS ParallelCluster 2** (`4`) AWS ParallelCluster is an AWS-supported open source cluster management tool that makes it easy for you to deploy and manage High Performance Computing (HPC) clusters on AWS.

## Analytics
* **Amazon Redshift** (`2`)
Data warehouse tool.
* **Elasticsearch Service** (`2`)
Run and scale Elasticsearch clusters for data management and analysis such as log traces.
* **Amazon MSK adds support for Apache Kafka version2** (`3`) Amazon MSK is a fully managed service that makes it easy for you to build and run applications that use Apache Kafka to process streaming data. Apache Kafka is an open-source platform for building real-time streaming data pipelines and applications. With Amazon MSK, you can use native Apache Kafka APIs to populate data lakes, stream changes to and from databases, and power machine learning and analytics applications.
* **AWS Lake Formation** (`3`) AWS Lake Formation is a service that makes it easy to set up a secure data lake in days. A data lake is a centralized, curated, and secured repository that stores all your data, both in its original form and prepared for analysis. 

## Security, Identity & Compliance
* **KMS** (`2`) 
Securely generate and manage AWS encryption keys.
* **AWS Single Sign-On** (`7`) Centrally manage single sign-on (SSO) access to multiple AWS accounts and business applications.
* **AWS Identity and Access Management (IAM)** (`2`) Securely manage access to AWS services and resources.
* **SSE** (`2`) Server-Side Encryption. Can be used for AWS KMS ro protect S3 buckets.
* **ABAC** (`4`) Attribute-based access control (ABAC) is an authorization strategy that defines permissions based on attributes. In AWS, these attributes are called tags. You can attach tags to IAM resources, including IAM entities (users or roles) and to AWS resources.
* **Role-Based Access Control** (`4`) Amazon Cognito identity pools assign your authenticated users a set of temporary, limited privilege credentials to access your AWS resources. The permissions for each user are controlled through IAM roles that you create.

## Application Integration
* **Amazon MQ** (`4`)
Managed message broker service for Apache ActiveMQ and RabbitMQ that makes it easy to set up and operate message brokers in the cloud, so you can migrate your messaging and applications without rewriting code. 
* **SQS** (`2`)
Simple Queue Service. Fully managed message queues for microservices, distributed systems, and serverless applications.
* **List Dead Letter Source Queues** (`2`) API call for Amazon Simple Queue Service Dead-Letter Queues.  
* **List Queues** (`2`) API call for Amazon Simple Queue Service.

## End User Computing
* **App Stream 2** (`10`)
Centrally manage your desktop applications on AppStream 2.0 and securely deliver them to any computer.

## Internet of Things
* **IoT Greengrass** (`2`) Open source edge runtime and cloud service that helps you build, deploy, and manage device software.
* **AWS IoT Device Tester v3** (`2`) AWS IoT Device Tester validates if the CPU architecture, Linux kernel configuration, and drivers for your device work with AWS IoT Greengrass.
 * **FreeRTOS** (`2`) An open source, real-time operating system for microcontrollers that makes small, low-power edge devices easy to program, deploy, secure, connect, and manage. 
 * **OTA** (`4`) Over-the-air (OTA) updates allow you to deploy firmware updates to one or more devices in your fleet.

## Containers
* **ECS** (`2`)
Elastic Container Service. Highly secure, reliable and scalable way to run containers.
* **EKS** (`4`)
Elastic Kubernetes Service. Fully managed Kubernetes control plane on AWS without needing to install and operate your own Kubernetes control plane.
* **AWS Copilot CLI launches v0** (`3`) The first official command line tool for Amazon Elastic Container Service (Amazon ECS).
        
## Other (currently not listed in AWS Services dashboard)
* **AWS Gov Cloud** (`2`) Amazon's Regions designed to host sensitive data, regulated workloads, and address the most stringent U.S. government security and compliance requirements.
* **Amazon Lumberyard Beta1** (`2`) 
Game engine. Not present as a service in the dashboard yet.
* **Corretto** (`2`) 
Amazon Corretto is a no-cost, multiplatform, production-ready distribution of the Open Java Development Kit (OpenJDK).
* **Service Quota** (`2`) 
Service Quotas is an AWS service that helps you manage your quotas for over 100 AWS services, from one location. Along with looking up the quota values, you can also request a quota increase from the Service Quotas console.
* **AWSConfig** (`3`) AWS Config is a service that enables you to assess, audit, and evaluate the configurations of your AWS resources. Config continuously monitors and records your AWS resource configurations and allows you to automate the evaluation of recorded configurations against desired configurations. 
* **New AWS Public Datasets Available from Ford** (`2`) Ford Multi-AV Seasonal Dataset from the Ford Motor Company is now available.

---

Thanks. This article was written by Nick Otter.
