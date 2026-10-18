# FR-09 -- Discount coupons (nội dung đã điền cho mục 2.2 -- ĐÃ THỰC THI)

> **Cách dùng:** copy toàn bộ phần dưới đây (từ `## 2.2.` đến hết mục 2.2.7) thay thế cho mục 2.2 tương ứng trong `HW02_Domain_Testing_Report.md`.
>
> **Nguồn nội dung:** tài liệu trong `discount-coupons-feature/` (feature-overview.md, domain-testing/, bva/, ai-mistake.md), đã đối chiếu với mã nguồn SUT: `eshop-sut/backend/server.js` (`POST /api/apply-coupon` dòng 363--441, `POST /api/coupon-usage` dòng 443--454, `POST /api/checkout` dòng 297--309, `POST /api/admin/coupons` dòng 457--481), `eshop-sut/backend/database.js` (bảng `coupons`/`coupon_usage`, seed dòng 106--111), `eshop-sut/frontend-web/src/pages/Checkout.jsx`.
>
> **Thực thi:** đã chạy toàn bộ 35 điểm kiểm thử (24 Domain Testing + 11 điểm BVA) ngày 18-10-2026 trên backend local (`http://localhost:3000`, DB reset + seed lại trước khi chạy).

---

## 2.2. FR-09 -- Discount coupons

### 2.2.1. Mô tả chức năng và yêu cầu

- **Mục đích chức năng:** Tại bước Checkout, người dùng nhập mã giảm giá và bấm "Áp dụng". Hệ thống kiểm tra đồng thời 5 điều kiện (C1--C5); nếu tất cả thỏa mãn thì tính tiền giảm theo công thức percent/fixed, hiển thị "Tiết kiệm" / "Thành tiền", và khi thanh toán, đơn hàng được tạo với tổng đã giảm.
- **Tiền điều kiện:** Người dùng đã đăng nhập (JWT hợp lệ); giỏ hàng có tổng tiền xác định; mã giảm giá tồn tại trong CSDL (`coupons`); bản ghi lượt dùng lưu trong bảng `coupon_usage`.
- **Quy tắc giảm giá (đã xác minh từ SRS + mã nguồn):**
  - C1: mã có trong CSDL và `is_active = 1`; C2: ngày hiện tại < `expired_at`; C3: `total_amount >= min_order_amount`; C4: JWT hợp lệ; C5: số lần dùng của user < `max_uses_per_user`.
  - `percent`: `discount_amount = total × discount_value / 100`; `fixed`: `discount_amount = discount_value`; `final_amount = total − discount_amount`.
  - Dữ liệu seed: `SAVE10` (percent 10%, min 300.000, max 1), `BIGBUY` (fixed 50.000, min 500.000, max 1), `VIP100` (fixed 100.000, min 300.000, max 2), `EXPIRED` (percent 20%, min 100.000, exp 2020-01-01, max 1).
- **Nguồn yêu cầu:** SRS FR-09 (v2.0, `eshop-sut/README.md` dòng 110--136); API `eshop-sut/backend/server.js` dòng 363--454; giao diện `eshop-sut/frontend-web/src/pages/Checkout.jsx` (ô input "Nhập mã giảm giá...", nút "Áp dụng", khối "Tiết kiệm"/"Thành tiền").
- **Giả định cần xác minh (đã đối chiếu mã nguồn trước khi chạy, xác nhận qua thực thi):**
  1. Endpoint `POST /api/apply-coupon` không yêu cầu JWT và nhận `user_id` từ body của client (`server.js:363-364`) -- nghi ngờ vi phạm C4/C5.
  2. Điều kiện C3 so sánh `total_amount > min_order_amount` (strict, `server.js:379`) -- nghi ngờ sai so với SRS (`>=`).
  3. Công thức percent trong mã là `Math.floor(total_amount * (1 - discount_value))` (`server.js:398-401, 418-421`) -- nghi ngờ sai hoàn toàn so với `total × value / 100`.
  4. Kiểm tra C2 lồng bên trong nhánh thỏa C3 (`server.js:379-384`) -- nghi ngờ sai thứ tự ưu tiên thông báo lỗi.
  5. `final_amount` không được clamp ≥ 0; checkout lưu nguyên `total_amount` client gửi (`server.js:297-309`) -- ảnh hưởng R8.

