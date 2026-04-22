# MVP Work Breakdown & Estimate

## 1) Giả định nguồn lực

- Team core: 4-6 người
- Thời lượng: 14-16 tuần
- Đơn vị estimate: person-day (PD)

---

## 2) Work Breakdown Structure (WBS)

| Mã | Hạng mục | Kết quả đầu ra | Estimate (PD) | Owner |
|---|---|---|---:|---|
| W1 | Product discovery & scope lock | Use case matrix, success KPI, scope freeze | 8 | Product Lead |
| W2 | Data policy & access model | RBAC matrix, policy log retention, risk classification | 10 | Architect + Reliability |
| W3 | Data pipeline & retrieval setup | Ingestion tài liệu, index, retrieval baseline | 16 | Backend/AI Eng |
| W4 | Core assistant service | API chat, orchestration, prompt/routing | 18 | Backend/AI Eng |
| W5 | Use case module: admin Q&A | Prompt pack + test cases | 8 | AI Eng |
| W6 | Use case module: SOP Q&A | Evidence-linked response + quality rule | 10 | AI Eng |
| W7 | Use case module: record summary | Summary template + source citation | 12 | AI Eng + Clinical Advisor |
| W8 | Human review workflow | Queue review, escalation state, approve/reject | 10 | Backend + Reliability |
| W9 | Observability & audit | Metrics, logs, dashboard, audit trail | 10 | Reliability Lead |
| W10 | Security hardening | PII masking, encryption config, secret/key policy | 8 | Architect |
| W11 | Pilot enablement | UAT checklist, user guide, training | 8 | Product + Clinical Advisor |
| W12 | Rollout & optimization | Cache, tiering, cost tuning, incident drill | 12 | Cost + Reliability |
| **Tổng** |  |  | **130 PD** |  |

---

## 3) Timeline mapping

| Giai đoạn | Tuần | Hạng mục chính | Tổng PD |
|---|---|---|---:|
| Phase 0 - Prep | 1-2 | W1, W2 (một phần), W10 (một phần) | 20 |
| Phase 1 - Build | 3-8 | W3, W4, W5, W6, W7, W8 | 74 |
| Phase 2 - Pilot | 9-12 | W9, W10, W11 | 26 |
| Phase 3 - Rollout | 13-16 | W12 + ổn định hệ thống | 10 |
| **Tổng** | 1-16 |  | **130 PD** |

---

## 4) Cost estimate theo công việc (one-time)

Giả định blended rate: `3,200,000 VND / PD`

- Tổng one-time cost ước lượng: `130 x 3,200,000 = 416,000,000 VND`
- Làm tròn cho budget request: `420,000,000 VND`

---

## 5) Critical path

1. Chốt policy dữ liệu + phân quyền (W2)
2. Dựng retrieval pipeline ổn định (W3)
3. Hoàn thành service orchestration (W4)
4. Chốt module summary + SOP Q&A (W6, W7)
5. Human review + observability (W8, W9)
6. Pilot UAT và training (W11)

---

## 6) Risk buffer đề xuất

- Buffer tiến độ: +15% cho tích hợp HIS/EMR
- Buffer chi phí: +10% cho thay đổi policy hoặc tăng log retention
- Buffer chất lượng: giữ 2 tuần cuối cho tuning và incident drill
