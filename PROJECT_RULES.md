# VETERINARY CENTER - PROJECT CONTEXT & CODING RULES

## 1. TỔNG QUAN DỰ ÁN & THIẾT LẬP CÔNG NGHỆ

Dự án **Website Trung tâm Thú y** được phát triển trong 5 tuần theo chương trình Lập trình Website.

- **Mục tiêu**: Xây dựng ứng dụng Web hoàn chỉnh phục vụ Đặt lịch khám thú y, Quản lý hồ sơ y tế/bệnh án, Cửa hàng bán sản phẩm thú cưng và Dashboard Quản trị.
- **Stack công nghệ chính**:
  - **Backend**: Python 3.11+, Django Framework 4.2+ (Kiến trúc MVT - Model-View-Template).
  - **Database**: MySQL 8.0 đóng gói qua Docker Compose.
  - **Task Queue & Cache**: Redis + Celery (Gửi Email tự động ngầm).
  - **Frontend**: HTML5, CSS3, Bootstrap 5 / Tailwind CSS, JavaScript (Vanilla/Fetch API).
  - **Môi trường & Tooling**: Docker & Docker Compose, Git/GitHub, MS Word/PowerPoint (Báo cáo/Slide).

---

## 2. PHÂN CÔNG THÀNH VIÊN & PHẠM VI TRÁCH NHIỆM (WBS)

Nhóm gồm 6 thành viên đảm nhận các vai trò chuyên biệt theo phân công WBS:

| Mã VT | Thành viên | Vai trò đảm nhận | Phạm vi công việc & Vị trí Code | Tiền tố nhánh Git |
| :---: | :--- | :--- | :--- | :--- |
| **TV1** | **Thành viên 1** | **Leader / Core Backend & DevOps** | Docker Compose, Custom User Model, Auth & Phân quyền 4 vai trò, Celery/Redis Email, Deploy VPS. | `feature/tv1-devops-...` |
| **TV2** | **Thành viên 2** | **Backend Dev 1 (Medical & Booking)** | CRUD Hồ sơ thú cưng, Logic Đặt lịch khám, Portal Bác sĩ (Bệnh án, Chẩn đoán, Đơn thuốc). | `feature/tv2-medical-...` |
| **TV3** | **Thành viên 3** | **Backend Dev 2 (Shop & Service)** | CRUD Sản phẩm & Dịch vụ, Session Cart (Giỏ hàng), Logic Checkout (Đơn hàng COD), Seed Data. | `feature/tv3-shop-...` |
| **TV4** | **Thành viên 4** | **Frontend Lead (UI/UX & Core UI)** | Figma Design System, Master Layout (`base.html`), UI Giỏ hàng, Checkout & Lịch sử đơn hàng. | `feature/tv4-core-ui-...` |
| **TV5** | **Thành viên 5** | **Frontend Dev 1 (Client Portal)** | UI Trang chủ, Chi tiết dịch vụ, UI Hồ sơ thú cưng, Form Đặt lịch khám trực tuyến. | `feature/tv5-client-ui-...` |
| **TV6** | **Thành viên 6** | **Frontend Dev 2 (Admin UI & Doc)** | UI Dashboard Admin, Portal Bác sĩ, Soạn thảo Báo cáo Đồ án Word/PDF & Slide thuyết trình. | `feature/tv6-admin-doc-...` |

---

## 3. NGUYÊN TẮC VÀ QUY TẮC BẮT BUỘC KHI VIẾT CODE (CRITICAL RULES)

### A. Nghiệp vụ & Dữ liệu

- **Tính tổng tiền**: Luôn tính toán Tổng tiền đơn hàng (`total_amount`) tại Backend Service/View dựa trên giá niêm yết trong DB. TUYỆT ĐỐI không tin tưởng tổng tiền do Client/Frontend gửi lên.
- **Phân quyền 4 Vai trò (4 Roles)**: Mọi truy cập vào views/APIs phải kiểm tra vai trò người dùng (`CUSTOMER`, `DOCTOR`, `STAFF`, `ADMIN`) thông qua Django Decorators/Mixins (`@login_required`, `@role_required`). Ngăn chặn khách hàng truy cập hồ sơ người khác bằng cách thay đổi ID trên URL.
- **Tồn kho & Giỏ hàng**: Xử lý giỏ hàng qua Session Cart. Khi chuyển đơn hàng sang trạng thái xử lý/giao hàng phải cập nhật tồn kho bằng Database Transaction.
- **Celery Async Task**: Các thao tác gửi Email xác nhận hoặc nhắc lịch BẮT BUỘC đẩy vào Celery Task ngầm (Redis Broker), không gửi trực tiếp trong HTTP request-response cycle.

### B. Kiến trúc & Cấu trúc Thư mục Django

- Tuân thủ cấu trúc thư mục Django chuẩn:
  - `templates/`: Đặt các file HTML theo ứng dụng (`templates/client/`, `templates/doctor/`, `templates/admin/`, `templates/base.html`).
  - `static/`: Đặt CSS, JS, Images (`static/css/`, `static/js/`, `static/images/`).
- Cấu hình kết nối MySQL, Redis, Celery tham chiếu đến Docker environment variables trong `settings.py`.

### C. Quy tắc Quản lý Git & Branching Strategy

- **`main`**: Nhánh Production / Nộp bài. Được bảo vệ nghiêm ngặt, tuyệt đối CẤM push trực tiếp.
- **`develop`**: Nhánh tích hợp chính của cả nhóm trong suốt 5 tuần.
- **`feature/<mã-tv>-<tên-chức-năng>`**: Nhánh tính năng cá nhân tạo ra từ `develop` (vd: `feature/tv1-docker-setup`, `feature/tv2-booking-logic`).
- **Commit Message**: Tuân thủ chuẩn Conventional Commits (`feat:`, `fix:`, `docs:`, `refactor:`, `style:`, `chore:`).
- **Pull Request & Code Review**: Mọi PR nộp vào `develop` phải được Leader (TV1) review và chấp thuận trước khi merge.
