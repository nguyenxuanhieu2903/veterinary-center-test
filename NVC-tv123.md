# PROMPT CHO NHÓM BACKEND (TV1 + TV2 + TV3) - NHIỆM VỤ THIẾT KẾ DATABASE & MODELS TUẦN 1

- **Dự án:** Website Trung tâm Thú y (Veterinary Center Web Application)
- **Học phần:** Lập trình Website (Mã HP: 012012103105)
- **Đối tượng thực hiện:**
  - **TV1 (Leader / DevOps / Core Backend):** Chủ trì & Custom User Model / Phân quyền
  - **TV2 (Backend Dev 1 - Medical & Booking):** Đảm nhiệm Models Thú cưng, Lịch hẹn, Bệnh án
  - **TV3 (Backend Dev 2 - Shop & Service):** Đảm nhiệm Models Sản phẩm, Dịch vụ, Đơn hàng
- **Thời gian thực hiện:** Tuần 1

---

### 🎯 MỤC TIÊU PHỐI HỢP CỦA TV1 + TV2 + TV3 TRONG TUẦN 1

Thống nhất Sơ đồ Quan hệ Dữ liệu (ERD Schema) và cùng hoàn thiện file `models.py` chuẩn hóa trên MySQL 8.0 bao phủ đầy đủ 6 phân hệ cốt lõi của hệ thống Website Trung tâm Thú y.

---

### 📋 PHÂN CÔNG VIẾT MODEL CHI TIẾT TRONG FILE `models.py`

#### 1. TV1 (Leader / Core Backend) - Đảm nhiệm Auth & Permission

- **`User` (Custom User Model):**
  - Kế thừa `AbstractUser`.
  - Thêm field `role`: `models.CharField` với `choices` (`CUSTOMER`, `DOCTOR`, `STAFF`, `ADMIN`).
  - Thêm các field: `phone_number`, `address`, `avatar`.
  - Đảm bảo tương thích hoàn toàn với Django Auth & Security.

#### 2. TV2 (Backend Dev 1) - Đảm nhiệm Phân hệ Y tế & Đặt lịch (Medical & Booking)

- **`Pet` (Hồ sơ Thú cưng):**
  - Foreign Key tới `User` (chỉ Khách hàng sở hữu).
  - Các field: `name`, `species` (Chó/Mèo...), `breed` (Giống), `gender`, `birth_date`, `weight`, `health_condition`, `image`.
- **`Appointment` (Lịch hẹn khám):**
  - Foreign Key tới `User` (Customer), `Pet`, `Service`, và `User` (Doctor được phân công).
  - Các field: `appointment_date`, `time_slot`, `status` (`PENDING`, `CONFIRMED`, `COMPLETED`, `CANCELLED`), `notes`.
- **`MedicalRecord` (Bệnh án & Lịch sử Y tế):**
  - Foreign Key (1-1 hoặc 1-N) tới `Appointment` & `Pet`, Foreign Key tới `User` (Doctor lập sổ).
  - Các field: `symptoms` (Triệu chứng), `diagnosis` (Chẩn đoán), `prescription` (Đơn thuốc), `revisit_date` (Ngày tái khám).

#### 3. TV3 (Backend Dev 2) - Đảm nhiệm Phân hệ Cửa hàng & Dịch vụ (Shop & Service)

- **`Service` (Danh mục Dịch vụ Thú y):**
  - Các field: `name`, `description`, `price`, `duration` (thời lượng khám dự kiến), `is_active`, `image`.
- **`Product` (Danh mục Sản phẩm Thú cưng):**
  - Các field: `name`, `category`, `description`, `price`, `stock_quantity` (số lượng tồn kho), `is_active`, `image`.
- **`Order` & `OrderItem` (Quản lý Đơn hàng COD):**
  - `Order`: Foreign Key tới `User` (Customer), `total_amount` (Backend tự tính), `status` (`PENDING`, `PROCESSING`, `SHIPPED`, `COMPLETED`, `CANCELLED`), `shipping_address`, `phone_number`, `created_at`.
  - `OrderItem`: Foreign Key tới `Order` và `Product`, `quantity`, `price_at_purchase`.

---

### ⚙️ QUY TRÌNH PHỐI HỢP & CODE REVIEW (WORKFLOW)

1. **Bước 1:** TV1 khởi tạo base project Django + Docker, đẩy lên nhánh `develop`.
2. **Bước 2:** TV2 và TV3 tạo nhánh feature riêng từ `develop`:
   - TV2 tạo `feature/tv2-medical-models`
   - TV3 tạo `feature/tv3-shop-models`
3. **Bước 3:** Cả 3 thành viên viết code model theo đúng phân công trên, tự chạy `makemigrations` kiểm tra syntax local.
4. **Bước 4:** TV2 và TV3 tạo Pull Request (PR) nộp code vào nhánh `develop`.
5. **Bước 5:** TV1 (Leader) tiến hành Code Review, check xem các khóa ngoại (Foreign Keys), ràng buộc dữ liệu đã khớp hoàn toàn với thiết kế ERD chưa, sau đó duyệt merge vào `develop`.

---

### 🏁 TIÊU CHÍ NGHIỆM THU CHUNG CỦA BỘ BA BACKEND (TV1 + TV2 + TV3)

- [ ] File `models.py` thống nhất không bị xung đột (conflict) khi merge code.
- [ ] Chạy thành công lệnh `python manage.py makemigrations` và `python manage.py migrate` tạo trơn tru các bảng trong database MySQL 8.0 Container.
- [ ] Tất cả các bảng dữ liệu hiển thị trực quan và có thể thao tác CRUD cơ bản trên giao diện Django Admin.
