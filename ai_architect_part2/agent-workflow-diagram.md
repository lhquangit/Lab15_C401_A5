# Agent Workflow Diagram (ASCII/Unicode)

Dưới đây là sơ đồ luồng được vẽ trực tiếp bằng ký tự để có thể hiển thị rõ ràng trên mọi trình đọc Markdown mà không cần plugin Mermaid.

```text
                             [ 👤 Người dùng hỏi ]
                                      │
                                      ▼
                           { 🤖 Router Agent }
                                      │
           ┌──────────────────────────┼──────────────────────────┐
           │                          │                          │
           ▼                          ▼                          ▼
 [Luồng 1: Hành chính]         [Luồng 2: SOP]          [Luồng 3: Hồ sơ]
 (Câu hỏi phổ biến)         (Quy trình nội bộ)        (Tóm tắt bệnh án)
           │                          │                          │
           ▼                          ▼                          ▼
  [( Cache / KB )]          [🔍 RAG Engine]           [( HIS / EMR )]
           │                          │                          │
           ▼                          ▼                          ▼
      { Có sẵn? } ──Không──┐   [ Reranker ]         [Gắn vào Template]
           │               │          │                          │
           Có              │          ▼                          ▼
           │               └─► [ GenSOP (GPT-4o) ]    [ GenRecord (GPT-4o) ]
           │                          │                          │
           │                          │ ┌ - - - - - - - - - - - -┤
           │                          │ ┊ Lỗi/Timeout            ┊ Lỗi/Timeout
           │                          ▼ ▼                        ▼ ▼
           │                 { ⚖️ Judge Model }        [⚙️ Fallback GPT-4o-mini]
           │                 (Kiểm tra Fact)                     │
           │                          │                          │
           │                          ├──── (Tin cậy <= 0.7) ────┐
           │                          │                          │
           │                    (Tin cậy > 0.7)                  ▼
           │                          │              [👥 Human-in-the-loop]
           │                          │                  (Chờ duyệt)
           │                          │                          │
           └──────────────────────────┼──────────────────────────┘
                                      │
                                      ▼
                            ([ ✅ Trả kết quả ])
```

## Chú thích các bước chính:
1. **Router Agent**: Nhận câu hỏi và phân luồng dựa vào ý định của người dùng.
2. **Luồng 1 (Hành chính)**: Ưu tiên trả lời nhanh từ Cache. Nếu Cache không có, đẩy sang Luồng 2 để tìm kiếm trong tài liệu.
3. **Luồng 2 (SOP) & Luồng 3 (Hồ sơ)**: Trích xuất dữ liệu, định dạng và gọi GPT-4o để tổng hợp câu trả lời.
4. **Judge Model**: Đóng vai trò là chốt chặn an toàn (Quality Gate). Mọi thông tin tổng hợp đều phải đi qua đây để chấm điểm độ tin cậy.
5. **Fallback & Human-in-the-loop**: 
   - Nếu LLM chính (GPT-4o) bị lỗi hoặc quá tải -> Chạy model phụ (GPT-4o-mini) để không làm đứt gãy trải nghiệm.
   - Nếu độ tin cậy quá thấp (<= 0.7) -> Treo request, báo cho con người duyệt để đảm bảo tính an toàn y tế.