### 2.2.2. Domain Testing -- phân tích miền

| Biến/điều kiện | Miền/lớp hợp lệ | Miền/lớp không hợp lệ | Căn cứ | Cần xác minh? |
| --- | --- | --- | --- | --- |
| Mã coupon (`code`) | Khớp chính xác coupon trong CSDL, `is_active=1`, còn hạn: `SAVE10`, `BIGBUY`, `VIP100` | (a) Không tồn tại (`INVALIDCODE`); (b) tồn tại nhưng `is_active=0`; (c) rỗng/toàn khoảng trắng; (d) sai chữ hoa/thường (`save10`); (e) tồn tại + active nhưng hết hạn (`EXPIRED` -- pass C1, fail C2) | SRS FR-09 C1; `server.js:370` (`WHERE code = ? AND is_active = 1`); seed `database.js:106-111` | Có -- đã xác minh qua thực thi |
| Trạng thái coupon | `is_active = 1` | `is_active = 0` (dựng qua Admin API + UPDATE SQLite) | `server.js:370`; FR-17 | Có -- đã xác minh |
| Thời hạn sử dụng (`expired_at` vs `now`) | `now < expired_at` (SAVE10/BIGBUY/VIP100: 2099-12-31) | `now >= expired_at` (EXPIRED: 2020-01-01 → fail C2) | SRS FR-09 C2; `server.js:382` | Có (thứ tự ưu tiên với C3) -- đã xác minh |
| Giá trị giỏ hàng (`total_amount` vs `min_order_amount` M) | `total ≥ M` (300.000 / 500.000 / 100.000 tùy mã) | `total < M` (299.999; 499.999; 100.000 − 1₫...) | SRS FR-09 C3; `server.js:379` | Có (nghi ngờ `>` thay vì `>=`) -- đã xác minh, sai |
| Giới hạn sử dụng (`usage_count` vs `max_uses_per_user`) | `count < max` (SAVE10 max 1: count 0; VIP100 max 2: count 0, 1) | `count ≥ max` (SAVE10: 1; VIP100: 2) | SRS FR-09 C5; `server.js:386-395`; bảng `coupon_usage` | Có (per-user; bypass khi thiếu `user_id`) -- đã xác minh |
| Trạng thái đăng nhập (JWT) | JWT hợp lệ kèm yêu cầu (C4) | Không có token; token sai (`Bearer abc.xyz.123`) | SRS FR-09 C4; `server.js:363` | Có (nghi ngờ endpoint bỏ check) -- đã xác minh, sai |
| Kiểu/mức giảm (`type`, `discount_value`) | percent 1--99% (`SAVE10` 10%); fixed > 0, nhỏ hơn total (`BIGBUY` 50.000) | fixed lớn hơn total → final âm (dựng `FIXBIG`); value ngoài miền chỉ tạo được qua Admin API không validate | SRS FR-09 công thức; `server.js:397-406` | Có (công thức percent; final âm) -- đã xác minh, sai |
| Trạng thái áp dụng/ghi nhận | Áp mã (không trừ lượt) → checkout → `POST /api/coupon-usage` ghi lượt → lần sau bị chặn nếu đạt max | Áp mã lặp lại không tạo lượt; bỏ gọi endpoint ghi lượt → cũng như chưa dùng | `Checkout.jsx:40-66` (gọi usage sau checkout); `server.js:443-454` | Có -- đã xác minh |

