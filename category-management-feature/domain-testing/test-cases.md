# Domain Testing - Test Cases

**Feature:** FR-14 -- Quản lý Danh mục (Category Management)
**SUT:** EShop — Backend API `http://localhost:3000` + Web Admin
**Ngày thực thi:** 2026-10-18 (thực thi trực tiếp qua API trên CSDL seed sạch)
**Tài khoản:** Admin `admin@eshop.com` / User `test@eshop.com`

## Phân hoạch miền giá trị (Equivalence Classes)

### Biến V1 — `name` (tên danh mục, thao tác Create)

| Lớp | Mô tả | Giá trị đại diện | Kết quả mong đợi |
|---|---|---|---|
| V1-N1 | Hợp lệ: chuỗi không rỗng sau khi trim | `"Máy tính bảng"` | Chấp nhận, tạo danh mục |
| V1-N2 | Không hợp lệ: chuỗi rỗng | `""` | Từ chối (FR-14: bắt buộc) |
| V1-N3 | Không hợp lệ: thiếu trường `name` | `{}` | Từ chối (FR-14: bắt buộc) |
| V1-N4 | Không hợp lệ: chỉ toàn khoảng trắng | `"   "` | Từ chối (thực chất là rỗng) |
| V1-N5 | Biên dưới: 1 ký tự | `"X"` | Chấp nhận |
| V1-N6 | Ký tự đặc biệt / emoji (SRS không cấm) | `"Laptop-Gaming!@# 🎮"` | Chấp nhận |
| V1-N7 | Sai kiểu dữ liệu (số thay vì chuỗi) | `12345` | Từ chối |
| V1-N8 | Tên trùng lặp (SRS không cấm trùng) | `"Điện thoại"` | Ghi nhận hành vi thực tế |

### Biến V2 — `id` (thao tác Delete/Update)

| Lớp | Mô tả | Giá trị đại diện | Kết quả mong đợi |
|---|---|---|---|
| V2-I1 | Hợp lệ: id tồn tại | `1` | Xóa/ cập nhật thành công |
| V2-I2 | Không hợp lệ: id dương không tồn tại | `9999` | 404 Not Found |
| V2-I3 | Không hợp lệ: id = 0 | `0` | 404 Not Found |
| V2-I4 | Không hợp lệ: id âm | `-1` | 404 Not Found |

### Biến V3 — Trạng thái token (Access Control theo FR-12)

| Lớp | Mô tả | Kết quả mong đợi (POST/PUT/DELETE) |
|---|---|---|
| V3-A1 | Token admin hợp lệ | Cho phép thao tác |
| V3-A2 | Token user thường | **Từ chối — yêu cầu role admin (FR-12, SEC-03)** |
| V3-A3 | Không có token | 401 Unauthorized |
| V3-A4 | Token sai định dạng/hết hạn | 401/403 |

### Biến V4 — Trạng thái danh mục liên quan sản phẩm (Delete)

| Lớp | Mô tả | Kết quả mong đợi |
|---|---|---|
| V4-S1 | Danh mục không có sản phẩm tham chiếu | Xóa thành công |
| V4-S2 | Danh mục đang được sản phẩm tham chiếu | Chặn xóa hoặc xử lý tham chiếu (không để sản phẩm mồ côi) |

### Biến V5 — Trạng thái danh sách (Read)

| Lớp | Mô tả | Kết quả mong đợi |
|---|---|---|
| V5-L1 | Danh sách có dữ liệu | 200 + mảng danh mục đúng |
| V5-L2 | Danh sách rỗng | 200 + mảng rỗng (empty state) |

## Test Cases

## Test Case 1
**ID:** FR14-DT-01
**Lớp miền:** V1-N1 (Create — tên hợp lệ)
**Precondition:** Backend chạy, CSDL seed sạch, có token admin
**Input Data:** `POST /api/categories` với `{"name": "Máy tính bảng"}`, header `Authorization: Bearer <admin_token>`
**Steps:**
1. Đăng nhập admin lấy JWT token
2. Gửi POST /api/categories với tên hợp lệ
**Expected Result:** HTTP 200, phản hồi `{"message": "Category created", "id": <mới>}`; danh mục xuất hiện trong GET /api/categories
**Actual Result:** ✅ HTTP 200, `{"message": "Category created", "id": 4}`; GET trả về 4 danh mục gồm "Máy tính bảng"
**Status:** **PASS**

