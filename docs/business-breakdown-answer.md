# Phần 4 - Kinh doanh

## 4.1 Quyết định kinh doanh chính

1. Sản phẩm bán cho ai?
   - Bán B2B cho bệnh viện và chuỗi y tế, không bán trực tiếp cho từng cá nhân.
2. Đơn vị mua là gì?
   - Đơn vị mua mặc định là bệnh viện. Với pilot nhỏ có thể ký theo khoa/phòng, nhưng hợp đồng chuẩn vẫn nên đi qua bệnh viện.
3. Giá tính theo gì?
   - Dùng mô hình hybrid: platform fee + seat/role bundle + usage cap theo request.
4. Có bao nhiêu gói?
   - 3 gói: `Starter`, `Growth`, `Enterprise`.
5. Segment nào cho từng gói?
   - `Starter`: bệnh viện tư cỡ vừa hoặc phòng khám lớn muốn pilot nhanh.
   - `Growth`: bệnh viện cỡ vừa đã có nhu cầu rollout nhiều khoa.
   - `Enterprise`: bệnh viện lớn, chuỗi y tế, hoặc tổ chức có yêu cầu cao về SLA/compliance.
6. Tính năng chính theo gói?
   - Tất cả gói đều có 3 use case MVP: record summary, SOP Q&A, admin Q&A.
   - `Growth` thêm dashboard quản trị, cost reporting, ưu tiên hỗ trợ.
   - `Enterprise` thêm SLA, private connectivity, policy tùy biến, human review workflow nâng cao.
7. Trần usage theo gói?
   - `Starter`: 60,000 request/tháng
   - `Growth`: 120,000 request/tháng
   - `Enterprise`: 300,000 request/tháng, vượt mức thì báo giá riêng
8. Overage tính thế nào?
   - `Starter`: 2,500 VND/request vượt mức
   - `Growth`: 2,200 VND/request vượt mức
   - `Enterprise`: đàm phán theo volume commit
9. Unit economics có dương không?
   - Có, nếu giữ human review ở nhóm truy vấn rủi ro cao và kiểm soát token bằng cache + model tiering.
10. Gói nào dùng để pilot, gói nào dùng để mở rộng?
   - `Starter` dùng cho pilot trả phí.
   - `Growth` dùng cho rollout bệnh viện cỡ vừa.
   - `Enterprise` dùng cho rollout diện rộng hoặc chuỗi y tế.

## 4.2 Target user và payment plan

1. Khách hàng mục tiêu đầu tiên là ai?
   - Bệnh viện tư cỡ vừa là mục tiêu đầu tiên.
2. ICP cụ thể là gì?
   - 200-500 nhân sự mục tiêu dùng nội bộ
   - Có HIS/EMR hoặc ít nhất có kho tài liệu quy trình số hóa
   - Có ngân sách đổi mới vận hành và chấp nhận pilot 3 tháng
   - Có yêu cầu dữ liệu nhạy cảm cao nhưng cho phép mô hình hybrid/on-prem connector
3. Người quyết định mua là ai?
   - Ban giám đốc, CIO/Head of IT, hoặc COO/giám đốc vận hành.
4. Người sử dụng thực tế là ai?
   - Bác sĩ, điều dưỡng, nhân viên tiếp nhận.
5. Bài toán nào khiến khách hàng willing to pay sớm nhất?
   - Giảm thời gian tra cứu quy trình, chuẩn hóa trả lời hành chính, và rút ngắn thời gian tóm tắt hồ sơ ở tuyến đầu.
6. Thanh toán theo tháng, quý hay năm?
   - Ưu tiên hợp đồng năm, thanh toán theo quý hoặc năm. Pilot có thể thanh toán theo tháng.
7. Có cần 3 gói `Starter`, `Growth`, `Enterprise` không?
   - Có.
8. Gói nào giới hạn theo seat?
   - `Starter` và `Growth` có soft cap seat.
9. Gói nào giới hạn theo request hoặc token?
   - Cả 3 gói đều giới hạn theo request/tháng.
10. Gói nào bao gồm human review, SLA, support priority?
   - `Growth` có hỗ trợ giờ hành chính và SLA cơ bản.
   - `Enterprise` có support priority, SLA chặt hơn, và workflow human review tùy biến.
11. Tính năng nào là add-on có thể bán riêng?
   - Setup tích hợp HIS/EMR
   - Human review bổ sung theo block giờ
   - Dashboard nâng cao
   - Audit/export log dài hạn
