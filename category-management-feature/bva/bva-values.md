# BVA 3-Point Values

**Feature:** FR-14 -- Quản lý Danh mục (Category Management)

## Biên 1 — Độ dài tên danh mục (`name.length`)

Quy tắc nghiệp vụ (FR-14): "Tên danh mục là bắt buộc, không được để trống" → **biên dưới min = 1 ký tự**. SRS không định nghĩa độ dài tối đa.

| Điểm | Giá trị | Dữ liệu test | Kết quả mong đợi |
|---|---|---|---|
| Min − 1 | 0 ký tự | `""` | **Từ chối (400)** — vi phạm quy tắc bắt buộc |
| Min | 1 ký tự | `"X"` | Chấp nhận — tạo danh mục |
| Min + 1 | 2 ký tự | `"XY"` | Chấp nhận — tạo danh mục |

Lưu ý biên bổ sung (không phải 3-point chuẩn nhưng sát biên nghiệp vụ): tên chỉ toàn khoảng trắng có `length > 0` nhưng thực chất rỗng → phủ ở FR14-DT-05.

Biên trên độ dài: **SRS không định nghĩa max** → chỉ thăm dò (`"Z" × 10.000 ký tự`), không kết luận Pass/Fail.

## Biên 2 — Miền `id` khi DELETE (id nguyên dương, min = 1)

| Điểm | Giá trị | Dữ liệu test | Kết quả mong đợi |
|---|---|---|---|
| Dưới biên dưới | id = 0 | `DELETE /api/categories/0` | 404 Not Found |
| Dưới biên dưới (xa) | id âm | `DELETE /api/categories/-1` | 404 Not Found |
| Tại biên trên (id lớn nhất đang tồn tại) | id = 14 | `DELETE /api/categories/14` | 200 — xóa thành công |
| Trên biên trên | id = max + 1 (15) | `DELETE /api/categories/15` | 404 Not Found |

Ghi chú: biên trên của miền id là **id của bản ghi cuối cùng** (thay đổi theo dữ liệu); tại thời điểm chạy, id lớn nhất tồn tại = 14.

## Biên 3 — Thời điểm token (không áp dụng 3-point)

Token là biến trạng thái rời rạc (hợp lệ/user/không có/sai) — không có biên số liên tục, đã phủ bằng Domain Testing (V3-A1 → V3-A4).
