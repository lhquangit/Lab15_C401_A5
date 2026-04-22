# Product Spec - Hospital Clinical Support Assistant (Phase 1)

## 1) Mục tiêu tài liệu

Tài liệu này chuyển hóa yêu cầu từ `docs/instruction.md` và `docs/scope.md` thành spec cụ thể để:
- Làm chuẩn chung cho team khi hoàn thành Worksheet 0-5
- Chốt phạm vi MVP rõ ràng cho proposal/slide
- Xác định các hạng mục bắt buộc phải estimate (cost, reliability, vận hành)

---

## 2) Bối cảnh & bài toán

### 2.1 Bối cảnh
Bệnh viện cần một trợ lý AI nội bộ để:
- Tóm tắt hồ sơ bệnh án
- Hướng dẫn quy trình nội bộ
- Trả lời câu hỏi hành chính

### 2.2 Người dùng chính
- Bác sĩ
- Điều dưỡng
- Nhân viên tiếp nhận

### 2.3 Vấn đề hiện tại
- Mất thời gian tra cứu quy trình và biểu mẫu
- Tóm tắt hồ sơ thủ công, không nhất quán
- Câu hỏi hành chính lặp lại nhiều, gây quá tải cho bộ phận tiếp nhận

### 2.4 Mục tiêu MVP (Phase 1)
- Giảm thời gian tra cứu/tóm tắt cho user nội bộ
- Tăng tính nhất quán trong câu trả lời hành chính
- Đảm bảo an toàn dữ liệu và có cơ chế fallback khi AI không chắc chắn

---

## 3) Phạm vi sản phẩm

### 3.1 In-scope (MVP)
- Chat assistant nội bộ theo vai trò (doctor/nurse/receptionist)
- 3 nhóm tác vụ:
  - Tóm tắt hồ sơ bệnh án theo mẫu chuẩn
  - Hỏi đáp quy trình nội bộ
  - Hỏi đáp hành chính (lịch khám, biểu mẫu, quy định tiếp nhận)
- Logging và audit cơ bản
- Cơ chế human-in-the-loop cho câu trả lời có rủi ro cao

### 3.2 Out-of-scope (Phase 1)
- Tự động chẩn đoán hoặc ra y lệnh
- Tích hợp sâu toàn bộ HIS/EMR realtime 2 chiều
- Hỗ trợ bệnh nhân trực tiếp từ internet/public app

### 3.3 Ranh giới nghiệp vụ an toàn
- AI chỉ hỗ trợ tác vụ hành chính và tóm tắt thông tin có kiểm tra
- Không cho phép AI đưa khuyến nghị điều trị trực tiếp
- Mọi nội dung có thể ảnh hưởng lâm sàng phải có bước xác nhận của con người

---

## 4) Yêu cầu chức năng (Functional Requirements)

### FR-01: Đăng nhập và phân quyền
- Người dùng đăng nhập tài khoản nội bộ
- Phân quyền theo vai trò và khoa/phòng
- Chỉ truy cập dữ liệu được phép

### FR-02: Tóm tắt hồ sơ bệnh án
- Input: mã hồ sơ hoặc nội dung hồ sơ đã cấp quyền
- Output: tóm tắt theo template (lịch sử, tình trạng hiện tại, ghi chú)
- Có chỉ dấu nguồn (trích mục dữ liệu chính đã dùng)

### FR-03: Hỏi đáp quy trình nội bộ
- Trả lời câu hỏi dựa trên tài liệu quy trình/biểu mẫu nội bộ
- Ưu tiên câu trả lời kèm link/tên tài liệu nguồn

### FR-04: Hỏi đáp hành chính
- Trả lời các câu hỏi lặp lại (đăng ký khám, thời gian, thủ tục)
- Với câu hỏi ngoài chính sách, trả về hướng dẫn chuyển tuyến nhân sự phù hợp

### FR-05: Cơ chế kiểm duyệt người dùng (Human approval)
- Với truy vấn rủi ro cao hoặc độ tin cậy thấp:
  - Không trả lời trực tiếp
  - Chuyển trạng thái "Cần xác nhận"
  - Gửi đến người có thẩm quyền duyệt

