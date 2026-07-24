# G-Labs Automation — Hướng dẫn tích hợp Webhook API

> Đối tượng đọc: lập trình viên, người tích hợp kỹ thuật, và AI agent.
> Mục tiêu: cung cấp đầy đủ mọi thứ cần để tích hợp đúng Webhook REST API nội bộ —
> danh sách endpoint, xác thực, schema request/response, model, tham số, xử lý lỗi,
> và quy trình bất đồng bộ (gửi yêu cầu → hỏi trạng thái → tải kết quả).

Tài liệu này mô tả API do tab **Webhook** của ứng dụng desktop G-Labs Automation
(v5.0.8+) cung cấp. Nó cho phép các công cụ bên ngoài (n8n, Make.com, script tự
viết, AI agent) điều khiển việc tạo ảnh / video / Grok / Meta AI qua REST API đơn giản.

---

## 1. Tổng quan & khái niệm chính

- **Yêu cầu gói MAX.** Máy chủ Webhook chỉ bật được với license **MAX**.
- **Địa chỉ bind (mặc định loopback).** API chạy ở `http://<host>:<port>`. Mặc định
  `host` là `127.0.0.1` (chỉ máy này). Bạn đổi được trong tab Webhook: đặt `0.0.0.0`
  (mọi interface) hoặc một **IP LAN** cụ thể để máy khác trong mạng truy cập được.
  **Đừng dùng `127.0.0.0`** — đó là địa chỉ *network* của loopback, không client nào
  connect tới được. Muốn truy cập qua internet thì vẫn phải tự dựng tunnel/reverse
  proxy. Cho truy cập LAN, nhớ mở cổng trên tường lửa hệ điều hành.
- **Port mặc định:** `8765` (đổi được trong tab Webhook).
- **Bạn phải bật máy chủ.** Mở app → tab **Webhook** → **Start**. Tab này cũng hiển
  thị **API key**, cho đổi host / port / tạo lại key, và có tùy chọn **Tự động bật**
  máy chủ khi mở app.
- **Bất đồng bộ theo thiết kế.** Việc tạo nội dung tốn thời gian, nên API theo mô
  hình **gửi → hỏi trạng thái → tải về**:
  1. `POST` yêu cầu tạo → nhận `task_id` ngay lập tức (HTTP `202`).
  2. `GET /api/status/{task_id}` lặp lại cho đến khi `status` là `completed` hoặc `failed`.
  3. Khi `completed`, tải file từ các URL trong `results`.
- **Đồng thời.** Máy chủ nhận nhiều request cùng lúc nhưng xử lý tối đa **10 task
  song song** (các task dư sẽ chờ tới lượt).
- **Tạo nội dung dùng tài khoản đã đăng nhập trong app.** Ảnh/Video dùng tài khoản
  Google (Flow/Veo) cấu hình trong app; Grok dùng phiên Super Grok đã kết nối;
  **Meta AI** dùng tài khoản Meta (vibes.ai) đã đăng nhập. Nếu không có tài khoản
  hợp lệ, task sẽ thất bại (xem bảng lỗi). App phải đang chạy với các tài khoản đó
  đã đăng nhập & đang bật.

---

## 2. Xác thực

Tất cả endpoint **trừ** `GET /api/health` và `GET /api/files/{filename}` đều yêu cầu
API key, gửi qua HTTP header:

```
X-API-Key: <api-key-của-bạn>
```

- Key do app sinh ra (token URL-safe 32 byte) và hiển thị trong tab **Webhook**.
  Bạn có thể tạo lại key ở đó.
- Thiếu/sai key → `401 {"error": "Invalid or missing API key"}`.

CORS đã bật (`Access-Control-Allow-Origin: *`) và có xử lý preflight `OPTIONS`, nên
client trên trình duyệt cũng dùng được.

---

## 3. Danh sách Endpoint

| Method | Path | Auth | Mục đích |
|--------|------|:----:|----------|
| `GET`  | `/api/health` | ❌ | Tình trạng máy chủ / uptime / số task trong hàng đợi |
| `POST` | `/api/image/generate` | ✅ | Đưa task tạo **ảnh** vào hàng đợi |
| `POST` | `/api/video/generate` | ✅ | Đưa task tạo **video** vào hàng đợi |
| `POST` | `/api/grok/generate`  | ✅ | Đưa task **Grok** (ảnh/video) vào hàng đợi |
| `POST` | `/api/meta/generate`  | ✅ | Đưa task **Meta AI** (ảnh/video) vào hàng đợi |
| `GET`  | `/api/status/{task_id}` | ✅ | Hỏi trạng thái task; trả kết quả hoặc lỗi |
| `GET`  | `/api/result/{task_id}` | ✅ | Lấy kết quả (chỉ khi đã `completed`) |
| `GET`  | `/api/files/{filename}` | ❌ | Tải file kết quả đã tạo |
| `GET`  | `/api/tasks` | ✅ | Liệt kê 50 task gần nhất |

Dấu `/` ở cuối được chấp nhận (vd `/api/health/`).

---

## 4. Quy trình bất đồng bộ (từng bước)

