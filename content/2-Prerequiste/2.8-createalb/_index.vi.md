---
title : "Tạo Application Load Balancer (ALB)"
date : "2025-09-04"
weight : 8
chapter : false
pre : " <b> 2.8 </b> "
---

### Khởi tạo Application Load Balancer (ALB)

ℹ️ **Mục tiêu**

*   **Application Load Balancer (ALB)** đóng vai trò là điểm tiếp nhận và phân phối lưu lượng truy cập từ người dùng đến các máy chủ backend.
*   Bằng cách đặt ALB trong các Public Subnet trên nhiều Availability Zone (AZ), chúng ta đảm bảo ứng dụng có tính sẵn sàng cao (High Availability). Nếu một AZ gặp sự cố, ALB sẽ tự động chuyển hướng traffic đến các máy chủ ở AZ còn lại.
*   ALB sẽ lắng nghe các yêu cầu đến và chuyển tiếp chúng đến **Target Group** (`project-backend-target-group`) mà chúng ta đã tạo ở bước trước.

---

🔒 **Các bước thực hiện**

#### **1. Bắt đầu tạo Load Balancer**

*   Từ **EC2 Dashboard**, ở menu bên trái, cuộn xuống mục **Load Balancing** và chọn **Load Balancers**.
*   Nhấn vào **Create load balancer**.

{{< figure src="/images/2.prerequisite/2.8-createalb/create-lb-button.png" title="Bắt đầu tạo Load Balancer" >}}

#### **2. Chọn loại Load Balancer**

*   Có nhiều loại Load Balancer. Vì ứng dụng của chúng ta hoạt động trên lớp ứng dụng (HTTP/HTTPS), hãy chọn **Application Load Balancer** bằng cách nhấn **Create**.

{{< figure src="/images/2.prerequisite/2.8-createalb/choose-alb-type.png" title="Chọn Application Load Balancer" >}}

#### **3. Cấu hình thông tin cơ bản**

*   **Load balancer name:** `project-backend-alb`
*   **Scheme:** `Internet-facing` (Vì ALB này sẽ nhận traffic trực tiếp từ Internet).
*   **IP address type:** `IPv4`

{{< figure src="/images/2.prerequisite/2.8-createalb/alb-basic-config.png" title="Điền thông tin cấu hình cơ bản cho ALB" >}}

#### **4. Cấu hình Network Mapping**

*   Đây là bước quan trọng để đảm bảo ALB có thể truy cập được từ Internet.
*   **VPC:** Chọn `project-vpc`.
*   **Mappings:** Chọn cả hai Availability Zone mà chúng ta đang sử dụng. Với mỗi AZ, hãy chọn **Public Subnet** tương ứng.

{{< figure src="/images/2.prerequisite/2.8-createalb/alb-network-mapping.png" title="Chọn VPC và các Public Subnet" >}}

#### **5. Cấu hình Security Group**

*   Giữ Security Group `default` đang được chọn.
*   Trong danh sách thả xuống, chọn Security Group đã tạo riêng cho ALB: `project-alb-sg`.

{{< figure src="/images/2.prerequisite/2.8-createalb/alb-security-group.png" title="Chọn Security Group cho ALB" >}}

#### **6. Cấu hình Listeners and Routing**

*   Đây là nơi chúng ta định nghĩa cách ALB xử lý các yêu cầu đến.
*   **Listener:** Giữ nguyên `HTTP` và Port `80`.
*   **Default action:** Trong danh sách thả xuống, chọn `project-backend-target-group` đã tạo ở bước trước. Thao tác này sẽ ra lệnh cho ALB chuyển tiếp tất cả traffic từ port 80 đến Target Group này.

{{< figure src="/images/2.prerequisite/2.8-createalb/alb-listeners-routing.png" title="Cấu hình Listener và chuyển tiếp đến Target Group" >}}

#### **7. Hoàn tất, kiểm tra trạng thái và truy cập thử**

*   Kiểm tra lại các thông tin trong phần **Summary** và nhấn **Create load balancer**.
*   Sau khi tạo thành công, nhấn **View load balancer**.

{{% notice info %}}
**Lưu ý:** Quá trình khởi tạo Load Balancer sẽ mất khoảng 5-10 phút. Trạng thái của nó sẽ chuyển từ `provisioning` sang `active`. Hãy kiên nhẫn chờ đợi.
{{% /notice %}}

{{< figure src="/images/2.prerequisite/2.8-createalb/alb-status-provisioning.png" title="Chờ ALB chuyển sang trạng thái Active" >}}

*   Khi ALB đã `active`, chọn vào nó và sao chép **DNS name** trong tab **Details**.

{{< figure src="/images/2.prerequisite/2.8-createalb/alb-copy-dns.png" title="Sao chép DNS name của ALB" >}}

*   Sau khi bạn kết nối với Backend và đăng tải dự án lên thì sẽ thấy được thấy được kết quả của bước sau
*   Dán DNS name này vào trình duyệt. Nếu mọi thứ được cấu hình chính xác, bạn sẽ thấy thông báo: `Backend is running!`