**Giải thích phân hoạch miền:** Mỗi mã coupon là một "profile" điều kiện (type, value, min_order_amount, expired_at, max_uses), nên `code` vừa là biến vừa quyết định tham số của các điều kiện còn lại. Các lớp được tách khi kỳ vọng đầu ra khác nhau: success JSON (discount/final) vs từng loại thông báo lỗi (không tồn tại / hết hạn / chưa đủ ngưỡng / đã đạt giới hạn). Lớp biên "total = min" được tách riêng khỏi "total > min" vì SRS quy định `>=` -- đây là điểm nghi ngờ triển khai được kiểm tra riêng qua BVA. Tổ hợp được ưu tiên: (1) single-fault từng điều kiện C1--C5 để xác định thông báo lỗi riêng; (2) biên 3 điểm quanh min_order_amount và max_uses_per_user; (3) tương tác giữa hai lỗi (C2+C3, C1+C3) để quan sát thứ tự ưu tiên; (4) e2e áp mã → checkout → ghi lượt.

### 2.2.3. Test case Domain Testing

| Test Case ID | Lớp miền/điều kiện | Tiền điều kiện | Dữ liệu đầu vào | Các bước thực hiện | Kết quả mong đợi | Kết quả thực tế | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FR09-DT-01 | Coupon percent hợp lệ (all-pass) | Đăng nhập (user 2); SAVE10 chưa dùng | `code=SAVE10, total=500.000` | 1. POST /api/apply-coupon `{code, total_amount, user_id:2}` (payload như frontend) | Thành công; giảm 50.000; thành 450.000; UI "Tiết kiệm 50.000 ₫" | 200 `success:true` nhưng `discount_amount = −4.500.000`, `final_amount = 5.000.000` (tăng ~10 lần), msg "Áp dụng thành công! Giảm 10%" | **Fail (BUG-001)** |
| FR09-DT-02 | Coupon fixed hợp lệ | User 2; BIGBUY chưa dùng | `code=BIGBUY, total=600.000` | Như DT-01 | Giảm 50.000; thành 550.000 | 200; discount 50.000; final 550.000, msg "Giảm 50,000 ₫" | Pass |
| FR09-DT-03 | Lượt cuối (count = max−1) | User 2 đã dùng VIP100 1 lần (insert qua /api/coupon-usage) | `code=VIP100, total=400.000` | 1. Ghi 1 lượt dùng 2. Áp VIP100 | Đậu (1 < 2); giảm 100.000; thành 300.000 | 200; discount 100.000; final 300.000 | Pass |
| FR09-DT-04 | Mã không tồn tại (C1) | User 2 | `code=INVALIDCODE, total=500.000` | Áp mã | Từ chối "Mã giảm giá không tồn tại hoặc đã bị vô hiệu hóa" | 404 "Mã giảm giá không tồn tại hoặc đã bị vô hiệu hóa" | Pass |
| FR09-DT-05 | Coupon bị vô hiệu (C1) | Tạo `SAVE10OFF` qua Admin API; UPDATE SQLite `is_active=0` (changes=1) | `code=SAVE10OFF, total=500.000` | 1. Xác nhận `is_active=0` qua GET /api/coupons 2. Áp mã | Từ chối như mã không tồn tại | 404 (cùng thông báo) | Pass |
| FR09-DT-06 | Mã rỗng / khoảng trắng | User 2 | `code=""` và `code="   "` | Áp mã (API) | Từ chối, yêu cầu nhập mã | `""`→400 "Vui lòng nhập mã giảm giá" (khớp); `"   "`→404 "không tồn tại" (vẫn từ chối đúng C1, thông điệp khác) | Pass (ghi chú) |
| FR09-DT-07 | Sai chữ hoa/thường | User 2 | `code=save10` (API raw) | Áp mã | Từ chối (không khớp CSDL) | 404 "không tồn tại" | Pass |
| FR09-DT-08 | Mã hết hạn (C2) | User 2; EXPIRED chưa dùng | `code=EXPIRED, total=200.000` (≥ min 100.000) | Áp mã | Từ chối "Mã giảm giá đã hết hạn" | 400 "Mã giảm giá đã hết hạn" | Pass |
| FR09-DT-09 | Đơn đúng bằng ngưỡng (C3 biên) | User 2; SAVE10 chưa dùng | `code=SAVE10, total=300.000` | Áp mã (API) | **Đậu theo SRS `>=`**, giảm 30.000, thành 270.000 | 400 "Đơn hàng chưa đủ giá trị tối thiểu 300,000 ₫" | **Fail (BUG-002)** |
| FR09-DT-10 | Thiếu đúng 1₫ | User 2 | `code=SAVE10, total=299.999` | Áp mã | Từ chối ngưỡng | 400 "chưa đủ 300,000 ₫" | Pass |
| FR09-DT-11 | Thấp hơn ngưỡng nhiều | User 2 | `code=SAVE10, total=100.000` | Áp mã | Từ chối | 400 "chưa đủ 300,000 ₫" | Pass |
| FR09-DT-12 | Không có JWT (C4) | Chưa xác thực | `POST /api/apply-coupon` `{code=SAVE10, total=500.000}` -- không header Authorization, không user_id | Áp mã (API trực tiếp) | 401 / từ chối theo C4 | 200 `success:true` -- mã được áp "thành công" (C5 cũng bị bỏ qua vì thiếu user_id) | **Fail (BUG-003, BUG-004)** |
| FR09-DT-13 | JWT sai (C4) | Token malformed | `Authorization: Bearer abc.xyz.123` | Áp mã | 401/403 | 200 `success:true` (header bị bỏ qua hoàn toàn) | **Fail (BUG-003)** |
| FR09-DT-14 | Hết lượt max=1 (C5) | User 2 đã dùng SAVE10 1 lần (sau checkout DT-23) | `code=SAVE10, total=500.000, user_id=2` | Áp mã lần nữa | Từ chối "đã đạt giới hạn" | 400 "Bạn đã sử dụng mã này 1 lần (đã đạt giới hạn)" | Pass |
| FR09-DT-15 | Hết lượt max=2 (C5) | User 2 đã dùng VIP100 2 lần (2 insert) | `code=VIP100, total=400.000, user_id=2` | Áp mã | Từ chối "đã đạt giới hạn" | 400 "Bạn đã sử dụng mã này 2 lần (đã đạt giới hạn)" | Pass |
| FR09-DT-16 | BIGBUY đúng bằng ngưỡng | User 2; BIGBUY chưa dùng | `code=BIGBUY, total=500.000` | Áp mã | Đậu theo SRS `>=`; giảm 50.000, thành 450.000 | 400 "chưa đủ giá trị tối thiểu 500,000 ₫" | **Fail (BUG-002)** |
| FR09-DT-17 | BIGBUY thiếu 1₫ | User 2 | `code=BIGBUY, total=499.999` | Áp mã | Từ chối | 400 "chưa đủ 500,000 ₫" | Pass |
| FR09-DT-18 | 2 lỗi cùng lúc (C2+C3), quan sát ưu tiên | User 2 | `code=EXPIRED, total=50.000` (< min) | Áp mã | Từ chối; ghi nhận thông báo (SRS không cố định ưu tiên) | 400 "chưa đủ giá trị tối thiểu 100,000 ₫" -- **trả lỗi ngưỡng thay vì "hết hạn"** (C2 nằm trong nhánh C3) | Pass (từ chối đúng; sai lệch ưu tiên → BUG-006) |
| FR09-DT-19 | Chỉ sai hạn (C2) | User 2 | `code=EXPIRED, total=200.000` | Áp mã | 400 "hết hạn" (phân biệt với DT-18) | 400 "Mã giảm giá đã hết hạn" | Pass |
| FR09-DT-20 | C1 ưu tiên (không tồn tại + dưới ngưỡng) | User 2 | `code=INVALIDCODE, total=100.000` | Áp mã | Lỗi C1 (mã không tồn tại), không bị lỗi ngưỡng che | 404 "không tồn tại" | Pass |
| FR09-DT-21 | Fixed > total → final âm | Admin tạo `FIXBIG` (fixed 500.000, min 0); user 2 | `code=FIXBIG, total=100.000` | 1. Tạo coupon 2. Áp mã | Hệ thống không được tạo final âm (từ chối hoặc clamp `discount ≤ total`, final ≥ 0) | 200 `success:true`, `discount_amount=500.000`, `final_amount=−400.000` | **Fail (BUG-005)** |
| FR09-DT-22 | C5 per-user (user khác) | User A (id 2) đã dùng SAVE10; User B (id 3, đăng ký `couponuser2@eshop.com`) chưa dùng | `code=SAVE10, total=500.000, user_id=3` | Áp mã với user B | Đậu: lượt dùng của B = 0 < 1; giảm 50.000, thành 450.000 | 200 `success:true` (C5 per-user đúng) nhưng amounts sai như DT-01 (discount −4.500.000 / final 5.000.000) | **Fail (BUG-001; phần C5 đúng)** |
| FR09-DT-23 | e2e: áp mã → checkout → đơn lưu tổng đã giảm | User 2; SAVE10 chưa dùng | `code=SAVE10, total=500.000, user_id=2` | 1. Áp SAVE10 lấy `final_amount` 2. POST /api/checkout (JWT) `{total_amount: final}` 3. GET /api/orders/my-orders 4. POST /api/coupon-usage | Đơn được tạo với tổng 450.000; lượt dùng +1 | Đơn #1 lưu `total_amount=5.000.000` (bằng `final_amount` sai trả về); usage SAVE10 ghi nhận → DT-14 bị chặn đúng | **Fail (BUG-001 lan vào đơn; cơ chế ghi lượt đúng)** |
| FR09-DT-24 | Dùng lại mã đã dùng sau checkout | Sau DT-23 (count=1) | `code=SAVE10, total=500.000, user_id=2` | Áp mã lần 2 | Từ chối "đã đạt giới hạn" | 400 "…1 lần (đã đạt giới hạn)" | Pass |

