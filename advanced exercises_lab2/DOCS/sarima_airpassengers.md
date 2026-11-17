# Bài tập nâng cao: Mô hình SARIMA trên dataset AirPassengers

## 1. Giới thiệu

Tài liệu này mô tả quy trình xây dựng mô hình **SARIMA** để dự báo số hành khách hàng không theo tháng (Dataset 2 – AirPassengers), và so sánh với mô hình **ARIMA** thông thường.

| Thông tin | Giá trị |
|---|---|
| Dataset | AirPassengers – Monthly Airline Passengers |
| Giai đoạn | 1949 – 1960 |
| Số quan sát | 144 |
| Tần suất | Hàng tháng (Monthly) |
| Chu kỳ mùa vụ | s = 12 |
| Nguồn | https://raw.githubusercontent.com/jbrownlee/Datasets/master/airline-passengers.csv |

**Tham số gợi ý của đề bài:** s = 12, d = 1 (loại trend), D = 1 (loại mùa vụ).

## 2. Cấu trúc thư mục

```
notebooks/
├── 01_ve_chuoi_thoi_gian.ipynb
├── 02_adf_chuoi_goc.ipynb
├── 03_sai_phan.ipynb
├── 04_acf_pacf.ipynb
├── 05_fit_sarima.ipynb
├── 06_forecast_danh_gia.ipynb
└── 07_so_sanh_arima.ipynb
sarima_airpassengers.py        # bản gộp toàn bộ 7 bước trong một file
```

Mỗi notebook chạy độc lập: phần đầu đã có sẵn đoạn tải dữ liệu và chia train/test.

## 3. Yêu cầu môi trường

```bash
pip install pandas numpy matplotlib statsmodels scikit-learn
```

Cần kết nối internet để tải dataset từ GitHub.

## 4. Quy trình thực hiện

### Bước 1. Vẽ chuỗi thời gian và nhận diện mùa vụ

*Notebook: `01_ve_chuoi_thoi_gian.ipynb`*

- Vẽ chuỗi gốc và phân rã (`seasonal_decompose`) thành trend, seasonal, residual.
- Chuỗi có xu hướng tăng và mùa vụ lặp lại mỗi 12 tháng.
- Biên độ mùa vụ tăng dần theo thời gian nên đây là mùa vụ **nhân** (multiplicative).

### Bước 2. Kiểm định ADF trên chuỗi gốc

*Notebook: `02_adf_chuoi_goc.ipynb`*

- Giả thuyết H0: chuỗi có nghiệm đơn vị (không dừng).
- Nếu p-value > 0.05 thì không bác bỏ H0, chuỗi **không dừng**.
- Với AirPassengers, chuỗi gốc không dừng nên cần sai phân.

### Bước 3. Sai phân thường và sai phân mùa vụ

*Notebook: `03_sai_phan.ipynb`*

- Sai phân thường `diff(1)` để loại trend (d = 1).
- Sai phân mùa vụ `diff(12)` để loại mùa vụ (D = 1, s = 12).
- Chạy lại ADF trên chuỗi đã sai phân; kỳ vọng p-value < 0.05, tức chuỗi đã **dừng**.

### Bước 4. ACF / PACF và xác định (p, q, P, Q)

*Notebook: `04_acf_pacf.ipynb`*

Vẽ ACF và PACF của chuỗi đã sai phân (d = 1, D = 1) với 36 độ trễ.

| Quan sát | Kết luận |
|---|---|
| ACF có spike rõ tại lag 1 | q = 1 |
| ACF có spike rõ tại lag 12 | Q = 1 |
| PACF giảm dần, không cắt rõ | p = 0, P = 0 |

Mô hình đề xuất: **SARIMA(0,1,1)(0,1,1)₁₂**, còn gọi là mô hình "airline".

### Bước 5. Fit mô hình SARIMA

*Notebook: `05_fit_sarima.ipynb`*

- Chia dữ liệu: **train** là 120 tháng đầu, **test** là 24 tháng cuối.
- Fit bằng `statsmodels.tsa.statespace.sarimax.SARIMAX`:

```python
SARIMAX(train,
        order=(0, 1, 1),
        seasonal_order=(0, 1, 1, 12),
        enforce_stationarity=False,
        enforce_invertibility=False).fit(disp=False)
```

- Kiểm tra phần dư bằng `plot_diagnostics()`: phần dư nên giống nhiễu trắng (không tự tương quan, phân phối gần chuẩn).

### Bước 6. Dự báo và đánh giá

*Notebook: `06_forecast_danh_gia.ipynb`*

- Dự báo 24 tháng của tập test, kèm khoảng tin cậy.
- Đánh giá bằng hai thước đo:

$$MAE = \frac{1}{n}\sum_{t=1}^{n}|y_t-\hat y_t| \qquad RMSE = \sqrt{\frac{1}{n}\sum_{t=1}^{n}(y_t-\hat y_t)^2}$$

### Bước 7. So sánh với ARIMA

*Notebook: `07_so_sanh_arima.ipynb`*

- Chọn ARIMA(p,1,q) với p, q ∈ {0,…,3} có **AIC** thấp nhất trên tập train.
- ARIMA không có thành phần mùa vụ nên dự báo gần như một đường xu hướng.
- So sánh MAE, RMSE của hai mô hình trên cùng tập test và vẽ biểu đồ dự báo chung.

| Mô hình | MAE | RMSE |
|---|---|---|
| ARIMA(p,1,q) | *(điền sau khi chạy)* | *(điền sau khi chạy)* |
| SARIMA(0,1,1)(0,1,1)₁₂ | *(điền sau khi chạy)* | *(điền sau khi chạy)* |

## 5. Nhận xét kỳ vọng

- SARIMA bám sát các đỉnh và đáy theo mùa nên sai số thấp hơn ARIMA rõ rệt.
- ARIMA chỉ nắm được xu hướng, bỏ qua mùa vụ.
- Kết luận chính: với dữ liệu có mùa vụ mạnh như AirPassengers, cần dùng mô hình có thành phần mùa vụ.

## 6. Hướng cải thiện

- Lấy **log** chuỗi trước khi fit để ổn định phương sai (do mùa vụ nhân), sau đó biến đổi ngược khi đánh giá.
- Thử lưới tham số (p, q, P, Q) rộng hơn và chọn theo AIC/BIC.
- Dùng cross-validation theo thời gian (rolling origin) thay vì một lần chia train/test.

## 7. Cách chạy

1. Cài thư viện như mục 3.
2. Mở lần lượt các notebook trong `notebooks/` (hoặc chạy `python sarima_airpassengers.py` để chạy toàn bộ).
3. Điền kết quả MAE, RMSE vào bảng ở Bước 7.
