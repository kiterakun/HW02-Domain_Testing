# FR-09 — Bằng chứng thực thi test case (Execution Log)

**Feature:** Mã Giảm Giá (Discount Coupons) — FR-09
**Ngày thực thi:** 18-10-2026 (khoảng 11:53–11:57 giờ máy, `created_at` của đơn: `2026-10-18 11:54:55`)
**Môi trường:** Windows; Node v24.10.0; Backend EShop (`eshop-sut/backend`, Express + SQLite) chạy tại `http://localhost:3000`; SQLite `database.sqlite`
**Điều kiện tiền đề đảm bảo:** Backend **khởi động lại ngay trước khi chạy** → `initDatabase()` DROP toàn bộ bảng và seed lại (`database.js:13-117`) → trạng thái đảm bảo: coupon_usage rỗng, đúng 2 user (admin id=1, test id=2), 4 coupon seed như SRS.

## Setup (Phase A)

| Bước | Kết quả |
|---|---|
| `GET /api/products` | 200 — server sẵn sàng |
| `POST /api/login` `test@eshop.com / Test1234!` | 200 — user id=2, JWT hợp lệ |
| `POST /api/login` `admin@eshop.com / Admin123!` | 200 — admin id=1 |
| `POST /api/register` `couponuser2@eshop.com` | 200 — user B id=3 |
| `GET /api/coupons` (JWT user) | 200 — SAVE10 id=1 (percent 10%, min 300.000, exp 2099-12-31, max 1); BIGBUY id=2 (fixed 50.000, min 500.000, max 1); VIP100 id=3 (fixed 100.000, min 300.000, max 2); EXPIRED id=4 (percent 20%, min 100.000, exp 2020-01-01, max 1) |

**Phương thức thực thi:** gọi trực tiếp API `POST /api/apply-coupon` — mô phỏng đúng payload của frontend-web `Checkout.jsx` (`{code, total_amount, user_id}`, KHÔNG gửi header Authorization vì frontend không gửi). Test C4 (TC-012/013) kiểm tra riêng việc thiếu/sai JWT. Setup lượt dùng qua `POST /api/coupon-usage` (JWT user, `coupon_id` tương ứng) — đúng thứ tự frontend gọi sau checkout.

## Bảng kết quả tổng hợp (34 điểm kiểm thử trên 28 test case + 2 probe)

> QĐ = Kỳ vọng theo SRS; TT = Kết quả thực tế; ✓ = khớp SRS (Pass), ✗ = sai lệch SRS (Fail).

### Domain Testing — 24 test case (FR09-DT-01…24 = TC-001…024)

