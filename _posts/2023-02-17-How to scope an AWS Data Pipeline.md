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

- OLAP or OLTP? 

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

# What is a data lake?

A data lake is a cloud infrastructure that rapidly collects and stores massive amounts of any and all types of data with their original attributes. Just like a lake collects water from several sources in their different natural states, a data lake holds unstructured and structured data from different sources until your business needs them.

# How do data lakes work?

New data enters the data lake through the data ingestion tier. Upon ingestion, the data lake breaks the new data into segments and organizes these segments in metadata catalogs. These catalogs specify the source, date of acquisition, and other attributes of every piece of data.

The data lake’s architecture follows strict data governance to maintain data quality. Good data governance – like pruning outdated data and assigning roles and permissions for controlled access – keeps the data lake organized, regardless of the amount of data in it. Without these, the data lake will become a data swamp. This means that all the data will be mixed up and disorganized, making it nearly impossible for anyone to find, trust, and use it.

# What is a data warehouse?

A data warehouse is a central repository of structured data intended for analytical purposes. Structured data has been processed and organized to allow humans and computer programs to access and interpret it seamlessly.

For example, let’s say a company gathered data about its customers. In a data warehouse, this information would be organized according to demographics, like age or geographical location. So anyone could access the data warehouse and look at information for customers based on any of these parameters.

# How do data warehouses work?

A data warehouse architecture has three core components:

1. A **relational database system** with multiple tables. Each table is like a grid of boxes – each row is a complete entry, and the columns are chunks of similar data like names or addresses.

2. **Online Analytical Processing (OLAP)** servers that map and act on multidimensional data processes. In other words, these servers allow you to extract and query data across multiple tables in your database concurrently.

3. The **front-end client interface** that displays meaningful insights derived from the data.

When the data warehouse ingests structured data, it stores the data in tables defined by a schema. Think of a schema as a logical description of what each table contains and how it relates to other tables in the data warehouse. Query tools use the schema to determine which data tables to access and analyze.

# The major differences between data lakes and data warehouses

![](/assets/data_l_data_w.png)



---