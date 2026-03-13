# G-Labs Webhook API — Hướng Dẫn Tích Hợp

> **Địa chỉ:** `http://127.0.0.1:8765`  
> **Xác thực:** API Key qua header `X-API-Key`  
> **Yêu cầu:** Gói MAX

---

## 🔑 Xác Thực

Tất cả endpoint (trừ `/api/health`) yêu cầu API key trong header:

```
X-API-Key: YOUR_API_KEY
```

API key được tạo tự động trong **Cài đặt → Webhook**. Bạn có thể tạo lại bất kỳ lúc nào.

**Phản hồi 401 (Không có quyền):**
```json
{
  "error": "Unauthorized — invalid or missing API key"
}
```

---

## 📡 Danh Sách Endpoint

### 1. Kiểm Tra Trạng Thái Server

```
GET /api/health
```

Không cần xác thực. Dùng để kiểm tra server có đang chạy không.

**Phản hồi `200`:**
```json
{
  "status": "ok",
  "server": "G-Labs Webhook",
  "uptime": 3600,
  "tasks_pending": 0,
  "tasks_running": 1
}
```

---

### 2. Tạo Ảnh

```
POST /api/image/generate
Content-Type: application/json
X-API-Key: YOUR_API_KEY
```

**Tham số:**

| Trường | Kiểu | Bắt buộc | Mặc định | Mô tả |
|---|---|---|---|---|
| `prompt` | string | ✅ | — | Mô tả ảnh cần tạo |
| `model` | string | — | `imagen4` | `imagen4`, `nano_banana`, `nano_banana_2`, `nano_banana_pro` |
| `count` | integer | — | `1` | Số lượng ảnh (1–8) |
| `aspect_ratio` | string | — | `1:1` | `1:1`, `16:9`, `9:16`, `4:3`, `3:4` |
| `reference_images` | array | — | `[]` | Danh sách ảnh base64 hoặc object (xem dưới) |
| `upscale` | array | — | `[]` | Danh sách định dạng: `["2K"]`, `["4K"]`, hoặc `["2K", "4K"]` |

**Ví dụ:**
```bash
curl -X POST http://127.0.0.1:8765/api/image/generate \
  -H "X-API-Key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "một chú mèo đeo kính râm trên bãi biển",
    "model": "imagen4",
    "count": 2,
    "aspect_ratio": "1:1"
  }'
```

**Định dạng Ảnh tham chiếu:**
```json
"reference_images": [
  "data:image/png;base64,...", 
  {"data": "base64...", "category": "subject"}
]
```
Các hạng mục (chỉ Whisk): `subject`, `scene`, `style`.

**Định dạng Upscale:**
```json
"upscale": ["2K", "4K"]
```

**Phản hồi `202` (Đã nhận):**
```json
{
  "task_id": "a1b2c3d4",
  "status": "pending",
  "message": "Task queued for processing",
  "poll_url": "/api/status/a1b2c3d4"
}
```

---

### 3. Tạo Video

```
POST /api/video/generate
Content-Type: application/json
X-API-Key: YOUR_API_KEY
```

**Tham số:**

| Trường | Kiểu | Bắt buộc | Mặc định | Mô tả |
|---|---|---|---|---|
| `prompt` | string | ✅ | — | Mô tả video cần tạo |
| `model` | string | — | `veo_31_fast_relaxed` | `veo_31_fast_relaxed`, `veo_31_fast`, `veo_31_quality` |
| `mode` | string | — | `text_to_video` | `text_to_video`, `start_image`, `start_end_image`, `components` |
| `aspect_ratio` | string | — | `16:9` | `16:9`, `9:16` |
| `resolution` | array | — | `["720p"]` | Danh sách: `["720p"]`, `["1080p"]`, `["4K"]` |
| `reference_images` | array | — | `[]` | Ảnh base64. Bắt buộc cho các mode không phải text. |

**Các model video khả dụng:**

| Giá trị `model` | Tên hiển thị | Credit | Ghi chú |
|---|---|---|---|
| `veo_31_fast_relaxed` | Veo 3.1 Fast Relaxed | 0 | Mặc định. Ưu tiên thấp (chỉ ULTRA) |
| `veo_31_fast` | Veo 3.1 Fast | 10 | Ưu tiên bình thường |
| `veo_31_quality` | Veo 3.1 Quality | 100 | Chất lượng cao nhất |

**Ví dụ:**
```bash
curl -X POST http://127.0.0.1:8765/api/video/generate \
  -H "X-API-Key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "chú chó golden chạy trên bãi biển, quay chậm, cinematic",
    "model": "veo_31_fast",
    "aspect_ratio": "16:9"
  }'
```

**Phản hồi `202`:** Giống format tạo ảnh.

---

### 4. Kiểm Tra Trạng Thái Tác Vụ

```
GET /api/status/{task_id}
X-API-Key: YOUR_API_KEY
```

**Phản hồi `200` (đang chờ/đang chạy):**
```json
{
  "task_id": "a1b2c3d4",
  "type": "image",
  "status": "running",
  "prompt": "một chú mèo đeo kính râm...",
  "created_at": 1709500000
}
```

**Phản hồi `200` (hoàn thành):**
```json
{
  "task_id": "a1b2c3d4",
  "type": "image",
  "status": "completed",
  "prompt": "một chú mèo đeo kính râm...",
  "created_at": 1709500000,
  "completed_at": 1709500120,
  "results": [
    "http://127.0.0.1:8765/api/files/image_001.jpg",
    "http://127.0.0.1:8765/api/files/image_002.jpg"
  ]
}
```