| TC | Kịch bản | Input | QĐ (SRS) | TT (API) | Đánh giá |
|---|---|---|---|---|---|
| TC-001 | SAVE10 hợp lệ percent | code=SAVE10, total=500.000, user_id=2 | Pass, giảm 50.000 → 450.000 | 200 `success:true`, **discount −4.500.000, final 5.000.000**, msg "Giảm 10%" | ✗ **Fail — BUG-001** |
| TC-002 | BIGBUY hợp lệ fixed | total=600.000 | Pass, giảm 50.000 → 550.000 | 200, discount 50.000, final 550.000 | ✓ Pass |
| TC-003 | VIP100 count=1 (max 2) | total=400.000 | Pass, giảm 100.000 → 300.000 | 200, discount 100.000, final 300.000 | ✓ Pass |
| TC-004 | Mã không tồn tại | code=INVALIDCODE, total=500.000 | Từ chối "không tồn tại" | 404 "Mã giảm giá không tồn tại hoặc đã bị vô hiệu hóa" | ✓ Pass |
| TC-005 | Mã is_active=0 (SAVE10OFF) | total=500.000 | Từ chối như không tồn tại | 404 (cùng thông báo); DB xác nhận `is_active=0` | ✓ Pass |
| TC-006 | Rỗng / toàn khoảng trắng | code="" và "   " | Từ chối, yêu cầu nhập mã | "" → 400 "Vui lòng nhập mã giảm giá"; "   " → 404 "không tồn tại" (vẫn từ chối đúng C1, thông điệp khác) | ✓ Pass (ghi chú) |
| TC-007 | Sai hoa/thường | code=save10 | Từ chối (không có trong CSDL) | 404 "không tồn tại" | ✓ Pass |
| TC-008 | Hết hạn, đủ ngưỡng | EXPIRED, total=200.000 | 400 "hết hạn" | 400 "Mã giảm giá đã hết hạn" | ✓ Pass |
| TC-009 | Đơn = đúng ngưỡng (biên) | SAVE10, total=300.000 | **Pass theo SRS `>=`: giảm 30.000 → 270.000** | 400 "chưa đủ giá trị tối thiểu 300,000 ₫" | ✗ **Fail — BUG-002** |
| TC-010 | Thiếu 1₫ | total=299.999 | Từ chối ngưỡng | 400 "chưa đủ 300,000 ₫" | ✓ Pass |
| TC-011 | Thấp hơn nhiều | total=100.000 | Từ chối | 400 "chưa đủ 300,000 ₫" | ✓ Pass |
| TC-012 | Không có JWT | không header, không user_id | **401 / từ chối (C4)** | **200 `success:true`, giảm "thành công"** — cả C5 cũng bị bỏ qua | ✗ **Fail — BUG-003 + BUG-004** |
| TC-013 | JWT sai | `Authorization: Bearer abc.xyz.123` | 403/401 | 200 `success:true` (header bị bỏ qua hoàn toàn) | ✗ **Fail — BUG-003** |
| TC-014 | Hết lượt max=1 | SAVE10, count=1 | 400 "đã đạt giới hạn" | 400 "Bạn đã sử dụng mã này 1 lần (đã đạt giới hạn)" | ✓ Pass |
| TC-015 | Hết lượt max=2 | VIP100, count=2 | 400 "đã đạt giới hạn" | 400 "Bạn đã sử dụng mã này 2 lần (đã đạt giới hạn)" | ✓ Pass |
| TC-016 | BIGBUY = đúng ngưỡng | total=500.000 | Pass theo SRS `>=`: giảm 50.000 → 450.000 | 400 "chưa đủ 500,000 ₫" | ✗ **Fail — BUG-002** |
| TC-017 | BIGBUY thiếu 1₫ | total=499.999 | Từ chối | 400 "chưa đủ 500,000 ₫" | ✓ Pass |
| TC-018 | Hết hạn + dưới ngưỡng (2 lỗi) | EXPIRED, total=50.000 | Từ chối (ghi nhận thông báo) | 400 "chưa đủ 100,000 ₫" **thay vì** "hết hạn" | ✓ Pass (từ chối đúng; thông báo sai ưu tiên → BUG-006) |
| TC-019 | Chỉ hết hạn | EXPIRED, total=200.000 | 400 "hết hạn" | 400 "Mã giảm giá đã hết hạn" | ✓ Pass |
| TC-020 | Không tồn tại + dưới ngưỡng | INVALIDCODE, total=100.000 | Lỗi C1 (không tồn tại) | 404 "không tồn tại" (C1 kiểm trước) | ✓ Pass |
| TC-021 | Fixed > total → final âm | FIXBIG (fixed 500.000, min 0) trên đơn 100.000 | Không được final âm (từ chối hoặc clamp ≥ 0) | 200 `success:true`, **final −400.000** | ✗ **Fail — BUG-005** |
| TC-022 | Per-user: B chưa dùng | user_id=3 (id B), SAVE10 | Pass, giảm 50.000 → 450.000 (count của B = 0) | 200 `success:true` (**C5 per-user đúng**) nhưng discount −4.500.000 / final 5.000.000 | ✗ **Fail — BUG-001** (C5 per-user: đúng) |
| TC-023 | e2e: áp mã → checkout | SAVE10 → final trả về → checkout → kiểm đơn | Đơn lưu tổng 450.000; usage +1 | Đơn #1 lưu **total 5.000.000** (bằng `final_amount` sai trả về); usage SAVE10 +1 ghi nhận đúng cơ chế | ✗ **Fail — BUG-001 lan vào đơn** |
| TC-024 | Dùng lại sau checkout | SAVE10, count=1 | 400 "đã đạt giới hạn" | 400 "…1 lần (đã đạt giới hạn)" | ✓ Pass |

