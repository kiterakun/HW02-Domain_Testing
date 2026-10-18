# Boundary Value Analysis - Test Cases

**Feature:** FR-07 — Giỏ hàng (Shopping Cart) — Mobile App
**Cơ sở:** `bva/bva-values.md` (ranh giới B1–B6, quy tắc 3 điểm); FR-06 (min qty = 1), FR-07, FR-23, FR-24.
**Dữ liệu seed:** SP-A = iPhone 15 Pro Max (30.000.000 ₫), SP-B = AirPods Pro 2 (6.000.000 ₫), SP-C = Keychron Q1 (4.000.000 ₫).
**Quy ước Actual column:** để trống — điền khi thực thi trên SUT.

## Test Case 1
**ID:** BVA-001
**Ranh giới:** B1 — add_quantity (min-1)
**Precondition:** Giỏ có thể trống hoặc có hàng; đang ở màn chi tiết SP-C (4.000.000 ₫).
**Input Data:** Nhập Số lượng `0` (min−1) rồi "Thêm vào giỏ hàng".
**Steps:**
1. Mở chi tiết SP-C.
2. Nhập 0 vào ô Số lượng.
3. Bấm "Thêm vào giỏ hàng"; mở giỏ.
**Expected Result:** Không thêm sản phẩm với qty=0 — hoặc báo lỗi/chặn, hoặc xử lý an toàn (nếu hệ thống chọn quy về min thì thêm qty=1); không âm, không crash.
**Reference:** B1 (0/1/2), FR-06 min = 1.
**Actual:** Nhập "0" -> thêm với SL=1 (ép về 1, an toàn), TT 4.000.000 ₫

## Test Case 2
**ID:** BVA-002
**Ranh giới:** B1 — add_quantity (min)
**Precondition:** Giỏ trống; màn chi tiết SP-C.
**Input Data:** Số lượng `1` (giá trị min hợp lệ).
**Steps:**
1. Mở chi tiết SP-C.
2. Nhập 1, bấm "Thêm vào giỏ hàng"; mở giỏ.
**Expected Result:** Giỏ có 1 dòng SP-C qty=1; Thành tiền "4.000.000 ₫".
**Reference:** B1, FR-06, FR-07.
**Actual:** Nhập "1" -> SL=1, TT 4.000.000 ₫

## Test Case 3
**ID:** BVA-003
**Ranh giới:** B1 — add_quantity (min+1)
**Precondition:** Giỏ trống; màn chi tiết SP-C.
**Input Data:** Số lượng `2`.
**Steps:**
1. Nhập 2 ở chi tiết SP-C, thêm vào giỏ; mở giỏ.
**Expected Result:** Giỏ có 1 dòng SP-C qty=2; Thành tiền "8.000.000 ₫".
**Reference:** B1, FR-06, FR-07.
**Actual:** Nhập "2" -> SL=2, TT 8.000.000 ₫

## Test Case 4
**ID:** BVA-004
**Ranh giới:** B2 — cart_quantity trong giỏ (min-1)
**Precondition:** Giỏ có dòng SP-B qty=1.
**Input Data:** Sửa ô Số lượng trong giỏ thành `0`.
**Steps:**
1. Mở giỏ, dòng SP-B.
2. Nhập 0 vào ô số lượng.
**Expected Result:** Hệ thống không chấp nhận 0 — xử lý an toàn (từ chối hoặc quy về 1); Thành tiền/Tổng không bị âm hay 0 sai quy tắc.
**Reference:** B2 (0/1/2), FR-06.
**Actual:** Nhập "0" -> SL=1 (nhánh invalid, an toàn)

## Test Case 5
**ID:** BVA-005
**Ranh giới:** B2 — cart_quantity (min)
**Precondition:** Giỏ có dòng SP-B (6.000.000 ₫).
**Input Data:** Sửa ô Số lượng trong giỏ thành `1`.
**Steps:**
1. Mở giỏ, dòng SP-B.
2. Nhập 1 vào ô số lượng.
**Expected Result (SRS):** Số lượng hiển thị = 1; Thành tiền = "6.000.000 ₫". ⚠️ Dự đoán SUT (code gán parsed+1): hiển thị 2 — lệch SRS.
**Reference:** B2, FR-06/FR-07.
**Actual:** Nhập "1" -> SL=2, TT 12.000.000 ₫ (BUG-101: parsed+1)

