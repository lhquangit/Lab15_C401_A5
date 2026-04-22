# Mini Project Proposal - MVP In-scope

## 1) Thông tin dự án

- Tên dự án: `Hospital Clinical Support Assistant`
- Scenario: Hospital Clinical Support Assistant (Scenario 2)
- Mục tiêu: Tối ưu vận hành nội bộ bệnh viện bằng trợ lý AI an toàn và có kiểm soát
- Phạm vi: Chỉ triển khai In-scope MVP (không chẩn đoán, không ra y lệnh)

---

## 2) Problem Statement

### Thực trạng
- Tra cứu quy trình và biểu mẫu mất thời gian, dễ sai khác giữa ca trực.
- Tóm tắt hồ sơ bệnh án thủ công, thiếu nhất quán.
- Bộ phận tiếp nhận xử lý lượng lớn câu hỏi hành chính lặp lại.

### Tác động
- Tăng tải nhân sự tuyến đầu.
- Chậm luồng xử lý nội bộ.
- Khó chuẩn hóa chất lượng phản hồi và khó đo hiệu quả vận hành.

---

## 3) Solution Overview (MVP)

### 3.1 In-scope use cases
1. Tóm tắt hồ sơ bệnh án theo template chuẩn.
2. Hỏi đáp quy trình nội bộ dựa trên tài liệu chính thức.
3. Hỏi đáp hành chính (lịch khám, thủ tục, biểu mẫu).

### 3.2 Safety by design
- Role-based access control theo khoa/phòng.
- Response có trích nguồn cho nhóm câu hỏi chính sách.
- Confidence gate + human approval cho case rủi ro cao.
- Audit log cho toàn bộ phiên tương tác.

### 3.3 Out-of-scope
- Tư vấn chẩn đoán/điều trị.
- Viết ngược dữ liệu vào HIS/EMR ở MVP.
- Public chatbot cho bệnh nhân ngoài hệ thống nội bộ.

---

## 4) Enterprise Context & Architecture Choice

### Ràng buộc enterprise
- Dữ liệu sức khỏe nhạy cảm.
- Bắt buộc phân quyền theo vai trò.
- Sai sót AI có thể ảnh hưởng vận hành.
- Phụ thuộc vào hệ thống HIS/EMR cũ.

### Lựa chọn triển khai
`Hybrid deployment`
- On-prem/VPN: dữ liệu hồ sơ, connector HIS/EMR.
- Cloud: model inference, autoscaling, monitoring tập trung.

Lý do:
- Cân bằng bảo mật và tốc độ triển khai.
- Dễ mở rộng sau pilot mà vẫn giữ kiểm soát dữ liệu nhạy cảm.

---

## 5) Cost Anatomy & Financial Case

### 5.1 Baseline usage assumptions
- 220 DAU nội bộ
- 2,640 requests/ngày
- 1,450 token/request trung bình
- 6% case cần human review

### 5.2 Baseline monthly cost

| Hạng mục | Chi phí/tháng (VND) |
|---|---:|
| Token/API | 36,000,000 |
| Compute | 18,000,000 |
| Storage + backup | 7,000,000 |
| Observability | 6,000,000 |
| Human review | 9,000,000 |
| Maintenance & on-call | 14,000,000 |
| **Tổng OPEX** | **90,000,000** |

### 5.3 Scale forecast
- 5x traffic: ~360,000,000 VND/tháng
- 10x traffic: ~660,000,000 VND/tháng

### 5.4 ROI (sơ bộ)
- Giá trị tiết kiệm thời gian vận hành: ~290,400,000 VND/tháng
- Net benefit sau OPEX: ~200,400,000 VND/tháng
- Payback ước tính: ~2.1 tháng sau go-live ổn định

---

## 6) Reliability & Risk Management

### 6.1 Rủi ro chính
- Traffic spike theo giờ cao điểm
- Provider timeout
- Response chậm
- Hallucination với câu hỏi chính sách/quy trình

### 6.2 Kế hoạch kiểm soát
- Rate limit theo user/role
- Queue + retry + timeout guard
- Fallback model cho truy vấn ít rủi ro
- Evidence requirement + human approval khi confidence thấp

### 6.3 KPI giám sát
- Uptime >= 99.5%
- P95 latency <= 8s, P99 <= 15s
- Timeout rate < 2%
- Tỷ lệ phản hồi bị đánh dấu sai < 5%

---

## 7) Implementation Plan & Timeline

| Giai đoạn | Thời gian | Mục tiêu |
|---|---|---|
| Phase 0 - Prep | 2 tuần | Policy dữ liệu, phân quyền, chuẩn hóa tài liệu |
| Phase 1 - Pilot | 6 tuần | Pilot 2 khoa + tiếp nhận, đo KPI thật |
| Phase 2 - Rollout | 6-8 tuần | Mở rộng toàn viện theo cụm khoa |

Tổng thời gian MVP đến rollout diện rộng: 14-16 tuần.

---

## 8) Team & Governance

### Core team
- Product Lead
- Architect
- AI Engineer
- Backend/Platform Engineer
- Cost Lead
- Reliability Lead
- Clinical Advisor

### Governance
- Weekly steering meeting (BGĐ + IT + nghiệp vụ)
- Báo cáo KPI tuần trong pilot
- Review rủi ro và incident sau mỗi đợt rollout

---

## 9) Deliverables cam kết

1. Worksheet 0-5 hoàn chỉnh theo checklist.
2. Tài liệu spec MVP.
3. Business plan + ROI model baseline/5x/10x.
4. Reliability plan + fallback runbook bản MVP.
5. Slide trình bày 7 trang + script Q&A.

---

## 10) Decision Request

Đề nghị phê duyệt:
1. Phạm vi MVP In-scope như mục 3.
2. Ngân sách one-time triển khai: 420,000,000 VND.
3. OPEX baseline: 90,000,000 VND/tháng.
4. Chính sách bắt buộc human review cho truy vấn rủi ro cao.

---

## 11) Slide-ready Outline (7 slides)

1. Project + User + Problem  
2. Enterprise context + constraints  
3. Architecture (Hybrid) + data safety boundaries  
4. Cost anatomy + baseline estimate  
5. Cost optimization (Now/Later)  
6. Reliability + fallback + KPI  
7. Track Phase 2 + next steps + approval ask

---

## 12) 5-minute Pitch Script (ngắn)

- Phút 1: Vấn đề vận hành và tác động thực tế.
- Phút 2: Giải pháp MVP trong phạm vi an toàn.
- Phút 3: Kiến trúc hybrid và cách bảo vệ dữ liệu.
- Phút 4: Cost + ROI + vì sao khả thi.
- Phút 5: Timeline, rủi ro chính, đề nghị phê duyệt.

---

## 13) Q&A gợi ý

### Nếu hỏi "AI có thể sai, sao dám dùng?"
Trả lời: MVP chỉ hỗ trợ hành chính/quy trình/tóm tắt có nguồn; case rủi ro cao phải human review, không dùng để chẩn đoán.

### Nếu hỏi "Chi phí có thể đội lên không?"
Trả lời: Có, nên đã thiết kế cache + model tiering + cảnh báo cost theo tuần; pilot sẽ thay estimate bằng số thực.

### Nếu hỏi "Tại sao không on-prem 100%?"
Trả lời: Hybrid giúp đi nhanh, giảm đầu tư ban đầu, vẫn giữ dữ liệu nhạy cảm trong vùng kiểm soát nội bộ.
