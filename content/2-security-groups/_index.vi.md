---
title : "Security Groups"
date: 2024-01-01
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

**Làm quen với Security Groups**

**Security Groups** là một công cụ vô cùng mạnh mẽ được tạo ra nhằm quản lý truy cập mạng tới những tài nguyên của AWS Cloud, đặc biệt là những **EC2 instances** qua đó thi hành việc bảo mật mạng ở tầng L3/4 những đối với **EC2 interfaces**.

#### Kiến thức cơ bản
Những điều cơ bản chúng ta nên biết khi tiến hành cấu hình:
- Mặc định sẽ có 2 loại quy định là **inbound** và **outbound**.
- Mặc định sẽ không có bất kỳ **inbound rules** nào cho phép truy cập ở chiều đi đến.
- Không thể tạo quy định với quy luật phủ nhận.
- Khi liên kết với một **EC2 instance** sẽ đóng vai trò là một `Host-based Firewall`.
- Một **EC2 instance** có thể được liên kết tối đa 5 Security Group.
- Không thể liên kết với một **VPC** hay **VPC Subnet**.
- Đối với một quy định, nguồn xuất phát có thể là:
  - IP Address
  - VPC Subnets CIDR
  - Security Group ID
- Có thể liên kết nhiều **EC2 instance**.

