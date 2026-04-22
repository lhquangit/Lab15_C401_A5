# Phần 2: Kiến trúc sư hệ thống AI - Trả lời Questionnaire

Dưới đây là phần trả lời cho các câu hỏi trong Section 2 của `system-breakdown-questionnaire.md`.

## 2.2 Các quyết định cần chốt

1. Hệ thống có cần multi-agent hay không?
   **Có, dùng Router Agent để phân phối use case.**
2. Nếu có, agent nào làm planning, agent nào làm retrieval, agent nào làm judgement?
   **Router (Planning), Knowledge Retriever (SOP/Record), Fact-Checker (Judgement).**
3. Route nào không nên dùng agent mà chỉ dùng single prompt?
   **Admin Q&A (hỏi đáp hành chính thông thường).**
4. Model chính cho từng use case là model nào?
   **GPT-4o cho Tóm tắt hồ sơ và SOP; GPT-4o-mini cho Admin Q&A.**
5. Model fallback cấp 1 và cấp 2 là gì?
   **Fallback 1: GPT-4o-mini; Fallback 2: Claude 3 Haiku.**
6. Rule fallback là gì: timeout, quota, cost, quality hay provider error?
   **Timeout (>8s), Provider Error (5xx), và Quality (Confidence score < 0.7).**
7. Khi nào cần evidence gate?
   **Bắt buộc cho SOP Q&A và Tóm tắt hồ sơ để tránh hallucination.**
8. Khi nào cần human-in-the-loop?
   **Record summary có rủi ro lâm sàng cao hoặc SOP Q&A không tìm thấy bằng chứng rõ ràng.**
9. Max input token và max output token cho từng route là bao nhiêu?
   **Input: 4,000 (Summary), 2,000 (SOP); Output: 1,000 (Summary), 500 (SOP).**
10. Có cần response streaming không?
    **Có, để giảm perceived latency cho user.**

## 2.3 Bộ câu hỏi breakdown về Agent và Model

1. Use case nào cần workflow 1 bước, use case nào cần workflow nhiều bước?
   **1 bước: Admin Q&A. Nhiều bước: Tóm tắt hồ sơ (Retrieve + Summarize + Check), SOP (Retrieve + Rerank + Gen).**
2. Tóm tắt hồ sơ có cần retrieval + summarization + validation không?
   **Có, bắt buộc validation để đảm bảo tính chính xác của dữ liệu y tế.**
3. SOP Q&A có cần retrieval + rerank + answer generation không?
   **Có, dùng Reranker để tăng độ chính xác của tài liệu quy trình.**
4. Admin Q&A có cần cache trước khi gọi model không?
   **Có, cache cho 80% câu hỏi phổ biến để tối ưu cost.**
5. Route nào có thể bỏ qua model nếu trả lời từ KB có cấu trúc?
   **Thông tin lịch khám, danh mục khoa/phòng, biểu mẫu hành chính.**
6. Context window của model chính có đủ cho use case phức tạp nhất không?
   **Có (GPT-4o 128k), đủ cho hồ sơ bệnh án dày.**
7. Embedding model nào dùng cho tài liệu nội bộ?
   **text-embedding-3-small (OpenAI).**
8. Có cần reranker không, và reranker được gọi trong bao nhiêu phần trăm request?
   **Có, gọi trong 35% request (nhóm SOP).**
9. Judge model có cần thiết không, nếu có thì dùng khi nào?
   **Cần thiết cho Record Summary (100%) và SOP (nếu confidence biên).**
10. Prompt versioning và prompt approval ai chịu trách nhiệm?
    **AI Engineer xây dựng, Clinical Advisor phê duyệt nội dung y tế.**

## 2.4 Bộ câu hỏi breakdown về chi phí token theo case

1. Mỗi use case có bao nhiêu route xử lý khác nhau?
   **3 route: Direct Answer (Cache/KB), Standard RAG, High-risk (Human Review).**
2. Với `record summary`, input token trung bình là bao nhiêu?
   **2,500 tokens (bao gồm context bệnh án).**
3. Với `record summary`, output token trung bình là bao nhiêu?
   **600 tokens (summary theo template).**
4. Với `record summary`, có thêm retrieval chunk, rerank, judge, fallback không?
   **Có Judge model để check fact.**
5. Với `SOP Q&A`, input token trung bình là bao nhiêu?
   **1,500 tokens (bao gồm docs retrieved).**
6. Với `SOP Q&A`, output token trung bình là bao nhiêu?
   **400 tokens.**
7. Với `admin Q&A`, input token trung bình là bao nhiêu?
   **500 tokens.**
8. Với `admin Q&A`, output token trung bình là bao nhiêu?
   **200 tokens.**
9. Phần trăm request vào từng route là bao nhiêu?
   **Hành chính (45%), SOP (35%), Tóm tắt (20%).**
10. Phần trăm request gọi fallback là bao nhiêu?
    **12%.**
11. Phần trăm request cần human review là bao nhiêu?
    **6%.**
12. Cost per request của từng route là bao nhiêu?
    **Tóm tắt (~540 VND), SOP (~400 VND), Hành chính (~5 VND).**
13. Cost per 1,000 requests của từng route là bao nhiêu?
    **Avg ~454,000 VND.**
14. Cost khi scale 1x, 5x, 10x là bao nhiêu?
    **90M, 360M, 660M VND/tháng.**

