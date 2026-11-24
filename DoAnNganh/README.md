# Hệ Thống Đánh Giá KPI & Cảnh Báo Nhu Cầu Tuyển Dụng (Odoo 18 Custom Module: `hrm_kpi`)

> **Vai trò:** Business Analyst / System Analyst  
> **Nền tảng triển khai:** Odoo 18 ERP (Tích hợp trực tiếp với module `HR` & `Recruitment`)[cite: 191, 201]  
> **Đề tài:** Đồ án ngành Hệ thống Thông tin Quản lý – Đại học Mở TP.HCM (2026)[cite: 178]

---

##  1. Bối Cảnh & Bài Toán Nghiệp Vụ 

### Vấn đề hiện trạng:
- Quy trình đánh giá KPI nhân viên thực hiện rời rạc trên Excel/Google Sheets, dễ sai sót, thiếu tính minh bạch và khó truy vết lịch sử[cite: 181, 187].
- Hoạt động hoạch định nhân sự bị động; bộ phận HR chỉ phát sinh nhu cầu tuyển dụng khi đã thiếu hụt nhân sự cục bộ thay vì dự báo dựa trên dữ liệu hiệu suất và tỷ lệ biến động[cite: 187].

### Giải pháp đề xuất:
Xây dựng phân hệ tùy chỉnh **`hrm_kpi`** trên Odoo 18 nhằm chuẩn hóa toàn bộ vòng đời đánh giá KPI (tạo, tính điểm tự động, phê duyệt, cảnh báo rủi ro) và tự động hóa việc phân tích, đề xuất định biên tuyển dụng theo từng phòng ban/vị trí công việc[cite: 189, 190].

---

##  2. Phân Tích Tác Nhân & Phạm Vi Nghiệp Vụ 
Hệ thống phục vụ 2 nhóm tác nhân chính với phân quyền rõ ràng[cite: 198, 214]:
1. **Nhân viên (Employee):** Tạo phiếu KPI định kỳ theo tháng/năm, nhập chỉ tiêu mục tiêu & kết quả thực tế, gửi duyệt, theo dõi kết quả xếp loại và chỉnh sửa nếu bị từ chối[cite: 198, 204].
2. **Quản lý KPI / Bộ phận Nhân sự:**
   - Xem xét, phê duyệt hoặc từ chối (kèm lý do) phiếu KPI của nhân viên[cite: 198, 215].
   - Theo dõi danh sách nhân sự nằm trong diện cảnh báo hiệu suất kém[cite: 198, 199].
   - Khởi tạo phân tích dự báo tuyển dụng theo phòng ban, chốt số lượng tuyển thực tế (Phê duyệt / Điều chỉnh / Từ chối đề xuất của hệ thống) và đồng bộ trực tiếp sang phân hệ Tuyển dụng (`Recruitment`)[cite: 189, 190].

---

##  3. Luồng Quy Trình Nghiệp Vụ Cốt Lõi (Core Business Workflows)

### Luồng 1: Đánh giá & Phê duyệt KPI
`Nháp (Draft)` ➔ `Đã gửi (Submitted)` ➔ `Đã duyệt (Approved)` *(hoặc `Từ chối (Rejected)` ➔ Nhân viên chỉnh sửa & gửi lại)*[cite: 204, 205, 218]

### Luồng 2: Phân tích Rủi ro & Cảnh báo Tuyển dụng End-to-End
1. **Tổng hợp dữ liệu:** HR chọn phòng ban cần phân tích và thiết lập tham số tỷ lệ biến động nhân sự dự kiến (`turnover_rate`)[cite: 188, 210].
2. **Tính toán tự động:** Hệ thống quét dữ liệu nhân sự hiện tại, đếm số nhân viên có mức cảnh báo KPI rủi ro để tính toán nhu cầu thay thế và đề xuất số lượng cần tuyển cho từng vị trí (`Job`)[cite: 188, 189].
3. **HR ra quyết định:** HR rà soát từng dòng đề xuất, chốt số lượng tuyển cuối cùng (`final_recruit_qty`)[cite: 208, 215].
4. **Đồng bộ hệ thống:** Khi xác nhận, hệ thống tự động cập nhật chỉ tiêu cần tuyển (`no_of_recruitment`) vào vị trí công việc tương ứng trên module `Recruitment` của Odoo[cite: 213, 216].

---

## 📏 4. Luật Nghiệp Vụ 

