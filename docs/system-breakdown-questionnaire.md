# Bộ Câu Hỏi Breakdown Hệ Thống

Mục tiêu của tài liệu này là chia rõ công việc discovery và decision theo 4 role chính:
- Scope & Spec
- Kiến trúc sư hệ thống AI
- Hạ tầng
- Kinh doanh

Mỗi role phải trả lời được 3 câu hỏi:
- Cần chốt quyết định gì?
- Cần thu thập dữ liệu gì?
- Cần xuất ra artifact nào để team khác tiếp tục làm việc?

Ngoài 4 role trên, tài liệu này bổ sung thêm các hạng mục liên chức năng để bảo đảm cover đầy đủ checklist trong [instruction.md](/Users/quanliver/Projects/AI_Vin_Learner/lab15/docs/instruction.md).

## 0) Cổng kiểm tra dữ liệu thật

Section này dùng để chặn trước một vấn đề rất hay gặp: proposal có số liệu, nhưng không ai biết số đó đến từ đâu, cập nhật lúc nào, và còn đúng hay không.

### 0.1 Điều kiện PASS trước khi viết proposal

1. Đã có `data freeze date` cho toàn bộ proposal chưa?
2. Mọi con số trong proposal đã có `owner + source + snapshot date` chưa?
3. Đã tách rõ `số liệu thật` và `estimate` chưa?
4. Estimate đã có `assumption + confidence + owner` chưa?
5. Đã có rule ưu tiên nguồn khi số liệu mâu thuẫn chưa?
6. Đã có sign-off của người phụ trách từng role chưa?

### 0.2 Artifact bắt buộc

1. `data-source-registry.csv`
2. `assumption-log.md`
3. `decision-log.md`
4. `signoff.md`

### 0.3 Checklist đầu ngày

1. Nhóm hiện có đủ 4-6 người chưa?
2. Chủ đề/scenario đã được chọn và khóa chưa?
3. Đã phân vai đủ `Product Lead`, `Architect`, `Cost Lead`, `Reliability Lead`, `Presenter` chưa?
4. Đã có workspace chung để ghi nhận tài liệu và quyết định chưa?

---

## 1) Scope & Spec

Role này chịu trách nhiệm khóa phạm vi MVP. Nếu phần này mơ hồ, các role còn lại sẽ estimate sai, chọn sai model, sai infra, và sai business model.

### 1.1 Quy trách nhiệm

- Xác định bài toán đúng cần giải quyết trong MVP
- Chốt in-scope và out-of-scope
- Chốt actor, use case, output, boundary của hệ thống
- Chốt definition of done cho từng use case

### 1.2 Các quyết định cần chốt

1. MVP giải quyết vấn đề nào đầu tiên?
2. User nào được phục vụ trong MVP?
3. User nào không nằm trong MVP?
4. 3-5 use case bắt buộc phải có để go-live là gì?
5. Use case nào được đưa sang Phase 2?
6. Output nào hệ thống được phép trả về?
7. Output nào hệ thống tuyệt đối không được trả về?
8. Hệ thống chỉ `read-only` hay có `write-back` vào HIS/EMR?
9. Tính năng nào là `must-have`, tính năng nào là `nice-to-have`?
10. Điều kiện nào khiến một request bị từ chối vì nằm ngoài scope?

### 1.3 Bộ câu hỏi breakdown

1. Bài toán nghiệp vụ lớn nhất hiện tại là gì, đo bằng metric nào?
2. User chính của MVP là bác sĩ, điều dưỡng, nhân viên tiếp nhận hay cả 3?
3. Mỗi nhóm user sẽ dùng hệ thống để làm việc gì cụ thể?
4. Sản phẩm/scenario sẽ được mô tả ngắn gọn như thế nào trong proposal?
5. Chủ đề xuyên suốt của proposal là gì?
6. Vì sao scenario này phù hợp để phân tích deployment + cost?
7. Use case nào tạo giá trị nhanh nhất trong 2-4 tuần đầu?
8. Use case nào có rủi ro cao nhất nếu AI trả lời sai?
9. Truy vấn nào bắt buộc phải có trích nguồn?
10. Truy vấn nào bắt buộc phải chuyển human review?
11. Truy vấn nào phải bị từ chối ngay?
12. Khi nào một feature được xem là "xong" về nghiệp vụ?
13. MVP có cần UI riêng hay chỉ cần chat interface nội bộ?
14. Cần bao nhiêu ngôn ngữ trong MVP?
15. Có cần phân quyền theo khoa/phòng ngay từ MVP không?
16. Có cần lịch sử hỏi đáp, audit trail, export log ngay từ MVP không?
17. Có cần đánh dấu feedback `đúng/sai/chưa đủ` ngay từ MVP không?
18. Có cần dashboard cho admin hay chỉ cho end-user?

