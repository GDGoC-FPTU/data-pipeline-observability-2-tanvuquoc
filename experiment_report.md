# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** 2A202600910
**Name:** Vu Quoc Tan
**Date:** 10/06/2026

---

## 1. Ket qua thi nghiem

Chay `agent_simulation.py` voi 2 bo du lieu va ghi lai ket qua:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | I'm choking on the data! ([Errno 2] No such file or directory: '../exercise-etl-automation/solution-code/processed_data.csv') | | |
| Garbage Data (`garbage_data.csv`) | Based on my data, the best choice is Nuclear Reactor at $999999. | | |

---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

(Viet nhan xet cua ban o day — it nhat 50 tu)

(Hay phan tich cac van de nhu Duplicate IDs, wrong data types, outliers, null values
va giai thich tai sao chung anh huong den ket qua cua Agent.)
Agent có thể trả lời sai khi dữ liệu đầu vào bị lỗi vì Agent không tự biết dữ liệu nào là đáng tin nếu pipeline chưa kiểm tra trước. Ví dụ, nếu có duplicate IDs, cùng một `id` xuất hiện nhiều lần với nội dung khác nhau, Agent có thể truy xuất nhầm bản cũ hoặc bản không chính xác. Nếu dữ liệu sai kiểu, như `price` là chuỗi thay vì số, hệ thống có thể lọc sai hoặc không phát hiện được outlier. Các giá trị bất thường như `price = 99999` hoặc `price < 0` cũng làm Agent hiểu sai ngữ cảnh, dẫn đến câu trả lời không hợp lý. Ngoài ra, null values hoặc nội dung rỗng khiến Agent thiếu thông tin để grounding, từ đó dễ tạo ra câu trả lời “nghe có vẻ đúng” nhưng thực chất không dựa trên dữ liệu sạch. Vì vậy, nếu không có bước cleaning và validation, chất lượng prompt tốt cũng không đảm bảo Agent trả lời đúng.
---

## 3. Ket luan

**Quality Data > Quality Prompt?** (Dong y hay khong? Giai thich ngan gon.)


Quality Data > Quality Prompt** là nhận định đúng. Prompt tốt chỉ giúp Agent hiểu nhiệm vụ rõ hơn, nhưng nếu dữ liệu đầu vào đã sai, cũ, trùng lặp hoặc chứa thông tin nhạy cảm thì Agent vẫn có thể đưa ra kết quả sai. Trong bài lab này, việc xóa PII, mask email, loại duplicate và bỏ outlier giúp dữ liệu an toàn và đáng tin hơn trước khi đưa vào hệ thống AI. Vì vậy, muốn Agent hoạt động ổn định thì cần ưu tiên xây dựng data pipeline sạch, có kiểm tra chất lượng và có khả năng phát hiện lỗi trước khi tối ưu prompt.