### FR-06: Audit & truy vết
- Lưu lịch sử prompt/response đã ẩn dữ liệu nhạy cảm theo chính sách
- Ghi nhận user, thời gian, tác vụ, model, trạng thái fallback

---

## 5) Yêu cầu phi chức năng (Non-functional Requirements)

### NFR-01: Bảo mật & quyền riêng tư
- Dữ liệu sức khỏe được mã hóa khi truyền và khi lưu
- Tuân thủ chính sách dữ liệu nội bộ bệnh viện
- Có cơ chế ẩn/giảm lộ PII trong log

### NFR-02: Độ chính xác vận hành
- Câu trả lời phải có nguồn nội bộ khi thuộc nhóm quy trình/chính sách
- Thiết lập ngưỡng confidence để kích hoạt human review

### NFR-03: Hiệu năng
- P95 latency mục tiêu cho truy vấn thường: <= 8 giây
- P99 cho truy vấn phức tạp: <= 15 giây

### NFR-04: Sẵn sàng hệ thống
- Mục tiêu uptime MVP: >= 99.5%
- Có fallback khi model provider timeout/lỗi

### NFR-05: Khả năng mở rộng
- Thiết kế chịu tải tăng 5x-10x so với baseline không đổi kiến trúc lõi

---

## 6) Kiến trúc triển khai đề xuất (MVP)

### 6.1 Lựa chọn
Mô hình đề xuất: `Hybrid`
- Cloud: model inference, elastic scaling, monitoring trung tâm
- On-prem/VPN nội bộ: kho dữ liệu nhạy cảm, connector HIS/EMR

### 6.2 Lý do
- Cân bằng giữa bảo mật dữ liệu y tế và tốc độ triển khai
- Giảm rủi ro lock-in toàn bộ hạ tầng trong giai đoạn MVP

### 6.3 Luồng xử lý mức cao
1. User xác thực và gửi truy vấn
2. Service kiểm tra quyền truy cập dữ liệu
3. Retrieval tài liệu/hồ sơ được phép
4. Gọi model tạo câu trả lời/tóm tắt
5. Chấm rủi ro-confidence
6. Trả kết quả hoặc chuyển human review
7. Ghi log/audit/metric

---

## 7) Reliability & Fallback Plan

### 7.1 Tình huống rủi ro chính
- Traffic spike theo khung giờ khám
- Provider timeout hoặc lỗi API
- Response chậm vượt SLA
- Hallucination ở câu trả lời quy trình/chính sách

### 7.2 Phương án ngắn hạn
- Rate limit theo user/role
- Queue + retry có backoff
- Timeout guard + fallback model nhẹ hơn
- Trả về "Không đủ chắc chắn, cần xác nhận thủ công"

### 7.3 Phương án dài hạn
- Multi-provider routing
- Cache câu hỏi hành chính phổ biến
- RAG quality gate (trả lời chỉ khi có evidence đạt ngưỡng)
- Dashboard theo dõi độ chính xác vận hành theo khoa/phòng

### 7.4 Metric theo dõi bắt buộc
- Request/min, error rate, timeout rate
- P50/P95/P99 latency
- Tỷ lệ fallback
- Tỷ lệ human review
- Tỷ lệ phản hồi bị người dùng đánh dấu sai

---

## 8) Cost Anatomy - Khung tính cho MVP

Tổng chi phí tháng (MVP):

`Total Cost = Token/API + Compute + Storage + Logging + Human Review + Maintenance`

Trong đó:
- Token/API: chi phí input/output token theo model
- Compute: app server, queue worker, retrieval service
- Storage: vector DB + object storage + log retention
- Logging/Monitoring: hạ tầng quan sát
- Human Review: thời gian nhân sự kiểm duyệt
- Maintenance: vận hành, trực sự cố, nâng cấp

---

## 9) Danh sách phần cần estimate

### 9.1 Nhu cầu sử dụng (Worksheet 2)
- Số user hoạt động/ngày theo vai trò
- Số request/ngày
- Peak QPS theo giờ cao điểm
- Tỷ lệ câu hỏi hành chính vs quy trình vs tóm tắt hồ sơ

