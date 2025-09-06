---
title : "Tạo Target Group"
date : "2025-09-04"
weight : 7
chapter : false
pre : " <b> 2.7 </b> "
---

### Khởi tạo Target Group

ℹ️ **Mục tiêu**

*   **Target Group** (Nhóm mục tiêu) được sử dụng để gom nhóm các tài nguyên (trong trường hợp này là các EC2 instance) mà Application Load Balancer (ALB) sẽ chuyển hướng lưu lượng truy cập đến.
*   ALB cũng sử dụng Target Group để thực hiện kiểm tra sức khỏe (Health Check), đảm bảo rằng nó chỉ gửi yêu cầu đến các instance đang hoạt động bình thường.
*   Chúng ta sẽ tạo một Target Group để quản lý các máy chủ backend của dự án.

---

🔒 **Các bước thực hiện**

#### **1. Truy cập Target Groups**

*   Từ **EC2 Dashboard**, ở menu bên trái, cuộn xuống mục **Load Balancing** và chọn **Target Groups**.
*   Nhấn vào **Create target group**.

{{< figure src="/images/2.prerequisite/2.7-createtargetgroup/create-tg-button.png" title="Bắt đầu tạo Target Group" >}}

#### **2. Chọn loại Target**

*   Trong bước **Choose a target type**, chọn **Instances** vì chúng ta muốn Load Balancer chuyển hướng traffic đến các EC2 instance.
*   Nhấn **Next**.

{{< figure src="/images/2.prerequisite/2.7-createtargetgroup/tg-choose-type.png" title="Chọn Target Type là Instances" >}}

#### **3. Cấu hình chi tiết cho Target Group**

*   **Target group name:** `project-backend-target-group`
*   **Protocol - Port:** Chọn `HTTP` và nhập `3000`. Đây là port mà ứng dụng Node.js backend của chúng ta đang lắng nghe.
*   **VPC:** Chọn `project-vpc`.
*   **Health checks (Kiểm tra sức khỏe):**
    *   **Health check protocol:** `HTTP`
    *   **Health check path:** `/health`. Đây là đường dẫn mà Load Balancer sẽ truy cập để kiểm tra xem máy chủ có đang hoạt động tốt không (liên quan đến code backend).

{{< figure src="/images/2.prerequisite/2.7-createtargetgroup/tg-basic-config-01.png" >}}
{{< figure src="/images/2.prerequisite/2.7-createtargetgroup/tg-basic-config-02.png" >}}
{{< figure src="/images/2.prerequisite/2.7-createtargetgroup/tg-basic-config-03.png" title="Điền thông tin cấu hình cho Target Group" >}}

#### **4. Đăng ký Targets ban đầu**

*   Ở bước này, chúng ta sẽ đăng ký instance backend đã tạo thủ công vào Target Group.
*   Trong bảng **Available instances**, chọn `project-backend-ec2`.
*   Nhấn vào **Include as pending below**. Thao tác này sẽ đưa instance vào danh sách chờ để được đăng ký vào group.

{{% notice info %}}
Việc đăng ký ít nhất một instance ban đầu giúp chúng ta kiểm tra xem Load Balancer và Target Group có hoạt động chính xác hay không sau khi cấu hình. Các instance sau này sẽ được Auto Scaling Group tự động đăng ký.
{{% /notice %}}

{{< figure src="/images/2.prerequisite/2.7-createtargetgroup/tg-register-targets.png" title="Đăng ký instance vào Target Group" >}}

#### **5. Hoàn tất và xem lại**

*   Cuộn xuống dưới và nhấn **Create target group**.
*   Sau khi tạo thành công, bạn sẽ được đưa đến trang chi tiết của Target Group. Ban đầu, trạng thái health status của instance có thể là `unhealthy` hoặc `initial`. Hãy đợi một vài phút để Load Balancer thực hiện health check, trạng thái sẽ chuyển sang `healthy`.

{{< figure src="/images/2.prerequisite/2.7-createtargetgroup/tg-success.png" title="Tạo Target Group thành công" >}}