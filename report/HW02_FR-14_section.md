# FR-14 -- Category management (nội dung đã điền cho mục 2.3)

> **Cách dùng:** copy toàn bộ phần dưới đây (từ `## 2.3.` đến hết mục 2.3.7) thay thế cho mục 2.3 tương ứng trong `HW02_Domain_Testing_Report.md`.
>
> **Nguồn nội dung:** tài liệu trong `category-management-feature/` (feature-overview.md, domain-testing/, bva/, ai-mistake.md), đã đối chiếu với mã nguồn SUT: `eshop-sut/backend/server.js` (dòng 243--278 -- các endpoint categories; dòng 100--110 -- middleware `authenticateToken`), `eshop-sut/backend/database.js` (bảng `categories` dòng 23--26, seed dòng 84--88), `eshop-sut/frontend-admin/src/App.jsx` (dòng 142--160 -- thêm/xóa danh mục; dòng 294--335 -- giao diện tab Danh mục).
>
> **Lưu ý:** khác với FR-03 (chưa thực thi), toàn bộ test case FR-14 **đã được thực thi trực tiếp trên backend API** (`http://localhost:3000`) ngày 2026-10-18 trên CSDL seed sạch (mỗi lượt chạy khởi động lại `node server.js` để re-seed về 3 danh mục gốc). Các cột "Kết quả thực tế" và "Trạng thái" đã điền theo kết quả thực thi; theo yêu cầu, không đưa cột "Bằng chứng" vào các bảng test case.

---

## 2.3. FR-14 -- Category management (CRUD)

### 2.3.1. Mô tả chức năng và yêu cầu

- **Mục đích chức năng:** Cho phép Admin quản lý danh mục sản phẩm: Thêm danh mục mới, Xem danh sách danh mục, Xóa danh mục, thông qua phân hệ Web Admin (tab "Quản lý Danh mục") hoặc API backend.
- **Vai trò/quyền truy cập:** Chỉ tài khoản `role = 'admin'`. Theo FR-12, mọi API có tính ảnh hưởng dữ liệu (`POST/PUT/DELETE /api/categories`) phải yêu cầu (1) Token JWT hợp lệ và (2) `role = 'admin'` trong Token. Riêng `GET /api/categories` là API công khai (dùng cho dropdown danh mục ở nhiều màn hình).
- **Các trường dữ liệu thực tế trên SUT:**
  - API: `name` (duy nhất) cho POST/PUT; `id` (tham số đường dẫn) cho PUT/DELETE.
  - CSDL: bảng `categories (id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT)` -- không NOT NULL, không UNIQUE, không giới hạn độ dài (`database.js` dòng 23--26).
  - UI Admin: 1 ô nhập "Tên danh mục mới" + nút "Thêm mới"; bảng cột ID / Tên Danh Mục / Hành động (nút Xóa). Không có chức năng cập nhật trên UI.
- **Nguồn yêu cầu:**
  - SRS FR-14 (Version 2.0): "Admin có thể Thêm / Xem / Xóa danh mục. Tên danh mục là bắt buộc, không được để trống." (`category-management-feature/feature-overview.md`).
  - SRS FR-12, SEC-03: kiểm soát truy cập API admin.
  - API backend: `server.js` dòng 243--247 (GET), 249--255 (POST), 257--267 (PUT), 269--278 (DELETE).
- **Quy tắc xóa/cập nhật (đã xác minh qua mã nguồn):**
  - Xóa: `DELETE FROM categories WHERE id = ?` -- không kiểm tra tồn tại của id, không kiểm tra danh mục có đang được sản phẩm tham chiếu, không trả 404.
  - Cập nhật: SRS FR-14 **không yêu cầu Update**; backend có `PUT /api/categories/:id` nhưng UI không có. Hai test PUT được giữ ở mức "tham chiếu, ngoài phạm vi SRS".
- **Giả định cần xác minh** (chưa xem là yêu cầu chính thức; đã đối chiếu mã nguồn):
  1. "Không được để trống" được hiểu là tên phải có ít nhất 1 ký tự không phải khoảng trắng (min = 1 ký tự sau trim) -- SRS không nêu độ dài tối đa, cũng không nêu quy tắc ký tự.
  2. Backend không có bất kỳ lệnh validate nào cho `name` (`server.js` dòng 249--251 chèn thẳng `req.body.name`) -- mọi giá trị (rỗng, NULL, kiểu số) đều lọt vào INSERT.
  3. Backend chỉ dùng middleware `authenticateToken` (verify JWT, dòng 100--110), **không kiểm tra role** -- trái với FR-12/SEC-03.
  4. SRS không yêu cầu tên danh mục duy nhất; bảng CSDL không có UNIQUE -- trùng lặp được phép về mặt kỹ thuật.