### BVA — 11 điểm kiểm thử (4 nhóm: FR09-BVA-01…03 = BVA-001; …04…06 = BVA-002; …07…09 = BVA-003; …10…11 = BVA-004)

| Điểm | Biến/Biên | Giá trị | QĐ (SRS) | TT (API) | Đánh giá |
|---|---|---|---|---|---|
| BVA-01 | SAVE10 min−1 | 299.999 | Từ chối | 400 "chưa đủ 300,000 ₫" | ✓ Pass |
| BVA-02 | SAVE10 min | 300.000 | **Đậu (`>=`): giảm 30.000 → 270.000** | 400 "chưa đủ 300,000 ₫" | ✗ **Fail — BUG-002** |
| BVA-03 | SAVE10 min+1 | 300.001 | Đậu; giảm ≈30.000,1 (ghi nhận làm tròn) | 200, **discount −2.700.009, final 3.000.010** | ✗ **Fail — BUG-001** |
| BVA-04 | BIGBUY min−1 | 499.999 | Từ chối | 400 "chưa đủ 500,000 ₫" | ✓ Pass |
| BVA-05 | BIGBUY min | 500.000 | Đậu (`>=`): giảm 50.000 → 450.000 | 400 "chưa đủ 500,000 ₫" | ✗ **Fail — BUG-002** |
| BVA-06 | BIGBUY min+1 | 500.001 | Đậu: giảm 50.000 → 450.001 | 200, discount 50.000, final 450.001 | ✓ Pass |
| BVA-07 | VIP100 count=0 | count=0, total=400.000 | Đậu: giảm 100.000 → 300.000 | 200, discount 100.000, final 300.000 | ✓ Pass |
| BVA-08 | VIP100 count=max−1 | count=1 | Đậu như trên | 200, discount 100.000, final 300.000 | ✓ Pass |
| BVA-09 | VIP100 count=max | count=2 | Từ chối "giới hạn" | 400 "…2 lần (đã đạt giới hạn)" | ✓ Pass |
| BVA-10 | SAVE10 count=max−1 (max=1 → 0) | count=0, total=500.000 | Đậu: giảm 50.000 → 450.000 | 200, **discount −4.500.000, final 5.000.000** | ✗ **Fail — BUG-001** |
| BVA-11 | SAVE10 count=max | count=1 | Từ chối "giới hạn" | 400 "…1 lần (đã đạt giới hạn)" | ✓ Pass |

### Probe (kiểm chứng bổ sung ngoài 28 TC, phục vụ bug report)

| Probe | Kịch bản | Kết quả | Ý nghĩa |
|---|---|---|---|
| Probe-1 | Sau khi user 2 đã dùng SAVE10 (count=1, bị từ chối ở TC-024), gọi lại API **không gửi user_id** | 200 `success:true`, giảm "thành công" | **BUG-004**: bỏ `user_id` trong body → bỏ qua kiểm tra C5 → dùng mã vô hạn lần |
| Probe-2 | `POST /api/checkout` với `total_amount: 1` | 200, đơn #2 được tạo với `total_amount = 1` | **BUG-007 (FR-08 liên quan)**: backend tin tuyệt đối total do client gửi, không tự tính lại (`server.js:297-309`) |

### Setup dữ liệu đặc biệt (bằng chứng)