## Test Case 6
**ID:** BVA-006
**Ranh giới:** B2 — cart_quantity (min+1)
**Precondition:** Giỏ có dòng SP-B.
**Input Data:** Sửa ô Số lượng trong giỏ thành `2`.
**Steps:**
1. Mở giỏ, dòng SP-B.
2. Nhập 2 vào ô số lượng.
**Expected Result (SRS):** Số lượng = 2; Thành tiền = "12.000.000 ₫". ⚠️ Dự đoán SUT: hiển thị 3.
**Reference:** B2, FR-06/FR-07.
**Actual:** Nhập "2" -> SL=3, TT 18.000.000 ₫ (BUG-101)

## Test Case 7
**ID:** BVA-007
**Ranh giới:** B3 — cart_state (0 dòng)
**Precondition:** Mới mở app, chưa thêm gì.
**Input Data:** —
**Steps:**
1. Bấm "Giỏ" trên header.
**Expected Result:** Empty state: hình minh họa + thông báo "Giỏ hàng của bạn đang trống" + "Tiếp tục mua sắm".
**Reference:** B3 điểm 0, FR-07, FR-24.
**Actual:** Text + "Tiếp tục mua sắm"; KHÔNG hình minh họa (BUG-103)

## Test Case 8
**ID:** BVA-008
**Ranh giới:** B3 — cart_state (1 dòng)
**Precondition:** Giỏ có đúng 1 dòng (SP-A qty=1).
**Input Data:** —
**Steps:**
1. Mở giỏ.
**Expected Result:** Render 1 dòng đầy đủ 5 mục thông tin; "Tổng cộng" = "30.000.000 ₫".
**Reference:** B3 điểm 1, FR-07.
**Actual:** 1 dòng đủ mục, tổng số 30.000.000 ₫ đúng; nhãn "Tổng tạm tính" (BUG-102)

## Test Case 9
**ID:** BVA-009
**Ranh giới:** B3 — cart_state (2 dòng — biên nhiều dòng)
**Precondition:** Giỏ có 2 dòng: SP-A + SP-B.
**Input Data:** —
**Steps:**
1. Mở giỏ.
**Expected Result:** Render đủ 2 dòng; "Tổng cộng" = "36.000.000 ₫" = 30.000.000 + 6.000.000.
**Reference:** B3 điểm 2, FR-07, BR-3.
**Actual:** 2 dòng, tổng số 36.000.000 ₫ đúng; nhãn sai (BUG-102)

## Test Case 10
**ID:** BVA-010
**Ranh giới:** B4 — repeat_add (lần 1)
**Precondition:** Giỏ trống.
**Input Data:** Thêm SP-B qty=1 từ trang chủ.
**Steps:**
1. Bấm "Thêm vào giỏ" trên thẻ SP-B; mở giỏ.
**Expected Result:** Tạo 1 dòng mới SP-B qty=1.
**Reference:** B4 điểm 1, FR-07.
**Actual:** Tạo 1 dòng mới, SL=1

## Test Case 11
**ID:** BVA-011
**Ranh giới:** B4 — repeat_add (lần 2 — chuyển sang cộng dồn)
**Precondition:** Giỏ có SP-B qty=1 (sau BVA-010).
**Input Data:** Thêm SP-B lần 2 (qty=1).
**Steps:**
1. Bấm "Thêm vào giỏ" trên thẻ SP-B lần nữa; mở giỏ.
**Expected Result:** Vẫn 1 dòng, qty = 2; không tạo dòng mới.
**Reference:** B4 điểm 2, FR-07 (BR-1).
**Actual:** Vẫn 1 dòng, SL=2 (cộng dồn đúng)

## Test Case 12
**ID:** BVA-012
**Ranh giới:** B4 — repeat_add (lần 3 — cộng dồn tiếp)
**Precondition:** Giỏ có SP-B qty=2 (sau BVA-011).
**Input Data:** Thêm SP-B lần 3 (qty=1).
**Steps:**
1. Bấm "Thêm vào giỏ" trên thẻ SP-B lần thứ ba; mở giỏ.
**Expected Result:** Vẫn 1 dòng, qty = 3.
**Reference:** B4 điểm 3, FR-07.
**Actual:** Vẫn 1 dòng, SL=3 (cộng dồn đúng)

