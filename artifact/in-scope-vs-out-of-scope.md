# In-Scope vs Out-of-Scope

## In-scope của MVP

- Trợ lý AI nội bộ cho bác sĩ, điều dưỡng, nhân viên tiếp nhận
- Tóm tắt hồ sơ bệnh án theo mẫu chuẩn
- Hỏi đáp quy trình nội bộ có trích nguồn
- Hỏi đáp hành chính nội bộ
- Phân quyền theo vai trò và khoa/phòng
- Logging, audit trail cơ bản
- Fallback model và human review cho case rủi ro
- Tích hợp HIS/EMR ở chế độ `read-only`

## Out-of-scope của MVP

- Chẩn đoán bệnh
- Khuyến nghị điều trị
- Kê đơn, ra y lệnh, chỉnh liều
- Ghi ngược hoặc cập nhật trực tiếp vào HIS/EMR
- Phục vụ bệnh nhân hoặc người dùng bên ngoài bệnh viện
- Public chatbot
- Phân tích nâng cao, BI dashboard đầy đủ cho quản trị
- Tích hợp sâu realtime hai chiều với nhiều hệ thống legacy

## Nguyên tắc từ chối request ngoài scope

Request phải bị từ chối nếu:
- Yêu cầu tư vấn chuyên môn lâm sàng
- Yêu cầu truy cập dữ liệu ngoài quyền được cấp
- Yêu cầu ghi ngược hoặc chỉnh sửa HIS/EMR
- Không có nguồn nội bộ đủ tin cậy để trả lời
- Confidence thấp nhưng không đủ điều kiện tự động trả lời
