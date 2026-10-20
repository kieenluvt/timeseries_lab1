# Báo cáo Thực hành - Bài 3: Sai phân bậc 1 và Kiểm định lại tính dừng

> Notebook thực thi: [`notebooks/03_differencing.ipynb`](../notebooks/03_differencing.ipynb)

## 1. Mục tiêu
- Thực hiện kỹ thuật sai phân bậc 1 ($d=1$) trên chuỗi thời gian doanh số `a10` để triệt tiêu xu hướng tăng dài hạn (Trend).
- Chạy lại kiểm định Augmented Dickey-Fuller (ADF) trên chuỗi sau sai phân để đánh giá mức độ biến đổi về tính dừng (Stationary).
- Kết luận và xác định giá trị tham số $d$ tối ưu phục vụ cho bước xây dựng mô hình ARIMA ở các bài sau.
## 2. Kết quả thực thi

### 2.1. Chuỗi dữ liệu sau sai phân bậc 1
- Công thức thực hiện: $Y'_t = Y_t - Y_{t-1}$
- Mẫu dữ liệu 5 dòng đầu tiên sau khi loại bỏ giá trị khuyết (`NaN`):
  ```text
  1   -0.345700
  2    0.071330
  3    0.358782
  4   -0.045134
  5    0.740502
  Name: value, dtype: float64
## Trả lời câu hỏi

###  Sau sai phân, chuỗi đã dừng chưa?
 Về mặt xu hướng (Trend): Đã dừng. Quan sát biểu đồ sau sai phân bậc 1, xu hướng tăng dài hạn đã bị triệt tiêu hoàn toàn, chuỗi dao động ổn định xoay quanh mức trung bình bằng 0.   
 
 Về mặt thống kê (ADF Test): Chỉ số $p\text{-value} = 0.1167 > 0.05$ (và $\vert{}\text{ADF Statistic}\vert{} = 2.4952 < 2.8771$ ở mức ý nghĩa 5%). Chuỗi chưa dừng tuyệt đối về mặt thống kê do bộ dữ liệu a10 chứa tính mùa vụ (Seasonality) 12 tháng rất mạnh (các đỉnh nhọn lặp lại hàng năm vẫn thể hiện rõ trên đồ thị)

### Giá trị $d$ trong ARIMA là bao nhiêu?
Giá trị chốt: $d = 1$. 
Sai phân bậc 1 ($d=1$) đã đủ để loại bỏ hoàn toàn xu hướng tăng của dữ liệu. Không chọn $d = 2$ để tránh hiện tượng over-differencing (sai phân quá đà), làm gia tăng độ nhiễu và mất thông tin chuỗi thời gian