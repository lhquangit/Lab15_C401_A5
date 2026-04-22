# Use Case List - MVP

## Use case bắt buộc để go-live

### UC-01: Tóm tắt hồ sơ bệnh án
- Mục tiêu: giúp bác sĩ rút ngắn thời gian đọc và tổng hợp hồ sơ.
- Input: mã hồ sơ hoặc hồ sơ đã được cấp quyền truy cập.
- Output: bản tóm tắt theo template chuẩn.
- Điều kiện an toàn: phải có chỉ dấu nguồn và chỉ hoạt động ở chế độ `read-only`.

### UC-02: Hỏi đáp SOP/quy trình nội bộ
- Mục tiêu: giúp điều dưỡng và nhân sự nội bộ tra cứu quy trình nhanh, nhất quán.
- Input: câu hỏi liên quan tới quy trình, biểu mẫu, chính sách nội bộ.
- Output: câu trả lời kèm trích nguồn.
- Điều kiện an toàn: nếu không đủ bằng chứng thì từ chối trả lời hoặc chuyển human review.

### UC-03: Hỏi đáp hành chính
- Mục tiêu: giảm tải cho bộ phận tiếp nhận với các câu hỏi lặp lại.
- Input: câu hỏi về lịch khám, thủ tục, biểu mẫu, quy trình tiếp nhận.
- Output: câu trả lời hành chính, bước tiếp theo, biểu mẫu liên quan.
- Điều kiện an toàn: không trả lời thay cho quyết định y khoa.

### UC-04: Phân quyền và kiểm soát truy cập
- Mục tiêu: bảo đảm user chỉ truy cập được dữ liệu đúng vai trò và khoa/phòng.
- Input: thông tin đăng nhập, vai trò, khoa/phòng.
- Output: quyền truy cập phù hợp vào từng route và từng nguồn dữ liệu.
- Điều kiện an toàn: từ chối mọi truy vấn vượt quyền.

### UC-05: Fallback và human review
- Mục tiêu: giảm rủi ro khi model lỗi, timeout, hoặc confidence thấp.
- Input: tín hiệu timeout, lỗi provider, thiếu bằng chứng, confidence thấp.
- Output: fallback sang route khác hoặc chuyển reviewer.
- Điều kiện an toàn: không được tự động trả lời bừa ở truy vấn nhạy cảm.

## Use case đưa sang Phase 2

- Chatbot cho bệnh nhân hoặc người nhà bệnh nhân
- Ghi ngược dữ liệu vào HIS/EMR
- Gợi ý điều trị hoặc hỗ trợ quyết định chuyên môn
- Dashboard phân tích quản trị nâng cao
- Tích hợp realtime sâu với nhiều hệ thống cũ