## Test Case 2
**ID:** FR14-DT-02
**Lớp miền:** V5-L1 (Read — danh sách có dữ liệu)
**Precondition:** Có ít nhất 1 danh mục trong CSDL
**Input Data:** `GET /api/categories` (không cần token)
**Steps:**
1. Gọi GET /api/categories
2. Đối chiếu số lượng và nội dung với CSDL
**Expected Result:** HTTP 200, mảng JSON đầy đủ các danh mục (id, name)
**Actual Result:** ✅ HTTP 200, trả đúng 4 danh mục (3 seed + 1 vừa thêm), đúng id và tên
**Status:** **PASS**

## Test Case 3
**ID:** FR14-DT-03
**Lớp miền:** V1-N2 (Create — tên rỗng)
**Precondition:** Có token admin
**Input Data:** `POST /api/categories` với `{"name": ""}`
**Steps:**
1. Gửi POST với tên là chuỗi rỗng
**Expected Result:** **Từ chối tạo (HTTP 400)** — FR-14: "Tên danh mục là bắt buộc, không được để trống"
**Actual Result:** ❌ HTTP 200, `{"message": "Category created", "id": 5}` — danh mục tên rỗng được lưu vào CSDL
**Status:** **FAIL → BUG-01**

## Test Case 4
**ID:** FR14-DT-04
**Lớp miền:** V1-N3 (Create — thiếu trường name)
**Precondition:** Có token admin
**Input Data:** `POST /api/categories` với body `{}` (không có trường name)
**Steps:**
1. Gửi POST thiếu hoàn toàn trường name
**Expected Result:** Từ chối tạo (HTTP 400)
**Actual Result:** ❌ HTTP 200, `{"message": "Category created", "id": 6}` — bản ghi có `name = NULL` được lưu
**Status:** **FAIL → BUG-01**

## Test Case 5
**ID:** FR14-DT-05
**Lớp miền:** V1-N4 (Create — tên chỉ toàn khoảng trắng)
**Precondition:** Có token admin
**Input Data:** `POST /api/categories` với `{"name": "   "}`
**Steps:**
1. Gửi POST với tên gồm 3 ký tự khoảng trắng
**Expected Result:** Từ chối tạo (HTTP 400) — tên thực chất là rỗng sau khi trim
**Actual Result:** ❌ HTTP 200, `{"message": "Category created", "id": 7}` — lưu nguyên chuỗi khoảng trắng
**Status:** **FAIL → BUG-02**

## Test Case 6
**ID:** FR14-DT-06
**Lớp miền:** V1-N8 (Create — tên trùng lặp)
**Precondition:** Danh mục "Điện thoại" (id=1) đã tồn tại
**Input Data:** `POST /api/categories` với `{"name": "Điện thoại"}`
**Steps:**
1. Tạo danh mục trùng tên với danh mục có sẵn
**Expected Result:** SRS không quy định tính duy nhất của tên — ghi nhận hành vi thực tế làm cơ sở đánh giá
**Actual Result:** ⚠️ HTTP 200, tạo thành công bản trùng lặp (id=8) — 2 danh mục cùng tên "Điện thoại" tồn tại song song
**Status:** **PASS (theo SRS)** — ghi nhận rủi ro UX/logic: bảng categories không có ràng buộc UNIQUE

## Test Case 7
**ID:** FR14-DT-07
**Lớp miền:** V1-N6 (Create — ký tự đặc biệt/emoji)
**Precondition:** Có token admin
**Input Data:** `POST /api/categories` với `{"name": "Laptop-Gaming!@# 🎮"}`
**Steps:**
1. Tạo danh mục tên chứa ký tự đặc biệt và emoji
**Expected Result:** Chấp nhận (SRS chỉ bắt buộc không rỗng, không cấm ký tự)
**Actual Result:** ✅ HTTP 200, `{"message": "Category created", "id": 9}`, lưu và hiển thị đúng
**Status:** **PASS**