### 2.3.2. Domain Testing -- phân tích miền

| Biến/điều kiện | Miền/lớp hợp lệ | Miền/lớp không hợp lệ | Căn cứ | Cần xác minh? |
| --- | --- | --- | --- | --- |
| Tên danh mục (`name`) | Chuỗi không rỗng sau khi trim (VD: "Máy tính bảng", "X", "Laptop-Gaming!@# 🎮") | (a) Chuỗi rỗng `""`; (b) thiếu trường `name` → NULL; (c) chỉ toàn khoảng trắng `"   "`; (d) sai kiểu dữ liệu (số `12345`) | SRS FR-14 (bắt buộc, không để trống); `server.js` dòng 249--251 (không validate); `database.js` dòng 23--26 (TEXT, không ràng buộc) | Có -- mã nguồn không có validate nên dự kiến cả 4 lớp bị chấp nhận khi thực thi |
| Độ dài tên | Từ 1 ký tự trở lên (min = 1); không có biên trên được SRS định nghĩa | 0 ký tự (rỗng) | SRS FR-14; không có maxlength ở UI (`App.jsx` dòng 298--304) và không có ràng buộc CSDL | Có -- xem BVA 2.3.4 |
| Tính duy nhất/tên trùng | SRS không quy định UNIQUE; CSDL không có ràng buộc UNIQUE | (tham chiếu) Tên trùng với danh mục có sẵn (VD: tạo lại "Điện thoại") | `database.js` dòng 23--26; SRS im lặng | Có -- ghi nhận hành vi, không kết luận vi phạm SRS |
| Thao tác CRUD | Create (POST, có token), Read (GET công khai), Delete (DELETE, có token); Update chỉ tồn tại ở API (ngoài SRS) | Gọi API thiếu trường bắt buộc; thao tác với id không hợp lệ | SRS FR-14; `api_specification.md` mục 3.4; `server.js` dòng 243--278 | Có -- đặc biệt DELETE với id không tồn tại |
| Trạng thái danh mục/sản phẩm liên quan | Xóa danh mục không có sản phẩm tham chiếu | Xóa danh mục đang được sản phẩm tham chiếu (id=1 có 2 sản phẩm) | `products.category_id` tham chiếu `categories.id` không có khóa ngoại (`database.js` dòng 64--71); phạm vi kiểm thử đề bài mục "Điều kiện xóa danh mục đang được sản phẩm tham chiếu (nếu có)" | Có -- không có cơ chế chặn trong mã nguồn |
| Quyền người dùng (token) | (a) Token admin hợp lệ cho POST/PUT/DELETE; (b) không cần token cho GET | (c) Không có token (kỳ vọng 401); (d) token sai định dạng (kỳ vọng 403); (e) token user thường (kỳ vọng 403 theo FR-12) | SRS FR-12, SEC-03; `server.js` dòng 100--110 (`authenticateToken` chỉ verify JWT, không đọc `role`) | **Có (quan trọng)** -- mã nguồn không kiểm tra role, dự đoán lớp (e) bị chấp nhận trái FR-12 |

**Giải thích phân hoạch miền:** Miền được phân theo 3 thao tác SRS yêu cầu (Create/Read/Delete) kết hợp 3 trục điều kiện: dữ liệu `name`, `id` đích, và trạng thái token. Mỗi lớp tách riêng vì tạo kết quả đầu ra khác nhau: 200 + bản ghi mới (create hợp lệ), 200 + danh sách (read), 200 + xóa (delete hợp lệ), 401/403 (lỗi xác thực), 400 (lỗi dữ liệu theo kỳ vọng SRS). Lớp "token user thường" được ưu tiên cao nhất vì FR-12/SEC-03 tuyên bố tường minh, trong khi mã nguồn chỉ verify JWT -- đây là khoảng cách lớn nhất giữa đặc tả và cài đặt. Lớp "danh mục có sản phẩm tham chiếu" liên quan tính toàn vẹn dữ liệu (dangling reference). Hai test PUT được giữ ngoài danh sách chính với nhãn "tham chiếu" vì SRS FR-14 không yêu cầu Update, nhưng vẫn chạy để xác minh pattern thiếu kiểm tra role lan sang cả PUT.