**Đánh giá Domain Testing:** 24/24 lớp miền đã thiết kế đều được kiểm tra: Pass 16, Fail 8. Các lớp fail đều do 2 nhóm lỗi SUT: (1) công thức percent sai (DT-01/22/23); (2) C3 dùng `>` thay vì `>=` (DT-09/16); ngoài ra C4 không được thực thi (DT-12/13) và final âm (DT-21). Lớp "hết hạn đúng hôm nay" và "count > max" không đưa vào danh sách chính (lý do tại 2.2.7); lớp "percent > 10% còn hạn" chỉ phủ gián tiếp.

### 2.2.4. BVA -- phân tích biên

| Biến/điều kiện | Biên dưới | Biên trên | Căn cứ xác định biên | Giá trị cần thử | Ghi chú |
| --- | --- | --- | --- | --- | --- |
| Giá trị đơn hàng tối thiểu (SAVE10, min 300.000) | 300.000 | Không có | SRS C3 (`>=`); seed `database.js:107` | min−1 = 299.999; min = 300.000; min+1 = 300.001 | min+1 kèm kiểm tra hành vi làm tròn (10% của 300.001 = 30.000,1) |
| Giá trị đơn hàng tối thiểu (BIGBUY, min 500.000) | 500.000 | Không có | SRS C3; seed `database.js:108` | 499.999; 500.000; 500.001 | -- |
| Giá trị đơn tối thiểu (EXPIRED, min 100.000) | 100.000 | Không có | seed `database.js:110` | Không thử riêng | Mọi total của EXPIRED đều fail C2 trước khi chạm C3 → biên không quan sát được qua hệ thống thực |
| Số lần sử dụng (VIP100, max 2) | count 0 | count 2 | SRS C5 (`count < max`); seed `database.js:109` | count = 0; 1 (max−1); 2 (max) | Dựng count qua 2 lần `POST /api/coupon-usage` |
| Số lần sử dụng (SAVE10, max 1) | count 0 (= max−1 vì max=1) | count 1 | SRS C5 | count = 0; 1 | count=1 dựng tự nhiên qua checkout FR09-DT-23 |
| Thời hạn coupon (`expired_at` vs now) | `now` (còn hạn/hết hạn) | -- | SRS C2 | now ≪ 2099-12-31 (đậu); now ≫ 2020-01-01 (trượt) | Biên `expired_at = now` bỏ qua (ngoài tầm kiểm soát đồng hồ/múi giờ hệ thống) |
| Mức giảm tối đa / final ≥ 0 | final = 0 | -- | Logic nghiệp vụ (final = total − discount không âm) | fixed 500.000 trên đơn 100.000 (`FIXBIG`) | SRS không quy định tường minh; kỳ vọng nghiệp vụ không cho final âm |

