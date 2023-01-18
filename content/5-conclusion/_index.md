---
title : "Conclusion"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5. </b> "
---


#### Conclusion
In this exercise, we carefully went through the case of setting the security policy of the **Firewall Manager** service for **Security Groups** resources via access protocols. as RDP, SSH, or SMB, which was covered by AWS CISO **Stephen Schmidt** on the topic `Limit Security Groups`.

**Firewall Manager** helps us to constantly have a security overview of the system as well as proactively deal with violating resources. Here is an overview of the possible scenarios you face:
1. Identify duplicate Security Groups.
2. Define Security Groups that are not used after N days.
3. List applications that can be accessed from the Internet.
4. Reject certain CIDR ranges.
5. Deny certain Ports.
6. Deny the `ALL` protocol in Security Groups and require the protocol to be specified.
7. Deploy moderated **Pre-approved Security Groups** and link them to resources automatically.