{{% notice tip %}}
Chúng ta có thể tham khảo thêm về [Security Group Rules](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html#SecurityGroupRules).
{{% /notice %}}

**Nội dung**
- [Kiến thức cơ bản](#kiến-thức-cơ-bản)
- [Khởi tạo Security Group](#khởi-tạo-security-group)
- [Best Practices](#best-practices)
- [Kích hoạt dịch vụ AWS Config](#kích-hoạt-dịch-vụ-aws-config)

#### Khởi tạo Security Group
1. Truy cập vào [Amazon EC2 console](https://console.aws.amazon.com/ec2/).
2. Ở thanh điều hướng bên tay trái, chọn **Security Groups**.

![ec2-security-groups](/images/1/0001.png?featherlight=false&width=90pc)

3. Chọn nút `Create security group`.

![ec2-security-groups](/images/1/0002.png?featherlight=false&width=90pc)

4. Ở phần *Basic details*, chúng ta cần nhập tên, miêu tả và chọn **VPC**.

![ec2-security-groups](/images/1/0003.png?featherlight=false&width=90pc)

5. Ở phần *Inbound rules*, nhấn nút `Add rule` để tiến hành thêm như sau:

![ec2-security-groups](/images/1/0004.png?featherlight=false&width=90pc)

6. Ở phần *Onbound rules*, chúng ta sẽ giữ mặc định.

![ec2-security-groups](/images/1/0005.png?featherlight=false&width=90pc)

7. Ở phần *Tags*, nhấn nút `Add new tag` để thêm các Tags cụ thể.
8. Nhấn nút `Create security group` để khởi tạo.

{{% notice note %}}
Ngoài ra, chúng ta có thể khởi tạo thông qua [AWS CLI](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-security-group.html).
{{% /notice %}}

#### Best Practices
**AWS** đề xuất một số tiêu chuẩn khi chúng ta tiến hành sử dụng **Security Groups**. Việc đảm bảo các tiêu chuẩn này trong một hệ thống lớn cũng như nhiều môi trường trên AWS Cloud luôn luôn là thách thức, bởi quá trình triển khai ứng dụng mới ngày càng nhanh chóng và thường xuyên hơn.

---
1. **Xoá các security groups không được sử dụng**

Nếu một lượng lớn không sử dụng security groups vẫn còn tồn tại, điều này có thể gây khó khăn cho các Administrators trong việc quản lý và tìm kiếm, quá trình cấu hình có thể dẫn tới lỗi bởi một số rule bị trùng lặp.

| Thành phần | Chi tiết |
| --- | --- |
| Tài nguyên AWS | Amazon EC2 security group |
| AWS Config Managed Rule | [ec2-security-group-attached-to-eni](https://docs.aws.amazon.com/config/latest/developerguide/ec2-security-group-attached-to-eni.html) |
| Tiêu chí trong PCI DSS | [PCI.EC2.3](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-pci-controls.html#pcidss-ec2-3) |
| Mức độ trong PCI DSS | **LOW** |

---
2. **Giới hạn chỉnh sửa**

Việc chỉnh sửa Security Group là tối quan trọng, chỉ một sai lầm nhỏ có thể gây ra ảnh hưởng đến cả một hệ thống. Khi đó, chúng ta tiến hành giới hạn thông qua dịch vụ **IAM** và chỉ có một số *IAM Roles* được phép thực hiện các hoạt động chỉnh sửa.

| Thành phần | Chi tiết |
| --- | --- |
| Tài nguyên AWS | Amazon EC2 security group |
| AWS Config Managed Rule | [iam-policy-no-statements-with-admin-access](https://docs.aws.amazon.com/config/latest/developerguide/iam-policy-no-statements-with-admin-access.html) |
| Tiêu chí trong PCI DSS | [PCI.IAM.3](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-pci-controls.html#pcidss-iam-3) |
| Mức độ trong PCI DSS | **HIGH** |

---
3. **Theo dõi sự tạo mới và xoá bỏ**

Đây luôn luôn là một trong những tiêu chí cơ bản nhất nhằm theo dõi những hoạt động liên quan đến việc tạo mới hay xoá bỏ các Security Group.

| Thành phần | Chi tiết |
| --- | --- |
| Tài nguyên AWS | Amazon EC2 security group |
| AWS Config Managed Rule | N/A |
| Tiêu chí trong CIS Benchmark | [3.10](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html#securityhub-cis-controls-3.10) |
| Mức độ trong CIS Benchmark | **LOW** |

---
4. **Đừng bỏ quên việc giới hạn Outbound**

Chúng ta nên luôn giới hạn truy cập ở mặt **Outbound** của một Security Group với những Subnets cụ thể.

| Thành phần | Chi tiết |
| --- | --- |
| Tài nguyên AWS | Amazon EC2 security group |
| AWS Config Managed Rule | [vpc-default-security-group-closed](https://docs.aws.amazon.com/config/latest/developerguide/vpc-default-security-group-closed.html) |
| Tiêu chí trong PCI DSS | [PCI.EC2.2](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-pci-controls.html#pcidss-ec2-2) |
| Mức độ trong PCI DSS | **MEDIUM** |

---
5. **Giới hạn dãy các Inbound Ports được phép truy cập**

Chỉ có những Ports cần thiết cho sự hoạt động của một ứng dụng mới được cân nhắc mở ở mặt **Inbound** của một Security Group. Với một lượng lớn Ports không cần thiết mà được mở ra, điều này có thể dẫn đến những lỗ hổng tiềm tàng và dễ dàng bị khai thác bởi những truy cập mang tính chất thăm dò cũng như tấn công.

| Thành phần | Chi tiết |
| --- | --- |
| Tài nguyên AWS | Amazon EC2 security group |
| AWS Config Managed Rule | [restricted-ssh](https://docs.aws.amazon.com/config/latest/developerguide/restricted-ssh.html), [restricted-common-ports](https://docs.aws.amazon.com/config/latest/developerguide/restricted-common-ports.html) |
| Tiêu chí trong CIS Benchmark | [4.1](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html#securityhub-cis-controls-4.1), [4.2](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html#securityhub-cis-controls-4.2) |
| Mức độ trong CIS Benchmark | **HIGH** |
| Tiêu chí trong PCI DSS | [PCI.EC2.5](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-pci-controls.html#pcidss-ec2-5) |
| Mức độ trong PCI DSS | **HIGH** |

#### Kích hoạt dịch vụ AWS Config
Các tổ chức, doanh nghiệp nên cấp thiết giải quyết những thách thức đó thông qua việc tự động hoá các tác vụ kiểm soát và theo dõi. Với dịch vụ **AWS Config**, chúng ta có thể cấu hình một số qui định nhằm tự động hoá quá trình phát hiện các vi phạm tới hệ thống hiện tại.

{{% notice info %}}
Việc kích hoạt dịch vụ **Config** sẽ là tiền đề cho phần tiếp theo của bài thực hành.
{{% /notice %}}

Sau đây là một số bước để có thể cấu hình một **Managed Rule** cơ bản.
1. Truy cập vào dịch vụ [Config](https://console.aws.amazon.com/config/)
2. Nếu là lần đầu tiên truy cập vào dịch vụ, chúng nhấn nút `Get Started`.
3. Ở phần *Settings*, giữ nguyên cấu hình mặc định như sau:

![ec2-security-groups](/images/1/0006.png?featherlight=false&width=90pc)

4. Ở phần *Rules*, chúng ta quan sát được một danh sách các **Managed Rules**.

![ec2-security-groups](/images/1/0007.png?featherlight=false&width=90pc)

5. Ở ô tìm kiếm, chúng ta nhập `restricted-ssh` và chọn qui định tương ứng.

![ec2-security-groups](/images/1/0008.png?featherlight=false&width=90pc)

6. Tiến hành khởi tạo.

![ec2-security-groups](/images/1/0009.png?featherlight=false&width=90pc)