### Bước 1 — Gửi yêu cầu

`POST` tới một trong ba endpoint generate kèm JSON body (xem schema ở §5). Phản hồi
là **HTTP 202**:

```json
{
  "task_id": "abc12345",
  "status": "pending",
  "message": "Image task queued for processing",
  "poll_url": "/api/status/abc12345"
}
```

> `task_id` là chuỗi hex 8 ký tự. Giữ lại để hỏi trạng thái và lấy kết quả.

### Bước 2 — Hỏi trạng thái

`GET /api/status/{task_id}` (kèm header `X-API-Key`). Các giá trị `status` có thể là:
`pending` → `running` → `completed` | `failed`.

**Đang chạy:**
```json
{ "task_id": "abc12345", "type": "image", "status": "running", "prompt": "a cat ...", "created_at": 1707782400.0 }
```

**Hoàn thành:**
```json
{
  "task_id": "abc12345",
  "type": "image",
  "status": "completed",
  "prompt": "a cat ...",
  "created_at": 1707782400.0,
  "results": ["http://127.0.0.1:8765/api/files/image_001.png"],
  "completed_at": 1707782460.0
}
```

**Thất bại:**
```json
{
  "task_id": "abc12345",
  "type": "image",
  "status": "failed",
  "prompt": "a cat ...",
  "created_at": 1707782400.0,
  "error_code": 429,
  "error": "Đã hết giới hạn tạo ảnh trong ngày (user@gmail.com)",
  "error_detail": "429: Đã hết giới hạn tạo ảnh trong ngày (user@gmail.com)"
}
```

> Khoảng thời gian hỏi đề xuất: mỗi 3–5 giây. Ảnh thường xong trong vài giây tới vài
> phút; video có thể mất vài phút (dịch vụ phía trên có thể xếp hàng). Máy chủ có cơ
> chế watchdog để báo thất bại cho task bị treo quá lâu không có kết quả.

### Bước 3 — Tải kết quả

`results` là mảng URL dạng `http://<host>:<port>/api/files/<tên-đã-url-encode>`.
`<host>` khớp với địa chỉ đã bind của máy chủ, nên client vào bằng IP LAN sẽ nhận link
tải trên đúng IP đó (khi bind `0.0.0.0`, máy chủ tự quảng bá IP LAN chính). `GET` từng
URL (không cần API key) để tải dữ liệu thô. `Content-Type` được đặt theo phần mở rộng
file (`image/png`, `image/jpeg`, `video/mp4`, …). File lưu nội bộ trên máy đang chạy app.

**Trả về bao nhiêu file (và ở độ phân giải nào):**

- **Ảnh, không `upscale`** → 1 file (ảnh gốc).
- **Ảnh có `upscale`** → một file **cho mỗi độ phân giải upscale yêu cầu**, theo thứ
  tự tăng dần — vd `["2K"]` → `[2K]`, `["2K","4K"]` → `[2K, 4K]`. Ảnh gốc
  (không upscale) **không** được kèm khi đã yêu cầu `upscale`.
- **Video** → một file **cho mỗi độ phân giải tạo được** — vd `["720p","1080p"]` →
  tối đa 2 file.
- **Grok** → luôn đúng 1 file.
- **Meta AI** → `count` file (1–4): `count` ảnh (một lô) hoặc `count` clip video.

(Nên `len(results)` và thứ tự khớp với các độ phân giải bạn yêu cầu.)

---

## 5. Schema body của request

Mọi body đều là JSON. `prompt` là **bắt buộc** cho mọi endpoint; prompt rỗng/thiếu sẽ
làm task thất bại với lỗi `Missing required field: prompt`.

### 5.1 Ảnh — `POST /api/image/generate`

| Trường | Kiểu | Bắt buộc | Mặc định | Ghi chú |
|--------|------|:--------:|----------|---------|
| `prompt` | string | ✅ | — | Mô tả ảnh. |
| `model` | string | ❌ | `nano_banana_2` | Một trong `nano_banana_pro`, `nano_banana_2`, `nano_banana_2_lite`. Không hợp lệ → `nano_banana_2`. |
| `aspect_ratio` | string | ❌ | `1:1` | Một trong `1:1`, `3:4`, `4:3`, `9:16`, `16:9`. Không hợp lệ → `1:1`. |
| `reference_images` | array | ❌ | `[]` | Tối đa **10** ảnh base64 (xem §6). Mỗi ảnh có thể kèm `name` để gắn theo `@tên` trong prompt (§6.1). |
| `upscale` | array | ❌ | `[]` | Bất kỳ `"2K"`, `"4K"`. **4K cần tài khoản ULTRA** và model hỗ trợ upscale. Giá trị sai bị bỏ. |

```json
{
  "prompt": "a modern minimalist house, golden hour",
  "model": "nano_banana_pro",
  "aspect_ratio": "16:9",
  "reference_images": ["data:image/png;base64,iVBORw0KGgo..."],
  "upscale": ["4K"]
}
```

> Nếu yêu cầu `upscale` mà không tạo được độ phân giải đó (vd 4K hết quota), task sẽ
> báo **failed** kèm lý do — **không** âm thầm trả về ảnh gốc độ phân giải thấp hơn.