## Test Case 8
**ID:** FR14-DT-08
**Lớp miền:** V1-N7 (Create — sai kiểu dữ liệu)
**Precondition:** Có token admin
**Input Data:** `POST /api/categories` với `{"name": 12345}` (số thay vì chuỗi)
**Steps:**
1. Gửi POST với name là kiểu number
**Expected Result:** Từ chối (HTTP 400) — name phải là chuỗi ký tự
**Actual Result:** ❌ HTTP 200, `{"message": "Category created", "id": 10}` — giá trị 12345 được lưu thẳng vào cột TEXT, GET trả về name dạng số
**Status:** **FAIL → BUG-03**

## Test Case 9
**ID:** FR14-DT-09
**Lớp miền:** V3-A3 (Access Control — không có token)
**Precondition:** Không gửi header Authorization
**Input Data:** `POST /api/categories` với `{"name": "No Token Cat"}`, không token
**Steps:**
1. Gửi POST không kèm token
**Expected Result:** HTTP 401 Unauthorized
**Actual Result:** ✅ HTTP 401, `{"error": "Unauthorized"}`
**Status:** **PASS**

## Test Case 10
**ID:** FR14-DT-10
**Lớp miền:** V3-A4 (Access Control — token không hợp lệ)
**Precondition:** Token sai định dạng `abc.def.ghi`
**Input Data:** `POST /api/categories` với `{"name": "Bad Token Cat"}`, token rác
**Steps:**
1. Gửi POST với token không verify được
**Expected Result:** HTTP 401/403
**Actual Result:** ✅ HTTP 403, `{"error": "Forbidden"}`
**Status:** **PASS**

## Test Case 11
**ID:** FR14-DT-11
**Lớp miền:** V3-A2 (Access Control — token user thường)
**Precondition:** Đăng nhập bằng `test@eshop.com` (role=user) lấy token
**Input Data:** `POST /api/categories` với `{"name": "User Token Cat"}`, token user
**Steps:**
1. Đăng nhập tài khoản user thường
2. Gửi POST tạo danh mục bằng token user
**Expected Result:** **HTTP 403 — FR-12: POST /api/categories yêu cầu `role = 'admin'` trong token (SEC-03)**
**Actual Result:** ❌ HTTP 200, `{"message": "Category created", "id": 11}` — user thường tạo danh mục thành công
**Status:** **FAIL → BUG-04 (CRITICAL)**

## Test Case 12
**ID:** FR14-DT-12
**Lớp miền:** V3-A3 (Delete — không có token)
**Precondition:** Danh mục id=4 đang tồn tại
**Input Data:** `DELETE /api/categories/4`, không token
**Steps:**
1. Gửi DELETE không kèm token
**Expected Result:** HTTP 401 Unauthorized
**Actual Result:** ✅ HTTP 401, `{"error": "Unauthorized"}` — danh mục id=4 còn nguyên
**Status:** **PASS**

## Test Case 13
**ID:** FR14-DT-13
**Lớp miền:** V3-A2 (Delete — token user thường)
**Precondition:** Danh mục id=4 đang tồn tại; token user
**Input Data:** `DELETE /api/categories/4`, token user thường
**Steps:**
1. Gửi DELETE danh mục bằng token user thường
**Expected Result:** HTTP 403 (FR-12 yêu cầu role admin)
**Actual Result:** ❌ HTTP 200, `{"message": "Category deleted"}` — **danh mục id=4 bị user thường xóa thành công**
**Status:** **FAIL → BUG-04 (CRITICAL)**

## Test Case 14
**ID:** FR14-DT-14
**Lớp miền:** V2-I2 (Delete — id không tồn tại)
**Precondition:** Có token admin; id 9999 không có trong CSDL
**Input Data:** `DELETE /api/categories/9999`, token admin
**Steps:**
1. Xóa danh mục có id không tồn tại
**Expected Result:** HTTP 404 Not Found với thông báo phù hợp
**Actual Result:** ❌ HTTP 200, `{"message": "Category deleted"}` — báo xóa thành công dù không xóa được gì
**Status:** **FAIL → BUG-06**

