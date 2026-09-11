## Tổng Quan Dự Án

Dự án thực hiện **Phân tích & Thiết kế hệ thống (System Analysis & Design - SAD)** cho quy trình vận hành, nhập hàng, bán hàng và quản lý kho tại chi nhánh cửa hàng thời trang **ICONDENIM** (Nguyễn Trãi, Q.5, TP.HCM).

**Mục tiêu bài toán:**
- Chuyển đổi phương thức quản lý thủ công (sổ sách/file riêng lẻ) sang **Hệ thống thông tin quản lý (MIS)** tập trung.
- Tối ưu hóa quy trình luân chuyển hàng hóa, giảm thiểu sai sót số liệu kho và công nợ.
- Chuẩn hóa quy trình nghiệp vụ (Business Processes) làm cơ sở cho việc số hóa/phát triển phần mềm.

---

## Các phân hệ và chức năng chính

1. **Phân hệ Quản lý Nhập hàng:**
   - Lập Đơn đặt hàng NCC và theo dõi tiến độ giao hàng.
   - Kiểm tra chất lượng hàng nhập, lập Phiếu nhập kho, Phiếu chi và cập nhật tồn kho tự động.

2. **Phân hệ Quản lý Bán hàng & Dịch vụ Khách hàng:**
   - Tiếp nhận đơn hàng, tra cứu tồn kho thời gian thực.
   - Xuất Hóa đơn bán hàng, phân công giao hàng và lập Phiếu thu tiền.

3. **Phân hệ Quản lý Kho & Tồn kho:**
   - Quản lý biến động kho (Nhập/Xuất/Điều chuyển/Kiểm kê định kỳ).
   - Xử lý chênh lệch kiểm kê và cảnh báo ngưỡng tồn kho tối thiểu.

4. **Phân hệ Xử lý Sự cố & Đổi trả:**
   - Tiếp nhận phản hồi hàng lỗi/giao sai, lập Biên bản sự cố.
   - Theo dõi tiến độ đền bù/đổi trả và lưu nhật ký đối soát.

5. **Phân hệ Báo cáo & Thống kê Quản trị:**
   - Thống kê doanh thu, chi phí, lợi nhuận theo khoảng thời gian.
   - Báo cáo giá trị tồn kho, tốc độ luân chuyển hàng hóa hỗ trợ ra quyết định kinh doanh.

---

## Sản Phẩm Phân Tích Nghiệp Vụ & Thiết Kế

Hệ thống được phân tích theo **Phương pháp Phân tích Structured Analysis (SSAD)** với các sản phẩm tài liệu chi tiết:

- **Khảo sát & Thu thập Yêu cầu (Requirements Gathering):** Thực hiện phỏng vấn Quản lý cửa hàng và phát phiếu khảo sát cho nhân viên bán hàng/khách hàng.
- **Sơ đồ Ngữ cảnh (Context Diagram - DFD Level 0):** Xác định ranh giới hệ thống và tương tác với các Tác nhân bên ngoài (Khách hàng, NCC, Ban quản lý).
- **Sơ đồ Phân rã Chức năng (FDD):** Cấu trúc cây chi tiết 5 phân hệ chính và 19 chức năng con mức lá.
- **Sơ đồ Dòng Dữ liệu (DFD Level 1):** Mô tả chi tiết dòng chảy dữ liệu cho cả 5 phân hệ nghiệp vụ.
- **Ma trận Chức năng - Thực thể (CRUD Matrix):** Bảng ánh xạ 14 thực thể dữ liệu với các chức năng nhằm đảm bảo tính toàn vẹn của kiến trúc dữ liệu.
- **Mô hình Dữ liệu (ERD & Relational Database):** Thiết kế CSDL quan hệ đạt chuẩn 3NF với các thực thể cốt lõi (`MAT_HANG`, `DON_DAT_HANG`, `HOA_DON`, `BIEN_BAN_SU_CO`,...).
- **Wireframes / UI Prototype:** Thiết kế giao diện mẫu cho màn hình POS, quản lý nhập hàng, kiểm kho, nhật ký sự cố và báo cáo quản trị.

---

## Phương Pháp & Công Cụ Sử Dụng

- **Kỹ năng BA:** Business Process Modeling, Requirement Engineering, Use Case Analysis, Database Normalization (1NF - 3NF).
- **Công cụ Thiết kế & Vẽ sơ đồ:** Draw.io / Visio / Enterprise Architect.
- **Công cụ UI/UX Wireframing:** Figma / Balsamiq.
- **Định dạng Tài liệu:** Markdown / Technical Specification Documentation.
