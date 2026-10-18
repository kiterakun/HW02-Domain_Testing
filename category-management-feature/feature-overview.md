# Feature Overview

**Feature Name:** FR-14 -- Quản lý Danh mục (Category Management)
**Description:** Chức năng cho phép Admin Thêm / Xem / Xóa danh mục sản phẩm trong phân hệ Web Admin.
**Version:** 2.0

## Requirements (SRS)

> **FR-14: Quản lý Danh mục (Category CRUD)**
> - Admin có thể Thêm / Xem / Xóa danh mục.
> - Tên danh mục là bắt buộc, không được để trống.

Yêu cầu liên quan:

> **FR-12: Kiểm soát truy cập (Access Control)** — Tất cả các API Admin (`/api/admin/*`) và các API có tính ảnh hưởng dữ liệu (`POST/PUT/DELETE /api/products`, `/api/categories`, `/api/coupons`) đều phải yêu cầu:
> 1. Token JWT hợp lệ.
> 2. `role = 'admin'` trong Token.

## User Flows

- Flow 1 (Create): Admin đăng nhập → mở tab "Danh mục" → nhập tên danh mục → bấm "Thêm mới" → danh mục mới xuất hiện trong bảng.
- Flow 2 (Read): Mở tab "Danh mục" (hoặc gọi `GET /api/categories`) → hiển thị danh sách danh mục (ID, Tên Danh Mục).
- Flow 3 (Delete): Admin bấm "Xóa" trên một dòng danh mục → danh mục biến mất khỏi bảng.
- Flow 4 (Update — ngoài phạm vi SRS): Backend có `PUT /api/categories/:id` nhưng SRS FR-14 không yêu cầu thao tác Update và UI không cung cấp; chỉ kiểm thử ở mức API để đối chiếu.

## Input Variables

- `name` (string): tên danh mục — bắt buộc, không được để trống theo SRS.
- `id` (integer): mã danh mục (tham số đường dẫn cho PUT/DELETE).
- Token (JWT): trạng thái xác thực — không có token / token không hợp lệ / token của user thường / token của admin.

## Expected Outputs

- Thành công: `{"message": "Category created"}` / `{"message": "Category updated"}` / `{"message": "Category deleted"}`; danh sách danh mục được làm mới trên UI.
- Thất bại: `401 Unauthorized` (thiếu token), `403 Forbidden` (token không hợp lệ); theo SRS (FR-12), user thường phải bị từ chối khi gọi POST/PUT/DELETE.

## Business Rules

- Tên danh mục bắt buộc, không được để trống (FR-14).
- POST/PUT/DELETE `/api/categories` yêu cầu JWT hợp lệ và `role = 'admin'` (FR-12, SEC-03).
- GET `/api/categories` là API công khai, không yêu cầu xác thực.

## Khảo sát SUT (bằng chứng mã nguồn)

- `eshop-sut/backend/server.js` dòng 243–278: 4 endpoint categories; POST/PUT/DELETE dùng middleware `authenticateToken` (chỉ verify JWT), **không kiểm tra role**.
- `eshop-sut/backend/database.js` dòng 23–26: bảng `categories (id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT)` — không NOT NULL, không UNIQUE, không giới hạn độ dài; seed 3 danh mục (Điện thoại, Laptop, Phụ kiện).
- `eshop-sut/frontend-admin/src/App.jsx` dòng 142–160, 294–335: form thêm danh mục (input không thuộc tính `required`, không dấu `*` theo FR-22), bảng danh sách, nút Xóa không có dialog xác nhận; không có UI cập nhật danh mục.
