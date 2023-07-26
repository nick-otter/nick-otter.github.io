---
layout: post
title:  "AWS Network Components"
categories: aws_network
---

# AWS Network Components
{: style="text-align: center"}

| Category | Use cases | Service | Descri­ption |
| --- | --- | --- | --- |
| **Build a cloud network** | Define and provision a logically isolated network for your AWS resources | VPC | VPC lets you provision a logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network that you define. |
| | Connect VPCs and on-pre­mises networks through a central hub | Transit Gateway | Transit Gateway connects VPCs & on-pre­mises networks through a central hub. This simplifies network & puts an end to complex peering relati­ons­hips.|
| | Provide private connec­tivity between VPCs, services, and on-pre­mises applic­ations | Privat­eLink | Privat­eLink provides private connec­tivity between VPCs & services hosted on AWS or on-pre­mises, securely on the Amazon network. |
| | Route users to Internet applic­ations with a managed DNS service | Route 53 | Route 53 is a highly available & scalable cloud DNS web service. | 
| **Scale your network design** | Automa­tically distribute traffic across a pool of resources, such as instances, contai­ners, IP addresses, and Lambda functions | Elastic Load Balancing | Elastic Load Balancing automa­tically distri­butes incoming applic­ation traffic across multiple targets, such as EC2's, contai­ners, IP addresses, & Lambda functions. |
| | Direct traffic through the AWS Global network to improve global applic­ation perfor­mance | Global Accele­rator | Global Accele­rator is a networking service that sends user’s traffic through AWS’s global network infras­tru­cture, improving internet user perfor­mance by up to 60%.|
| **Secure your network traffic** | Safeguard applic­ations running on AWS against DDoS attacks | Shield | Shield is a managed Distri­buted Denial of Service (DDoS) protection service that safeguards applic­ations running on AWS.|
| | Protect your web applic­ations from common web exploits | WAF | WAF is a web applic­ation firewall that helps protect your web applic­ations or APIs against common web exploits that may affect availa­bility, compromise security, or consume excessive resources. |
| | Centrally configure and manage firewall rules | Firewall Manager | Firewall Manager is a security management service which allows to centrally configure & manage firewall rules across accounts & apps in AWS Organi­zation.|
| **Build a hybrid IT network** | Connect your users to AWS or on-pre­mises resources using a Virtual Private Network | (VPN) - Client | VPN solutions establish secure connec­tions between on-pre­mises networks, remote offices, client devices, & the AWS global network. |
| | Create an encrypted connection between your network and your Amazon VPCs or AWS Transit Gateways | (VPN) - Site to Site | Site-t­o-Site VPN creates a secure connection between data center or branch office & AWS cloud resources.|
| | Establish a private, dedicated connection between AWS and your datace­nter, office, or colocation enviro­nment | Direct Connect | Direct Connect is a cloud service solution that makes it easy to establish a dedicated network connection from your premises to AWS. |
| **Content delivery networks** | Securely deliver data, videos, applic­ations, and APIs to customers globally with low latency, and high transfer speeds | CloudFront | CloudFront expedites distri­bution of static & dynamic web content.|
| **Build a network for micros­ervices archit­ectures** |Provide applic­ati­on-­level networking for containers and micros­ervices | App Mesh| App Mesh makes it accessible to guide & control micros­ervices operating on AWS.|
| | Create, maintain, and secure APIs at any scale | API Gateway |API Gateway allows the user to design & expand their own REST and WebSocket APIs at any scale.|
| | Discover AWS services connected to your applic­ations | Cloud Map | Cloud Map permits the name & handles the cloud resources.|

---