### 9.2 Token & model usage (Worksheet 2)
- Input token trung bình/request theo từng use case
- Output token trung bình/request theo từng use case
- Số lượt gọi model bổ sung (rerank, judge, fallback)
- Tỷ lệ fallback sang model thứ cấp

### 9.3 Hạ tầng (Worksheet 2)
- CPU/RAM/service cho app + retrieval + queue
- Dung lượng lưu trữ hồ sơ/tài liệu/index
- Tăng trưởng dữ liệu theo tháng
- Băng thông nội bộ và cloud egress

### 9.4 Chi phí con người (Worksheet 2, 4)
- Tỷ lệ case cần human review
- Thời gian review trung bình/case
- Chi phí nhân sự theo giờ
- Chi phí trực sự cố/on-call

### 9.5 Reliability (Worksheet 4)
- SLO latency/error/availability
- MTTD/MTTR mục tiêu
- Error budget/tháng
- Tải cực đại giả định khi spike (5x, 10x)

### 9.6 Bảo mật & tuân thủ
- Chi phí audit log retention
- Chi phí mã hóa, key management, access control
- Chi phí kiểm tra bảo mật định kỳ

### 9.7 Năng lực team & roadmap (Worksheet 5)
- Mức sẵn sàng từng vai trò (Product, Infra/Ops, AI Eng)
- Phần năng lực thiếu ảnh hưởng trực tiếp đến timeline
- Chi phí/khối lượng đào tạo bổ sung

---

## 10) Bảng estimate nhanh (điền số)

| Hạng mục | Biến số cần điền | Đơn vị | Owner | Mức tự tin |
|---|---|---:|---|---|
| User traffic | DAU, requests/day, peak QPS | user, req, qps | Product Lead | M/L/H |
| Token cost | avg input/output token, model mix | token, % | Cost Lead | M/L/H |
| Compute | app/retrieval/queue sizing | vCPU, GB RAM | Architect | M/L/H |
| Storage | hồ sơ, index, log retention | GB/TB | Architect | M/L/H |
| Human review | review rate, min/case, cost/hour | %, phút, tiền | Reliability Lead | M/L/H |
| Observability | log, metric, alerting | tiền/tháng | Reliability Lead | M/L/H |
| Security/compliance | key mgmt, audit, access policy | tiền/tháng | Architect | M/L/H |
| Incident handling | on-call effort, MTTR target | giờ, phút | Reliability Lead | M/L/H |

---

## 11) Chiến lược tối ưu chi phí đề xuất (Worksheet 3)

### Làm ngay (Now)
1. Cache các truy vấn hành chính lặp lại cao
2. Dùng model tiering (query đơn giản dùng model rẻ hơn)
3. Prompt/routing chuẩn hóa để giảm token thừa

### Làm sau (Later)
1. Multi-provider để tối ưu giá theo thời điểm
2. Fine-tune hoặc adapter cho tác vụ tóm tắt lặp lại
3. Tối ưu retrieval pipeline theo domain từng khoa

---

## 12) Deliverables mapping theo checklist cuối ngày

- Worksheet 0: Bài toán + user + lý do chọn scenario
- Worksheet 1: Bối cảnh enterprise + deployment hybrid + lý do
- Worksheet 2: Cost anatomy + baseline MVP + scale 5x/10x
- Worksheet 3: 3 chiến lược optimize + trade-off
- Worksheet 4: Reliability scenario + fallback + monitor metrics
- Worksheet 5: Năng lực team + Track Phase 2 + skill gap
- Proposal slide/poster: tổng hợp từ mục 2 -> 11 của tài liệu này

---

## 13) Open questions cần chốt trước khi khóa estimate

- Phạm vi AI ở mức hành chính hay bao gồm hỗ trợ chuyên môn có kiểm duyệt?
- Mức tích hợp HIS/EMR hiện tại là read-only hay có write-back?
- Quy định nội bộ về lưu log prompt/response cho dữ liệu y tế là bao lâu?
- Ngưỡng nào bắt buộc human review theo loại truy vấn?