### 2.3.3. Test case Domain Testing

| Test Case ID | Thao tác/lớp miền | Tiền điều kiện | Dữ liệu đầu vào | Các bước thực hiện | Kết quả mong đợi | Kết quả thực tế | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FR14-DT-01 | Create -- dữ liệu hợp lệ (V1-N1) | Backend chạy, CSDL seed sạch (3 danh mục), có token admin | `POST /api/categories`, body `{"name": "Máy tính bảng"}`, header `Authorization: Bearer <admin_token>` | 1. Đăng nhập `admin@eshop.com` lấy JWT<br>2. Gửi POST với tên hợp lệ<br>3. Gọi GET /api/categories đối chiếu | HTTP 200, `{"message": "Category created", "id": <mới>}`; danh mục xuất hiện trong danh sách | HTTP 200, id=4; GET trả 4 danh mục gồm "Máy tính bảng" | **Pass** |
| FR14-DT-02 | Read -- danh sách có dữ liệu (V5-L1) | Có ít nhất 1 danh mục trong CSDL | `GET /api/categories` (không cần token) | 1. Gọi GET /api/categories<br>2. Đối chiếu số lượng, id, tên với CSDL | HTTP 200, mảng JSON đầy đủ danh mục | HTTP 200, đúng 4 danh mục (3 seed + 1 vừa thêm), id và tên chính xác | **Pass** |
| FR14-DT-03 | Create -- tên rỗng (V1-N2) | Có token admin | `POST /api/categories`, body `{"name": ""}` | 1. Gửi POST với tên là chuỗi rỗng | **Từ chối (HTTP 400)** -- FR-14: "không được để trống" | HTTP 200, `{"message": "Category created", "id": 5}` -- danh mục rỗng được lưu | **Fail → BUG-01** |
| FR14-DT-04 | Create -- thiếu trường name (V1-N3) | Có token admin | `POST /api/categories`, body `{}` | 1. Gửi POST không có trường name | Từ chối (HTTP 400) | HTTP 200, id=6, bản ghi `name = NULL` được lưu | **Fail → BUG-01** |
| FR14-DT-05 | Create -- tên chỉ toàn khoảng trắng (V1-N4) | Có token admin | `POST /api/categories`, body `{"name": "   "}` | 1. Gửi POST với 3 ký tự khoảng trắng | Từ chối (HTTP 400) -- thực chất là tên rỗng sau trim | HTTP 200, id=7, lưu nguyên chuỗi khoảng trắng | **Fail → BUG-02** |
| FR14-DT-06 | Create -- tên trùng lặp (V1-N8) | Danh mục "Điện thoại" (id=1) đã tồn tại | `POST /api/categories`, body `{"name": "Điện thoại"}` | 1. Tạo danh mục trùng tên có sẵn<br>2. Gọi GET đối chiếu | Ghi nhận hành vi -- SRS không quy định UNIQUE | HTTP 200, id=8; 2 bản ghi cùng tên "Điện thoại" tồn tại song song | **Pass (theo SRS)** -- ghi nhận rủi ro |
| FR14-DT-07 | Create -- ký tự đặc biệt/emoji (V1-N6) | Có token admin | `POST /api/categories`, body `{"name": "Laptop-Gaming!@# 🎮"}` | 1. Tạo danh mục tên chứa ký tự đặc biệt + emoji<br>2. GET đối chiếu | Chấp nhận (SRS chỉ bắt buộc không rỗng, không cấm ký tự) | HTTP 200, id=9, lưu và hiển thị đúng | **Pass** |
| FR14-DT-08 | Create -- sai kiểu dữ liệu (V1-N7) | Có token admin | `POST /api/categories`, body `{"name": 12345}` (number) | 1. Gửi POST với name là kiểu số | Từ chối (HTTP 400) -- name phải là chuỗi | HTTP 200, id=10; giá trị 12345 lưu thẳng cột TEXT, GET trả name dạng số | **Fail → BUG-03** |
| FR14-DT-09 | Access Control -- POST không có token (V3-A3) | Không gửi header Authorization | `POST /api/categories`, body `{"name": "No Token Cat"}` | 1. Gửi POST không kèm token | HTTP 401 Unauthorized | HTTP 401, `{"error": "Unauthorized"}` | **Pass** |
| FR14-DT-10 | Access Control -- token sai định dạng (V3-A4) | Token rác `abc.def.ghi` | `POST /api/categories`, token không verify được | 1. Gửi POST với token sai | HTTP 401/403 | HTTP 403, `{"error": "Forbidden"}` | **Pass** |
| FR14-DT-11 | Access Control -- token user thường, POST (V3-A2) | Đăng nhập `test@eshop.com` (role=user) lấy token | `POST /api/categories`, body `{"name": "User Token Cat"}`, token user | 1. Đăng nhập tài khoản user thường<br>2. Gửi POST tạo danh mục bằng token user | **HTTP 403** -- FR-12: POST /api/categories yêu cầu `role='admin'` trong Token (SEC-03) | HTTP 200, `{"message": "Category created", "id": 11}` -- **user thường tạo danh mục thành công** | **Fail → BUG-04 (Critical)** |
| FR14-DT-12 | Access Control -- DELETE không có token (V3-A3) | Danh mục id=4 đang tồn tại | `DELETE /api/categories/4`, không token | 1. Gửi DELETE không kèm token | HTTP 401 Unauthorized; danh mục còn nguyên | HTTP 401; id=4 không bị xóa | **Pass** |
| FR14-DT-13 | Access Control -- DELETE bằng token user (V3-A2) | Danh mục id=4 tồn tại; token user thường | `DELETE /api/categories/4`, token user | 1. Gửi DELETE danh mục bằng token user thường | HTTP 403 (FR-12 yêu cầu role admin) | HTTP 200, `{"message": "Category deleted"}` -- **danh mục bị user thường xóa thành công** | **Fail → BUG-04 (Critical)** |
| FR14-DT-14 | Delete -- id không tồn tại (V2-I2) | Có token admin; id 9999 không có trong CSDL | `DELETE /api/categories/9999`, token admin | 1. Xóa danh mục id không tồn tại | HTTP 404 Not Found, thông báo phù hợp | HTTP 200, `{"message": "Category deleted"}` -- báo xóa thành công dù không xóa được gì | **Fail → BUG-06** |
| FR14-DT-15 | Delete -- danh mục có sản phẩm tham chiếu (V4-S2) | Danh mục id=1 "Điện thoại" có 2 sản phẩm (iPhone 15 Pro Max, Samsung Galaxy S24 Ultra) trỏ tới `category_id = 1` | `DELETE /api/categories/1`, token admin | 1. GET /api/products đếm sản phẩm có category_id=1 (→ 2)<br>2. DELETE danh mục id=1<br>3. GET /api/products kiểm tra lại | Chặn xóa hoặc xử lý tham chiếu -- không để sản phẩm trỏ tới danh mục đã xóa | HTTP 200 xóa thành công; **2 sản phẩm vẫn giữ category_id=1** (dangling reference) | **Fail → BUG-05** |
| FR14-DT-16 | Update -- admin (ngoài phạm vi SRS, tham chiếu) | Có token admin; SRS FR-14 không yêu cầu Update, API PUT tồn tại ở backend, UI không có | `PUT /api/categories/2`, body `{"name": "Laptop & Notebook"}` | 1. Gửi PUT đổi tên danh mục id=2<br>2. Khôi phục tên "Laptop" sau test | HTTP 200, chỉ danh mục id=2 thay đổi (tham chiếu -- không kết luận theo SRS) | HTTP 200, `{"message": "Category updated"}`; đúng id=2, đã khôi phục | **Pass (tham chiếu)** |
| FR14-DT-17 | Update -- token user (ngoài phạm vi SRS, tham chiếu) | Token user thường | `PUT /api/categories/2`, body `{"name": "Hacked"}` | 1. Gửi PUT bằng token user thường<br>2. Khôi phục tên sau test | HTTP 403 (FR-12: PUT /api/categories yêu cầu role admin) | HTTP 200, `{"message": "Category updated"}` -- **user thường sửa được tên danh mục**; đã khôi phục | **Fail → BUG-04 (Critical, áp dụng cả PUT)** |
| FR14-DT-18 | Read -- danh sách rỗng (V5-L2) | CSDL seed sạch 3 danh mục (re-start backend để re-seed) | Xóa lần lượt id 1, 2, 3 bằng admin → `GET /api/categories` | 1. Xóa cả 3 danh mục seed<br>2. Gọi GET /api/categories | HTTP 200, mảng rỗng `[]`; UI hiển thị empty state (FR-24) | HTTP 200, `[]` -- API trả mảng rỗng đúng. **Lưu ý UI:** bảng hiển thị trống, không có empty-state thân thiện (icon + message) -- cần đối chiếu bằng mắt trên trình duyệt | **Pass (mức API)** |

