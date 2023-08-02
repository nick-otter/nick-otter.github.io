---
layout: post
title:  "Interview Questions"
categories: misc
---

# Interview Questions
{: style="text-align: center"}



Background For Candidate (For “Any Questions” at the end of the interview)  

First round, but could be final round or technical test 



0 Intro Yourself Time at / position at

0 Intro Interview a mix of technical / scenario / teamfit questions, won’t take 60mins 

0 Do you have any questions before we start? 

 

1. Could you walk me through your experience so far? 

Significant initiative on a major project that you led or were a key contributor on?  

What did you do to accelerate delivery?  

How did you approach making key decisions and navigating trade offs?  

What data did you use to make decisions? 

Tell us about a time you failed or had to correct course on a project l. Lessons you learned, ideas for improvement? 

Have you ever lead a team before? Managed a project?  

Would you be happy to stay up to date with certifications for this roles? 

 

2. DevOps 

What is DevOps and why is it valuable to an organisation? 

Abstract a technical solution from key business milestones 

How can you measure DevOps? 

Dora metrics DORA metrics: Deployment frequency, Change lead time, Change failure rate, Time to restore service 

Can you give a real world example of where you have delivered a good DevOps solution? 

What was the cost of that solution?  

Incl. team size, infra cost, time spent 

Monorepo vs. multi repo? 

What is your opinion on AI Ops? 

 

3. What is your experience with microservices? 

What is microservice architecture vs. monolith? 

A monolithic application is built as a single unified unit while a microservices architecture is a collection of smaller, independently deployable services 

What are some weaknesses or challenges of Microservice architecture? 

Development sprawl – Microservices add more complexity compared to a monolith architecture, since there are more services in more places created by multiple teams. If development sprawl isn’t properly managed, it results in slower development speed and poor operational performance.  

Exponential infrastructure costs – Each new microservice can have its own cost for test suite, deployment playbooks, hosting infrastructure, monitoring tools, and more. 

Added organizational overhead – Teams need to add another level of communication and collaboration to coordinate updates and interfaces.  

Debugging challenges – Each microservice has its own set of logs, which makes debugging more complicated. Plus, a single business process can run across multiple machines, further complicating debugging.  

Lack of standardization – Without a common platform, there can be a proliferation of languages, logging standards, and monitoring.  

Lack of clear ownership – As more services are introduced, so are the number of teams running those services. Over time it becomes difficult to know the available services a team can leverage and who to contact for support. 

 

What is a docker container?  

Encapsulated process, Hypervisor partitioned vm  

What is a Container? | Docker 

 

 

What are some of the weaknesses of Docker?  

Managing storage 

Reliance on baked images, scratch image 

 

What is a scratch image in Docker? 

