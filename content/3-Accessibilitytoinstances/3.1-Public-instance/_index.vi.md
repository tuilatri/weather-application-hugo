---
title : "Kết nối đến máy chủ"
date : "2025-09-04" 
weight : 1 
chapter : false
pre : " <b> 3.1. </b> "
---

### Hướng dẫn chi tiết các lệnh trên MobaXterm

Trong phần này, bạn sẽ thực hiện các lệnh để kết nối từ máy tính cá nhân vào Bastion Host, sau đó từ Bastion Host kết nối vào máy chủ Backend để cài đặt và chạy ứng dụng.

{{% notice info %}}
**Lưu ý:** Các lệnh được thực hiện tuần tự. Hãy đảm bảo bạn đã kết nối thành công ở bước trước rồi mới tiếp tục bước sau.
{{% /notice %}}

#### **1. Kết nối vào Bastion Host**
Đây là bước đầu tiên, kết nối từ máy tính của bạn vào máy chủ Bastion Host trong Public Subnet.

*   Mở MobaXterm và tạo một phiên SSH mới.
*   **Remote host:** Nhập địa chỉ Public IPv4 của `project-bastion-host-ec2`.
*   **Specify username:** `ec2-user`.
*   **Use private key:** Chọn file `project-keypair.pem` bạn đã tải về khi tạo EC2.

{{< figure src="/images/3.connect/3.1-connect/mobaxterm-session-setup.png" title="Thiết lập phiên SSH trong MobaXterm" >}}

#### **2. Chuyển tiếp (Jump) vào Backend Instance**
Bây giờ, khi đã ở trong Bastion Host, chúng ta cần chuẩn bị để kết nối tới máy chủ Backend.

*   **Bước 1: Tạo lại file key pair trên Bastion Host.**
    Mở một trình soạn thảo văn bản như `nano` và dán nội dung của file `.pem` từ máy tính của bạn vào.
    ```bash
    nano project-keypair.pem
    ```
    {{< figure src="/images/3.connect/3.1-connect/nano-key-file.png" title="Dán nội dung key vào nano" >}}

*   **Bước 2: Phân quyền chính xác cho file key.**
    SSH yêu cầu file key phải được bảo mật, nếu không sẽ từ chối kết nối.
    ```bash
    chmod 400 project-keypair.pem
    ```
    {{< figure src="/images/3.connect/3.1-connect/chmod-key.png" title="Phân quyền cho file key" >}}

*   **Bước 3: Thực hiện kết nối SSH đến Backend.**
    Sử dụng địa chỉ **Private IPv4** của `project-backend-ec2`.
    ```bash
    ssh -i project-keypair.pem ec2-user@<Private_IP_của_EC2_Backend>
    ```
    {{< figure src="/images/3.connect/3.1-connect/ssh-to-backend.png" title="Kết nối thành công vào máy chủ Backend" >}}

#### **3. Cài đặt môi trường và ứng dụng trên Backend**
Khi đã ở trong máy chủ Backend, hãy cài đặt các công cụ cần thiết.

*   **Cập nhật hệ thống:**
    ```bash
    sudo yum update -y
    ```
    {{< figure src="/images/3.connect/3.1-connect/yum-update.png" title="Cập nhật các gói hệ thống" >}}

*   **Cài đặt Git:**
    ```bash
    sudo yum install -y git
    ```
    {{< figure src="/images/3.connect/3.1-connect/install-git.png" title="Cài đặt Git" >}}

*   **Sao chép mã nguồn dự án từ GitHub:**
    ```bash
    git clone https://github.com/tuilatri/weather-application.git
    ```
    {{< figure src="/images/3.connect/3.1-connect/git-clone.png" title="Sao chép dự án" >}}

*   **Di chuyển vào thư mục backend:**
    ```bash
    cd weather-application/backend
    ```
    {{< figure src="/images/3.connect/3.1-connect/cd-backend.png" title="Di chuyển vào thư mục backend" >}}