**Giải thích BVA:** Hai nhóm biên chính được chọn 3 điểm: ngưỡng đơn hàng (min−1/min/min+1 cho SAVE10 và BIGBUY -- kỳ vọng: min−1 bị từ chối, min và min+1 được chấp nhận theo SRS `>=`) và giới hạn lượt dùng (max−1/max cho VIP100 max=2 và SAVE10 max=1 -- kỳ vọng: max−1 đậu, max bị chặn). Các giá trị biên được đưa trực tiếp qua API vì giao diện chỉ nhận tổng giỏ tổng hợp. Biên thời gian chỉ thử nổi 2 điểm đại diện 2 bên do dữ liệu seed; biên "hết hạn đúng ngày" và biên ngưỡng của EXPIRED bị che bởi điều kiện C2 nên không thiết kế test case, đã ghi Missing trong coverage.

### 2.2.5. Test case BVA

| Test Case ID | Biến/biên kiểm thử | Giá trị | Tiền điều kiện | Các bước thực hiện | Kết quả mong đợi | Kết quả thực tế | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FR09-BVA-01 | SAVE10 ngưỡng -- dưới biên | 299.999 | User 2; count=0 | Áp `SAVE10` total 299.999 | Từ chối "chưa đủ 300.000 ₫" | 400 "chưa đủ giá trị tối thiểu 300,000 ₫" | Pass |
| FR09-BVA-02 | SAVE10 ngưỡng -- tại biên | 300.000 | User 2; count=0 | Áp `SAVE10` total 300.000 | Đậu (`>=`): giảm 30.000, thành 270.000 | 400 "chưa đủ 300,000 ₫" | **Fail (BUG-002)** |
| FR09-BVA-03 | SAVE10 ngưỡng -- trên biên | 300.001 | User 2; count=0 | Áp `SAVE10` total 300.001 | Đậu; giảm ≈30.000,1 (ghi nhận làm tròn); thành ≈270.001 | 200 nhưng discount **−2.700.009**, final **3.000.010** | **Fail (BUG-001)** |
| FR09-BVA-04 | BIGBUY ngưỡng -- dưới biên | 499.999 | User 2; count=0 | Áp `BIGBUY` | Từ chối "chưa đủ 500.000 ₫" | 400 "chưa đủ 500,000 ₫" | Pass |
| FR09-BVA-05 | BIGBUY ngưỡng -- tại biên | 500.000 | User 2; count=0 | Áp `BIGBUY` | Đậu (`>=`): giảm 50.000, thành 450.000 | 400 "chưa đủ 500,000 ₫" | **Fail (BUG-002)** |
| FR09-BVA-06 | BIGBUY ngưỡng -- trên biên | 500.001 | User 2; count=0 | Áp `BIGBUY` | Đậu: giảm 50.000, thành 450.001 | 200; discount 50.000; final 450.001 | Pass |
| FR09-BVA-07 | VIP100 lượt -- dưới biên (count=0) | count 0, total 400.000 | User 2 chưa dùng VIP100 | Áp `VIP100` | Đậu: giảm 100.000, thành 300.000 | 200; discount 100.000; final 300.000 | Pass |
| FR09-BVA-08 | VIP100 lượt -- tại max−1 (count=1) | count 1 | Đã ghi 1 lượt (coupon-usage) | Áp `VIP100` | Đậu (1 < 2): giảm 100.000, thành 300.000 | 200; discount 100.000; final 300.000 | Pass |
| FR09-BVA-09 | VIP100 lượt -- tại max (count=2) | count 2 | Đã ghi 2 lượt | Áp `VIP100` | Từ chối "đã đạt giới hạn" | 400 "Bạn đã sử dụng mã này 2 lần (đã đạt giới hạn)" | Pass |
| FR09-BVA-10 | SAVE10 lượt -- dưới biên (count=0, max=1) | count 0, total 500.000 | User 2 chưa dùng SAVE10 | Áp `SAVE10` | Đậu: giảm 50.000, thành 450.000 | 200 nhưng discount **−4.500.000**, final **5.000.000** | **Fail (BUG-001)** |
| FR09-BVA-11 | SAVE10 lượt -- tại max (count=1) | count 1 | Sau checkout FR09-DT-23 | Áp `SAVE10` | Từ chối "đã đạt giới hạn" | 400 "…1 lần (đã đạt giới hạn)" | Pass |

