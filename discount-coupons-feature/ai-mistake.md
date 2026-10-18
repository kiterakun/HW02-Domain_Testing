# ai-mistake

**Feature:** Mã Giảm Giá (Discount Coupons) — FR-09

## Summary of Mistakes

- **Step 6 (lần đầu trình bày):** AI trình bày sai định dạng — bảng "Giá trị đại diện cho công thức (T1–T3)" bị lặp 3 lần với nội dung sai lệch nhau, chứa ký tự lỗi soạn thảo (`50. (000`, cột `suggested`, cụm "need xác minh", `270.0 0`), và có dòng giá trị đại diện sai bản chất (`BIGBUY × 500.000` ghi nhầm thành percent 10% giảm 25.000 thay vì fixed 50.000; xuất hiện mã `SAVE4` không tồn tại trong hệ thống). AI tự phát hiện và đã trình bày lại bảng chuẩn ở ngay sau đó; các dòng lặp không được dùng để thiết kế test case.

- **Step 7 (ghi file test-cases.md lần đầu):** File `domain-testing/test-cases.md` bị 2 lỗi soạn thảo do AI sinh text thừa: (1) dòng `**ID:** TC-0010 sẽ nói ở Test Case 10.` chèn sai trước Test Case 9; (2) dòng `## Test Case  test 18 được mô tả dưới đây.` chèn sai trước Test Case 18. AI tự phát hiện, đã sửa cả hai bằng edit (lần sửa Test Case 9 đầu tiên thất bại vì oldString không khớp chính xác — đọc lại file và sửa thành công ở lần 2). Nội dung 28 test case không bị ảnh hưởng.

## Notes

### Bài học rút ra (lessons learned)
1. **Trình bày dài nhiều bảng dễ sinh text trùng/lẫn:** Khi trình bày nhiều bảng giá trị giống nhau trong một step, AI sinh lặp block với nội dung sai lệch (Step 6) hoặc chèn câu thừa giữa các test case (Step 7). Khắc phục: trình bày từng bảng một, kiểm lại block vừa viết trước khi viết block kế tiếp.
2. **Đọc file trước khi edit:** Lần edit ai-mistake.md đầu tiên thất bại do oldString không khớp; luôn đọc lại file để lấy nội dung chính xác rồi mới edit.
3. **Giữ expected theo SRS, tách riêng quan sát triển khai:** Không để các điểm bất thường của mã nguồn (strict `>`, thiếu JWT, công thức percent sai) làm sai lệch expected — chúng được đưa vào cột "Quan sát khi chạy" giúp test vừa đúng spec vừa dễ phát hiện bug.

### Tổng kết
- 2 sai sót được ghi nhận (Step 6: lỗi trình bày bảng công thức; Step 7: 2 dòng text thừa trong file test-cases.md). Cả hai đều do AI **tự phát hiện**, đã sửa trong cùng step và không ảnh hưởng đến chất lượng sản phẩm cuối: 28 test case, expected theo SRS, đầy đủ reference tới equivalence class/biên.
- Không có sai sót nào do user chỉnh yêu cầu (user xác nhận thẳng các bước 1→8).

### Bổ sung — phiên thực thi + viết báo cáo (18-10-2026)
- **Kết quả thực thi khớp 100% các điểm "Quan sát khi chạy"** từ phân tích mã nguồn: BUG-001..006 đều thành hiện thực (không phải sai sót của AI — ngược lại, đây là xác nhận thiết kế test đúng).
- **AI mistake 3 (viết báo cáo FR-09):** bản đầu file `report/HW02_FR-09_section.md` chứa 2 lỗi text do AI sinh: (1) cụm "vì ắt Fah điểm" (ký tự rác chèn giữa câu) tại 2.2.2; (2) hàng FR09-DT-23 trong bảng 2.2.3 bị thiếu cột "Dữ liệu đầu vào" (nội dung steps bị dồn vào cột sai). AI tự soát ngay sau khi ghi, đã sửa cả hai bằng edit.
- **Lưu ý quy trình (không sai sót):** backend đang có sẵn process trên cổng 3000 với trạng thái DB không xác định → AI đã dừng process và khởi động lại để đảm bảo tiền đề "DB seed sạch" cho bộ test C5; điều kiện này được ghi trong bằng chứng thực thi.