Ghi chú:

1.  Thực thi trực tiếp qua API (fetch/cURL) trên backend chạy local, CSDL được re-seed trước mỗi lượt lớn để loại bỏ ảnh hưởng dữ liệu rác. Kết quả từng test được ghi lại đầy đủ trong `category-management-feature/domain-testing/test-cases.md`.
2.  FR14-DT-16/17 thuộc API `PUT` -- SRS FR-14 không yêu cầu thao tác Update nên hai test này được giữ ở dạng "tham chiếu" để làm bằng chứng pattern thiếu kiểm tra role áp dụng cho cả PUT; không tính vào số liệu Pass/Fail của FR-14 theo SRS khi cần tách bạch (xem thống kê ở 2.3.6 và phần số liệu cuối).
3.  Rà soát UI (`frontend-admin/src/App.jsx`): ô nhập tên không có thuộc tính `required`, nhãn không có dấu `*` (FR-22); nút "Xóa" không có dialog xác nhận (SRS chỉ bắt buộc dialog với xóa khỏi giỏ hàng -- FR-07/FR-24 -- nên ghi nhận là đề xuất UX, không phải bug).

**Đánh giá Domain Testing:** 18 test case phủ đủ 22 lớp tương đương đã xác định trên 5 biến (name: 8 lớp; id: 4 lớp; token: 4 lớp; trạng thái tham chiếu sản phẩm: 2 lớp; trạng thái danh sách: 2 lớp). Kết quả: 9 Pass, 9 Fail (trong đó DT-16 Pass tham chiếu ngoài SRS). Tất cả Fail đều là vi phạm thật của SUT so với SRS, tập trung ở 2 nhóm: (1) không validate `name` (rỗng/khoảng trắng/kiểu dữ liệu) vi phạm FR-14; (2) không kiểm tra role admin trên POST/PUT/DELETE vi phạm FR-12/SEC-03 -- nghiêm trọng nhất. Chi tiết bao phủ từng lớp: `category-management-feature/domain-testing/coverage-report.md`.

