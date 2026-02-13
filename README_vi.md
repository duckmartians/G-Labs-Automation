[![Download for Windows](https://img.shields.io/badge/Download%20for%20Windows-%F0%9F%92%BB-blue?style=for-the-badge)](https://github.com/duckmartians/G-Labs-Automation/releases/latest)

Tham gia cộng đồng G-Labs Automation tại đây: [https://discord.gg/munMZEBMw5](https://discord.gg/munMZEBMw5)

Hướng dẫn sử dụng: [![Tiếng Việt](https://img.shields.io/badge/Tiếng%20Việt-green)](README_vi.md)

User manual: [![English](https://img.shields.io/badge/English-blue)](README.md) 

# G-Labs Automation - Hướng Dẫn Sử Dụng Chi Tiết

**Công cụ tự động hóa tạo ảnh & video AI sử dụng Google Labs (Imagen, Veo)**

---

## 🎯 Giới Thiệu

G-Labs Automation là công cụ desktop GUI giúp tự động hóa việc tạo ảnh và video AI thông qua Google Labs APIs:
- **Imagen 4 / Nano Banana**: Tạo ảnh từ text hoặc reference images
- **Veo 3.1**: Tạo video từ text, images hoặc components
- **Workflow System**: Tạo pipeline tự động với node-based editor

### Yêu Cầu Hệ Thống
- Windows 10/11
- Tài khoản Google có quyền truy cập Google Labs

## ⚠️ Lưu ý về cảnh báo bảo mật:
- Do đây là phần mềm cá nhân và chưa có chứng chỉ số (Digital Signature) đắt tiền từ Microsoft, nên Windows Defender hoặc bộ lọc SmartScreen có thể nhận diện nhầm là phần mềm lạ/nguy hiểm (False Positive).
- Cam kết an toàn: Tool hoàn toàn sạch. Nếu bạn quét bằng các phần mềm diệt virus chuyên sâu và uy tín như Kaspersky, Bitdefender hay ESET, kết quả sẽ là AN TOÀN. Vui lòng chọn "Run anyway" (Vẫn chạy) hoặc thêm vào danh sách loại trừ để sử dụng.

---

Khởi chạy **G-LabsAutomation**

<img width="147" height="162" alt="image" src="https://github.com/user-attachments/assets/754240c1-9924-44ef-9214-7aab59d5cfeb" />

## ⚙️ Cài Đặt Ban Đầu

### 1. Thêm Tài Khoản Google

#### Bước 1: Thêm Vào Ứng Dụng
1. Vào tab **⚙️ Cài Đặt**
2. Click **📋 Thêm Tài Khoản**

#### Bước 2: Kiểm Tra
- Tài khoản xuất hiện trong danh sách với trạng thái **✅ Ready**
- Nếu lỗi, xem phần [Xử Lý Lỗi](#-xử-lý-lỗi)

## TƯ DUY VỀ TÀI KHOẢN

Để sử dụng hiệu quả và an toàn, bạn cần phân biệt rõ **2 loại tài khoản** trong tool:

1. **Tài Khoản Bản Quyền (License Account):**
* Đây là tài khoản Google chính chủ của bạn dùng để đăng nhập *vào phần mềm* lần đầu tiên.
* Hệ thống sẽ ghi nhận gói cước (Basic/Plus/Max) dựa trên email này.
* **Khuyến nghị:** Nên dùng Email chính, có độ tin cậy cao để đảm bảo quyền lợi mua hàng và hỗ trợ lâu dài.


2. **Tài Khoản Chạy Tool (Worker Accounts):**
* Với mô hình Nano Banana và Imagen 4: chỉ cần gmail loại thường (free) là có thể chạy tạo ảnh.
* Với mô hình Nano Banana Pro và Veo 3.1: cần có gmail loại có gói Google One Pro hoặc Ultra mới có thể tạo.
* Đây là các tài khoản Google (Gmail) được thêm vào trong phần *Cài đặt (Settings)* để thực hiện việc tạo ảnh/video.
* Tool hỗ trợ thêm **không giới hạn** số lượng tài khoản worker.
* Trong tương lai, chúng tôi sẽ hỗ trợ thêm các nền tảng khác, nên worker account không chỉ giới hạn ở Google.
* **Mẹo:** Bạn có thể dùng các tài khoản phụ, tài khoản giá rẻ để chạy tính năng này mà không lo ảnh hưởng đến Email bản quyền chính.

---

## THIẾT LẬP HỆ THỐNG & THÊM TÀI KHOẢN

Trước khi bắt đầu, hãy nạp "nguyên liệu" (tài khoản worker) cho cỗ máy này.

1. Tại giao diện chính, bấm nút **"⚙️ Cài đặt" (Settings)** hoặc biểu tượng bánh răng ở góc dưới bên trái.
2. Chuyển sang tab **"Tài khoản Google"**.
3. **Thêm tài khoản:**
* Bấm nút **"➕ Thêm tài khoản"**. Một trình duyệt sẽ hiện ra, bạn chỉ cần đăng nhập Gmail như bình thường. Tool sẽ tự động bắt lấy Cookie và Token.

4. **Cấu hình Proxy (Dành cho dân chuyên):**
* Để nuôi số lượng lớn tài khoản và chạy đa luồng mà không bị Google chặn IP, bạn nên gán Proxy cho từng tài khoản.
* Bấm vào biểu tượng "Sửa" (cây bút) bên cạnh tài khoản để thêm Proxy (HTTP/SOCKS5).

> **Điểm tối ưu:** Tool có cơ chế **Auto-Renew Token**. Khi Token của Google hết hạn, tool sẽ tự động mở trình duyệt ngầm để gia hạn phiên làm việc (Session), đảm bảo quy trình treo máy 24/7 không bị gián đoạn.

---

## 📞 Hỗ Trợ

- **Website**: [https://duckmartians.info/](https://duckmartians.info/)
- **Discord**: [https://discord.gg/munMZEBMw5](https://discord.gg/munMZEBMw5)

---

**Tạo bởi Đặng Minh Đức [@duckmartians](https://github.com/duckmartians)**
