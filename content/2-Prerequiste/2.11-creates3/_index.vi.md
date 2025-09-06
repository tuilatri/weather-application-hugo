---
title : "Tạo S3 Bucket và cấu hình Static Website Hosting"
date : "2025-09-04"
weight : 11
chapter : false
pre : " <b> 2.11 </b> "
---

### Tạo S3 Bucket để lưu trữ Frontend

ℹ️ **Mục tiêu**

*   **Amazon S3 (Simple Storage Service)** là một dịch vụ lưu trữ đối tượng có khả năng mở rộng, độ bền cao và chi phí thấp.
*   Chúng ta sẽ sử dụng S3 để lưu trữ toàn bộ các tệp tĩnh của ứng dụng frontend (HTML, CSS, JavaScript, hình ảnh).
*   Sau đó, chúng ta sẽ cấu hình bucket này để hoạt động như một máy chủ web tĩnh (Static Website Hosting), cho phép truy cập trực tiếp vào trang web qua một URL của S3.

---

🔒 **Các bước thực hiện**

#### **1. Bắt đầu tạo S3 Bucket**

*   Trong **AWS Management Console**, tìm kiếm và chọn dịch vụ **S3**.
*   Nhấn vào **Create bucket**.

{{< figure src="/images/2.prerequisite/2.11-creates3/create-s3-button.png" title="Bắt đầu tạo S3 Bucket" >}}

#### **2. Cấu hình thông tin cơ bản**

*   **Bucket name:** `project-frontend-030925`
    {{% notice warning %}}
    Tên S3 bucket là duy nhất trên toàn cầu. Nếu tên này đã tồn tại, bạn cần thêm các ký tự hoặc số để làm cho nó trở nên độc nhất (ví dụ: `project-frontend-030925-yourname`).
    {{% /notice %}}
*   **AWS Region:** Chọn Region bạn đang làm việc.

*   **Object Ownership:** Chọn `ACLs enabled`.
    {{% notice info %}}
    Chúng ta chọn tùy chọn này để có thể cấp quyền công khai cho từng tệp riêng lẻ bằng Access Control Lists (ACL), một phương pháp cần thiết cho bước cấu hình tiếp theo.
    {{% /notice %}}

{{< figure src="/images/2.prerequisite/2.11-creates3/s3-basic-config-acl.png" title="Cấu hình tên, Region và bật ACLs" >}}

#### **3. Cấu hình Public Access Settings**

*   Tích ở ô **Block all public access**.

{{% notice warning %}}
Bước này cho phép chúng ta làm cho các đối tượng trong bucket có thể truy cập công khai. Trong các bước tiếp theo, CloudFront sẽ được cấu hình để bảo vệ bucket này một cách an toàn hơn.
{{% /notice %}}

{{< figure src="/images/2.prerequisite/2.11-creates3/s3-enable-block-public-access.png" title="Bật Block Public Access" >}}

*   **Bucket Versioning:** Chọn `Enable`.
*   Cuộn xuống và nhấn **Create bucket**.

#### **4. Tải file Frontend lên Bucket**

*   Sau khi tạo bucket thành công, hãy vào bucket `project-frontend-030925`.
*   Nhấn **Upload** và tải toàn bộ các tệp và thư mục của dự án frontend của bạn lên.

{{< figure src="/images/2.prerequisite/2.11-creates3/s3-upload-files.png" title="Tải các tệp Frontend lên S3" >}}

#### **5. Cấu hình Static Website Hosting**

*   Trong bucket, chuyển sang tab **Properties**.
*   Cuộn xuống dưới cùng đến mục **Static website hosting** và nhấn **Edit**.
*   Chọn **Enable**.
*   **Index document:** `index.html`
*   Nhấn **Save changes**.

{{< figure src="/images/2.prerequisite/2.11-creates3/s3-static-hosting.png" title="Bật và cấu hình Static Website Hosting" >}}