### 5.2 Video — `POST /api/video/generate`

| Trường | Kiểu | Bắt buộc | Mặc định | Ghi chú |
|--------|------|:--------:|----------|---------|
| `prompt` | string | ✅ | — | Mô tả chuyển động / khung cảnh. |
| `model` | string | ❌ | `veo_31_fast` | Một trong `veo_31_fast`, `veo_31_lite`, `veo_31_quality`, `veo_31_lite_relaxed`, `omni_flash`. Không hợp lệ → `veo_31_fast`. `veo_31_lite_relaxed` cần tài khoản **ULTRA**. **`omni_flash`**: xem ghi chú `mode`. |
| `aspect_ratio` | string | ❌ | `16:9` | `16:9` hoặc `9:16`. |
| `mode` | string | ❌ | `text_to_video` | `text_to_video` (0 ảnh) · `start_image` (1 ảnh) · `start_end_image` (2 ảnh) · `components` (Veo tối đa 3 ảnh, Omni Flash tối đa 7; hỗ trợ `voice`). **Omni Flash KHÔNG hỗ trợ `start_end_image`** (chưa có khung cuối) — gửi sẽ bị từ chối; các mode còn lại đều dùng được. |
| `reference_images` | array | ❌ | `[]` | Tối đa **3** ảnh base64 (Veo); **Omni Flash `components` tối đa 7**. **Bắt buộc khi `mode != text_to_video`.** Mỗi ảnh có thể kèm `name` để gắn theo `@tên` trong prompt — Veo (§6.1). |
| `resolution` | array | ❌ | `["720p"]` | Bất kỳ `"720p"`, `"1080p"`, `"4K"`. `1080p`/`4K` tạo bằng upscale; **chỉ `4K` cần tài khoản ULTRA** (`1080p` không cần ULTRA). Giá trị sai → `720p`. Mỗi độ phân giải tạo ra một file. |
| `voice` | string | ❌ | `""` | Tên giọng (chữ thường). Chỉ dùng ở mode `components`. |
| `video_length` | int | ❌ | (mặc định model) | Độ dài clip (giây). Veo: `4`/`6`/`8`; Omni Flash: `4`/`6`/`8`/`10`. Giá trị không hỗ trợ → dùng mặc định (8s). **Veo `4`/`6` cần tài khoản ULTRA** (Omni Flash không cần). |

```json
{
  "prompt": "water flowing over rocks, cinematic",
  "model": "veo_31_fast",
  "aspect_ratio": "16:9",
  "mode": "start_image",
  "reference_images": ["data:image/png;base64,iVBORw0KGgo..."],
  "resolution": ["720p", "1080p"],
  "video_length": 8
}
```

> **Thứ tự ảnh tham chiếu:** với `start_end_image`, `reference_images[0]` là khung
> đầu, `reference_images[1]` là khung cuối. Với `start_image`, chỉ dùng
> `reference_images[0]`.
> **`mode` không được kiểm tra chặt:** mode không hợp lệ sẽ hoạt động như
> `text_to_video` (bỏ qua ảnh tham chiếu). Chỉ `components` đọc trường `voice`
> (Veo tối đa 3 ảnh tham chiếu, Omni Flash tối đa 7).
> **Dùng được cả Veo lẫn Omni Flash.** Omni Flash (`model: "omni_flash"`) hỗ trợ
> `text_to_video`, `start_image`, và `components` — **không** hỗ trợ
> `start_end_image` (chưa có khung cuối). Nó không cần tài khoản ULTRA và có thêm
> mốc `video_length` **10s**.
> `resolution` có thể liệt kê nhiều giá trị; mỗi giá trị được tạo nếu hạng tài khoản
> cho phép.

### 5.3 Grok — `POST /api/grok/generate`

| Trường | Kiểu | Bắt buộc | Mặc định | Ghi chú |
|--------|------|:--------:|----------|---------|
| `prompt` | string | ✅ | — | Prompt. |
| `mode` | string | ❌ | `t2v` | `t2i` (văn bản→ảnh) · `i2i` (ảnh→ảnh) · `t2v` (văn bản→video) · `i2v` (ảnh→video). Mode sai → task thất bại. |
| `aspect_ratio` | string | ❌ | `9:16` | Một trong `9:16`, `16:9`, `1:1`, `2:3`, `3:2`. Không hợp lệ → `9:16`. |
| `reference_images` | array | ❌ | `[]` | Tối đa **5** ảnh base64. **Bắt buộc cho `i2i` và `i2v`** (≥1, không có thì task thất bại). Bị bỏ qua với `t2i` / `t2v`. |
| `video_length` | int | ❌ | `6` | `6`, `10` hoặc `15` (giây). **Chỉ mode video** (`t2v`/`i2v`). Giá trị khác → `6`. |
| `resolution` | string | ❌ | `480p` | `480p` hoặc `720p`. **Chỉ mode video.** Mode ảnh (`t2i`/`i2i`) luôn xuất **1K**. |

