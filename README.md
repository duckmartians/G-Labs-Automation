# 📘 TÀI LIỆU HƯỚNG DẪN SỬ DỤNG: G-LABS AUTOMATION

**(Phiên bản v1.0.1 - All-in-One AI Content Creator)**

---

## 🌟 1. GIỚI THIỆU CHUNG & TẦM NHÌN

**G-Labs Automation** không chỉ là một tool "bấm nút tạo ảnh". Đây là một **hệ sinh thái tự động hóa** được thiết kế cho các nhà sáng tạo nội dung (Content Creators), Marketing Agency và những người đam mê AI.

**Điểm khác biệt cốt lõi:**

* **Vô hạn & Miễn phí:** Khai thác tối đa tài nguyên Google Labs để tạo ảnh không giới hạn.
* **Workflow Chuyên Nghiệp:** Tự do thiết kế quy trình làm việc (Node-based) thay vì bị gò bó trong các nút bấm cố định.
* **Tương lai All-in-One:** Hướng tới việc tích hợp tất cả các siêu AI (Meta, Grok, Sora...) vào một phần mềm duy nhất.

---

## 📥 2. CÀI ĐẶT & THIẾT LẬP BAN ĐẦU

### Bước 1: Cài đặt Phần mềm

1. Tải file cài đặt `Setup_G-Labs_Automation_v1.0.1.exe`.
2. Chạy file `.exe`. Nếu Windows hiện cảnh báo (do phần mềm mới chưa có chứng chỉ số lượng lớn), chọn **More Info** -> **Run anyway**.
3. Làm theo hướng dẫn trên màn hình (Next -> Next -> Install).
* *Lưu ý:* Trình cài đặt đã tích hợp sẵn nhân trình duyệt Chromium cần thiết, bạn không cần cài thêm gì cả.



### Bước 2: Quản lý Tài khoản (Hệ thống Multi-Account)

Đây là "trái tim" của phần mềm. Việc quản lý tốt tài khoản giúp bạn chạy đa luồng cực mạnh.

1. Mở phần mềm, vào menu **Cài đặt (Settings)** -> Tab **Tài khoản Google**.
2. Bấm **Thêm tài khoản (Add Account)** -> Đăng nhập Gmail của bạn trên cửa sổ trình duyệt hiện ra.
3. **Cơ chế phân loại tài khoản:**
* Phần mềm tự động nhận diện loại tài khoản (Free, Gemini Advanced).
* Nên add nhiều tài khoản Free để tận dụng tối đa tốc độ (Load balancing).


4. **Proxy (Nâng cao):** Nếu dùng >5 tài khoản, hãy gán Proxy (HTTP/SOCKS5) cho từng acc để tránh bị Google chặn IP.

---

## 🎨 3. CHẾ ĐỘ 1: TẠO ẢNH HÀNG LOẠT (AI IMAGE)

Chế độ này dành cho việc "sản xuất công nghiệp" - nhanh, gọn, số lượng lớn.

### A. Lựa chọn Model (Quan trọng)

* **🟢 Imagen 4 / Nano Banana (Khuyên dùng):**
* **Giá:** MIỄN PHÍ 100%.
* **Giới hạn:** KHÔNG GIỚI HẠN số lượng ảnh.
* **Ứng dụng:** Tạo ảnh minh họa, idea, stock photo số lượng lớn.


* **🟣 Nano Banana Pro:**
* **Yêu cầu:** Tài khoản Google phải có gói **Gemini Advanced** (Pro/Ultra).
* **Sức mạnh:** Hiểu prompt cực sâu, chi tiết ảnh sắc nét vượt trội (High Fidelity), hỗ trợ text trong ảnh tốt hơn.



### B. Thiết lập thông số

1. **Tỷ lệ (Ratio):** Hỗ trợ đầy đủ 16:9 (Youtube), 9:16 (Tiktok/Reels), 1:1 (Avatar/Insta).
2. **Số lượng (Count):** Số ảnh sinh ra cho MỖI dòng prompt.
3. **Số luồng (Threads):**
* *Tài khoản Free:* Mặc định 1 luồng.
* *License Plus/Max:* Mở khóa chạy song song 10-20 luồng cùng lúc (Tốc độ tên lửa).



### C. Nhập Prompt & Chạy

