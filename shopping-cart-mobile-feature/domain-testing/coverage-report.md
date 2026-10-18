# Coverage Report — Domain Testing

**Feature:** FR-07 — Giỏ hàng (Shopping Cart) — Mobile App
**Tổng test cases:** 23 (TC-001 → TC-023) — xem `domain-testing/test-cases.md`

## Equivalence Classes Coverage

| Biến | Lớp xác định | Đã phủ | Test Case phủ | Ghi chú |
|---|---|---|---|---|
| V1 `add_quantity` | 7 (EC1–EC7) | 7/7 | TC-001, TC-002, TC-005 → TC-009, TC-018 | Đủ valid (1, 2, 5, 9999) + invalid (0, âm, rỗng, chữ, số thực) |
| V2 `product_id` | 3 (mới / trùng / khác) | 3/3 | TC-001/002, TC-003, TC-004 | SP không tồn tại không phát sinh qua UI mobile → N/A (không thể test qua giao diện) |
| V3 `cart_quantity` | 5 (EC8–EC12) | 5/5 | TC-010, TC-011 | Đủ valid (5) + invalid (0, âm, rỗng, chữ) |
| V4 `cart_state` | 3 (0 / 1 / ≥2 dòng) | 3/3 | TC-012, TC-013, TC-014, TC-016 | Cả 3 trạng thái render |
| V5 `repeat_add` | 2 (lần đầu / cộng dồn) | 2/2 | TC-001/003 | Kèm chuỗi cộng dồn 3 lần ở BVA-010 → 012 |
| V6 `delete_action` | 2 (còn dòng khác / dòng cuối) | 2/2 | TC-012, TC-013 | Xóa khi giỏ trống: UI không hiển thị nút Xóa → N/A |
| V7 `auth_state` | 2 (guest / logged-in) | 1/2 | TC-017 (guest) | Logged-in chưa phủ tường minh |

**Tỷ lệ phủ lớp tương đương: 23/24 (~96%)**

## Yêu cầu nghiệp vụ Coverage (FR-07 + liên quan)

| Rule | Mô tả | Test Case | Trạng thái |
|---|---|---|---|
| BR-1 | Cộng dồn cùng SP, không tạo dòng mới | TC-003 | ✅ Phủ |
| BR-2 | Số lượng nguyên dương, min 1 | TC-005 → TC-009, TC-011 | ✅ Phủ |
| BR-3 | Thành tiền = Đơn giá × SL; Tổng = Σ | TC-014, TC-015, TC-010 | ✅ Phủ |
| BR-4 | Dialog xác nhận khi xóa | TC-021, TC-012 | ✅ Phủ |
| BR-5 | Nút "Tiếp tục mua sắm" về trang chủ | TC-023 | ✅ Phủ |
| BR-6 | Nhãn tổng là "Tổng cộng" | TC-022, TC-014 | ✅ Phủ |
| BR-7 | Empty state có hình minh họa + thông báo | TC-016 | ✅ Phủ |
| BR-8 | Cột Số lượng có nút +/- | TC-020 | ✅ Phủ |
| BR-9 | Badge "Giỏ (n)" cập nhật | TC-019 | ✅ Phủ |
| FR-21 | Đơn vị ₫, phân cách hàng nghìn | TC-014, TC-015 | ✅ Phủ |
| FR-24 | Dialog xóa + empty state có hình | TC-021, TC-016 | ✅ Phủ |

## Missing / Ghi nhận

1. **V7 logged-in chưa test tường minh** — lý do: giỏ hàng là state phía client, đăng nhập không ảnh hưởng hành vi giỏ (chỉ Checkout FR-08 yêu cầu đăng nhập, ngoài phạm vi). Mọi test case đều có thể chạy lại trong trạng thái đã đăng nhập để phủ ngầm.
2. **TC-018 (qty = 9999)** là exploratory vì SRS không định nghĩa giá trị tối đa của số lượng — thiếu yêu cầu, khuyến nghị bổ sung vào SRS.
3. **Kỳ vọng của TC-005 → TC-009** (invalid add_quantity) dựa trên diễn giải FR-06: SRS không nói rõ là "chặn" hay "ép về 1" — khi thực thi cần ghi nhận actual và đối chiếu đội phát triển.

## Summary

- 23 test cases phủ 12/12 tổ hợp điều kiện quan trọng (C1–C12 ở Bước 5).
- Phủ 6/6 điều khoán nghiệp vụ của FR-07 và các yêu cầu giáp ranh (FR-06, FR-21, FR-23, FR-24).
- Phủ lớp tương đương ~96%; còn 1 lớp (logged-in) có lý do bỏ qua chính đáng.
- 6 điểm nghi vấn SUT (dự đoán từ mã nguồn) đã cài sẵn kỳ vọng vào test case: nhãn "Tổng tạm tính", thiếu nút +/-, thiếu dialog xóa, empty state thiếu hình, sửa qty bị +1, badge đếm dòng.
