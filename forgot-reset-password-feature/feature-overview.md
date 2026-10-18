# Feature Overview

**Feature Name:** FR-03 -- Forgot Password and Password Reset
**Description:** Chức năng cho phép người dùng khôi phục tài khoản khi quên mật khẩu thông qua 2 bước: lấy mã OTP qua email và tiến hành đặt lại mật khẩu mới.
**Version:** 2.0

## Requirements (SRS)
- **Bước 1 — Lấy mã OTP:**
  - Người dùng nhập địa chỉ Email đã đăng ký.
  - Hệ thống sinh mã OTP 6 chữ số ngẫu nhiên và gửi qua Email (trong môi trường demo: hiển thị trực tiếp trên màn hình).
  - Giao diện phải hiển thị chỉ báo bước (Step Indicator) — ví dụ: "Bước 1 / 2".
  - Có nút Quay lại đăng nhập.
- **Bước 2 — Đặt lại mật khẩu:**
  - Người dùng nhập OTP, Mật khẩu mới, và Xác nhận mật khẩu mới.
  - Mật khẩu mới phải tuân thủ điều kiện như FR-01 (tối thiểu 8 ký tự, có chữ hoa, thường, số, ký tự đặc biệt/khoảng trắng).
  - Hai trường mật khẩu phải khớp nhau.
  - OTP chỉ hợp lệ cho email đã yêu cầu.

## User Flows
- Flow 1: Nhập email → Nhận mã OTP (Hiển thị UI Step 2) → Nhập OTP & Mật khẩu mới → Đổi mật khẩu thành công → Chuyển hướng về trang đăng nhập.
- Flow 2: Nhập email không tồn tại / sai định dạng → Báo lỗi.
- Flow 3: Nhập sai OTP hoặc mật khẩu không đúng định dạng → Báo lỗi.

## Input Variables
- `email`: string
- `resetToken`: string (OTP)
- `newPassword`: string
- `confirmPassword`: string

## Expected Outputs
- Thành công: Thông báo "Mã đặt lại mật khẩu đã được tạo" / "Đổi mật khẩu thành công" và chuyển trang.
- Thất bại: Thông báo lỗi ("User not found", "Invalid token or email", "Mật khẩu quá yếu").

## Business Rules
- OTP phải là 6 chữ số (theo SRS).
- Mật khẩu phải đạt độ phức tạp theo FR-01.
- Token chỉ dùng 1 lần và bị xóa (`NULL`) sau khi reset thành công.
