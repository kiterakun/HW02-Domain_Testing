# Boundary Value Analysis - Test Cases

**Feature:** Mã Giảm Giá (Discount Coupons) — FR-09

> Expected Result dựa trên SRS. Các test BVA chạy qua API `POST /api/apply-coupon` (body `code`, `total_amount`, `user_id`) để kiểm soát chính xác giá trị biên; đăng nhập user test (`test@eshop.com`, id=2). Tham chiếu giá trị: `bva/bva-values.md`.

## Test Case 1
**ID:** BVA-001
**Precondition:** Đăng nhập; SAVE10 chưa được dùng (count=0); backend đang chạy
**Input Data:** code=`SAVE10`, total_amount = 299999 (min−1) / 300000 (min) / 300001 (min+1), user_id=2
**Steps:**
1. Gọi API với total=299.999, ghi kết quả
2. Gọi API với total=300.000, ghi kết quả
3. Gọi API với total=300.001, ghi kết quả
**Expected Result:**
- 299.999 → Từ chối ("chưa đủ giá trị tối thiểu 300.000 ₫")
- 300.000 → **Thành công** theo SRS (`>=`) — giảm 30.000, thành 270.000
- 300.001 → **Thành công** — giảm 30.000,1 (hành vi làm tròn được ghi nhận, SRS không quy định), thành ≈270.001
**Quan sát khi chạy:** mã nguồn dùng strict `>` → 300.000 (min) có thể bị từ chối sai (bug dự kiến)
**Reference:** BVA — ngưỡng min_order_amount của SAVE10 (min−1 / min / min+1); Step 5-B6/B7; total-EC2/EC3; bva-values §1

## Test Case 2
**ID:** BVA-002
**Precondition:** Đăng nhập; BIGBUY chưa được dùng
**Input Data:** code=`BIGBUY`, total_amount = 499999 (min−1) / 500000 (min) / 500001 (min+1), user_id=2
**Steps:**
1. Gọi API với total=499.999, ghi kết quả
2. Gọi API với total=500.000, ghi kết quả
3. Gọi API với total=500.001, ghi kết quả
**Expected Result:**
- 499.999 → Từ chối ("chưa đủ giá trị tối thiểu 500.000 ₫")
- 500.000 → **Thành công** — giảm 50.000 (fixed), thành 450.000
- 500.001 → **Thành công** — giảm 50.000, thành 450.001
**Quan sát khi chạy:** same strict `>` bug tại biên min
**Reference:** BVA — ngưỡng min_order_amount của BIGBUY (min−1 / min / min+1); Step 5-C1/C2; total-EC2/EC3; bva-values §1

## Test Case 3
**ID:** BVA-003
**Precondition:** Đăng nhập; điều khiển số lần dùng VIP100 của user: 0 / 1 / 2 (setup bằng `POST /api/coupon-usage` hoặc checkout thật; ⚠️ DB reset khi restart backend)
**Input Data:** code=`VIP100`, total_amount=400000, user_id=2; count = 0 / 1 / 2 (max=2)
**Steps:**
1. Với count=0: áp `VIP100` → ghi kết quả
2. Tăng count lên 1 (ghi lượt dùng), áp lại → ghi kết quả
3. Tăng count lên 2, áp lại → ghi kết quả
**Expected Result:**
- count=0 → **Thành công** — giảm 100.000, thành 300.000
- count=1 (= max−1) → **Thành công** — giảm 100.000, thành 300.000
- count=2 (= max) → **Từ chối** — "đã đạt giới hạn" (2 < 2 sai)
**Reference:** BVA — biên max_uses_per_user của VIP100 (max−1 / max); usage-EC1/EC2/EC3; bva-values §2

## Test Case 4
**ID:** BVA-004
**Precondition:** Đăng nhập; điều khiển số lần dùng SAVE10 của user: 0 / 1 (setup như BVA-003)
**Input Data:** code=`SAVE10`, total_amount=500000, user_id=2; count = 0 / 1 (max=1)
**Steps:**
1. Với count=0: áp `SAVE10` → ghi kết quả
2. Ghi lượt dùng (count=1), áp lại → ghi kết quả
**Expected Result:**
- count=0 → **Thành công** — giảm 50.000 (10% của 500.000), thành 450.000
- count=1 (= max) → **Từ chối** — "đã đạt giới hạn"
**Reference:** BVA — biên max_uses_per_user của SAVE10 (max−1 / max, max=1 nên max−1=0); usage-EC1/EC3; bva-values §2

---

## KẾT QUẢ THỰC THI (18-10-2026 — chi tiết tại `execution/execution-results.md`)

| Điểm | Giá trị | QĐ (SRS) | Kết quả thực tế | Trạng thái |
|---|---|---|---|---|
| BVA-001 p1 | total=299.999 | Từ chối | 400 "chưa đủ 300,000 ₫" | Pass |
| BVA-001 p2 | total=300.000 (min) | Đậu: giảm 30.000 → 270.000 | 400 "chưa đủ 300,000 ₫" | **Fail — BUG-002** |
| BVA-001 p3 | total=300.001 (min+1) | Đậu, giảm ≈30.000,1 (làm tròn ghi nhận) | 200 nhưng discount **−2.700.009**, final **3.000.010** | **Fail — BUG-001** |
| BVA-002 p1 | total=499.999 | Từ chối | 400 "chưa đủ 500,000 ₫" | Pass |
| BVA-002 p2 | total=500.000 (min) | Đậu: giảm 50.000 → 450.000 | 400 "chưa đủ 500,000 ₫" | **Fail — BUG-002** |
| BVA-002 p3 | total=500.001 (min+1) | Đậu: giảm 50.000 → 450.001 | 200, discount 50.000, final 450.001 | Pass |
| BVA-003 p1 | VIP100 count=0 | Đậu: giảm 100.000 → 300.000 | 200, discount 100.000, final 300.000 | Pass |
| BVA-003 p2 | count=1 (max−1) | Đậu như trên | 200, discount 100.000, final 300.000 | Pass |
| BVA-003 p3 | count=2 (max) | Từ chối "giới hạn" | 400 "…2 lần (đã đạt giới hạn)" | Pass |
| BVA-004 p1 | SAVE10 count=0 | Đậu: giảm 50.000 → 450.000 | 200 nhưng discount **−4.500.000**, final **5.000.000** | **Fail — BUG-001** |
| BVA-004 p2 | count=1 (max) | Từ chối "giới hạn" | 400 "…1 lần (đã đạt giới hạn)" | Pass |

**Tổng 11 điểm:** Pass 7 · Fail 4 (BUG-001: BVA-001p3, BVA-004p1 · BUG-002: BVA-001p2, BVA-002p2). Khớp hoàn toàn với phân tích "Quan sát khi chạy" của bộ test.
