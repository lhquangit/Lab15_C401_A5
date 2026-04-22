# Fallback Policy

## 1. Triggers
- **Timeout**: Response time > 8 seconds.
- **Provider Error**: HTTP 5xx or API quota exceeded.
- **Quality Gate**: Confidence score from Judge model < 0.7.
- **Safety Filter**: PII detected in output (before release).

## 2. Fallback Chain
- **Primary**: GPT-4o / Claude 3.5 Sonnet.
- **Secondary (Tier 1)**: GPT-4o-mini (Cost-effective, high speed).
- **Secondary (Tier 2)**: Claude 3 Haiku (Alternative provider).
- **Final Fallback**: "Hệ thống đang bận, vui lòng thử lại hoặc liên hệ quản trị viên."

## 3. Human-in-the-Loop (HITL)
- Every "Record Summary" for critical departments (e.g., ICU, Surgery) must be flagged for human review.
- Any SOP Q&A with confidence < 0.5 must be routed to a Human Supervisor.
