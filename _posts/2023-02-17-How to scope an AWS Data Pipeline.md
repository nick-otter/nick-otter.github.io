---
layout: post
title:  "How to scope an AWS Data Pipeline"
categories: aws_datapipelines
---

# How to scope an AWS Data Pipeline
{: style="text-align: center"}

# Investigative Questions


- What type of data do you want to store?

| Structured | Unstructured |
| --- | --- |
| Relational | Non-Relational |
| Schema stays the same | Schema changes often |

- How much data do you want to store?

| Snowflake | Redshift |
| --- | --- |
| Scale up scale down | Manually add nodes |

- How quickly do you need to access the data?

| Real-time analytics | 

- How much are you willing to spend?

- How connected your warehouse is to other critical tools and services?


# AWS Data Warehouse comparisons

**Redshift Cons**
- It's unsuitable for transactional systems due to the need to use two different database services (e.g., RDS/Aurora + Redshift).
- When waiting for the latest patch from Amazon Web Services, sometimes it's necessary to roll back your Redshift version.
- The Amazon Redshift Spectrum service charges extra based on the number of scanned bytes.
- Redshift does not support many common PostgreSQL data types.
- There can be problems with hanging queries in external tables.
- You will also need to rely on other means to ensure your data isn't compromised.
- The system does not enforce uniqueness, so you'll need to use another process for data deduplication.

**Snowflake Cons**
* Snowflake may not be the right fit if you're running an on-premises business that doesn't easily integrate with cloud-based services.
* You'll use a minute's worth of Snowflake credits whenever you start a virtual warehouse and then charge every second


| When to use Redshift | When to use Snowflake |
| --- | --- |
| AWS Redshift is best suited when your organization is already using services from this company, and there are heavy query loads on applications that need analytics and structured information in real time. | Snowflake is the best option for organizations with lighter query loads, which need frequent scaling. It's also built on automation without operational overhead. |

# AWS Data Lake comparisons

# Geneal comparisons

| Data Lake | Data Warehouse |
| --- | --- |
| Large volume of data in multiple formats | Visualize data and extract insights |
| Store IoT data for real-time analysis | Decision making not just collecting data for analysis |
| Raw unstructured data to generate output, e.g. machine learning | Original data source is not suitable for querying, and you need to separate analytical data from your transactional data |

![](/assets/db_olap_oltp.png)

![](/assets/db_dw.png)


# Data lakes and data warehouses comparisons

Data warehouse:
- A **relational database system** with multiple tables
- **Online Analytical Processing (OLAP)**
- **front-end client interface** 

![](/assets/data_l_data_w.png)


---