### 1.4 Artifact bắt buộc

1. `scope-matrix.md`
2. `use-case-list.md`
3. `in-scope-vs-out-of-scope.md`
4. `acceptance-criteria.md`

### 1.5 Mẫu scope matrix cần có

| Use case | User | In scope | Out of scope | Mức rủi ro | Human review | Definition of done |
|---|---|---|---|---|---|---|
| Tóm tắt hồ sơ | Bác sĩ | Yes/No | Mô tả | Thấp/Vừa/Cao | Yes/No | Mô tả |
| Hỏi đáp SOP | Điều dưỡng | Yes/No | Mô tả | Thấp/Vừa/Cao | Yes/No | Mô tả |
| Hỏi đáp hành chính | Tiếp nhận | Yes/No | Mô tả | Thấp/Vừa/Cao | Yes/No | Mô tả |

---

## 2) Kiến trúc sư hệ thống AI

Role này chịu trách nhiệm thiết kế logic AI system:
- Agent có cần hay không
- Dùng model nào cho use case nào
- Fallback chain là gì
- Token flow từ input -> processing -> output ra sao
- Cost per request và cost theo từng route

### 2.1 Quy trách nhiệm

- Thiết kế orchestration cho từng use case
- Chọn model chính, model fallback, embedding model, reranker
- Xác định route nào cần agentic workflow, route nào chỉ cần single call
- Tính cost token theo từng case để dev cover được

### 2.2 Các quyết định cần chốt

1. Hệ thống có cần multi-agent hay không?
2. Nếu có, agent nào làm planning, agent nào làm retrieval, agent nào làm judgement?
3. Route nào không nên dùng agent mà chỉ dùng single prompt?
4. Model chính cho từng use case là model nào?
5. Model fallback cấp 1 và cấp 2 là gì?
6. Rule fallback là gì: timeout, quota, cost, quality hay provider error?
7. Khi nào cần evidence gate?
8. Khi nào cần human-in-the-loop?
9. Max input token và max output token cho từng route là bao nhiêu?
10. Có cần response streaming không?

### 2.3 Bộ câu hỏi breakdown về Agent và Model

1. Use case nào cần workflow 1 bước, use case nào cần workflow nhiều bước?
2. Tóm tắt hồ sơ có cần retrieval + summarization + validation không?
3. SOP Q&A có cần retrieval + rerank + answer generation không?
4. Admin Q&A có cần cache trước khi gọi model không?
5. Route nào có thể bỏ qua model nếu trả lời từ KB có cấu trúc?
6. Context window của model chính có đủ cho use case phức tạp nhất không?
7. Embedding model nào dùng cho tài liệu nội bộ?
8. Có cần reranker không, và reranker được gọi trong bao nhiêu phần trăm request?
9. Judge model có cần thiết không, nếu có thì dùng khi nào?
10. Prompt versioning và prompt approval ai chịu trách nhiệm?

### 2.4 Bộ câu hỏi breakdown về chi phí token theo case

1. Mỗi use case có bao nhiêu route xử lý khác nhau?
2. Với `record summary`, input token trung bình là bao nhiêu?
3. Với `record summary`, output token trung bình là bao nhiêu?
4. Với `record summary`, có thêm retrieval chunk, rerank, judge, fallback không?
5. Với `SOP Q&A`, input token trung bình là bao nhiêu?
6. Với `SOP Q&A`, output token trung bình là bao nhiêu?
7. Với `admin Q&A`, input token trung bình là bao nhiêu?
8. Với `admin Q&A`, output token trung bình là bao nhiêu?
9. Phần trăm request vào từng route là bao nhiêu?
10. Phần trăm request gọi fallback là bao nhiêu?
11. Phần trăm request cần human review là bao nhiêu?
12. Cost per request của từng route là bao nhiêu?
13. Cost per 1,000 requests của từng route là bao nhiêu?
14. Cost khi scale 1x, 5x, 10x là bao nhiêu?

