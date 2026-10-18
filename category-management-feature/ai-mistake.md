# ai-mistake

**Feature:** FR-14 -- Quản lý Danh mục (Category Management)

> Tài liệu tổng hợp gồm 2 phần: (1) các điểm triển khai SUT **không tuân thủ đặc tả** phát hiện được qua Domain Testing/BVA; (2) các sai sót của AI trong quá trình thực hiện workflow và cách đã khắc phục.

## Summary of Mistakes (AI trong quy trình)

- **Bước thực thi probe gây bẩn CSDL:** Ở lượt khảo sát đầu, AI gọi trực tiếp POST/DELETE lên SUT đang chạy mà không cô lập dữ liệu, làm CSDL seed sinh ra các bản ghi rác (id 4–13) và mất danh mục id=1. **Khắc phục:** khởi động lại backend để trigger re-seed (database.js chạy `DROP TABLE` + seed khi khởi động) và chạy lại toàn bộ lượt thực thi chính thức trên dữ liệu sạch, theo đúng thứ tự test case.
- **Giả định về mã lỗi 401/403:** AI ban đầu mặc định token sai sẽ trả 401; thực tế middleware verify JWT trả 403 cho token sai định dạng và 401 cho token thiếu. Đã điều chỉnh kết quả mong đợi dựa trên bằng chứng thực thi thay vì giả định.
- **Nhầm lẫn phạm vi Update:** Tiêu đề mục báo cáo ghi "Category management (CRUD)" nhưng SRS FR-14 chỉ yêu cầu **Thêm/Xem/Xóa** (không có Update), trong khi backend có API PUT. **Khắc phục:** giữ 2 test PUT (DT-16, DT-17) nhưng gắn nhãn rõ "ngoài phạm vi SRS — tham chiếu" để không nhận sai lệch là bug của FR-14 theo đúng đặc tả.
- **Số liệu id biên trên động:** Biên trên của miền id (max id tồn tại) thay đổi theo dữ liệu tại thời điểm chạy; AI phải tính max id bằng GET ngay trước khi chạy BVA-06/07 thay vì hard-code.

## 1. SUT Non-Compliances — Domain Testing (vi phạm đặc tả)

- **BUG-01 (FR-14 — Critical):** `POST /api/categories` chấp nhận **tên rỗng** (`""`) và **thiếu trường name** (lưu `NULL`) — vi phạm trực tiếp "Tên danh mục là bắt buộc, không được để trống". Bằng chứng: DT-03, DT-04, BVA-01 (HTTP 200, tạo bản ghi id 5, 6, 12). Giao diện form thêm danh mục cũng không có thuộc tính `required` và không dấu `*` trên nhãn (vi phạm FR-22).
- **BUG-02 (FR-14):** Chấp nhận tên **chỉ toàn khoảng trắng** (`"   "` → id 7) — thực chất vẫn là tên rỗng. Bằng chứng: DT-05.
- **BUG-03 (FR-14):** **Không kiểm tra kiểu dữ liệu** — `name = 12345` (number) được lưu thẳng vào cột TEXT và GET trả về dạng số. Bằng chứng: DT-08.
- **BUG-04 (FR-12, SEC-03 — Critical):** `POST/PUT/DELETE /api/categories` **không kiểm tra `role = 'admin'`** — user thường (`test@eshop.com`) tạo, sửa, xóa danh mục thành công (HTTP 200). Vi phạm FR-12 ("đều phải yêu cầu Token JWT hợp lệ + `role = 'admin'`") và SEC-03 ("API Admin phải kiểm tra role trong Token, không chỉ kiểm tra sự tồn tại của Token"). Bằng chứng: DT-11, DT-13, DT-17.
- **BUG-05 (Toàn vẹn dữ liệu):** Xóa danh mục **đang được sản phẩm tham chiếu** không bị chặn và không xử lý tham chiếu — 2 sản phẩm (iPhone 15 Pro Max, Samsung S24 Ultra) vẫn giữ `category_id = 1` sau khi danh mục id=1 bị xóa (dangling reference). Bằng chứng: DT-15.
- **BUG-06 (API contract):** `DELETE /api/categories/:id` với **id không tồn tại** (9999, max+1) và **id ngoài miền** (0, −1) đều trả HTTP 200 `"Category deleted"` thay vì 404 — handler không kiểm tra `this.changes` sau khi chạy DELETE. Bằng chứng: DT-14, BVA-04, BVA-05, BVA-06.
- **Ghi nhận hành vi (không phải bug theo SRS):** Tên danh mục trùng lặp được phép tạo (bảng không có UNIQUE) — SRS không cấm nhưng là rủi ro UX/nghiệp vụ đáng đề xuất (DT-06).

## 2. SUT Non-Compliances — BVA (vi phạm tại biên)

- **BUG-01 (tại biên min−1):** Độ dài tên = 0 (min−1 của biên "tối thiểu 1 ký tự không rỗng") — SUT vẫn tạo danh mục thành công (BVA-01), trùng root-cause với DT-03.
- **BUG-06 (tại và ngoài biên id):** Toàn bộ giá trị id ngoài miền hợp lệ (0, −1, max+1) đều trả "xóa thành công" (BVA-04/05/06); chỉ tại biên hợp lệ (max, BVA-07) hành vi mới đúng.
- **Biên trên độ dài tên — chưa xác minh:** SRS không định nghĩa max length; SUT cũng không giới hạn (thăm dò 10.000 ký tự được chấp nhận, input UI không có `maxlength`) — ghi nhận là khoảng trống đặc tả (BVA-08, INFORMATIONAL).

## Notes

- Backend chỉ dùng middleware `authenticateToken` (verify JWT) cho POST/PUT/DELETE categories, **không có bước kiểm tra role** — nguyên nhân gốc của BUG-04 (`backend/server.js`, dòng 249–278).
- Bảng `categories` khai báo `name TEXT` không NOT NULL, không UNIQUE (`backend/database.js`, dòng 23–26) — nguyên nhân gốc của BUG-01/02/03.
- Giao diện Web Admin không có chức năng cập nhật danh mục (không có nút Sửa) — **không vi phạm** SRS FR-14 (SRS không yêu cầu Update), nhưng mâu thuẫn với tên "CRUD" ở một số chỗ của đề bài; đã thống nhất phạm vi theo SRS.
- Nút "Xóa" danh mục trên UI **không có dialog xác nhận** — khuyến nghị cải thiện UX (SRS chỉ bắt buộc dialog xác nhận khi xóa sản phẩm khỏi giỏ hàng ở FR-07/FR-24, nên ghi nhận là đề xuất chứ không phải bug).
- Cách tái hiện/dọn môi trường: khởi động lại `node server.js` trong `eshop-sut/backend` để CSDL re-seed về 3 danh mục gốc (Điện thoại, Laptop, Phụ kiện).