### 2.3.4. BVA -- phân tích biên

| Biến/điều kiện | Biên dưới | Biên trên | Căn cứ xác định biên | Giá trị cần thử | Ghi chú |
| --- | --- | --- | --- | --- | --- |
| Độ dài tên danh mục | 1 ký tự (min = 1 -- quy tắc "không được để trống") | Không có -- SRS không quy định độ dài tối đa; CSDL TEXT và ô nhập UI đều không giới hạn | SRS FR-14; `database.js` dòng 23--26; `App.jsx` dòng 298--304 (input không có `maxlength`) | Min−1: 0 ký tự (`""`); Min: 1 ký tự (`"X"`); Min+1: 2 ký tự (`"XY"`). Biên trên: thăm dò `"Z"×10.000` (không kết luận Pass/Fail) | Giá trị Min−1 trùng lớp V1-N2 của Domain Testing (DT-03) -- dùng để xác nhận biên bằng phương pháp 3 điểm độc lập |
| Trường bắt buộc rỗng theo quy tắc trim | Tên có ≥ 1 ký tự khác khoảng trắng | Không áp dụng nếu không có biên trên | SRS FR-14 ("không được để trống") | `"   "` (3 khoảng trắng) -- length > 0 nhưng thực chất rỗng sau trim | Sát biên nghiệp vụ (không phải 3-point chuẩn) -- phủ ở DT-05 |
| Miền `id` khi DELETE | 1 (id nguyên dương nhỏ nhất của AUTOINCREMENT) | Id lớn nhất đang tồn tại (động theo dữ liệu; tại thời điểm chạy = 14) | `database.js` dòng 24 (`INTEGER PRIMARY KEY AUTOINCREMENT`); `server.js` dòng 269--278 | Dưới biên dưới: id = 0 và id = −1; Tại biên trên: id = max (14); Trên biên trên: id = max+1 (15) | Biên trên là miền động -- phải GET danh sách ngay trước khi chạy để lấy max id hiện thời |
| Số lượng bản ghi trong danh sách (Read) | 0 (danh sách rỗng) | Không có | SRS FR-24 (empty state) | 0 bản ghi → GET trả `[]`; ≥ 1 bản ghi → GET trả mảng | Biến trạng thái rời rạc -- phủ bằng DT-02/DT-18, không áp dụng 3-point |
| Trạng thái token | Không có (biến rời rạc: admin / user / không có / sai) | Không có | FR-12; `server.js` dòng 100--110 | Không áp dụng 3-point | Phủ bằng lớp tương đương (DT-09 → DT-13, DT-17) |

