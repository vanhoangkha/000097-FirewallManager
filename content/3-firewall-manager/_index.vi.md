---
title : "AWS Firewall Manager"
date: 2024-01-01
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

**Làm quen với dịch vụ AWS Firewall Manager**

**Firewall Manager** là dịch vụ quản lý bảo mật cho phép chúng ta cấu hình những **Firewall rules** trên những ứng dụng và tài khoản AWS mà nằm trong phạm vi **AWS Organization** của một tổ chức.

Khi một ứng dụng được triển khai, các qui định bảo mật sẽ được thiết lập bởi **Firewall Manager**, nhằm áp đặt và bảo vệ dựa trên những quy tắc cơ sở. Qua đó, chắc chắn rằng những tài nguyên (ví dụ như **Security Group**) vi phạm sẽ được rà soát và loại bỏ một cách tự động.

Đây là một dịch vụ trung tâm bao gồm các tính năng như tạo ra các chính sách bảo mật, áp đặt các chính sách đó cũng như tự động rà soát các tài nguyên của một hệ thống lớn một cách xuyên suốt.

Các khả năng mà **Firewall Manager** cung cấp cho Security Group được phân làm 3 loại chính:
1. Khởi tạo và áp dụng bảo mật cơ sở cho Security Group.
2. Rà soát và thu dọn các Security Group trùng lặp và không sử dụng.
3. Xác định bất kỳ Security Group định nghĩa quy định quá mở và có rủi ro cao.

![firewall-manager-architecture](/images/2-firewall-manager-architecture.png?featherlight=false&width=60pc)

**Nội dung**
- [Điều kiện tiên quyết](#điều-kiện-tiên-quyết)
- [Thiết lập AWS Organizations](#thiết-lập-aws-organizations)
- [Cấu hình tài khoản Firewall Manager Administrator](#cấu-hình-tài-khoản-firewall-manager-administrator)
- [Kích hoạt dịch vụ AWS Config](#kích-hoạt-dịch-vụ-aws-config)
- [Kích hoạt tính năng chia sẻ tài nguyên](#kích-hoạt-tính-năng-chia-sẻ-tài-nguyên)

#### Điều kiện tiên quyết
Để có thể chuẩn bị và kích hoạt **AWS Firewall Manager**, đối với lần đầu tiên sử dụng dịch vụ này, chúng ta cần phải lần lượt thực hiện các bước sau:
1. Thiết lập [AWS Organizations](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_introduction.html).
2. Cấu hình tài khoản **Firewall Manager Administrator**.
3. Kích hoạt dịch vụ **AWS Config**.
4. Kích hoạt tính năng chia sẻ tài nguyên (Đối với các chính sách *Network Firewall*)

{{% notice tip %}}
Tìm hiểu thêm về [AWS Firewall Manager Prerequisites](https://docs.aws.amazon.com/waf/latest/developerguide/fms-prereq.html).
{{% /notice %}}

#### Thiết lập AWS Organizations
Nếu như tài khoản AWS đã là một thành viên của **AWS Organization**, bạn có thể di chuyển tới bước kế tiếp. Nếu không, bạn cần phải tiến hành thiết lập **AWS Organization**.

![ec2-security-groups](/images/2/0001.png?featherlight=false&width=90pc)

{{% notice tip %}}
Tìm hiểu thêm về [Creating & Managing AWS Organizations](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_org.html).
{{% /notice %}}

#### Cấu hình tài khoản Firewall Manager Administrator
1. Truy cập vào AWS Console thông qua tài khoản IAM User có đầy đủ quyền hạn.
2. Truy cập vào dịch vụ [Firewall Manager](https://console.aws.amazon.com/wafv2/fms).

![ec2-security-groups](/images/2/0002.png?featherlight=false&width=90pc)

3. Nhấn nút `Get Started`.
4. Nhập ID của tài khoản AWS mà bạn muốn liên kết.

![ec2-security-groups](/images/2/0003.png?featherlight=false&width=90pc)

5. Nhấn nút `Set administrator account`.
6. Một khi cấu hình thành công, bạn sẽ nhận được thông báo tương ứng như sau.

![ec2-security-groups](/images/2/0004.png?featherlight=false&width=90pc)

#### Kích hoạt dịch vụ AWS Config
Ở phần trước, chúng ta đã kích hoạt dịch vụ **AWS Config** từ AWS Console. Tuy nhiên, chúng ta hoàn toàn có thể nhanh chóng kích hoạt thông qua việc khởi tạo một **CloudFormation Stacksets**.

| Thành Phần | Giá Trị (Bắt buộc) |
| ---------- | ------- |
| Stack Name | enable-aws-config |
| Template URL | [EnableAWSConfig.yml](https://s3.amazonaws.com/cloudformation-stackset-sample-templates-us-east-1/EnableAWSConfig.yml) |

#### Kích hoạt tính năng chia sẻ tài nguyên
Để có thể quản lý các chính sách *Network Firewall* xuyên suốt các tài khoản AWS, bạn cần phải kích hoạt tính năng chia sẻ tài nguyên với **AWS Organization** thông qua dịch vụ **AWS Resource Access Manager**.

1. Truy cập vào dịch vụ [Resource Access Manager](https://console.aws.amazon.com/ram).
2. Ở thanh điều hướng bên tay trái, chọn **Settings**.

![ec2-security-groups](/images/2/0005.png?featherlight=false&width=90pc)

3. Nhấn chọn `Enable sharing with AWS Organizations`.


![ec2-security-groups](/images/2/0006.png?featherlight=false&width=90pc)

4. Chọn nút `Save Settings`.


![ec2-security-groups](/images/2/0007.png?featherlight=false&width=90pc)

Ngoài ra, chúng ta có thể sử dụng [AWS CLI](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ram/enable-sharing-with-aws-organization.html) để tiến hành kích hoạt.

```bash
aws ram enable-sharing-with-aws-organization
```

{{% notice tip %}}
Tìm hiểu thêm về [Enable Sharing with AWS Organizations](https://docs.aws.amazon.com/ram/latest/userguide/getting-started-sharing.html#getting-started-sharing-orgs)
{{% /notice %}}
