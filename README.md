# timeseries_lab1
# Phân Tích Chuỗi Thời Gian & Mô Hình Hóa ARIMA (Statsmodels Lab)

Dự án thực hành môn học về phân tích chuỗi thời gian, kiểm định tính dừng (ADF Test), phân tích cấu trúc tự tương quan (ACF/PACF) và xây dựng mô hình dự báo ARIMA trên dữ liệu thực tế doanh số bán dược phẩm điều trị đái tháo đường (Dataset a10).

---

## 1. Thông Tin Nhóm & Phân Công Nhiệm Vụ

* **Môn học:** Phân tích Dữ liệu Chuỗi Thời gian
* **Nhóm thực hiện:** Nhóm 6 người

| STT | Họ và Tên | MSSV | Vai trò & Nhiệm vụ chính | Sản phẩm bàn giao |
| :---: | :--- | :---: | :--- | :--- |
| 1 | Đinh Bùi Trung Kiên | 001205054267 | **Thành viên:** Quản trị Git/Repo, thiết lập cấu trúc, thực hiện Bài 1 (EDA) | `01_eda.ipynb`, `docs/lab01_eda.md` |
| 2 | [Tên TV 2] | [MSSV] | **Thành viên:** Thực hiện Bài 2 (Kiểm định ADF trên chuỗi gốc) | `02_adf_test.ipynb`, `docs/lab02_adf.md` |
| 3 | [Tên TV 3] | [MSSV] | **Thành viên:** Thực hiện Bài 3 (Sai phân & Kiểm định lại ADF)[cite: 2] | `03_differencing.ipynb`, `docs/lab03_differencing.md` |
| 4 | [Tên TV 4] | [MSSV] | **Thành viên:** Thực hiện Bài 4 (Phân tích biểu đồ ACF & PACF)[cite: 2] | `04_acf_pacf.ipynb`, `docs/lab04_acf_pacf.md` |
| 5 | [Tên TV 5] | [MSSV] | **Thành viên:** Thực hiện Bài 5A (Chia tập, Fit mô hình ARIMA & Dự báo) | `05_arima_modeling.ipynb`, `reports/figures/` |
| 6 | [Tên TV 6] | [MSSV] | **Thành viên:** Thực hiện Bài 5B (Chẩn đoán phần dư & Viết báo cáo 9 câu hỏi) | `docs/lab05_arima.md` |

---

## 2. Cấu Trúc Thư Mục Dự Án

```text
timeseries_lab1/
├── README.md                      # Báo cáo tổng quan dự án và hướng dẫn
├── requirements.txt               # Danh sách thư viện phụ thuộc
├── .gitignore                     # Bỏ qua file rác, virtual environment
│
├── data/
│   └── raw/                       # Chứa dữ liệu gốc a10.csv
│        ├── a10.csv              
│        ├── save_data_file.py  
│
├── notebooks/                     # Toàn bộ mã nguồn thực thi dạng Jupyter Notebook
│   ├── 01_eda.ipynb               # Bài 1: Khám phá dữ liệu (Trend, Seasonality)[cite: 2]
│   ├── 02_adf_test.ipynb          # Bài 2: Kiểm định ADF chuỗi gốc[cite: 2]
│   ├── 03_differencing.ipynb      # Bài 3: Sai phân bậc 1 và kiểm định lại tính dừng[cite: 2]
│   ├── 04_acf_pacf.ipynb          # Bài 4: Vẽ và phân tích ACF/PACF[cite: 2]
│   └── 05_arima_modeling.ipynb    # Bài 5: Huấn luyện ARIMA, chẩn đoán & đánh giá
│
├── docs/                          # Báo cáo chi tiết và giải thích lý thuyết dạng Markdown
│   ├── lab01_eda.md               # Diễn giải kết quả phân tích chuỗi thời gian[cite: 2]
│   ├── lab02_adf.md               # Giải thích giả thuyết H0, p-value của kiểm định ADF[cite: 2]
│   ├── lab03_differencing.md      # Lập luận lựa chọn bậc sai phân d[cite: 2]
│   ├── lab04_acf_pacf.md          # Lập luận lựa chọn các bậc trễ p và q[cite: 2]
│   └── lab05_arima.md             # Trả lời chi tiết 9 câu hỏi đánh giá mô hình[cite: 3]
│
└── reports/
    └── figures/                   # Toàn bộ hình ảnh, biểu đồ trực quan hóa được xuất ra
