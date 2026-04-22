# Business Plan - Hospital Clinical Support Assistant (MVP In-scope)

## 1) Executive Summary

Mục tiêu của dự án là triển khai trợ lý AI nội bộ cho bệnh viện nhằm giải quyết 3 tác vụ trong phạm vi MVP:

- Tóm tắt hồ sơ bệnh án
- Hỏi đáp quy trình nội bộ
- Hỏi đáp hành chính

Kết quả kỳ vọng sau 6 tháng:

- Giảm 25-35% thời gian tra cứu và tổng hợp thông tin nội bộ
- Chuẩn hóa câu trả lời hành chính, giảm sai lệch vận hành
- Thiết lập nền tảng AI an toàn (phân quyền, audit, human review) để mở rộng Phase 2

---

## 2) Bài toán kinh doanh

### 2.1 Pain points hiện tại

- Nhân sự y tế mất nhiều thời gian cho tác vụ lặp lại, giảm thời gian cho công việc chuyên môn.
- Thông tin quy trình phân tán, câu trả lời hành chính thiếu nhất quán giữa các ca trực.
- Tóm tắt hồ sơ thủ công tốn thời gian, dễ thiếu sót.

### 2.2 Chi phí cơ hội nếu không làm

- Tăng tải vận hành giờ cao điểm khám bệnh.
- Trải nghiệm nội bộ giảm, áp lực nhân sự tuyến đầu tăng.
- Chậm chuẩn hóa dữ liệu/quy trình cho chuyển đổi số.

---

## 3) Đề xuất giải pháp MVP (In-scope)

### 3.1 Sản phẩm

`Hospital Clinical Support Assistant` cho user nội bộ:

- Doctor/Nurse/Receptionist có phân quyền theo vai trò
- Chat truy vấn tài liệu nội bộ và hồ sơ được cấp quyền
- Cơ chế đánh dấu rủi ro và chuyển human approval

### 3.2 Ranh giới an toàn

- Không chẩn đoán, không ra y lệnh, không thay thế quyết định lâm sàng.
- Câu trả lời rủi ro cao hoặc confidence thấp bắt buộc xác nhận thủ công.

### 3.3 Triển khai kỹ thuật

- Mô hình `Hybrid`: dữ liệu nhạy cảm ở on-prem/VPN, inference/scale ở cloud.
- Có logging, audit trail và dashboard vận hành tối thiểu.

---

## 4) Stakeholder & Giá trị mang lại


| Stakeholder         | Vấn đề chính                    | Giá trị nhận được                                        |
| ------------------- | ------------------------------- | -------------------------------------------------------- |
| Bác sĩ              | Tốn thời gian tổng hợp hồ sơ    | Rút ngắn thời gian chuẩn bị thông tin, tăng tốc xử lý ca |
| Điều dưỡng          | Tra cứu quy trình mất thời gian | Có hướng dẫn nhanh, nhất quán theo chính sách bệnh viện  |
| Nhân viên tiếp nhận | Câu hỏi hành chính lặp lại      | Giảm tải trả lời thủ công giờ cao điểm                   |
| Ban giám đốc        | Khó đo hiệu quả số hóa          | Có KPI rõ (latency, accuracy ops, time saved, ROI)       |
| IT/Compliance       | Lo ngại bảo mật và rủi ro AI    | Có phân quyền, log, fallback, human-in-the-loop          |


---

## 5) Giả định kinh doanh & vận hành cho MVP

### 5.1 Quy mô sử dụng baseline (1 bệnh viện cỡ trung)

- Tổng user mục tiêu: 400
- Tỷ lệ active/ngày: 55% -> 220 DAU
- Request/người active/ngày: 12
- Tổng request/ngày: 2,640
- Phân bổ use case:
  - Hành chính: 45%
  - Quy trình nội bộ: 35%
  - Tóm tắt hồ sơ: 20%

### 5.2 Giả định token trung bình/request

- Input: 1,100 token
- Output: 350 token
- Tổng: 1,450 token/request

### 5.3 Giả định xử lý rủi ro

- Tỷ lệ fallback model: 12%
- Tỷ lệ case cần human review: 6%
- Thời gian review/case: 3 phút

---

## 6) Mô hình chi phí (OPEX tháng)

### 6.1 Baseline monthly cost (ước lượng)


| Nhóm chi phí                      | Chi phí/tháng (VND) | Ghi chú                                |
| --------------------------------- | ------------------- | -------------------------------------- |
| Token/API                         | 36,000,000          | Tính theo mức dùng baseline + fallback |
| Compute (app + retrieval + queue) | 18,000,000          | Cloud compute + worker                 |
| Storage + backup                  | 7,000,000           | Vector index, tài liệu, log            |
| Observability (log/metric/alert)  | 6,000,000           | Monitoring và retention                |
| Human review                      | 9,000,000           | 6% case, 3 phút/case                   |
| Maintenance & on-call             | 14,000,000          | Vận hành + xử lý sự cố                 |
| **Tổng OPEX/tháng**               | **90,000,000**      |                                        |


### 6.2 Scale projection


