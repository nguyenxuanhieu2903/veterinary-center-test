# 🐾 Website Trung Tâm Thú Y (Veterinary Center Web Application)

> **Học phần:** Lập trình Website (Mã HP: 012012103105)  
> **Trường:** Đại học Giao thông vận tải TP. Hồ Chí Minh (UTH)  
> **Nhóm thực hiện:** Nhóm 2  
> **Chủ trì Hạ tầng & Core Backend (TV1):** Leader / Core Backend & DevOps  

---

## 📌 1. Tổng quan Công nghệ (Tech Stack)

- **Backend Framework:** Python 3.11+ / Django 4.2+ (Kiến trúc MVT).
- **Cơ sở dữ liệu:** MySQL 8.0 đóng gói qua Docker Compose (Persistent Volume).
- **Bộ đệm & Message Broker:** Redis 7.x + Celery 5.x (Hỗ trợ gửi Email/nhắc lịch tự động ngầm).
- **Frontend:** HTML5, CSS3, JavaScript (Fetch API / AJAX), Bootstrap 5 / Tailwind CSS.
- **Hạ tầng & Đóng gói:** Docker Desktop & Docker Compose v2.

---

## 🚀 2. Hướng dẫn Khởi chạy Nhanh (Dành cho TV2, TV3, TV4, TV5, TV6)

Tất cả thành viên chỉ cần làm theo 4 bước chuẩn hóa sau để chạy ứng dụng trên máy cá nhân:

### 🔹 Bước 1: Clone Repository
```bash
git clone <URL_REPO_CUA_NHOM>
cd Website_TrungTamThuY
```

### 🔹 Bước 2: Tạo File Môi Trường `.env`
Sao chép cấu hình mẫu từ `.env.example`:
- **Trên Windows PowerShell / CMD:**
  ```powershell
  copy .env.example .env
  ```
- **Trên Linux / macOS / Git Bash:**
  ```bash
  cp .env.example .env
  ```

*(File `.env` mặc định đã được cấu hình sẵn các thông số chuẩn để kết nối trơn tru với MySQL và Redis trong Docker).*

### 🔹 Bước 3: Khởi chạy bằng Docker Compose
Đảm bảo **Docker Desktop** trên máy bạn đang mở, sau đó chạy:
```bash
docker compose up --build
```
> 💡 **Lưu ý:** Trong lần chạy đầu tiên, Docker sẽ tải image MySQL 8.0, Redis và build môi trường Python. Quá trình này sẽ tự động đợi MySQL sẵn sàng và tự động chạy `makemigrations` + `migrate`.

Khi màn hình terminal hiển thị:
```
Starting development server at http://0.0.0.0:8000/
Quit the server with CONTROL-C.
```
Ứng dụng đã sẵn sàng!

### 🔹 Bước 4: Tạo Tài Khoản Quản trị Superuser (Admin)
Mở một cửa sổ Terminal mới tại thư mục dự án và chạy:
```bash
docker compose exec web python manage.py createsuperuser
```
Nhập các thông tin:
- `Username`: `admin` (hoặc tên tùy chọn)
- `Email`: `admin@vetcenter.local`
- `Password`: Nhập mật khẩu (tối thiểu 6 ký tự)
- `Role`: Sau khi tạo xong, tài khoản mặc định có đầy đủ quyền Superuser/Admin.

