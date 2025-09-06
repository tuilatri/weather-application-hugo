---
title : "Tạo Launch Template cho Auto Scaling"
date : "2025-09-04"
weight : 6
chapter : false
pre : " <b> 2.6 </b> "
---

### Khởi tạo Launch Template

ℹ️ **Mục tiêu**

*   **Launch Template** hoạt động như một "bản thiết kế" chi tiết cho việc khởi tạo EC2 instance. Nó lưu trữ tất cả các thông số cấu hình cần thiết như AMI, loại instance, key pair, security group, và các thiết lập mạng.
*   Mục đích của chúng ta là tạo một Launch Template sử dụng AMI của máy chủ backend (`project-backend-ec2-ami`) đã tạo ở bước trước. Auto Scaling Group sẽ dựa vào bản thiết kế này để tự động khởi chạy các instance mới một cách đồng nhất và chính xác.

---

🔒 **Các bước thực hiện**

#### **1. Truy cập Launch Templates**

*   Từ **EC2 Dashboard**, ở menu bên trái, cuộn xuống và chọn **Launch Templates**.
*   Nhấn vào **Create launch template**.

{{< figure src="/images/2.prerequisite/2.6-createlaunchtemplate/create-launch-template-button.png" title="Bắt đầu tạo Launch Template" >}}

#### **2. Cấu hình thông tin cơ bản**

*   **Launch template name:** `project-backend-lauch-template`
*   **Template version description:** `Template for weather project backend servers`

{{< figure src="/images/2.prerequisite/2.6-createlaunchtemplate/lt-basic-info.png" title="Điền thông tin cơ bản cho Launch Template" >}}

#### **3. Chọn AMI (Amazon Machine Image)**

*   Trong mục **Application and OS Images (Amazon Machine Image)**, chọn tab **My AMIs**.
*   Chọn **Owned by me** và bạn sẽ thấy AMI đã tạo ở bước trước. Hãy chọn `project-backend-ec2-ami`.

{{< figure src="/images/2.prerequisite/2.6-createlaunchtemplate/lt-select-ami.png" title="Chọn AMI đã tạo" >}}

#### **4. Chọn Instance Type và Key Pair**

*   **Instance type:** Chọn `t2.micro`.
*   **Key pair (login):** Chọn `project-keypair` từ danh sách thả xuống.

{{< figure src="/images/2.prerequisite/2.6-createlaunchtemplate/lt-instance-type-keypair.png" title="Chọn loại Instance và Key Pair" >}}

#### **5. Cấu hình Network Settings**

*   Trong mục **Network settings**, chúng ta sẽ chỉ định Security Group.
*   **Security groups:** Chọn **Select existing security group**.
*   Trong danh sách **Common security groups**, chọn `project-backend-sg`.

{{< figure src="/images/2.prerequisite/2.6-createlaunchtemplate/lt-network-settings.png" title="Cấu hình Security Group" >}}

#### **6. Hoàn tất và xem lại Launch Template**

*   Kiểm tra lại tất cả các thông tin đã cấu hình.
*   Cuộn xuống dưới và nhấn **Create launch template**.
*   Sau khi tạo thành công, nhấn **View launch templates** để xem lại template bạn vừa tạo trong danh sách.

{{< figure src="/images/2.prerequisite/2.6-createlaunchtemplate/lt-success.png" title="Tạo Launch Template thành công" >}}

{{< figure src="/images/2.prerequisite/2.6-createlaunchtemplate/lt-view-list.png" title="Xem Launch Template trong danh sách" >}}