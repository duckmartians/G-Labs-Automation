# HƯỚNG DẪN SỬ DỤNG G-LABS AUTOMATION (Phiên bản v1.0.1)

Chào mừng bạn đến với **G-Labs Automation** – Giải pháp tự động hóa sáng tạo nội dung AI hàng đầu. Đây không chỉ là một công cụ tạo ảnh, mà là nền tảng **All-in-One** giúp bạn xây dựng quy trình làm việc (Workflow) chuyên nghiệp, tối ưu hóa thời gian và khai thác tối đa sức mạnh của các mô hình AI tiên tiến nhất hiện nay.

---

## 1. Cài Đặt Phần Mềm

Phần mềm đã được đóng gói thành bộ cài đặt tiêu chuẩn (Installer), giúp việc thiết lập trở nên đơn giản chỉ với vài cú click chuột.

1. **Tải Bộ Cài Đặt:**
* Tìm file cài đặt có tên `Setup_G-Labs_Automation_v1.0.1.exe`.


2. **Chạy File Cài Đặt:**
* Nhấn đúp vào file `.exe` vừa tải.


3. **Thiết Lập:**
* Chọn ngôn ngữ cài đặt (Mặc định: English) -> Nhấn **OK**.
* Chọn thư mục cài đặt (Mặc định nằm trong `AppData\Local`) -> Nhấn **Next**.
* Chọn tạo shortcut ngoài màn hình (Create a desktop shortcut) -> Nhấn **Next** -> **Install**.


4. **Hoàn Tất:**
* Chờ quá trình cài đặt hoàn tất (Trình duyệt lõi Chromium sẽ được tự động giải nén kèm theo).
* Nhấn **Finish** để khởi động phần mềm ngay lập tức.



---

## 2. Thiết Lập Tài Khoản (Bước Quan Trọng Nhất)

Để sử dụng các tính năng tạo ảnh, bạn cần kết nối tài khoản Google. Phần mềm hỗ trợ **đa tài khoản** để tối ưu hóa hiệu suất.

1. Tại giao diện chính, chọn mục **"Cài đặt" (Settings)** ở thanh điều hướng bên trái hoặc icon bánh răng.
2. Tại tab **"Tài khoản Google"**, nhấn nút **"Thêm tài khoản"** (Add Account).
3. Một cửa sổ trình duyệt sẽ hiện lên. Hãy đăng nhập vào tài khoản Google của bạn.
4. Sau khi đăng nhập thành công, phần mềm sẽ tự động lưu Session và Cookie an toàn vào máy tính của bạn.
5. **Lặp lại** bước trên nếu bạn muốn thêm nhiều tài khoản khác (Giúp chạy đa luồng nhanh hơn).

> **💡 Mẹo:** Bạn có thể cấu hình Proxy cho từng tài khoản nếu muốn quản lý IP riêng biệt.

---

## 3. Chế Độ Tạo Ảnh (AI Image Generator)

Đây là chế độ cơ bản giúp bạn tạo hàng loạt ảnh nhanh chóng từ danh sách Prompts.

1. **Chọn Chế độ:** Nhấn vào **"Tạo Ảnh" (AI Image)** ở menu bên trái.
2. **Cấu hình Model:**
* **Imagen 4 & Nano Banana:** Sử dụng MIỄN PHÍ và KHÔNG GIỚI HẠN số lượng ảnh tạo ra. Phù hợp cho mọi tài khoản thường.
* **Nano Banana Pro:** Yêu cầu tài khoản Google có gói **Gemini Advanced (Pro/Ultra)** trở lên. Cho chất lượng ảnh vượt trội và chi tiết cao hơn.


3. **Thiết lập thông số:**
* **Tỷ lệ ảnh:** Chọn tỷ lệ mong muốn (16:9, 9:16, 1:1, v.v.).
* **Số lượng:** Số ảnh muốn tạo cho mỗi Prompt.
* **Số luồng:** Số lượng tác vụ chạy song song (Tùy thuộc vào gói License của tool bạn đang sở hữu: Basic, Plus hoặc Max).


