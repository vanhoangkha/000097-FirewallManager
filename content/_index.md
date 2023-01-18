---
title : "Continuous Audit and Limit Security Groups with AWS Firewall Manager"
date : "`r Sys.Date()`"
weight : 1
chapter : false
---


# Continuously Audit and Limit Security Groups with AWS Firewall Manager

## Overview
If you have been learning about the security process on AWS Cloud, the topic of [10 most notable tips on safety and information security on AWS Cloud](https://aws.amazon.com/ blogs/security/top-10-security-items-to-improve-in-your-aws-account/) from AWS CISO **Stephan Schmidt** should be of particular interest to you.

![cloud-security-10-places](/images/1-cloud-security-10-places.png?width=40pc)

In particular, one of the fundamental and basic tips about network security is secure remote access to servers or services. In an on-premise environment, we are all too familiar with the **Firewall** component or similar technologies that limit remote access by allowing only certain network addresses ( IP Address), certain communication port (Port) and protocol (Protocol). So we also continue to deploy a similar solution on AWS Cloud? The answer is *YES*.

As we gradually migrate existing workloads or deploy new resources on AWS, we will need to familiarize ourselves with concepts like ***Security Groups** or the **AWS Network Firewall** service.

In this exercise, we will familiarize ourselves with some scenarios and summarize each step to better understand the **AWS Network Firewall** service during remote access security through 2 familiar protocols. belonging are *RDP* and *SSH*.
- Level: 300
- Duration: 1-2 hours

#### Prerequisites:
  - IAM User (Admin) and AWS CLI

#### AWS services used:
  - Amazon EC2 Security Groups
  - AWS Configuration
  - AWS Organizations
  - AWS Firewall Manager
  - AWS Resource Access Manager

#### Content

1. [Intro](1-intro/)
2. [Security Groups](2-security-groups/)
3. [Firewall](3-firewall-manager/)
4. [Manage Security Groups](4-firewall-manager-security-groups-policies/)
5. [Conclusion](5-conclusion/)