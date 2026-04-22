# Profit and Loss Scenarios - Hospital Clinical Support Assistant

## Assumptions

- One-time implementation cost: `420,000,000 VND`
- Baseline OPEX/customer/month: `90,000,000 VND`
- Dùng giá gói để ước tính doanh thu theo 3 mức khách hàng
- Các số dưới đây là estimate cho proposal, chưa phải forecast tài chính chính thức

## Scenario table

| Scenario | Customer mix assumption | Monthly revenue (VND) | Monthly COGS (VND) | Monthly gross profit (VND) | Notes |
|---|---|---:|---:|---:|---|
| 1 customer | 1 x Starter | 135,000,000 | 96,000,000 | 39,000,000 | Lãi gộp dương nhưng chưa hoàn vốn implementation ngay |
| 5 customers | 2 x Starter, 2 x Growth, 1 x Enterprise | 1,210,000,000 | 1,020,000,000 | 190,000,000 | Bắt đầu hấp thụ tốt chi phí nền tảng và support |
| 10 customers | 3 x Starter, 4 x Growth, 3 x Enterprise | 3,145,000,000 | 2,820,000,000 | 325,000,000 | Cần quản trị chặt token cost và delivery scope |

## Break-even interpretation

- Nếu chỉ có 1 khách hàng Starter, cần khoảng `420,000,000 / 39,000,000 ~= 10.8 tháng` gross profit để bù chi phí implementation
- Nếu mix khách hàng tốt hơn hoặc bán `Growth` sớm hơn, thời gian hoàn vốn ngắn hơn
- Nếu human review tăng cao hoặc tài liệu retrieval kém, thời gian hoàn vốn sẽ xấu đi đáng kể

## Main cost risks

- Token/API tăng nhanh theo usage
- Human review vượt giả định
- Setup integration và governance phát sinh thêm effort không tính trước