4. **Nhập Prompt:**
* Nhập danh sách mô tả ảnh vào ô trống, mỗi dòng là một prompt riêng biệt.


5. **Chạy:**
* Nhấn **"🚀 CHẠY NGAY" (START NOW)**. Phần mềm sẽ tự động phân phối công việc cho các tài khoản đang hoạt động.



---

## 4. Chế Độ Workflow (Sáng Tạo Không Giới Hạn)

Đây là tính năng mạnh mẽ nhất của G-Labs Automation, cho phép bạn thiết kế quy trình làm việc dạng sơ đồ tư duy (Node-based).

* **Truy cập:** Chọn tab **"Luồng CV" (Workflow)** ở menu trái.
* **Cách hoạt động:** Bạn kéo thả các "Node" (Khối chức năng) và nối chúng lại với nhau để tạo thành một dây chuyền xử lý tự động.
* **Các Node hỗ trợ:**
* **Batch Loader:** Tự động quét và lấy ảnh từ thư mục trên máy tính để làm nguyên liệu đầu vào.
* **Batch Prompt:** Nạp danh sách prompt hàng loạt từ file text.
* **Reference (Ảnh tham chiếu):** Dùng ảnh mẫu để hướng dẫn AI tạo ảnh theo phong cách hoặc bố cục mong muốn (Hỗ trợ chế độ Standard và Pro).
* **Generate (Tạo ảnh):** Khối xử lý chính để sinh ra hình ảnh từ Prompt và Reference.
* **Save (Lưu):** Tự động lưu kết quả vào thư mục chỉ định.
* **Reroute:** Giúp đi dây nối gọn gàng hơn.



> **✨ Điểm đặc biệt:** Bạn có thể tạo ra các quy trình cực kỳ phức tạp, ví dụ: *Load 100 ảnh sản phẩm -> Dùng làm Reference -> Tạo ảnh quảng cáo với Model Nano Banana -> Lưu kết quả*. Tất cả diễn ra tự động hoàn toàn!

---

## 5. Lưu Ý Về Gói Tài Khoản & Model

Để đảm bảo trải nghiệm tốt nhất, hãy lưu ý sự khác biệt giữa các Model:

| Model AI | Yêu cầu Tài khoản Google | Giới hạn |
| --- | --- | --- |
| **Imagen 4** | Tài khoản thường (Free) | **KHÔNG GIỚI HẠN** số lượng tạo. |
| **Nano Banana** | Tài khoản thường (Free) | **KHÔNG GIỚI HẠN** số lượng tạo. |
| **Nano Banana Pro** | **Gemini Advanced (Pro/Ultra)** | Yêu cầu gói trả phí của Google. Chất lượng cao nhất. |

---

## 6. Lộ Trình Phát Triển (Future Roadmap)

G-Labs Automation đang hướng tới trở thành công cụ **"All-in-One"** cho nhà sáng tạo nội dung. Trong các bản cập nhật sắp tới, chúng tôi sẽ không chỉ giới hạn ở Google mà sẽ mở rộng tích hợp đa nền tảng:

* **Đa Nền Tảng AI:** Tích hợp các model đình đám như **Meta (Imagine)**, **Hailuo**, **Grok (xAI)**, **Sora (OpenAI)** ngay trên cùng một giao diện.
* **Tạo Video AI:** Module "AI Video" sẽ sớm được kích hoạt, cho phép tạo video chất lượng cao tự động.
* **Viết Kịch Bản (AI Script):** Tự động hóa quy trình sáng tạo nội dung từ ý tưởng đến kịch bản chi tiết.

Chúng tôi cam kết cập nhật liên tục để mang lại những công nghệ mới nhất cho cộng đồng người dùng.

---

*Chúc bạn có những trải nghiệm tuyệt vời và bùng nổ sáng tạo cùng G-Labs Automation!*
