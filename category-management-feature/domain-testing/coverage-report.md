# Coverage Report - Domain Testing

**Feature:** FR-14 -- Quản lý Danh mục (Category Management)

## Equivalence Classes Coverage

### V1 — `name` (Create): 8/8 lớp đã phủ ✅

| Lớp | Mô tả | Test case | Trạng thái phủ |
|---|---|---|---|
| V1-N1 | Tên hợp lệ | FR14-DT-01 | ✅ PASS |
| V1-N2 | Chuỗi rỗng | FR14-DT-03 | ✅ phủ (FAIL — BUG-01) |
| V1-N3 | Thiếu trường name | FR14-DT-04 | ✅ phủ (FAIL — BUG-01) |
| V1-N4 | Chỉ khoảng trắng | FR14-DT-05 | ✅ phủ (FAIL — BUG-02) |
| V1-N5 | 1 ký tự (biên dưới) | FR14-BVA-02 | ✅ phủ (PASS) |
| V1-N6 | Ký tự đặc biệt/emoji | FR14-DT-07 | ✅ PASS |
| V1-N7 | Sai kiểu dữ liệu | FR14-DT-08 | ✅ phủ (FAIL — BUG-03) |
| V1-N8 | Tên trùng lặp | FR14-DT-06 | ✅ PASS (SRS không cấm — ghi nhận rủi ro) |

### V2 — `id` (Delete/Update): 4/4 lớp đã phủ ✅

| Lớp | Mô tả | Test case | Trạng thái phủ |
|---|---|---|---|
| V2-I1 | id tồn tại | FR14-DT-13, DT-16, BVA-07 | ✅ phủ |
| V2-I2 | id dương không tồn tại | FR14-DT-14, BVA-06 | ✅ phủ (FAIL — BUG-06) |
| V2-I3 | id = 0 | FR14-BVA-04 | ✅ phủ (FAIL — BUG-06) |
| V2-I4 | id âm | FR14-BVA-05 | ✅ phủ (FAIL — BUG-06) |

### V3 — Trạng thái token: 4/4 lớp đã phủ ✅

| Lớp | Mô tả | Test case | Trạng thái phủ |
|---|---|---|---|
| V3-A1 | Token admin | DT-01 → DT-08, DT-14, DT-15, DT-16 | ✅ PASS |
| V3-A2 | Token user thường | DT-11 (POST), DT-13 (DELETE), DT-17 (PUT) | ✅ phủ (FAIL — BUG-04 CRITICAL) |
| V3-A3 | Không token | DT-09 (POST), DT-12 (DELETE) | ✅ PASS (401) |
| V3-A4 | Token sai định dạng | DT-10 | ✅ PASS (403) |

### V4 — Trạng thái tham chiếu sản phẩm: 2/2 lớp đã phủ ✅

| Lớp | Mô tả | Test case | Trạng thái phủ |
|---|---|---|---|
| V4-S1 | Danh mục trống (không SP tham chiếu) | DT-13, DT-18 | ✅ PASS |
| V4-S2 | Danh mục có SP tham chiếu | DT-15 | ✅ phủ (FAIL — BUG-05) |

### V5 — Trạng thái danh sách (Read): 2/2 lớp đã phủ ✅

| Lớp | Mô tả | Test case | Trạng thái phủ |
|---|---|---|---|
| V5-L1 | Có dữ liệu | DT-02 | ✅ PASS |
| V5-L2 | Rỗng | DT-18 | ✅ PASS (mức API) |

## Missing / Chưa kiểm tra

| Mục | Lý do |
|---|---|
| Kiểm thử UI bằng tay trên trình duyệt (form thêm, bảng hiển thị, nút Xóa) | Đã rà soát mã nguồn UI (App.jsx) nhưng chưa thao tác click thực tế; khuyến nghị bổ sung bằng chứng screenshot khi làm báo cáo |
| Empty-state UI khi danh sách rỗng (FR-24) | Chỉ xác minh mức API trả `[]`; cần chạy frontend-admin để đối chiếu hiển thị |
| PUT /api/categories là API ngoài SRS FR-14 (SRS chỉ yêu cầu Thêm/Xem/Xóa) | Đã test ở mức tham chiếu (DT-16, DT-17) và ghi rõ "ngoài phạm vi SRS" |
| Token hết hạn (expired JWT) | Middleware verify JWT sẽ trả 403 tương tự token sai — hành vi chung của jwt.verify, không riêng cho categories |
| ID dạng chuỗi không phải số (vd. `DELETE /api/categories/abc`) | SQLite so sánh kiểu loose; ngoài các lớp miền đã định nghĩa ở Bước 2 — có thể bổ sung sau nếu cần |

## Summary

- **18/18 test case Domain Testing đã thiết kế và thực thi** — không có test case bỏ dở.
- Phủ **100% lớp tương đương đã xác định** (22/22 lớp trên 5 biến V1–V5).
- **9 PASS / 9 FAIL**: tất cả FAIL đều là vi phạm thực tế của SUT so với SRS (không phải lỗi test), tập trung vào 2 nhóm: thiếu validate `name` (FR-14) và thiếu kiểm tra role admin (FR-12/SEC-03 — nghiêm trọng nhất).
- Rủi ro còn lại chủ yếu ở mức UI (chưa kiểm bằng tay) — đã ghi rõ ở bảng Missing.