The scratch image is the smallest possible image for docker. Actually, by itself it is empty (in that it doesn't contain any folders or files) and is the starting point for building out images. In order to run binary files on a scratch image, your executables need to be statically compiled and self-contained – go, or bash  

Can you name a programming language that doesn’t not require any binaries? 

What is a .dockerignore file? 

a configuration file that describes files and directories that you want to exclude when building a Docker image 

 

How could you reduce the size of a Docker image? 

Leverage Multi-stage builds Multi-stage builds separate the build environment from the final runtime environment. They allow you to compile & package your application in one stage and then copy only the necessary artifacts to the final image, reducing its size significantly. 

Build Images from Scratch If you only need to run a statically-compiled, standalone executable (like a C++ or Go application), pack it inside an empty Image by using “scratch” as the base image.  

Use fewer Layers Each instruction like RUN or COPY adds another layer to your image, thus increasing its size. Each layer comes with its own metadata & file system structures. The fewer layers you use, the lesser data overhead your image has. 

.dockerignore file can help us to avoid copy unnecessary files and reduce the image size too. 

 

 

 

What is Kubernetes? 

a portable, extensible, open source platform for managing containerized workloads and services, that facilitates both declarative configuration and automation 

 

What is service mesh in Kubernetes? 

A service mesh is a mesh of Layer 7 proxies (Applicatoin layer), not a mesh of services. Microservices can use a service mesh to abstract the network away 

It controls the delivery of service requests to other services, performs load balancing, encrypts data, and discovers other services. 

 

What is a service in Kubernetes? 

a logical abstraction for a deployed group of pods in a cluster (which all perform the same function). Since pods are ephemeral, a service enables a group of pods, which provide specific functions (web services, image processing, etc.) to be assigned a name and unique IP address (clusterIP). 

 

What is horizontal pod autoscaling vs. Vertical pod autoscaling? 

 HPA scales the number of pods in a deployment based on CPU and memory usage.  
          VPA scales the resources of each pod based on CPU and memory usage. 

 

kubectl top node 
	This command displays resource utilization (CPU/Memory/Storage) for nodes.  

 It is a great way to find any potential bottlenecks in the node 
kubectl top pod 
	This command displays resource utilization (CPU/Memory/Storage) for pods.  
	It helps to identify potential performance issues in pod. 
kubectl logs POD_NAME [-c CONTAINER_NAME] [flags] 
	This command displays the logs for a pod.  
	Troubleshoot problems with your pods and see what is happening inside of them. 
kubectl describe RESOURCE_TYPE RESOURCE_NAME [flags] 
	This command provides detailed information about a Kubernetes resource.  
	This is used to have detailed view of a resource and its properties. 
kubectl edit RESOURCE_TYPE RESOURCE_NAME [flags] 

This command opens a text editor so you can edit the definition of a Kubernetes resource.  

Make changes to a resource without having to recreate it. 
kubectl exec POD_NAME [-c CONTAINER_NAME] -- COMMAND 
	This command executes a command in a pod.  

Troubleshoot problems with your pods by running commands in pod. 
kubectl port-forward POD_NAME LOCAL_PORT:REMOTE_PORT [flags] 

This command forwards a port from a pod to your local machine.  

Access the application running in a pod from your local machine. 

 

What is a Helm chart?  

Yaml manifest that describes Kubernetes resources  

 

What is gRPC? 

A gRPC-based RPC framework is a great choice for inter-process communication in microservices applications. Not only the gRPC services are faster compared to RESTful services, but also they are strongly typed. The Protocol Buffer, a binary format for exchanging data, is used for defining gRPC APIs. 

 

 

 

 

 

 

What is your experience with Operations? Or managing releases? 

Ops – CI/CD 

What is your experience with CI/CD? 

What is blue/green deployment? 

What is Trunk Based development? 

Microservices deployment types? 

 

 

 

3. What is your experience with System Design? 

If you have a component in infrastructure and it is stateful? What does that mean? 

Past transactions used as a reference point 

Stateless? 

Previous transactions not stored or reference, not cached 

What does ephemeral mean? 

Volatile, temporary storage – would not survive reboot 

What is a Thundering Herd? 

When a large number of processes or threads waiting for an event are awoken when that event occurs, but only one process is able to handle the event 

What is master master replication? And how might you set it up? 

log-binbinlog-do-db=<database name> 
binlog-ignore-db=mysql 
binlog-ignore-db=test 
server-id=1 

mysql> GRANT REPLICATION SLAVE ON *.* TO ‘replication’@master1 identified by ‘slave2’; 

What is the difference between horizontal and vertical scaling? 

With vertical scaling (“scaling up”), you're adding more compute power to your existing instances/nodes. In horizontal scaling (“scaling out”), you get the additional capacity in a system by adding more instances to your environment, sharing the processing and memory workload across multiple devices. 

 

Ops – Linux – What experience do you have with Linux?/You said that 

What is systemd?  

Service manager for linux 

/etc/systemd enable service at boot 

How would you set a static IP address on a linux box? 

Nmtui 

/etc/sysconfig/network/scripts/{nic} 

Reboot to reload  

Ip a 

ip link set down br0 
ip link del dev br0 
systemctl restart systemd-networkd 

What is a kernel dump? And how would you debug it? 

All memory in use by the kernel when it errors.The following events can cause a kernel disruption: Kernel panic. Non-maskable interrupts (NMI) 

What is a seg fault? 

errors due to a process's attempts to access memory regions that it shouldn't 

 

 

General Programming – What experience/You said that/Handover to Jason/Kenny 

What is Big O notation? 

Measure time complexity, how long script takes to run/function calls 

What is an instance variable? 

How would you manage secrets in a Python script? 

How do you access the first element in a list/array? 

Is bash a high level or low level programming language? 

Is Python single threaded or multi threaded programming language? 

Is Python a declarative or imperative language? 

What is a tuple? 

Ordered unchangeable data type 

What is an iterator? 

Is Python a functional programming language? What does that mean? 

What is Pep8? 

What is are the 5 SOLID principles? 

single responsibility principle, open-closed principle, Liskov substitution principle, interface segregation principle, and dependency inversion principle 

DRY 

How would you test a Python script? 

What is object oriented programming? Classes, objects rather than data or objects 

 

 

Ops – Networking 

How would you debug a DNS request that’s failing?  

Dig, nslookup, ping  

What is the OSI model?  

What level is TCP in the OSI model?  

Layer 4 Transport Layer  

What level is Ping in the OSI model? 

Layer 3 Network Layer  

ICMP 

Scenario: running a service on a port, but you receive an error message: port already in use? 

lsof -i :20102 

netstat -ntupl | grep :20102 

Ps –aux  

Kill 15 (sigterm) or kill 9 (sigkill) 

 

 

 

AWS – Easy 

What are regions and availability zones? 

Availability zones are geographically separate locations. As a result, failure in one zone has no effect on EC2 instances in other zones. When it comes to regions, they may have one or more availability zones. This configuration also helps to reduce latency and costs. 

AWS regions are separate geographical areas, like the US-West 1 (North California) and Asia South (Mumbai). On the other hand, availability zones are the areas that are present inside the regions. These are generally isolated zones that can replicate themselves whenever required.   

 

What are Key-Pairs in AWS? 

The Key-Pairs are password-protected login credentials for the Virtual Machines that are used to prove our identity while connecting the Amazon EC2 instances. The Key-Pairs are made up of a Private Key and a Public Key which lets us connect to the instances. 

 

What is the difference between stopping and terminating an EC2 instance?  

While you may think that both stopping and terminating are the same, there is a difference. When you stop an EC2 instance, it performs a normal shutdown on the instance and moves to a stopped state. However, when you terminate the instance, it is transferred to a stopped state, and the EBS volumes attached to it are deleted and can never be recovered. 

 

What is the difference between dynamo db and RDS MySQL, Aurora? 

Key value db, versus relational database schema, read replicas auto restart clear long running jobs 

 

What is a snapshot? 

Snapshot copies the state of a system at a certain point in time, preserving a virtual picture of your server's file system and settings. Unlike a backup, which performs a full copy of your data, a snapshot only copies the settings and metadata required to restore your data in the event of a disruption 

EBS snapshot – copy of your data..  

RDS creates a storage volume snapshot of your database, backing up the entire database and not just individual databases. RDS saves the automated backups of your database according to the backup retention period that you specify. If necessary, you can recover your database to any point in time during the backup retention period 

 

AWS – Up to date 

What is EC2 instance Connect? 

 

AWS – Scenario 

If you have an EC2 instance, developers/no one can ssh to the box, how would you debug this? How can you recover/login to an EC2 instance for which you have lost the key? 

Detach the volume, re attach to another machine 

Follow the steps provided below to recover an EC2 instance if you have lost the key: 

Verify that the EC2Config service is running 

Detach the root volume for the instance 

Attach the volume to a temporary instance 

Modify the configuration file 

Restart the original instance 

 

How do you upgrade or downgrade a system with near-zero downtime? 

You can upgrade or downgrade a system with near-zero downtime using the following steps of migration: 

Open EC2 console 

Choose Operating System AMI 

Launch an instance with the new instance type 

Install all the updates 

Install applications 

Test the instance to see if it’s working 

If working, deploy the new instance and replace the older instance 

Once it’s deployed, you can upgrade or downgrade the system with near-zero downtime. 

 

How would you address a situation in which the relational database engine frequently collapses when traffic to your RDS instances increases, given that the RDS instance replica is not promoted as the master instance? 

A larger RDS instance type is required for handling significant quantities of traffic, as well as producing manual or automated snapshots to recover data if the RDS instance fails. 

 

On an EC2 instance, an application of yours is active. Once the CPU usage on your instance hits 80%, you must reduce the load on it. What strategy do you use to complete the task? 

It can be accomplished by setting up an autoscaling group to deploy additional instances, when an EC2 instance's CPU use surpasses 80% and by allocating traffic across instances via the creation of an application load balancer and the designation of EC2 instances as target instances. 

 

 

IaC 

Idempotency is a very important concept for IaC and Ansible for example, what is Idempotency? And why should Ansible scripts be idempotent? 

Idempotency, it should be run multiple times without changing the result beyond the initial application 

 

What is the tf state file in Terraform? 

The terraform state file, by default, is named terraform. tfstate and is held in the same directory where Terraform is run. It is created after running terraform apply . The actual content of this file is a JSON formatted mapping of the resources defined in the configuration and those that exist in your infrastructure 

How would you write an application to analyze a tf state file? Convert json to dict  

 

What are the sempahore options for Terraform? 

 a semaphore is a variable or abstract data type used to control access to a common resource by multiple threads and avoid critical section problems in a concurrent system such as a multitasking operating system 

Tfstate.lock  

 

What’s the difference between Terraform and CloudFormation? 

CloudFormation vendor specific, terraform has a state file, terraform will fail if a machine is down, CloudFormation wind back  

 

Cons of Terraform 

It does not support any revert function for wrong/invalid changes to resources.  

Terraform is difficult to debug.  

Difficult to operate the existing stacks. It does not contain GUI. 

 

What are the steps involved in a CloudFormation Solution? 

Create or use an existing CloudFormation template using JSON or YAML format. 

Save the code in an S3 bucket, which serves as a repository for the code. 

Use AWS CloudFormation to call the bucket and create a stack on your template.  

CloudFormation reads the file and understands the services that are called, their order, the relationship between the services, and provisions the services one after the other. 

 

How could you manage secrets in Terraform? 

Step 1: Use a secure remote backend 

By default, the terraform.tfstate file gets auto-generated in plain text after a terraform command runs. We use the remote backend to collaborate on Terraform projects along with securely storing the Terraform state file. It is possible to securely store sensitive data like access keys and secrets, along with other state information, ensuring that the sensitive data is not exposed to unauthorized parties. 

Step 2: Use environment variables 

In Terraform, we use environment variables to store and configure variables that are needed 	for our configuration to provision desired infrastructure components. We specifically use 	environment variables with the prefix TF_VAR_<variable_name> to define them as 		Terraform variables. During runtime, the values of the input variable (variable_name) are 	referenced from the environment with a corresponding variable with TF_VAR_ prefix. 

Step 3: Secure the Terraform host 

Terraform hosts are the virtual or physical machines that serve as the deployment targets for infrastructure code implementation. They are used to provision and manage resources on cloud providers and on-premises infrastructure, ensuring consistency and reproducibility of the infrastructure. 

Securing the Terraform host is crucial because: 

As the Terraform CLI is installed in the host, it must be protected against unauthorized access. 

Stop unwanted Terraform code execution that can impact infrastructure security. 

Protect Terraform config and state files against data leakage. 

Ensure compliance requirements are met, such as HIPAA and PCI-DSS. 

Protect against insider threats who have access to the infrastructure. 

 

Step 4: Encrypt files with AWS KMS, PGP, or SOPS 

AWS KMS is the key management service from Amazon that encrypts sensitive data stored 	in terraform config and state files. To implement AWS KMS in the RDS database example 		discussed previously, create a file with credentials as content in key-value format. 

 

Step 5: Use secret stores like Key Vault and AWS Secrets Manager 

AWS Secrets Manager 

HashiCorp Vault 

AWS Param Store 

GCP Secret Manager 

 

Step 6: Mask sensitive values 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

AWS – Other 

What's the difference between an Internet Gateway and a NAT Gateway? 

Internet Gateway traffic can go egress and ingress, NAT one way 

 

21. Explain Amazon EC2 root device volume? 

The image that will be used to boot an EC2 instance is stored on the root device drive. This occurs when an Amazon AMI runs a new EC2 instance. And this root device volume is supported by EBS or an instance store. In general, the root device data on Amazon EBS is not affected by the lifespan of an EC2 instance. 

 

22. Mention the different types of instances in  Amazon EC2 and explain its features. 

General Purpose Instances: They are used to compute a range of workloads and aid in the allocation of processing, memory, and networking resources. 

Compute Optimized Instances: These are ideal for compute-intensive applications. They can handle  batch processing workloads, high-performance web servers, machine learning inference, and various other tasks. 

Memory Optimized: They process workloads that handle massive datasets in memory and deliver them quickly. 

Accelerated Computing: It aids in the execution of floating-point number calculations, data pattern matching, and graphics processing. These functions are carried out using hardware accelerators. 

Storage Optimised: They handle tasks that require sequential read and write access to big data sets on local storage. 

 

 

What are the elements of an AWS CloudFormation template? 

AWS CloudFormation templates are YAML or JSON formatted text files that are comprised of five essential elements, they are: 

Template parameters 

Output values 

Data tables 

Resources 

File format version 

. What is auto-scaling? 

Auto-scaling is a function that allows you to provision and launch new instances whenever there is a demand. It allows you to automatically increase or decrease resource capacity in relation to the demand. 

8. Is there any other alternative tool to log into the cloud environment other than console? 

The that can help you log into the AWS resources are: 

Putty 

AWS CLI for Linux 

AWS CLI for Windows 

AWS CLI for Windows CMD 

AWS SDK 

Eclipse 

What are the tools and techniques that you can use in AWS to identify if you are paying more than you should be, and how to correct it? 

You can know that you are paying the correct amount for the resources that you are using by employing the following resources: 

Check the Top Services Table 

It is a dashboard in the cost management console that shows you the top five most used services. This will let you know how much money you are spending on the resources in question. 

Cost Explorer 

There are cost explorer services available that will help you to view and analyze your usage costs for the last 13 months. You can also get a cost forecast for the upcoming three months. 

AWS Budgets 

This allows you to plan a budget for the services. Also, it will enable you to check if the current plan meets your budget and the details of how you use the services. 

Cost Allocation Tags 

This helps in identifying the resource that has cost more in a particular month. It lets you organize your resources and cost allocation tags to keep track of your AWS costs. 

You are trying to provide a service in a particular region, but you do not see the service in that region. Why is this happening, and how do you fix it? 

Not all Amazon AWS services are available in all regions. When Amazon initially launches a new service, it doesn’t get immediately published in all the regions. They start small and then slowly expand to other regions. So, if you don’t see a specific service in your region, chances are the service hasn’t been published in your region yet. However, if you want to get the service that is not available, you can switch to the nearest region that provides the services. 

How do you set up a system to monitor website metrics in real-time in AWS? 

Amazon CloudWatch helps you to monitor the application status of various AWS services and custom events. It helps you to monitor: 

State changes in Amazon EC2 

Auto-scaling lifecycle events 

Scheduled events 

AWS API calls 

Console sign-in events 

What are some of the manual tasks that RDS has functionality for? 

Define and explain the three basic types of cloud services and the AWS products that are built based on them? 

The three basic types of cloud services are: 

Computing 

Storage 

Networking 

Define Snapshots in Amazon Lightsail? 

The point-in-time backups of EC2 instances, block storage drives, and databases are known as snapshots. They can be produced manually or automatically at any moment. Your resources can always be restored using snapshots, even after they have been created. These resources will also perform the same tasks as the original ones from which the snapshots were made. 

Multiple Linux Amazon EC2 instances running a web application for a firm are being used, and data is being stored on Amazon EBS volumes. The business is searching for a way to provide storage that complies with atomicity, consistency, isolation, and durability while also increasing the application's resilience in the event of a breakdown (ACID). What steps should a solutions architect take to fulfill these demands? 

AWS Auto Scaling groups can create an application load balancer that spans many availability zones. Mount a target on each instance and save data on Amazon EFS. 

What are some critical differences between AWS S3 and EBS? 

Here are some differences between AWS S3 and EBS 

feature differences 

4. What is geo-targeting in CloudFront? 

Geo-Targeting is a concept where businesses can show personalized content to their audience based on their geographic location without changing the URL. This helps you create customized content for the audience of a specific geographical area, keeping their needs in the forefront. 

What services can be used to create a centralized logging solution? 

The essential services that you can use are Amazon CloudWatch Logs, store them in Amazon S3, and then use Amazon Elastic Search to visualize them. You can use Amazon Kinesis Firehose to move the data from Amazon S3 to Amazon ElasticSearch. 

10. What are the native AWS Security logging capabilities? 

Most of the AWS services have their logging options. Also, some of them have an account level logging, like in AWS CloudTrail, AWS Config, and others. Let’s take a look at two services in specific: 

AWS CloudTrail 

This is a service that provides a history of the AWS API calls for every account. It lets you perform security analysis, resource change tracking, and compliance auditing of your AWS environment as well. The best part about this service is that it enables you to configure it to send notifications via AWS SNS when new logs are delivered. 

AWS Config  

This helps you understand the configuration changes that happen in your environment. This service provides an AWS inventory that includes configuration history, configuration change notification, and relationships between AWS resources. It can also be configured to send information via AWS SNS when new logs are delivered. 

14. What are the different types of virtualization in AWS, and what are the differences between them? 

The three major types of virtualization in AWS are:  

Hardware Virtual Machine (HVM) 

It is a fully virtualized hardware, where all the virtual machines act separate from each other. These virtual machines boot by executing a master boot record in the root block device of your image. 

Paravirtualization (PV) 

Paravirtualization-GRUB is the bootloader that boots the PV AMIs. The PV-GRUB chain loads the kernel specified in the menu. 

Paravirtualization on HVM 

PV on HVM helps operating systems take advantage of storage and network I/O available through the host. 

15. Name some of the AWS services that are not region-specific 

AWS services that are not region-specific are: 

IAM 

Route 53 

Web Application Firewall  

CloudFront 

16. What are the differences between NAT Gateways and NAT Instances? 

While both NAT Gateways and NAT Instances serve the same function, they still have some key differences. 

17. What is CloudWatch? 

The Amazon CloudWatch has the following features: 

Depending on multiple metrics, it participates in triggering alarms. 

Helps in monitoring the AWS environments like CPU utilization, EC2, Amazon RDS instances, Amazon SQS, S3, Load Balancer, SNS, etc. 

19. With specified private IP addresses, can an Amazon Elastic Compute Cloud (EC2) instance be launched? If so, which Amazon service makes it possible? 

Yes. Utilizing VPC makes it possible (Virtual Private Cloud). 

23. Will your standby RDS be launched in the same availability zone as your primary? 

No, standby instances are launched in different availability zones than the primary, resulting in physically separate infrastructures. This is because the entire purpose of standby instances is to prevent infrastructure failure. As a result, if the primary instance fails, the backup instance will assist in recovering all of the data. 

24. What is the difference between a Spot Instance, an On-demand Instance, and a Reserved Instance? 

Spot instances are unused EC2 instances that users can use at a reduced cost. 

When you use on-demand instances, you must pay for computing resources without making long-term obligations. 

Reserved instances, on the other hand, allow you to specify attributes such as instance type, platform, tenancy, region, and availability zone. Reserved instances offer significant reductions and capacity reservations when instances in certain availability zones are used. 

32. Describe PaaS. 

PaaS supports the operation of multiple cloud platforms, primarily for the development, testing, and  oversight of the operation of the program. 

36. What Are Some of the Security Best Practices for Amazon EC2? 

Security best practices for Amazon EC2 include using Identity and Access Management (IAM) to control access to AWS resources; restricting access by only allowing trusted hosts or networks to access ports on an instance; only opening up those permissions you require, and disabling password-based logins for instances launched from your AMI. 

37. Can S3 Be Used with EC2 Instances, and If Yes, How? 

Amazon S3 can be used for instances with root devices backed by local instance storage. That way, developers have access to the same highly scalable, reliable, fast, inexpensive data storage infrastructure that Amazon uses to run its own global network of websites. To execute systems in the Amazon EC2 environment, developers load Amazon Machine Images (AMIs) into Amazon S3 and then move them between Amazon S3 and Amazon EC2. 

Amazon EC2 and Amazon S3 are two of the best-known web services that make up AWS. 

39. What are the different types of EC2 instances based on their costs? 

The three types of EC2 instances are: 

On-demand Instance 

It is cheap for a short time but not when taken for the long term 

Spot Instance 

It is less expensive than the on-demand instance and can be bought through bidding.  

Reserved Instance 

If you are planning to use an instance for a year or more, then this is the right one for you. 

42. How do you configure CloudWatch to recover an EC2 instance? 

Here’s how you can configure them: 

Create an Alarm using Amazon CloudWatch 

In the Alarm, go to Define Alarm -> Actions tab 

Choose Recover this instance option 

43. What are the common types of AMI designs? 

There are many types of AMIs, but some of the common AMIs are: 

Fully Baked AMI 

Just Enough Baked AMI (JeOS AMI) 

Hybrid AMI 

45. What is Amazon S3?  

S3 is short for Simple Storage Service, and Amazon S3 is the most supported storage platform available. S3 is object storage that can store and retrieve any amount of data from anywhere. Despite that versatility, it is practically unlimited as well as cost-effective because it is storage available on demand. In addition to these benefits, it offers unprecedented levels of durability and availability. Amazon S3 helps to manage data for cost optimization, access control, and compliance. 

 

 
 

8. How do you allow a user to gain access to a specific bucket? 

You need to follow the four steps provided below to allow access. They are: 

Categorize your instances 

Define how authorized users can manage specific servers. 

Lockdown your tags 

Attach your policies to IAM users 

52. What Is Amazon Virtual Private Cloud (VPC) and Why Is It Used? 

A VPC is the best way of connecting to your cloud resources from your own data center. Once you connect your datacenter to the VPC in which your instances are present, each instance is assigned a private IP address that can be accessed from your data center. That way, you can access your public cloud resources as if they were on your own private network. 

56. How do you monitor Amazon VPC? 

You can monitor VPC by using: 

CloudWatch and CloudWatch logs 

VPC Flow Logs 

General AWS Interview Questions 

58. When Would You Prefer Provisioned IOPS over Standard Rds Storage? 

You would use Provisioned IOPS when you have batch-oriented workloads. Provisioned IOPS delivers high IO rates, but it is also expensive. However, batch processing workloads do not require manual intervention.  

59. How Do Amazon Rds, Dynamodb, and Redshift Differ from Each Other? 

Amazon RDS is a database management service for relational databases. It manages patching, upgrading, and data backups automatically. It’s a database management service for structured data only. On the other hand, DynamoDB is a NoSQL database service for dealing with unstructured data. Redshift is a data warehouse product used in data analysis. 

60. What Are the Benefits of AWS’s Disaster Recovery? 

Businesses use cloud computing in part to enable faster disaster recovery of critical IT systems without the cost of a second physical site. The AWS cloud supports many popular disaster recovery architectures ranging from small customer workload data center failures to environments that enable rapid failover at scale. With data centers all over the world, AWS provides a set of cloud-based disaster recovery services that enable rapid recovery of your IT infrastructure and data. 

61. How can you add an existing instance to a new Auto Scaling group? 

Here’s how you can add an existing instance to a new Auto Scaling group: 

Open EC2 console 

Select your instance under Instances 

Choose Actions -> Instance Settings -> Attach to Auto Scaling Group 

Select a new Auto Scaling group 

Attach this group to the Instance 

Edit the Instance if needed 

Once done, you can successfully add the instance to a new Auto Scaling group 

69. How is AWS CloudFormation different from AWS Elastic Beanstalk? 

Here are some differences between AWS CloudFormation and AWS Elastic Beanstalk: 

AWS CloudFormation helps you provision and describe all of the infrastructure resources that are present in your cloud environment. On the other hand, AWS Elastic Beanstalk provides an environment that makes it easy to deploy and run applications in the cloud. 

AWS CloudFormation supports the infrastructure needs of various types of applications, like legacy applications and existing enterprise applications. On the other hand, AWS Elastic Beanstalk is combined with the developer tools to help you manage the lifecycle of your applications. 

 

71. What happens when one of the resources in a stack cannot be created successfully? 

If the resource in the stack cannot be created, then the CloudFormation automatically rolls back and terminates all the resources that were created in the CloudFormation template. This is a handy feature when you accidentally exceed your limit of Elastic IP addresses or don’t have access to an EC2 AMI. 

AWS Questions for Elastic Block Storage 

72. How can you automate EC2 backup using EBS? 

Use the following steps in order to automate EC2 backup using EBS: 

Get the list of instances and connect to AWS through API to list the Amazon EBS volumes that are attached locally to the instance. 

List the snapshots of each volume, and assign a retention period of the snapshot. Later on, create a snapshot of each volume. 

Make sure to remove the snapshot if it is older than the retention period. 

73. What is the difference between EBS and Instance Store? 

EBS is a kind of permanent storage in which the data can be restored at a later point. When you save data in the EBS, it stays even after the lifetime of the EC2 instance. On the other hand, Instance Store is temporary storage that is physically attached to a host machine. With an Instance Store, you cannot detach one instance and attach it to another. Unlike in EBS, data in an Instance Store is lost if any instance is stopped or terminated. 

77. What are the different uses of the various load balancers in AWS Elastic Load Balancing? 

Application Load Balancer 

Used if you need flexible application management and TLS termination. 

Network Load Balancer 

Used if you require extreme performance and static IPs for your applications. 

Classic Load Balancer 

Used if your application is built within the EC2 Classic network 

AWS Interview Questions for Route 53 

86. What Is Amazon Route 53? 

Amazon Route 53 is a scalable and highly available Domain Name System (DNS). The name refers to TCP or UDP port 53, where DNS server requests are addressed. 

 
AWS Interview Questions for Database 

93. How are reserved instances different from on-demand DB instances? 

Reserved instances and on-demand instances are the same when it comes to function. They only differ in how they are billed. 

Reserved instances are purchased as one-year or three-year reservations, and in return, you get very low hourly based pricing when compared to the on-demand cases that are billed on an hourly basis. 

94. Which type of scaling would you recommend for RDS and why? 

There are two types of scaling - vertical scaling and horizontal scaling. Vertical scaling lets you vertically scale up your master database with the press of a button. A database can only be scaled vertically, and there are 18 different instances in which you can resize the RDS. On the other hand, horizontal scaling is good for replicas. These are read-only replicas that can only be done through Amazon Aurora. 

95. What is a maintenance window in Amazon RDS? Will your DB instance be available during maintenance events? 

RDS maintenance window lets you decide when DB instance modifications, database engine version upgrades, and software patching have to occur. The automatic scheduling is done only for patches that are related to security and durability. By default, there is a 30-minute value assigned as the maintenance window and the DB instance will still be available during these events though you might observe a minimal effect on performance. 

96. What are the consistency models in DynamoDB? 

There are two consistency models In DynamoDB. First, there is the Eventual Consistency Model, which maximizes your read throughput. However, it might not reflect the results of a recently completed write. Fortunately, all the copies of data usually reach consistency within a second. The second model is called the Strong Consistency Model. This model has a delay in writing the data, but it guarantees that you will always see the updated data every time you read it.  

97. What type of query functionality does DynamoDB support? 

DynamoDB supports GET/PUT operations by using a user-defined primary key. It provides flexible querying by letting you query on non-primary vital attributes using global secondary indexes and local secondary indexes. 

1. Suppose you are a game designer and want to develop a game with single-digit millisecond latency, which of the following database services would you use? 

Amazon DynamoDB 

2. If you need to perform real-time monitoring of AWS services and get actionable insights, which services would you use? 

Amazon CloudWatch 

 

 

 


---