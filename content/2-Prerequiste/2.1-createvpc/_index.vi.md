---
title : "Tạo VPC"
date : "2025-09-04" 
weight : 1 
chapter : false
pre : " <b> 2.1 </b> "
---

### Tạo Amazon Virtual Private Cloud (VPC)

ℹ️ **Mục tiêu**

*   Tạo một môi trường mạng ảo (VPC) riêng biệt trong AWS cho dự án.
*   Thiết lập các thành phần mạng cần thiết một cách tự động, bao gồm Subnet, Internet Gateway, NAT Gateway và Route Tables.

---

🔒 **Các bước thực hiện**

#### **1. Truy cập dịch vụ VPC**

*   Từ giao diện **AWS Management Console**, tìm kiếm dịch vụ **VPC**.
*   Chọn **VPC** từ kết quả tìm kiếm để truy cập vào trang quản lý VPC.

{{< figure src="/images/2.prerequisite/2.1-createvpc/vpc-search.png" title="Giao diện quản lý VPC" >}}

#### **2. Bắt đầu tạo VPC**

*   Trong giao diện VPC Dashboard, nhấn vào nút **Create VPC**.

{{< figure src="/images/2.prerequisite/2.1-createvpc/vpc-choose.png" title="Nhấn nút Create VPC" >}}

#### **3. Cấu hình thông số VPC**

*   Tại trang **Create VPC**, trong phần **Resources to create**, chọn **VPC and more** để sử dụng trình hướng dẫn tạo tự động.

*   **Cấu hình chi tiết:**
    *   **Name tag auto-generation:** `project-vpc`
    *   **IPv4 CIDR block:** Để mặc định `10.0.0.0/16`.
    *   **Number of Availability Zones (AZs):** `2`
    *   **Number of Public subnets:** `2`
    *   **Number of Private subnets:** `2`
    *   **NAT gateways:** `1 per AZ` (Hoặc chọn `In 1 AZ` nếu bạn muốn tiết kiệm chi phí)
    *   **VPC endpoints:** `None`

{{< figure src="/images/2.prerequisite/2.1-createvpc/vpc-edit-01.png" >}}
{{< figure src="/images/2.prerequisite/2.1-createvpc/vpc-edit-02.png" title="Cấu hình VPC and more" >}}

{{% notice warning %}}
**Lưu ý:** Việc tạo NAT Gateway sẽ phát sinh chi phí. Hãy xóa tài nguyên sau khi hoàn thành workshop để tránh phát sinh chi phí không mong muốn.
{{% /notice %}}

#### **4. Xác nhận và tạo VPC**

*   Sau khi đã điền đầy đủ thông tin, nhấn vào **Create VPC** ở góc dưới cùng bên phải để bắt đầu quá trình tạo.

{{< figure src="/images/2.prerequisite/2.1-createvpc/vpc-edit-03.png" title="Hoàn tất và tạo VPC" >}}

#### **5. Kiểm tra trạng thái VPC**

*   Quá trình tạo các tài nguyên có thể mất vài phút. Sau khi hoàn tất, bạn sẽ thấy màn hình thông báo thành công.
*   Nhấn vào **View VPC** để xem lại các tài nguyên đã được tạo.

{{< figure src="/images/2.prerequisite/2.1-createvpc/vpc-done.png" title="Thông báo tạo VPC thành công" >}}

*   Bạn sẽ được chuyển đến trang danh sách các VPC, nơi VPC `project-vpc` vừa tạo đã ở trạng thái **Available**.

{{< figure src="/images/2.prerequisite/2.1-createvpc/vpc-check.png" title="Kiểm tra VPC trong danh sách" >}}