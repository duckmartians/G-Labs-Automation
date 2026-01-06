# HƯỚNG DẪN SỬ DỤNG TOÀN TẬP: G-LABS AUTOMATION v1.0.3

**Siêu Công Cụ Tự Động Hóa Tạo Ảnh & Video AI Đỉnh Cao**

Chào mừng bạn đến với **G-Labs Automation** – giải pháp "All-in-One" giúp bạn khai thác tối đa sức mạnh của các mô hình AI tiên tiến nhất hiện nay (Imagen, Veo, Nano Banana) từ Google Labs. Tool được thiết kế để biến những công việc thủ công nhàm chán thành quy trình tự động hóa thông minh, giúp bạn tiết kiệm hàng trăm giờ làm việc.

Dưới đây là hướng dẫn chi tiết từ A-Z.

---

## PHẦN 1: TẢI VỀ VÀ CÀI ĐẶT

### 1. Tải phần mềm

Đầu tiên, bạn cần tải bộ cài đặt chính thức tại kho lưu trữ của chúng tôi:

* **Link tải:** [https://github.com/duckmartians/G-Labs-Automation](https://github.com/duckmartians/G-Labs-Automation)
* **File cần tải:** `Setup_G-Labs_Automation_v1.0.3.exe`

### 2. Cài đặt lên máy tính

1. Nhấn đúp vào file `.exe` vừa tải về.
2. Chọn ngôn ngữ cài đặt (English) và nhấn **Next**.
4. Sau khi cài xong, tích chọn "Launch G-Labs Automation" và nhấn **Finish**.

> **Lưu ý:** Tool sẽ tự động tải thêm các thành phần bổ trợ (như trình duyệt lõi) trong lần chạy đầu tiên, vui lòng đợi một chút.

---

## PHẦN 2: TƯ DUY VỀ TÀI KHOẢN (RẤT QUAN TRỌNG)

Để sử dụng hiệu quả và an toàn, bạn cần phân biệt rõ **2 loại tài khoản** trong tool:

1. **Tài Khoản Bản Quyền (License Account):**
* Đây là tài khoản Google chính chủ của bạn dùng để đăng nhập *vào phần mềm* lần đầu tiên.
* Hệ thống sẽ ghi nhận gói cước (Basic/Plus/Max) dựa trên email này.
* **Khuyến nghị:** Nên dùng Email chính, có độ tin cậy cao để đảm bảo quyền lợi mua hàng và hỗ trợ lâu dài.


2. **Tài Khoản Chạy Tool (Worker Accounts):**
* Đây là các tài khoản Google (Gmail) được thêm vào trong phần *Cài đặt (Settings)* để thực hiện việc tạo ảnh/video.
* Tool hỗ trợ thêm **không giới hạn** số lượng tài khoản worker.
* Trong tương lai, chúng tôi sẽ hỗ trợ thêm các nền tảng khác, nên worker account không chỉ giới hạn ở Google.
* **Mẹo:** Bạn có thể dùng các tài khoản phụ, tài khoản giá rẻ để chạy tính năng này mà không lo ảnh hưởng đến Email bản quyền chính.



---

## PHẦN 3: THIẾT LẬP HỆ THỐNG & THÊM TÀI KHOẢN

Trước khi bắt đầu, hãy nạp "nguyên liệu" (tài khoản worker) cho cỗ máy này.

1. Tại giao diện chính, bấm nút **"⚙️ Cài đặt" (Settings)** hoặc biểu tượng bánh răng ở góc dưới bên trái.
2. Chuyển sang tab **"Tài khoản Google"**.
3. **Thêm tài khoản:**
* **Cách 1 (Tự động):** Bấm nút **"➕ Thêm tài khoản"**. Một trình duyệt sẽ hiện ra, bạn chỉ cần đăng nhập Gmail như bình thường. Tool sẽ tự động bắt lấy Cookie và Token.
* **Cách 2 (Thủ công):** Dán Cookie (định dạng JSON hoặc Netscape) vào ô trống và bấm "Thêm".


4. **Cấu hình Proxy (Dành cho dân chuyên):**
* Để nuôi số lượng lớn tài khoản và chạy đa luồng mà không bị Google chặn IP, bạn nên gán Proxy cho từng tài khoản.
* Bấm vào biểu tượng "Sửa" (cây bút) bên cạnh tài khoản để thêm Proxy (HTTP/SOCKS5).



> **Điểm tối ưu:** Tool có cơ chế **Auto-Renew Token**. Khi Token của Google hết hạn, tool sẽ tự động mở trình duyệt ngầm để gia hạn phiên làm việc (Session), đảm bảo quy trình treo máy 24/7 không bị gián đoạn.

---

## PHẦN 4: TẠO ẢNH AI (AI IMAGE) - TỐI ƯU HÓA CHI PHÍ

Chức năng này giúp bạn tạo hàng nghìn ảnh mỗi ngày với chi phí cực thấp hoặc miễn phí hoàn toàn.

### 1. Lựa chọn Model thông minh

* **Imagen 4 & Nano Banana:** Đây là điểm mạnh nhất! Bạn có thể sử dụng **tài khoản Gmail miễn phí (Free Tier)** để chạy các model này. Tool tự động luân chuyển tài khoản để tận dụng tối đa quota miễn phí.
* **Nano Banana Pro:** Yêu cầu tài khoản Gmail gói **PRO** hoặc **ULTRA**.

### 2. Upscale (Nâng cấp độ phân giải)

* Hỗ trợ upscale lên **2K** (cần tài khoản PRO/ULTRA) và **4K** (cần tài khoản ULTRA).
* Giúp ảnh sắc nét vượt trội so với bản gốc.

### 3. Quản lý hàng đợi (Queue Manager)

* Bạn không cần ngồi chờ từng ảnh. Hãy nhập hàng loạt Prompt, cấu hình số lượng, tỷ lệ ảnh, sau đó bấm **"➕ Thêm vào hàng chờ"**.
* Bấm **"🚀 CHẠY NGAY"** và đi làm việc khác. Tool sẽ tự động xử lý từng task, tự động thử lại (retry) nếu lỗi mạng, và tự động bỏ qua nếu gặp lỗi Prompt vi phạm chính sách.

---

## PHẦN 5: WORKFLOW EDITOR - TÙY BIẾN CỰC CAO (ĐIỂM NHẤN)

Đây là tính năng dành cho người dùng chuyên nghiệp muốn kiểm soát mọi ngóc ngách của quy trình sáng tạo. Không cứng nhắc như các tool khác, Workflow của G-Labs cho phép bạn "vẽ" quy trình làm việc:

* **Giao diện Node-based:** Kéo thả các node (nút) chức năng và nối dây chúng lại với nhau.
* *Ví dụ:* `Batch Loader (Load ảnh từ thư mục)` -> `Reference Node (Lấy ảnh làm mẫu)` -> `Generate Node (Tạo ảnh mới)` -> `Save Node (Lưu ảnh)`.


* **Tính tùy biến cực cao:**
* Bạn có thể tạo một luồng chạy đồng thời 2-3 model khác nhau để so sánh kết quả.
* Có thể dùng đầu ra của lần tạo 1 làm đầu vào (Reference) cho lần tạo 2 (Image-to-Image nâng cao).


* **Batch Processing (Xử lý lô):**
* **Batch Image Loader:** Tự động quét một thư mục trên máy tính, lấy từng ảnh ra để xử lý và tự động lặp lại cho đến hết folder.
* **Batch Prompt Loader:** Tự động đọc từng dòng trong file text để làm prompt.



> **Tại sao nó thông minh?** Bạn có thể lưu lại các Workflow (file `.json`) thành công để tái sử dụng hoặc chia sẻ cho team. Tool xử lý logic luồng cực nhanh và đa luồng.

---

## PHẦN 6: TẠO VIDEO AI (VIDEO CREATOR) - SỨC MẠNH CỦA VEO

Đây là tính năng "sát thủ" với khả năng tối ưu hóa tài nguyên cực tốt.

### 1. Cơ chế tài khoản thông minh

* **Model Veo 3.1 Fast (Lower Priority):** Tool cho phép bạn **tạo vô hạn video** nếu bạn sở hữu tài khoản Gmail gói **ULTRA**. Đây là một món hời lớn so với việc mua credits ở các nền tảng khác.
* Tool tự động lọc ra các tài khoản đủ điều kiện (Pro/Ultra) để chạy tác vụ video, các tài khoản thường sẽ không bị lãng phí vào đây.

### 2. Tab "Tạo video từ các thành phần" - Đỉnh cao nhận diện

Đây là tính năng thông minh nhất giúp bạn làm video hàng loạt (Bulk Create):

* **Bài toán:** Bạn có 100 câu prompt, và bạn có 100 ảnh nhân vật (Character) + 100 ảnh bối cảnh (Background). Bạn muốn tạo 100 video khớp nhau.
* **Giải pháp của Tool:**
* Bạn chỉ cần chọn thư mục chứa ảnh.
* Tool sẽ **tự động quét tên file ảnh** và so sánh với **từ khóa trong Prompt**.
* *Ví dụ:* Prompt là "A cat running in the forest". Nếu trong folder ảnh có file `cat.png` và `forest.jpg`, tool sẽ **tự động nhặt** 2 ảnh này ném vào ô Reference Image của dòng prompt đó.
* Điều này giúp bạn không phải ngồi chọn thủ công từng ảnh cho từng prompt.



### 3. Các chế độ ghép (Pair Mode)

* **Start - End:** Tạo video chuyển cảnh từ ảnh A sang ảnh B.
* **Chain Mode (Nối tiếp):** Tự động lấy ảnh End của video 1 làm ảnh Start của video 2. Cực kỳ hữu ích để làm các video storytelling dài và liền mạch.

---

## PHẦN 7: CÁC ĐIỂM TỐI ƯU KHÁC

1. **Multi-threading (Đa luồng):**
* Chạy song song nhiều tài khoản cùng lúc. Gói MAX hỗ trợ số luồng không giới hạn (tùy thuộc vào sức mạnh máy tính và số lượng tài khoản bạn có).


2. **Chống phát hiện (Anti-Detect):**
* Tích hợp sẵn `Playwright` với cơ chế giả lập vân tay trình duyệt (Stealth JS), giúp giảm thiểu tối đa việc bị Google quét checkpoint hay khóa tài khoản.


3. **Tự động cập nhật (Auto Update):**
* Bạn không cần lo lắng về việc bỏ lỡ tính năng mới. Ngay khi mở tool, nếu có phiên bản mới (với các tính năng như Script Writer, Auto Upload Youtube sắp ra mắt), tool sẽ hiển thị thông báo ngay trên thanh trạng thái để bạn cập nhật.



---

**G-Labs Automation** không chỉ là một công cụ, nó là một trợ lý ảo cần mẫn giúp bạn nhân bản năng suất làm việc lên gấp nhiều lần.

Hãy bắt đầu ngay hôm nay! Nếu cần hỗ trợ hoặc mua bản quyền nâng cao (Plus/Max), hãy liên hệ với chúng tôi qua thông tin trong phần Cài đặt.

*Chúc bạn tạo ra những tác phẩm tuyệt vời!*
