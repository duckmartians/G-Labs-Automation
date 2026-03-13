<p align="center">
  <a href="N8N_INTEGRATION_GUIDE_EN.md"><img src="https://img.shields.io/badge/English-blue" alt="English"></a>
  <a href="N8N_INTEGRATION_GUIDE_VI.md"><img src="https://img.shields.io/badge/Tiếng%20Việt-green" alt="Tiếng Việt"></a>
</p>

# 🔗 Hướng Dẫn Tích Hợp G-Labs Webhook vào n8n — Từng Bước Chi Tiết

> **Tạo ảnh & video AI tự động** bằng cách kết nối G-Labs Automation với n8n — nền tảng tự động hóa workflow mã nguồn mở.

> ⚠️ **Yêu cầu:** Gói **MAX** để sử dụng Webhook API.

---

## 📑 Mục Lục

| # | Nội dung |
|---|---------|
| 1 | [Tổng Quan](#tong-quan) |
| 2 | [Chuẩn Bị](#chuan-bi) |
| 3 | [Bật Webhook Server trong G-Labs](#bat-webhook) |
| 4 | [Workflow 1: Tạo Ảnh AI](#workflow-anh) |
| 5 | [Workflow 2: Tạo Video AI](#workflow-video) |
| 6 | [Workflow 3: Tạo Hàng Loạt (Batch)](#workflow-batch) |
| 7 | [Xử Lý Lỗi & Mẹo Nâng Cao](#xu-ly-loi) |
| 8 | [Tham Khảo API](#tham-khao) |

---

<a name="tong-quan"></a>

## 1. 🌟 Tổng Quan

### G-Labs Webhook API là gì?

G-Labs Automation cung cấp một **máy chủ REST API cục bộ** chạy trên máy tính của bạn. Bạn có thể gửi yêu cầu tạo ảnh/video AI từ bất kỳ công cụ nào — bao gồm **n8n**.

### Tại sao dùng n8n?

- **Tự động hóa hoàn toàn**: Lên lịch tạo ảnh/video theo giờ, theo sự kiện
- **Kết hợp đa nền tảng**: Nhận prompt từ Google Sheets, Telegram, email → tạo ảnh → gửi kết quả
- **Giao diện kéo thả**: Không cần viết code
- **Miễn phí & tự host**: Chạy trên máy tính cá nhân

### Luồng hoạt động

```
n8n Workflow → [HTTP Request] → G-Labs Webhook API (127.0.0.1:8765)
                                        ↓
                              Thêm vào hàng đợi G-Labs
                                        ↓
                              G-Labs tự động tạo ảnh/video
                                        ↓
n8n Workflow ← [Polling / Check status] ← Trả kết quả (file URL)
```

---

<a name="chuan-bi"></a>

## 2. 🛠️ Chuẩn Bị

### 2.1 Yêu cầu

| Thành phần | Chi tiết |
|-----------|----------|
| **G-Labs Automation** | Đã cài đặt, gói **MAX** đang hoạt động |
| **n8n** | Đã cài đặt ([hướng dẫn cài n8n](#cai-n8n)) |
| **Tài khoản Google** | Ít nhất 1 tài khoản hợp lệ trong G-Labs |

<a name="cai-n8n"></a>

### 2.2 Cài đặt n8n (nếu chưa có)

**Cách 1 — n8n Desktop (đơn giản nhất):**

1. Truy cập [n8n.io/get-started](https://n8n.io/get-started)
2. Tải bản **Desktop** cho Windows
3. Cài đặt và mở n8n → giao diện mở tại `http://localhost:5678`

**Cách 2 — npm (nếu có Node.js):**

```bash
npm install n8n -g
n8n start
```

**Cách 3 — Docker:**

```bash
docker run -it --rm --name n8n -p 5678:5678 n8nio/n8n
```

> 💡 **Lưu ý quan trọng:** n8n và G-Labs **phải chạy trên cùng một máy tính** (vì Webhook API chỉ lắng nghe trên `127.0.0.1`).

---

<a name="bat-webhook"></a>

## 3. 🚀 Bật Webhook Server trong G-Labs

### Bước 3.1 — Mở trang Webhook

1. Mở **G-Labs Automation**
2. Ở thanh bên trái, nhấn vào biểu tượng **Webhook** (🔗)

### Bước 3.2 — Lấy API Key

1. Trong trang Webhook, bạn sẽ thấy trường **API Key** — đây là khóa xác thực
2. Nhấn nút **Sao chép** để copy API Key
3. **Lưu lại** API Key này — bạn sẽ cần nó khi cấu hình n8n

> 💡 Nếu muốn tạo key mới, nhấn nút **Tạo lại** (🔄). Key cũ sẽ không còn hoạt động.

### Bước 3.3 — Cấu hình cổng (Port)

1. Cổng mặc định: **8765**
2. Nếu cổng bị trùng hoặc bạn muốn đổi, nhập số cổng mới (phạm vi: 1024–65535)

### Bước 3.4 — Bật Server

1. Nhấn nút **Bật Server** (hoặc toggle ON)
2. Trạng thái chuyển sang **🟢 Đang chạy**
3. Kiểm tra nhanh: mở trình duyệt, truy cập `http://127.0.0.1:8765/api/health`

Nếu thấy phản hồi như dưới đây, server đã sẵn sàng:

```json
{
  "status": "ok",
  "server": "G-Labs Webhook",
  "uptime": 10,
  "tasks_pending": 0,
  "tasks_running": 0
}
```

---

<a name="workflow-anh"></a>

## 4. 🖼️ Workflow 1: Tạo Ảnh AI

> **Mục tiêu:** Gửi 1 prompt → G-Labs tạo ảnh → Lấy kết quả về.

### 📋 Tổng quan luồng xử lý

```
[Manual Trigger] → [HTTP: Tạo ảnh] → [Wait 30s] → [HTTP: Kiểm tra] → [IF hoàn thành?]
                                                                            ↓ Yes
                                                                    [HTTP: Tải file]
                                                                            ↓ No
                                                                    [Quay lại Wait]
```

---

### Bước 4.1 — Tạo Workflow mới

1. Mở n8n (`http://localhost:5678`)
2. Nhấn **+ Add workflow** (hoặc **Tạo workflow mới**)
3. Đặt tên: `G-Labs Tạo Ảnh AI`

---

### Bước 4.2 — Thêm node "Manual Trigger"

1. Workflow mới sẽ tự có node **Manual Trigger** (nút kích hoạt thủ công)
2. Giữ nguyên, không cần chỉnh gì

---

### Bước 4.3 — Thêm node "HTTP Request" (Gửi yêu cầu tạo ảnh)

1. Nhấn nút **+** sau Manual Trigger → chọn **HTTP Request**
2. Cấu hình:

| Thuộc tính | Giá trị |
|-----------|---------|
| **Method** | `POST` |
| **URL** | `http://127.0.0.1:8765/api/image/generate` |

3. Mở phần **Headers** → nhấn **Add Header**:

| Header Name | Header Value |
|------------|-------------|
| `X-API-Key` | `PASTE_API_KEY_CỦA_BẠN` |

4. Mở phần **Body** → chọn **JSON** → nhập:

```json
{
  "prompt": "a beautiful sunset over the ocean, cinematic, 8k",
  "model": "imagen4",
  "count": 2,
  "aspect_ratio": "16:9",
  "upscale": ["2K"],
  "reference_images": []
}
```

> 💡 **Các model có sẵn:** `imagen4`, `nano_banana`, `nano_banana_2`, `nano_banana_pro`

> 💡 **Tỉ lệ có sẵn:** `1:1`, `16:9`, `9:16`, `4:3`, `3:4`

5. Đổi tên node: click đúp vào tên → đổi thành `Tạo Ảnh`

---

### Bước 4.4 — Thêm node "Wait" (Chờ xử lý)

1. Nhấn **+** sau node "Tạo Ảnh" → tìm **Wait**
2. Cấu hình:

| Thuộc tính | Giá trị |
|-----------|---------|
| **Resume** | `After Time Interval` |
| **Amount** | `30` |
| **Unit** | `Seconds` |

> 💡 Thời gian tạo ảnh thường **10–60 giây**. Đặt 30 giây là hợp lý cho lần kiểm tra đầu tiên.

---

### Bước 4.5 — Thêm node "HTTP Request" (Kiểm tra trạng thái)

1. Nhấn **+** sau node Wait → chọn **HTTP Request**
2. Cấu hình:

| Thuộc tính | Giá trị |
|-----------|---------|
| **Method** | `GET` |
| **URL** | `http://127.0.0.1:8765/api/status/{{ $('Tạo Ảnh').item.json.task_id }}` |

3. Thêm **Header** giống bước 4.3:

| Header Name | Header Value |
|------------|-------------|
| `X-API-Key` | `PASTE_API_KEY_CỦA_BẠN` |

4. Đổi tên node: `Kiểm Tra Trạng Thái`

> ⚠️ **Quan trọng:** Biểu thức `{{ $('Tạo Ảnh').item.json.task_id }}` sẽ tự động lấy `task_id` từ kết quả của node "Tạo Ảnh". Đảm bảo tên node khớp chính xác.

---

### Bước 4.6 — Thêm node "IF" (Kiểm tra hoàn thành)

1. Nhấn **+** sau node "Kiểm Tra Trạng Thái" → tìm **IF**
2. Cấu hình điều kiện:

| Thuộc tính | Giá trị |
|-----------|---------|
| **Value 1** | `{{ $json.status }}` |
| **Operation** | `is equal to` |
| **Value 2** | `completed` |

3. Đổi tên: `Đã Hoàn Thành?`

---

### Bước 4.7 — Khi chưa xong → Quay lại chờ tiếp

1. Kéo output **false** của node "Đã Hoàn Thành?" → nối **ngược** lại node **Wait**
2. Điều này tạo vòng lặp: nếu chưa xong, chờ 30 giây rồi kiểm tra lại

> 💡 n8n tự động xử lý vòng lặp. Workflow sẽ tiếp tục polling cho đến khi status = `completed` hoặc `failed`.

---

### Bước 4.8 — Khi hoàn thành → Lấy kết quả

1. Kéo output **true** của node "Đã Hoàn Thành?" → thêm node **HTTP Request** mới
2. Cấu hình:

| Thuộc tính | Giá trị |
|-----------|---------|
| **Method** | `GET` |
| **URL** | `{{ $('Kiểm Tra Trạng Thái').item.json.results[0] }}` |

3. Thêm **Header**: `X-API-Key: PASTE_API_KEY_CỦA_BẠN`
4. Mở phần **Options** (nếu muốn tải file):
   - **Response Format**: `File`
5. Đổi tên: `Tải Ảnh Kết Quả`

> 💡 `results` là mảng URL. `results[0]` là ảnh đầu tiên, `results[1]` là ảnh thứ 2...

---

### Bước 4.9 — Chạy thử!

1. Đảm bảo G-Labs đang chạy + Webhook Server **🟢 ON**
2. Nhấn nút **Test workflow** (▶️) trong n8n
3. Theo dõi từng node sáng lên khi thực thi
4. Kết quả cuối cùng sẽ là file ảnh tải về

---

<a name="workflow-video"></a>

## 5. 🎬 Workflow 2: Tạo Video AI

> Tương tự workflow ảnh, chỉ thay đổi endpoint và tham số.

### Bước 5.1 — Tạo Workflow mới

Tạo workflow `G-Labs Tạo Video AI`, hoặc sao chép workflow ảnh rồi chỉnh sửa.

### Bước 5.2 — Node "HTTP Request" (Tạo video)

| Thuộc tính | Giá trị |
|-----------|---------|
| **Method** | `POST` |
| **URL** | `http://127.0.0.1:8765/api/video/generate` |

**Body (JSON):**

```json
{
  "prompt": "a golden retriever running on the beach, slow motion, cinematic",
  "model": "veo_31_fast",
  "mode": "text_to_video",
  "aspect_ratio": "16:9",
  "resolution": ["720p", "1080p"]
}
```

**Các model video có sẵn:**

| Model | Credit | Ghi chú |
|-------|--------|---------|
| `veo_31_fast_relaxed` | 0 | Mặc định, ưu tiên thấp (chỉ tài khoản ULTRA) |
| `veo_31_fast` | 10 | Ưu tiên bình thường |
| `veo_31_quality` | 100 | Chất lượng cao nhất |

### Bước 5.3 — Tăng thời gian Wait

Video mất **1–5 phút** để tạo. Đặt **Wait = 60 giây** (hoặc hơn) giữa mỗi lần kiểm tra.

### Bước 5.4 — Các node còn lại

Giữ nguyên: **Kiểm Tra Trạng Thái** → **IF** → **Tải File** (giống workflow ảnh).

---

<a name="workflow-batch"></a>

## 6. 📦 Workflow 3: Tạo Hàng Loạt (Batch)

> **Mục tiêu:** Đọc danh sách prompt từ Google Sheets / file CSV → tạo ảnh hàng loạt.

### 📋 Tổng quan luồng

```
[Schedule Trigger] → [Google Sheets: Đọc prompt] → [Loop Over Items]
                                                         ↓
                            [HTTP: Tạo ảnh] → [Wait] → [HTTP: Kiểm tra]
                                                         ↓
                                              [IF] → [Lưu kết quả]
```

### Bước 6.1 — Thêm node trigger

Chọn 1 trong:
- **Schedule Trigger**: Chạy tự động theo lịch (ví dụ: 6h sáng mỗi ngày)
- **Webhook Trigger**: Kích hoạt từ bên ngoài (Telegram, Slack...)
- **Manual Trigger**: Kích hoạt thủ công

### Bước 6.2 — Đọc danh sách prompt

**Từ Google Sheets:**
1. Thêm node **Google Sheets** → chọn **Read Rows**
2. Kết nối tài khoản Google
3. Chọn spreadsheet chứa danh sách prompt
4. Cột ví dụ: `A` = Prompt, `B` = Model, `C` = Tỉ lệ

**Từ file CSV/JSON (thay thế):**
1. Thêm node **Read/Write Files from Disk**
2. Trỏ đến file prompt

### Bước 6.3 — Xử lý từng prompt

1. Thêm node **Loop Over Items** (hoặc **Split In Batches**)
2. Batch size: `1` (gửi từng prompt một để tránh quá tải)

### Bước 6.4 — Gửi yêu cầu tạo ảnh (trong vòng lặp)

Giống Bước 4.3, nhưng thay prompt cố định bằng biểu thức động:

**Body (JSON):**
```json
{
  "prompt": "={{ $json.prompt }}",
  "model": "={{ $json.model || 'imagen4' }}",
  "count": 1,
  "aspect_ratio": "={{ $json.aspect_ratio || '16:9' }}"
}
```

> 💡 `$json.prompt` lấy giá trị cột prompt từ Google Sheets. `||` đặt giá trị mặc định nếu cột trống.

### Bước 6.5 — Vòng lặp polling + xử lý kết quả

Giống Bước 4.4–4.8. Sau khi lấy kết quả, connect về **Loop Over Items** để tiếp tục prompt tiếp theo.

---

<a name="xu-ly-loi"></a>

## 7. ⚠️ Xử Lý Lỗi & Mẹo Nâng Cao

### 7.1 Xử lý task thất bại

Thêm nhánh xử lý khi task `failed`:

1. Trong node **IF** "Đã Hoàn Thành?", thêm nhánh kiểm tra lỗi
2. Thêm node **IF** thứ 2 sau nhánh `false`:

| Thuộc tính | Giá trị |
|-----------|---------|
| **Value 1** | `{{ $json.status }}` |
| **Operation** | `is equal to` |
| **Value 2** | `failed` |

3. Nhánh **true** (failed) → Gửi thông báo lỗi (Email, Telegram, Slack...)
4. Nhánh **false** (pending/running) → Quay lại **Wait** để tiếp tục polling

### 7.2 Bảng mã lỗi HTTP

| Mã | Ý nghĩa | Hành động |
|----|---------|-----------|
| `200` | Thành công | — |
| `202` | Đã nhận tác vụ | Bắt đầu polling |
| `400` | Yêu cầu sai (thiếu prompt, JSON không hợp lệ) | Kiểm tra body request |
| `401` | API Key sai hoặc thiếu | Kiểm tra header `X-API-Key` |
| `404` | Không tìm thấy task/file | Kiểm tra `task_id` |
| `500` | Lỗi server | Kiểm tra G-Labs còn chạy không |

### 7.3 Mẹo tối ưu

| Mẹo | Chi tiết |
|-----|---------|
| **Kiểm tra health trước** | Thêm node GET `/api/health` ở đầu workflow để đảm bảo server online |
| **Timeout bảo vệ** | Thêm counter đếm số lần poll. Nếu > 20 lần (10 phút) → dừng và báo lỗi |
| **Lưu credential** | Trong n8n, vào **Settings → Credentials** → tạo **Header Auth** để tái sử dụng API Key |
| **Song song** | Gửi nhiều request cùng lúc (G-Labs tự quản lý hàng đợi nội bộ) |
| **Retry** | Dùng node **Error Trigger** của n8n + **Retry on Fail** trên HTTP Request |

### 7.4 Thiết lập Credential tái sử dụng

Thay vì nhập API Key thủ công ở mỗi node HTTP Request:

1. Vào node HTTP Request → mở **Authentication**
2. Chọn **Header Auth**
3. Cấu hình:

| Thuộc tính | Giá trị |
|-----------|---------|
| **Name** | `X-API-Key` |
| **Value** | `PASTE_API_KEY_CỦA_BẠN` |

4. Lưu credential → tái sử dụng ở tất cả node HTTP Request

---

<a name="tham-khao"></a>

## 8. 📖 Tham Khảo API Nhanh

### Danh sách Endpoint

| Endpoint | Method | Auth | Mô tả |
|----------|--------|------|--------|
| `/api/health` | GET | ❌ | Kiểm tra server |
| `/api/image/generate` | POST | ✅ | Tạo ảnh AI |
| `/api/video/generate` | POST | ✅ | Tạo video AI |
| `/api/status/{task_id}` | GET | ✅ | Kiểm tra trạng thái |
| `/api/result/{task_id}` | GET | ✅ | Lấy kết quả |
| `/api/files/{filename}` | GET | ❌ | Tải file (không cần xác thực) |
| `/api/tasks` | GET | ✅ | Danh sách tác vụ |

### Vòng đời Task

```
pending → running → completed ✅
                  → failed ❌
```

### Model ảnh

| Giá trị `model` | Mô tả | Tỉ lệ hỗ trợ |
|-----------------|--------|---------------|
| `imagen4` | Mặc định, chất lượng cao | `1:1`, `3:4`, `4:3`, `9:16`, `16:9` |
| `nano_banana` | Nhanh, hỗ trợ Whisk | `1:1`, `9:16`, `16:9` |
| `nano_banana_2` | Nhanh, hỗ trợ Flow | `9:16`, `16:9` |
| `nano_banana_pro` | Chất lượng cao, hỗ trợ Flow | `9:16`, `16:9` |

### Model video

| Giá trị `model` | Credit | Mô tả |
|-----------------|--------|--------|
| `veo_31_fast_relaxed` | 0 | Mặc định, ưu tiên thấp (chỉ ULTRA) |
| `veo_31_fast` | 10 | Ưu tiên bình thường |
| `veo_31_quality` | 100 | Chất lượng cao nhất |

### Tỉ lệ khung hình

| Loại | Tỉ lệ hỗ trợ |
|------|--------------|
| **Ảnh (imagen4)** | `1:1`, `3:4`, `4:3`, `9:16`, `16:9` |
| **Ảnh (nano_banana)** | `1:1`, `9:16`, `16:9` |
| **Ảnh (nano_banana_2 / pro)** | `9:16`, `16:9` |
| **Video (tất cả model)** | `16:9`, `9:16` |

---

## 🔗 Tài Liệu Liên Quan

- 📖 [Webhook API Guide (đầy đủ)](WEBHOOK_API_GUIDE_VI.md)
- 📖 [Hướng Dẫn Sử Dụng G-Labs](USER_GUIDE_VI.md)
- 🌐 [n8n Documentation](https://docs.n8n.io/)
- 💬 [Discord Community](https://discord.gg/munMZEBMw5)

---

<p align="center">
  <b>G-Labs Automation × n8n</b> — Tự động hóa tạo ảnh & video AI<br><br>
  🌐 <a href="https://duckmartians.info/">duckmartians.info</a> · 💬 <a href="https://discord.gg/munMZEBMw5">Discord Community</a><br>
  <b>Tác giả: Đặng Minh Đức</b> · <a href="https://github.com/duckmartians">@duckmartians</a>
</p>