**Giải thích BVA:** Hai biên có thể áp dụng đúng 3 điểm: (1) **độ dài tên -- biên dưới min = 1**: Min−1 (0 ký tự, `""`) kỳ vọng bị từ chối với HTTP 400 theo FR-14, Min (1 ký tự, `"X"`) và Min+1 (2 ký tự, `"XY"`) kỳ vọng được chấp nhận -- kết quả thực tế chỉ đúng tại Min và Min+1, còn Min−1 **vẫn được tạo thành công (HTTP 200)** do backend không có bất kỳ lệnh validate nào, xác nhận sai lệch ở đúng điểm biên; (2) **biên trên của miền id DELETE** là id bản ghi cuối cùng (giá trị động, tại thời điểm chạy là 14): tại biên (id=14) xóa đúng bản ghi tồn tại, ngay trên biên (id=15) và dưới biên dưới (id=0, id=−1) kỳ vọng trả 404 -- thực tế **cả ba điểm ngoài miền đều trả HTTP 200 "Category deleted"** vì handler không kiểm tra `this.changes` sau lệnh DELETE. Biên trên độ dài tên không được SRS định nghĩa nên chỉ thăm dò (10.000 ký tự vẫn được chấp nhận) và ghi "chưa xác minh" thay vì tự đặt quy tắc. Trạng thái token và trạng thái danh sách là biến rời rạc nên không áp dụng 3-point, đã phủ bằng Domain Testing.

### 2.3.5. Test case BVA

| Test Case ID | Biến/biên kiểm thử | Giá trị | Tiền điều kiện | Các bước thực hiện | Kết quả mong đợi | Kết quả thực tế | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FR14-BVA-01 | Độ dài tên -- Min−1 | `name = ""` (0 ký tự) | Có token admin | 1. Gửi `POST /api/categories` với tên rỗng | HTTP 400 -- từ chối (FR-14: không được để trống) | HTTP 200, `{"message": "Category created", "id": 12}` | **Fail → BUG-01** |
| FR14-BVA-02 | Độ dài tên -- Min | `name = "X"` (1 ký tự) | Có token admin | 1. Gửi `POST /api/categories` với tên 1 ký tự | HTTP 200 -- chấp nhận (tên ngắn nhất hợp lệ) | HTTP 200, id=13 | **Pass** |
| FR14-BVA-03 | Độ dài tên -- Min+1 | `name = "XY"` (2 ký tự) | Có token admin | 1. Gửi `POST /api/categories` với tên 2 ký tự | HTTP 200 -- chấp nhận | HTTP 200, id=14 | **Pass** |
| FR14-BVA-04 | Miền id DELETE -- dưới biên dưới | `id = 0` | Có token admin | 1. Gửi `DELETE /api/categories/0` | HTTP 404 Not Found | HTTP 200, `{"message": "Category deleted"}` | **Fail → BUG-06** |
| FR14-BVA-05 | Miền id DELETE -- dưới biên dưới (xa) | `id = −1` | Có token admin | 1. Gửi `DELETE /api/categories/-1` | HTTP 404 Not Found | HTTP 200, `{"message": "Category deleted"}` | **Fail → BUG-06** |
| FR14-BVA-06 | Miền id DELETE -- trên biên trên | `id = 15` (max tồn tại = 14, nên 15 không tồn tại) | Có token admin; đã GET danh sách xác nhận max id hiện thời | 1. Gửi `DELETE /api/categories/15` | HTTP 404 Not Found | HTTP 200, `{"message": "Category deleted"}` | **Fail → BUG-06** |
| FR14-BVA-07 | Miền id DELETE -- tại biên trên | `id = 14` (bản ghi cuối đang tồn tại) | Có token admin; danh mục id=14 (`"XY"`) tồn tại | 1. Gửi `DELETE /api/categories/14` | HTTP 200 -- xóa đúng bản ghi cuối | HTTP 200, `{"message": "Category deleted"}` -- xóa đúng bản ghi tồn tại | **Pass** |
| FR14-BVA-08 | Độ dài tên -- biên trên (thăm dò) | `name = "Z" × 10.000` | Có token admin; SRS không định nghĩa max | 1. Gửi POST với tên 10.000 ký tự<br>2. Xóa bản ghi sau test | Chưa xác minh (SRS không quy định) -- ghi nhận hành vi | HTTP 200, chấp nhận tên 10.000 ký tự (SQLite TEXT không giới hạn; UI không có `maxlength`) | **Informational** |