### 2.5 Bảng mà kiến trúc sư AI bắt buộc phải xuất

| Use case | Main model | Fallback 1 | Fallback 2 | Embedding | Reranker | Judge | Max input | Max output | Cost/request |
|---|---|---|---|---|---|---|---:|---:|---:|
| Record summary | TBD | TBD | TBD | TBD | Yes/No | Yes/No | TBD | TBD | TBD |
| SOP Q&A | TBD | TBD | TBD | TBD | Yes/No | Yes/No | TBD | TBD | TBD |
| Admin Q&A | TBD | TBD | TBD | TBD | Yes/No | Yes/No | TBD | TBD | TBD |

### 2.6 Artifact bắt buộc

1. `model-routing-matrix.md`
2. `fallback-policy.md`
3. `token-cost-by-use-case.xlsx` hoặc `.csv`
4. `agent-workflow-diagram.md`

---

## 3) Hạ tầng

Role này chịu trách nhiệm quyết định hệ thống sẽ chạy ở đâu, theo topology nào, scale thế nào, và vì sao lựa chọn đó hợp lý hơn các lựa chọn còn lại.

### 3.1 Quy trách nhiệm

- So sánh cloud, on-prem, hybrid
- Chọn region, network model, data boundary
- Sizing compute, storage, queue, vector DB
- Chốt cách scale, observe, backup, DR

### 3.2 Các quyết định cần chốt

### 3.2 Các quyết định cần chốt

1. Chọn `cloud`, `on-prem`, hay `hybrid`? -> **Hybrid Deployment**.
   - *Giải thích:* Cân bằng giữa tính bảo mật của dữ liệu y tế (On-prem) và khả năng tính toán mạnh mẽ của AI (Cloud).
2. Nếu hybrid, phần nào ở cloud và phần nào ở on-prem? -> **Cloud: AI Model Inference (Azure OpenAI), Orchestration Logic, Monitoring & Observability. On-prem: HIS/EMR Connectors, PHI/PII Database, Identity Provider (AD/LDAP).**
   - *Giải thích:* Giữ dữ liệu nhạy cảm nhất trong vùng kiểm soát nội bộ, chỉ gửi dữ liệu đã được xử lý (anonymized if possible) lên Cloud để inference.
3. Region nào được phép đặt workload? -> **Southeast Asia (Singapore)** để tối ưu Latency.
4. Database/vector store nào được chọn? -> **PostgreSQL (RDBMS) + pgvector (Vector Store)**. Redis cho Distributed Caching.
5. Có cần Kubernetes hay chỉ cần managed service là đủ? -> **Managed Services (PaaS)** là đủ cho giai đoạn MVP (ví dụ: Azure App Service).
   - *Giải thích:* Giảm thiểu Complexity vận hành và tăng tốc độ triển khai (Time-to-Market).
6. Có cần queue không, nếu có thì dùng công nghệ nào? -> **Có**, sử dụng **Azure Service Bus** hoặc **RabbitMQ** cho các tác vụ Async (Long-running tasks).
7. Có cần GPU không, hay toàn bộ là API-based? -> Toàn bộ là **API-based Serverless Inference**.
   - *Giải thích:* Không cần đầu tư Capex cho phần cứng GPU đắt đỏ và phức tạp trong giai đoạn MVP.
8. Hệ thống nào cần HA ngay từ MVP? -> **API Gateway, Retrieval Service, và Vector Database**.
9. Backup, restore, DR mục tiêu là gì? -> **Daily Backup**. RPO: 1 hour, RTO: 4 hours. **Warm Standby** tại secondary region.
10. Metric nào dùng để autoscale? -> **Requests Per Second (RPS)** và **CPU/Memory Utilization**.
11. Ba ràng buộc enterprise lớn nhất là gì? -> **(1) Data Privacy Compliance (HIPAA/PII), (2) Legacy Integration (HIS/EMR), (3) Latency SLA (< 8s P95).**
12. Hai lý do mạnh nhất cho lựa chọn deployment cuối cùng là gì? -> **(1) Security cho dữ liệu y tế nhạy cảm tại On-prem, (2) Khả năng Elastic Scaling cho AI workload tại Cloud.**
13. Dữ liệu nào được sử dụng trong MVP? -> **Medical Records (Bệnh án), Hospital SOPs (Quy trình), Admin Forms (Biểu mẫu).**
14. Dữ liệu nào nhạy cảm nhất, và mức độ nhạy cảm ra sao? -> **PHI (Protected Health Information)**. Mức độ: **Extremely High** (Cần mã hóa và kiểm soát truy cập chặt chẽ).