### 🔹 Bước 5: Truy cập Hệ Thống
- **Trang chủ & Kiểm tra Hạ tầng:** [http://localhost:8000](http://localhost:8000)
- **API Health Check:** [http://localhost:8000/api/health/](http://localhost:8000/api/health/)
- **Trang Quản trị Django Admin:** [http://localhost:8000/admin](http://localhost:8000/admin)

---

## 🗄️ 3. Danh mục 6 Bảng Models Cốt Lõi (Đã triển khai)

Hệ thống đã hoàn thiện đầy đủ cấu trúc Database Schema bao gồm:

1. **`CustomUser` (core.CustomUser):** Kế thừa `AbstractUser`, mở rộng trường `role` phân quyền 4 vai trò độc lập:
   - `CUSTOMER`: Khách hàng / Chủ thú cưng.
   - `DOCTOR`: Bác sĩ thú y trực tiếp khám và kê đơn.
   - `STAFF`: Nhân viên điều phối, quản lý đơn hàng & sản phẩm.
   - `ADMIN`: Quản trị viên hệ thống.
   - Bổ sung: `phone_number`, `address`, `avatar`.
2. **`Pet`:** Quản lý hồ sơ thú cưng (tên, loài, giống, giới tính, ngày sinh, cân nặng, tình trạng sức khỏe, ảnh).
3. **`Service`:** Danh mục dịch vụ thú y (tên dịch vụ, đơn giá, thời lượng khám dự kiến, trạng thái hoạt động, ảnh).
4. **`Product`:** Danh mục sản phẩm thú cưng (tên, phân loại, mô tả, đơn giá, số lượng tồn kho, ảnh).
5. **`Appointment`:** Lịch hẹn khám (liên kết Khách hàng, Thú cưng, Dịch vụ, Bác sĩ phụ trách, ngày/giờ khám, trạng thái `PENDING`/`CONFIRMED`/`COMPLETED`/`CANCELLED`).
6. **`MedicalRecord`:** Bệnh án y tế (liên kết Lịch hẹn, Thú cưng, Bác sĩ chẩn đoán, triệu chứng, chẩn đoán, đơn thuốc, lịch hẹn tái khám).
7. **`Order` & `OrderItem`:** Quản lý đơn hàng COD (Khách hàng, tổng tiền Backend tự tính toán, trạng thái đơn, địa chỉ giao hàng, chi tiết từng món hàng và số lượng).

---

## 🌿 4. Quy định Git & Gitflow Cho Từng Thành Viên

### Quy tắc Nhánh (Branching Strategy):
- **`main`**: Nhánh Production / Nộp bài. **TUYỆT ĐỐI KHÔNG PUSH TRỰC TIẾP**.
- **`develop`**: Nhánh tích hợp chính của cả nhóm.
- **`feature/<mã-tv>-<tên-chức-năng>`**: Nhánh tính năng cá nhân tạo ra từ `develop`.

### Tiền tố nhánh theo phân công:
| Thành viên | Vai trò | Tiền tố nhánh quy định | Ví dụ |
| :---: | :--- | :--- | :--- |
| **TV1** | Leader / DevOps & Core Backend | `feature/tv1-...` | `feature/tv1-auth-user` |
| **TV2** | Backend Dev 1 (Medical & Booking) | `feature/tv2-...` | `feature/tv2-medical-models` |
| **TV3** | Backend Dev 2 (Shop & Service) | `feature/tv3-...` | `feature/tv3-shop-models` |
| **TV4** | Frontend Lead (Core UI / Layout) | `feature/tv4-...` | `feature/tv4-base-layout` |
| **TV5** | Frontend Dev 1 (Client Portal) | `feature/tv5-...` | `feature/tv5-booking-form` |
| **TV6** | Frontend Dev 2 (Admin UI & Doc) | `feature/tv6-...` | `feature/tv6-admin-dashboard` |

### Quy trình tạo và nộp code:
1. Kéo code mới nhất từ `develop`:
   ```bash
   git checkout develop
   git pull origin develop
   ```
2. Tạo nhánh làm việc cá nhân:
   ```bash
   git checkout -b feature/<mã-tv>-<tên-nhiệm-vụ>
   ```
3. Commit code theo chuẩn Conventional Commits:
   - `feat: thêm chức năng đặt lịch khám`
   - `fix: sửa lỗi tính tổng tiền đơn hàng`
   - `docs: cập nhật tài liệu hướng dẫn`
   - `refactor: tối ưu hóa truy vấn ORM`
4. Push nhánh lên GitHub và tạo **Pull Request (PR)** vào nhánh `develop`:
   ```bash
   git push origin feature/<mã-tv>-<tên-nhiệm-vụ>
   ```
5. Leader (TV1) tiến hành Code Review và duyệt merge vào `develop`.

---

## 🛠️ 5. Các Lệnh Thường Dùng Khi Phát Triển

| Mục đích | Lệnh thực hiện |
| :--- | :--- |
| Khởi động container | `docker compose up` |
| Khởi động ngầm (Detached) | `docker compose up -d` |
| Build lại image khi đổi `requirements.txt` | `docker compose up --build` |
| Dừng toàn bộ containers | `docker compose down` |
| Xem log container | `docker compose logs -f web` |
| Tạo migrations mới | `docker compose exec web python manage.py makemigrations` |
| Chạy migrations vào MySQL | `docker compose exec web python manage.py migrate` |
| Mở bash shell trong container Web | `docker compose exec web sh` |
| Truy cập MySQL CLI trực tiếp | `docker compose exec db mysql -u vet_user -p vet_center_db` |

---

## 📞 Hỗ trợ & Liên hệ
Mọi thắc mắc hoặc sự cố về môi trường Docker/Database, các thành viên vui lòng liên hệ trực tiếp **TV1 (Leader)** để được hỗ trợ xử lý ngay.
