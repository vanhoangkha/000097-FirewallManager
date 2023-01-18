---
title : "Liên tục Audit và Giới hạn Security Groups với AWS Firewall Manager"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
---


# Liên tục Audit và Giới hạn Security Groups với AWS Firewall Manager

## Tổng Quan
Nếu bạn đã và đang tìm hiểu đến quá trình bảo mật trên AWS Cloud, chủ đề về [10 thủ thuật đáng chú ý nhất về an toan và bảo mật thông tin trên AWS Cloud](https://aws.amazon.com/blogs/security/top-10-security-items-to-improve-in-your-aws-account/) đến từ AWS CISO **Stephan Schmidt** sẽ khiến bạn phải đặc biệt quan tâm.

![cloud-security-10-places](/images/1-cloud-security-10-places.png?width=40pc)

Trong đó, một trong những thủ thuật nền tảng và cơ bản về an ninh mạng đó là bảo mật truy cập từ xa đến các máy chủ hoặc dịch vụ. Trong môi trường on-premise, chúng ta đã quá quen thuộc khi nhắc đến thành phần **Firewall** hay những công nghệ có kĩ thuật tương tự nhằm giới hạn truy cập từ xa bằng cách chỉ cho phép một số địa chỉ mạng (IP Address), cổng giao tiếp (Port) và giao thức (Protocol) nhất định. Vậy chúng ta cũng tiếp tục triển khai một giải pháp tương tự trên AWS Cloud? Câu trả lời là *YES*.

Khi chúng ta dần dịch chuyển các workload hiện tại hoặc triển khai các tài nguyên mới trên AWS, chúng ta sẽ cần phải làm quen với các khái niệm như ***Security Groups** hay dịch vụ **AWS Network Firewall**.

Trong bài thực hành này, chúng ta sẽ làm quen một số kịch bản và tóm tắt lại từng bước để hiểu rõ hơn về dịch vụ **AWS Network Firewall** trong quá trình bảo mật truy cập từ xa thông qua 2 giao thức quen thuộc là *RDP* và *SSH*.
- Cấp độ: 300
- Thời lượng: 1-2 tiếng

#### Điều kiện cần:
  - IAM User (Admin) và AWS CLI

#### Các dịch vụ AWS được sử dụng:
  - Amazon EC2 Security Groups
  - AWS Config
  - AWS Organizations
  - AWS Firewall Manager
  - AWS Resource Access Manager

#### Nội dung

1. [Giới thiệu](1-intro/)
2. [Security Groups](2-security-groups/)
3. [Firewall](3-firewall-manager/)
4. [Quản lý Security Groups](4-firewall-manager-security-groups-policies/)
5. [Kết luận](5-conclusion/)