### 3.3 Bộ câu hỏi breakdown về lựa chọn cloud

1. Mục tiêu của hạ tầng là tối ưu bảo mật, chi phí, tốc độ triển khai hay khả năng scale? -> Tối ưu **Security** và **Deployment Speed**.
2. Policy của bệnh viện có cho phép đưa PHI/PII lên cloud không? -> **Không hoàn toàn**, do đó cần lưu Database chính tại On-prem.
3. Nếu không, phần nào bắt buộc phải giữ on-prem? -> **HIS/EMR Database** và các **User Identity data**.
4. Nếu dùng cloud, cần private networking/VPN/connectivity nào? -> **Site-to-Site VPN** hoặc **Azure ExpressRoute**.
   - *Giải thích:* Đảm bảo kênh truyền dữ liệu an toàn và tách biệt với Public Internet.
5. AWS, Azure, GCP hay private cloud đang là lựa chọn thực tế? -> **Azure** (ưu tiên do hệ sinh thái Microsoft có độ phủ lớn trong mảng Enterprise/Healthcare).
6. Team hiện tại đã có sẵn kỹ năng trên nền tảng nào? -> **Azure / DevOps**.
7. Nền tảng nào có pricing và managed service phù hợp nhất với MVP? -> **Azure** với các gói **Pay-as-you-go** cho AI.
8. Nền tảng nào giúp compliance dễ hơn? -> **Azure (HIPAA, HITRUST ready)**.
9. Lock-in của từng lựa chọn lớn đến đâu? -> **Medium** (phụ thuộc vào Proprietary AI APIs).
10. Lựa chọn nào giúp pilot nhanh nhất mà vẫn an toàn? -> **Hybrid Cloud với Managed AI Services**.

### 3.4 Bộ câu hỏi breakdown về sizing và capacity

1. DAU, request/day, peak QPS là bao nhiêu? -> **DAU: 220, Requests: 2,640/day, Peak QPS: ~10.**
   - *Giải thích:* Dựa trên giả định 55% active rate của 400 users nội bộ.
2. App service cần bao nhiêu CPU/RAM/replica cho baseline? -> **2 vCPU, 4GB RAM, Minimum 2 Replicas.**
3. Vector store cần bao nhiêu dung lượng cho 3 tháng, 6 tháng, 12 tháng? -> **3 months: 50GB, 6 months: 100GB, 12 months: 250GB.**
4. Queue worker cần bao nhiêu concurrency? -> **10-20 concurrent jobs**.
5. Log và monitoring retention bao lâu? -> **Operational Logs: 30 days. Audit Logs: 1-2 years.**
   - *Giải thích:* Tuân thủ quy định về lưu trữ dữ liệu y tế của Enterprise.
6. Egress/ingress giữa on-prem và cloud là bao nhiêu? -> Low (chủ yếu là Text Data). Ước tính **< 50GB/month**.
7. Nút cổ chai đầu tiên khi traffic tăng 5x sẽ là gì? -> **Vector DB Search Performance**.
8. Nút cổ chai đầu tiên khi traffic tăng 10x sẽ là gì? -> **LLM Provider Rate Limits (RPM/TPM)**.
   - *Giải thích:* LLM providers thường có quota cứng về throughput, cần chiến lược Multi-region hoặc Multi-provider khi scale lớn.
9. Có cần CDN/cache cho admin Q&A không? -> **Có**, sử dụng **Redis Cache** cho các câu trả lời tĩnh.
10. Có cần warm standby hoặc multi-zone ngay từ MVP không? -> **Cần Multi-zone** để đảm bảo HA.

### 3.5 Bảng so sánh phương án hạ tầng bắt buộc

