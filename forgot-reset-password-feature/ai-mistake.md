# Tổng hợp Lỗi và Sai sót (AI Mistakes Aggregate)

## 1. Domain Testing Mistakes / Non-Compliances
- **OTP Length Mismatch:** SRS yêu cầu OTP 6 chữ số, thực tế code sinh 4 chữ số (`1000` - `9999`).
- **Missing Confirm Password Field:** Giao diện thực tế `ForgotPassword.jsx` thiếu trường xác nhận mật khẩu mới (`confirmPassword`) so với SRS.
- **Missing Step Indicator:** Thiếu hiển thị trực quan chỉ báo "Bước 1 / 2" trên giao diện người dùng.

## 2. BVA Mistakes / Non-Compliances
- **Strict Whitespace Regex:** Biểu thức chính quy kiểm tra mật khẩu trong code thực tế yêu cầu bắt buộc có khoảng trắng (`(?=.*\s)`), khác so với chuẩn đặc tả chung (chỉ yêu cầu ký tự đặc biệt).
