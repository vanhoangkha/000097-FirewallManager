---
title : "AWS Firewall Manager"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

**Getting familiar with the AWS Firewall Manager service**

**Firewall Manager** is a security management service that allows us to configure **Firewall rules** across AWS applications and accounts that are within an organization's **AWS Organization**.

When an application is deployed, security rules are established by the **Firewall Manager**, which are imposed and protected based on the underlying rules. Thereby, make sure that the resources (eg **Security Group**) infringing will be checked and removed automatically.

It is a central service that includes features such as creating security policies, imposing them, and automatically scanning the resources of a large system throughout.

The capabilities that **Firewall Manager** provides to Security Groups fall into three main categories:
1. Initialize and apply basic security to the Security Group.
2. Review and clean up duplicate and unused Security Groups.
3. Identify any Security Group regulatory definitions that are too open and high-risk.

![firewall-manager-architecture](/images/2-firewall-manager-architecture.png?featherlight=false&width=60pc)

**Content**
- [Prerequisites](#prerequisites)
- [Setting up AWS Organizations](#setting-up-aws-organizations)
- [Configure the Firewall Manager Administrator account](#configure-the-firewall-manager-administrator-account)
- [Enable AWS Config Service](#enable-aws-config-service)
- [Enable resource sharing](#enable-resource-sharing)

#### Prerequisites
To be able to prepare and activate **AWS Firewall Manager**, for the first time using this service, we need to perform the following steps in turn:
1. Set up [AWS Organizations](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_introduction.html).
2. Configure the **Firewall Manager Administrator** account.
3. Enable **AWS Config** service.
4. Enable Resource Sharing (For *Network Firewall* Policies)

{{% notice tip %}}
Learn more about [AWS Firewall Manager Prerequisites](https://docs.aws.amazon.com/waf/latest/developerguide/fms-prereq.html).
{{% /notice %}}

#### Setting up AWS Organizations
If your AWS account is already a member of **AWS Organization**, you can move on to the next step. If not, you will need to proceed with the **AWS Organization** setup.

![ec2-security-groups](/images/2/0001.png?featherlight=false&width=90pc)

{{% notice tip %}}
Learn more about [Creating & Managing AWS Organizations](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_org.html).
{{% /notice %}}

#### Configure the Firewall Manager Administrator account
1. Access the AWS Console through a fully authorized IAM User account.
2. Access the service [Firewall Manager](https://console.aws.amazon.com/wafv2/fms).

![ec2-security-groups](/images/2/0002.png?featherlight=false&width=90pc)

3. Click the `Get Started` button.
4. Enter the ID of the AWS account you want to link.

![ec2-security-groups](/images/2/0003.png?featherlight=false&width=90pc)

5. Click the `Set administrator account` button.
6. Once the configuration is successful, you will receive the corresponding message as follows.

![ec2-security-groups](/images/2/0004.png?featherlight=false&width=90pc)

#### Enable AWS Config Service
In the previous section, we enabled the **AWS Config** service from the AWS Console. However, we can quickly activate it through the initialization of a **CloudFormation Stacksets**.

| Ingredients | Value (Required) |
| ---------- | ------- |
| Stack Name | enable-aws-config |
| Template URL | [EnableAWSConfig.yml](https://s3.amazonaws.com/cloudformation-stackset-sample-templates-us-east-1/EnableAWSConfig.yml) |

#### Enable resource sharing
To be able to manage *Network Firewall* policies across AWS accounts, you need to enable resource sharing with **AWS Organization** through the **AWS Resource Access Manager** service.

1. Access the service [Resource Access Manager](https://console.aws.amazon.com/ram).
2. In the left-hand navigation bar, select **Settings**.

![ec2-security-groups](/images/2/0005.png?featherlight=false&width=90pc)

3. Click `Enable sharing with AWS Organizations`.


![ec2-security-groups](/images/2/0006.png?featherlight=false&width=90pc)

4. Select the `Save Settings` button.


![ec2-security-groups](/images/2/0007.png?featherlight=false&width=90pc)

Also, we can use [AWS CLI](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ram/enable-sharing-with-aws-organization.html) to progress activation action.

```bash
aws ram enable-sharing-with-aws-organization
```

{{% notice tip %}}
Learn more about [Enable Sharing with AWS Organizations](https://docs.aws.amazon.com/ram/latest/userguide/getting-started-sharing.html#getting-started-sharing-orgs)
{{% /notice %}}