---
title : "Quản lý Security Group"
date: 2024-01-01
weight : 4
chapter : false
pre : " <b> 4. </b> "
---


**Quản lý Security Groups với Firewall Manager**

Thông thường, việc truy cập từ xa tới hạ tầng ứng dụng thông qua các giao thức quen thuộc như RDP, SSH hay SMB thường ít được chú ý và để mở truy cập lỏng lẻo ra ngoài Internet. Khi đó, bất kỳ tổ chức hay cá nhân nào cũng có thể dò tìm bằng cách sử dụng các công cụ dò tìm tự động. Đây là một vấn đề cấp thiết và cần có giải pháp khắc phục ngay lập tức.

Trong nhiều trường hợp, tác nhân phổ biến nhất là việc cấu hình tạm thời từ phía người quản trị hệ thống nhằm phục vụ cho các hoạt động thử nghiệm. Giả sử cấu hình này tiếp tục được giữ nguyên sau đó, chúng ta có thể thấy được rằng các cơ hội tấn công tiềm tàng từ các mối nguy hại đến từ Internet là vô cùng lớn.

Để có thể giải quyết các mối nguy được kể đến ở trên, chúng ta sẽ tiến hành thiết lập dịch vụ **Firewall Manager** để có thể rà soát và giới hạn các Security Groups mà được xác định là mối nguy hại tiềm tàng qua đó để lộ các lỗ hổng hạ tầng ứng dụng trước các mối đe doạ từ bên ngoài.

![firewall-manager-security-group-policies](/images/3-firewall-manager-security-group-policies.jpeg?featherlight=false&width=60pc)

