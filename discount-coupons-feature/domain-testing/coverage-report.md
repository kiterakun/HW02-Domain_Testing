# Coverage Report — Domain Testing

**Feature:** Mã Giảm Giá (Discount Coupons) — FR-09
**Test cases:** 24 (TC-001 → TC-024) trong `domain-testing/test-cases.md`

## Equivalence Classes Coverage

| Biến / Điều kiện | Classes phủ | Chi tiết (Test IDs) |
|---|---|---|
| `code` (C1) | **6/6** | EC1 hợp lệ (TC-001/002/003), EC2 không tồn tại (TC-004), EC3 bị vô hiệu (TC-005), EC4 rỗng (TC-006), EC5 sai hoa/thường (TC-007), EC6 tồn tại nhưng hết hạn (TC-008) |
| `total_amount` (C3) | **4/4** | EC1 đủ ngưỡng (TC-001/002/003), EC2 đúng bằng ngưỡng (TC-009/016), EC3 thiếu 1₫ (TC-010/017), EC4 thấp hơn nhiều (TC-011/018) |
| Trạng thái đăng nhập (C4) | **3/3** | EC1 JWT hợp lệ (TC-001…), EC2 không token (TC-012), EC3 token sai (TC-013) |
| Lượt dùng (C5) | **3/4** | EC1 count=0 (TC-001), EC2 count=max−1 (TC-003), EC3 count=max (TC-014/015/024); EC4 count>max **chưa phủ** |
| Hạn dùng (C2) | **2/3** | EC1 còn hạn lâu (TC-001…), EC2 hết hạn từ lâu (TC-008/018/019); EC3 hết hạn đúng hôm nay **chưa phủ** |
| Công thức (T1/T2/T3) | **4/4** | EC1 percent 10% (TC-001/009), EC3 fixed < total (TC-002/003/016), EC4 fixed > total (TC-021), T3 final trên mọi test thành công + TC-023; EC2 percent giá trị lớn chỉ phủ gián tiếp (EXPIRED bị chặn ở C2) |
| Tổ hợp điều kiện (R9) | **5/7 nhóm** | All-pass (A1–A3), single-fault đủ cả 5 điều kiện (B1–B12), đa lỗi C2+C3 (TC-018), C1 ưu tiên (TC-020), per-user (TC-022), end-to-end (TC-023/024) |

## Boundaries Coverage
- min_order_amount (SAVE10 300.000 / BIGBUY 500.000): ✅ min−1, min, min+1 (TC-009/010/016/017 + BVA-001/002)
- max_uses_per_user (SAVE10 1: count 0/1; VIP100 2: count 1/2): ✅ max−1, max (TC-003/014/015 + BVA-003/004)
- final_amount biên âm (fixed > total): ✅ TC-021
- Thời gian (now vs expired_at): ✅ 2 điểm đại diện 2 bên

## Missing
- **usage-EC4 (count > max):** chỉ dựng được bằng ghi trực tiếp CSDL `coupon_usage`; hành vi kỳ vọng trùng với count = max (từ chối) → chọn count = max làm đại diện, đánh đổi chấp nhận.
- **time-EC3 (expired_at = now):** cần Admin tạo coupon hết hạn đúng hôm nay; phụ thuộc đồng hồ/múi giờ hệ thống, không kiểm soát ổn định → bỏ qua có chủ đích, đã phủ 2 bên xung quanh.
- **BVA ngưỡng EXPIRED (100.000):** mọi total của EXPIRED fail tại C2 trước khi chạm C3 → không quan sát được qua hệ thống thực.
- **Percent giá trị lớn (20–99%) còn hạn:** không có coupon seed phù hợp → chỉ phủ gián tiếp; có thể bổ sung bằng Admin tạo coupon percent 99% (test tùy chọn, ngoài danh sách chính).
- **Biên trên total_amount:** SRS không định nghĩa max → không có biên trên để test; dùng 600.000 làm giá trị "xa".

## Summary
- 24 test case Domain Testing phủ **22/25 equivalence classes (≈88%)** và 5/6 nhóm biên quan trọng.
- Cả 5 điều kiện C1–C5 đều có ≥ 1 test pass và ≥ 1 test fail (C2 fail chỉ dựng được qua EXPIRED seed) → phân biệt được thông báo lỗi theo từng điều kiện.
- Các khoảng trống đều nêu rõ lý do (cần can thiệp CSDL / phụ thuộc thời gian hệ thống / bị che bởi thứ tự kiểm tra) và được chấp nhận cho phạm vi HW02.
- Điểm nghi ngờ triển khai lớn nhất cần quan sát khi chạy: C3 dùng `>` thay vì `>=` (TC-009/016), C4 không kiểm tra JWT (TC-012), công thức percent sai (TC-001), final âm khi fixed > total (TC-021).