> Ánh xạ kiểm thử → thiết kế: FR09-BVA-01…03 = BVA-001; 04…06 = BVA-002; 07…09 = BVA-003; 10…11 = BVA-004 (trong `bva/test-cases.md`).

### 2.2.6. Kết quả thực thi và bằng chứng

- Tổng test case Domain Testing: **24** (đã thực thi 24/24)
- Tổng điểm kiểm thử BVA: **11** (gộp trong 4 nhóm BVA-001…004; đã thực thi 11/11)
- Pass: **23** | Fail: **12** | Blocked: **0** | Not Executed: **0**
  - DT: Pass 16 / Fail 8 (DT-01, 09, 12, 13, 16, 21, 22, 23)
  - BVA: Pass 7 / Fail 4 (BVA-02, 03, 05, 10)
- Bug liên quan (chi tiết tại Chương 3):
  - **BUG-001 (Critical):** công thức percent sai hoàn toàn → discount âm, final tăng ~10× (DT-01/22/23, BVA-03/10)
  - **BUG-002 (High):** C3 dùng `>` thay vì `>=` → đơn đúng bằng ngưỡng bị từ chối (DT-09/16, BVA-02/05)
  - **BUG-003 (Critical):** C4 không được cài đặt -- áp mã không cần JWT (DT-12/13)
  - **BUG-004 (High):** C5 bypass được khi bỏ `user_id` trong body (DT-12 + probe)
  - **BUG-005 (Medium):** `final_amount` âm không bị chặn (DT-21)
  - **BUG-006 (Low):** sai thứ tự ưu tiên thông báo khi vi phạm C2+C3 (DT-18)
  - **BUG-007 (Medium, FR-08 liên quan):** checkout tin tổng tiền client gửi (probe: tạo đơn total=1) -- phát hiện trong quá trình test FR-09
