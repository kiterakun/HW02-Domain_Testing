# Domain Testing - Test Cases

**Feature:** FR-03 -- Forgot Password and Password Reset

## Test Case 1
**ID:** TC-DT-01  
**Precondition:** User đã đăng ký tài khoản  
**Input Data:** `email = "test@eshop.com"`  
**Steps:**  
1. Truy cập trang Quên mật khẩu  
2. Nhập email hợp lệ  
3. Bấm "Lấy mã OTP"  
**Expected Result:** Hiển thị thông báo chứa mã OTP, chuyển sang Bước 2 (Step Indicator = 2)  
**Reference:** Domain Testing Step 6, Class 1 (Valid email)

## Test Case 2
**ID:** TC-DT-02  
**Precondition:** Không có  
**Input Data:** `email = "notfound@eshop.com"`  
**Steps:**  
1. Nhập email chưa đăng ký  
2. Bấm "Lấy mã OTP"  
**Expected Result:** Hiển thị lỗi "User not found"  
**Reference:** Domain Testing Step 6, Class 2 (Non-existent email)

## Test Case 3
**ID:** TC-DT-03  
**Precondition:** Đã nhận mã OTP (VD: 1234)  
**Input Data:** `email = "test@eshop.com"`, `resetToken = "1234"`, `newPassword = "Abc12345!"`, `confirmPassword = "Abc12345!"`  
**Steps:**  
1. Nhập mã OTP chính xác  
2. Nhập mật khẩu mới hợp lệ và xác nhận khớp  
3. Bấm "Đặt lại mật khẩu"  
**Expected Result:** Thông báo "Đổi mật khẩu thành công!", chuyển hướng về trang đăng nhập  
**Reference:** Domain Testing Step 6, Happy Path

## Test Case 4
**ID:** TC-DT-04  
**Precondition:** Đã nhận mã OTP  
**Input Data:** `resetToken = "9999"` (Sai OTP)  
**Steps:**  
1. Nhập mã OTP sai  
2. Nhập mật khẩu hợp lệ  
3. Bấm "Đặt lại mật khẩu"  
**Expected Result:** Hiển thị lỗi "Mã OTP không đúng hoặc có lỗi xảy ra."  
**Reference:** Domain Testing Step 6, Invalid OTP

## Test Case 5
**ID:** TC-DT-05  
**Precondition:** Đã nhận mã OTP  
**Input Data:** `newPassword = "Ab1!"` (Dưới 8 ký tự)  
**Steps:**  
1. Nhập OTP đúng  
2. Nhập mật khẩu quá ngắn  
3. Bấm "Đặt lại mật khẩu"  
**Expected Result:** Hiển thị cảnh báo mật khẩu quá yếu, từ chối submit  
**Reference:** Domain Testing Step 6, Weak Password