Ghi chú:

1.  Biên trên của miền id là giá trị động (id bản ghi cuối cùng thay đổi theo dữ liệu): tại thời điểm thực thi BVA, sau các test Domain Testing, danh mục có id lớn nhất là 14 nên chọn id=15 cho "trên biên" và id=14 cho "tại biên".
2.  FR14-BVA-08 là test thăm dò (probe) cho biên mà SRS chưa định nghĩa -- không tính vào số liệu Pass/Fail; kết quả dùng làm căn cứ đề xuất bổ sung quy tắc max length vào SRS.
3.  Chi tiết giá trị 3 điểm: `category-management-feature/bva/bva-values.md`.

### 2.3.6. Kết quả thực thi

- Tổng test case Domain Testing: **18** (trong đó 2 test tham chiếu ngoài SRS: DT-16, DT-17)
- Tổng test case BVA: **7** chính thức (3-point) + **1** thăm dò (BVA-08)
- Pass: **12** | Fail: **13** | Blocked: 0 | Not Executed: 0
  - Domain Testing: 9 Pass / 9 Fail (DT-16 Pass ở dạng tham chiếu)
  - BVA: 3 Pass / 4 Fail
- Bug liên quan: **6 nhóm lỗi xác nhận thực thi** (BUG-01 → BUG-06, chi tiết 2.3.7 và `category-management-feature/ai-mistake.md`):
  - BUG-01 (Critical): chấp nhận tên rỗng `""` và thiếu trường `name` (NULL) -- DT-03, DT-04, BVA-01
  - BUG-02: chấp nhận tên chỉ toàn khoảng trắng -- DT-05
  - BUG-03: không kiểm tra kiểu dữ liệu (`name = 12345`) -- DT-08
  - BUG-04 (Critical): POST/PUT/DELETE không kiểm tra `role='admin'` -- user thường thao tác được -- DT-11, DT-13, DT-17
  - BUG-05: xóa danh mục đang được sản phẩm tham chiếu, để lại dangling reference -- DT-15
  - BUG-06: DELETE id không tồn tại/ngoài miền trả 200 thay vì 404 -- DT-14, BVA-04/05/06
- Nhận xét: Luồng Create/Read/Delete với dữ liệu hợp lệ và xác thực admin (hoặc thiếu/sai token ở mức 401/403) hoạt động đúng. Hai nhóm vi phạm trọng yếu: (1) **không validate `name`** -- mọi giá trị không rỗng-nghĩa-là-hợp lệ đều lọt vào CSDL; (2) **không kiểm tra role admin** -- middleware chỉ verify chữ ký JWT. Ngoài ra, handler DELETE không kiểm tra tồn tại của bản ghi và không quan tâm khóa ngoại với bảng `products`. Mọi thao tác test đã dọn dữ liệu và re-seed CSDL về 3 danh mục gốc sau mỗi đợt chạy.

### 2.3.7. Đánh giá feature

