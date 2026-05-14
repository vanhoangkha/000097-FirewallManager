---
title : "Manage Security Group"
date: 2024-01-01
weight : 4
chapter : false
pre : " <b> 4. </b> "
---


**Manage Security Groups with Firewall Manager**

Usually, remote access to application infrastructure through familiar protocols such as RDP, SSH, or SMB is often overlooked and leaves access loosely open to the Internet. At that time, any organization or individual can be detected using automated detection tools. This is an urgent problem and needs an immediate solution.

In many cases, the most common agent is a temporary configuration from the system administrator for testing activities. Assuming this configuration continues to be maintained after that, we can see that the potential attack opportunities from threats coming from the Internet are extremely large.

To be able to address the threats mentioned above, we will proceed to set up the **Firewall Manager** service to be able to check and limit Security Groups that are identified as potential threats. thereby exposing application infrastructure vulnerabilities to external threats.

![firewall-manager-security-group-policies](/images/3-firewall-manager-security-group-policies.jpeg?featherlight=false&width=60pc)

{{% notice tip %}}
Learn more about [Firewall Manager's Security Group Policies](https://docs.aws.amazon.com/waf/latest/developerguide/security-group-policies.html)
{{% /notice %}}

**Content**
- [Policy setting](#policy-setting)
- [Conduct survey and evaluation](#conduct-survey-and-evaluation)
- [Perform the Remediation process](#perform-the-remediation-process)

#### Policy setting
The assumption is that we will proceed to determine which *Public IP* addresses are allowed to access the application infrastructure, then perform the evaluation process and finally automate the **Remediation** process.

1. Access the [Firewall Manager](https://console.aws.amazon.com/wafv2/fms) service under the **Firewall Manager Administrator** account.

![ec2-security-groups](/images/3/0001.png?featherlight=false&width=90pc)

2. In the left-hand navigation bar, select **Security policies**.
3. Click the `Create policy` button.
4. In the *Choose policy type and Region* section
   1. Policy Type: `Security Group`
   2. Security group policy type: `Auditing and enforcement of security group rules`


![ec2-security-groups](/images/3/0002.png?featherlight=false&width=90pc)

5. In the *Describe policy* section
   1. Policy name: Enter the policy name

![ec2-security-groups](/images/3/0003.png?featherlight=false&width=90pc)

   2. Policy rule options: `Configure managed audit policy rules`
   3. Policy rules
      1. Security group rules to audit: `Inbound Rules`
      2. Audit high-risk applications: `Applications that can use public CIDR ranges`

![ec2-security-groups](/images/3/0004.png?featherlight=false&width=90pc)

   4. Click the `Add Application List` button.
   5. Select **FMS-Default-Public-Access-Apps-Denied**.

![ec2-security-groups](/images/3/0005.png?featherlight=false&width=90pc)

   6. Policy action: `Identify resources that don't comply with the policy rules, but don't auto remediate`.

![ec2-security-groups](/images/3/0006.png?featherlight=false&width=90pc)

1. Click the `Next` button.
2. In the *Define policy scope* section

![ec2-security-groups](/images/3/0007.png?featherlight=false&width=90pc)

   1. Policy scope:
      1. AWS accounts this policy applies to: `Include all accounts under my AWS organization`
      2. Resource type: Select all resources
      3. Resources: `Include all resources that match the selected resource type`
3. Click the `Next` button.
4. In the *Configure policy tags* section, click the `Next` button.
5. In the *Review and create policy* section, click the `Create policy` button.

![ec2-security-groups](/images/3/0008.png?featherlight=false&width=90pc)

{{% notice note %}}
For the configuration of **FMS-Default-Public-Access-Apps-Denied**, this policy determines which resources are open only and allows the source range to be [Private IP Ranges](https:// tools.ietf.org/html/rfc1918) access to.
{{% /notice %}}

![ec2-security-groups](/images/3/0009.png?featherlight=false&width=90pc)

{{% notice note %}}
Upon successful initialization, identifying the associated AWS accounts and resources will take approximately 5-10 minutes.
{{% /notice %}}

#### Conduct survey and evaluation
There are basically two approaches that allow you to examine the results that come from Firewall Manager's policies:
1. Through the AWS Firewall Manager service
2. Through the service [AWS Security Hub](https://aws.amazon.com/security-hub/)

{{% notice note %}}
By default, when the AWS Security Hub service has been set up, Firewall Manager results are automatically sent here.
{{% /notice %}}

After we have successfully initialized the policy and resource definition process, we will perform the survey process.

![firewall-manager-new-policy-accounts-status](/images/3-firewall-manager-new-policy-accounts-status.png)

From the image above, we can easily see that there are 2 AWS accounts defined. When looking at more details, such as which accounts are not properly configured, or which accounts have infringing resources are communicated here.

![firewall-manager-new-policy-account-status](/images/3-firewall-manager-new-policy-account-status.png)

We will now look at the account with the offending resources. The offending resources will be displayed in turn in the **Noncompliant resources in scope** section.

![firewall-manager-new-policy-account-resources](/images/3-firewall-manager-new-policy-account-resources.png)

We will select the resource as a Security Group as shown and see why this resource violates the policy for the reason described as `RESOURCE_VIOLATES_AUDIT_SECURITY_GROUP`?

![firewall-manager-new-policy-account-resource-details](/images/3-firewall-manager-new-policy-account-resource-details.png)

Regarding the *Referenced rule* specification, we can easily define a rule with the source CIDR range that allows access to `0.0.0.0/0` over the SSH protocol.

About how to solve *Remediation action*, the Firewall Manager suggests that we should remove this rule.

#### Perform the Remediation process
When configuring a policy, we can choose between only receiving **Alert** notifications and automatically **Remediation**.

After the evaluation is done, if we want to perform **Remediation** automatically, in the *Policy Action* section, we need to configure `Auto remediate non-compliant resources`.

1. In the **Policy details** bar, we drag to the *Policy Action* section.

![firewall-manager-new-policy-details](/images/3-firewall-manager-new-policy-details.png)

2. Press the `Edit` button.
3. Select `Auto remediate any noncompliant resources`.

![firewall-manager-new-policy-details-edit](/images/3-firewall-manager-new-policy-details-edit.png)

4. Press the `Save` button.
5. After successful editing, under *Automatic remediation*, the status will be displayed as `Enabled`.

![firewall-manager-new-policy-details-with-remediation](/images/3-firewall-manager-new-policy-details-with-remediation.png)

After a while, the status of each AWS account will be updated, if there aren't any offending resources, the status will be `Compliant`.

![firewall-manager-new-policy-after-remediation](/images/3-firewall-manager-new-policy-after-remediation.png)

We can verify by checking the **Inbound Rules** section of the Security Group for violations.

![ec2-security-group-after-remediation](/images/3-ec2-security-group-after-remediation.png)

The security team can then rest assured that their systems will be actively monitored and compliant with security policies through the **Firewall Manager** service.

{{% notice note %}}
Firewall Manager will automate the **Remediation** process for all offending resources through the [Service-linked Role](https://docs.aws.amazon.com/waf/latest/developerguide/fms-using-service-linked-roles.html).
{{% /notice %}}