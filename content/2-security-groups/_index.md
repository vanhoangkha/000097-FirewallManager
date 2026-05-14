---
title : "Security Groups"
date: 2024-01-01
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

**Get to know Security Groups**

**Security Groups** is a potent tool created to manage network access to AWS Cloud resources, especially **EC2 instances**, thereby enforcing network security in the L3/4 layer but for **EC2 interfaces**.

#### Basic knowledge
Basic things we should know when conducting configuration:
- By default, there will be two rules: **inbound** and **outbound**.
- By default, there will not be any **inbound rules** that allow access in the incoming direction.
- It is not possible to create a rule with a negation rule.
- When associated with an **EC2 instance** will act as a `Host-based Firewall`.
- One **EC2 instance** can be associated with up to 5 Security Groups.
- Cannot link to a **VPC** or **VPC Subnet**.
- For regulation, the source can be:
  - IP Address
  - VPC Subnets CIDR
  - Security Group ID
- Multiple **EC2 instances** can be linked.

{{% notice tip %}}
We can refer to [Security Group Rules](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html#SecurityGroupRules).
{{% /notice %}}

**Content**
- [Basic knowledge](#basic-knowledge)
- [Create Security Group](#create-security-group)
- [Best Practices](#best-practices)
- [Enable AWS Config Service](#enable-aws-config-service)

#### Create Security Group
1. Go to the [Amazon EC2 console](https://console.aws.amazon.com/ec2/).
2. In the left-hand navigation bar, select **Security Groups**.

![ec2-security-groups](/images/1/0001.png?featherlight=false&width=90pc)

3. Select the `Create security group` button.

![ec2-security-groups](/images/1/0002.png?featherlight=false&width=90pc)

4. In the *Basic details* section, we need to enter a name, and description and select **VPC**.

![ec2-security-groups](/images/1/0003.png?featherlight=false&width=90pc)

5. In the *Inbound rules* section, click the `Add rule` button to proceed further as follows:

![ec2-security-groups](/images/1/0004.png?featherlight=false&width=90pc)

6. In the *Onbound rules* section, we will keep the default.

![ec2-security-groups](/images/1/0005.png?featherlight=false&width=90pc)

7. In the *Tags* section, click the `Add new tag` button to add specific Tags.
8. Click the `Create security group` button to initialize.

{{% notice note %}}
Alternatively, we can initialize via [AWS CLI](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-security-group.html).
{{% /notice %}}

#### Best Practices
**AWS** recommends several standards as we proceed to use **Security Groups**. Securing these standards in a large system as well as multiple environments on the AWS Cloud is always a challenge, as new application deployments become faster and more frequent.

---
1. **Delete unused security groups**

If a large number of unused security groups still exist, this can make it difficult for Administrators to manage and find, the configuration process can lead to errors due to some duplicate rules.

| Ingredients | Details |
| --- | --- |
| AWS Resources | Amazon EC2 security group |
| AWS Config Managed Rule | [ec2-security-group-attached-to-eni](https://docs.aws.amazon.com/config/latest/developerguide/ec2-security-group-attached-to-eni.html) |
| Criteria in PCI DSS | [PCI.EC2.3](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-pci-controls.html#pcids-ec2-3) |
| Level in PCI DSS | **LOW** |

---
2. **Limited to Edit**

Editing the Security Group is very important, just a small mistake can affect the whole system. Then we proceed to limit through **IAM** service and only some *IAM Roles* are allowed to perform editing operations.

| Ingredients | Details |
| --- | --- |
| AWS Resources | Amazon EC2 security group |
| AWS Config Managed Rule | [iam-policy-no-statements-with-admin-access](https://docs.aws.amazon.com/config/latest/developerguide/iam-policy-no-statements-with-admin-access.html) |
| Criteria in PCI DSS | [PCI.IAM.3](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-pci-controls.html#pcids-iam-3) |
| Level in PCI DSS | **HIGH** |

---
3. **Track creation and deletion**

This has always been one of the most basic criteria for tracking activities related to the creation or deletion of Security Groups.

| Ingredients | Details |
| --- | --- |
| AWS Resources | Amazon EC2 security group |
| AWS Config Managed Rule | N/A |
| Criteria in CIS Benchmark | [3.10](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html#securityhub-cis-controls-3.10) |
| Degree in CIS Benchmark | **LOW** |

---
4. **Don't forget to limit Outbound**

We should always limit access to the **Outbound** side of a Security Group to specific Subnets.

| Ingredients | Details |
| --- | --- |
| AWS Resources | Amazon EC2 security group |
| AWS Config Managed Rule | [vpc-default-security-group-closed](https://docs.aws.amazon.com/config/latest/developerguide/vpc-default-security-group-closed.html) |
| Criteria in PCI DSS | [PCI.EC2.2](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-pci-controls.html#pcids-ec2-2) |
| Level in PCI DSS | **MEDIUM** |

---
5. **Limit the range of Inbound Ports allowed to access**

Only Ports required for the operation of an application should be considered open on the **Inbound** side of a Security Group. With a large number of unnecessary ports open, this can lead to potential vulnerabilities that are easily exploited by exploratory access as well as attacks.

| Ingredients | Details |
| --- | --- |
| AWS Resources | Amazon EC2 security group |
| AWS Config Managed Rule | [restricted-ssh](https://docs.aws.amazon.com/config/latest/developerguide/restricted-ssh.html), [restricted-common-ports](https://docs.aws.amazon.com /config/latest/developerguide/restricted-common-ports.html) |
| Criteria in CIS Benchmark | [4.1](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html#securityhub-cis-controls-4.1), [4.2](https://docs.aws .amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html#securityhub-cis-controls-4.2) |
| Degree in CIS Benchmark | **HIGH** |
| Criteria in PCI DSS | [PCI.EC2.5](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-pci-controls.html#pcids-ec2-5) |
| Level in PCI DSS | **HIGH** |

#### Enable AWS Config Service
Organizations and businesses should urgently address those challenges through automated control and monitoring tasks. With the **AWS Config** service, we can configure many rules to automate the detection of violations to the current system.

{{% notice info %}}
Enabling the **Config** service will be the premise for the next part of the exercise.
{{% /notice %}}

Here are a few steps to a basic **Managed Rule** configuration.
1. Access the service [Config](https://console.aws.amazon.com/config/)
2. If it is the first time accessing the service, they press the `Get Started` button.
3. In the *Settings* section, keep the default configuration as follows:

![ec2-security-groups](/images/1/0006.png?featherlight=false&width=90pc)

4. In the *Rules* section, we see a list of **Managed Rules**.

![ec2-security-groups](/images/1/0007.png?featherlight=false&width=90pc)

5. In the search box, enter `restricted-ssh` and select the corresponding rule.

![ec2-security-groups](/images/1/0008.png?featherlight=false&width=90pc)

6. Conduct initialization.

![ec2-security-groups](/images/1/0009.png?featherlight=false&width=90pc)