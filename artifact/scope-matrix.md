# Scope Matrix - Hospital Clinical Support Assistant (MVP)

| Use case | User chính | In scope | Out of scope | Mức rủi ro | Human review | Definition of done |
|---|---|---|---|---|---|---|
| Tóm tắt hồ sơ bệnh án | Bác sĩ | Tóm tắt hồ sơ theo mẫu chuẩn, chỉ đọc dữ liệu đã được phân quyền | Chẩn đoán, khuyến nghị điều trị, ghi ngược HIS/EMR | Cao | Có, khi confidence thấp hoặc thiếu bằng chứng | Tạo được bản tóm tắt theo template, có chỉ dấu nguồn dữ liệu chính, đúng quyền truy cập |
| Hỏi đáp SOP/quy trình nội bộ | Điều dưỡng | Trả lời dựa trên tài liệu nội bộ chính thức, kèm trích nguồn | Trả lời suy diễn không có nguồn, tư vấn lâm sàng | Vừa đến cao | Có, với câu hỏi nhạy cảm hoặc thiếu bằng chứng | Trả lời đúng tài liệu, có tên tài liệu hoặc nguồn tham chiếu, nhất quán giữa các ca trực |
| Hỏi đáp hành chính | Nhân viên tiếp nhận | Trả lời thủ tục, lịch khám, biểu mẫu, hướng dẫn tiếp theo | Tư vấn cho bệnh nhân ngoài hệ thống, cam kết chuyên môn y khoa | Thấp đến vừa | Có, nếu policy mơ hồ hoặc câu trả lời không chắc chắn | Trả lời nhanh, đúng chính sách, có thể cache cho câu hỏi lặp lại |
| Phân quyền theo vai trò/khoa/phòng | Tất cả user nội bộ | RBAC theo user, vai trò, khoa/phòng, phạm vi dữ liệu | Truy cập chéo dữ liệu ngoài quyền được cấp | Cao | Không, đây là kiểm soát hệ thống bắt buộc | User chỉ xem được dữ liệu đúng quyền của mình |
| Fallback / Human review | Tất cả route rủi ro | Chuyển fallback model hoặc chuyển reviewer khi AI không chắc chắn | Tự động trả lời khi confidence thấp ở case nhạy cảm | Cao | Bắt buộc | Khi route lỗi hoặc không chắc chắn, hệ thống không bịa câu trả lời mà fallback hoặc chuyển xác nhận thủ công |

## Ghi chú khóa scope

- Toàn bộ MVP ở chế độ `read-only` với HIS/EMR.
- Không hỗ trợ quyết định lâm sàng trong Phase 1.
- Không phục vụ bệnh nhân và người dùng bên ngoài bệnh viện.
