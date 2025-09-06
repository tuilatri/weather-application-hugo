---
title : "Tạo CloudFront cho Backend (ALB)"
date : "2025-09-04"
weight : 10
chapter : false
pre : " <b> 2.10 </b> "
---

### Tạo Distribution CloudFront cho Backend

ℹ️ **Mục tiêu**

*   **Amazon CloudFront** là một dịch vụ mạng phân phối nội dung (CDN) giúp tăng tốc độ phân phối nội dung web (như API, video, dữ liệu) đến người dùng cuối với độ trễ thấp và tốc độ truyền tải cao.
*   Trong kiến trúc này, CloudFront sẽ đóng vai trò là "cổng vào" công khai cho backend API của chúng ta, thay vì truy cập trực tiếp vào ALB.
*   **Lợi ích chính:**
    *   **HTTPS Termination:** CloudFront sẽ xử lý kết nối HTTPS từ người dùng. Sau đó, nó sẽ giao tiếp với ALB qua HTTP trong mạng nội bộ của AWS. Điều này giúp đơn giản hóa việc quản lý SSL/TLS, vì chúng ta không cần cấu hình chứng chỉ trên ALB hoặc EC2 instance.
    *   **Tăng hiệu suất và giảm độ trễ:** CloudFront đưa API của bạn đến gần người dùng hơn thông qua các điểm hiện diện (Edge Location) trên toàn cầu.
    *   **Tăng cường bảo mật:** Cung cấp một lớp bảo vệ bổ sung, che giấu ALB và có thể tích hợp với AWS WAF (Web Application Firewall).

---

🔒 **Các bước thực hiện**

#### **1. Bắt đầu tạo Distribution**

*   Từ **AWS Management Console**, tìm kiếm và chọn dịch vụ **CloudFront**.
*   Nhấn vào **Create a CloudFront distribution**.

{{< figure src="/images/2.prerequisite/2.10-createcloudfrontbe/create-cf-button.png" title="Bắt đầu tạo CloudFront Distribution" >}}

#### **2. Cấu hình Origin (Nguồn gốc)**

*   **Origin domain:** Nhấn vào ô và chọn DNS name của Application Load Balancer `project-backend-alb` từ danh sách thả xuống.
*   **Protocol:** Chọn `HTTP only`.
*   **HTTP port:** Để mặc định là `80`.

{{< figure src="/images/2.prerequisite/2.10-createcloudfrontbe/cf-origin-config.png" title="Cấu hình Origin là Application Load Balancer" >}}

#### **3. Cấu hình Default Cache Behavior**

*   Đây là phần cấu hình quan trọng nhất để đảm bảo API hoạt động đúng cách.
*   **Viewer protocol policy:** Chọn `Redirect HTTP to HTTPS` để tự động chuyển hướng người dùng sang kết nối an toàn.
*   **Allowed HTTP methods:** Chọn `GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE` để cho phép tất cả các phương thức API.
*   **Cache policy:** Chọn `CachingDisabled`. Điều này rất quan trọng vì chúng ta không muốn CloudFront lưu cache các phản hồi của API (vốn có tính động).
*   **Origin request policy - optional:** Chọn `AllViewer`. Chính sách này sẽ chuyển tiếp tất cả thông tin từ người dùng (headers, query strings, cookies) đến ALB, đảm bảo backend nhận được đầy đủ dữ liệu cần thiết để xử lý yêu cầu.

{{< figure src="/images/2.prerequisite/2.10-createcloudfrontbe/cf-behavior-config.png" title="Cấu hình Cache Behavior cho API" >}}

#### **4. Cấu hình các thiết lập khác**

*   **Web Application Firewall (WAF):** Chọn `Do not enable security protections` cho mục đích của workshop này.

#### **5. Hoàn tất và kiểm tra**

*   Cuộn xuống cuối trang và nhấn **Create distribution**.
*   Quá trình triển khai một distribution mới ra toàn bộ mạng lưới của CloudFront có thể mất từ 5 đến 15 phút. Bạn sẽ thấy trạng thái `Deploying`.

{{< figure src="/images/2.prerequisite/2.10-createcloudfrontbe/cf-deploying.png" title="Chờ CloudFront hoàn tất triển khai" >}}

*   Khi trạng thái chuyển sang `Enabled` (hiển thị ngày Last modified), hãy sao chép **Distribution domain name** (ví dụ: `d12345abcdef.cloudfront.net`).

{{< figure src="/images/2.prerequisite/2.10-createcloudfrontbe/cf-copy-domain.png" title="Sao chép Domain Name của Distribution" >}}

*   Sau khi bạn kết nối với Backend và đăng tải dự án lên thì sẽ thấy được thấy được kết quả của bước sau
*   Dán domain name này vào trình duyệt. Kết quả trả về phải giống hệt như khi bạn truy cập qua DNS của ALB: `Backend is running!`.