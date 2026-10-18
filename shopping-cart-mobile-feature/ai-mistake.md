# ai-mistake

**Feature:** FR-07 — Giỏ hàng (Shopping Cart — Mobile App)

## Summary of Mistakes
- Bước 1–4: Các lỗi hiển thị (mã hóa ký tự / Markdown hỏng) xảy ra khi trình bày phân tích trong chat; nội dung phân tích được người dùng duyệt không thay đổi.
- Bước 4: Lỗi Markdown nghiêm trọng nhất — một số bảng bị vỡ cấu trúc.
- Cam kết đã thực hiện: các file output cuối đều được tự đọc lại (self-check) sau khi ghi và sửa lỗi ngay.

## Notes

### Bước 1 (tóm tắt trong chat)
- Lỗi mã hóa trong bảng tóm tắt: xuất hiện chữ lạ thay vì "code" trong dòng mô tả nguồn mã nguồn. File `feature-overview.md` không bị ảnh hưởng.

### Bước 2 (tóm tắt trong chat)
- Lỗi mã hóa: một cụm mô tả về SRS bị hiển thị lẫn ký tự lạ. File output không bị ảnh hưởng.

### Bước 4 (trình bày trong chat)
- Bảng lớp tương đương của V2 (`product_id`), V4 (`cart_state`), V5 (`repeat_add`), V6 (`delete_action`) bị hỏng Markdown: thiếu/dư dòng phân cách header, cột lệch, giá trị "TC-B01" rơi vào vị trí sai.
- Lỗi gõ/mã hóa rải rác: "ting tự đặc biệt" (đúng: "ký tự đặc biệt"), "Mini-1 boundary" (đúng: "min-1"), một mô tả invalid class của V2 bị cụt.
- Nguyên nhân: sinh chuỗi Markdown dài trực tiếp trong chat. Người dùng vẫn duyệt nội dung; phân hoạch EC1–EC16 giữ nguyên không đổi.

### Xử lý trong các file output (sau khi ghi)
- `ai-mistake.md`: ghi hai lần liên tiếp còn sót ký tự lỗi ("tu的还是", "đ切尔") — phải viết lại toàn bộ file và edit từng dòng để làm sạch. Bài học: với log bằng tiếng Việt, ưu tiên write toàn file thay vì edit nhỏ khi phát hiện ký tự lạ.
- `bva/bva-values.md`: phát hiện 2 lỗi chính tả khi self-check ("Ranhr giới" → "Ranh giới"; cụm tiếng Anh trộn lẫn "only exploratory") — đã sửa bằng edit trước khi trình duyệt.
- `feature-overview.md`, `domain-testing/test-cases.md`, `bva/test-cases.md`, 2 file `coverage-report.md`: self-check đạt, không phát hiện lỗi.

### Bước thực thi + viết báo cáo (2026-10-18)
- Bản nháp đầu của harness tái lập handler sửa số lượng bằng cách tách biểu thức, dễ tiền lệch với mã gốc — đã viết lại nguyên văn đúng `App.js:617-621` (`parsed + 1` trong cùng biểu thức) và chạy lại toàn bộ 41 test trước khi lấy số liệu chính thức.
- `report/HW02_FR-07_section.md` (self-check sau khi ghi):
  1. Lỗi gõ "Ba dí 0 dòng" trong bảng BVA (FR07-BVA-13) — sửa thành "Giỏ 0 dòng".
  2. Câu "Các trường hợp chưa kiểm" viết lần đầu sai ngữ nghĩa ("duplicate-dùng giỏ khi đổi IP") — viết lại danh sách cho đúng phạm vi, kèm lỗi dấu gạch dư ";-percent" — đã sửa.
- Kiểm tra tự động (PowerShell): quét ký tự CJK/lệch mã hóa trong file báo cáo và execution-results — kết quả sạch (False).
