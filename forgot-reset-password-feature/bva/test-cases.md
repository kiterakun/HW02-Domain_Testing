# Boundary Value Analysis - Test Cases

**Feature:** FR-03 -- Forgot Password and Password Reset (Password Length Boundary)

## Test Case 1
**ID:** TC-BVA-01  
**Precondition:** Đã nhận mã OTP  
**Input Data:** `newPassword = "Abc12!"` (7 ký tự - Min - 1)  
**Steps:**  
1. Nhập OTP hợp lệ  
2. Nhập mật khẩu có độ dài 7 ký tự  
3. Bấm Đặt lại mật khẩu  
**Expected Result:** Hệ thống báo lỗi mật khẩu quá yếu (chưa đạt tối thiểu 8 ký tự).  
**Reference:** BVA - Length boundary Min-1

## Test Case 2
**ID:** TC-BVA-02  
**Precondition:** Đã nhận mã OTP  
**Input Data:** `newPassword = "Abc1234!"` (8 ký tự - Min)  
**Steps:**  
1. Nhập OTP hợp lệ  
2. Nhập mật khẩu đúng 8 ký tự kèm ký tự đặc biệt/hoa/thường/số  
3. Bấm Đặt lại mật khẩu  
**Expected Result:** Đổi mật khẩu thành công.  
**Reference:** BVA - Length boundary Min

## Test Case 3
**ID:** TC-BVA-03  
**Precondition:** Đã nhận mã OTP  
**Input Data:** `newPassword = "Abc12345!"` (9 ký tự - Min + 1)  
**Steps:**  
1. Nhập OTP hợp lệ  
2. Nhập mật khẩu 9 ký tự hợp lệ  
3. Bấm Đặt lại mật khẩu  
**Expected Result:** Đổi mật khẩu thành công.  
**Reference:** BVA - Length boundary Min+1
