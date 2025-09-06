---
title : "Tạo kết nối đến máy chủ EC2"
date : "2025-09-04" 
weight : 3 
chapter : false
pre : " <b> 3. </b> "
---

Sau khi đã xây dựng xong toàn bộ hạ tầng trên AWS, bước tiếp theo là kết nối vào các máy chủ ảo (EC2 instance) để triển khai mã nguồn ứng dụng backend.

Do kiến trúc của chúng ta có Public và Private Subnet, chúng ta không thể kết nối trực tiếp vào máy chủ Backend từ Internet. Thay vào đó, chúng ta sẽ thực hiện quy trình kết nối an toàn:
1.  Kết nối từ máy tính cá nhân vào **Bastion Host** (đang nằm trong Public Subnet).
2.  Từ Bastion Host, tiếp tục kết nối vào **Backend Instance** (đang nằm trong Private Subnet).

{{% notice info %}}
Trong chương này, **MobaXterm** sẽ là công cụ SSH client trên Windows để thực hiện các kết nối. Bạn cũng có thể sử dụng các công cụ khác như Terminal (macOS/Linux), PuTTY, hoặc Windows Terminal.
{{% /notice %}}