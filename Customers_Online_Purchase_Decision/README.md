## Tổng quan dự án

### 1. Tổng quan bài toán nghiệp vụ
Trong ngành Thương mại điện tử, việc nhận diện sớm khách hàng tiềm năng có ý định mua hàng giúp doanh nghiệp tối ưu hóa chi phí tiếp thị và tăng tỷ lệ chuyển đổi. 

* **Mục tiêu kinh doanh:** Dự đoán xem một phiên truy cập (Session) của khách hàng trên website có tạo ra doanh thu (`Revenue = True`) hay không.
* **Mục tiêu kỹ thuật:** Xây dựng mô hình phân loại **Gradient Boosting Classifier** đạt độ chính xác cao và xác định các chỉ số hành vi (Features) quyết định đến hành vi mua hàng.

---

### 2. Từ điển Dữ liệu & Quy tắc Nghiệp vụ (Data Dictionary & Business Rules)

#### Bảng chỉ số đặc trưng (Key Feature Metrics):
| Tên đặc trưng (Feature) | Loại dữ liệu | Ý nghĩa nghiệp vụ |
| :--- | :--- | :--- |
| `PageValues` | Continuous | Giá trị trung bình của các trang web mà người dùng đã truy cập trước khi hoàn tất giao dịch. |
| `ProductRelated` | Discrete | Số lượng trang chi tiết sản phẩm mà khách hàng đã xem trong phiên. |
| `ProductRelated_Duration` | Continuous | Tổng thời gian (giây) khách hàng xem các trang sản phẩm. |
| `BounceRates` | Continuous | Tỷ lệ khách hàng truy cập vào trang và thoát ra ngay mà không thực hiện thêm thao tác. |
| `ExitRates` | Continuous | Tỷ lệ lượt xem trang cuối cùng trong một phiên truy cập. |
| `Revenue` **(Target)** | Binary (0/1) | **Biến mục tiêu:** `1` (Khách chốt đơn thành công), `0` (Khách không mua). |

#### Luật nghiệp vụ:
* **BR-01 (Định dạng & Tiền xử lý):** Dữ liệu logic chuyển sang nhị phân (Binary), dữ liệu phân loại áp dụng `One-Hot Encoding`, dữ liệu số chuẩn hóa qua `StandardScaler`.
* **BR-02 (Tỷ lệ phân chia dữ liệu):** Dữ liệu phân tích được chia theo tỷ lệ **80% Training** và **20% Testing** để đảm bảo tính tổng quát hóa của mô hình dự báo.
* **BR-03 (Trọng số ảnh hưởng):** Chỉ số `PageValues` được xác định là yếu tố tiên quyết (đóng góp trên 70% vào quyết định của mô hình) ảnh hưởng tới hành vi chốt đơn.

---

### 3. Quy trình Xử lý Dữ liệu End-to-End (Data Pipeline Flow)

[Dữ liệu thô: Kaggle]
│
▼
[Tiền xử lý & Mã hóa] ──► (Clean missing values, One-Hot Encoding, StandardScaler)
│
▼
[Lựa chọn Đặc trưng]  ──► (Áp dụng SelectKBest + Mutual Information ➔ Lọc Top 5 biến)
│
▼
[Huấn luyện Mô hình]   ──► (Gradient Boosting Classifier)
│
▼
[Đánh giá & Xuất Insights] ──► (Confusion Matrix, Classification Report, Feature Importance)


---

### 4. Kết quả Mô hình & Đánh giá Hiệu năng (Model Performance)

Mô hình **Gradient Boosting Classifier** đạt các chỉ số kinh doanh chính trên tập kiểm tra:

* **Accuracy:** **88.3%**
* **Ma trận nhầm lẫn (Confusion Matrix):**
  * **True Negative (1,873):** Dự đoán chính xác 1,873 khách hàng **Không mua**.
  * **True Positive (305):** Dự đoán chính xác 305 khách hàng **Có mua**.
  * **False Positive (182):** Dự đoán nhầm 182 khách hàng không mua thành có mua (Lãng phí chi phí Remarketing nhẹ).
  * **False Negative (106):** Bỏ sót 106 khách hàng có nhu cầu mua thực sự (Bỏ lỡ cơ hội bán hàng).

---

### 5. Đề xuất Giải pháp 

1. **Tối ưu hóa các Trang có PageValues Cao:** 
   * Đưa các nút kêu gọi hành động (CTA), mã giảm giá hoặc ưu đãi giới hạn thời gian (Flash Sale) vào các trang có chỉ số `PageValues` cao để thúc đẩy hành vi chốt đơn.
2. **Chiến lược giữ chân Khách hàng (Giảm Bounce/Exit Rates):**
   * Tăng tốc độ tải trang sản phẩm và gợi ý sản phẩm liên quan (Recommendation System) dựa trên thời gian khách xem `ProductRelated_Duration`.
3. **Cải tiến kỹ thuật cho mô hình:**
   * **Vấn đề:** Dữ liệu có sự mất cân bằng lớn giữa nhóm Mua (411) và Không mua (2,055).
   * **Đề xuất:** Áp dụng kỹ thuật cân bằng dữ liệu **SMOTE** hoặc điều chỉnh **Class Weight** ở pha tiếp theo nhằm tăng chỉ số **Precision (hiện tại 63%)** cho nhóm người mua hàng.