| Mức tải  | Hệ số request | OPEX ước lượng/tháng (VND) | Driver chính                   |
| -------- | ------------- | -------------------------- | ------------------------------ |
| Baseline | 1x            | 90,000,000                 | Token/API                      |
| Growth   | 5x            | 360,000,000                | Token + human review + compute |
| Peak     | 10x           | 660,000,000                | Token + hạ tầng dự phòng       |


Ghi chú: scale không tăng tuyến tính hoàn toàn nhờ cache, model tiering, và tối ưu routing.

---

## 7) Lợi ích định lượng & ROI

### 7.1 Lợi ích năng suất (baseline)

- Thời gian tiết kiệm trung bình/request: 2.5 phút
- Tổng thời gian tiết kiệm/ngày: `2,640 x 2.5 = 6,600 phút` (~110 giờ/ngày)
- Với 22 ngày làm việc/tháng: ~2,420 giờ/tháng
- Chi phí lao động hiệu dụng (blended): 120,000 VND/giờ
- Giá trị tiết kiệm tương đương: `2,420 x 120,000 = 290,400,000 VND/tháng`

### 7.2 ROI sơ bộ

- Lợi ích định lượng/tháng: ~290,400,000 VND
- OPEX/tháng: 90,000,000 VND
- Net benefit/tháng: ~200,400,000 VND
- Chi phí triển khai ban đầu (one-time, 3 tháng): 420,000,000 VND
- Payback period dự kiến: ~2.1 tháng sau go-live ổn định

---

## 8) Go-to-Operation Plan (không phải GTM bán ra ngoài)

### Phase 0 - Chuẩn bị (2 tuần)

- Chốt policy dữ liệu, phân quyền vai trò
- Chuẩn hóa tài liệu quy trình ưu tiên cao
- Thiết kế quy trình human review và escalation

### Phase 1 - Pilot (6 tuần)

- Triển khai cho 2 khoa + bộ phận tiếp nhận
- Theo dõi KPI hằng tuần, tinh chỉnh prompt/routing
- Chốt baseline chi phí thật thay cho estimate

### Phase 2 - Rollout nội bộ (6-8 tuần)

- Mở rộng toàn bệnh viện theo cụm khoa
- Áp dụng caching và model tiering để tối ưu chi phí
- Chuẩn hóa dashboard cho Ban giám đốc và IT Ops

---

## 9) Operating Model & Nhân sự


| Vai trò          | Trách nhiệm chính              | FTE gợi ý (MVP) |
| ---------------- | ------------------------------ | --------------- |
| Product Lead     | use case, KPI, adoption        | 0.5             |
| Architect        | kiến trúc hybrid, bảo mật      | 0.5             |
| AI Engineer      | prompt/routing/RAG quality     | 1.0             |
| Backend/Platform | tích hợp, logging, reliability | 1.0             |
| Cost Lead/FinOps | theo dõi cost driver, tối ưu   | 0.3             |
| Reliability Lead | SLO, incident, fallback drill  | 0.3             |
| Clinical Advisor | kiểm định nghiệp vụ            | 0.3             |


---

## 10) KPI thành công

### KPI vận hành

- Uptime >= 99.5%
- P95 latency <= 8s
- Timeout rate < 2%
- Tỷ lệ fallback < 15%

### KPI chất lượng

- Tỷ lệ phản hồi có nguồn nội bộ (với câu hỏi chính sách): >= 95%
- Tỷ lệ phản hồi bị đánh dấu sai: < 5%
- Tỷ lệ case cần human review ổn định: 4-8%

### KPI kinh doanh

- Tỷ lệ user active theo tuần >= 60% nhóm mục tiêu
- Thời gian xử lý tác vụ nội bộ giảm >= 25%
- Net benefit dương trong 3 tháng sau pilot

---

## 11) Rủi ro & kế hoạch giảm thiểu


| Rủi ro                                     | Mức ảnh hưởng  | Giảm thiểu                                              |
| ------------------------------------------ | -------------- | ------------------------------------------------------- |
| Hallucination trong câu trả lời quan trọng | Cao            | Confidence gate + evidence requirement + human approval |
| Lộ dữ liệu nhạy cảm qua log                | Cao            | PII masking + retention policy + access control         |
| Chi phí token tăng nhanh khi scale         | Trung bình-Cao | Caching + model tiering + prompt budget                 |
| Kháng cự người dùng nội bộ                 | Trung bình     | Training ngắn, champion user theo khoa                  |
| Phụ thuộc hệ thống HIS/EMR cũ              | Trung bình     | Tích hợp read-only trước, rollout theo cụm              |


---

## 12) Quyết định cần phê duyệt

1. Phê duyệt phạm vi MVP đúng In-scope đã định nghĩa.
2. Phê duyệt ngân sách triển khai ban đầu: 420,000,000 VND.
3. Phê duyệt ngân sách vận hành baseline: 90,000,000 VND/tháng.
4. Phê duyệt chính sách human review và ranh giới không hỗ trợ chuyên môn lâm sàng.

---

## 13) Phụ lục giả định tài chính

- Các con số trên là mô hình estimate phục vụ proposal, chưa phải quote vendor.
- Khi pilot, cần cập nhật:
  - Giá model thực tế theo hợp đồng
  - Tải thật theo ca/khoa
  - Tỷ lệ human review thực tế
  - Chi phí hạ tầng sau tối ưu cache/tiering

