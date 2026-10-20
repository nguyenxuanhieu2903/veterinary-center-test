# PROMPT GIAO NHIỆM VỤ TUẦN 1 - DÀNH CHO TV1 (LEADER / CORE BACKEND & DEVOPS)

- **Dự án:** Website Trung tâm Thú y (Veterinary Center Web Application)
- **Học phần:** Lập trình Website (Mã HP: 012012103105)
- **Vai trò:** Leader / Core Backend & DevOps (Mã VT: TV1)
- **Thời gian thực hiện:** Tuần 1

---

### 🎯 MỤC TIÊU CÔNG VIỆC TUẦN 1 CỦA TV1

1. Thiết lập hạ tầng Containerization chuẩn hóa bằng Docker & Docker Compose (gồm App Django 4.2+ và MySQL 8.0).
2. Khởi tạo Git Repository, cấu hình Gitflow (`main`, `develop`, `feature/...`) và thiết lập quy trình PR / Code Review.
3. Chủ trì thiết kế Database Schema (ERD) và viết `models.py` cho 6 bảng cốt lõi (phối hợp cùng TV2, TV3).
4. Xây dựng Custom User Model hỗ trợ phân quyền 4 vai trò (`CUSTOMER`, `DOCTOR`, `STAFF`, `ADMIN`).
5. Hướng dẫn 5 thành viên còn lại clone repo, cài đặt môi trường và chạy thành công lệnh `docker compose up`.

---

### 📋 NHIỆM VỤ CHI TIẾT & DELIVERABLES BẮT BUỘC

#### Task 1: Setup Hạ tầng Docker & Docker Compose

- Viết file `Dockerfile` chuẩn cho môi trường Python 3.11+.
- Viết file `docker-compose.yml` định nghĩa 2 services ban đầu:
  - `web`: Chạy Django Framework 4.2+ (port `8000`), mount source code qua volume.
  - `db`: Chạy MySQL 8.0 Container (port `3306`), cấu hình Environment Variables (`MYSQL_DATABASE`, `MYSQL_USER`, `MYSQL_PASSWORD`, `MYSQL_ROOT_PASSWORD`) và Persistent Volume.
- Cấu hình file `settings.py` kết nối ORM tới MySQL Container.

#### Task 2: Khởi tạo Git Repo & Quy định Gitflow

- Tạo Git Repository trên GitHub/GitLab.
- Khởi tạo 2 nhánh chính: `main` (Production) và `develop` (Tích hợp).
- Đặt quy tắc branch cho cá nhân TV1: `feature/tv1-docker-setup` và `feature/tv1-auth-user`.
- Soạn thảo quy ước Commit Message (Conventional Commits: `feat:`, `fix:`, `docs:`, `chore:`).

#### Task 3: Chủ trì Thiết kế Database Models (models.py) & ERD (Phối hợp TV2, TV3)

- Chủ trì cuộc họp thiết kế Sơ đồ ERD chuẩn hóa trên MySQL.
- Viết `CustomUser` kế thừa `AbstractUser` của Django, bổ sung field `role` với 4 giá trị choices: `CUSTOMER`, `DOCTOR`, `STAFF`, `ADMIN`.
- Phối hợp khai báo các class models còn lại vào file `models.py`:
  - Table `Pet` (Thuộc sở hữu của User - Customer).
  - Table `Product` & `Service` (Danh mục mua bán & Khám chữa).
  - Table `Appointment` & `MedicalRecord` (Lịch hẹn & Bệnh án).
  - Table `Order` & `OrderItem` (Đơn hàng & Chi tiết).
- Chạy `python manage.py makemigrations` và `migrate` thành công vào MySQL Container.

#### Task 4: Onboarding & Hướng dẫn Nhóm

- Viết file `README.md` hướng dẫn ngắn gọn các bước: `git clone`, `cp .env.example .env`, `docker compose up --build`.
- Hỗ trợ TV2, TV3, TV4, TV5, TV6 khởi chạy thành công dự án trên máy cá nhân.

---

### 🏁 TIÊU CHÍ NGHIỆM THU TUẦN 1 DÀNH CHO TV1 (ACCEPTANCE CRITERIA)

- [ ] Chạy lệnh `docker compose up` không báo lỗi, kết nối MySQL thành công.
- [ ] Truy cập `http://localhost:8000/admin` tạo được Superuser (Admin) và đăng nhập bình thường.
- [ ] File `models.py` hiển thị đầy đủ các bảng dữ liệu trên Django Admin / MySQL WorkBench.
- [ ] 5 thành viên còn lại xác nhận đã clone được code và chạy được container thành công.
