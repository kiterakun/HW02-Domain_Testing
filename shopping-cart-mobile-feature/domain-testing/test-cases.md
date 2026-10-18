# Domain Testing - Test Cases

**Feature:** FR-07 — Giỏ hàng (Shopping Cart) — Mobile App (React Native + Expo)
**SUT:** `eshop-sut/frontend-mobile/App.js` | **Yêu cầu:** FR-07 (+ FR-06, FR-21, FR-23, FR-24)
**Dữ liệu seed:** SP-A = iPhone 15 Pro Max (30.000.000 ₫, id=1), SP-B = AirPods Pro 2 (6.000.000 ₫, id=4), SP-C = Bàn phím Keychron Q1 (4.000.000 ₫, id=5)

## Ma trận ánh xạ Tổ hợp → Test Case

| Tổ hợp (Bước 5) | Test Case |
|---|---|
| C1 — Giỏ trống + thêm SP mới | TC-001, TC-002 |
| C2 — Thêm cùng SP (cộng dồn) | TC-003 |
| C3 — Thêm SP khác | TC-004 |
| C4 — add_quantity không hợp lệ | TC-005 → TC-009, TC-018 |
| C5/C6 — cart_quantity hợp lệ / không hợp lệ | TC-010, TC-011 |
| C7 — Xóa dòng cuối → giỏ trống | TC-012 |
| C8 — Xóa khi còn ≥2 dòng | TC-013 |
| C9 — Thành tiền / Tổng cộng / định dạng ₫ | TC-014, TC-015 |
| C10 — Giỏ trống (empty state) | TC-016 |
| C11 — Guest dùng giỏ | TC-017 |
| C12 — Badge cập nhật | TC-019 |
| Yêu cầu UI FR-07 (cột, +/-, dialog, nhãn) | TC-020 → TC-023 |

---

## Test Case 1
**ID:** TC-001
**Precondition:** Ứng mobile đã kết nối API; giỏ hàng trống (mới mở app).
**Input Data:** SP-A (iPhone 15 Pro Max, 30.000.000 ₫); thêm nhanh từ trang chủ (qty mặc định = 1).
**Steps:**
1. Mở trang chủ, tìm thẻ sản phẩm SP-A.
2. Bấm "Thêm vào giỏ" trên thẻ SP-A.
3. Mở giỏ hàng (bấm "Giỏ" trên header).
**Expected Result:** Alert "Thành công — Đã thêm vào giỏ hàng"; giỏ có đúng 1 dòng SP-A với Số lượng = 1, Đơn giá "30.000.000 ₫", Thành tiền "30.000.000 ₫".
**Reference:** FR-07 (hiển thị giỏ), FR-06 (phản hồi thêm vào giỏ), EC1 (add_quantity hợp lệ), C1.

## Test Case 2
**ID:** TC-002
**Precondition:** Giỏ hàng trống.
**Input Data:** SP-B (AirPods Pro 2, 6.000.000 ₫); màn chi tiết SP, nhập số lượng `2`.
**Steps:**
1. Từ trang chủ bấm "Xem chi tiết" SP-B.
2. Nhập Số lượng = 2.
3. Bấm "Thêm vào giỏ hàng".
4. Mở giỏ hàng.
**Expected Result:** Giỏ có 1 dòng SP-B với Số lượng = 2; Thành tiền = "12.000.000 ₫".
**Reference:** FR-06/FR-07; EC1; C1.

## Test Case 3
**ID:** TC-003
**Precondition:** Giỏ đã có SP-A số lượng 1 (sau TC-001).
**Input Data:** Thêm SP-A lần 2 từ trang chủ (qty = 1).
**Steps:**
1. Ở trang chủ bấm "Thêm vào giỏ" trên thẻ SP-A.
2. Mở giỏ hàng.
**Expected Result:** Vẫn **1 dòng** SP-A, Số lượng tăng lên 2; **không** tạo dòng mới.
**Reference:** FR-07 ("Thêm cùng một sản phẩm... tăng số lượng, không tạo dòng mới"); BR-1; V5 lớp cộng dồn; C2.

