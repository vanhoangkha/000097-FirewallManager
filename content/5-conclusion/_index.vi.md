---
title : "Kết Luận"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 5. </b> "
---


#### Kết Luận
Trong bài thực hành này, chúng ta đã cẩn thận xem xét qua trường hợp về thiết lập chính sách bảo mật của dịch vụ **Firewall Manager** đối với các tài nguyên **Security Groups** thông qua các giao thức truy cập từ xa như RDP, SSH hay SMB, điều mà đã được nhắc tới bởi AWS CISO **Stephen Schmidt** về chủ đề `Limit Security Groups`.

**Firewall Manager** giúp ta chúng ta liên tục có cái nhìn tổng quan bảo mật về hệ thống cũng như chủ động đối phó trước các tài nguyên vi phạm. Sau đây là tổng quan về trường hợp có thể xảy ra mà bạn phải đối mặt:
1. Xác định Security Groups trùng lặp.
2. Xác định Security Groups không được sử dụng sau N ngày.
3. Liệt kê các ứng dụng có thể bị truy cập từ Internet.
4. Từ chối các dãy CIDR nhất định.
5. Từ chối một số Port nhất định.
6. Từ chối giao thức `ALL` ở các Security Groups và yêu cầu giao thức phải được định nghĩa cụ thể.
7. Triển khai các **Pre-approved Security Groups** đã được kiểm duyệt và liên kết chúng với các tài nguyên một cách tự động.