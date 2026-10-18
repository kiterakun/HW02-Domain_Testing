# Boundary Value Analysis - Test Cases

**Feature:** FR-14 -- Quản lý Danh mục (Category Management)
**Ngày thực thi:** 2026-10-18 (thực thi trực tiếp qua API trên CSDL seed sạch, sau các test Domain Testing)
**Quy ước 3 điểm:** mỗi biên lấy đúng 3 giá trị — ngay dưới biên, tại biên, ngay trên biên.

## Test Case 1
**ID:** FR14-BVA-01
**Biên kiểm thử:** Độ dài tên danh mục — min = 1 ký tự
**Điểm biên:** Min − 1 (0 ký tự)
**Precondition:** Có token admin; CSDL seed sạch + dữ liệu từ Domain Testing
**Input Data:** `POST /api/categories` với `{"name": ""}`
**Steps:**
1. Đăng nhập admin lấy token
2. Gửi POST với tên có độ dài 0
**Expected Result:** HTTP 400 — từ chối tạo danh mục (FR-14: không được để trống)
**Actual Result:** ❌ HTTP 200, `{"message": "Category created", "id": 12}` — tạo thành công danh mục rỗng
**Status:** **FAIL → BUG-01**

## Test Case 2
**ID:** FR14-BVA-02
**Biên kiểm thử:** Độ dài tên danh mục — min = 1 ký tự
**Điểm biên:** Min (1 ký tự)
**Precondition:** Có token admin
**Input Data:** `POST /api/categories` với `{"name": "X"}`
**Steps:**
1. Gửi POST với tên đúng 1 ký tự
**Expected Result:** HTTP 200 — chấp nhận (tên ngắn nhất vẫn hợp lệ)
**Actual Result:** ✅ HTTP 200, `{"message": "Category created", "id": 13}`
**Status:** **PASS**

## Test Case 3
**ID:** FR14-BVA-03
**Biên kiểm thử:** Độ dài tên danh mục — min = 1 ký tự
**Điểm biên:** Min + 1 (2 ký tự)
**Precondition:** Có token admin
**Input Data:** `POST /api/categories` với `{"name": "XY"}`
**Steps:**
1. Gửi POST với tên 2 ký tự
**Expected Result:** HTTP 200 — chấp nhận
**Actual Result:** ✅ HTTP 200, `{"message": "Category created", "id": 14}`
**Status:** **PASS**

## Test Case 4
**ID:** FR14-BVA-04
**Biên kiểm thử:** Miền `id` khi DELETE — biên dưới (min id = 1)
**Điểm biên:** Dưới biên dưới (id = 0)
**Precondition:** Có token admin; id 0 không tồn tại
**Input Data:** `DELETE /api/categories/0`
**Steps:**
1. Gửi DELETE với id = 0
**Expected Result:** HTTP 404 Not Found
**Actual Result:** ❌ HTTP 200, `{"message": "Category deleted"}` — báo thành công cho id không hợp lệ
**Status:** **FAIL → BUG-06**

## Test Case 5
**ID:** FR14-BVA-05
**Biến kiểm thử:** Miền `id` khi DELETE — dưới biên dưới
**Điểm biên:** id âm (−1)
**Precondition:** Có token admin
**Input Data:** `DELETE /api/categories/-1`
**Steps:**
1. Gửi DELETE với id âm
**Expected Result:** HTTP 404 Not Found
**Actual Result:** ❌ HTTP 200, `{"message": "Category deleted"}`
**Status:** **FAIL → BUG-06**

## Test Case 6
**ID:** FR14-BVA-06
**Biên kiểm thử:** Miền `id` khi DELETE — biên trên (id lớn nhất đang tồn tại = 14)
**Điểm biên:** Trên biên trên (id = max + 1 = 15, không tồn tại)
**Precondition:** Có token admin; id 15 chưa được tạo
**Input Data:** `DELETE /api/categories/15`
**Steps:**
1. Gửi DELETE với id ngay trên id lớn nhất đang tồn tại
**Expected Result:** HTTP 404 Not Found
**Actual Result:** ❌ HTTP 200, `{"message": "Category deleted"}`
**Status:** **FAIL → BUG-06**

## Test Case 7
**ID:** FR14-BVA-07
**Biên kiểm thử:** Miền `id` khi DELETE — biên trên
**Điểm biên:** Tại biên trên (id = 14 — bản ghi cuối cùng đang tồn tại)
**Precondition:** Có token admin; danh mục id=14 ("XY") tồn tại
**Input Data:** `DELETE /api/categories/14`
**Steps:**
1. Gửi DELETE với id tại biên trên của miền id hợp lệ
**Expected Result:** HTTP 200 — xóa thành công bản ghi cuối
**Actual Result:** ✅ HTTP 200, `{"message": "Category deleted"}` — xóa đúng bản ghi tồn tại
**Status:** **PASS**

## Test Case 8 (thăm dò — không tính Pass/Fail)
**ID:** FR14-BVA-08
**Biên kiểm thử:** Độ dài tên danh mục — biên trên (SRS không định nghĩa max)
**Điểm biên:** Thăm dò xa (10.000 ký tự)
**Precondition:** Có token admin
**Input Data:** `POST /api/categories` với `{"name": "Z" × 10.000}` — đã xóa bản ghi sau test
**Steps:**
1. Gửi POST với tên cực dài
**Expected Result:** Chưa xác minh (SRS không quy định max) — ghi nhận hành vi thực tế
**Actual Result:** ⚠️ HTTP 200, chấp nhận tên 10.000 ký tự (SQLite TEXT không giới hạn) — **biên trên không tồn tại trong cài đặt hiện tại**; UI không có maxlength trên input
**Status:** **INFORMATIONAL** — đề xuất SRS bổ sung quy tắc max length

## Tổng kết thực thi BVA

| Chỉ số | Giá trị |
|---|---|
| Tổng test case BVA (3-point chính thức) | 7 |
| Đã thực thi | 7 (+1 thăm dò) |
| **Pass** | **3** (BVA-02, 03, 07) |
| **Fail** | **4** (BVA-01, 04, 05, 06) |
| Bug liên quan | BUG-01 (tên rỗng được chấp nhận tại min−1), BUG-06 (DELETE id ngoài miền trả 200) |

**Nhận xét:**
- **Biên độ dài tên:** đúng 3 điểm (0/1/2 ký tự) — chỉ giá trị tại biên min và min+1 hoạt động đúng; **giá trị min−1 (rỗng) bị SUT chấp nhận trái FR-14**.
- **Biên id:** 4 điểm kiểm (0, −1, max, max+1) — **toàn bộ id ngoài miền hợp lệ đều trả "xóa thành công" 200** thay vì 404, cho thấy handler DELETE không kiểm tra `this.changes` sau khi xóa.
- **Biên trên độ dài tên không tồn tại** trong SRS lẫn cài đặt — đã ghi là "chưa xác minh" thay vì tự đặt quy tắc, đúng quy trình.