## Test Case 15
**ID:** FR14-DT-15
**Lớp miền:** V4-S2 (Delete — danh mục đang được sản phẩm tham chiếu)
**Precondition:** Danh mục id=1 "Điện thoại" đang có 2 sản phẩm (iPhone 15 Pro Max, Samsung Galaxy S24 Ultra) tham chiếu `category_id = 1`
**Input Data:** `DELETE /api/categories/1`, token admin
**Steps:**
1. Kiểm tra số sản phẩm có category_id=1 trước khi xóa (→ 2 sản phẩm)
2. Gửi DELETE danh mục id=1
3. Kiểm tra lại sản phẩm sau khi xóa
**Expected Result:** Hệ thống chặn xóa hoặc xử lý tham chiếu (không để sản phẩm trỏ tới danh mục không tồn tại)
**Actual Result:** ❌ HTTP 200 xóa thành công; **2 sản phẩm vẫn giữ `category_id = 1` (dangling reference)** — danh mục đã biến mất
**Status:** **FAIL → BUG-05**

## Test Case 16
**ID:** FR14-DT-16
**Lớp miền:** V2-I1 + V3-A1 (Update — tham chiếu ngoài phạm vi SRS)
**Precondition:** Có token admin; SRS FR-14 chỉ yêu cầu Thêm/Xem/Xóa — API PUT tồn tại ở backend nhưng không có trong SRS và UI
**Input Data:** `PUT /api/categories/2` với `{"name": "Laptop & Notebook"}`
**Steps:**
1. Gửi PUT cập nhật tên danh mục id=2 bằng admin
**Expected Result:** HTTP 200, chỉ danh mục id=2 thay đổi (tham chiếu, ngoài SRS)
**Actual Result:** ✅ HTTP 200, `{"message": "Category updated"}`; đã khôi phục tên "Laptop" sau test
**Status:** **PASS** (tham chiếu — ngoài phạm vi FR-14 của SRS)

## Test Case 17
**ID:** FR14-DT-17
**Lớp miền:** V3-A2 (Update — token user thường, tham chiếu ngoài SRS)
**Precondition:** Token user thường
**Input Data:** `PUT /api/categories/2` với `{"name": "Hacked"}`
**Steps:**
1. Gửi PUT bằng token user thường
**Expected Result:** HTTP 403 (FR-12: PUT /api/categories yêu cầu role admin)
**Actual Result:** ❌ HTTP 200, `{"message": "Category updated"}` — **user thường sửa được tên danh mục** (đã khôi phục lại "Laptop" sau test)
**Status:** **FAIL → BUG-04 (CRITICAL, áp dụng cho cả PUT)**

## Test Case 18
**ID:** FR14-DT-18
**Lớp miền:** V5-L2 (Read — danh sách rỗng)
**Precondition:** CSDL seed sạch 3 danh mục (chạy lại backend để re-seed)
**Input Data:** Xóa lần lượt id 1, 2, 3 bằng admin → `GET /api/categories`
**Steps:**
1. Xóa cả 3 danh mục seed
2. Gọi GET /api/categories
**Expected Result:** HTTP 200, mảng rỗng `[]` (không lỗi server)
**Actual Result:** ✅ HTTP 200, `[]` — API trả mảng rỗng đúng (lưu ý: UI Admin hiển thị bảng trống, không có empty-state thân thiện như FR-24 — cần kiểm bằng mắt trên UI)
**Status:** **PASS** (mức API)

## Tổng kết thực thi Domain Testing

| Chỉ số | Giá trị |
|---|---|
| Tổng test case thiết kế | 18 |
| Đã thực thi | 18 |
| **Pass** | **9** (DT-01, 02, 06, 07, 09, 10, 12, 16, 18) |
| **Fail** | **9** (DT-03, 04, 05, 08, 11, 13, 14, 15, 17) |
| Bug xác nhận | 6 nhóm lỗi (BUG-01 → BUG-06, xem ai-mistake.md) |

**Nhận xét:** Luồng Create/Read/Delete cơ bản hoạt động với dữ liệu hợp lệ và token hợp lệ/không hợp lệ ở mức cơ bản (401/403). Tuy nhiên SUT vi phạm nặng ở 2 nhóm: (1) **không validate tên rỗng/khoảng trắng/kiểu dữ liệu** — vi phạm trực tiếp FR-14; (2) **không kiểm tra role admin** trên cả POST/PUT/DELETE — vi phạm FR-12 và SEC-03, cho phép user thường thao tác dữ liệu danh mục. Ngoài ra xóa danh mục không xử lý tham chiếu sản phẩm (toàn vẹn dữ liệu) và DELETE id không tồn tại trả 200 thay vì 404.
