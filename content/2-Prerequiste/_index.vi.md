---
title : "Các bước chuẩn bị"
date : "2025-09-04"
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

Trang này cung cấp tổng quan về các bước cần thiết để triển khai dự án ứng dụng thời tiết lên môi trường AWS. Mỗi phần trong danh sách dưới đây sẽ dẫn bạn đến một trang hướng dẫn chi tiết.

{{% notice info %}}
Hãy tuần tự thực hiện các bước để đảm bảo hạ tầng được thiết lập một cách chính xác.
{{% /notice %}}

### Nội dung chính của workshop

1.  **Cấu hình Môi trường Mạng (VPC)**
    *   [Tạo VPC](2.1-createvpc/)
    *   [Chỉnh sửa Public Subnet](2.2-modifysubnet/)

2.  **Thiết lập các Lớp Bảo mật (Security Group)**
    *   [Tạo Security Group cho các thành phần](2.3-createsg/)

3.  **Khởi tạo Máy chủ Ảo (EC2)**
    *   [Tạo EC2 Instance cho Bastion Host và Backend](2.4-createec2/)

4.  **Chuẩn bị cho việc Mở rộng (AMI & Launch Template)**
    *   [Tạo Amazon Machine Image (AMI)](2.5-createami/)
    *   [Tạo Launch Template cho Auto Scaling](2.6-createlaunchtemplate/)

5.  **Thiết lập Cân bằng tải (Load Balancer & Target Group)**
    *   [Tạo Target Group](2.7-createtargetgroup/)
    *   [Tạo Application Load Balancer (ALB)](2.8-createalb/)

6.  **Cấu hình Tự động Mở rộng (Auto Scaling Group)**
    *   [Tạo Auto Scaling Group](2.9-createasg/)

7.  **Lưu trữ Frontend trên S3**
    *   [Tạo S3 Bucket và cấu hình Static Website Hosting](2.11-creates3/)

8.  **Tăng tốc và Phân phối Nội dung (CloudFront)**
    *   [Tạo CloudFront cho Backend (ALB)](2.10-createcloudfrontbe/)
    *   [Tạo CloudFront cho Frontend (S3)](2.12-createcloudfrontfe/)