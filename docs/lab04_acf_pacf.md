# Bài 4: Phân tích ACF & PACF

> Notebook thực thi: [`notebooks/04_acf_pacf.ipynb`](../notebooks/04_acf_pacf.ipynb)

## 1. Mục tiêu

Sử dụng biểu đồ ACF và PACF của chuỗi sau sai phân bậc 1 để nhận diện dấu hiệu **cutoff** hoặc **tail-off**, từ đó đề xuất các bộ tham số ARIMA cho Bài 5.

## 2. Chuỗi đầu vào

Theo Bài 3, sử dụng chuỗi sai phân bậc 1 và chọn `d = 1`. Kết quả ADF của Bài 3 có `p-value = 0.1167 > 0.05`, nên chưa đủ bằng chứng thống kê để kết luận chuỗi đã dừng ở mức ý nghĩa 5%.

## 3. Phân tích ACF/PACF

![ACF và PACF của sai phân bậc 1](../reports/figures/04_acf_pacf_diff1.png)

Một số giá trị ACF:

| Lag |    ACF |
| --: | -----: |
|   1 | -0.365 |
|   2 |  0.006 |
|  12 |  0.757 |
|  24 |  0.615 |
|  36 |  0.539 |

- **Lag 1:** ACF âm tương đối rõ, có thể gợi ý thành phần MA bậc thấp.
- **Lag 12, 24 và 36:** ACF còn cao tại các bội số của 12, cho thấy dấu hiệu mùa vụ theo năm.
- **Cutoff/tail-off:** Thông thường, AR(p) có PACF cắt cụt và ACF suy giảm dần; MA(q) có ACF cắt cụt và PACF suy giảm dần. Trong dữ liệu này, mẫu hình không cắt cụt rõ ràng, nên cần xem các mô hình đề xuất là ứng viên ban đầu.

### Cách đọc cutoff và tail-off

| Mẫu hình lý thuyết | Dấu hiệu thường gặp |
|---|---|
| AR(p) | PACF cắt cụt quanh lag p; ACF suy giảm dần |
| MA(q) | ACF cắt cụt quanh lag q; PACF suy giảm dần |
| ARMA(p,q) | Cả ACF và PACF thường suy giảm dần |

## 4. Đề xuất tham số cho Bài 5

| Mô hình        | Mục đích thử nghiệm       |
| -------------- | ------------------------- |
| `ARIMA(1,1,0)` | Kiểm tra thành phần AR(1) |
| `ARIMA(0,1,1)` | Kiểm tra thành phần MA(1) |
| `ARIMA(1,1,1)` | Kết hợp AR(1) và MA(1)    |

Trong các cấu hình này, `d=1` tuân theo giá trị đã chốt ở Bài 3. Đây là bậc sai phân ban đầu để thử mô hình, không phải khẳng định rằng chuỗi đã dừng hoàn toàn: ADF có `p-value = 0.1167`, còn ACF cho thấy tự tương quan mùa vụ đáng kể ở các lag 12 và 24.

Đặc biệt, các đỉnh ACF tại lag 12 và 24 cho thấy cần lưu ý mùa vụ. Nếu mô hình ARIMA không mùa vụ cho phần dư còn tự tương quan theo chu kỳ 12, nhóm nên cân nhắc mô hình SARIMA hoặc một cách xử lý mùa vụ phù hợp.

## 5. Kết luận

Bài 4 đề xuất ba mô hình trên để so sánh ở Bài 5. Do ACF có các đỉnh đáng kể tại lag 12 và các bội số của 12, cần kiểm tra phần dư để xác định liệu ARIMA không mùa vụ có mô tả dữ liệu đủ tốt hay không. Việc chọn mô hình cuối cùng dựa trên AIC/BIC, sai số dự báo và chẩn đoán phần dư ở Bài 5.