## Test Case 4
**ID:** TC-004
**Precondition:** Giỏ có 1 dòng SP-A (qty=2).
**Input Data:** SP-B (AirPods Pro 2) — sản phẩm khác, thêm qty = 1 từ trang chủ.
**Steps:**
1. Bấm "Thêm vào giỏ" trên thẻ SP-B.
2. Mở giỏ hàng.
**Expected Result:** Giỏ có 2 dòng: SP-A (qty=2) và SP-B (qty=1); dòng SP-A không bị ảnh hưởng.
**Reference:** FR-07; V2 (SP khác → dòng mới); C3.

## Test Case 5
**ID:** TC-005
**Precondition:** Giỏ có 1 dòng SP-B (qty=1).
**Input Data:** Màn chi tiết SP-C, nhập Số lượng = `0`, bấm "Thêm vào giỏ hàng".
**Steps:**
1. Mở chi tiết SP-C (Keychron Q1, 4.000.000 ₫).
2. Nhập Số lượng = 0.
3. Bấm "Thêm vào giỏ hàng" và kiểm tra giỏ.
**Expected Result (SRS):** Hệ thống không chấp nhận 0 — yêu cầu số nguyên dương tối thiểu 1 hoặc xử lý an toàn về 1; không thêm 0 sản phẩm; không crash.
**Reference:** FR-06 (min = 1); EC2 (invalid 0); C4.

## Test Case 6
**ID:** TC-006
**Precondition:** Giỏ có ≥1 dòng.
**Input Data:** Màn chi tiết SP, nhập Số lượng = `-1`, bấm "Thêm vào giỏ hàng".
**Steps:**
1. Mở chi tiết một sản phẩm.
2. Nhập Số lượng = -1.
3. Bấm "Thêm vào giỏ hàng" và kiểm tra giỏ.
**Expected Result (SRS):** Không chấp nhận số âm; xử lý an toàn (từ chối hoặc ép về 1); không tạo số lượng âm trong giỏ.
**Reference:** FR-06; EC3 (số âm); C4.

## Test Case 7
**ID:** TC-007
**Precondition:** Giỏ có ≥1 dòng.
**Input Data:** Màn chi tiết SP, xóa trắng ô Số lượng (rỗng), bấm "Thêm vào giỏ hàng".
**Steps:**
1. Mở chi tiết một sản phẩm.
2. Xóa nội dung ô Số lượng.
3. Bấm "Thêm vào giỏ hàng" và kiểm tra giỏ.
**Expected Result (SRS):** Không thêm hàng với số lượng rỗng; xử lý an toàn (từ chối hoặc ép về 1); không crash.
**Reference:** FR-06; EC4 (rỗng); C4.

## Test Case 8
**ID:** TC-008
**Precondition:** Giỏ có ≥1 dòng.
**Input Data:** Màn chi tiết SP, nhập Số lượng = `abc`, bấm "Thêm vào giỏ hàng".
**Steps:**
1. Mở chi tiết một sản phẩm.
2. Nhập "abc" vào ô Số lượng.
3. Bấm "Thêm vào giỏ hàng" và kiểm tra giỏ.
**Expected Result (SRS):** Không chấp nhận chữ; xử lý an toàn; không crash.
**Reference:** FR-06 ("chỉ nhận số nguyên dương"); EC5 (chữ); C4.

## Test Case 9
**ID:** TC-009
**Precondition:** Giỏ có ≥1 dòng.
**Input Data:** Màn chi tiết SP, nhập Số lượng = `3.5`, bấm "Thêm vào giỏ hàng".
**Steps:**
1. Mở chi tiết một sản phẩm.
2. Nhập 3.5 vào ô Số lượng.
3. Bấm "Thêm vào giỏ hàng" và kiểm tra giỏ.
**Expected Result (SRS):** Chỉ nhận số nguyên — từ chối 3.5 hoặc xử lý an toàn; không thêm 3.5 sản phẩm.
**Reference:** FR-06; EC6 (số thực); C4. (Ghi chú: SUT dùng `parseInt` → có thể lấy 3; cần ghi nhận actual.)

## Test Case 10
**ID:** TC-010
**Precondition:** Giỏ có 1 dòng SP-B (6.000.000 ₫, qty=1).
**Input Data:** Sửa ô Số lượng trong giỏ của SP-B thành `5`.
**Steps:**
1. Mở giỏ hàng.
2. Trong dòng SP-B, sửa ô Số lượng thành 5.
3. Kiểm tra Thành tiền và Tổng cộng.
**Expected Result (SRS):** Số lượng = 5; Thành tiền SP-B = "30.000.000 ₫"; Tổng cộng cập nhật tương ứng.
**Reference:** FR-07 (cột Số lượng chỉnh được, Thành tiền), BR-3; EC8; C5. (Ghi chú: code SUT gán `parsed + 1` → nghi ngờ hiển thị 6; cần ghi nhận actual.)