```json
{
  "prompt": "make it rain over the city",
  "mode": "i2v",
  "aspect_ratio": "16:9",
  "video_length": 6,
  "resolution": "720p",
  "reference_images": ["data:image/png;base64,iVBORw0KGgo..."]
}
```

> Grok luôn tạo **một** ảnh/video mỗi request và trả về kết quả đầu tiên. Không có
> tham số `image_generation_count`.

### 5.4 Meta AI — `POST /api/meta/generate`

Tạo nội dung trên **Meta AI (vibes.ai)** bằng tài khoản Meta đã đăng nhập. Khác với
các endpoint kia, ảnh tham chiếu được truyền qua **trường có tên** (không phải mảng
`reference_images`) — xem §6.2.

| Trường | Kiểu | Bắt buộc | Mặc định | Ghi chú |
|--------|------|:--------:|----------|---------|
| `prompt` | string | ✅ | — | Prompt. |
| `mode` | string | ❌ | `t2i` | `t2i` (văn bản→ảnh) · `t2v` (văn bản→video) · `i2i` (ảnh→ảnh, thành phần) · `i2v` (ảnh→video). Mode sai → task thất bại. |
| `aspect_ratio` | string | ❌ | `9:16` | Một trong `9:16`, `16:9`, `1:1`. Không hợp lệ → `9:16`. |
| `resolution` | string | ❌ | `720p` | `480p` hoặc `720p`. **Chỉ mode video** (`t2v`/`i2v`). Ảnh tự suy theo site: `1:1` → 1280p, còn lại → 720p. |
| `count` | int | ❌ | `1` | Số đầu ra mỗi prompt, **1–4** (kẹp về khoảng này). Ảnh: `count` ảnh trong một lô; video: `count` clip. |
| `character_image` | base64 | i2i¹ | — | Thành phần nhân vật/chủ thể (§6.2). Cũng chấp nhận `subject_image`. |
| `scene_image` | base64 | i2i¹ | — | Thành phần bối cảnh. |
| `style_image` | base64 | i2i¹ | — | Thành phần phong cách. |
| `start_image` | base64 | i2v | — | **Bắt buộc cho `i2v`** — khung đầu. |
| `end_image` | base64 | ❌ | — | Khung cuối tùy chọn cho `i2v` (nội suy đầu→cuối). |

¹ **i2i** cần **ít nhất một** trong `character_image` / `scene_image` / `style_image`.
**i2v** cần `start_image`. Mode văn bản (`t2i` / `t2v`) bỏ qua mọi ảnh gửi kèm.

```json
{
  "prompt": "a cat astronaut floating in space",
  "mode": "t2i",
  "aspect_ratio": "1:1",
  "count": 1
}
```
```json
{
  "prompt": "same character in a forest at dawn",
  "mode": "i2i",
  "aspect_ratio": "16:9",
  "character_image": "data:image/png;base64,iVBORw0KGgo...",
  "scene_image": "data:image/jpeg;base64,/9j/4AAQ..."
}
```
```json
{
  "prompt": "slow pan across the scene",
  "mode": "i2v",
  "aspect_ratio": "16:9",
  "resolution": "720p",
  "start_image": "data:image/png;base64,iVBORw0KGgo...",
  "end_image": "data:image/png;base64,iVBORw0KGgo..."
}
```

> Meta AI **không có `@tag`** gắn theo tên (cái đó chỉ có ở Flow/Veo); thành phần
> được gắn bằng các trường có tên ở trên.

### 5.5 Bảng tham chiếu Model

| Giá trị API (`model` / `mode`) | Tên hiển thị | Đầu ra / Tỉ lệ |
|------|--------------|--------|
| `nano_banana_pro` | Nano Banana Pro | `1:1, 3:4, 4:3, 9:16, 16:9` |
| `nano_banana_2` | Nano Banana 2 | `1:1, 3:4, 4:3, 9:16, 16:9` |
| `nano_banana_2_lite` | Nano Banana 2 Lite | `1:1, 3:4, 4:3, 9:16, 16:9` |
| `veo_31_fast` | Veo 3.1 Fast | `16:9, 9:16` |
| `veo_31_lite` | Veo 3.1 Lite | `16:9, 9:16` |
| `veo_31_quality` | Veo 3.1 Quality | `16:9, 9:16` |
| `veo_31_lite_relaxed` | Veo 3.1 Lite Relaxed (chỉ ULTRA) | `16:9, 9:16` |
| `omni_flash` | Omni Flash (video; 4/6/8/10s; tối đa 7 ảnh ref; không cần ULTRA) | `16:9, 9:16` |
| Grok `mode=t2i` | Text → Image (1K) | `9:16, 16:9, 1:1, 2:3, 3:2` |
| Grok `mode=i2i` | Image → Image (1K) | `9:16, 16:9, 1:1, 2:3, 3:2` |
| Grok `mode=t2v` | Text → Video (480p/720p) | `9:16, 16:9, 1:1, 2:3, 3:2` |
| Grok `mode=i2v` | Image → Video (480p/720p) | `9:16, 16:9, 1:1, 2:3, 3:2` |
| Meta `mode=t2i` | Meta AI Văn bản → Ảnh | `9:16, 16:9, 1:1` |
| Meta `mode=t2v` | Meta AI Văn bản → Video (480p/720p) | `9:16, 16:9, 1:1` |
| Meta `mode=i2i` | Meta AI Ảnh → Ảnh (thành phần) | `9:16, 16:9, 1:1` |
| Meta `mode=i2v` | Meta AI Ảnh → Video (đầu/cuối) | `9:16, 16:9, 1:1` |

