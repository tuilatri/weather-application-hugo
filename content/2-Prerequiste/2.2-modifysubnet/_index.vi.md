---
title : "Chỉnh sửa Subnet"
date : "2025-09-04" 
weight : 2 
chapter : false
pre : " <b> 2.2 </b> "
---

### Cấu hình Auto-assign Public IP cho Public Subnet

ℹ️ **Mục tiêu**

*   Kích hoạt tính năng tự động gán địa chỉ IP công cộng (Public IP) cho các tài nguyên (như EC2 instance) được khởi chạy trong các **Public Subnet**. Điều này là cần thiết để có thể truy cập vào Bastion Host từ Internet.

---

🔒 **Các bước thực hiện**

#### **1. Truy cập trang quản lý Subnet**

*   Từ giao diện **VPC Dashboard**, chọn **Subnets** ở menu bên trái để xem danh sách các subnet đã được tạo.

{{< figure src="/images/2.prerequisite/2.2-modifysubnet/subnet-list.png" title="Danh sách các Subnet" >}}

#### **2. Cấu hình cho Public Subnet đầu tiên**

*   Xác định và chọn một trong hai **Public Subnet** đã tạo (ví dụ: `project-vpc-public-subnet-ap-southeast-1a`).
*   Nhấn vào nút **Actions** và chọn **Edit subnet settings**.

{{< figure src="/images/2.prerequisite/2.2-modifysubnet/subnet-action-01.png" title="Chọn Edit subnet settings" >}}

#### **3. Kích hoạt tính năng Auto-assign IP**

*   Trong trang **Edit subnet settings**, tìm đến mục **Auto-assign IP settings**.
*   Tích vào ô **Enable auto-assign public IPv4 address**.
*   Cuộn xuống dưới và nhấn **Save** để lưu lại thay đổi.

{{< figure src="/images/2.prerequisite/2.2-modifysubnet/subnet-edit-01.png" title="Kích hoạt tính năng Auto-assign Public IP" >}}

#### **4. Lặp lại cho Public Subnet còn lại**

*   Thực hiện lại các bước **2** và **3** cho **Public Subnet thứ hai** (ví dụ: `project-vpc-public-subnet-ap-southeast-1b`) để đảm bảo các instance trong cả hai Availability Zone đều có thể nhận được IP công cộng khi cần thiết.

{{< figure src="/images/2.prerequisite/2.2-modifysubnet/subnet-action-02.png" >}}
{{< figure src="/images/2.prerequisite/2.2-modifysubnet/subnet-edit-02.png" title="Hoàn tất cấu hình cho cả hai Public Subnet" >}}