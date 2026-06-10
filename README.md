# Day 10 Lab: Data Pipeline & Data Observability

**Student Email:** lemquan6688@gmail.com  
**Name:** Lê Nguyễn Minh Quân
**Student ID:** 2A202600821

---

## Mô tả (Description)

Bài Lab này thiết lập một đường ống dẫn dữ liệu ETL (Extract, Validate, Transform, Load) tự động bằng Python và Pandas nhằm xử lý và chuẩn hóa dữ liệu sản phẩm từ file JSON sang CSV. Qua đó, tiến hành thử nghiệm "Stress Test" để đánh giá mức độ ảnh hưởng của chất lượng dữ liệu đầu vào (Data Quality) đối với câu trả lời của AI Agent (mô phỏng hệ thống RAG).

Các bước chính đã thực hiện:
1. **Extract**: Đọc dữ liệu thô từ file `raw_data.json` có xử lý lỗi.
2. **Validate**: Loại bỏ các bản ghi không hợp lệ (giá trị price <= 0 hoặc category rỗng).
3. **Transform**: Áp dụng mức giảm giá 10% cho tất cả sản phẩm, chuẩn hóa danh mục sang dạng Title Case, và thêm cột dấu thời gian `processed_at`.
4. **Load**: Xuất dữ liệu sạch ra file `processed_data.csv`.
5. **Stress Test**: Tạo dữ liệu lỗi (`garbage_data.csv`) và chạy mô phỏng AI Agent để phân tích lỗi.

---

## Cách chạy (How to Run)

### Cài đặt thư viện (Prerequisites)
Đảm bảo bạn đã kích hoạt môi trường ảo `venv` và cài đặt các thư viện cần thiết:
```bash
source venv/bin/activate
pip install pandas pytest
```

### Chạy ETL Pipeline
Để thực hiện quá trình xử lý dữ liệu và tạo file `processed_data.csv`:
```bash
python solution.py
```

### Chạy sinh dữ liệu lỗi (Garbage Data)
Để tạo ra file chứa dữ liệu lỗi cho Stress Test:
```bash
python generate_garbage.py
```

### Chạy Agent Simulation (Stress Test)
Chạy thử nghiệm RAG Agent để xem sự khác biệt giữa dữ liệu sạch và dữ liệu lỗi:
```bash
python agent_simulation.py
```

### Chạy Unit Test cục bộ
Để chạy kiểm tra tự động tất cả các bài test chấm điểm cục bộ:
```bash
pytest tests/test_autograder.py -v
```

---

## Cấu trúc thư mục

```
├── solution.py              # Script chạy ETL Pipeline
├── processed_data.csv       # File đầu ra sau khi làm sạch
├── garbage_data.csv         # File dữ liệu lỗi phục vụ Stress Test
├── agent_simulation.py      # Script mô phỏng AI Agent
├── generate_garbage.py      # Script tạo dữ liệu lỗi
├── experiment_report.md     # Báo cáo kết quả thử nghiệm
└── README.md                # File hướng dẫn này
```

---

## Kết quả (Results)

* **Tổng số bản ghi đầu vào**: 5 bản ghi.
* **Số bản ghi hợp lệ được giữ lại**: 3 bản ghi (Laptop, Chair, Monitor).
* **Số bản ghi lỗi bị loại bỏ**: 2 bản ghi (Mystery Box vì giá < 0, Phone vì category rỗng).
* **Quan sát AI Agent**: Agent trả lời chính xác khi dùng dữ liệu sạch (`Laptop at $1200`), và bị đánh lừa đưa ra câu trả lời sai lệch khi dùng dữ liệu rác (`Nuclear Reactor at $999999`).