| Option | Bảo mật | Chi phí | Tốc độ triển khai | Độ phức tạp vận hành | Khả năng scale | Kết luận |
|---|---|---|---|---|---|---|
| Cloud | Vừa | Thấp | Cao | Thấp | Cao | Không |
| On-prem | Cao | Cao | Thấp | Cao | Thấp | Không |
| Hybrid | Cao | Vừa | Vừa | Vừa | Cao | **Chọn (Hybrid)** |

*Giải thích bảng:* Hybrid là phương án duy nhất đáp ứng được yêu cầu khắt khe về bảo mật dữ liệu y tế (PHI) mà vẫn đảm bảo tính linh hoạt và khả năng mở rộng của hệ thống AI.


### 3.6 Artifact bắt buộc

1. `infra-options-comparison.md`
2. `target-architecture.md`
3. `capacity-plan.xlsx` hoặc `.csv`
4. `network-data-boundary.md`
5. `sre-readiness-checklist.md`

---

## 4) Kinh doanh

Role này chịu trách nhiệm trả lời câu hỏi: xây xong rồi thì kinh doanh thế nào, tính gói dịch vụ thế nào, target user nào hợp với gói nào, và với cấu trúc chi phí đã biết thì lỗ lãi ra sao.

### 4.1 Quy trách nhiệm

- Chọn target customer segment
- Định giá gói dịch vụ
- Mô hình hóa doanh thu, chi phí, gross margin
- Chốt quyết định kinh doanh cho MVP và sau MVP

### 4.2 Các quyết định cần chốt

1. Sản phẩm bán cho ai?
2. Đơn vị mua là bệnh viện, khoa, hay từng user?
3. Giá tính theo seat, theo request, theo usage hay hybrid?
4. Có bao nhiêu gói thanh toán?
5. Mỗi gói nhắm tới segment nào?
6. Tính năng nào nằm trong mỗi gói?
7. Trần usage của mỗi gói là bao nhiêu?
8. Overage tính thế nào?
9. Unit economics có dương không?
10. Gói nào được dùng để pilot, gói nào dùng để mở rộng?

### 4.3 Bộ câu hỏi breakdown về target user và payment plan

1. Khách hàng mục tiêu đầu tiên là bệnh viện tư, bệnh viện công, phòng khám hay chuỗi y tế?
2. ICP cụ thể là gì: quy mô, ngân sách, maturity công nghệ, mức độ nhạy cảm dữ liệu?
3. Người quyết định mua là ai?
4. Người sử dụng thực tế là ai?
5. Bài toán nào khiến khách hàng willing to pay sớm nhất?
6. Thanh toán theo tháng, quý hay năm?
7. Gói `Starter`, `Growth`, `Enterprise` có cần thiết không?
8. Gói nào giới hạn theo seat?
9. Gói nào giới hạn theo request hoặc token?
10. Gói nào bao gồm human review, SLA, support priority?
11. Tính năng nào là add-on có thể bán riêng?
12. Có cần phí setup/implementation một lần không?

### 4.4 Bộ câu hỏi breakdown về lỗ lãi và đơn vị kinh tế

1. Cost phục vụ 1 khách hàng/tháng là bao nhiêu?
2. Cost biến đổi lớn nhất là model hay human review?
3. Gross margin theo từng gói là bao nhiêu?
4. Gói nào có nguy cơ lỗ nhất nếu user dùng sát trần?
5. Mức usage nào thì cần tăng giá?
6. Mức usage nào thì cần buộc fallback sang model rẻ hơn?
7. Human review có được tính riêng thành phí phụ trội không?
8. Pilot trả phí hay free?
9. Nếu free pilot, điều kiện để chuyển sang paid là gì?
10. Break-even point theo số khách hàng là bao nhiêu?
11. Nếu chỉ có 1 bệnh viện đầu tiên, dự án đang lỗ hay lãi?
12. Nếu scale lên 5 bệnh viện và 10 bệnh viện, gross profit là bao nhiêu?
13. Hidden cost lớn nhất là gì?
14. Ước lượng hiện tại có quá lạc quan không, và giả định nào dễ sai nhất?
15. Tổng cost MVP ở mức baseline là bao nhiêu?

### 4.5 Bảng mà business bắt buộc phải xuất

| Gói | Target customer | Included usage | Overage | Giá/tháng | Est. COGS | Gross margin | Phù hợp với ai |
|---|---|---|---|---:|---:|---:|---|
| Starter | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
| Growth | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
| Enterprise | TBD | TBD | TBD | TBD | TBD | TBD | TBD |

