# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** 2A202600821
**Name:** Lê Nguyễn Minh Quân
**Date:** 2026-06-10

---

## 1. Ket qua thi nghiem

Chay `agent_simulation.py` voi 2 bo du lieu va ghi lai ket qua:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | Agent: Based on my data, the best choice is Laptop at $1200. | 10 | Trả về chính xác sản phẩm điện tử có giá cao nhất sau khi dữ liệu đã được làm sạch và chuẩn hóa. |
| Garbage Data (`garbage_data.csv`) | Agent: Based on my data, the best choice is Nuclear Reactor at $999999. | 1 | Trả về một sản phẩm cực kỳ vô lý và đắt tiền do dữ liệu rác chứa giá trị dị biệt cực lớn (extreme outlier). |

---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

Khi sử dụng Garbage Data, RAG Agent đã đưa ra câu trả lời hoàn toàn sai lệch và chọn 'Nuclear Reactor' có giá $999999 làm sản phẩm điện tử tốt nhất. Nguyên nhân chính là do dữ liệu rác chứa nhiều lỗi chất lượng nghiêm trọng:
1. **Dữ liệu dị biệt cực đại (Extreme Outliers)**: Giá trị $999999 của 'Nuclear Reactor' là quá lớn và phi thực tế so với các thiết bị điện tử thông thường, nhưng thuật toán tìm giá lớn nhất vẫn chọn nó vì nó mang nhãn category là 'electronics' nhưng không được lọc bỏ trước khi đưa vào tri thức của Agent.
2. **Sai kiểu dữ liệu (Wrong Data Types)**: Sản phẩm 'Broken Chair' có giá là chuỗi chữ 'ten dollars' thay vì dạng số, làm gãy các phép toán so sánh số học của hệ thống hoặc gây lỗi ép kiểu trong gấu trúc (pandas).
3. **Trùng lặp ID (Duplicate IDs)**: ID bị lặp lại (như ID=1) gây mất tính toàn vẹn của dữ liệu và nhầm lẫn khi truy xuất bản ghi trong cơ sở dữ liệu.
4. **Giá trị rỗng (Null values)**: Dữ liệu thiếu thông tin (Null ở category hoặc id như sản phẩm 'Ghost Item') làm suy giảm độ chính xác của quá trình lọc dữ liệu, khiến Agent không thể nhận dạng đúng danh mục sản phẩm hoặc bị lỗi khi xử lý chuỗi.

---

## 3. Ket luan

**Quality Data > Quality Prompt?** Đồng ý.

Dù prompt có tối ưu hay thông minh đến đâu thì nếu dữ liệu đầu vào bị "nhiễm độc" (outliers, null, sai định dạng), AI Agent vẫn sẽ đưa ra câu trả lời sai lệch hoặc bị crash theo nguyên tắc "Garbage In, Garbage Out". Do đó, việc xây dựng một đường ống dẫn dữ liệu (ETL pipeline) có cơ chế kiểm duyệt chất lượng nghiêm ngặt (data quality & validation) có vai trò quyết định đến độ chính xác và tính tin cậy của hệ thống AI hơn là chỉ tập trung tối ưu hóa prompt.
