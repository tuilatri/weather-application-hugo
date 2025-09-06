---
title : "Tạo Auto Scaling Group"
date : "2025-09-04"
weight : 9
chapter : false
pre : " <b> 2.9 </b> "
---

### Khởi tạo Auto Scaling Group (ASG)

ℹ️ **Mục tiêu**

*   **Auto Scaling Group (ASG)** là trái tim của một kiến trúc linh hoạt và có khả năng co giãn trên AWS.
*   Nhiệm vụ của nó là tự động điều chỉnh số lượng EC2 instance đang chạy để đáp ứng nhu cầu lưu lượng truy cập hiện tại. Khi traffic tăng, ASG sẽ tự động thêm instance mới (Scale Out). Khi traffic giảm, nó sẽ tự động loại bỏ các instance không cần thiết (Scale In) để tiết kiệm chi phí.
*   Chúng ta sẽ cấu hình một ASG sử dụng **Launch Template** đã tạo để biết *cách* tạo instance, và gắn nó với **Load Balancer** để biết *khi nào* cần tạo.

---

🔒 **Các bước thực hiện**

#### **1. Bắt đầu tạo Auto Scaling Group**

*   Từ **EC2 Dashboard**, ở menu bên trái, cuộn xuống dưới cùng và chọn **Auto Scaling Groups**.
*   Nhấn vào **Create Auto Scaling group**.

{{< figure src="/images/2.prerequisite/2.9-createasg/create-asg-button.png" title="Bắt đầu tạo Auto Scaling Group" >}}

#### **2. Đặt tên và chọn Launch Template**

*   **Auto Scaling group name:** `project-backend-asg`
*   **Launch template:** Chọn `project-backend-lauch-template` từ danh sách.
*   Sau khi chọn, nhấn **Next**.

{{< figure src="/images/2.prerequisite/2.9-createasg/asg-name-template.png" title="Chọn tên và Launch Template" >}}

#### **3. Cấu hình Network**

*   **VPC:** Chọn `project-vpc`.
*   **Availability Zones and subnets:** Chọn cả hai **Private Subnet** mà chúng ta có. ASG sẽ sử dụng các subnet này để khởi chạy instance mới, đảm bảo chúng được bảo vệ và có tính sẵn sàng cao.
*   Nhấn **Next**.

{{< figure src="/images/2.prerequisite/2.9-createasg/asg-network-config.png" title="Cấu hình VPC và các Private Subnet" >}}

#### **4. Tích hợp với Load Balancer**

*   Chọn **Attach to an existing load balancer**.
*   Chọn **Choose from your load balancer target groups**.
*   Trong danh sách thả xuống, chọn `project-backend-target-group`.
*   Nhấn **Next**.

{{< figure src="/images/2.prerequisite/2.9-createasg/asg-load-balancing.png" title="Gắn ASG vào Target Group đã có" >}}

#### **5. Cấu hình quy mô và chính sách Scaling**

*   **Group size (Quy mô nhóm):**
    *   **Desired capacity:** `1` (Số lượng instance mong muốn khi ASG được tạo).
    *   **Minimum capacity:** `1` (Số lượng instance tối thiểu phải có).
    *   **Maximum capacity:** `2` (Số lượng instance tối đa được phép tạo).

*   **Scaling policies (Chính sách mở rộng):**
    *   Chọn **Target tracking scaling policy**.
    *   **Scaling policy name:** `Target Tracking Policy`
    *   **Metric type:** `Application Load Balancer request count per target`.
    *   **Target value:** `30`. (Nghĩa là: nếu mỗi instance phải xử lý trung bình hơn 30 yêu cầu/phút, hãy thêm instance mới).
*   Nhấn **Next**.

{{< figure src="/images/2.prerequisite/2.9-createasg/asg-size-and-scaling-01.png" >}}
{{< figure src="/images/2.prerequisite/2.9-createasg/asg-size-and-scaling-02.png" title="Cấu hình quy mô và chính sách Scaling" >}}

#### **6. Bỏ qua các bước tùy chọn**

*   Bạn sẽ được chuyển đến các trang **Add notifications** và **Add tags**. Chúng ta không cần cấu hình chúng trong workshop này.
*   Nhấn **Next** ở cả hai trang để bỏ qua.

#### **7. Review và Hoàn tất**

*   Kiểm tra lại tất cả các thông tin đã cấu hình trên trang **Review**.
*   Cuộn xuống dưới và nhấn **Create Auto Scaling group**.

{{< figure src="/images/2.prerequisite/2.9-createasg/asg-review-and-create.png" title="Xem lại và tạo Auto Scaling Group" >}}

#### **8. Kiểm tra trạng thái**

*   Sau khi tạo xong, ASG sẽ xuất hiện trong danh sách.
*   Chọn vào `project-backend-asg` và chuyển sang tab **Instance management**.
*   Bạn sẽ thấy ASG đang trong quá trình khởi tạo một instance mới để đạt được **Desired capacity** là `1`. Hãy đợi cho đến khi **Lifecycle** của instance là `InService`. Điều này cho thấy instance đã sẵn sàng hoạt động và đã được đăng ký vào Target Group.

{{< figure src="/images/2.prerequisite/2.9-createasg/asg-instance-management.png" title="Kiểm tra instance do ASG quản lý" >}}