### 4.6 Artifact bắt buộc

1. `pricing-packages.md`
2. `unit-economics.xlsx` hoặc `.csv`
3. `target-customer-profile.md`
4. `go-to-market-assumptions.md`
5. `profit-loss-scenarios.md`

---

## 5) Hạng mục liên chức năng để cover `instruction.md`

Section này bổ sung những phần mà 4 role phía trên chưa cover hết nếu chỉ nhìn theo góc hệ thống.

### 5.1 Mini Reflection và năng lực nhóm

1. Mỗi thành viên học được 3 điều gì trong Phase 1?
2. Điểm yếu lớn nhất của từng người là gì?
3. Mỗi người muốn đào sâu hướng nào ở Phase 2?
4. Nhóm có thể tổng hợp được 3-5 insight về năng lực chung không?
5. 2-3 kỹ năng mạnh nhất của nhóm là gì?

### 5.2 Cost Optimization

1. 3 chiến lược tối ưu chi phí ưu tiên là gì?
2. Mỗi chiến lược tiết kiệm khoản chi phí nào?
3. Lợi ích lớn nhất của từng chiến lược là gì?
4. Trade-off của từng chiến lược là gì?
5. Khi nào nên áp dụng từng chiến lược?
6. Chiến lược nào làm ngay?
7. Chiến lược nào làm sau?

### 5.3 Reliability & Risk Management

Section này gom toàn bộ phần reliability và risk management vào một chỗ, để team không bị sót các câu hỏi vận hành quan trọng khi viết proposal.

#### 5.3.1 Risk register

1. Top 10 rủi ro lớn nhất của MVP là gì?
2. Mỗi rủi ro thuộc nhóm nào: `nghiệp vụ`, `model`, `dữ liệu`, `hạ tầng`, `bảo mật`, `compliance`, `vận hành`?
3. Mức độ ảnh hưởng `Impact` của từng rủi ro là gì?
4. Xác suất xảy ra `Likelihood` của từng rủi ro là gì?
5. Tín hiệu phát hiện sớm của từng rủi ro là gì?
6. Biện pháp giảm thiểu `Mitigation` cho từng rủi ro là gì?
7. Phương án dự phòng `Contingency` nếu rủi ro xảy ra là gì?
8. Owner chịu trách nhiệm theo dõi và xử lý từng rủi ro là ai?
9. Rủi ro nào được chấp nhận tạm thời trong MVP?
10. Rủi ro nào bắt buộc phải xử lý trước go-live?

#### 5.3.2 Failure scenarios và tác động lên người dùng

1. Nếu `traffic spike`, impact lên user là gì?
2. Nếu `provider timeout`, hệ thống fallback thế nào?
3. Nếu `response chậm`, ngưỡng chấp nhận là bao nhiêu?
4. Nếu `fallback` cũng lỗi, hệ thống trả về gì cho user?
5. Nếu `hallucination` xảy ra ở truy vấn nhạy cảm, hệ thống chặn bằng cơ chế nào?
6. Nếu `retrieval` không tìm thấy bằng chứng, hệ thống trả lời hay từ chối?
7. Nếu `HIS/EMR` mất kết nối, hệ thống xuống cấp dịch vụ ra sao?
8. Nếu `vector store` hoặc `cache` lỗi, route nào còn hoạt động được?
9. Nếu quota model hết hoặc rate limit chạm ngưỡng, route nào bị ảnh hưởng trước?
10. Với mỗi failure scenario, giải pháp ngắn hạn và dài hạn là gì?

#### 5.3.3 SLO, SLA và khả năng quan sát

1. Availability SLO của hệ thống là bao nhiêu?
2. P95/P99 latency target cho từng use case là bao nhiêu?
3. Timeout rate chấp nhận được là bao nhiêu?
4. Error rate chấp nhận được là bao nhiêu?
5. Tỷ lệ fallback chấp nhận được là bao nhiêu?
6. Tỷ lệ human review chấp nhận được là bao nhiêu?
7. Error budget theo tháng hoặc theo quý là bao nhiêu?
8. Metric nào bắt buộc phải monitor theo thời gian thực?
9. Metric nào dùng để cảnh báo sớm trước khi user bị ảnh hưởng?
10. Dashboard nào cần cho `Engineering`, `Operations`, `Business`?