{{% notice tip %}}
Tìm hiểu thêm về [Firewall Manager's Security Group Policies](https://docs.aws.amazon.com/waf/latest/developerguide/security-group-policies.html)
{{% /notice %}}

**Nội dung**
- [Thiết lập chính sách](#thiết-lập-chính-sách)
- [Tiến hành khảo sát và đánh giá](#tiến-hành-khảo-sát-và-đánh-giá)
- [Thực hiện quá trình Remediation](#thực-hiện-quá-trình-remediation)

#### Thiết lập chính sách
Giả định là chúng ta sẽ tiến hành xác định địa chỉ *Public IP* nào được phép truy cập đến hạ tầng ứng dụng, sau đó thực hiện quá trình đánh giá và sau cùng tự động hoá quá trình **Remediation**.

1. Truy cập vào dịch vụ [Firewall Manager](https://console.aws.amazon.com/wafv2/fms) ở tài khoản **Firewall Manager Administrator**.

![ec2-security-groups](/images/3/0001.png?featherlight=false&width=90pc)

2. Ở thanh điều hướng bên tay trái, chọn **Security policies**.
3. Nhấn nút `Create policy`.
4. Ở phần *Choose policy type and Region*
   1. Policy Type: `Security Group`
   2. Security group policy type: `Auditing and enforcement of security group rules`


![ec2-security-groups](/images/3/0002.png?featherlight=false&width=90pc)

5. Ở phần *Describe policy*
   1. Policy name: Nhập tên chính sách

![ec2-security-groups](/images/3/0003.png?featherlight=false&width=90pc)

   2. Policy rule options: `Configure managed audit policy rules`
   3. Policy rules
      1. Security group rules to audit: `Inbound Rules`
      2. Audit high risk applications: `Applications that can use public CIDR ranges`

![ec2-security-groups](/images/3/0004.png?featherlight=false&width=90pc)

   4. Nhấn nút `Add Application List`.
   5. Chọn **FMS-Default-Public-Access-Apps-Denied**.

![ec2-security-groups](/images/3/0005.png?featherlight=false&width=90pc)

   6. Policy action: `Identify resources that don’t comply with the policy rules, but don’t auto remediate`.

![ec2-security-groups](/images/3/0006.png?featherlight=false&width=90pc)

1. Nhấn nút `Next`.
2. Ở phần *Define policy scope*

![ec2-security-groups](/images/3/0007.png?featherlight=false&width=90pc)

   1. Policy scope:
      1. AWS accounts this policy applies to: `Include all accounts under my AWS organization`
      2. Resource type: Chọn tất cả tài nguyên
      3. Resources: `Include all resources that match the selected resource type`
3. Nhấn nút `Next`.
4.  Ở phần *Configure policy tags*, nhấn nút `Next`.
5.  Ở phần *Review and create policy*, nhấn nút `Create policy`.

![ec2-security-groups](/images/3/0008.png?featherlight=false&width=90pc)

{{% notice note %}}
Đối với cấu hình của **FMS-Default-Public-Access-Apps-Denied**, chính sách này sẽ xác định xem tài nguyên nào đang chỉ mở và cho phép dãy nguồn là [Private IP Ranges](https://tools.ietf.org/html/rfc1918) truy cập đến.
{{% /notice %}}

![ec2-security-groups](/images/3/0009.png?featherlight=false&width=90pc)

{{% notice note %}}
Sau khi khởi tạo thành công, quá trình xác định các tài nguyên và tài khoản AWS liên quan sẽ mất khoảng 5-10 phút.
{{% /notice %}}

#### Tiến hành khảo sát và đánh giá
Về cơ bản, có 2 cách tiếp cận cho phép bạn khảo sát kết quả đến từ các chính sách của Firewall Manager:
1. Thông qua dịch vụ AWS Firewall Manager
2. Thông qua dịch vụ [AWS Security Hub](https://aws.amazon.com/security-hub/)

{{% notice note %}}
Mặc định, khi dịch vụ AWS Security Hub đã được thiết lập, các kết quả của Firewall Manager sẽ tự động được gửi về đây.
{{% /notice %}}

Sau khi đã thành công khởi tạo chính sách và quá trình xác định tài nguyên, chúng ta sẽ thực hiện quá trình khảo sát.

![firewall-manager-new-policy-accounts-status](/images/3-firewall-manager-new-policy-accounts-status.png)

Từ hình trên, chúng ta dễ dàng nhận thấy có 2 tài khoản AWS được xác định. Khi xem chi tiết hơn, chẳng hạn như tài khoản nào chưa được cấu hình đúng, hay tài khoản nào có tài nguyên vi phạm đều được thông tin rõ ràng ở đây.

![firewall-manager-new-policy-account-status](/images/3-firewall-manager-new-policy-account-status.png)

Bây giờ chúng ta sẽ xem xét tài khoản có các tài nguyên vi phạm. Các tài nguyên vi phạm sẽ được hiển thị lần lượt ở mục **Noncompliant resources in scope**.

![firewall-manager-new-policy-account-resources](/images/3-firewall-manager-new-policy-account-resources.png)

Chúng ta sẽ chọn tài nguyên là một Security Group như hình và đánh giá xem tại sao tài nguyên này vi phạm chính sách khi ở lí do được tả là `RESOURCE_VIOLATES_AUDIT_SECURITY_GROUP`?

![firewall-manager-new-policy-account-resource-details](/images/3-firewall-manager-new-policy-account-resource-details.png)

Về đặc tả *Referenced rule*, chúng ta dễ dàng xác định được một quy định được với dãy CIDR nguồn cho phép truy cập là `0.0.0.0/0` thông qua giao thức SSH.

Về cách thức giải quyết *Remediation action*, Firewall Manager đề xuất cho chúng ta là nên loại bỏ quy định này.

#### Thực hiện quá trình Remediation
Về cơ bản, khi cấu hình một chính sách, chúng ta có thể chọn lựa giữa chỉ nhận các thông báo **Alert** và tự động **Remediation**.  

Sau khi đánh giá xong, nếu như chúng ta muốn thực hiện quá trình **Remediation** một cách tự động, ở phần *Policy Action*, chúng ta cần phải cấu hình `Auto remediate non-compliant resources`.

1. Ở thanh **Policy details**, chúng ta kéo đến phần *Policy Action*.

![firewall-manager-new-policy-details](/images/3-firewall-manager-new-policy-details.png)

2. Nhấn nút `Edit`.
3. Chọn `Auto remediate any noncompliant resources`.

![firewall-manager-new-policy-details-edit](/images/3-firewall-manager-new-policy-details-edit.png)

4. Nhấn nút `Save`.
5. Sau khi chỉnh sửa thành công, ở mục *Automatic remediation*, trạng thái sẽ hiển thị là `Enabled`.

![firewall-manager-new-policy-details-with-remediation](/images/3-firewall-manager-new-policy-details-with-remediation.png)

Sau một lúc, trạng thái của từng tài khoản AWS sẽ được cập nhật, nếu không có bất kỳ tài nguyên vi phạm nào, trạng thái sẽ là `Compliant`.

![firewall-manager-new-policy-after-remediation](/images/3-firewall-manager-new-policy-after-remediation.png)

Chúng ta có thể kiểm chứng bằng cách kiểm tra mục **Inbound Rules** của Security Group vi phạm.

![ec2-security-group-after-remediation](/images/3-ec2-security-group-after-remediation.png)

Khi đó, đội ngũ bảo mật có thể an tâm rằng hệ thống của họ sẽ được chủ động giám sát và tuân thủ các chính sách bảo mật thông qua dịch vụ **Firewall Manager**.

{{% notice note %}}
Firewall Manager sẽ tự động hoá quá trình **Remediation** cho tất cả các tài nguyên vi phạm thông qua [Service-linked Role](https://docs.aws.amazon.com/waf/latest/developerguide/fms-using-service-linked-roles.html).
{{% /notice %}}