## Test Case 11
**ID:** TC-011
**Precondition:** Giỏ có 1 dòng SP-B (qty=1).
**Input Data:** Từng lần sửa ô Số lượng trong giỏ thành: `abc`, rỗng, `0`, `-1`.
**Steps:**
1. Mở giỏ hàng, dòng SP-B.
2. Lần lượt nhập các giá trị không hợp lệ trên vào ô Số lượng.
3. Sau mỗi lần nhập, kiểm tra số lượng, Thành tiền, Tổng cộng.
**Expected Result (SRS):** Mỗi giá trị không hợp lệ được xử lý an toàn (từ chối hoặc quy về 1); số lượng không bao giờ âm/rỗng; không crash.
**Reference:** FR-06; EC9–EC12; C6.

## Test Case 12
**ID:** TC-012
**Precondition:** Giỏ có đúng 1 dòng (SP-C, qty=1).
**Input Data:** Bấm "Xóa" trên dòng duy nhất.
**Steps:**
1. Mở giỏ hàng.
2. Bấm "Xóa" trên dòng SP-C.
3. Quan sát trạng thái giỏ.
**Expected Result (SRS):** Có dialog xác nhận trước khi xóa (FR-24); sau khi xác nhận, giỏ trống và hiển thị empty state (hình minh họa + thông báo + "Tiếp tục mua sắm"); badge cập nhật về 0.
**Reference:** FR-07, FR-24; V6 (xóa dòng cuối); B6 điểm 0; C7.

## Test Case 13
**ID:** TC-013
**Precondition:** Giỏ có 3 dòng: SP-A (30.000.000 ₫), SP-B (6.000.000 ₫), SP-C (4.000.000 ₫), qty đều 1. Tổng = 40.000.000 ₫.
**Input Data:** Xóa dòng SP-B.
**Steps:**
1. Mở giỏ hàng.
2. Bấm "Xóa" trên dòng SP-B và xác nhận.
3. Kiểm tra danh sách dòng và Tổng cộng.
**Expected Result (SRS):** Còn đúng 2 dòng (SP-A, SP-C); các dòng còn lại giữ nguyên số lượng; Tổng cộng = "34.000.000 ₫".
**Reference:** FR-07; V6 (xóa khi còn ≥2 dòng); C8; B6 điểm 1←.

## Test Case 14
**ID:** TC-014
**Precondition:** Giỏ có SP-B qty=2 và SP-C qty=1.
**Input Data:** Kiểm tra hiển thị các cột và tổng.
**Steps:**
1. Mở giỏ hàng.
2. Đối chiếu từng dòng: Sản phẩm, Đơn giá, Số lượng, Thành tiền, Thao tác.
3. Tự tính: 2×6.000.000 + 1×4.000.000 = 16.000.000 ₫.
**Expected Result (SRS):** Mỗi dòng đủ 5 thông tin; Thành tiền = Đơn giá × Số lượng ("12.000.000 ₫" và "4.000.000 ₫"); nhãn tổng là **"Tổng cộng"** với giá trị "16.000.000 ₫" (₫ + phân cách hàng nghìn).
**Reference:** FR-07 (nhãn "Tổng cộng"), FR-21 (đơn vị ₫); BR-3, BR-6, BR-8; C9.

## Test Case 15
**ID:** TC-015
**Precondition:** Giỏ có SP-A qty=1 (30.000.000 ₫).
**Input Data:** Kiểm định dạng tiền giá trị lớn.
**Steps:**
1. Mở giỏ hàng.
2. Quan sát Đơn giá, Thành tiền và Tổng cộng.
**Expected Result (SRS):** Hiển thị "30.000.000 ₫" đúng phân cách hàng nghìn và ký hiệu ₫.
**Reference:** FR-21; BR-3; C9.

