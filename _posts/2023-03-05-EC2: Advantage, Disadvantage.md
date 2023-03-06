---
layout: post
title:  "EC2: Advantage Over Data Center"
categories: aws
---

# EC2: Advantange Over Data Center
{: style="text-align: center"}

Written by Nick Otter. 

# Contents

- [**Introduction**](#introduction)<br>
- [**Advantages**](#last-week-in-aws)<br> 
  - [**HA**](#ha)<br>
  - [**CPU Power**](#power)<br>
  - [**High Throughput, Low Latency**](#high-throughput-low-latency)<br>
  - [**Multiple Storage Options**](#multiple-storage-options)<br>
  - [**Auto Scaling**](#auto-scaling)<br>
  - [**ELB**](#elb)<br>
  - [**VPC**](#vpc)<br>
  - [**Elastic IP**](#elastic-ip)<br>
  - [**OS Choice**](#os-choice)<br>
- [**Conclusion**](#conclusion)


# Introduction

Welcome to *"Advantage Over Data Center"*. We're looking at AWS EC2 today. Let's take a look at each of it's features and compare with our old friend, The DC.

# HA

AWS offers a minimum of 3 Availability Zones (data centers with redundant power, networking, and connectivity in an AWS Region) per Region (North America, South America, Europe, Middle East, Africa, Asia Pacific, Australia and New Zealand).

*Result*: We have to chalk this up as a draw versus a Data Center. DC resource is available in all these regions also. But would you have to go multi-vendor for your High Availability estate? Most probably.

AWS 1 - 1 Data Center

# CPU Power

EC2 offers "Amazon Graviton2 processors". Hmm.

* 64-bit Arm Neoverse cores 
* Each core is a single-threaded vCPU.  
* always-on fully encrypted DRAM memory, 
* hardware acceleration for compression workloads, 
* dedicated engines per vCPU 
* int8/fp16 CPU-based machine learning inference acceleration. 
* Customize number of vCPUs when launching new instances 

_Result:_ Okay let's speed this up it's [cheaper than AMD and Intel](https://www.anandtech.com/show/15578/cloud-clash-amazon-graviton2-arm-against-intel-and-amd) and most probably the DC you want to use will not have top spec servers.

![](/assets/ec2vsamdxeon.png)

AWS 2 - 1 Data Center

# High Throughput Low Latency

Cluster Compute, Cluster GPU, and High Memory Cluster instances.. I would describe these as cute bundles. Nifty. 

Definite score. 

_Result:_ AWS 3 - 1 Data Center

# Multiple Storage Options

Amazon Elastic Block Store (Amazon EBS) optimized instances for IOPS. This is just a block storage volume right? So, just increasing the disk size? Err. No goals scored here.

# Auto Scaling

The equivalent of AWS EC2 Auto Scaling would be to write your own custom Terraform wrapper maybe (been there!) - or just bite the bullet and Go Big or Go Home in VCenter. Big win for AWS EC2 here.

![](/assets/as-basic-diagram.png)

_Result:_ AWS 4 - 1 Data Center

# ELB

Automatic Traffic Distribution with Elastic Load Balancing. Is there anyone who longs to use Citrix? Who wishes to have a totally separate vendor for a specific LB resource? Logging in in a spearate window to an LB GUI? Win for AWS. 

_Result:_ AWS 5 - 1 Data Center

# VPC


# Elastic IP

# OS Choice

This is a draw. Nice to have, sure. But these .iso's are free yaknow.

---

Thanks. This article was written by Nick Otter.
