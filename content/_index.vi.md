---
title : "Dự án Ứng dụng Thời tiết"
date : "2025-09-05" 
weight : 1 
chapter : false
---
# Xây dựng Ứng dụng Thời tiết Tương tác

### Tổng quan

Trong workshop này, bạn sẽ được hướng dẫn từng bước để xây dựng một ứng dụng thời tiết hoàn chỉnh và có tính tương tác cao. Dự án sử dụng JavaScript cho phần frontend để xử lý giao diện và tương tác người dùng, cùng với một backend đơn giản bằng Node.js (Express) để giao tiếp an toàn với API thời tiết bên ngoài.

![Weather App Preview](/images/default-fe.png) 

### Nội dung

1.  [Giới thiệu và Tổng quan kiến trúc](1-introduce/)
2.  [Các bước chuẩn bị môi trường](2-prerequisite/)
3.  [Xây dựng Backend với Node.js và Express](3-backend/)
4.  [Xây dựng Frontend - Giao diện và Hiển thị cơ bản](4-frontend-basic/)
5.  [Triển khai các tính năng chính](5-main-features/)
6.  [Hoàn thiện giao diện và hiệu ứng](6-polishing/)
7.  [Tổng kết và Mở rộng](7-summary/)

---

### Chức năng chính của dự án

ℹ️ Ứng dụng này không chỉ hiển thị thông tin thời tiết cơ bản mà còn tích hợp nhiều tính năng nâng cao để mang lại trải nghiệm tốt nhất cho người dùng.

💡 **Các tính năng nổi bật:**

*   **Hiển thị thời tiết chi tiết:** Cung cấp thông tin thời tiết hiện tại và dự báo cho 7 ngày tới.
*   **Tìm kiếm thông minh:** Tự động gợi ý các địa điểm khi người dùng nhập.
*   **Định vị tự động:** Tự động lấy dữ liệu thời tiết dựa trên vị trí hiện tại của người dùng.
*   **Đa ngôn ngữ:** Hỗ trợ hơn 40 ngôn ngữ khác nhau, cho phép người dùng tùy chọn ngôn ngữ hiển thị.
*   **Lưu địa điểm yêu thích:** Sử dụng `localStorage` để lưu lại các địa điểm người dùng quan tâm.
*   **Chuyển đổi đơn vị:** Dễ dàng chuyển đổi nhiệt độ giữa độ C (°C) và độ F (°F).
*   **Chỉ số nâng cao:** Hiển thị các chỉ số quan trọng như Chất lượng không khí (AQI), chỉ số UV, và khả năng mưa.
*   **Giao diện động:** Thay đổi hình nền ngẫu nhiên và áp dụng các hiệu ứng chuyển động mượt mà.

### Kiến trúc và Phạm vi

ℹ️ Dự án được chia thành hai phần chính: **Frontend** (phía người dùng) và **Backend** (phía máy chủ), giao tiếp với nhau qua API.

**Frontend (Client-side)**

*   Chịu trách nhiệm hiển thị toàn bộ giao diện người dùng và xử lý các tương tác.
*   **`main.js`**: Tập tin trung tâm, quản lý luồng chính và các sự kiện (tìm kiếm, chọn ngôn ngữ, lưu địa điểm).
*   **`ui.js`**: Quản lý việc cập nhật và hiển thị dữ liệu lên DOM (Document Object Model), bao gồm cả hiệu ứng loading skeleton.
*   **`api.js`**: Gửi yêu cầu `fetch` đến backend để lấy dữ liệu thời tiết và gợi ý tìm kiếm.
*   **`lang.js`**: Chứa dữ liệu dịch thuật và logic để cập nhật văn bản tĩnh theo ngôn ngữ được chọn.
*   **`favorites.js`**: Cung cấp các hàm để quản lý danh sách địa điểm yêu thích trong `localStorage`.
*   **`clock.js`**: Logic cho cả đồng hồ kim và đồng hồ số hiển thị trên giao diện.
*   **`animations.js`**: Chứa các hàm tạo hiệu ứng (ví dụ: hiệu ứng đếm số cho nhiệt độ).

🔒 **Backend (Server-side)**

*   Đóng vai trò là một lớp trung gian (proxy) an toàn giữa frontend và WeatherAPI.
*   **`server.js`**: Xây dựng bằng Node.js và Express, tạo ra các endpoint để frontend có thể gọi.
    *   `/weather`: Nhận yêu cầu từ frontend, sau đó gọi đến WeatherAPI với API key được bảo mật trên server để lấy dữ liệu thời tiết.
    *   `/search`: Nhận yêu cầu tìm kiếm, gọi đến WeatherAPI để lấy danh sách gợi ý.

💡 **Hãy cùng bắt đầu xây dựng dự án thú vị này trong các phần tiếp theo!**