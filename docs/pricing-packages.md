# Pricing Packages - Hospital Clinical Support Assistant

## Pricing principle

- Mô hình giá: `platform fee + included usage + overage`
- Đơn vị mua chính: bệnh viện
- Mục tiêu: giá dễ ký pilot nhưng vẫn giữ gross margin dương khi usage tăng

## Package table

| Package | Target customer | Included usage | Seats | Included features | Overage | Monthly price (VND) |
|---|---|---|---:|---|---|---:|
| Starter | Bệnh viện tư cỡ vừa, phòng khám lớn | 60,000 requests/month | 120 | 3 use case MVP, RBAC cơ bản, audit log cơ bản | 2,500 VND/request | 135,000,000 |
| Growth | Bệnh viện cỡ vừa rollout nhiều khoa | 120,000 requests/month | 300 | Toàn bộ Starter + dashboard admin + cost reporting + support giờ hành chính | 2,200 VND/request | 230,000,000 |
| Enterprise | Bệnh viện lớn, chuỗi y tế | 300,000 requests/month | Flexible | Toàn bộ Growth + SLA, private connectivity, policy tùy biến, human review workflow nâng cao | Theo volume commit | 480,000,000 |

## One-time implementation fee

- Khuyến nghị: `420,000,000 VND` cho giai đoạn triển khai MVP
- Bao gồm:
  - Scope lock và RBAC setup
  - Ingestion tài liệu, chuẩn hóa nguồn
  - Tích hợp read-only với HIS/EMR hoặc connector nội bộ
  - Prompt/routing baseline
  - Pilot training và go-live support

## Add-ons

- Human review bổ sung theo block giờ
- Audit/export log dài hạn
- Dashboard nâng cao theo khoa/phòng
- Tích hợp bổ sung với hệ thống nội bộ khác

## Package recommendation

- Dùng `Starter` cho pilot trả phí 1-2 khoa
- Dùng `Growth` khi rollout nhiều khoa trong cùng bệnh viện
- Dùng `Enterprise` cho bệnh viện lớn hoặc chuỗi y tế có yêu cầu compliance/SLA cao
