# Domain Testing - Test Cases

**Feature:** Mã Giảm Giá (Discount Coupons) — FR-09

> Expected Result dựa trên SRS (đặc tả nghiệp vụ đúng). Chuỗi thông báo trong ngoặc là tham chiếu từ SUT để đối chiếu khi chạy. Cột "Quan sát khi chạy" đánh dấu điểm nghi ngờ triển khai đã thấy trong mã nguồn (không thay đổi expected theo SRS).

## Quy ước chung

- **UI:** Đăng nhập `test@eshop.com / Test1234!` → thêm hàng vào giỏ đạt tổng yêu cầu → trang Checkout → nhập mã → bấm "Áp dụng".
- **API:** `POST http://localhost:3000/api/apply-coupon`, body `{"code": ..., "total_amount": ..., "user_id": 2}`. Các total chính xác đến 1₫ (299.999 / 300.000…) phải chạy qua API vì giỏ thực khó dựng đúng số.
- **Setup lượt dùng (C5):** `POST /api/coupon-usage` với JWT của user và `coupon_id` (SAVE10=1, BIGBUY=2, VIP100=3, EXPIRED=4), hoặc checkout thật. ⚠️ DB bị reset (drop + seed lại) mỗi khi restart backend.
- Coupon bị vô hiệu (TC-005) dựng bằng cách sửa CSDL `UPDATE coupons SET is_active=0 WHERE code='SAVE10OFF'` (mã do Admin tạo), khôi phục sau test.

## Test Case 1
**ID:** TC-001
**Precondition:** Đăng nhập user test; SAVE10 chưa được user dùng (count=0); backend đang chạy
**Input Data:** code=`SAVE10`, total_amount=500000, JWT hợp lệ
**Steps:**
1. Vào trang Checkout với giỏ hàng tổng 500.000 ₫
2. Nhập `SAVE10` vào ô mã giảm giá
3. Bấm "Áp dụng"
**Expected Result:** Áp dụng thành công — giảm 50.000 ₫ (= 500.000×10/100), thành tiền 450.000 ₫. UI hiển thị "Tiết kiệm 50.000 ₫" và "Thành tiền 450.000 ₫"
**Reference:** Step 5-A1; Equivalence Classes: code-EC1, total-EC1, auth-EC1, usage-EC1, calc-EC1 (T1)

## Test Case 2
**ID:** TC-002
**Precondition:** Đăng nhập; BIGBUY chưa dùng
**Input Data:** code=`BIGBUY`, total_amount=600000, JWT hợp lệ
**Steps:**
1. Checkout với giỏ 600.000 ₫
2. Nhập `BIGBUY` → "Áp dụng"
**Expected Result:** Thành công — giảm 50.000 ₫ (= value fixed), thành tiền 550.000 ₫
**Reference:** Step 5-A2; calc-EC3 (T2)

## Test Case 3
**ID:** TC-003
**Precondition:** Đăng nhập; user đã dùng VIP100 đúng 1 lần (count=1/2)
**Input Data:** code=`VIP100`, total_amount=400000, JWT hợp lệ
**Steps:**
1. Setup count=1 (gọi /api/coupon-usage 1 lần hoặc checkout 1 lần với VIP100)
2. Checkout với giỏ 400.000 ₫, áp `VIP100`
**Expected Result:** Thành công — giảm 100.000 ₫, thành tiền 300.000 ₫ (C5 pass: 1 < 2)
**Reference:** Step 5-A3; usage-EC2, calc-EC3

## Test Case 4
**ID:** TC-004
**Precondition:** Đăng nhập
**Input Data:** code=`INVALIDCODE`, total_amount=500000, JWT hợp lệ
**Steps:**
1. Checkout, nhập `INVALIDCODE` → "Áp dụng"
**Expected Result:** Từ chối, không giảm giá, thông báo mã không hợp lệ ("Mã giảm giá không tồn tại hoặc đã bị vô hiệu hóa")
**Reference:** Step 5-B1; code-EC2 (C1)

## Test Case 5
**ID:** TC-005
**Precondition:** Đăng nhập; tồn tại coupon `is_active=0` (dựng qua Admin + sửa CSDL, VD mã `SAVE10OFF`)
**Input Data:** code=`SAVE10OFF` (is_active=0), total_amount=500000, JWT hợp lệ
**Steps:**
1. Dựng coupon bị vô hiệu
2. Checkout, áp mã bị vô hiệu
**Expected Result:** Từ chối như mã không tồn tại (C1 fail)
**Reference:** Step 5-B2; code-EC3 (C1)