**Phản hồi `200` (thất bại):**
```json
{
  "task_id": "a1b2c3d4",
  "type": "image",
  "status": "failed",
  "error_code": 403,
  "error": "FORBIDDEN",
  "error_detail": "Tài khoản bị chặn"
}
```

**Các trạng thái:** `pending` → `running` → `completed` / `failed`

---

### 5. Lấy Kết Quả

```
GET /api/result/{task_id}
X-API-Key: YOUR_API_KEY
```

Trả về dữ liệu kết quả (URL tải file). Tương tự status nhưng chỉ tập trung vào file output.

---

### 6. Danh Sách Tác Vụ

```
GET /api/tasks
X-API-Key: YOUR_API_KEY
```

**Phản hồi `200`:**
```json
{
  "tasks": [
    {
      "task_id": "a1b2c3d4",
      "type": "image",
      "status": "completed",
      "prompt": "một chú mèo đeo kính râm...",
      "created_at": 1709500000
    }
  ]
}
```

Trả về tối đa 50 tác vụ gần nhất, sắp xếp mới nhất trước.

---

### 7. Tải File

```
GET /api/files/{filename}
X-API-Key: YOUR_API_KEY
```

Trả về file nhị phân (ảnh/video) với header `Content-Type` phù hợp.

---

## 🔄 Quy Trình Polling (Theo Dõi Kết Quả)

Vì quá trình tạo ảnh/video mất thời gian (10 giây – 5 phút), sử dụng polling để kiểm tra:

```python
import requests
import time

API_KEY = "YOUR_API_KEY"
BASE = "http://127.0.0.1:8765"
HEADERS = {"X-API-Key": API_KEY, "Content-Type": "application/json"}

# 1. Gửi yêu cầu tạo ảnh
resp = requests.post(f"{BASE}/api/image/generate", headers=HEADERS, json={
    "prompt": "thành phố tương lai lúc hoàng hôn",
    "model": "imagen4",
    "count": 2,
    "aspect_ratio": "16:9"
})
task_id = resp.json()["task_id"]
print(f"Đã gửi tác vụ: {task_id}")

# 2. Theo dõi cho đến khi hoàn thành
while True:
    status = requests.get(f"{BASE}/api/status/{task_id}", headers=HEADERS).json()
    print(f"Trạng thái: {status['status']}")
    
    if status["status"] == "completed":
        for url in status["results"]:
            # 3. Tải file kết quả
            img = requests.get(url, headers=HEADERS)
            filename = url.split("/")[-1]
            with open(filename, "wb") as f:
                f.write(img.content)
            print(f"Đã lưu: {filename}")
        break
    elif status["status"] == "failed":
        print(f"Lỗi: {status['error']}")
        break
    
    time.sleep(5)  # Chờ 5 giây giữa mỗi lần kiểm tra
```

---

## 🔗 Ví Dụ Tích Hợp

### n8n Workflow

1. **HTTP Request node** → POST `http://127.0.0.1:8765/api/image/generate`
2. Đặt header: `X-API-Key: YOUR_KEY`
3. **Wait node** → 30 giây
4. **HTTP Request node** → GET `http://127.0.0.1:8765/api/status/{{task_id}}`
5. **IF node** → Kiểm tra `status == "completed"`
6. **HTTP Request node** → GET URL file kết quả

### JavaScript / Node.js

```javascript
const API_KEY = "YOUR_API_KEY";
const BASE = "http://127.0.0.1:8765";

// Gửi yêu cầu
const res = await fetch(`${BASE}/api/image/generate`, {
  method: "POST",
  headers: { "X-API-Key": API_KEY, "Content-Type": "application/json" },
  body: JSON.stringify({
    prompt: "một chú mèo trong vũ trụ",
    model: "imagen4",
    aspect_ratio: "1:1"
  })
});
const { task_id } = await res.json();

// Theo dõi kết quả
const poll = setInterval(async () => {
  const status = await fetch(`${BASE}/api/status/${task_id}`, {
    headers: { "X-API-Key": API_KEY }
  }).then(r => r.json());
  
  if (status.status === "completed") {
    clearInterval(poll);
    console.log("File kết quả:", status.results);
  }
}, 5000);
```

### cURL Kiểm Tra Nhanh

```bash
# Kiểm tra server
curl http://127.0.0.1:8765/api/health

# Tạo ảnh
curl -X POST http://127.0.0.1:8765/api/image/generate \
  -H "X-API-Key: YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{"prompt": "xin chào", "model": "imagen4"}'

# Kiểm tra trạng thái
curl -H "X-API-Key: YOUR_KEY" http://127.0.0.1:8765/api/status/TASK_ID

# Danh sách tác vụ
curl -H "X-API-Key: YOUR_KEY" http://127.0.0.1:8765/api/tasks
```

---

## ⚠️ Bảng Mã Lỗi

| Mã HTTP | Ý nghĩa |
|---|---|
| `200` | Thành công |
| `202` | Đã nhận tác vụ (đang xếp hàng) |
| `400` | Yêu cầu không hợp lệ (thiếu prompt, JSON sai) |
| `401` | API key không hợp lệ hoặc thiếu |
| `404` | Không tìm thấy tác vụ/file/endpoint |
| `500` | Lỗi server nội bộ |
