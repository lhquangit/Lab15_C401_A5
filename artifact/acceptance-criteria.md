# Acceptance Criteria - MVP

## AC-01: Tóm tắt hồ sơ bệnh án

- User là bác sĩ đã đăng nhập và có quyền truy cập hồ sơ.
- Hệ thống chỉ đọc dữ liệu, không ghi ngược.
- Hệ thống trả về bản tóm tắt theo template chuẩn.
- Bản tóm tắt có chỉ dấu nguồn từ dữ liệu chính.
- Nếu thiếu dữ liệu hoặc confidence thấp, hệ thống chuyển human review hoặc thông báo không đủ chắc chắn.

## AC-02: Hỏi đáp SOP/quy trình nội bộ

- User gửi câu hỏi liên quan tới SOP, quy trình, biểu mẫu, chính sách nội bộ.
- Hệ thống truy xuất đúng tài liệu liên quan.
- Câu trả lời phải kèm trích nguồn hoặc tên tài liệu.
- Nếu không có bằng chứng nội bộ đủ mạnh, hệ thống từ chối trả lời hoặc yêu cầu xác nhận thủ công.

## AC-03: Hỏi đáp hành chính

- User hỏi về lịch khám, thủ tục, biểu mẫu, quy định tiếp nhận.
- Hệ thống trả lời nhất quán theo chính sách nội bộ.
- Các câu hỏi lặp lại có thể được cache hoặc route theo logic tối ưu chi phí.
- Nếu câu hỏi nằm ngoài policy hoặc quá mơ hồ, hệ thống hướng dẫn chuyển tới bộ phận phù hợp.

## AC-04: Phân quyền và bảo mật truy cập

- Mọi user phải đăng nhập trước khi sử dụng.
- Quyền truy cập được kiểm soát theo vai trò và khoa/phòng.
- User không thể truy cập dữ liệu ngoài quyền của mình.
- Hệ thống ghi log truy cập và tương tác cơ bản theo policy.

## AC-05: Fallback, reliability và human review

- Khi provider timeout, hệ thống kích hoạt fallback theo policy.
- Khi confidence thấp hoặc rủi ro cao, hệ thống không tự động trả lời mà chuyển human review.
- Khi fallback cũng không khả dụng, hệ thống trả về trạng thái an toàn, không bịa nội dung.
- Các tình huống lỗi được ghi log và phục vụ giám sát reliability.

## Điều kiện chấp nhận tổng thể để go-live

- 5 use case bắt buộc của MVP đều hoạt động đúng phạm vi.
- Không có route nào vi phạm nguyên tắc `read-only`.
- Không có output nào rơi vào nhóm chẩn đoán, điều trị, kê đơn, y lệnh.
- Có phân quyền, audit cơ bản, fallback và human review.
- Có thể chứng minh scope MVP rõ ràng, không lẫn với Phase 2.