- TC-005: `POST /api/admin/coupons` (JWT admin) tạo `SAVE10OFF` (percent 10, min 100.000, exp 2099-12-31) → id=5; UPDATE trực tiếp SQLite: `UPDATE coupons SET is_active=0 WHERE code='SAVE10OFF'` (changes=1); `GET /api/coupons` xác nhận `is_active: 0`; áp mã → 404.
- TC-021: tạo `FIXBIG` (fixed 500.000, min 0, exp 2099-12-31) → id=6; áp trên đơn 100.000 → 200 `success:true`, `final_amount: -400000`.
- Setup lượt dùng VIP100: 2 lần `POST /api/coupon-usage` (coupon_id=3, JWT user id=2) → đều 200 "Usage recorded".

## Thống kê

| Nhóm | Số test case/điểm | Pass | Fail | Blocked | Not Executed |
|---|---|---|---|---|---|
| Domain Testing (FR09-DT-01…24) | 24 | 16 | 8 | 0 | 0 |
| BVA (FR09-BVA-01…11) | 11 | 7 | 4 | 0 | 0 |
| **Tổng FR-09** | **35** | **23** | **12** | **0** | **0** |

**Nhóm Fail (12):** BUG-001 (percent): TC-001, TC-022, TC-023, BVA-03, BVA-10 · BUG-002 (C3 `>`): TC-009, TC-016, BVA-02, BVA-05 · BUG-003 (C4 thiếu JWT): TC-012, TC-013 · BUG-005 (final âm): TC-021.
**Pass đúng theo SRS (23):** toàn bộ nhánh từ chối C1/C2/C5, công thức fixed, biên max−1/max lượt dùng, ưu tiên C1, per-user C5 (TC-022 phần C5).

## Bug xác nhận qua thực thi (FR-09)

| Bug ID | Tiêu đề | Severity | Bằng chứng (file:line) |
|---|---|---|---|
| BUG-001 | **Công thức percent sai hoàn toàn**: `discount = floor(total × (1 − value))` thay vì `total × value / 100` → discount ÂM, final tăng ~10× nền | Critical | `server.js:398-401, 418-421` |
| BUG-002 | **C3 dùng `>` thay vì `>=`**: đơn đúng bằng ngưỡng bị từ chối | High | `server.js:379` |
| BUG-003 | **C4 thiếu hoàn toàn**: `POST /api/apply-coupon` không yêu cầu JWT; thiếu/sai token vẫn áp dụng thành công | Critical | `server.js:363` |
| BUG-004 | **C5 bypass được**: không gửi `user_id` trong body → bỏ qua kiểm tra giới hạn lượt dùng | High | `server.js:364, 386` |
| BUG-005 | **`final_amount` âm khi discount > total** (fixed), không clamp ≥ 0 | Medium | `server.js:406` |
| BUG-006 | **Sai ưu tiên thông báo lỗi**: coupon hết hạn + thiếu ngưỡng → trả lỗi ngưỡng (C2 kiểm tra lồng trong nhánh C3) | Low | `server.js:379-384` (TC-018) |
| BUG-007* | *FR-08 liên quan*: checkout lưu tổng tiền do client gửi (không tự tính lại, `coupon_id` không lưu) | Medium | `server.js:297-309` (Probe-2) |

\* Ghi nhận trong quá trình test FR-09; thuộc feature FR-08 — trình bày chi tiết trong mục FR-08.

## Ghi chú tái lập

1. Khởi động lại backend (`node server.js` trong `eshop-sut/backend`) — DB tự reset + seed lại.
2. Đăng nhập lấy JWT: `POST /api/login {email:'test@eshop.com', password:'Test1234!'}`.
3. Thực hiện các lượt gọi theo thứ tự trong log này (trạng thái usage count phụ thuộc thứ tự: VIP100 cần 2 lần `coupon-usage`, SAVE10 cần 1 lần sau checkout).
4. Script chạy test: `discount-coupons-feature/execution/run-fr09-tests.js` (nếu cần tái lập).
```