---

## 6. Định dạng ảnh tham chiếu

Mỗi phần tử trong `reference_images` là một trong hai dạng:

1. **Chuỗi base64** — data URI hoặc base64 thô:
   - `"data:image/png;base64,iVBORw0KGgo..."`
   - `"data:image/jpeg;base64,/9j/4AAQ..."`
   - base64 thô `"iVBORw0KGgo..."` (được coi là PNG)
2. **Object**: `{"data": "data:image/...;base64,...", "category": "subject", "name": "red_car.png"}`
   - `category` không bắt buộc: `subject` | `scene` | `style`. Được chấp nhận nhưng
     **bị bỏ qua với model ảnh Flow** (chúng dùng một ô tham chiếu chung).
   - `name` (hoặc `filename`) không bắt buộc: tên file gốc của ảnh. Khi có, bạn có thể
     gắn ảnh này vào đúng vị trí trong prompt bằng `@<từ_khoá>` (xem §6.1).

Ràng buộc:
- Kiểu giải mã hỗ trợ: PNG, JPG/JPEG, WEBP (nhận diện qua header data-URI; mặc định
  PNG khi không có header).
- Dữ liệu giải mã dưới ~100 byte sẽ bị bỏ (coi là không hợp lệ).
- Số lượng tối đa theo endpoint: **ảnh = 10**, **grok = 5**, **video = 3**. Phần dư
  vượt quá mức tối đa sẽ bị bỏ qua.

### 6.1 Gắn ảnh theo tên bằng `@tag`

Khi một ảnh tham chiếu có `name` (hoặc `filename`), bạn có thể trỏ tới nó **ngay trong
prompt** bằng `@<từ_khoá>` — **đúng cùng cơ chế** ở trang Image / Veo trong app (tín hiệu
webhook đi qua chính các hàm xử lý đó nên payload gửi Flow API khớp y như tạo trực tiếp).

- **Khớp**: `<từ_khoá>` là **chuỗi con** không phân biệt hoa thường của tên file (đã bỏ
  đuôi). Vd `name: "red_car.png"` → `@red_car`, `@car`, `@red` đều trúng; ảnh đầu tiên theo
  thứ tự trong `reference_images` thắng.
- **Tác dụng**: gắn đúng ảnh đó vào đúng vị trí danh từ trong câu (Flow structured prompt
  "Mode-2"), thay vì truyền tất cả ảnh như tham chiếu chung vô danh.
- **Phạm vi**: dùng cho **image (Flow)** và **video (Veo)**; **Grok không hỗ trợ** `@tag`.
- Tên file được làm sạch (bỏ thành phần thư mục và ký tự không hợp lệ) trước khi dùng.
- **Tag không khớp** được giữ nguyên là văn bản — không gây lỗi.
- **Không gửi `name`**: ảnh vẫn dùng như tham chiếu theo vị trí như trước (tương thích ngược).

**Image (Flow):**
```json
{
  "prompt": "a @red_car parked next to a @house at night",
  "reference_images": [
    {"data": "data:image/png;base64,...", "name": "red_car.png"},
    {"data": "data:image/jpeg;base64,...", "name": "house.jpg"}
  ]
}
```

**Video (Veo) — mode `components`** (ghép nhiều "nguyên liệu" theo tên; cũng áp dụng cho
`start_image` / `start_end_image`):
```json
{
  "prompt": "the @character walks through the @forest at dawn",
  "model": "veo_31_fast",
  "mode": "components",
  "aspect_ratio": "16:9",
  "reference_images": [
    {"data": "data:image/png;base64,...", "name": "character.png"},
    {"data": "data:image/jpeg;base64,...", "name": "forest.jpg"}
  ]
}
```

### 6.2 Trường ảnh tham chiếu có tên của Meta AI

**Meta AI (`/api/meta/generate`) KHÔNG dùng mảng `reference_images`.** Nó có các
trường riêng có tên, mỗi trường là một ảnh base64 theo đúng định dạng ở §6 (data URI
hoặc base64 thô; PNG/JPG/WEBP; dưới ~100 byte bị bỏ):

| Trường | Mode | Vai trò |
|--------|------|---------|
| `character_image` (hoặc `subject_image`) | `i2i` | Thành phần nhân vật / chủ thể |
| `scene_image` | `i2i` | Thành phần bối cảnh |
| `style_image` | `i2i` | Thành phần phong cách |
| `start_image` | `i2v` | Khung đầu (**bắt buộc** cho i2v) |
| `end_image` | `i2v` | Khung cuối (tùy chọn; bật nội suy đầu→cuối) |

- **i2i** dùng tổ hợp bất kỳ của `character_image` / `scene_image` / `style_image`
  (ít nhất một).
