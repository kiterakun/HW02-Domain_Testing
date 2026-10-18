# Coverage Report — Boundary Value Analysis

**Feature:** FR-07 — Giỏ hàng (Shopping Cart) — Mobile App
**Tổng test cases:** 18 (BVA-001 → BVA-018) — xem `bva/test-cases.md` và `bva/bva-values.md`

## Boundaries Coverage

| Ranh giới | Mô tả | 3 điểm | Test cases | Trạng thái |
|---|---|---|---|---|
| B1 | `add_quantity` — số lượng thêm vào giỏ (min = 1) | 0 / 1 / 2 | BVA-001 → BVA-003 | ✅ Đủ 3 điểm |
| B2 | `cart_quantity` — số lượng sửa trong giỏ (min = 1) | 0 / 1 / 2 | BVA-004 → BVA-006 | ✅ Đủ 3 điểm |
| B3 | `cart_state` — số dòng giỏ | 0 / 1 / 2 dòng | BVA-007 → BVA-009 | ✅ Đủ 3 điểm |
| B4 | `repeat_add` — số lần thêm cùng SP | lần 1 / 2 / 3 | BVA-010 → BVA-012 | ✅ Đủ 3 điểm |
| B5 | Badge "Giỏ (n)" | 0 / 1 / 2 | BVA-013 → BVA-015 | ✅ Đủ 3 điểm |
| B6 | Xóa — số dòng còn lại | 1→0 / 2→1 / 3→2 | BVA-016 → BVA-018 | ✅ Đủ 3 điểm |

**Tỷ lệ phủ ranh giới: 6/6 ranh giới xác định được, mỗi ranh giới đủ giá trị 3 điểm.**

## Missing / Ranh giới không phủ (có lý do)

| # | Ranh giới | Lý do không phủ |
|---|---|---|
| B7 | Biên trên của số lượng (max − 1 / max / max + 1) | SRS không quy định giá trị tối đa — **thiếu yêu cầu**. Chỉ exploratory với 9999 (TC-018). Khuyến nghị bổ sung max vào SRS. |
| B8 | Định dạng tiền quanh mốc 1.000 ₫ (999 / 1.000 / 1.001) | Không thể tạo giá trị qua UI với dữ liệu seed (giá thấp nhất 4.000.000 ₫). Cần sản phẩm giá thấp do Admin thêm mới (ngoài phạm vi FR-07). |

## Ghi nhận rủi ro tại ranh giới (dự đoán từ mã nguồn SUT)

| Ranh giới | Dự đoán lỗi SUT | Test case kiểm chứng |
|---|---|---|
| B2 min | Sửa qty = 1 trong giỏ → hiển thị 2 (code gán `parsed + 1`) | BVA-005 |
| B2 min+1 | Sửa qty = 2 → hiển thị 3 | BVA-006 |
| B3 điểm 0 | Empty state thiếu hình minh họa | BVA-007 |
| B5 | Badge đếm số dòng (`cart.length`) thay vì "số lượng sản phẩm" theo FR-23 | BVA-013 → BVA-015 |
| B6 1→0 | Xóa không có dialog xác nhận | BVA-016 |

## Summary

- 18 test cases BVA theo quy tắc 3 điểm, phủ 100% ranh giới xác định được từ SRS + UI.
- Toàn bộ ranh giới là **ranh giới dưới / ranh giới trạng thái** — hệ quả của việc SRS bỏ ngỏ biên trên (B7 đã ghi Missing).
- 5 điểm rủi ro tại ranh giới đã được cài sẵn vào Expected Result để đối chiếu khi thực thi.
