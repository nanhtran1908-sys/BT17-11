# BÀI THỰC HÀNH 6.1 & 6.2 – LẬP TRÌNH WINDOWS FORMS

**Công nghệ:** C# WinForms – .NET Framework 4.8 – SQL Server  
**Mô hình:** 3-Tier (DTO – DAL – BUS – GUI)

## MỤC 1: THÔNG TIN SINH VIÊN

- **Họ và tên sinh viên:** Trần Ngọc Anh
- **Mã sinh viên:** 24810310211
- **Lớp / Khóa:** D19CNPM3
- **Trường:** Đại học Điện lực
- **Khoa:** Điện toán và công nghệ số năng lượng 

---

## MỤC 2: HÌNH ẢNH KẾT QUẢ CÁC MÀN HÌNH CHỨC NĂNG

Các hình ảnh dưới đây minh họa kết quả chạy chương trình thực tế trên Visual Studio.

### 1. Màn hình chính / Tổng quan hệ thống

Hiển thị giao diện chính của ứng dụng và các chức năng quản lý.

![Màn hình chính](images/01_main_screen.png)

### 2. Màn hình quản lý danh mục (CRUD)

Minh họa chức năng thêm, sửa, xóa và tìm kiếm dữ liệu trong hệ thống.

![Màn hình CRUD](images/02_crud_screen.png)

### 3. Màn hình thực thi nghiệp vụ chính

Minh họa nghiệp vụ bán hàng, lập hóa đơn hoặc chấm công của chương trình.

![Màn hình nghiệp vụ](images/03_transaction_screen.png)

### 4. Màn hình báo cáo – thống kê

Hiển thị doanh thu, sản phẩm bán chạy hoặc kết quả thống kê, tính lương.

![Màn hình thống kê](images/04_report_screen.png)

### 5. Minh họa an toàn dữ liệu và xử lý lỗi

Minh họa chương trình xử lý lỗi khi nhập dữ liệu không hợp lệ, sử dụng câu lệnh SQL có tham số hoặc thực hiện Transaction Rollback.

![Minh họa xử lý lỗi](images/05_error_handling.png)

---

## MỤC 3: GIỚI THIỆU CHƯƠNG TRÌNH

Bài thực hành gồm hai ứng dụng Windows Forms xây dựng bằng C# theo mô hình 3-Tier, sử dụng SQL Server để lưu trữ dữ liệu.

**Bài 6.1 – Quản lý bán hàng:**

- Quản lý sản phẩm, khách hàng, nhân viên.
- Lập hóa đơn bán hàng.
- Tính tiền và điểm tích lũy.
- Quản lý tồn kho khi bán hàng.
- Thống kê doanh thu và sản phẩm bán chạy.

**Bài 6.2 – Quản lý nhân sự:**

- Quản lý thông tin nhân viên.
- Quản lý phòng ban.
- Chuyển phòng ban.
- Quản lý chấm công.
- Tính và chốt lương nhân viên.

Cả hai project sử dụng **.NET Framework 4.8**, thư viện `System.Data.SqlClient` có sẵn nên không cần cài thêm NuGet.

Chương trình được mở và chạy bằng Visual Studio 2019/2022 với workload ".NET desktop development".

---

## MỤC 4: HƯỚNG DẪN CÀI ĐẶT VÀ CHẠY

### Bước 1: Khởi tạo cơ sở dữ liệu

Mở SQL Server Management Studio (SSMS), chạy lần lượt các file:

- `Bai6_1_RetailSales/SalesDB.sql`
- `Bai6_2_HRM/HRMDB.sql`

Các file SQL đã có dữ liệu mẫu.

### Bước 2: Mở project

Mở một trong hai file bằng Visual Studio:

- `RetailSales.sln`
- `HRM.sln`

### Bước 3: Cấu hình kết nối SQL Server

Mở file `App.config` và chỉnh chuỗi kết nối phù hợp với SQL Server trên máy.

Ví dụ:

- `Data Source=.` – SQL Server mặc định.
- `Data Source=.\SQLEXPRESS` – SQL Server Express.
- `Data Source=(localdb)\MSSQLLocalDB` – LocalDB.

### Bước 4: Chạy chương trình

Nhấn **F5** trong Visual Studio để chạy và kiểm tra các chức năng.

---

## MỤC 5: THIẾT KẾ GIAO DIỆN WINDOWS FORMS

Giao diện được xây dựng bằng Windows Forms Designer của Visual Studio.

Mỗi màn hình có các file:

- `frmX.cs`: Xử lý các sự kiện và nghiệp vụ giao diện.
- `frmX.Designer.cs`: Khởi tạo và bố trí các thành phần giao diện.

Để mở chế độ thiết kế, nhấn chuột phải vào file `frmX.cs` và chọn **View Designer**.

Có thể kéo thả, thay đổi kích thước và chỉnh sửa các thành phần bằng Toolbox và Properties.

Lưu ý: Không tự ý thay đổi tên các control đang được sử dụng trong code như `txtMaSP`, `dgvSanPham`, `btnThem` nếu chưa sửa các tham chiếu tương ứng.

---

## MỤC 6: CẤU TRÚC CHƯƠNG TRÌNH (3-TIER)

Chương trình được tổ chức thành các tầng:

| Tầng | Chức năng |
|---|---|
| DTO | Chứa các đối tượng truyền dữ liệu |
| DAL | Truy cập và thao tác cơ sở dữ liệu |
| BUS | Xử lý nghiệp vụ và kiểm tra dữ liệu |
| GUI | Hiển thị giao diện và tương tác với người dùng |

Luồng xử lý:

`GUI → BUS → DAL → SQL Server`

GUI không gọi trực tiếp DAL.

---

## MỤC 7: CÁC CHỨC NĂNG BÀI 6.1 – QUẢN LÝ BÁN HÀNG

| Yêu cầu | Vị trí thực hiện |
|---|---|
| Cơ sở dữ liệu 5 bảng, khóa ngoại và ràng buộc | `SalesDB.sql` |
| CRUD sản phẩm, khách hàng, nhân viên | `frmSanPham`, `frmKhachHang`, `frmNhanVien` |
| Tìm sản phẩm theo tên, mã vạch | `SanPhamDAL.Search` |
| Sử dụng SqlParameter và LIKE | `SanPhamDAL.Search` |
| Bán hàng, quét mã vạch, giỏ hàng | `frmBanHang` |
| Lập hóa đơn và xử lý giao dịch | `HoaDonDAL.LapHoaDon` |
| Tính