## Test Case 16
**ID:** TC-016
**Precondition:** Mới mở app nhưng chưa thêm sản phẩm nào (giỏ trống).
**Input Data:** Không nhập gì; mở giỏ hàng.
**Steps:**
1. Bấm "Giỏ" trên header ngay khi mở app.
2. Quan sát empty state.
**Expected Result (SRS):** Hiển thị **hình minh họa** + thông báo rõ ràng ("Giỏ hàng của bạn đang trống") + nút/link "Tiếp tục mua sắm".
**Reference:** FR-07, FR-24 (empty state có icon/hình); BR-7; EC14; C10.

## Test Case 17
**ID:** TC-017
**Precondition:** Chưa đăng nhập (guest).
**Input Data:** Thêm SP-A qty=1, mở giỏ.
**Steps:**
1. Mở app (chưa đăng nhập).
2. Thêm SP-A vào giỏ, mở giỏ hàng.
**Expected Result (SRS):** Các chức năng giỏ (xem, sửa số lượng, xóa) hoạt động bình thường không cần đăng nhập.
**Reference:** FR-07 (giỏ không yêu cầu đăng nhập — chỉ FR-08 Checkout mới yêu cầu); V7; C11.

## Test Case 18
**ID:** TC-018
**Precondition:** Giỏ có 1 dòng SP-C.
**Input Data:** Màn chi tiết SP-A, nhập Số lượng = `9999`, bấm "Thêm vào giỏ hàng".
**Steps:**
1. Mở chi tiết SP-A.
2. Nhập 9999 rồi bấm "Thêm vào giỏ hàng".
3. Mở giỏ hàng, kiểm tra dòng SP-A.
**Expected Result (SRS):** SRS không giới hạn trên — hệ thống chấp nhận 9999 (số nguyên dương hợp lệ), cộng dồn đúng; UI không crash. (Exploratory do thiếu ranh giới trên — xem Coverage.)
**Reference:** FR-06/FR-07; EC7; C4 mở rộng.

## Test Case 19
**ID:** TC-019
**Precondition:** Không.
**Input Data:** Thao tác thêm/xóa giỏ liên tục.
**Steps:**
1. Thêm SP-A và SP-B → quan sát badge header "Giỏ (n)".
2. Thêm tiếp SP-A lần 2 → kiểm badge.
3. Xóa 1 dòng → kiểm badge.
**Expected Result (SRS):** Badge cập nhật ngay sau **mỗi** thao tác thêm/xóa, phản ánh đúng giỏ.
**Reference:** FR-23 (badge số lượng); BR-9; C12; B5.

## Test Case 20
**ID:** TC-020
**Precondition:** Giỏ có ≥1 dòng.
**Input Data:** Quan sát cấu trúc cột số lượng.
**Steps:**
1. Mở giỏ hàng.
2. Kiểm tra cột Số lượng có nút tăng/giảm (+/-) không.
**Expected Result (SRS):** Cột Số lượng phải có nút +/- để chỉnh.
**Reference:** FR-07 ("Số lượng (có nút +/- để chỉnh)"); BR-8.

## Test Case 21
**ID:** TC-021
**Precondition:** Giỏ có ≥1 dòng.
**Input Data:** Bấm "Xóa" trên một dòng.
**Steps:**
1. Mở giỏ hàng.
2. Bấm "Xóa" trên dòng bất kỳ.
3. Quan sát xem có hộp thoại xác nhận xuất hiện trước khi dòng bị xóa không.
**Expected Result (SRS):** Phải có dialog xác nhận trước khi thực hiện xóa.
**Reference:** FR-07, FR-24; BR-4.

## Test Case 22
**ID:** TC-022
**Precondition:** Giỏ có ≥1 dòng.
**Input Data:** Quan sát nhãn tổng tiền.
**Steps:**
1. Mở giỏ hàng.
2. Đọc nhãn của dòng tổng tiền.
**Expected Result (SRS):** Nhãn chính xác là **"Tổng cộng"** (không phải "Tổng tạm tính").
**Reference:** FR-07; BR-6.

## Test Case 23
**ID:** TC-023
**Precondition:** Giỏ trống.
**Input Data:** Bấm nút "Tiếp tục mua sắm".
**Steps:**
1. Ở giỏ trống (hoặc giỏ có hàng), tìm nút "Tiếp tục mua sắm".
2. Bấm nút.
**Expected Result (SRS):** Quay về trang chủ (danh sách sản phẩm).
**Reference:** FR-07; BR-5.