*   **Cài đặt Node.js:**
    ```bash
    sudo dnf install -y nodejs
    ```
    {{< figure src="/images/3.connect/3.1-connect/install-nodejs.png" title="Cài đặt Node.js" >}}

*   **Cài đặt các thư viện phụ thuộc của dự án:**
    ```bash
    npm install
    ```
    {{< figure src="/images/3.connect/3.1-connect/npm-install.png" title="Cài đặt các gói npm" >}}

*   **Cấu hình biến môi trường:**
    ```bash
    nano .env
    ```
    {{< figure src="/images/3.connect/3.1-connect/nano-env.png" title="Cấu hình file .env" >}}

#### **4. Khởi chạy và kiểm tra Backend**

*   **Khởi chạy ứng dụng:**
    ```bash
    npm run start
    ```
    {{< figure src="/images/3.connect/3.1-connect/npm-start.png" title="Ứng dụng backend đang chạy" >}}

*   **Kiểm tra hoạt động của ứng dụng ngay trên máy chủ:**
    Mở một cửa sổ terminal khác trên MobaXterm và kết nối lại, sau đó dùng lệnh `curl` để kiểm tra.
    ```bash
    curl http://localhost:3000/
    ```
    {{< figure src="/images/3.connect/3.1-connect/curl-test.png" title="Kiểm tra backend bằng curl" >}}

### **5. Cập nhật Frontend với địa chỉ CloudFront của Backend**
Sau khi Backend đã hoạt động, bạn cần lấy đường dẫn CloudFront của Backend và cập nhật vào file cấu hình của Frontend trên S3.

*   **Bước 1:** Lấy đường dẫn CloudFront Distribution Domain Name của Backend.
Truy cập dịch vụ CloudFront trong AWS Console, tìm và sao chép lại Domain Name của distribution trỏ đến backend.

*   **Bước 2:** Truy cập S3 bucket chứa mã nguồn Frontend.
Vào dịch vụ S3, chọn bucket có chứa mã nguồn của frontend.

*   **Bước 3:** Mở và chỉnh sửa file api.js.
Tìm đến file api.js (thường nằm trong thư mục js hoặc tương tự), chọn file và nhấn Edit.

*   **Bước 4:** Cập nhật địa chỉ API.
Trong nội dung file, tìm đến dòng khai báo biến API và dán đường dẫn CloudFront của backend vào. Sau đó nhấn Save changes.
code
JavaScript
// Thay đổi giá trị của biến này
const API_URL = 'https://your-cloudfront-backend-domain.cloudfront.net';
{{< figure src="/images/3.connect/3.1-connect/modify-api.png" title="Dán đường dẫn CloudFront và lưu lại" >}}

*   **Bước 5:** Chờ CloudFront của Frontend cập nhật.
Sau khi bạn lưu file api.js, CloudFront phục vụ cho Frontend sẽ cần vài phút để xóa cache cũ và cập nhật phiên bản mới của file. Hãy chờ khoảng 5-10 phút trước khi kiểm tra lại trang web.
{{< figure src="/images/3.connect/3.1-connect/result.png" title="Chờ đợi CloudFront cập nhật" >}}

#### **6. Hướng dẫn xử lý sự cố (Troubleshooting)**
Nếu bạn cập nhật code trên GitHub nhưng thay đổi không xuất hiện trên web, hãy làm theo các bước sau trên máy chủ Backend:

*   **Kéo code mới nhất về:**
    ```bash
    git pull origin main
    ```
*   **Tìm tiến trình Node.js đang chạy:**
    ```bash
    ps aux | grep node
    ```
*   **Dừng tiến trình cũ bằng PID của nó:**
    ```bash
    kill -9 <PID>
    ```
    *Ví dụ thực tế:*
    ```
    [ec2-user@ip-10-0-144-101 backend]$ ps aux | grep node
    ec2-user    3117  0.0  6.6 1127836 64896 pts/1   Sl+   10:15   0:00 node server.js
    
    [ec2-user@ip-10-0-144-101 backend]$ kill -9 3117
    ```

*   **Khởi động lại ứng dụng:**
    ```bash
    npm run start
    ```