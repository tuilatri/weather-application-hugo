---
title : "Giới thiệu và Tổng quan kiến trúc"
date : "2025-09-02" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---

### Tổng quan kiến trúc

Trong workshop này, chúng ta sẽ triển khai một ứng dụng dự báo thời tiết hoàn chỉnh lên môi trường AWS. Kiến trúc được thiết kế để đảm bảo tính sẵn sàng cao (High Availability), khả năng mở rộng (Scalability), và bảo mật (Security) bằng cách sử dụng các dịch vụ cốt lõi của AWS.

Dưới đây là sơ đồ kiến trúc tổng quan của dự án mà chúng ta sẽ xây dựng:

![Kiến trúc dự án](/images/1.introduction/weather-application-architecture.png) 

### Các dịch vụ AWS được sử dụng

Dưới đây là danh sách các dịch vụ chính và vai trò của chúng trong dự án này:

#### Mạng và Bảo mật

*   **Amazon VPC (Virtual Private Cloud)**: Là nền tảng mạng cho toàn bộ dự án. Chúng ta sử dụng VPC để tạo ra một môi trường mạng riêng biệt và cô lập trên AWS, cho phép chúng ta kiểm soát hoàn toàn các tài nguyên mạng, bao gồm dải địa chỉ IP, subnets, và route tables.

*   **Public & Private Subnets**:
    *   **Public Subnets**: Được sử dụng cho các tài nguyên cần truy cập trực tiếp từ Internet như Application Load Balancer và Bastion Host.
    *   **Private Subnets**: Dùng để chứa các tài nguyên nhạy cảm như máy chủ backend EC2, không cho phép truy cập trực tiếp từ Internet để tăng cường bảo mật.

*   **Security Groups**: Hoạt động như một tường lửa ảo cho các máy chủ EC2 để kiểm soát lưu lượng truy cập vào và ra. Trong dự án, chúng ta cấu hình các Security Group riêng biệt cho Bastion Host, máy chủ Backend, và Application Load Balancer để chỉ cho phép các kết nối cần thiết.

#### Tính toán và Mở rộng

*   **Amazon EC2 (Elastic Compute Cloud)**: Cung cấp các máy chủ ảo để chạy ứng dụng.
    *   **Backend Instances**: Các máy chủ EC2 nằm trong private subnet, chịu trách nhiệm chạy ứng dụng Node.js để xử lý logic và giao tiếp với API thời tiết.
    *   **Bastion Host**: Một máy chủ EC2 đặt trong public subnet, đóng vai trò là một cổng truy cập an toàn để các quản trị viên có thể SSH vào và quản lý các máy chủ backend trong private subnet.

*   **Application Load Balancer (ALB)**: Tự động phân phối lưu lượng truy cập từ người dùng đến nhiều máy chủ backend EC2. ALB giúp tăng tính sẵn sàng và khả năng chịu lỗi của ứng dụng. Nó cũng thực hiện kiểm tra sức khỏe (health checks) để đảm bảo chỉ gửi request đến các máy chủ đang hoạt động tốt.

*   **Auto Scaling Group (ASG)**: Tự động điều chỉnh số lượng máy chủ EC2 backend dựa trên tải thực tế. Khi lưu lượng truy cập tăng, ASG sẽ tự động thêm máy chủ mới và sẽ loại bỏ chúng khi lưu lượng giảm, giúp tối ưu chi phí và đảm bảo hiệu năng.

#### Lưu trữ và Phân phối nội dung

*   **Amazon S3 (Simple Storage Service)**: Là dịch vụ lưu trữ đối tượng có khả năng mở rộng cao. Chúng ta sử dụng S3 để lưu trữ toàn bộ mã nguồn frontend (HTML, CSS, JavaScript) và cấu hình nó để hoạt động như một trang web tĩnh.

*   **Amazon CloudFront**: Là một dịch vụ mạng phân phối nội dung (CDN) giúp tăng tốc độ tải trang web và API cho người dùng trên toàn cầu.
    *   **Frontend Distribution**: Phân phối nội dung tĩnh từ S3, cache tại các điểm biên (Edge Locations) gần người dùng nhất, đồng thời bảo mật S3 bucket bằng Origin Access Identity (OAI).
    *   **Backend Distribution**: Cung cấp một endpoint HTTPS an toàn cho API, chuyển tiếp các yêu cầu đến Application Load Balancer.