## Test Case 6
**ID:** TC-006
**Precondition:** Đăng nhập
**Input Data:** code=`""` và `"   "`, total_amount=500000 (chạy qua API vì UI disable nút khi rỗng)
**Steps:**
1. Gọi API apply-coupon với code rỗng
2. Lặp lại với code toàn khoảng trắng
**Expected Result:** Từ chối, thông báo yêu cầu nhập mã ("Vui lòng nhập mã giảm giá")
**Reference:** Step 5-B3; code-EC4 (C1/input)

## Test Case 7
**ID:** TC-007
**Precondition:** Đăng nhập
**Input Data:** code=`save10` (gọi API raw, không qua uppercase của UI), total_amount=500000
**Steps:**
1. Gọi API apply-coupon với code=`save10`
**Expected Result:** Từ chối — `save10` không có trong CSDL (so khớp chính xác) → thông báo không tồn tại
**Quan sát khi chạy:** frontend-web tự `.toUpperCase()` trước khi gửi → qua UI mã có thể vẫn pass (ghi nhận hành vi, không đổi expected)
**Reference:** Step 5-B4; code-EC5 (C1)

## Test Case 8
**ID:** TC-008
**Precondition:** Đăng nhập; EXPIRED chưa dùng
**Input Data:** code=`EXPIRED`, total_amount=200000 (≥ min 100.000), JWT hợp lệ
**Steps:**
1. Checkout với giỏ 200.000 ₫, áp `EXPIRED`
**Expected Result:** Từ chối, thông báo hết hạn ("Mã giảm giá đã hết hạn")
**Reference:** Step 5-B5; code-EC6 + time-EC2 (C2)

## Test Case 9
**ID:** TC-009
**Precondition:** Đăng nhập; SAVE10 chưa dùng
**Input Data:** code=`SAVE10`, total_amount=300000 (= min_order_amount), JWT hợp lệ
**Steps:**
1. Áp `SAVE10` với total đúng 300.000 ₫ (qua API)
**Expected Result:** Thành công theo SRS (`>=`) — giảm 30.000 ₫, thành 270.000 ₫
**Quan sát khi chạy:** mã nguồn dùng strict `>` (server.js:379) → có thể bị từ chối sai (bug dự kiến)
**Reference:** Step 5-B6; total-EC2 (C3 biên)

## Test Case 10
**ID:** TC-010
**Precondition:** Đăng nhập; SAVE10 chưa dùng
**Input Data:** code=`SAVE10`, total_amount=299999 (min−1), JWT hợp lệ
**Steps:**
1. Áp `SAVE10` với total 299.999 ₫ (qua API)
**Expected Result:** Từ chối, thông báo chưa đủ ngưỡng ("Đơn hàng chưa đủ giá trị tối thiểu 300.000 ₫...")
**Reference:** Step 5-B7; total-EC3 (C3 biên dưới)

## Test Case 11
**ID:** TC-011
**Precondition:** Đăng nhập
**Input Data:** code=`SAVE10`, total_amount=100000, JWT hợp lệ
**Steps:**
1. Áp `SAVE10` với total 100.000 ₫
**Expected Result:** Từ chối như TC-010 (thấp hơn ngưỡng nhiều)
**Reference:** Step 5-B8; total-EC4 (C3)

## Test Case 12
**ID:** TC-012
**Precondition:** Không đăng nhập
**Input Data:** API POST /api/apply-coupon, code=`SAVE10`, total_amount=500000, KHÔNG header Authorization
**Steps:**
1. Gọi API không token
**Expected Result:** Theo SRS C4 → 401 / từ chối, không áp dụng mã
**Quan sát khi chạy:** endpoint không kiểm tra JWT (server.js:363) → có thể áp dụng thành công (bug dự kiến)
**Reference:** Step 5-B9; auth-EC2 (C4)

## Test Case 13
**ID:** TC-013
**Precondition:** Không có phiên đăng nhập hợp lệ
**Input Data:** API với header `Authorization: Bearer abc.xyz.123`, code=`SAVE10`, total_amount=500000
**Steps:**
1. Gọi API với token malformed
**Expected Result:** 401 / từ chối
**Reference:** Step 5-B10; auth-EC3 (C4)

## Test Case 14
**ID:** TC-014
**Precondition:** Đăng nhập; user đã dùng SAVE10 1 lần (count=1/1)
**Input Data:** code=`SAVE10`, total_amount=500000, JWT hợp lệ
**Steps:**
1. Setup count=1 (coupon-usage hoặc checkout)
2. Áp `SAVE10` lần nữa
**Expected Result:** Từ chối, thông báo đã đạt giới hạn ("Bạn đã sử dụng mã này 1 lần (đã đạt giới hạn)")
**Reference:** Step 5-B11; usage-EC3 (C5)

