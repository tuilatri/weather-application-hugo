---
title : "Tạo EC2 Instance cho Bastion Host và Backend"
date : "2025-09-04"
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---

### Khởi tạo EC2 Instances

ℹ️ **Mục tiêu**

*   Khởi tạo hai máy chủ ảo (EC2 Instance) riêng biệt:
    1.  **Bastion Host:** Đặt trong Public Subnet để làm "trạm trung chuyển", giúp chúng ta kết nối an toàn vào các tài nguyên trong Private Subnet.
    2.  **Backend Instance:** Đặt trong Private Subnet để chạy ứng dụng, được bảo vệ khỏi truy cập trực tiếp từ Internet.

---

🔒 **Các bước thực hiện**

Đầu tiên, chúng ta sẽ khởi tạo EC2 Instance cho **Bastion Host**.

#### **1. Tạo EC2 Instance cho Bastion Host (`project-bastion-host-ec2`)**

*   **Bước 1: Bắt đầu khởi tạo Instance**
    *   Trong giao diện **AWS Management Console**, tìm và chọn dịch vụ **EC2**.
    *   Từ **EC2 Dashboard**, nhấn vào **Launch instance**.

    {{< figure src="/images/2.prerequisite/2.4-createec2/launch-instance-dashboard.png" title="Bắt đầu Launch Instance" >}}

*   **Bước 2: Đặt tên và chọn AMI**
    *   **Name:** `project-bastion-host-ec2`
    *   **Application and OS Images (Amazon Machine Image):** Chọn **Amazon Linux**.

    {{< figure src="/images/2.prerequisite/2.4-createec2/bastion-name-ami.png" title="Đặt tên và chọn Amazon Machine Image" >}}

*   **Bước 3: Chọn loại Instance**
    *   **Instance type:** Chọn `t2.micro` (thuộc Free Tier).

*   **Bước 4: Tạo Key Pair để truy cập**
    *   Đây là bước quan trọng để có thể SSH vào instance.
    *   Nhấn vào **Create new key pair**.
    *   **Key pair name:** `project-keypair`
    *   **Key pair type:** `RSA`
    *   **Private key file format:** `.pem`
    *   Nhấn **Create key pair** và trình duyệt sẽ tự động tải về file `.pem`.

    {{% notice warning %}}
    **QUAN TRỌNG:** Hãy lưu trữ file `.pem` này ở một nơi an toàn. Bạn sẽ không thể tải lại nó lần thứ hai. Nếu làm mất, bạn sẽ không thể truy cập vào EC2 instance.
    {{% /notice %}}

    {{< figure src="/images/2.prerequisite/2.4-createec2/create-key-pair.png" title="Tạo Key Pair mới" >}}

*   **Bước 5: Cấu hình Network Settings**
    *   Nhấn vào **Edit** ở mục **Network settings**.
    *   **VPC:** Chọn `project-vpc` đã tạo.
    *   **Subnet:** Chọn một trong hai **Public Subnet** (ví dụ: `...public-subnet-ap-southeast-1a`).
    *   **Auto-assign public IP:** `Enable`.
    *   **Firewall (security groups):** Chọn **Select existing security group** và chọn `project-bastion-host-sg`.

    {{< figure src="/images/2.prerequisite/2.4-createec2/bastion-network-settings.png" title="Cấu hình mạng cho Bastion Host" >}}

*   **Bước 6: Khởi tạo Instance**
    *   Kiểm tra lại các thông tin trong bảng **Summary**.
    *   Nhấn **Launch instance**.

---

#### **2. Tạo EC2 Instance cho Backend (`project-backend-ec2`)**

Tiếp theo, chúng ta lặp lại quy trình để tạo instance cho Backend, với một vài thay đổi quan trọng trong phần Network Settings.

*   **Bước 1: Bắt đầu khởi tạo Instance**
    *   Từ **EC2 Dashboard**, nhấn **Launch instance** một lần nữa.

*   **Bước 2: Đặt tên và chọn AMI**
    *   **Name:** `project-backend-ec2`
    *   **AMI:** Chọn **Amazon Linux**.

*   **Bước 3: Chọn loại Instance**
    *   **Instance type:** Chọn `t2.micro`.

*   **Bước 4: Chọn Key Pair đã có**
    *   Trong mục **Key pair (login)**, chọn `project-keypair` đã tạo ở phần trước.

    {{< figure src="/images/2.prerequisite/2.4-createec2/backend-select-key-pair.png" title="Chọn Key Pair đã tồn tại" >}}

*   **Bước 5: Cấu hình Network Settings**
    *   Nhấn vào **Edit** ở mục **Network settings**.
    *   **VPC:** Chọn `project-vpc`.
    *   **Subnet:** Chọn một trong hai **Private Subnet** (ví dụ: `...private-subnet-ap-southeast-1b`).
    *   **Auto-assign public IP:** `Disable`. Đây là điểm khác biệt quan trọng để bảo vệ máy chủ backend.
    *   **Firewall (security groups):** Chọn **Select existing security group** và chọn `project-backend-sg`.

    {{< figure src="/images/2.prerequisite/2.4-createec2/backend-network-settings.png" title="Cấu hình mạng cho Backend Instance" >}}

*   **Bước 6: Khởi tạo Instance**
    *   Kiểm tra lại thông tin và nhấn **Launch instance**.

Sau khi hoàn tất, bạn có thể nhấn **View all instances** để xem cả hai máy chủ vừa tạo đang ở trạng thái `Running`.

{{< figure src="/images/2.prerequisite/2.4-createec2/view-all-instances.png" title="Danh sách các EC2 Instance đã tạo" >}}