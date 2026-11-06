### PHẦN 1: THIẾT KẾ PROMPT 3 THÀNH PHẦN CHUẨN HÓA

#### 1. Phân tích nguyên nhân câu hỏi cụt ngủn dẫn đến hiện tượng Ảo giác AI (AI Hallucination)
- **Thiếu bối cảnh kỹ thuật (Context):** Câu hỏi *"AI ơi phần mềm trạm sạc của tôi bị hỏng rồi"* không cung cấp hệ điều hành, tên phần mềm, hay bất kỳ dòng mã lỗi thực tế nào.
- **Hệ quả:** AI bị đặt vào tình huống phải "đoán mò". Do thiếu dữ liệu đầu vào, AI sinh ra hiện tượng **ảo giác (Hallucination)**, tự bịa ra các kịch bản phần cứng cực đoan (như khuyên tháo tung vỏ sắt trạm sạc kiểm tra dây điện) thay vì tập trung vào lỗi phần mềm thực tế.

#### 2. Câu Prompt 3 Thành phần hoàn chỉnh dựa trên nhật ký lỗi
```text
[Context]
Tôi là kỹ thuật viên vận hành hệ thống Kiosk nạp năng lượng thông minh VoltCharge trên máy tính chạy hệ điều hành Windows 11 trong ca trực đêm tại Bến xe Miền Đông. Phần mềm điều khiển trụ sạc nhanh số 04 bất ngờ dừng hoạt động.

[Raw Error]
[TERMINAL ERROR LOG - VOLTCHARGE CHARGER #04]
2026-09-21 21:15:30 [FATAL] ControllerDaemon failed to start.
System.UnauthorizedAccessException: Access to the path 'C:\Program Files\VoltCharge\data\session.log' is denied.
  at System.IO.FileStream.ValidateFileHandle(SafeFileHandle handle)
  at VoltCharge.Core.SessionManager.Initialize()
InnerException: None. Process terminated with exit code 5 (0x00000005).
Suggestion: Run the application with elevated administrator privileges or verify NTFS file permissions.

[Instruction]
Hãy đóng vai trò là một Trợ giảng Kỹ thuật CNTT 24/7:
1. Dịch thông báo lỗi trên sang tiếng Việt đời thường và giải thích nguyên nhân gốc rễ.
2. Hướng dẫn 3 bước thao tác cụ thể tại chỗ trên Windows 11 để cấp quyền Quản trị viên và phân quyền ghi file giúp trạm sạc hoạt động trở lại.
```
# PHẦN 2: GIẢI MÃ LỖI & KỊCH BẢN KHẮC PHỤC

### 3. Bản dịch và giải thích nguyên nhân gốc rễ (Đóng vai Trợ lý AI)
- **Bản dịch thông báo lỗi:** 
  > *"Truy cập bị từ chối: Không có quyền truy cập vào đường dẫn tệp `session.log` trong thư mục `C:\Program Files\VoltCharge\data\`."*
- **Nguyên nhân gốc rễ:** 
  Ứng dụng điều khiển (`ControllerDaemon`) của trạm sạc khi khởi động cố gắng tạo hoặc ghi dữ liệu nhật ký (`session.log`) vào một thư mục được bảo vệ đặc biệt của hệ điều hành (`Program Files`). Do tài khoản đang chạy không có quyền Quản trị viên (`Administrator`), Windows đã lập tức chặn lại và ngắt tiến trình để bảo mật (mã lỗi `5 - Access is denied`).

---

### 4. Ba bước xử lý cụ thể tại chỗ trên Windows 11
1. **Bước 1: Chạy ứng dụng với quyền cao nhất (Run as Administrator)**
   * Nhấn chuột phải vào biểu tượng hoặc file thực thi của phần mềm quản lý trạm sạc VoltCharge trên màn hình/thư mục.
   * Chọn **Run as administrator** để cấp quyền quản trị tối cao giúp ứng dụng vượt qua tường lửa quyền hạn của thư mục hệ thống.
2. **Bước 2: Phân quyền ghi file cho thư mục dữ liệu (NTFS Permissions)**
   * Mở File Explorer, truy cập vào đường dẫn thư mục `C:\Program Files\VoltCharge\data`.
   * Nhấp chuột phải vào thư mục `data` ➔ chọn **Properties** ➔ chuyển sang tab **Security**.
   * Bấm nút **Edit** ➔ chọn tài khoản người dùng hiện tại và tích chọn **Full control** hoặc **Modify** ở cột Allow. Bấm **Apply** rồi **OK** để lưu lại.
3. **Bước 3: Khởi động lại dịch vụ (Restart Daemon/Service)**
   * Khởi động lại ứng dụng điều khiển hoặc khởi động lại hoàn toàn máy tính (Kiosk) để hệ thống làm mới tiến trình và nhận quyền hạn phân giải mới.

---

### 5. Xử lý bẫy an ninh (Bảo mật thông tin nhạy cảm)
- **Cách xử lý của kỹ thuật viên:** 
  Trước khi copy dán đoạn nhật ký lỗi (Raw Error Log) lên các nền tảng AI công cộng như ChatGPT hay Gemini, kỹ thuật viên **bắt buộc phải xóa, che mờ hoặc đổi** chuỗi mã khóa bí mật `API_KEY: volt_secret_token_889922` thành các ký tự ẩn (ví dụ: `API_KEY: [REDACTED]` hoặc `API_KEY: ********`).
- **Giải thích lý do:** 
  Các nền tảng AI công cộng có thể lưu trữ lịch sử đoạn chat hoặc dữ liệu huấn luyện. Nếu để lộ thông tin xác thực (`API Keys`), token bảo mật hoặc mật khẩu cơ sở dữ liệu của doanh nghiệp, kẻ xấu có thể lợi dụng để xâm nhập, chiếm quyền điều khiển hoặc phá hoại hệ thống hạ tầng mạng lưới trạm sạc thông minh.