- **BR-01 (Tính điểm & Xếp loại tự động):** Điểm KPI được hệ thống tự động tính toán dựa trên tỷ lệ giữa kết quả thực tế (`actual`) và mục tiêu (`target`), từ đó tự động gán nhãn xếp loại[cite: 188, 208, 218].
- **BR-02 (Phân tầng cảnh báo hiệu suất):** Dựa trên số kỳ/tháng liên tiếp không đạt chỉ tiêu, nhân viên được phân vào 4 nhóm cảnh báo: `Bình thường`, `Cảnh báo`, `Nghiêm trọng` và `Xem xét`[cite: 189, 205]. Các nhóm rủi ro sẽ tự động trở thành dữ liệu đầu vào cho bài toán dự báo nhân sự[cite: 203].
- **BR-03 (Kiểm soát toàn vẹn phê duyệt):** Phiếu KPI sau khi gửi duyệt sẽ khóa chỉnh sửa; nếu Quản lý từ chối bắt buộc phải nhập lý do từ chối để nhân viên có cơ sở điều chỉnh[cite: 205, 215].
- **BR-04 (Quyền quyết định của con người):** Số lượng tuyển dụng do hệ thống tính toán chỉ mang tính chất khuyến nghị[cite: 215, 220]. HR có toàn quyền điều chỉnh tăng/giảm hoặc từ chối trước khi đẩy dữ liệu sang module tuyển dụng chính thức[cite: 215, 216].
- **BR-05 (Ràng buộc dữ liệu):** Các trường khóa ngoại (`employee_id`, `department_id`, `job_id`) là bắt buộc; số lượng tuyển dụng chốt cuối cùng không được nhận giá trị âm[cite: 213].

---

##  5. Mô Hình Dữ Liệu Nghiệp Vụ

Tận dụng các bảng chuẩn của Odoo (`HR_DEPARTMENT`, `HR_EMPLOYEE`, `HR_JOB`) kết hợp thiết kế mới 3 thực thể nghiệp vụ[cite: 211, 212]:
- **`HRM_KPI`** (`1 Employee - N KPI`): Lưu trữ mục tiêu, thực tế, điểm số, mức cảnh báo và trạng thái duyệt theo từng tháng/năm[cite: 208, 212].
- **`HRM_RECRUITMENT_FORECAST`** (`1 Department - N Forecast`): Lưu trữ phiên dự báo theo phòng ban, tỷ lệ biến động, tổng nhân sự rủi ro và quyết định tổng thể của HR[cite: 212, 213].
- **`HRM_RECRUITMENT_FORECAST_LINE`** (`1 Forecast - N Lines` & `1 Job - N Lines`): Chi tiết hóa số lượng đề xuất, mức độ ưu tiên (`Cao`, `Trung bình`...) và số lượng HR chốt cho từng vị trí công việc cụ thể[cite: 212, 213, 220].

---

##  6. Sản Phẩm Phân Tích Đã Thực Hiện 

- **Mô hình hóa quy trình & chức năng:** Sơ đồ Use Case tổng quát và 5 bảng Đặc tả Use Case chi tiết (`UC01` – `UC05`) bao gồm luồng chính, luồng thay thế và luồng ngoại lệ[cite: 204, 205, 206, 207].
- **Mô hình hóa hành vi & cấu trúc:** Sơ đồ lớp (Class Diagram) và 3 Sơ đồ tuần tự (Sequence Diagrams) mô tả tương tác giữa Actor, Giao diện XML, Python ORM và PostgreSQL[cite: 208, 209, 210, 211].
- **Thiết kế cơ sở dữ liệu:** ERD chuẩn hóa, xác định khóa chính/khóa ngoại và ràng buộc toàn vẹn dữ liệu[cite: 211, 212, 213].
- **Thiết kế giao diện & Tích hợp:** Đặc tả giao diện Form/List/Dashboard trực quan trên Odoo 18 và đặc tả cấu trúc JSON cho API `/api/recruitment-forecast` phục vụ tích hợp hệ thống ngoài[cite: 202, 214, 218].
- **Kiểm thử nghiệp vụ (UAT / Functional Testing):** Xây dựng kịch bản kiểm thử hộp đen trên dữ liệu mô phỏng cho toàn bộ luồng tính điểm, phân loại rủi ro và đồng bộ định biên tuyển dụng[cite: 191].