#### 5.3.4 Incident response và vận hành sự cố

1. Đã có phân loại `incident severity` như `Sev-1`, `Sev-2`, `Sev-3` chưa?
2. Điều kiện để một sự cố được nâng lên `Sev-1` là gì?
3. `MTTD` mục tiêu là bao nhiêu?
4. `MTTR` mục tiêu là bao nhiêu?
5. `RTO` mục tiêu là bao nhiêu?
6. `RPO` mục tiêu là bao nhiêu?
7. On-call owner cho từng nhóm sự cố là ai?
8. Escalation path khi incident kéo dài là gì?
9. Runbook xử lý cho `traffic spike`, `provider timeout`, `HIS/EMR down`, `vector store down`, `quota exceeded` đã có chưa?
10. Sau incident có postmortem, action item, và owner theo dõi không?

#### 5.3.5 Artifact bắt buộc

1. `risk-register.md`
2. `reliability-slo-sla.md`
3. `incident-severity-matrix.md`
4. `runbook-fallback-and-recovery.md`
5. `monitoring-and-alerting-matrix.md`
6. `oncall-escalation-matrix.md`

#### 5.3.6 Bảng bắt buộc phải có

| Rủi ro | Nhóm | Impact | Likelihood | Cách phát hiện | Mitigation | Contingency | Owner | Trạng thái |
|---|---|---|---|---|---|---|---|---|
| Ví dụ: Provider timeout | Model | Cao | Vừa | Timeout rate tăng | Fallback model | Queue + retry | Architect | Mở |

| Tình huống | User impact | SLA bị ảnh hưởng | Fallback | Runbook | Owner |
|---|---|---|---|---|---|
| Traffic spike | TBD | TBD | TBD | TBD | TBD |
| Provider timeout | TBD | TBD | TBD | TBD | TBD |
| Response chậm | TBD | TBD | TBD | TBD | TBD |
| HIS/EMR down | TBD | TBD | TBD | TBD | TBD |
| Vector store down | TBD | TBD | TBD | TBD | TBD |

### 5.4 Skills & Track Phase 2

1. Mỗi người tự chấm `Business/Product`, `Infra/Data/Ops`, `AI Engineering` từ 1-5 như thế nào?
2. Điểm mạnh chung của nhóm là gì?
3. Track Phase 2 mà nhóm chọn là gì?
4. 2-3 kỹ năng nhóm cần bổ sung tiếp là gì?

### 5.5 Proposal, slide và presentation

1. Proposal sẽ theo format `slide`, `poster` hay `1-page`?
2. Người trình bày chính là ai?
3. Người xử lý Q&A là ai?
4. Zone trình bày đã được xác định chưa?
5. Slide 1 đã cover `Project + User + Problem` chưa?
6. Slide 2 đã cover `Enterprise context` chưa?
7. Slide 3 đã cover `Architecture` chưa?
8. Slide 4 đã cover `Cost anatomy` chưa?
9. Slide 5 đã cover `Optimization` chưa?
10. Slide 6 đã cover `Reliability & scaling` chưa?
11. Slide 7 đã cover `Track + next steps` chưa?
12. Project overview đã rõ problem, user, value proposition chưa?
13. Enterprise context, architecture, cost, optimization, reliability, track đã có đủ chưa?
14. Team đã chuẩn bị format `5 phút trình bày + 3 phút Q&A` chưa?

### 5.6 Peer review

1. Nhóm sẽ review ít nhất 2 đội khác như thế nào?
2. Nhóm đã chốt nguyên tắc `không tự chấm` chưa?
3. Form feedback đã có đủ `1 điểm mạnh`, `1 điểm cần cải thiện`, `1 đề xuất` chưa?
4. Ai chịu trách nhiệm tổng hợp phiếu peer review?

### 5.7 Nộp cuối ngày và Team Note

1. Đã có đủ `Worksheet 0-5` chưa?
2. Đã có `slide/poster` hoàn chỉnh chưa?
3. Đã có phiếu `peer review` chưa?
4. Đã có kết luận `Track Phase 2` chưa?
5. Team Note đã điền đủ các mục `Tên nhóm`, `Chủ đề`, `3 ràng buộc enterprise`, `Cost driver lớn nhất`, `3 chiến lược optimize`, `Fallback/reliability plan`, `Track Phase 2` chưa?