## Test Case 13
**ID:** BVA-013
**Ranh giới:** B5 — badge (0)
**Precondition:** Giỏ trống.
**Input Data:** —
**Steps:**
1. Quan sát badge header khi giỏ trống.
**Expected Result:** "Giỏ (0)".
**Reference:** B5 điểm 0, FR-23.
**Actual:** "Giỏ (0)"

## Test Case 14
**ID:** BVA-014
**Ranh giới:** B5 — badge (1 dòng)
**Precondition:** Giỏ đúng 1 dòng (SP-A qty=1).
**Input Data:** —
**Steps:**
1. Quan sát badge header.
**Expected Result (theo FR-23):** Badge phản ánh đúng giỏ — "Giỏ (1)". (Ghi chú mơ hồ SRS: "số lượng sản phẩm" có thể hiểu đếm dòng hoặc tổng qty; ghi nhận cả 2 khi thực thi.)
**Reference:** B5 điểm 1, FR-23.
**Actual:** "Giỏ (1)" (đếm dòng theo cart.length)

## Test Case 15
**ID:** BVA-015
**Ranh giới:** B5 — badge (2 dòng)
**Precondition:** Giỏ 2 dòng (SP-A, SP-B).
**Input Data:** —
**Steps:**
1. Quan sát badge header.
**Expected Result:** "Giỏ (2)".
**Reference:** B5 điểm 2, FR-23.
**Actual:** "Giỏ (2)"

## Test Case 16
**ID:** BVA-016
**Ranh giới:** B6 — xóa về 0 dòng (1 → empty)
**Precondition:** Giỏ có đúng 1 dòng (SP-C).
**Input Data:** Xóa dòng duy nhất.
**Steps:**
1. Mở giỏ; bấm "Xóa" trên dòng SP-C; xác nhận (nếu có dialog).
**Expected Result:** Giỏ trống; hiện empty state (hình minh họa + thông báo); badge về (0).
**Reference:** B6 điểm 0, FR-07, FR-24.
**Actual:** Xóa NGAY không dialog (BUG-104); empty state không hình (BUG-103); badge (0)

## Test Case 17
**ID:** BVA-017
**Ranh giới:** B6 — xóa còn 1 dòng (2 → 1)
**Precondition:** Giỏ có 2 dòng (SP-A, SP-B).
**Input Data:** Xóa dòng SP-B.
**Steps:**
1. Mở giỏ; xóa SP-B; xác nhận.
**Expected Result:** Còn 1 dòng SP-A; "Tổng cộng" = "30.000.000 ₫".
**Reference:** B6 điểm 1, FR-07.
**Actual:** Còn 1 dòng, tổng số 30.000.000 ₫ đúng; nhãn sai (BUG-102); không dialog (BUG-104)

## Test Case 18
**ID:** BVA-018
**Ranh giới:** B6 — xóa còn 2 dòng (3 → 2)
**Precondition:** Giỏ có 3 dòng (SP-A, SP-B, SP-C).
**Input Data:** Xóa dòng SP-B.
**Steps:**
1. Mở giỏ; xóa SP-B; xác nhận.
**Expected Result:** Còn 2 dòng (SP-A, SP-C); "Tổng cộng" = "34.000.000 ₫".
**Reference:** B6 điểm 2, FR-07.
**Actual:** Còn 2 dòng, tổng số 34.000.000 ₫ đúng; nhãn sai (BUG-102)

## Ghi chú thực thi
- 18 test BVA phủ 6 ranh giới × tối thiểu 3 điểm (B3, B4, B5, B6 tổ hợp theo trạng thái).
- Với mỗi test: ghi nhận **Actual** trên SUT mobile và đánh Pass/Fail so với kỳ vọng SRS.
- Các giá trị vượt biên trên (B7 — `9999`, số thực `3.5`) đã thuộc nhóm Domain Testing (TC-018, TC-009).