- Nhận xét: Kết quả thực tế khớp **100%** với các điểm "Quan sát khi chạy" đã đánh dấu từ bước phân tích mã nguồn (6/6 điểm nghi ngờ đều thành hiện thực). Cơ chế hoạt động đúng: toàn bộ nhánh từ chối C1/C2/C5, công thức fixed, biên max−1/max, tính per-user của C5, thứ tự ưu tiên C1, và việc ghi lượt dùng sau checkout. Sai lệch nghiêm trọng nhất là công thức percent: SAVE10 trên đơn 500.000₫ trả về `discount_amount = −4.500.000` và `final_amount = 5.000.000` -- về bản chất là **tăng giá 10 lần** thay vì giảm 10%, và con số này được ghi nguyên vào đơn hàng (DT-23).

### 2.2.7. Đánh giá feature

- **Độ bao phủ lớp miền và biên:** Domain Testing phủ 22/25 lớp (88%): thiếu "hết hạn đúng hôm nay" (ngoài tầm kiểm soát đồng hồ/múi giờ), "count > max" (chỉ dựng được bằng ghi trực tiếp CSDL; hành vi trùng count = max), "percent 20--99% còn hạn" (không có seed; phủ gián tiếp qua EXPIRED). BVA phủ 5/6 nhóm biên: thiếu biên ngưỡng EXPIRED (bị C2 che) và biên `expired_at = now` (đã nêu).
- **Rủi ro còn lại:**
  1. Bug percent (BUG-001) là lỗi nghiêm trọng của tính năng: mọi mã percent đang seed (SAVE10, EXPIRED) đều không thể dùng đúng; rủi ro tài chính và uy tín nếu lên production.
  2. Kết hợp BUG-003 + BUG-004 + không verify `total_amount`: một caller ẩn danh có thể áp mã bất kỳ, bỏ qua giới hạn lượt, và checkout với tổng tùy ý (probe tạo đơn total=1) -- lỗ hổng kinh doanh/bảo mật ngoài phạm vi SRS nhưng cần ghi nhận.
  3. `coupon_id` không được lưu vào đơn hàng (`orders` không có cột) → không thể truy vết doanh thu giảm giá theo mã; lượt dùng phụ thuộc frontend tự gọi `/api/coupon-usage` sau checkout -- một client khác ý muốn có thể bỏ qua endpoint này để "không bao giờ hết lượt".
  4. DB reset + seed lại mọi khi restart backend: số liệu đếm lượt chỉ có ý nghĩa trong một phiên chạy; không ảnh hưởng test case nhưng ảnh hưởng vận hành demo.
