# Coverage Report — BVA

**Feature:** Mã Giảm Giá (Discount Coupons) — FR-09
**Test cases:** 4 (BVA-001 → BVA-004) trong `bva/test-cases.md`; giá trị biên tại `bva/bva-values.md`

## Equivalence Classes Coverage
(Phụ trách bởi Domain Testing — xem `domain-testing/coverage-report.md`; BVA bổ sung điểm biên trong các class total-EC2/EC3 và usage-EC1/EC2/EC3.)

## Boundaries Coverage

### min_order_amount — 3-point rule (min−1 / min / min+1)

| Mã | min−1 | min | min+1 | Test |
|---|---|---|---|---|
| `SAVE10` (300.000) | ✅ 299.999 (fail) | ✅ 300.000 (pass theo SRS `>=`) | ✅ 300.001 (pass, kiểm làm tròn 30.000,1) | BVA-001 |
| `BIGBUY` (500.000) | ✅ 499.999 (fail) | ✅ 500.000 (pass) | ✅ 500.001 (pass) | BVA-002 |
| `EXPIRED` (100.000) | ❌ | ❌ | ❌ | **Chưa phủ** — mọi total của EXPIRED fail tại C2 trước khi chạm C3 |

### max_uses_per_user — 3-point rule quanh max

| Mã | count = max−1 | count = max | Test |
|---|---|---|---|
| `SAVE10` (max 1) | ✅ 0 (pass) | ✅ 1 (fail) | BVA-004 |
| `VIP100` (max 2) | ✅ 1 (pass) | ✅ 2 (fail) | BVA-003 |

### Thời gian (now vs expired_at)
- ✅ now << expired_at (pass) và now >> expired_at (fail) — dùng SAVE10/EXPIRED seed
- ❌ **expired_at = now** (biên đúng ngày hết hạn): không dựng được bằng seed; cần Admin tạo coupon + phụ thuộc đồng hồ/múi giờ hệ thống → bỏ qua có chủ đích

### Biên kết quả tính toán
- ✅ final_amount = 0 / âm khi fixed value > total: TC-021 (Domain Testing)
- ✅ Làm tròn percent (total = min+1 → discount lẻ 30.000,1): BVA-001 — SRS không quy định chế độ làm tròn, ghi nhận hành vi thực

## Missing
- Biên ngưỡng EXPIRED (99.999 / 100.000 / 101.000): bị che bởi C2 (mã hết hạn bị từ chối trước khi so với ngưỡng) → không quan sát được qua hệ thống thực.
- Biên expired_at = now: ngoài tầm kiểm soát test (đồng hồ/múi giờ hệ thống) — đã phủ 2 bên đại diện.
- Biên trên của total_amount: SRS không định nghĩa max → không có biên trên để test.

## Summary
- 4 test BVA phủ trọn vẹn đường biên `min_order_amount` (3-point) của 2 mã còn hạn (SAVE10, BIGBUY) và biên `max_uses_per_user` (max−1/max) của 2 mã có cấu hình khác nhau (max 1 và max 2).
- Hai biên quan trọng nhất phát hiện bug tiềm ẩn: `total = min` (SUT dùng strict `>` thay vì `>=` như SRS → BVA-001/002 có khả năng fail đúng ở điểm biên) và `count = max` (khớp spec).
- Các khoảng trống đều do giới hạn kiểm soát hệ thống (mã hết hạn bị chặn sớm, thời gian thực, không có biên trên), không làm giảm khả năng phát hiện lỗi của bộ test.