## Test Case 15
**ID:** TC-015
**Precondition:** Đăng nhập; user đã dùng VIP100 2 lần (count=2/2)
**Input Data:** code=`VIP100`, total_amount=400000, JWT hợp lệ
**Steps:**
1. Setup count=2
2. Áp `VIP100` lần nữa
**Expected Result:** Từ chối — đã đạt giới hạn 2 lần
**Reference:** Step 5-B12; usage-EC3 (C5)

## Test Case 16
**ID:** TC-016
**Precondition:** Đăng nhập; BIGBUY chưa dùng
**Input Data:** code=`BIGBUY`, total_amount=500000 (= min), JWT hợp lệ
**Steps:**
1. Áp `BIGBUY` với total 500.000 ₫ (qua API)
**Expected Result:** Thành công theo SRS — giảm 50.000 ₫, thành 450.000 ₫
**Quan sát khi chạy:** same strict `>` như TC-009
**Reference:** Step 5-C1; total-EC2 (C3 biên)

## Test Case 17
**ID:** TC-017
**Precondition:** Đăng nhập
**Input Data:** code=`BIGBUY`, total_amount=499999 (min−1), JWT hợp lệ
**Steps:**
1. Áp `BIGBUY` với total 499.999 ₫
**Expected Result:** Từ chối, thông báo chưa đủ 500.000 ₫
**Reference:** Step 5-C2; total-EC3 (C3 biên dưới)

## Test Case 18
**ID:** TC-018
**Precondition:** Đăng nhập
**Input Data:** code=`EXPIRED`, total_amount=50000 (< min 100.000 của EXPIRED), JWT hợp lệ
**Steps:**
1. Áp `EXPIRED` với total 50.000 ₫
**Expected Result:** Từ chối (vi phạm cả C2 lẫn C3); ghi nhận thông báo cụ thể
**Quan sát khi chạy:** mã nguồn lồng C2 trong nhánh C3 → trả "chưa đủ ngưỡng" thay vì "hết hạn" (thứ tự ưu tiên thông báo)
**Reference:** Step 5-C3; time-EC2 + total-EC4 (C2+C3)

## Test Case 19
**ID:** TC-019
**Precondition:** Đăng nhập
**Input Data:** code=`EXPIRED`, total_amount=200000 (đủ ngưỡng), JWT hợp lệ
**Steps:**
1. Áp `EXPIRED` với total 200.000 ₫
**Expected Result:** Từ chối với thông báo **hết hạn** (khác TC-018 → chứng minh phân tách điều kiện)
**Reference:** Step 5-C4; time-EC2 (C2)

## Test Case 20
**ID:** TC-020
**Precondition:** Đăng nhập
**Input Data:** code=`INVALIDCODE`, total_amount=100000, JWT hợp lệ
**Steps:**
1. Áp mã không tồn tại với total dưới ngưỡng
**Expected Result:** Từ chối với lỗi C1 (mã không tồn tại) — C1 được kiểm trước, không bị lỗi ngưỡng che
**Reference:** Step 5-C5; code-EC2 + total-EC4 (C1 ưu tiên)

## Test Case 21
**ID:** TC-021
**Precondition:** Admin tạo coupon `FIXBIG`: type=fixed, discount_value=500000, min_order_amount=0, expired_at=2099-12-31, max_uses_per_user=1 (POST /api/admin/coupons)
**Input Data:** code=`FIXBIG`, total_amount=100000, JWT user test
**Steps:**
1. Admin tạo coupon FIXBIG
2. User áp `FIXBIG` với giỏ 100.000 ₫
**Expected Result:** Hệ thống không được tạo final_amount âm — từ chối mã hoặc giảm tối đa bằng total (final = 0)
**Quan sát khi chạy:** mã nguồn không clamp → final = −400.000 (bug dự kiến); đồng thời quan sát FR-17 không validate
**Reference:** Step 5-C6; calc-EC4 (T2/T3)

## Test Case 22
**ID:** TC-022
**Precondition:** User A (`test@eshop.com`) đã dùng SAVE10 1 lần; User B (mới đăng ký qua FR-01, VD `couponuser2@eshop.com`) chưa dùng
**Input Data:** User B áp code=`SAVE10`, total_amount=500000, JWT của user B
**Steps:**
1. User A dùng SAVE10 1 lần
2. User B đăng nhập, áp `SAVE10` trên giỏ 500.000 ₫
**Expected Result:** Thành công — giảm 50.000 ₫, thành 450.000 ₫ (lượt dùng tính per-user: count của B = 0 < 1)
**Reference:** Step 5-C7; usage-EC1 (C5 per-user)