- **Độ bao phủ lớp miền và biên:** Domain Testing phủ **22/22 lớp** đã xác định (name 8/8, id 4/4, token 4/4, tham chiếu sản phẩm 2/2, trạng thái danh sách 2/2). BVA phủ đủ 3 điểm cho biên duy nhất xác minh được về độ dài tên (0/1/2 ký tự) và 4 điểm biên của miền id (0, −1, max, max+1); biên trên độ dài tên ghi "chưa xác minh" vì SRS không định nghĩa. Không có lớp hay biên nào thiết kế mà bỏ thực thi (18/18 và 7/7 đã chạy).
- **Rủi ro còn lại:** (1) UI Admin chưa kiểm bằng tay trên trình duyệt (form thêm, nút Xóa, empty state khi bảng trống) -- đã rà soát mã nguồn nhưng chưa có ảnh chụp màn hình; (2) trùng lặp tên danh mục được phép -- SRS không cấm nhưng là rủi ro nghiệp vụ; (3) token hết hạn chưa test riêng (xác thực qua `jwt.verify` chung, hành vi tương tự token sai); (4) `DELETE /api/categories/abc` (id không phải số) ngoài các lớp đã định nghĩa -- có thể bổ sung sau.
- **Đóng góp hữu ích của AI:** phân lớp 5 biến bám sát SRS, tách bạch rõ "lớp SRS không quy định" (trùng lặp, ký tự đặc biệt) khỏi "lớp SRS cấm" (rỗng); nhận diện đúng khoảng cách FR-12 ↔ mã nguồn ngay từ bước khảo sát (middleware không đọc role) và thiết kế 3 test (POST/DELETE/PUT) để chứng minh pattern; giá trị biên id được tính động theo max id thực tế thay vì hard-code; quy trình tái lập dữ liệu sạch bằng re-seed.
- **Đầu ra AI chưa chính xác/sai sót** (tổng hợp từ `category-management-feature/ai-mistake.md`):
  1. **Probe gây bẩn CSDL:** lượt khảo sát đầu gọi POST/DELETE trên SUT mà chưa cô lập dữ liệu, tạo bản ghi rác (id 4--13) và mất danh mục id=1; khắc phục bằng cách restart backend để re-seed và chạy lại toàn bộ lượt chính thức trên dữ liệu sạch, theo thứ tự test case.
  2. **Giả định mã lỗi 401/403:** ban đầu mặc định token sai trả 401; thực tế `jwt.verify` trả 403 cho token sai định dạng, 401 cho token thiếu -- điều chỉnh kỳ vọng theo bằng chứng thực thi.
  3. **Nhầm lẫn phạm vi Update:** đề bài/báo cáo ghi "CRUD" nhưng SRS FR-14 chỉ yêu cầu Thêm/Xem/Xóa; AI ban đầu đặt test Update vào danh sách chính -- đã chuyển 2 test PUT (DT-16/17) thành "tham chiếu, ngoài phạm vi SRS" và tách bạch khi thống kê.
  4. **Biên id động:** phải tính max id bằng GET ngay trước khi chạy BVA (14 tại thời điểm chạy), không hard-code.
- **Phần người kiểm thử đã bổ sung:** đối chiếu khai báo của AI với mã nguồn (`server.js` dòng 100--110, 243--278; `database.js` dòng 23--26; `App.jsx` dòng 142--160, 294--335) trước khi chấp nhận từng lớp miền; ghi rõ căn cứ dòng lệnh cho từng quy tắc; xác định 6 nhóm bug có bằng chứng thực thi; dọn dữ liệu và re-seed sau mỗi đợt chạy để bảo đảm tính lặp lại.

---

## Số liệu dùng cho các bảng tổng hợp (không copy vào mục 2.3)

Khi cập nhật bảng 2.5 và bảng 6.2 của báo cáo, dùng các số sau cho FR-14:

- Test case thiết kế -- Domain Testing: **18**
- Test case thiết kế -- BVA: **7** (+1 thăm dò BVA-08 không tính số liệu)
- Tổng test case thiết kế: **25**
- Đã thực thi: **25** | Pass: **12** | Fail: **13** | Blocked: **0** | Not Executed: **0**
- Bug xác nhận: **6** (BUG-01 → BUG-06; trong đó 2 nhóm mức Critical: BUG-01, BUG-04)

Ghi chú thống nhất số liệu: nếu muốn tách bạch 2 test PUT "tham chiếu ngoài SRS" khỏi phạm vi FR-14 theo SRS (Thêm/Xem/Xóa), dùng bộ số rút gọn: Domain Testing 16 (7 Pass / 9 Fail), BVA 7 (3 Pass / 4 Fail), tổng 23, đã thực thi 23, Pass 10, Fail 13 -- tuy nhiên toàn bộ file này thống nhất dùng bộ số 18+7=25 để nhất quán với test-cases.md.