- **i2v** dùng `start_image` (và tùy chọn `end_image`).
- `t2i` / `t2v` **không** nhận ảnh — ảnh gửi kèm sẽ bị bỏ qua.
- Meta AI **không có `@tag`**; tên trường quyết định vai trò.

---

## 7. Tham chiếu Response & trạng thái

### POST generate → `202`
```json
{ "task_id": "abc12345", "status": "pending", "message": "...", "poll_url": "/api/status/abc12345" }
```

### GET `/api/status/{task_id}` → `200`
- `pending` / `running`: `{ task_id, type, status, prompt, created_at }`
- `completed`: thêm `results` (mảng URL file) và `completed_at` (unix giây).
- `failed`: thêm `error_code` (int), `error` (string), `error_detail` (string).
- task_id không tồn tại → `404 {"error": "Task <id> not found"}`.

### GET `/api/result/{task_id}` → `200`
- Nếu chưa xong: `{ task_id, status, message: "Task not yet completed" }`.
- Nếu đã xong: `{ task_id, status: "completed", results: [...], completed_at }`.

### GET `/api/health` → `200`
```json
{ "status": "ok", "server": "G-Labs Webhook", "uptime": 123, "tasks_pending": 0, "tasks_running": 1 }
```

### GET `/api/tasks` → `200`
```json
{ "tasks": [ { "task_id": "...", "type": "image", "status": "completed", "prompt": "50 ký tự đầu...", "created_at": 1707782400.0 } ] }
```
(Mới nhất trước, tối đa 50.)

### Các response lỗi
| HTTP | Body | Khi nào |
|------|------|---------|
| `400` | `{"error": "Empty body"}` | POST không có body |
| `401` | `{"error": "Invalid or missing API key"}` | Thiếu/sai `X-API-Key` |
| `404` | `{"error": "Not found"}` | Route không tồn tại |
| `404` | `{"error": "Task <id> not found"}` | task không tồn tại ở status/result |
| `404` | `{"error": "File not found: <name>"}` | File không tồn tại/đã hết hạn |

---

## 8. Mã lỗi (`error_code` / `error` / `error_detail`)

Khi task thất bại, response trạng thái mang ba trường:

- **`error_code`** — số nguyên. Với lỗi từ API phía trên, nó phản ánh mã kiểu HTTP
  (`400`, `403`, `429`, `500`, …). **`429` nghĩa là hết quota / giới hạn tần suất**
  (vd hết giới hạn tạo trong ngày). Bằng `0` khi không có mã số phù hợp (vd lỗi
  validate, "không có tài khoản").
- **`error`** — thông điệp dễ đọc. Với các status đã biết từ phía trên, đây là status
  tiếng Anh ổn định (vd `PERMISSION_DENIED`, `RESOURCE_EXHAUSTED`). Với các thông điệp
  thân thiện (quota, upscale thất bại, timeout), phần chữ **theo ngôn ngữ giao diện
  của app** (vd tiếng Việt nếu app đang để tiếng Việt).
- **`error_detail`** — chuỗi lỗi gốc đầy đủ (vd `"429: Đã hết giới hạn tạo ảnh trong ngày (user@gmail.com)"`).

Các trường hợp phổ biến:

| `error_code` | Ý nghĩa | `error` điển hình |
|:---:|---------|-------------------|
| `429` | Hết quota trong ngày / bị giới hạn tần suất | thông điệp quota (kèm email tài khoản) |
| `403` | Bị từ chối quyền / lỗi phiên | `PERMISSION_DENIED` |
| `400` | Request không hợp lệ / vi phạm chính sách prompt | `INVALID_ARGUMENT`, thông điệp vi phạm |
| `500` | Lỗi máy chủ phía trên | `INTERNAL` / `HTTP_500` |
| `0` | Lỗi validate / môi trường | `No active accounts available`, `Missing required field: prompt`, `Invalid mode '...'`, thông điệp upscale thất bại, `Timeout: no result generated` |

> Mẹo xử lý bằng code: rẽ nhánh theo `error_code` (không phụ thuộc ngôn ngữ). Dùng
> `error` / `error_detail` cho log hiển thị cho người. `error_code == 429` là tín hiệu
> để giãn nhịp / xoay tài khoản / thử lại sau.

Lưu ý riêng của app này:
- **Video** yêu cầu độ phân giải không tạo được (vd `4K` mà không có tài khoản ULTRA)
  → `failed`.
- **Ảnh** yêu cầu `upscale` (`2K`/`4K`) không tạo được → `failed` kèm lý do upscale
  (không trả về ảnh gốc nhỏ hơn).
- **Meta AI**: cookie tài khoản hết hạn → `failed` `401` (`Meta cookie expired`);
  hết quota → `429` (`Meta quota exhausted`); không có tài khoản Meta đang bật cho
  loại yêu cầu → `error_code 0` (`No enabled Meta account for image/video`); không
  tạo ra gì → `Meta produced no output`.

---

## 9. Ràng buộc, hạng tài khoản & lưu ý