- **Các trường hợp chưa kiểm:** percent 20--99% còn hạn; coupon hết hạn đúng ngày; count > max; áp nhiều mã trong cùng đơn (SRS chỉ hỗ trợ 1 mã/đơn -- không có sẵn UI, bỏ qua); làm tròn chính xác của percent (không thể chạy do BUG-001 chồng lên mọi kịch bản percent).
- **Đóng góp hữu ích của AI:** phân tích 5 điều kiện C1--C5, bảng lớp tương đương theo "single-fault" và bộ giá trị 3 điểm dày đặc (11 điểm biên/4 nhóm) phát hiện trọn bộ 6 bug FR-09 trong một phiên chạy; các cảnh báo "Quan sát khi chạy" từ rà soát mã nguồn khớp 100% kết quả thực thi, giúp kỳ vọng kiểm thử tách bạch SRS-so-với-SUT.
- **Đầu ra AI chưa chính xác/sai sót** (tổng hợp từ `discount-coupons-feature/ai-mistake.md`):
  1. Step 6: trình bày lặp 3 bản bảng "giá trị đại diện công thức" với ký tự lỗi và nhầm `BIGBUY × 500.000` thành percent 10% -- đã phát hiện, thay bằng bảng chuẩn trước khi thiết kế.
  2. Step 7: file test-cases.md bị 2 dòng text thừa (trước TC-09 và TC-18) -- đã phát hiện và sửa; nội dung 28 test case không bị ảnh hưởng.
- **Phần người kiểm thử đã bổ sung:** xác minh trực tiếp mã nguồn (`server.js`, `database.js`, `Checkout.jsx`) trước khi chạy; thêm 2 probe (bypass C5 bằng bỏ `user_id`; checkout total=1) để xác nhận BUG-004 và ghi nhận BUG-007; thiết lập dữ liệu đặc biệt (coupon `is_active=0` qua SQLite; `FIXBIG` qua Admin API) cho các lớp không có seed.

---

## Số liệu dùng cho các bảng tổng hợp (không copy vào mục 2.2)

Khi cập nhật bảng 2.5 và bảng 6.2 của báo cáo, dùng các số sau cho FR-09:

- Test case thiết kế -- Domain Testing: **24**
- Test case thiết kế -- BVA: **11** (4 nhóm biên, 11 điểm)
- Tổng test case thiết kế: **35**
- Đã thực thi: **35** | Pass: **23** | Fail: **12** | Blocked: **0** | Not Executed: **0**
- Bug xác nhận (FR-09): **6** (BUG-001 Critical, BUG-002 High, BUG-003 Critical, BUG-004 High, BUG-005 Medium, BUG-006 Low); BUG-007 Medium ghi nhận cho FR-08.