## Test Case 23
**ID:** TC-023
**Precondition:** Đăng nhập; giỏ 500.000 ₫; SAVE10 chưa dùng
**Input Data:** `SAVE10` → final 450.000 ₫ → thanh toán
**Steps:**
1. Áp `SAVE10` thành công (giảm 50.000)
2. Bấm thanh toán
3. Xem lịch sử đơn hàng
**Expected Result:** Đơn hàng được tạo với tổng 450.000 ₫; lượt dùng SAVE10 của user +1
**Quan sát khi chạy:** web không xóa giỏ sau thanh toán (FR-08) — ghi nhận riêng
**Reference:** Step 5-D1; R8 (T3 + tích hợp checkout)

## Test Case 24
**ID:** TC-024
**Precondition:** Sau TC-023 (user đã dùng SAVE10 1 lần)
**Input Data:** code=`SAVE10`, total_amount=500000, JWT hợp lệ
**Steps:**
1. Áp `SAVE10` lần 2
**Expected Result:** Từ chối — đã đạt giới hạn 1 lần
**Reference:** Step 5-D2; usage-EC3 (C5 tích hợp)

---

## Ma trận phủ nhanh

- C1: TC-004, TC-005, TC-006, TC-007, TC-020
- C2: TC-008, TC-018, TC-019
- C3: TC-009, TC-010, TC-011, TC-016, TC-017, TC-018
- C4: TC-012, TC-013
- C5: TC-003, TC-014, TC-015, TC-022, TC-024
- T1 (percent): TC-001, TC-009
- T2 (fixed): TC-002, TC-003, TC-016, TC-021
- T3 (final): mọi test thành công + TC-023

---

## KẾT QUẢ THỰC THI (18-10-2026 — backend đã reset + seed lại; chi tiết tại `execution/execution-results.md`)

| ID | Trạng thái | Kết quả thực tế (tóm tắt) |
|---|---|---|
| TC-001 | **Fail** | 200 `success:true` nhưng discount **−4.500.000**, final **5.000.000** → BUG-001 (công thức percent sai) |
| TC-002 | Pass | discount 50.000, final 550.000 — đúng |
| TC-003 | Pass | discount 100.000, final 300.000 (count=1 < 2) — đúng |
| TC-004 | Pass | 404 "Mã giảm giá không tồn tại hoặc đã bị vô hiệu hóa" |
| TC-005 | Pass | 404 như mã không tồn tại (coupon SAVE10OFF `is_active=0` xác nhận qua DB) |
| TC-006 | Pass | ""→400 "Vui lòng nhập mã"; "   "→404 "không tồn tại" (vẫn từ chối đúng C1, thông điệp khác — ghi chú) |
| TC-007 | Pass | 404 "không tồn tại" (API raw; UI tự uppercase trước khi gửi) |
| TC-008 | Pass | 400 "Mã giảm giá đã hết hạn" |
| TC-009 | **Fail** | 400 "chưa đủ giá trị tối thiểu 300,000 ₫" dù total=300.000 = min → BUG-002 (dùng `>` thay vì `>=`) |
| TC-010 | Pass | 400 "chưa đủ 300,000 ₫" |
| TC-011 | Pass | 400 "chưa đủ 300,000 ₫" |
| TC-012 | **Fail** | 200 `success:true` khi không có JWT → BUG-003 (C4 thiếu) + BUG-004 (C5 bỏ qua khi không có user_id) |
| TC-013 | **Fail** | 200 `success:true` với `Bearer abc.xyz.123` → BUG-003 |
| TC-014 | Pass | 400 "Bạn đã sử dụng mã này 1 lần (đã đạt giới hạn)" |
| TC-015 | Pass | 400 "Bạn đã sử dụng mã này 2 lần (đã đạt giới hạn)" |
| TC-016 | **Fail** | 400 "chưa đủ 500,000 ₫" dù total=500.000 = min → BUG-002 |
| TC-017 | Pass | 400 "chưa đủ 500,000 ₫" |
| TC-018 | Pass (ghi chú) | Từ chối đúng nhưng thông báo "chưa đủ 100,000 ₫" thay vì "hết hạn" → BUG-006 (sai ưu tiên lỗi) |
| TC-019 | Pass | 400 "Mã giảm giá đã hết hạn" |
| TC-020 | Pass | 404 "không tồn tại" (C1 kiểm trước) |
| TC-021 | **Fail** | 200 `success:true` với final **−400.000** → BUG-005 (không clamp final âm) |
| TC-022 | **Fail** | C5 per-user đúng (user B pass) nhưng amounts sai giống TC-001 → BUG-001 |
| TC-023 | **Fail** | Đơn #1 lưu total **5.000.000** (= final sai của BUG-001); usage +1 ghi nhận đúng cơ chế |
| TC-024 | Pass | 400 "…1 lần (đã đạt giới hạn)" |

**Tổng:** 24 đã chạy · Pass 16 · Fail 8 · Blocked 0 · Not Executed 0.