- **Yêu cầu gói MAX** để bật máy chủ Webhook.
- **Địa chỉ bind.** Mặc định `127.0.0.1` (chỉ cùng máy). Đặt host thành `0.0.0.0`
  hoặc IP LAN trong tab Webhook để máy khác trong mạng vào được (nhớ mở cổng trên
  tường lửa); URL file kết quả khi đó dùng đúng host này. `127.0.0.0` không phải địa
  chỉ bind/connect hợp lệ. Muốn qua internet thì tự dựng tunnel.
- **Tài khoản & hạng:**
  - Ảnh/Video cần tài khoản Google đã đăng nhập và **đang bật** trong app.
  - Upscale ảnh `4K`, video **`4K`**, và `veo_31_lite_relaxed` cần tài khoản
    **ULTRA** (video `1080p` không cần ULTRA). Không có tài khoản hợp lệ thì task `failed`.
  - Tài khoản đã **TẮT** trong app sẽ không được dùng.
- **Grok** cần phiên Super Grok đang kết nối trong app.
- **Mỗi request Grok trả 1 kết quả** (ảnh/video đầu tiên được tạo).
- **Meta AI** cần tài khoản Meta (vibes.ai) đã đăng nhập và **đang bật** trong app
  (tab Meta AI) — `image_enabled` cho `t2i`/`i2i`, `video_enabled` cho `t2v`/`i2v`.
  Dùng tài khoản hợp lệ đầu tiên (không xoay vòng); tài khoản đầu hết hạn sẽ làm task
  thất bại.
- **Task lưu trong bộ nhớ.** Trạng thái task và ánh xạ `task_id` → kết quả nằm trong
  app đang chạy; sẽ mất nếu app khởi động lại. Hãy gửi, hỏi trạng thái và tải về
  trong cùng một phiên chạy app.
- **File lưu nội bộ** và có thể bị dọn theo thời gian; hãy tải ngay sau khi `completed`.
- **Tính tái lập:** cùng một `prompt` không đảm bảo cho ra kết quả giống hệt.

---

## 10. Ví dụ đầu-cuối

### 10.1 cURL

```bash
# --- Ảnh (cơ bản) ---
curl -X POST http://127.0.0.1:8765/api/image/generate \
  -H "Content-Type: application/json" \
  -H "X-API-Key: YOUR_API_KEY" \
  -d '{"prompt": "a cat wearing sunglasses", "model": "nano_banana_2"}'
# → {"task_id":"abc12345","status":"pending","poll_url":"/api/status/abc12345"}

# --- Hỏi trạng thái tới khi xong ---
curl http://127.0.0.1:8765/api/status/abc12345 -H "X-API-Key: YOUR_API_KEY"

# --- Ảnh + tham chiếu + upscale 4K (cần ULTRA) ---
curl -X POST http://127.0.0.1:8765/api/image/generate \
  -H "Content-Type: application/json" -H "X-API-Key: YOUR_API_KEY" \
  -d '{"prompt":"modern house","model":"nano_banana_pro","reference_images":["data:image/png;base64,..."],"upscale":["4K"]}'

# --- Video (ảnh đầu) ---
curl -X POST http://127.0.0.1:8765/api/video/generate \
  -H "Content-Type: application/json" -H "X-API-Key: YOUR_API_KEY" \
  -d '{"prompt":"flowing water","mode":"start_image","reference_images":["data:image/png;base64,..."],"resolution":["1080p"]}'

# --- Video (mode components + voice) ---
curl -X POST http://127.0.0.1:8765/api/video/generate \
  -H "Content-Type: application/json" -H "X-API-Key: YOUR_API_KEY" \
  -d '{"prompt":"she says hello","mode":"components","reference_images":["data:image/png;base64,..."],"voice":"aoede"}'

# --- Grok: văn bản → ảnh ---
curl -X POST http://127.0.0.1:8765/api/grok/generate \
  -H "Content-Type: application/json" -H "X-API-Key: YOUR_API_KEY" \
  -d '{"prompt":"a girl swimming in a pool","mode":"t2i","aspect_ratio":"16:9"}'

# --- Grok: ảnh → video ---
curl -X POST http://127.0.0.1:8765/api/grok/generate \
  -H "Content-Type: application/json" -H "X-API-Key: YOUR_API_KEY" \
  -d '{"prompt":"make it rain","mode":"i2v","aspect_ratio":"16:9","resolution":"720p","reference_images":["data:image/png;base64,..."]}'

# --- Meta AI: văn bản → ảnh ---
curl -X POST http://127.0.0.1:8765/api/meta/generate \
  -H "Content-Type: application/json" -H "X-API-Key: YOUR_API_KEY" \
  -d '{"prompt":"a cat astronaut","mode":"t2i","aspect_ratio":"1:1","count":1}'

# --- Meta AI: văn bản → video ---
curl -X POST http://127.0.0.1:8765/api/meta/generate \
  -H "Content-Type: application/json" -H "X-API-Key: YOUR_API_KEY" \
  -d '{"prompt":"waves on a beach at dawn","mode":"t2v","aspect_ratio":"9:16","resolution":"720p"}'

# --- Meta AI: ảnh → ảnh (thành phần) ---
curl -X POST http://127.0.0.1:8765/api/meta/generate \
  -H "Content-Type: application/json" -H "X-API-Key: YOUR_API_KEY" \
  -d '{"prompt":"same character in a forest","mode":"i2i","aspect_ratio":"16:9","character_image":"data:image/png;base64,...","scene_image":"data:image/png;base64,..."}'

# --- Meta AI: ảnh → video (đầu + cuối tùy chọn) ---
curl -X POST http://127.0.0.1:8765/api/meta/generate \
  -H "Content-Type: application/json" -H "X-API-Key: YOUR_API_KEY" \
  -d '{"prompt":"pan across the scene","mode":"i2v","aspect_ratio":"16:9","resolution":"720p","start_image":"data:image/png;base64,...","end_image":"data:image/png;base64,..."}'

# --- Tải file kết quả ---
curl -o out.png "http://127.0.0.1:8765/api/files/image_001.png"

# --- Liệt kê task gần đây ---
curl http://127.0.0.1:8765/api/tasks -H "X-API-Key: YOUR_API_KEY"
```