### 5.8 Số lượng user và chi phí tương ứng

Section này dùng để nối trực tiếp `quy mô sử dụng` với `chi phí`, tránh tình trạng có số user nhưng không quy đổi được thành cost, hoặc có cost nhưng không biết đang giả định trên mức tải nào.

1. Tổng số user mục tiêu của MVP là bao nhiêu?
2. Số user hoạt động theo ngày `DAU`, theo tuần `WAU`, theo tháng `MAU` là bao nhiêu?
3. Tỷ lệ user theo từng vai trò `bác sĩ`, `điều dưỡng`, `tiếp nhận` là bao nhiêu?
4. Số request trung bình trên mỗi user mỗi ngày là bao nhiêu?
5. Tỷ lệ phân bổ request theo từng use case là bao nhiêu?
6. Peak traffic theo giờ cao điểm là bao nhiêu?
7. Input token trung bình theo từng use case là bao nhiêu?
8. Output token trung bình theo từng use case là bao nhiêu?
9. Cost per request của từng use case là bao nhiêu?
10. Cost theo ngày, theo tháng ở baseline là bao nhiêu?
11. Cost tương ứng khi scale `5x` và `10x` là bao nhiêu?
12. Cost driver lớn nhất khi user tăng là gì?
13. Mốc user nào bắt đầu làm cost vượt ngân sách mục tiêu?
14. Mốc user nào bắt buộc phải thay đổi model, fallback policy, hoặc pricing plan?
15. Team đã có bảng quy đổi chuẩn `user -> request -> token -> cost` chưa?

### 5.9 Bảng quy đổi bắt buộc

| Mức tải | DAU | Request/ngày | Token/ngày | Cost/ngày | Cost/tháng | Ghi chú |
|---|---:|---:|---:|---:|---:|---|
| Baseline | TBD | TBD | TBD | TBD | TBD | |
| 5x | TBD | TBD | TBD | TBD | TBD | |
| 10x | TBD | TBD | TBD | TBD | TBD | |

---

## 6) Bàn giao giữa các role

Để tài liệu này dùng được trong thực tế, mỗi role không chỉ trả lời câu hỏi của mình, mà còn phải xuất artifact để role tiếp theo sử dụng.

### 6.1 Luồng dependency

1. `Scope & Spec` khóa use case, output, boundary trước.
2. `Kiến trúc sư hệ thống AI` dựa trên scope để chọn route, model, fallback, token budget.
3. `Hạ tầng` dựa trên route và traffic để chốt topology, sizing, cloud decision.
4. `Kinh doanh` dựa trên cost thật từ kiến trúc và hạ tầng để xây pricing và lỗ lãi.

### 6.2 Câu hỏi kiểm tra hand-off

1. Scope đã khóa đủ chưa để kiến trúc sư AI tính token theo từng route?
2. Kiến trúc sư AI đã chia cost theo use case đủ chưa để Infra sizing và Business pricing?
3. Hạ tầng đã chốt baseline và scale 5x/10x đủ chưa để Business tính margin?
4. Business có feedback ngược lại nếu plan pricing không cover được COGS không?

---

## 7) Checklist cuối cùng để viết proposal

1. Đã có scope MVP rõ ràng, không còn mơ hồ?
2. Đã có model routing và fallback chain theo từng use case?
3. Đã có bảng cost input -> output cho từng route?
4. Đã có so sánh cloud/on-prem/hybrid và quyết định cuối cùng?
5. Đã có payment packages, target user, và unit economics?
6. Đã có scenario lỗ/lãi tại 1x, 5x, 10x?
7. Đã có hạng mục tối ưu chi phí `làm ngay` và `làm sau`?
8. Đã có `risk register` với `impact`, `likelihood`, `mitigation`, `owner` chưa?
9. Đã có reliability plan cho `traffic spike`, `provider timeout`, `response chậm`, `HIS/EMR down`, `vector store down` chưa?
10. Đã có `MTTD`, `MTTR`, `RTO`, `RPO`, `incident severity`, `runbook`, `on-call escalation` chưa?
11. Đã có phần đánh giá năng lực nhóm và Track Phase 2?
12. Đã có artifact và owner cho từng role?
