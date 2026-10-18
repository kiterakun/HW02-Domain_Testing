# BVA — Representative Values (3-point)

**Feature:** Mã Giảm Giá (Discount Coupons) — FR-09

## 1. `total_amount` quanh ngưỡng `min_order_amount` (C3) — 3-point rule

| Mã | min−1 (fail) | min (pass theo SRS `>=`) | min+1 (pass) | Giá trị "xa" trong miền pass |
|---|---|---|---|---|
| `SAVE10` (M = 300.000) | 299.999 | **300.000** | 300.001 | 500.000 |
| `BIGBUY` (M = 500.000) | 499.999 | **500.000** | 500.001 | 600.000 |
| `EXPIRED` (M = 100.000) | 99.999 | **100.000** | 101.000 | 200.000 |

## 2. Lượt dùng quanh `max_uses_per_user` (C5) — 3-point rule

| Mã | count = max−1 (pass) | count = max (fail) | Ghi chú setup |
|---|---|---|---|
| `SAVE10` (max 1) | 0 | 1 | count 0 là mặc định sau seed; count 1 = user đã checkout thành công 1 lần với SAVE10 |
| `VIP100` (max 2) | 1 | 2 | count 1 = đã checkout 1 lần với VIP100; count 2 = đã checkout 2 lần |

## 3. Giới hạn thời gian (C2)

| Giá trị | Kỳ vọng |
|---|---|
| `now` << `expired_at` (SAVE10, BIGBUY, VIP100 — exp 2099-12-31, test tại 2026-10-18) | Pass C2 |
| `now` >> `expired_at` (`EXPIRED` — exp 2020-01-01) | Fail C2 |

## 4. Giá trị đại diện cho `code` (C1)

| Giá trị | Class | Lý do chọn |
|---|---|---|
| `SAVE10` | code-EC1 | Mã percent tiêu chuẩn, còn hạn, max 1 — dùng cho hầu hết tổ hợp |
| `BIGBUY` | code-EC1 | Mã fixed tiêu chuẩn — phân biệt công thức T2 |
| `VIP100` | code-EC1 | Mã fixed với max 2 — cho biên lượt dùng count = max−1 |
| `EXPIRED` | code-EC6 + time-EC2 | Tồn tại + active nhưng fail C2 |
| `INVALIDCODE` | code-EC2 | Mã không tồn tại |
| `""` / `"   "` | code-EC4 | Rỗng / toàn khoảng trắng |
| `save10` | code-EC5 | Sai chữ hoa/thường |
| Coupon `is_active = 0` | code-EC3 | Tồn tại nhưng bị vô hiệu (dựng bằng dữ liệu: Admin tạo rồi sửa CSDL / seed) |

## 5. Trạng thái đăng nhập (C4)

| Giá trị | Class | Lý do |
|---|---|---|
| JWT hợp lệ của `test@eshop.com` (id=2) | auth-EC1 | User tiêu chuẩn |
| Không gửi token (gọi trực tiếp POST /api/apply-coupon không header) | auth-EC2 | C4 yêu cầu JWT |
| Token sai chữ ký (malformed, VD `Bearer abc.xyz.123`) | auth-EC3 | Token không hợp lệ |

## 6. Người dùng (cho C5 per-user)

| User | Mục đích |
|---|---|
| `test@eshop.com` (id=2, seed) | User chính cho hầu hết test |
| User thứ 2 (tự đăng ký qua FR-01, VD `couponuser2@eshop.com`) | Chứng minh C5 per-user: user 2 chưa dùng SAVE10 → pass dù user 1 đã dùng |

## 7. Công thức (T1–T3) — bảng chuẩn (sau khi sửa lỗi trình bày ở Step 6)

| Tổ hợp mã × total | Loại | discount kỳ vọng (SRS) | final kỳ vọng |
|---|---|---|---|
| `SAVE10` × 500.000 (10%) | percent | **50.000** (= 500.000×10/100) | **450.000** |
| `BIGBUY` × 600.000 | fixed | **50.000** (= value) | **550.000** |
| `BIGBUY` × 500.000 (biên pass) | fixed | **50.000** | **450.000** |
| `VIP100` × 400.000 (count=1, lượt cuối) | fixed | **100.000** | **300.000** |
| `SAVE10` × 300.000 (biên pass) | percent | **30.000** | **270.000** |
| `SAVE10` × 300.001 (min+1, số lẻ 30.000,1) | percent | 30.000,1 → hành vi làm tròn cần xác minh (SRS không quy định) | 270.000,9 → kiểm chứng làm tròn |

## 8. Tổ hợp cảnh báo từ mã nguồn (kiểm chứng khi chạy test, không phải expected cứng)

- Mã nguồn `server.js:379` dùng `total_amount > min_order_amount` (strict `>`) thay vì `>=` như SRS → các test tại biên `total = min` (300.000 / 500.000) có khả năng phát hiện bug.
- Mã nguồn `server.js:363` không kiểm tra JWT và lấy `user_id` từ body → test B9 (không token) có khả năng phát hiện vi phạm C4.
