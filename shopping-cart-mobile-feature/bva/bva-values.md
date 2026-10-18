# BVA — Giá trị theo quy tắc 3 điểm (3-Point Rule)

**Feature:** FR-07 — Giỏ hàng (Shopping Cart) — Mobile App
**Cơ sở:** FR-06 (min quantity = 1), FR-07, FR-21, FR-23; code `App.js` (mobile SUT).

## Ranh giới xác định được

| # | Ranh giới | min-1 | min | min+1 | Ghi chú |
|---|-----------|-------|-----|-------|---------|
| B1 | `add_quantity` — số lượng thêm vào giỏ (SRS min = 1) | `0` (invalid) | `1` (valid) | `2` (valid) | Không có max trong SRS |
| B2 | `cart_quantity` — số lượng sửa trong giỏ (SRS min = 1) | `0` (invalid) | `1` (valid) | `2` (valid) | Nghi ngờ bug SUT: gán `parsed + 1` |
| B3 | `cart_state` — số dòng giỏ hàng | `0` (empty state) | `1` (1 dòng) | `2` (nhiều dòng) | Ranh giới render 2 nhánh |
| B4 | `repeat_add` — số lần thêm cùng SP | `1` (tạo dòng mới) | `2` (cộng dồn) | `3` (cộng dồn tiếp) | Biên chuyển "tạo mới" ↔ "cộng dồn" |
| B5 | Badge "Giỏ (n)" | `0` | `1` | `2` | SRS FR-23: "số lượng sản phẩm trong giỏ" (mơ hồ: đếm dòng hay tổng qty) |
| B6 | Xóa — số dòng còn lại sau xóa | `0` (giỏ trống) | `1` | `2` | Chuyển trạng thái 1→0 phải rơi empty state |

## Ranh giới KHÔNG xác định được

| # | Ranh giới | Lý do |
|---|-----------|-------|
| B7 | Max của `add_quantity` / `cart_quantity` | SRS không quy định giá trị tối đa → chỉ khám phá thêm (exploratory) với `9999` |
| B8 | Định dạng tiền quanh mốc 1.000 (999 / 1.000 / 1.001 ₫) | Không thể tạo ra qua UI với dữ liệu seed: giá thấp nhất 4.000.000 ₫ → mọi thành tiền ≥ 4.000.000 ₫ |

## Bảng giá trị chi tiết

### B1 — add_quantity
| Điểm | Giá trị nhập | Kỳ vọng theo SRS | Hành vi SUT dự đoán (từ code) |
|------|--------------|-------------------|-------------------------------|
| min-1 | `0` | Không chấp nhận / xử lý an toàn (min 1) | Ép về 1 → thêm qty=1 |
| min | `1` | Thêm 1 SP | Thêm qty=1 ✔ |
| min+1 | `2` | Thêm 2 SP | Thêm qty=2 ✔ |

### B2 — cart_quantity
| Điểm | Giá trị nhập | Kỳ vọng theo SRS | Hành vi SUT dự đoán (từ code) |
|------|--------------|-------------------|-------------------------------|
| min-1 | `0` | Không chấp nhận / về 1 | Rơi nhánh invalid → 1 |
| min | `1` | qty = 1; Thành tiền = Đơn giá | ⚠️ gán 1+1=2 → hiển thị 2 (bug) |
| min+1 | `2` | qty = 2 | ⚠️ gán 2+1=3 → hiển thị 3 (bug) |

### B3 — cart_state (số dòng)
| Điểm | Trạng thái | Kỳ vọng |
|-------|-----------|---------|
| 0 | Giỏ trống | Hình minh họa + thông báo + "Tiếp tục mua sắm" |
| 1 | 1 dòng | Render 1 dòng đủ 5 cột; "Tổng cộng" = thành tiền dòng đó |
| 2 | 2 dòng | Render ≥2 dòng; "Tổng cộng" = Σ thành tiền |

### B4 — repeat_add
| Điểm | Thao tác | Kỳ vọng |
|-------|----------|---------|
| 1 | Thêm SP-A lần 1 | Tạo dòng mới qty=n |
| 2 | Thêm SP-A lần 2 | Cộng dồn: qty = n + m, vẫn 1 dòng |
| 3 | Thêm SP-A lần 3 | Tiếp tục cộng dồn: qty = n + m + k, vẫn 1 dòng |

### B5 — Badge
| Điểm | Giỏ | Kỳ vọng |
|-------|-----|---------|
| 0 | Trống | "Giỏ (0)" |
| 1 | 1 dòng (qty bất kỳ) | Cập nhật đúng (1 dòng → (1); nếu tổng qty → theo qty — cần làm rõ) |
| 2 | 2 dòng | "Giỏ (2)" |

### B6 — Xóa (số dòng còn lại)
| Điểm | Trước khi xóa | Kỳ vọng sau xóa |
|-------|---------------|------------------|
| 0→ | Giỏ 1 dòng, xóa dòng duy nhất | Giỏ trống + empty state |
| 1← | Giỏ 2 dòng, xóa 1 | Còn 1 dòng, tổng cập nhật |
| 2← | Giỏ 3 dòng, xóa 1 | Còn 2 dòng, tổng cập nhật |

## Ghi nhận
- Mọi ranh giới trên đều là ranh giới **dưới** hoặc **trạng thái**; không có ranh giới trên chính thức do SRS bỏ ngỏ (B7) → đánh dấu thiếu yêu cầu trong Coverage Report.
- Sản phẩm dùng test: SP-A iPhone 15 Pro Max (30.000.000 ₫), SP-B AirPods Pro 2 (6.000.000 ₫), SP-C Keychron Q1 (4.000.000 ₫).
