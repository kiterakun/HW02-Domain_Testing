# Coverage Report - BVA

## Boundaries Coverage
- Password Length (Min = 8): ✅ Đã kiểm thử đầy đủ 3 điểm biên (Min-1, Min, Min+1).

## Summary
Phân tích giá trị biên (BVA) cho thấy hệ thống thực thi đúng ngưỡng giới hạn 8 ký tự của mật khẩu, tuy nhiên biểu thức regex kiểm tra thực tế yêu cầu thêm khoảng trắng (`\s`), dẫn đến khác biệt nhỏ so với SRS chung.
