## Tổng Quan Dự Án

### 1. Phân tích Tác nhân & Quyền hạn 
Hệ thống thiết lập 3 nhóm tác nhân chính với các phạm vi nghiệp vụ tách biệt:
* **Student (Sinh viên):** Quản lý profile cá nhân, tìm kiếm vị trí thực tập theo Ngành học/Đợt thực tập, upload CV (PDF), ứng tuyển (Apply), theo dõi trạng thái hồ sơ (FSM) và nhận lịch phỏng vấn.
* **Company (Doanh nghiệp):** Cập nhật thông tin công ty, đăng tuyển bài viết thực tập (Internship Postings), sàng lọc hồ sơ ứng viên, chuyển trạng thái hồ sơ (Shortlist/Reject/Accept) và xếp lịch phỏng vấn (Interview Scheduling).
* **Admin (Quản trị viên hệ thống):** Quản lý danh mục (Ngành học, Đợt thực tập), duyệt tài khoản doanh nghiệp, kiểm soát dữ liệu ứng tuyển và theo dõi Dashboard thống kê hệ thống.

---

### 2. Quy trình nghiệp vụ cốt lõi

#### Luồng kết nối thực tập End-to-End:
1. **Đăng tin:** Company tạo bài đăng tuyển dụng (`Internship`) liên kết với Ngành học (`Major`) và Đợt thực tập (`Internship_Period`).
2. **Ứng tuyển:** Student chọn bài đăng, upload CV (định dạng PDF) để tạo đơn ứng tuyển (`Application`).
3. **Sàng lọc & Duyệt:** Company xem danh sách hồ sơ, tải/xem CV và cập nhật trạng thái đơn ứng tuyển.
4. **Phỏng vấn:** Khi đơn ứng tuyển đạt trạng thái **Shortlisted**, Company khởi tạo lịch phỏng vấn (`Interview`) gồm thời gian và địa điểm.
5. **Hoàn tất:** Student theo dõi trạng thái và lịch phỏng vấn thời gian thực trên giao diện di động.

#### Quản lý vòng đời đơn ứng tuyển (FSM):
Trạng thái của `Application` được kiểm soát chặt chẽ theo luồng tuyến tính:
`Applied` -> `Shortlisted` -> `Accepted` / `Rejected`

---

### 3. Thiết kế mô hình dữ liệu quan hệ

Hệ thống được chuẩn hóa dữ liệu với các mối quan hệ thực thể cốt lõi:
* **User - Student Profile / Company:** Quan hệ `1 - 1` (Mỗi tài khoản gắn liền với một hồ sơ sinh viên hoặc doanh nghiệp).
* **Company - Internship:** Quan hệ `1 - n` (Một doanh nghiệp có thể đăng nhiều tin tuyển dụng).
* **Student Profile - Internship:** Quan hệ `n - n` thông qua bảng trung gian **Application** (Sinh viên có thể ứng tuyển nhiều tin tuyển dụng và một tin tuyển dụng nhận nhiều ứng viên).
* **Application - Interview:** Quan hệ `1 - 1` hoặc `1 - n` (Đơn ứng tuyển đủ điều kiện mới tạo lịch phỏng vấn).
* **Internship - Major / Internship_Period:** Quan hệ `n - 1` (Phân loại bài đăng theo chuyên ngành và kỳ thực tập).

---

### 4. Quy định nghiệp vụ

* **BR-01 (Định dạng tệp CV):** Hệ thống chỉ chấp nhận tệp CV ở định dạng `.pdf`. Các định dạng khác sẽ bị từ chối ở cả giao diện UI và Backend Validator.
* **BR-02 (Ràng buộc ứng tuyển):** Sinh viên không được phép tạo nhiều hơn 1 đơn ứng tuyển (`Application`) cho cùng một bài đăng tuyển dụng (`Internship`). Nút ứng tuyển sẽ chuyển trạng thái sang "Đã ứng tuyển" để ngăn chặn thao tác trùng lặp.
* **BR-03 (Điều kiện xếp lịch phỏng vấn):** Doanh nghiệp chỉ có thể khởi tạo lịch phỏng vấn (`Interview`) đối với các đơn ứng tuyển đã được chuyển sang trạng thái **Shortlisted**.
* **BR-04 (Bảo mật thông tin):** Doanh nghiệp chỉ được quyền truy cập và thao tác trên danh sách ứng viên nộp hồ sơ vào các bài đăng tuyển dụng thuộc sở hữu của chính doanh nghiệp đó.

---

### 5. Yêu cầu Phi chức năng 

* **Tính khả dụng:** Giao diện thiết kế theo chuẩn ứng dụng di động, tối ưu trải nghiệm thao tác trên màn hình cảm ứng, hiển thị thông báo lỗi thân thiện với người dùng.
* **Tính bảo mật:** Mật khẩu được mã hóa an toàn, xác thực truy cập API bằng cơ chế Token (JWT/Session Authentication), phân quyền chặt chẽ theo vai trò (RBAC).
* **Tính toàn vẹn dữ liệu:** Ràng buộc khóa ngoại nghiêm ngặt giữa các bảng trong CSDL MySQL, hỗ trợ lưu trữ tệp đa phương tiện (CV/Hình ảnh) an toàn trên nền tảng Cloudinary.UX Wireframing:** Figma / Balsamiq.
- **Định dạng Tài liệu:** Markdown / Technical Specification Documentation.
