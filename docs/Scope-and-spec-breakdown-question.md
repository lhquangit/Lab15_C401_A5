1. Bài toán nghiệp vụ lớn nhất hiện tại là gì, đo bằng metric nào?

Bài toán hiện tại là nhân sự y tế mất nhiều thời gian cho các tác vụ lặp lại như tra cứu thông tin quy trình, tổng hợp hồ sơ; đồng thời bộ phận tiếp nhận bị quá tải bởi các câu hỏi hành chính lặp lại.

Được đo bằng các metric: Giảm 25-35% thời gian xử lý tác vụ nội bộ, và thời gian tiết kiệm trung bình đạt 2.5 phút/request.

2. User chính của MVP là bác sĩ, điều dưỡng, nhân viên tiếp nhận hay cả 3?

User chính của MVP bao gồm cả 3 nhóm: Bác sĩ, điều dưỡng, và nhân viên tiếp nhận.

3. Mỗi nhóm user sẽ dùng hệ thống để làm việc gì cụ thể?

Bác sĩ: Dùng để tóm tắt hồ sơ bệnh án theo các template chuẩn.

Điều dưỡng: Dùng để tra cứu quy trình nội bộ, văn bản và biểu mẫu theo chính sách bệnh viện.

Nhân viên tiếp nhận: Dùng để hỏi đáp các thông tin hành chính (lịch khám, thủ tục, quy định) giúp giảm tải việc trả lời thủ công.

4. Sản phẩm/scenario sẽ được mô tả ngắn gọn như thế nào trong proposal?

Sản phẩm mang tên Hospital Clinical Support Assistant - là một trợ lý AI nội bộ nhằm tối ưu vận hành cho bệnh viện thông qua việc hỗ trợ 3 tác vụ in-scope (tóm tắt hồ sơ, hỏi đáp quy trình và hành chính) một cách an toàn và có kiểm soát.

5. Chủ đề xuyên suốt của proposal là gì?

Chủ đề xuyên suốt là tối ưu vận hành nội bộ bệnh viện thông qua trợ lý AI đảm bảo an toàn, có kiểm soát rủi ro (Safety by design), bắt buộc áp dụng human-in-the-loop và không thay thế quyết định lâm sàng.

6. Vì sao scenario này phù hợp để phân tích deployment + cost?

Bối cảnh bệnh viện có các ràng buộc enterprise khắt khe (dữ liệu sức khỏe nhạy cảm, tích hợp HIS/EMR cũ) bắt buộc phải phân tích kỹ mô hình Hybrid deployment để cân bằng bảo mật và tốc độ.

Về chi phí, do lưu lượng sử dụng phân bổ không đều (traffic spike giờ cao điểm), cần tính toán kỹ chi phí token trung bình/request (1,450 token/request) và scale forecast để đảm bảo hệ thống duy trì được ROI dương.

7. Use case nào tạo giá trị nhanh nhất trong 2-4 tuần đầu?

Hỏi đáp hành chính cho nhân viên tiếp nhận, vì nhóm này chiếm tới 45% tổng lượng request, giải quyết các câu hỏi lặp lại nhiều và dễ dàng áp dụng cache để tối ưu hiệu năng.

8. Use case nào có rủi ro cao nhất nếu AI trả lời sai?

Tóm tắt hồ sơ bệnh án và hỏi đáp quy trình/chính sách y tế. Nếu AI bị hallucination ở các tính năng này, có thể gây sai lệch vận hành hoặc ảnh hưởng tới quyết định lâm sàng.

9. Truy vấn nào bắt buộc phải có trích nguồn?

Các truy vấn hỏi đáp quy trình nội bộ/chính sách (bắt buộc kèm link hoặc tên tài liệu nguồn).

Tóm tắt hồ sơ bệnh án (phải có chỉ dấu nguồn trích từ mục dữ liệu chính).

10. Truy vấn nào bắt buộc phải chuyển human review?

Các truy vấn có rủi ro cao hoặc hệ thống đánh giá có độ tin cậy (confidence) thấp.

11. Truy vấn nào phải bị từ chối ngay?

Các yêu cầu tư vấn chẩn đoán, đưa ra y lệnh điều trị hoặc phục vụ trực tiếp bệnh nhân ngoài hệ thống nội bộ.

12. Khi nào một feature được xem là "xong" về nghiệp vụ?

Khi tính năng đó giúp giảm thời gian thao tác cho người dùng, đảm bảo phản hồi nhất quán, tuân thủ chặt chẽ ranh giới an toàn dữ liệu, và vận hành trơn tru cơ chế fallback/human review cho các rủi ro đã xác định.

13. MVP có cần UI riêng hay chỉ cần chat interface nội bộ?

Chỉ cần chat interface nội bộ (Chat assistant nội bộ theo vai trò) phục vụ cho việc truy vấn.

14. Cần bao nhiêu ngôn ngữ trong MVP?

Dựa trên bối cảnh cung cấp, hệ thống xử lý các tài liệu quy trình, biểu mẫu nội bộ và bệnh án của một bệnh viện. Hệ thống chủ yếu sẽ phục vụ theo ngôn ngữ được sử dụng trong tài liệu nghiệp vụ này (Tiếng Việt).

15. Có cần phân quyền theo khoa/phòng ngay từ MVP không?

Có. Role-based access control (RBAC) phân quyền chặt chẽ theo vai trò và khoa/phòng là yêu cầu bắt buộc ngay từ Phase 1.

16. Có cần lịch sử hỏi đáp, audit trail, export log ngay từ MVP không?

Có. Hệ thống bắt buộc phải lưu trữ lịch sử tương tác, logging và audit trail cơ bản nhằm phục vụ truy vết, nhưng phải có cơ chế ẩn/giảm lộ PII (dữ liệu nhạy cảm).

17. Có cần đánh dấu feedback đúng/sai/chưa đủ ngay từ MVP không?

Có. Cần theo dõi KPI "Tỷ lệ phản hồi bị đánh dấu sai < 5%", do đó tính năng cho phép user phản hồi trực tiếp chất lượng câu trả lời là bắt buộc.

18. Có cần dashboard cho admin hay chỉ cho end-user?

Có. Yêu cầu triển khai một dashboard vận hành tối thiểu dành cho Ban giám đốc và IT Ops để quan sát các chỉ số hiệu quả và bảo mật.