# Coverage Report - BVA

**Feature:** FR-14 -- Quản lý Danh mục (Category Management)

## Boundaries Coverage

### Biên 1 — Độ dài tên danh mục (min = 1)

| Điểm biên | Test case | Phủ | Kết quả |
|---|---|---|---|
| Min − 1 (0 ký tự, rỗng) | FR14-BVA-01 | ✅ | FAIL — SUT chấp nhận tên rỗng (BUG-01) |
| Min (1 ký tự) | FR14-BVA-02 | ✅ | PASS |
| Min + 1 (2 ký tự) | FR14-BVA-03 | ✅ | PASS |

### Biên 2 — Miền id khi DELETE

| Điểm biên | Test case | Phủ | Kết quả |
|---|---|---|---|
| Dưới biên dưới (id = 0) | FR14-BVA-04 | ✅ | FAIL — trả 200 thay vì 404 (BUG-06) |
| Dưới biên dưới (id = −1) | FR14-BVA-05 | ✅ | FAIL — trả 200 (BUG-06) |
| Tại biên trên (id = max tồn tại) | FR14-BVA-07 | ✅ | PASS |
| Trên biên trên (id = max + 1) | FR14-BVA-06 | ✅ | FAIL — trả 200 (BUG-06) |

### Biên 3 — Độ dài tên, biên trên (max)

| Điểm biên | Test case | Phủ | Kết quả |
|---|---|---|---|
| Biên trên | FR14-BVA-08 (thăm dò 10.000 ký tự) | ⚠️ Thăm dò | INFORMATIONAL — SRS không định nghĩa max; cài đặt cũng không giới hạn |

### Biên không áp dụng được 3-point

| Biến | Lý do |
|---|---|
| Trạng thái token | Biến rời rạc (admin/user/không có/sai) — không có biên số liên tục; phủ bằng Domain Testing (DT-09 → DT-13, DT-17) |
| Trạng thái tham chiếu sản phẩm | Biến boolean (có/không SP tham chiếu) — phủ bằng DT-15 |
| Số lượng danh mục (danh sách rỗng/có dữ liệu) | Biến trạng thái — phủ bằng DT-02, DT-18 |

## Missing / Chưa kiểm tra

| Biên | Lý do |
|---|---|
| Độ dài tên — max chính thức | SRS FR-14 không quy định max length → không thể kết luận Pass/Fail; chỉ thăm dò 10.000 ký tự và ghi "chưa xác minh" |
| Độ dài tên tại các điểm 255/256 | Đã phủ bằng thăm dò xa (10.000); SRS không tham chiếu ngưỡng nào (FR-15 là 255 cho **sản phẩm**, không áp dụng cho danh mục) |
| PUT id ngoài miền | PUT ngoài phạm vi SRS FR-14; hành vi DELETE (BUG-06) đủ làm bằng chứng cho pattern thiếu kiểm tra `changes` chung của handler |

## Summary

- **7/7 test case BVA 3-point chính thức đã thực thi + 1 thăm dò** — không bỏ điểm biên nào đã xác định được.
- **3 PASS / 4 FAIL.** Cả 4 FAIL đều là vi phạm thật của SUT:
  - **BVA-01:** tại min−1 (tên rỗng) SUT vẫn tạo danh mục — vi phạm trực tiếp FR-14, trùng với DT-03 (cùng root-cause BUG-01).
  - **BVA-04/05/06:** DELETE với id ngoài miền hợp lệ (0, −1, max+1) đều trả 200 "Category deleted" — handler không kiểm tra số dòng ảnh hưởng (`this.changes`), vi phạm nguyên tắc API contract cơ bản (BUG-06).
- Biên hợp lệ (min, min+1, tại biên trên id) hoạt động đúng → lỗi của SUT nằm ở **các phía ngoài miền hợp lệ**, đúng vùng mà BVA nhắm tới.
- Kết luận: BVA phát hiện hiệu quả 2 nhóm lỗi mà các test lớp hợp lệ thông thường không chạm tới.
