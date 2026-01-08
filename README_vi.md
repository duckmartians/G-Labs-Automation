[![Download for Windows](https://img.shields.io/badge/Download%20for%20Windows-%F0%9F%92%BB-blue?style=for-the-badge)](https://github.com/duckmartians/G-Labs-Automation/releases/latest) [![Download Here](https://img.shields.io/badge/⬇_Download-Here-success?style=for-the-badge)](https://github.com/duckmartians/G-Labs-Automation/releases/latest)

Hướng dẫn sử dụng: [![Tiếng Việt](https://img.shields.io/badge/Tiếng%20Việt-green)](README_vi.md)

User manual: [![English](https://img.shields.io/badge/English-blue)](README.md) 

# G-Labs Automation - Hướng Dẫn Sử Dụng Chi Tiết v1.0.4

**Công cụ tự động hóa tạo ảnh & video AI sử dụng Google Labs (Imagen, Veo)**

---

## 📋 Mục Lục

- [Giới Thiệu](#-giới-thiệu)
- [Cài Đặt Ban Đầu](#️-cài-đặt-ban-đầu)
  - [Thêm Tài Khoản Google](#1-thêm-tài-khoản-google)
  - [Cấu Hình Proxy (Tùy Chọn)](#2-cấu-hình-proxy-tùy-chọn)
  - [Cài Đặt Chung](#3-cài-đặt-chung)
- [Tạo Ảnh với Imagen](#️-tạo-ảnh-với-imagen)
  - [Nano Banana (Standard)](#nano-banana-standard)
  - [Nano Banana Pro (Advanced)](#nano-banana-pro-advanced)
  - [Whisk](#whisk)
- [Tạo Video với Veo](#-tạo-video-với-veo)
  - [Text-to-Video](#1-text-to-video)
  - [Image-to-Video](#2-image-to-video)
  - [Components (3 References)](#3-components-3-references)
- [Workflow System](#-workflow-system)
  - [Các Loại Node](#các-loại-node)
  - [Tạo Workflow](#tạo-workflow)
  - [Batch Processing](#batch-processing)
  - [Lưu & Load Workflow](#lưu--load-workflow)
- [Tính Năng Nâng Cao](#-tính-năng-nâng-cao)
- [Xử Lý Lỗi](#-xử-lý-lỗi)
- [FAQ](#-faq)

---

## 🎯 Giới Thiệu

G-Labs Automation là công cụ desktop GUI giúp tự động hóa việc tạo ảnh và video AI thông qua Google Labs APIs:
- **Imagen 4 / Nano Banana**: Tạo ảnh từ text hoặc reference images
- **Veo 2**: Tạo video từ text, images hoặc components
- **Workflow System**: Tạo pipeline tự động với node-based editor

### Yêu Cầu Hệ Thống
- Windows 10/11
- Tài khoản Google có quyền truy cập Google Labs

---

## ⚙️ Cài Đặt Ban Đầu

### 1. Thêm Tài Khoản Google

#### Bước 1: Lấy Cookie
1. Mở trình duyệt Chrome/Edge
2. Đăng nhập vào [Google Labs](https://labs.google/fx)
3. Nhấn `F12` để mở DevTools
4. Chuyển sang tab **Application** → **Cookies**
5. Copy toàn bộ cookies (có thể dùng extension như [Cookie Editor](https://cookie-editor.cgagnier.ca/))

#### Bước 2: Thêm Vào Ứng Dụng
1. Vào tab **⚙️ Cài Đặt**
2. Click **📋 Thêm Tài Khoản**
3. Paste cookies vào ô **Cookies**
4. Nhập **Email** tương ứng
5. (Tùy chọn) Nhập **Proxy** nếu cần
6. Click **💾 Lưu**

**Format Cookie Hỗ Trợ:**
```
# Netscape format (from Cookie Editor)
.google.com	TRUE	/	TRUE	1234567890	COOKIE_NAME	value

# JSON format
[{"name": "COOKIE_NAME", "value": "...", "domain": ".google.com"}]

# Header format
COOKIE_NAME=value; ANOTHER=value2
```

#### Bước 3: Kiểm Tra
- Tài khoản xuất hiện trong danh sách với trạng thái **✅ Ready**
- Nếu lỗi, xem phần [Xử Lý Lỗi](#-xử-lý-lỗi)

---

### 2. Cấu Hình Proxy (Tùy Chọn)

Nếu sử dụng proxy để tránh rate limit hoặc geo-restriction:

**Format Proxy:**
```
# HTTP/HTTPS
http://username:password@proxy.example.com:8080

# SOCKS5
socks5://username:password@proxy.example.com:1080

# Không auth
http://proxy.example.com:8080
```

**Cách Thêm:**
1. Vào tab **⚙️ Cài Đặt**
2. Chọn tài khoản cần thêm proxy
3. Click **✏️ Sửa**
4. Nhập proxy vào ô **Proxy**
5. Click **💾 Lưu**

---

### 3. Cài Đặt Chung

Vào tab **⚙️ Cài Đặt** → Mục **Cài Đặt Chung**:

| Tùy Chọn | Mô Tả | Giá Trị Đề Xuất |
|----------|-------|-----------------|
| **Ngôn Ngữ** | Giao diện tiếng Việt/English | `Tiếng Việt` |
| **Output Folder** | Thư mục lưu ảnh/video | [./output](file:///d:/Data_Online/G-Labs%20Automation/DEV/PC-Home/G-Labs%20Automation%20-%20backup%208%20-%20Copy/src/ui/workflow/node_editor.py#541-544) |
| **Max Threads** | Số luồng xử lý đồng thời | `3-5` (Plus/Max) |
| **Auto Upscale** | Tự động upscale ảnh 1080p | `✅` |
| **Retry Count** | Số lần retry khi lỗi | `3` |

**Lưu ý:**
- Free accounts: 1 thread
- Plus accounts: 3-5 threads
- Max accounts: 5-10 threads

---

## 🖼️ Tạo Ảnh với Imagen

### Nano Banana (Standard)

Tạo ảnh từ prompt text + tối đa 4 reference images (2 Subject, 1 Scene, 1 Style).

#### Bước 1: Chọn Model
Vào tab **🎨 Tạo Ảnh** → Chọn **Nano Banana**

#### Bước 2: Nhập Prompt
```
Prompt: A futuristic cyberpunk city at night with neon lights
```

**Tips viết prompt:**
- Mô tả rõ ràng, chi tiết
- Thêm style keywords: `realistic, anime, oil painting, 3D render`
- Độ dài: 10-100 từ

#### Bước 3: Thêm Reference Images (Tùy Chọn)

**Subject (Chủ thể - Max 2):**
```
📷 Subject 1: [person.jpg]
📷 Subject 2: [object.jpg]
```

**Scene (Bối cảnh - Max 1):**
```
🌆 Scene: [background.jpg]
```

**Style (Phong cách - Max 1):**
```
🎨 Style: [art_style.jpg]
```

**Cách thêm:**
1. Click **📁 Chọn Ảnh** bên cạnh mỗi Reference
2. Chọn file ảnh (JPG, PNG, WebP)
3. Ảnh hiển thị preview

#### Bước 4: Cấu Hình

| Tùy Chọn | Giá Trị |
|----------|---------|
| **Aspect Ratio** | `1:1 Square` / `16:9 Landscape` / `9:16 Portrait` |
| **Resolution** | `720p` / `1080p (Upscale)` |
| **Number of Images** | `1-4` |
| **Seed Mode** | `Random` / `Fixed` |

#### Bước 5: Tạo Ảnh
1. Click **▶️ Tạo Ảnh**
2. Theo dõi tiến trình trong **Log**
3. Ảnh được lưu vào **Output Folder**

**File Output:**
```
output/
├── 01_A_futuristic_cyberpunk_city_subject1_20260108_220000.jpg
├── 02_A_futuristic_cyberpunk_city_subject1_20260108_220001.jpg
└── ...
```

---

### Nano Banana Pro (Advanced)

Tạo ảnh với tối đa **10 reference images** (không phân loại).

#### Điểm Khác Biệt
- ✅ Hỗ trợ 10 refs (vs 4 trong Standard)
- ✅ Không cần phân loại (Subject/Scene/Style)
- ⚠️ Chỉ hỗ trợ **Landscape (16:9)**

#### Cách Sử Dụng
1. Vào tab **🎨 Tạo Ảnh** → **Nano Banana Pro**
2. Nhập prompt
3. Click **📁 Thêm Reference** (max 10 lần)
4. Aspect ratio tự động lock ở **16:9**
5. Click **▶️ Tạo Ảnh**

---

### Whisk

Tạo ảnh từ 3 reference images (Subject + Scene + Style) mà không cần prompt.

#### Bước 1: Thêm References
1. Vào tab **🎨 Tạo Ảnh** → **Whisk**
2. Click **📁 Subject** → Chọn ảnh chủ thể
3. Click **📁 Scene** → Chọn ảnh bối cảnh
4. Click **📁 Style** → Chọn ảnh phong cách

**Yêu Cầu:**
- ✅ Cả 3 reference đều **bắt buộc**
- ✅ Không cần prompt (optional)

#### Bước 2: Cấu Hình & Tạo
- Chọn aspect ratio
- Chọn resolution
- Click **▶️ Tạo Ảnh**

---

## 🎬 Tạo Video với Veo

Vào tab **🎥 Tạo Video**

### 1. Text-to-Video

Tạo video từ mô tả text.

#### Cách Dùng
```
Prompt: A cat walking on a beach during sunset, waves crashing
```

1. Nhập prompt mô tả video
2. Chọn **Aspect Ratio**: 16:9 / 9:16 / 1:1
3. Chọn **Resolution**: 720p / 1080p
4. Click **▶️ Tạo Video**

**Tips:**
- Mô tả chuyển động rõ ràng: `walking, running, flying, rotating`
- Thêm chi tiết môi trường: `during sunset, in the rain, underwater`
- Độ dài: 10-50 từ

---

### 2. Image-to-Video

Tạo video chuyển động từ 1 hoặc 2 ảnh.

#### Mode 1: Single Image
```
Start Image: cat.jpg
Prompt: The cat starts walking forward
```

Video sẽ animate từ ảnh tĩnh theo prompt.

#### Mode 2: Two Images (Interpolation)
```
Start Image: cat_standing.jpg
End Image: cat_sitting.jpg
Prompt: The cat slowly sits down
```

Video sẽ transition từ start → end.

#### Cách Dùng
1. Click **📁 Start Image** → Chọn ảnh đầu
2. (Tùy chọn) Click **📁 End Image** → Chọn ảnh cuối
3. Nhập prompt mô tả chuyển động
4. Chọn aspect ratio & resolution
5. Click **▶️ Tạo Video**

---

### 3. Components (3 References)

Tạo video từ 3 ảnh reference (giống Whisk nhưng cho video).

#### Yêu Cầu
- ✅ 3 references: Subject + Style + Scene
- ⚠️ **Chỉ hỗ trợ Landscape (16:9)**

#### Cách Dùng
1. Click **📁 Subject** → Chọn ảnh chủ thể
2. Click **📁 Style** → Chọn ảnh phong cách
3. Click **📁 Scene** → Chọn ảnh bối cảnh
4. Nhập prompt mô tả chuyển động
5. Aspect ratio tự động lock ở **16:9**
6. Click **▶️ Tạo Video**

---

### Video Output

**File Format:**
```
output/
├── 01_prompt_text_slot1_20260108_220500.mp4
└── ...
```

**Thông Số:**
- Duration: ~5 seconds
- FPS: 24
- Codec: H.264
- Resolution: 1280x720 (720p) hoặc 1920x1080 (1080p)

---

## 🔀 Workflow System

Node-based visual programming cho automation phức tạp.

### Các Loại Node

#### 1. 📷 Reference (Ảnh Tham Chiếu)
**Inputs:** None  
**Outputs:** Image  
**Chức năng:** Load ảnh từ file làm input cho nodes khác

**Cách dùng:**
1. Right-click canvas → **Add: Reference Standard/Pro**
2. Click **📁 Browse** → Chọn ảnh
3. Kéo dây từ output socket

**2 Modes:**
- **Standard**: Cho Nano Banana (4 refs)
- **Pro**: Cho Nano Banana Pro (10 refs)

---

#### 2. 🎨 Generate (Tạo Ảnh)
**Inputs:** Image (0-10 refs)  
**Outputs:** Image  
**Chức năng:** Tạo ảnh từ prompt + references

**Widgets:**
- Model: Nano Banana / Nano Banana Pro
- Prompt: Text mô tả
- Aspect Ratio: 1:1 / 16:9 / 9:16
- Resolution: 720p / 1080p
- Seed Mode: Random / Fixed
- Number of Images: 1-4

**Validation Rules:**
- Nano Banana: Max 2 Subject + 1 Scene + 1 Style
- Nano Banana Pro: Max 10 refs, chỉ Landscape

---

#### 3. 💾 Save (Lưu Ảnh)
**Inputs:** Image  
**Outputs:** None  
**Chức năng:** Lưu ảnh ra file

**Widgets:**
- **📁 Select Folder**: Chọn thư mục lưu custom
- **Filename Prefix**: Tiền tố tên file

**Default:** Lưu vào Output folder chung

---

#### 4. 📂 Batch Loader (Load Ảnh Hàng Loạt)
**Inputs:** None  
**Outputs:** Image (batch)  
**Chức năng:** Load nhiều ảnh từ folder để xử lý tuần tự

**Widgets:**
- **📁 Select Folder**: Chọn folder chứa ảnh
- **Sort Order**: A-Z / Z-A / Newest / Oldest / Random
- **Limit**: All / Custom number

**Batch Processing:**
```
Folder: /my_images/
  ├── cat1.jpg
  ├── cat2.jpg
  └── cat3.jpg

→ Workflow runs 3 times (1 for each image)
```

---

#### 5. 📝 Batch Prompt (Load Prompt Hàng Loạt)
**Inputs:** None  
**Outputs:** Prompt (batch)  
**Chức năng:** Load nhiều prompts để xử lý tuần tự

**Widgets:**
- **Prompt List**: Nhập prompts (mỗi dòng 1 prompt)
- **Mode**: Sequential / Random
- **Limit**: All / Custom

**Example:**
```
A cat on the beach
A dog in the park
A bird in the sky
```
→ Workflow runs 3 times

---

#### 6. 🔄 Reroute
**Inputs:** Any  
**Outputs:** Any  
**Chức năng:** Điểm trung gian để dây gọn gàng hơn

---

### Tạo Workflow

#### Ví Dụ 1: Batch Image Generation
```
[Batch Prompt] → [Generate] → [Save]
```

**Mục tiêu:** Tạo nhiều ảnh từ list prompts

**Steps:**
1. Right-click → **Add: Batch Prompt**
2. Nhập prompts (mỗi dòng 1 cái)
3. Right-click → **Add: Generate**
4. Kéo dây: Batch Prompt output → Generate prompt input
5. Cấu hình Generate (model, ratio, etc.)
6. Right-click → **Add: Save**
7. Kéo dây: Generate output → Save input
8. Click **▶️ Run Workflow**

---

#### Ví Dụ 2: Reference Image + Batch Prompts
```
[Reference] ──┐
              ├─→ [Generate] → [Save]
[Batch Prompt]┘
```

**Mục tiêu:** Tạo variations của 1 ref image với nhiều prompts

**Steps:**
1. Add Reference node → Load ảnh
2. Add Batch Prompt node → Nhập 10 prompts
3. Add Generate node
4. Kết nối:
   - Reference output → Generate image input
   - Batch Prompt output → Generate prompt input
5. Add Save node → Connect
6. Run → Tạo 10 ảnh variations

---

### Batch Processing

#### Multiple Batch Nodes

Workflow hỗ trợ **nhiều Batch nodes** cùng lúc với **intelligent looping**.

**Example:**
```
[Batch Loader] ──┐ (5 images)
                 ├─→ [Generate] → [Save]
[Batch Prompt] ──┘ (10 prompts)
```

**Result:** 10 iterations (max count)
- Iteration 1: image1 + prompt1
- Iteration 2: image2 + prompt2
- Iteration 3: image3 + prompt3
- Iteration 4: image4 + prompt4
- Iteration 5: image5 + prompt5
- Iteration 6: image1 + prompt6 (loop back)
- ...

**Rule:** Modulo wrapping cho list ngắn hơn.

---

### Quick Add Menu

Kéo dây từ socket ra ngoài → Menu hiện để add node nhanh.

**Cách dùng:**
1. **Từ OUTPUT socket**: Kéo ra → Menu → Chọn node → Auto-connect
2. **Từ INPUT socket**: Kéo ra → Menu → Chọn node → Auto-connect ngược

**Keyboard Shortcuts:**
- `L`: Auto-arrange nodes
- `Ctrl+C`: Copy selected nodes
- `Ctrl+V`: Paste nodes
- `Ctrl+S`: Save workflow
- `Ctrl+O`: Load workflow
- [Delete](file:///d:/Data_Online/G-Labs%20Automation/DEV/PC-Home/G-Labs%20Automation%20-%20backup%208%20-%20Copy/src/ui/workflow/node_editor.py#143-196): Delete selected

---

### Lưu & Load Workflow

**Lưu:**
1. Click **💾 Save Workflow**
2. Chọn vị trí & tên file (.json)

**Load:**
1. Click **📂 Load Workflow**
2. Chọn file .json

**Format:**
```json
{
  "nodes": [...],
  "edges": [...],
  "groups": [...]
}
```

---

## 🚀 Tính Năng Nâng Cao

### 1. Import Prompts Hàng Loạt

Vào tab **🎨 Tạo Ảnh** hoặc **🎥 Tạo Video** → Click **📥 Import Prompts**

**Format hỗ trợ:**

**TXT File:**
```
A cat on the beach
A dog in the park
A bird in the sky
```

**Excel File (.xlsx):**
| Column A | Column B (ignored) |
|----------|-------------------|
| Prompt 1 | ... |
| Prompt 2 | ... |

**Mode:**
- **Append**: Thêm vào cuối prompts hiện tại
- Hỗ trợ Unicode & newlines trong Excel cells

---

### 2. Parallel Processing

**Plus/Max Accounts:**
- Cấu hình `Max Threads` = 3-5
- Workflow chạy đồng thời nhiều jobs
- Tiết kiệm thời gian cho batch lớn

**Smart Delays:**
- Account khác nhau: 10-20s stagger
- Cùng account: 20-30s delay

---

### 3. Captcha Queue System

Tự động xử lý reCAPTCHA trong background.

**Hoạt động:**
- Worker duy trì 1 Chrome instance
- Queue tokens cho workers
- Tránh block do quá nhiều captcha requests

---

### 4. Unified Filename Convention

Tất cả output files:
```
{row:02d}_{prompt}_{slot}_{timestamp}

Example:
01_A_cat_on_beach_subject1_20260108_220500.jpg
```

---

## 🛠️ Xử Lý Lỗi

### Lỗi Thường Gặp

#### 1. `401 Unauthorized`
**Nguyên nhân:** Cookie hết hạn  
**Giải pháp:**
1. Lấy cookie mới từ browser
2. Update lại trong **Cài Đặt** → **Sửa tài khoản**

---

#### 2. `403 Forbidden / reCAPTCHA Failed`
**Nguyên nhân:** Google block do spam requests  
**Giải pháp:**
- Đợi 5-10 phút
- Giảm số threads
- Sử dụng proxy
- Refresh browser cookies

---

#### 3. `400 Bad Request - INVALID_ARGUMENT`
**Nguyên nhân:** Tham số không hợp lệ  
**Kiểm tra:**
- Aspect ratio với model (Pro = Landscape only)
- Reference count (Nano Banana = 4 max, Pro = 10 max)
- Image format (JPG, PNG, WebP)

---

#### 4. Workflow `Missing Input`
**Nguyên nhân:** Node chưa có input cần thiết  
**Giải pháp:**
- Check tất cả required inputs đã connect
- Generate node cần prompt (từ widget hoặc socket)

---

#### 5. Không Tạo Được Video
**Nguyên nhân:**
- Components mode + không phải landscape
- Start image missing (image-to-video mode)

**Giải pháp:**
- Đổi aspect ratio về 16:9
- Thêm start image

---

## ❓ FAQ

### Q: Tài khoản Free có giới hạn gì?
**A:** 
- 1 thread (không parallel)
- Daily quota giới hạn bởi Google
- Tất cả tính năng khác đều available

---

### Q: Làm sao để tăng tốc độ?
**A:**
1. Nâng cấp lên Plus/Max account
2. Tăng `Max Threads` lên 3-5
3. Sử dụng nhiều Google accounts
4. Sử dụng proxy để tránh rate limit

---

### Q: Ảnh/video lưu ở đâu?
**A:** 
- Default: [./output/](file:///d:/Data_Online/G-Labs%20Automation/DEV/PC-Home/G-Labs%20Automation%20-%20backup%208%20-%20Copy/src/ui/workflow/node_editor.py#541-544)
- Custom: Cấu hình trong **Settings** → **Output Folder**
- Workflow Save node: Có thể chọn folder riêng

---

### Q: Reference image cần kích thước bao nhiêu?
**A:**
- Min: 200x200px
- Max: 4096x4096px (auto-resize)
- Format: JPG, PNG, WebP
- Recommended: 1024x1024px

---

### Q: Workflow có giới hạn số nodes?
**A:** Không. Tuy nhiên:
- Nhiều nodes phức tạp = chậm hơn
- Recommend: < 20 nodes/workflow

---

### Q: Có thể export video format khác không?
**A:** 
- Output mặc định: MP4 (H.264)
- Để convert: Dùng FFmpeg external
```bash
ffmpeg -i video.mp4 -c:v libx265 output.mp4
```

---

### Q: Làm sao để backup workflows?
**A:**
1. Save workflow thành .json files
2. Copy vào cloud storage (Google Drive, Dropbox)
3. Version control với Git

---

## 📞 Hỗ Trợ

- **Website**: [https://duckmartians.info/](https://duckmartians.info/)
- **Discord**: [https://discord.gg/munMZEBMw5](https://discord.gg/munMZEBMw5)

---

**Tạo bởi Đặng Minh Đức [@duckmartians](https://github.com/duckmartians)**