### 10.2 Python (gửi → hỏi → tải)

```python
import base64, time, requests

BASE = "http://127.0.0.1:8765"
KEY  = "YOUR_API_KEY"
H    = {"X-API-Key": KEY, "Content-Type": "application/json"}

def b64(path):
    ext = "png" if path.lower().endswith(".png") else "jpeg"
    with open(path, "rb") as f:
        return f"data:image/{ext};base64," + base64.b64encode(f.read()).decode()

def generate(endpoint, body, poll=4, timeout=900):
    r = requests.post(f"{BASE}/api/{endpoint}/generate", json=body, headers=H, timeout=30)
    r.raise_for_status()
    task_id = r.json()["task_id"]

    deadline = time.time() + timeout
    while time.time() < deadline:
        s = requests.get(f"{BASE}/api/status/{task_id}", headers=H, timeout=30).json()
        status = s.get("status")
        if status == "completed":
            return s["results"]                      # danh sách URL file
        if status == "failed":
            raise RuntimeError(f"[{s.get('error_code')}] {s.get('error')}")
        time.sleep(poll)
    raise TimeoutError(f"task {task_id} chưa xong trong {timeout}s")

# Ảnh
urls = generate("image", {
    "prompt": "a modern minimalist house, golden hour",
    "model": "nano_banana_pro",
    "aspect_ratio": "16:9",
    "upscale": ["2K"],
})

# Tải về
for i, u in enumerate(urls):
    data = requests.get(u, timeout=120).content   # /api/files không cần key
    open(f"out_{i}{u[u.rfind('.'):]}" if '.' in u else f"out_{i}.bin", "wb").write(data)

# Ảnh→Ảnh qua Grok
grok_urls = generate("grok", {
    "prompt": "same character, sunset lighting",
    "mode": "i2i",
    "aspect_ratio": "1:1",
    "reference_images": [b64("char.png")],
})
```

### 10.3 JavaScript (Node, fetch)

```js
const BASE = "http://127.0.0.1:8765";
const KEY  = "YOUR_API_KEY";
const H = { "X-API-Key": KEY, "Content-Type": "application/json" };

async function generate(endpoint, body, { poll = 4000, timeout = 900000 } = {}) {
  const r = await fetch(`${BASE}/api/${endpoint}/generate`, {
    method: "POST", headers: H, body: JSON.stringify(body),
  });
  if (!r.ok) throw new Error(`submit ${r.status}`);
  const { task_id } = await r.json();

  const end = Date.now() + timeout;
  while (Date.now() < end) {
    const s = await (await fetch(`${BASE}/api/status/${task_id}`, { headers: H })).json();
    if (s.status === "completed") return s.results;          // URL file
    if (s.status === "failed") throw new Error(`[${s.error_code}] ${s.error}`);
    await new Promise(r => setTimeout(r, poll));
  }
  throw new Error(`task ${task_id} hết thời gian chờ`);
}

// Video, văn bản→video
const urls = await generate("video", {
  prompt: "neon city at night, drone shot",
  model: "veo_31_fast",
  aspect_ratio: "16:9",
  resolution: ["720p"],
});
console.log(urls);
```

---

## 11. Checklist nhanh cho người tích hợp / AI agent

1. Bật máy chủ Webhook trong app; sao chép **port** và **API key**.
2. Luôn gửi `X-API-Key` ở các call generate/status/result/tasks.
3. `POST /api/{image|video|grok}/generate` với body hợp lệ (`prompt` bắt buộc).
4. Đọc `task_id` từ response `202`.
5. Hỏi `GET /api/status/{task_id}` mỗi 3–5 giây tới khi `completed` hoặc `failed`.
6. Khi `completed`: `GET` từng URL trong `results` để tải file.
7. Khi `failed`: xem `error_code` (vd `429` = hết quota) và `error` / `error_detail`.
8. Tôn trọng tỉ lệ theo từng model, số ảnh tham chiếu theo từng mode, và các tính
   năng chỉ dành cho ULTRA.
