---
title : "Tạo Security Group cho các thành phần"
date : "2025-09-04" 
weight : 3 
chapter : false
pre : " <b> 2.3 </b> "
---

### Tạo Security Group trong Amazon VPC

ℹ️ **Mục tiêu**

*   **Security Group** hoạt động như một bức tường lửa ảo ở cấp độ instance để kiểm soát lưu lượng truy cập ra vào.
*   Trong phần này, chúng ta sẽ tạo ra 3 Security Group riêng biệt cho từng thành phần trong kiến trúc:
    1.  **Bastion Host:** Cho phép truy cập SSH an toàn từ bên ngoài.
    2.  **Backend:** Nhận lưu lượng từ Load Balancer và cho phép quản trị từ Bastion Host.
    3.  **Application Load Balancer (ALB):** Nhận lưu lượng truy cập công khai từ người dùng.

---

🔒 **Các bước thực hiện**

#### **1. Tạo Security Group cho Bastion Host (`project-bastion-host-sg`)**

Đây là lớp bảo vệ cho máy chủ Bastion Host, cho phép chúng ta kết nối vào từ Internet một cách an toàn.

*   **Bước 1: Bắt đầu tạo Security Group**
    *   Từ giao diện **VPC Dashboard**, chọn **Security Groups** ở menu bên trái.
    *   Nhấn vào **Create security group**.

    {{< figure src="/images/2.prerequisite/2.3-createsg/sg-create.png" title="Tạo Security Group mới" >}}

*   **Bước 2: Cấu hình thông tin cơ bản**
    *   **Security group name:** `project-bastion-host-sg`
    *   **Description:** `Cho phep truy cap vao Bastion Host`
    *   **VPC:** Chọn VPC `project-vpc` đã tạo ở bước trước.

    {{< figure src="/images/2.prerequisite/2.3-createsg/sg-bh-des.png"  title="Thông tin cơ bản của Bastion Host SG" >}}

*   **Bước 3: Thiết lập Inbound Rules (Luồng truy cập vào)**
    *   Nhấn **Add rule** và cấu hình các quy tắc sau:

| Type | Protocol | Port range | Source | Description |
| :--- | :--- | :--- | :--- | :--- |
| SSH | TCP | 22 | My IP | Cho phép bạn kết nối SSH từ IP hiện tại. |
| HTTP | TCP | 80 | Anywhere-IPv4 | Cho phép truy cập HTTP từ mọi nơi. |
| HTTPS | TCP | 443 | Anywhere-IPv4 | Cho phép truy cập HTTPS từ mọi nơi. |
| All ICMP-IPv4 | All | All | Anywhere-IPv4 | Cho phép `ping` để kiểm tra kết nối. |

    {{< figure src="/images/2.prerequisite/2.3-createsg/sg-bh-ib.png" title="Cấu hình Inbound Rules cho Bastion Host SG" >}}

*   **Bước 4:** Cuộn xuống và nhấn **Create security group**.

---

#### **2. Tạo Security Group cho Backend (`project-backend-sg`)**

Lớp bảo vệ này chỉ cho phép truy cập từ Application Load Balancer và Bastion Host, đảm bảo các máy chủ backend không bị truy cập trực tiếp từ Internet.

*   **Bước 1: Cấu hình thông tin cơ bản**
    *   Nhấn **Create security group** một lần nữa.
    *   **Security group name:** `project-backend-sg`
    *   **Description:** `Cho phep truy cap tu ALB va Bastion Host`
    *   **VPC:** Chọn VPC `project-vpc`.

    {{< figure src="/images/2.prerequisite/2.3-createsg/sg-be-des.png" title="Thông tin cơ bản của Backend SG" >}}

*   **Bước 2: Thiết lập Inbound Rules**
    *   Nhấn **Add rule** và cấu hình các quy tắc sau:

| Type | Protocol | Port range | Source | Description |
| :--- | :--- | :--- | :--- | :--- |
| Custom TCP | TCP | 3000 | Custom -> `project-alb-sg` | Cho phép ALB gửi traffic đến ứng dụng backend. |
| SSH | TCP | 22 | Custom -> `project-bastion-host-sg` | Cho phép kết nối SSH từ Bastion Host để quản trị. |
| All ICMP-IPv4 | All | All | Anywhere-IPv4 | Cho phép `ping` để kiểm tra kết nối. |

{{% notice info %}}
Khi chọn **Source**, bạn gõ `sg-` vào ô tìm kiếm, AWS sẽ hiển thị danh sách các Security Group có sẵn để bạn chọn.
{{% /notice %}}

    {{< figure src="/images/2.prerequisite/2.3-createsg/sg-be-ib.png" title="Cấu hình Inbound Rules cho Backend SG" >}}

*   **Bước 3:** Nhấn **Create security group**.

---

#### **3. Tạo Security Group cho Application Load Balancer (`project-alb-sg`)**

Lớp bảo vệ này cho phép người dùng từ Internet có thể truy cập vào ứng dụng của chúng ta thông qua ALB.

*   **Bước 1: Cấu hình thông tin cơ bản**
    *   Nhấn **Create security group**.
    *   **Security group name:** `project-alb-sg`
    *   **Description:** `Cho phep public traffic vao ALB`
    *   **VPC:** Chọn VPC `project-vpc`.

    {{< figure src="/images/2.prerequisite/2.3-createsg/sg-alb-des.png" title="Thông tin cơ bản của ALB SG" >}}

*   **Bước 2: Thiết lập Inbound Rules**
    *   Nhấn **Add rule** và cấu hình các quy tắc sau:

| Type  | Protocol | Port range | Source        | Description |
| :---  | :---     | :---       | :---          | :---        |
| HTTP  | TCP      | 80         | Anywhere-IPv4 | Cho phép người dùng truy cập qua HTTP. |
| HTTPS | TCP      | 443        | Anywhere-IPv4 | Cho phép người dùng truy cập qua HTTPS. |

    {{< figure src="/images/2.prerequisite/2.3-createsg/sg-alb-ib.png" title="Cấu hình Inbound Rules cho ALB SG" >}}

*   **Bước 3: Thiết lập Outbound Rules (Luồng truy cập ra)**
    *   ALB cần gửi traffic đến backend trên port 3000. Mặc định, Outbound Rules cho phép tất cả traffic đi ra, nhưng để bảo mật tốt hơn, chúng ta nên giới hạn lại.
    *   Chọn tab **Outbound rules** -> **Edit outbound rules**.
    *   Xóa rule mặc định và **Add rule** mới:

| Type | Protocol | Port range | Destination | Description |
| :--- | :--- | :--- | :--- | :--- |
| Custom TCP | TCP | 3000 | Custom -> `project-backend-sg` | Cho phép ALB gửi traffic tới các máy chủ backend. |

    {{< figure src="/images/2.prerequisite/2.3-createsg/sg-alb-ob.png" title="Cấu hình Outbound Rules cho ALB SG" >}}

*   **Bước 4:** Nhấn **Save rules**, sau đó nhấn **Create security group**.

Sau khi hoàn thành, bạn sẽ có 3 Security Group sẵn sàng để gán cho các tài nguyên tương ứng.