* Nhập danh sách prompt (mỗi dòng 1 cái).
* Bấm **"🚀 CHẠY NGAY"**.
* Ảnh sẽ tự động lưu vào thư mục `Output`, được phân loại gọn gàng.

---

## 🔄 4. CHẾ ĐỘ 2: WORKFLOW EDITOR (SÁNG TẠO KHÔNG GIỚI HẠN)

Đây là tính năng "Killer Feature" giúp bạn tùy biến quy trình làm việc như những chuyên gia AI thực thụ (tương tự ComfyUI nhưng dễ dùng hơn).

### Cách tư duy theo "Node" (Khối)

Thay vì bấm nút, bạn sẽ nối dây các khối chức năng để tạo ra một dây chuyền tự động.

### Danh sách các Node & Công dụng:

#### 1. Input Nodes (Đầu vào)

* **📁 Batch Loader (Load ảnh hàng loạt):**
* Chỉ đường dẫn đến 1 thư mục ảnh trên máy tính.
* Node này sẽ lần lượt bốc từng ảnh ra để xử lý (Vòng lặp).
* *Ứng dụng:* Dùng để nạp 1000 ảnh sản phẩm thô vào quy trình.


* **📝 Batch Prompt (Load chữ hàng loạt):**
* Nạp danh sách prompt từ text.
* Kết hợp với Batch Loader để vừa thay ảnh, vừa thay prompt tự động.



#### 2. Processing Nodes (Xử lý)

* **📷 Reference (Ảnh tham chiếu):**
* Nhận ảnh từ *Batch Loader* hoặc ảnh tải lên trực tiếp.
* **Chức năng:** Dùng ảnh này làm mẫu về Bố cục (Structure) hoặc Phong cách (Style) cho ảnh mới.
* *Ví dụ:* Bạn có ảnh cái chai (Input) -> Nối vào Ref Node (chế độ Subject) -> AI sẽ giữ nguyên hình dáng cái chai nhưng thay đổi background.


* **🎨 Generate (Tạo ảnh AI):**
* Trái tim của workflow. Nhận dữ liệu từ Ref Node và Prompt.
* Tại đây bạn chọn Model (Imagen 4/Nano Banana/Pro) và Tỷ lệ ảnh.



#### 3. Utility Nodes (Tiện ích)

* **🔀 Reroute:** Nút trung gian giúp đi dây nối đẹp và gọn hơn.
* **💾 Save (Lưu):** Điểm cuối của quy trình. Tự động lưu ảnh thành phẩm với tên file tùy chỉnh (có thể thêm tiền tố, hậu tố, ngày giờ).

### Ví dụ Kịch bản Workflow: "Biến hóa sản phẩm thời trang"

1. **Node 1 (Batch Loader):** Trỏ vào thư mục chứa 50 ảnh ma-nơ-canh mặc áo.
2. **Node 2 (Reference):** Nối từ Node 1 sang. Chọn chế độ "Subject" (Giữ nguyên cái áo).
3. **Node 3 (Generate):** Nối từ Node 2 sang. Prompt: "A model wearing this shirt, walking on paris street, fashion photography". Chọn Model Nano Banana Pro.
4. **Node 4 (Save):** Nối từ Node 3 sang.
5. **Kết quả:** Bạn bấm "Run Flow", tool tự động biến 50 ảnh ma-nơ-canh thành 50 ảnh người mẫu thật đang đi ở Paris.

---

## ⚙️ 5. QUẢN LÝ TÁC VỤ (QUEUE MANAGER)

Khi bạn làm việc chuyên nghiệp, bạn sẽ không ngồi chờ từng ảnh xong.

* **Thêm vào hàng đợi (Add to Queue):** Bạn có thể setup 10 task khác nhau (Task 1: Mèo, Task 2: Xe hơi, Task 3: Workflow sản phẩm...).
* **Quản lý:** Bấm vào nút "Queue Manager" để xem tiến độ, xóa, hoặc ưu tiên task nào chạy trước.
* Tool sẽ tự động chạy lần lượt đến khi hết danh sách, kể cả khi bạn đi ngủ.

---

## 🚀 6. LỘ TRÌNH PHÁT TRIỂN (ROADMAP) - TƯƠNG LAI CỦA G-LABS

Chúng tôi đang xây dựng G-Labs Automation trở thành một "Siêu công cụ" (Super App) cho AI

---

**G-Labs Automation - Trao quyền năng AI vào tay bạn.**
