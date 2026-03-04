<p align="center">
  <a href="https://github.com/duckmartians/G-Labs-Automation/releases/latest">
    <img src="https://img.shields.io/badge/Tải%20Về%20cho%20Windows-%F0%9F%92%BB-blue?style=for-the-badge" alt="Tải về cho Windows">
  </a>
</p>

<p align="center">
  <a href="README.md"><img src="https://img.shields.io/badge/English-blue" alt="English"></a>
  <a href="README_vi.md"><img src="https://img.shields.io/badge/Tiếng%20Việt-green" alt="Tiếng Việt"></a>
  <a href="https://discord.gg/munMZEBMw5"><img src="https://img.shields.io/badge/Discord-Cộng%20đồng-5865F2?logo=discord&logoColor=white" alt="Discord"></a>
</p>

---

# 📖 G-Labs Automation — Hướng Dẫn Sử Dụng

> Tài liệu hướng dẫn sử dụng chi tiết cho **G-Labs Automation** — ứng dụng tạo ảnh và video AI tự động.

> 🎬 **Video hướng dẫn:** [Xem trên YouTube](https://www.youtube.com/playlist?list=PLGHIReR0l_N2TbhPADNwn0aRbJfLf5YSj)

## Mục Lục

| # | Nội dung | Mô tả |
|---|---------|-------|
| 1 | [Tổng Quan & Cài Đặt](#phan-1) | Giới thiệu, yêu cầu hệ thống, cài đặt |
| 2 | [Quản Lý Tài Khoản](#phan-2) | Thêm, xóa, gia hạn tài khoản Google |
| 3 | [Hệ Thống Bản Quyền](#phan-3) | 3 gói Basic/Plus/Max, thanh toán |
| 4 | [Tạo Ảnh AI](#phan-4) | 3 model AI, cấu hình, xử lý hàng loạt |
| 5 | [Tạo Video AI](#phan-5) | 3 tab: Văn bản/Ảnh, Thành phần, Khung hình |
| 6 | [Biên Tập Quy Trình](#phan-6) | Trình soạn thảo trực quan dạng node |
| 7 | [Tính Năng Bổ Sung](#phan-7) | Webhook API, Extension Auth, Cài đặt, Khôi phục lỗi |

---

<a name="phan-1"></a>
<details>
<summary><h2>📦 Phần 1: Tổng Quan & Cài Đặt Ban Đầu</h2></summary>

### 1.1 Giới Thiệu

**G-Labs Automation** là ứng dụng desktop Windows tự động hóa việc tạo ảnh và video bằng AI. Ứng dụng hỗ trợ nhiều model AI (Nano Banana, Imagen 4, Nano Banana Pro), xử lý hàng loạt, và tích hợp luồng tự động hóa nâng cao.

**Tính năng chính:**
- **Tạo Ảnh AI**: 3 model, hỗ trợ ảnh tham chiếu, nâng cấp độ phân giải, xử lý hàng loạt
- **Tạo Video AI**: Văn bản thành video, Chuỗi cảnh, Ghép ảnh thành phần
- **Trình Soạn Luồng**: Biên tập trực quan dạng node cho quy trình tự động
- **Webhook API**: Máy chủ API cục bộ để tích hợp với hệ thống bên ngoài
- **Đa ngôn ngữ**: Hỗ trợ 2 ngôn ngữ

### 1.2 Yêu Cầu Hệ Thống

| Yêu cầu | Chi tiết |
|----------|----------|
| **Hệ điều hành** | Windows 10/11 (64-bit) |
| **RAM** | Tối thiểu 4GB, khuyến nghị 8GB+ |
| **Ổ cứng** | Tối thiểu 500MB trống (chưa tính dữ liệu đầu ra) |
| **Kết nối mạng** | Bắt buộc |

### 1.3 Tải Về & Cài Đặt

1. Tải file **G-Labs-Automation-vX.Y.Z.zip** từ nguồn phân phối chính thức
2. Giải nén vào thư mục mong muốn (ví dụ: `D:\G-Labs Automation\`)
3. Mở thư mục đã giải nén, chạy file **G-LabsAutomation.exe**

> ⚠️ **Không** đặt thư mục ứng dụng trong `C:\Program Files` hoặc các thư mục hệ thống yêu cầu quyền quản trị.

<details>
<summary>⚠️ Xử Lý Cảnh Báo Phần Mềm Diệt Virus (nhấn để mở)</summary>

Do ứng dụng được đóng gói, một số phần mềm diệt virus có thể **nhận nhầm là mã độc** (báo nhầm).

**Cách thêm vào danh sách loại trừ của Windows Defender:**

1. Mở **Windows Security** → **Bảo vệ khỏi virus & mối đe dọa**
2. Nhấn **Quản lý thiết đặt** → cuộn xuống **Loại trừ**
3. Nhấn **Thêm loại trừ** → **Thư mục** → Chọn thư mục G-Labs Automation

> 💡 Nên thêm loại trừ **trước khi giải nén** để tránh bị xóa file.

</details>

### 1.4 Khởi Chạy Lần Đầu

- **Màn hình khởi động**: Hiển thị tiến trình trong khi tải tài khoản, kiểm tra bản quyền
- **Chọn ngôn ngữ**: Lần đầu mở sẽ hỏi Tiếng Việt / Tiếng Anh (thay đổi sau trong Cài đặt)

### 1.5 Giao Diện Chính

| Vùng | Mô tả |
|------|--------|
| **Thanh bên (trái)** | Điều hướng: Tạo Ảnh, Tạo Video, Workflow, Webhook + Cài đặt/Nhật ký/Cộng đồng |
| **Vùng nội dung (phải)** | Nội dung trang được chọn |



| Huy hiệu gói | Màu nền |
|---------------|---------|
| 🌱 BASIC | Xám |
| 💎 PLUS | Xanh dương |
| 👑 MAX | Tím |

Hỗ trợ **chế độ Tối / Sáng** (nút ở cuối thanh bên), và **thanh thông báo** tin nhắn hệ thống + cập nhật tự động.

</details>

---

<a name="phan-2"></a>
<details>
<summary><h2>👤 Phần 2: Quản Lý Tài Khoản</h2></summary>

Quản lý tài khoản nằm trong **Cài đặt → tab Tài khoản**.

> ⚠️ Cần ít nhất **một tài khoản Google hợp lệ** với phiên đăng nhập còn hạn để sử dụng tính năng tạo ảnh/video.

### 2.1 Thêm Tài Khoản

**Đăng nhập qua trình duyệt (khuyến nghị):**
1. Nhấn **"Đăng nhập qua trình duyệt"**
2. Đăng nhập Google trong cửa sổ Chromium
3. Hệ thống tự động trích xuất dữ liệu phiên, email, hạng tài khoản, số dư

**Nhập cookie thủ công:** Dán cookie từ trình duyệt vào ô nhập.

### 2.2 Bảng Tài Khoản (9 cột)

| Cột | Mô tả |
|-----|--------|
| Bật/Tắt | Kích hoạt hoặc vô hiệu tài khoản |
| Email | Địa chỉ email |
| Hạng | Free / Pro / Ultra |
| Số dư | Credits còn lại |
| Proxy | Proxy riêng |
| Hạn cookie | Thời điểm hết hạn phiên |
| Hạn token | Thời điểm hết hạn xác thực |
| Trạng thái | ✅ Hợp lệ / ❌ Hết hạn / 🔄 Đang kiểm tra / ⚠️ Trùng |
| Hành động | Gia hạn / Sửa / Xóa |

### 2.3 Gia Hạn Phiên

- **Bước 1**: Làm mới cookie (mở trình duyệt → đăng nhập lại)
- **Bước 2**: Kiểm tra hạng (gọi API → lấy token mới + hạng + số dư)
- Hỗ trợ **gia hạn hàng loạt** (tất cả tài khoản) và **tự động gia hạn** khi khởi động (Plus/Max)

### 2.4 Sửa & Xóa

- **Sửa**: Xem/chỉnh proxy cho từng tài khoản (`IP:Cổng` hoặc `IP:Cổng:Tên:Mật khẩu`)
- **Xóa**: Xóa vĩnh viễn tài khoản + hồ sơ trình duyệt

</details>

---

<a name="phan-3"></a>
<details>
<summary><h2>🔑 Phần 3: Hệ Thống Bản Quyền</h2></summary>

Bản quyền nằm trong **Cài đặt → tab Bản quyền**.

### 3.1 Bảng So Sánh

| Tính năng | 🌱 Basic | 💎 Plus | 👑 Max |
|-----------|---------|---------|--------|
| **Số luồng** | 1 | 10 | ♾️ |
| **Hàng đợi** | 1 tác vụ | 10 tác vụ | ♾️ |
| **Ảnh/Prompt** | 1 | 4 | 4 |
| **Video/Prompt** | 1 | 4 | 4 |
| **Giới hạn prompt** | 10 dòng | ♾️ | ♾️ |
| **Nhóm proxy** | ❌ | ✅ | ✅ |
| **Nâng cấp ảnh** | ❌ | ✅ (2K+4K) | ✅ (2K+4K) |
| **Nâng cấp video** | ❌ | ✅ | ✅ |
| **Video đầy đủ** | Chỉ Đầu+Cuối | ✅ | ✅ |
| **Luồng làm việc** | ❌ | ✅ | ✅ |
| **Thử lại lỗi** | ❌ | ✅ | ✅ |
| **Tự động gia hạn** | ❌ | ✅ | ✅ |
| **Webhook API** | ❌ | ❌ | ✅ |

### 3.2 Đăng Nhập

1. Nhấn **"Đăng nhập Google"**
2. Chọn tài khoản → xác thực → nhận gói, mã người dùng, ngày hết hạn
3. Phiên lưu trên máy, không cần đăng nhập lại

> ⚠️ Mỗi bản quyền chỉ hoạt động trên **một thiết bị**. Đăng nhập thiết bị khác sẽ đăng xuất thiết bị cũ.

### 3.3 Thanh Toán

<details>
<summary>💳 Bảng giá & phương thức (nhấn để mở)</summary>

**Gói PLUS 💎:**

| Thời hạn | VND |
|----------|-----|
| 1 tháng | 79,000 |
| 6 tháng | 399,000 |
| 1 năm | 799,000 |

**Gói MAX 👑:**

| Thời hạn | VND |
|----------|-----|
| 1 tháng | 149,000 |
| 6 tháng | 749,000 |
| 1 năm | 1,499,000 |

**Phương thức:**
- 🇻🇳 **Chuyển khoản VietQR** (VPBank, tự động tạo mã QR)
- 🌍 **Thẻ quốc tế** (Visa/MC/Apple Pay/Google Pay qua Polar)

</details>

### 3.4 Kiểm Tra Tự Động

Hệ thống tự động kiểm tra trạng thái bản quyền định kỳ (im lặng, không hiện thông báo).

</details>

---

<a name="phan-4"></a>
<details>
<summary><h2>🖼️ Phần 4: Tạo Ảnh AI ⭐</h2></summary>

### 4.1 Giao Diện

| Vùng | Mô tả |
|------|--------|
| **Bảng điều khiển trái** | Cấu hình + Nhập prompt + Nút điều khiển |
| **Bảng điều khiển phải** | Bảng prompt 7 cột + Thanh công cụ |

### 4.2 Ba Model AI

| Đặc điểm | 🍌 Nano Banana | 🖼️ Imagen 4 | 🍌 Nano Banana Pro |
|-----------|---------------|-------------|-------------------|
| **Chế độ tham chiếu** | Whisk — chia danh mục | Whisk — chia danh mục | Flow — không chia danh mục |
| **Số ảnh tham chiếu** | 5 (Chủ thể ×3, Cảnh ×1, Phong cách ×1) | 5 (Chủ thể ×3, Cảnh ×1, Phong cách ×1) | 10 (ảnh chung, không phân loại) |
| **Captcha** | ❌ | ❌ | ✅ Tự động |
| **Tỷ lệ** | 16:9, 9:16, 1:1 | 16:9, 9:16, 4:3, 3:4, 1:1 | 16:9, 9:16 |

### 4.3 Cấu Hình

| Mục | Chi tiết |
|-----|----------|
| **Model** | Nano Banana (mặc định), Imagen 4, Nano Banana Pro |
| **Chất lượng** | 1K (gốc), 2K (Plus/Max), 4K (Plus/Max + tài khoản Ultra) |
| **Tỷ lệ** | Tùy model (xem bảng trên) |
| **Số ảnh/Prompt** | 1-4 (Basic: chỉ 1) |
| **Số luồng** | Basic: 1 / Plus: 10 / Max: ♾️ |
| **Độ trễ** | 2-3 giây mặc định, phạm vi 1-300 giây |
| **Chế độ tham chiếu** | Mặc định, 1 cho tất cả (Plus/Max), Chạy tuần tự (Plus/Max) |
| **Khóa Seed** | 6 chữ số, tái tạo kết quả |
| **Nhập prompt** | TXT, Excel (.xlsx/.xls) |
| **Chế độ lưu** | Không thư mục con / Theo tác vụ / Theo prompt |

### 4.4 Tự Động Khớp Ảnh Tham Chiếu

Tự động gán ảnh tham chiếu từ thư mục dựa trên **từ khóa trong tên file** khớp với prompt.

- Tách từ khóa theo `_`, tối thiểu 3 ký tự
- Ví dụ: `red_car.png` → từ khóa `red_car`, `red`, `car`

### 4.5 Bảng Prompt (7 cột)

| Cột | Mô tả |
|-----|--------|
| ☑️ Chọn | Đánh dấu dòng |
| STT | Số thứ tự |
| Tác vụ | Tên tác vụ hàng đợi |
| Ảnh tham chiếu | Ảnh mẫu (nhấn để thêm/xóa) |
| Prompt | Nội dung (chỉnh sửa trực tiếp) |
| Kết quả | Ảnh đã tạo (hình thu nhỏ) |
| Tiến trình | Trạng thái + nút Thử lại/Mở thư mục |

**Thanh công cụ:** Thêm dòng, Thêm ảnh, Xóa, Xóa hết, Chạy lại lỗi, Chạy mục chọn, Load Session, Bộ lọc (Tất cả/Chờ/Đang chạy/Thành công/Lỗi)

### 4.6 Quản Lý Hàng Đợi

- **Thêm vào hàng đợi**: Lưu cấu hình hiện tại thành tác vụ
- **Quản lý hàng đợi**: Đổi tên, Bỏ qua, Thử lại, Xóa, Chỉnh sửa tác vụ
- **Giới hạn**: Basic 1 / Plus 10 / Max ♾️

### 4.7 Phiên & Sao Lưu

- Tự động sao lưu sau mỗi thay đổi (tối đa 10 file)
- Tải phiên: phục hồi hoặc xuất ra Excel

</details>

---

<a name="phan-5"></a>
<details>
<summary><h2>🎬 Phần 5: Tạo Video AI 🎬</h2></summary>

### 5.1 Giao Diện

| Vùng | Mô tả |
|------|--------|
| **Bảng điều khiển trái** | Cấu hình + Nút điều khiển |
| **Bảng điều khiển phải** | 3 tab + Thanh công cụ |

> ⚠️ Yêu cầu tài khoản Google hạng **PRO** hoặc **ULTRA**. Model Fast [0 Credit] chỉ hỗ trợ hạng **ULTRA**.

### 5.2 Ba Model Video

| Model | Credit | Mô tả |
|-------|--------|-------|
| **Fast [0 Credit]** | 0 | Nhanh, ưu tiên thấp (chỉ ULTRA) |
| **Fast [10 Credit]** | 10 | Nhanh, ưu tiên bình thường (mặc định) |
| **Quality [100 Credit]** | 100 | Chất lượng cao nhất (không hỗ trợ ghép 3 ảnh) |

**Chế độ tạo:** Văn bản thành video, Ảnh đầu, Ảnh đầu+cuối, Ghép 3 ảnh thành phần

### 5.3 Cấu Hình

| Mục | Chi tiết |
|-----|----------|
| **Độ phân giải** | 720p (gốc), 1080p (Plus/Max), 4K (Plus/Max) |
| **Tỷ lệ** | 16:9, 9:16 |
| **Số video/Prompt** | 1-4 (Basic: chỉ 1) |
| **Số luồng** | Basic: 1 / Plus: 10 / Max: ♾️ |
| **Độ trễ** | 10-20 giây mặc định |

### 5.4 Chế Độ Ghép Cặp (Tab 1: Tạo video từ văn bản - hình ảnh)

| Chế độ | Mô tả |
|--------|-------|
| **Ảnh Đầu - Ảnh Cuối** | Ghép cặp 1:1 |
| **1 Ảnh Đầu - Nhiều Ảnh Cuối** | 1 ảnh đầu → N ảnh cuối (Plus/Max) |
| **Nhiều Ảnh Đầu - 1 Ảnh Cuối** | N ảnh đầu → 1 ảnh cuối (Plus/Max) |
| **Ghép Nối Tiếp (1:2 -> 2:3...)** | Chuỗi: ảnh cuối N = ảnh đầu N+1 (Plus/Max) |

### 5.5 Tab 2: Tạo video từ các thành phần ⭐ (Plus/Max)

Kết hợp **3 ảnh thành phần** thành video. Hỗ trợ tự động khớp từ khóa từ thư mục.

### 5.6 Tab 3: Xây Dựng Khung Hình 🎞️ (Plus/Max)

Chuỗi video **liên tiếp** — khung hình cuối video N = ảnh đầu video N+1.

| Đặc điểm | Chi tiết |
|-----------|----------|
| Độ phân giải | Chỉ 720p |
| Số luồng | Chỉ 1 (tuần tự) |
| Ảnh tham chiếu | Chỉ video đầu tiên |

### 5.7 Phân Quyền Theo Gói

| Tính năng | Basic | Plus | Max |
|-----------|-------|------|-----|
| Tab 1 (Tạo video từ văn bản - hình ảnh) | ✅ | ✅ | ✅ |
| Tab 2 (Tạo video từ các thành phần) | ❌ | ✅ | ✅ |
| Tab 3 (Xây Dựng Khung Hình) | ❌ | ✅ | ✅ |
| Độ phân giải | 720p | +1080p+4K | +1080p+4K |

</details>

---

<a name="phan-6"></a>
<details>
<summary><h2>🔗 Phần 6: Biên Tập Quy Trình (Workflow) 🔗</h2></summary>

Trình soạn thảo trực quan dạng node — kết nối các bước xử lý thành luồng tự động.

> ⚠️ Trình soạn luồng yêu cầu gói **Plus hoặc Max** để chạy.

### 6.1 Giao Diện

| Vùng | Mô tả |
|------|--------|
| **Thanh công cụ** | Lưu Flow, Mở Flow, Sắp xếp, Dừng, Chạy Flow |
| **Vùng vẽ** | Khu vực đặt node + kết nối |
| **Bản đồ thu nhỏ** | Tổng quan toàn bộ luồng |
| **Bảng nhật ký** | Nhật ký thực thi |

### 6.2 Thao Tác

| Thao tác | Hành động |
|----------|-----------|
| Chuột phải | Menu thêm node |
| Kéo thả | Di chuyển node |
| Cuộn chuột | Phóng to/thu nhỏ |
| Ctrl+Z/Y | Hoàn tác/Làm lại |
| Ctrl+C/V | Sao chép/Dán |
| Delete | Xóa node/kết nối |

### 6.3 Các Loại Node

| Node | Đầu vào | Đầu ra | Mô tả |
|------|---------|--------|-------|
| 📦 **Tải Ảnh Hàng Loạt** | — | Ảnh | Tải ảnh từ thư mục (sắp xếp: A-Z, mới/cũ, ngẫu nhiên, khớp prompt) |
| 📝 **Tải Prompt Hàng Loạt** | — | Prompt | Danh sách prompt (tuần tự/ngẫu nhiên, giới hạn số lượng) |
| 🖼 **Tạo Ảnh** | Tham chiếu, Prompt | Ảnh | Tạo ảnh (3 model, tỷ lệ, seed, độ phân giải) |
| 📎 **Ảnh Tham Chiếu** | Ảnh | Tham chiếu | Dùng cho Nano Banana / Imagen 4 — chia danh mục (Chủ thể/Cảnh/Phong cách) |
| 📎 **Ảnh Tham Chiếu (Pro)** | Ảnh | Tham chiếu | Dùng cho Nano Banana Pro — không phân danh mục |
| 💾 **Lưu Ảnh** | Ảnh | — | Lưu ảnh (tiền tố + thư mục) |
| 🔀 **Chuyển Hướng** | Bất kỳ | Bất kỳ | Định tuyến lại kết nối |

### 6.4 Thực Thi

Dùng thuật toán **sắp xếp topo** để xác định thứ tự chạy. Trạng thái node: 🟡 Đang chạy / 🟢 Thành công / 🔴 Lỗi / Tím = Bỏ qua / Xám = Vô hiệu.

### 6.5 Lưu / Mở

- Lưu và mở file **JSON**
- Luồng mặc định tự động tải khi mở ứng dụng

</details>

---

<a name="phan-7"></a>
<details>
<summary><h2>🔧 Phần 7: Tính Năng Bổ Sung 🔧</h2></summary>

### 7.1 Máy Chủ Webhook API (chỉ gói Max)

Máy chủ API cục bộ cho công cụ bên ngoài (n8n, Make.com, Zapier, Python...).

> 📖 **Tài liệu tích hợp chi tiết:** [WEBHOOK_API_GUIDE_VI.md](WEBHOOK_API_GUIDE_VI.md) (bao gồm code mẫu Python, JavaScript, cURL)

| Đường dẫn | Phương thức | Xác thực | Mô tả |
|------------|-------------|----------|-------|
| `/api/health` | GET | ❌ | Kiểm tra máy chủ |
| `/api/image/generate` | POST | ✅ | Gửi yêu cầu tạo ảnh |
| `/api/video/generate` | POST | ✅ | Gửi yêu cầu tạo video |
| `/api/status/{task_id}` | GET | ✅ | Xem trạng thái |
| `/api/result/{task_id}` | GET | ✅ | Lấy kết quả |
| `/api/files/{filename}` | GET | ✅ | Tải file về |
| `/api/tasks` | GET | ✅ | Danh sách tác vụ |

**Xác thực:** Thêm header `X-API-Key: KHÓA_CỦA_BẠN` | **Cổng:** 1024-65535 (mặc định 8765)

```json
// POST /api/image/generate
{
  "prompt": "mô tả ảnh",
  "model": "imagen4",
  "aspect_ratio": "16:9"
}
```

### 7.2 Cài Đặt (6 Tab)

| Tab | Nội dung |
|-----|----------|
| **Tài khoản Google** | Thêm/xóa/gia hạn tài khoản Google |
| **Proxy Pool** | Proxy tùy chỉnh (HTTP/SOCKS5) + WARP VPN + Tự động xoay IP |
| **Bản quyền & Nâng cấp** | Đăng nhập/mua Plus/Max, tự động kiểm tra định kỳ |
| **Chế độ Xác thực** | Chọn chế độ: Tích hợp sẵn hoặc Chrome Extension ([xem 7.5](#ext-auth)) |
| **Cài đặt chung** | Ngôn ngữ (9+), giao diện sáng/tối, thông tin tác giả |
| **Logs** | Nhật ký hoạt động chi tiết |

### 7.3 Khôi Phục & Xử Lý Lỗi

| Loại lỗi | Xử lý tự động |
|-----------|-------|
| 403 (Bị từ chối) | Làm mới phiên → xoay tài khoản |
| Hết thời gian chờ | Thử lại tối đa 3 lần |
| Captcha (Tích hợp) | Trình duyệt tự giải, 10 lượt/phiên, xoay profile |
| Captcha (Extension) | Chrome Extension tự xử lý, yêu cầu giữ Chrome mở |
| Mất kết nối | WARP tự kết nối lại |
| Hết tài khoản | Dừng sau 5 lần liên tiếp không tìm được |

### 7.4 Cập Nhật Tự Động

Ứng dụng kiểm tra phiên bản từ máy chủ → thông báo → tải xuống → thay file → khởi động lại.

<a name="ext-auth"></a>

### 7.5 Chế Độ Xác Thực Chrome Extension 🧩

Ngoài chế độ xác thực **Tích hợp sẵn** (tự động sử dụng trình duyệt Chromium nội bộ), ứng dụng hỗ trợ chế độ **Chrome Extension** — sử dụng phiên trình duyệt Chrome thật của bạn.

**So sánh 2 chế độ:**

| | 🖥️ Tích hợp sẵn (Mặc định) | 🧩 Chrome Extension |
|---|---|---|
| **Thiết lập** | Không cần | Cài extension từ Chrome Web Store |
| **Trình duyệt** | Chromium tự động (ẩn) | Chrome thật của bạn |
| **Ưu điểm** | Hoàn toàn tự động, không cần tương tác | Dùng phiên đăng nhập thật, ổn định hơn |
| **Yêu cầu** | Không | Giữ Chrome mở với trang Labs |

**Cách thiết lập Chrome Extension:**

1. **Cài đặt Extension** — Cài extension **G-Labs Automation - Auth Helper** từ Chrome Web Store (nút trong Cài đặt → Chế độ Xác thực)
2. **Đăng nhập Google Labs** — Mở [labs.google/fx/tools/flow](https://labs.google/fx/tools/flow) trong Chrome và đăng nhập tài khoản Google
3. **Bắt đầu tạo ảnh** — Quay lại G-Labs Automation, extension tự động kết nối và xử lý xác thực ở nền

> ⚠️ **Lưu ý:** Giữ Chrome mở cùng trang Labs trong khi tạo ảnh hoặc video. Nếu gặp lỗi, tải lại trang Labs trên Chrome.

**Trạng thái kết nối:** Xem trong Cài đặt → Chế độ Xác thực:
- 🟢 **Extension: Đã kết nối** — Sẵn sàng hoạt động
- 🔴 **Extension: Chưa kết nối** — Kiểm tra Chrome đã mở trang Labs chưa

### 7.6 Phần Mềm Xung Đột

Các phần mềm sau có thể gây **xung đột** khi chạy đồng thời với G-Labs Automation, dẫn đến lỗi `Environment check failed — worker cannot start`.

| # | Phần mềm | Loại |
|---|---|---|
| 1 | **Fiddler** / Fiddler Everywhere | HTTP Debugging Proxy |
| 2 | **Charles Proxy** | HTTP Proxy / Monitor |
| 3 | **mitmproxy** / mitmweb / mitmdump | Network Proxy Tool |
| 4 | **Burp Suite** / Burp Loader | Web Testing Platform |
| 5 | **Proxyman** | HTTP Debugging Proxy |
| 6 | **HTTP Toolkit** | HTTP Debugging Tool |
| 7 | **HTTP Debugger Pro** | Network Analyzer |
| 8 | **Reqable** | API Debugging Tool |
| 9 | **OWASP ZAP** (Zed Attack Proxy) | Web App Scanner |

> ⚠️ **Khắc phục:** Tắt hoàn toàn phần mềm xung đột trước khi sử dụng, sau đó khởi động lại ứng dụng.

</details>

---

<p align="center">
  <b>G-Labs Automation</b> — Tạo ảnh & video AI hàng loạt<br><br>
  🌐 <a href="https://duckmartians.info/">duckmartians.info</a> · 💬 <a href="https://discord.gg/munMZEBMw5">Discord Community</a><br>
  <b>Tác giả: Đặng Minh Đức</b> · <a href="https://github.com/duckmartians">@duckmartians</a>
</p>