12. Có cần phí setup/implementation một lần không?
   - Có. Nên thu phí implementation một lần để cover cấu hình RBAC, ingestion tài liệu, tuning prompt và tích hợp read-only.

## 4.3 Lỗ lãi và đơn vị kinh tế

1. Cost phục vụ 1 khách hàng/tháng là bao nhiêu?
   - Khoảng `90,000,000 VND/tháng` ở mức 1 bệnh viện baseline.
2. Cost biến đổi lớn nhất là model hay human review?
   - Token/model là cost biến đổi lớn nhất; human review đứng thứ hai.
3. Gross margin theo từng gói là bao nhiêu?
   - `Starter`: khoảng 25-30%
   - `Growth`: khoảng 35-40%
   - `Enterprise`: khoảng 40-45%
4. Gói nào có nguy cơ lỗ nhất nếu user dùng sát trần?
   - `Starter`.
5. Mức usage nào thì cần tăng giá?
   - Khi usage vượt 80-85% trần trong 2 tháng liên tiếp hoặc tỷ lệ human review vượt 8%.
6. Mức usage nào thì cần buộc fallback sang model rẻ hơn?
   - Khi request thuộc nhóm hành chính lặp lại cao, hoặc khi cost/request vượt ngân sách route đã phê duyệt.
7. Human review có được tính riêng thành phí phụ trội không?
   - Gói chuẩn chỉ bao gồm mức human review giới hạn; vượt ngưỡng thì tính add-on hoặc block giờ.
8. Pilot trả phí hay free?
   - Nên là pilot trả phí thấp.
9. Nếu free pilot, điều kiện để chuyển sang paid là gì?
   - Có ít nhất 2 khoa active, KPI tiết kiệm thời gian đạt >= 20%, và sign-off tiếp tục rollout.
10. Break-even point theo số khách hàng là bao nhiêu?
   - Nếu tính chi phí triển khai một lần `420,000,000 VND` và gross profit baseline khoảng `30,000,000 VND/tháng/khách hàng`, break-even xấp xỉ sau 14 tháng với 1 khách hàng, hoặc nhanh hơn nếu có 2-3 khách hàng song song.
11. Nếu chỉ có 1 bệnh viện đầu tiên, dự án đang lỗ hay lãi?
   - Ở mức vận hành tháng thì lãi gộp dương nếu bán đúng giá gói. Nếu cộng cả chi phí triển khai ban đầu thì chưa hoàn vốn ngay.
12. Nếu scale lên 5 bệnh viện và 10 bệnh viện, gross profit là bao nhiêu?
   - 5 bệnh viện: gross profit tháng ước tính khoảng `150,000,000-190,000,000 VND`
   - 10 bệnh viện: gross profit tháng ước tính khoảng `320,000,000-400,000,000 VND`
13. Hidden cost lớn nhất là gì?
   - Làm sạch tài liệu nội bộ, governance cho log/PII, và thời gian phối hợp tích hợp HIS/EMR.
14. Ước lượng hiện tại có quá lạc quan không, và giả định nào dễ sai nhất?
   - Giả định dễ sai nhất là tỷ lệ human review thực tế và mức request/người/ngày.
15. Tổng cost MVP ở mức baseline là bao nhiêu?
   - One-time implementation khoảng `420,000,000 VND`
   - OPEX baseline khoảng `90,000,000 VND/tháng`

## 4.4 Bảng pricing đề xuất

| Gói | Target customer | Included usage | Overage | Giá/tháng | Est. COGS | Gross margin | Phù hợp với ai |
|---|---|---|---|---:|---:|---:|---|
| Starter | Bệnh viện tư cỡ vừa, phòng khám lớn | 60,000 request/tháng, tối đa 120 seat, 3 use case MVP | 2,500 VND/request | 135,000,000 | 96,000,000 | 28.9% | Đơn vị muốn pilot 1-2 khoa và kiểm chứng ROI nhanh |
| Growth | Bệnh viện cỡ vừa rollout nhiều khoa | 120,000 request/tháng, tối đa 300 seat, dashboard quản trị | 2,200 VND/request | 230,000,000 | 145,000,000 | 37.0% | Đơn vị đã có nhu cầu mở rộng toàn viện |
| Enterprise | Bệnh viện lớn, chuỗi y tế | 300,000 request/tháng, seat linh hoạt, SLA + private connectivity | Báo giá theo commit | 480,000,000 | 270,000,000 | 43.8% | Tổ chức cần compliance, SLA và scale cao |
