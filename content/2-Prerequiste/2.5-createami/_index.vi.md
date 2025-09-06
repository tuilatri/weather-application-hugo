---
title : "Tạo Amazon Machine Image (AMI)"
date : "2025-09-04"
weight : 5
chapter : false
pre : " <b> 2.5 </b> "
---

### Khởi tạo AMI từ Backend Instance

ℹ️ **Mục tiêu**

*   **Amazon Machine Image (AMI)** là một bản "snapshot" hay bản sao lưu hoàn chỉnh của một EC2 instance, bao gồm hệ điều hành, cấu hình và dữ liệu.
*   Mục đích chính của việc tạo AMI trong workshop này là để có một "khuôn mẫu" chuẩn cho máy chủ backend. Khuôn mẫu này sẽ được **Launch Template** và **Auto Scaling Group** sử dụng để tự động tạo ra các bản sao y hệt của máy chủ backend khi cần mở rộng quy mô.

---

🔒 **Các bước thực hiện**

#### **1. Chọn EC2 Instance nguồn**

*   Trong **EC2 Dashboard**, điều hướng đến **Instances**.
*   Từ danh sách, chọn instance `project-backend-ec2`.

{{< figure src="/images/2.prerequisite/2.5-createami/select-backend-instance.png" title="Chọn EC2 Instance Backend" >}}

#### **2. Bắt đầu quá trình tạo Image**

*   Sau khi đã chọn instance, nhấn vào menu **Actions**.
*   Chọn **Image and templates**.
*   Chọn **Create image**.

{{< figure src="/images/2.prerequisite/2.5-createami/actions-create-image.png" title="Bắt đầu tạo Image từ menu Actions" >}}

#### **3. Cấu hình thông tin cho AMI**

*   Tại trang **Create image**, điền các thông tin sau:
    *   **Image name:** `project-backend-ec2-ami`
    *   **Image description:** `project-backend-ec2-ami`
*   Các tùy chọn khác có thể để mặc định.

{{< figure src="/images/2.prerequisite/2.5-createami/ami-configuration.png" title="Điền thông tin cấu hình cho AMI" >}}

#### **4. Hoàn tất và kiểm tra trạng thái**

*   Cuộn xuống dưới và nhấn **Create image**.
*   Bạn sẽ nhận được thông báo rằng quá trình tạo AMI đã bắt đầu.

*   Để theo dõi tiến trình:
    *   Ở menu bên trái, dưới mục **Images**, chọn **AMIs**.
    *   Bạn sẽ thấy AMI `project-backend-ec2-ami` đang ở trạng thái `pending`.
    *   Quá trình này có thể mất vài phút. Hãy đợi cho đến khi **Status** chuyển sang `Available`.

{{< figure src="/images/2.prerequisite/2.5-createami/ami-status-available.png" title="Kiểm tra trạng thái AMI đã hoàn tất" >}}