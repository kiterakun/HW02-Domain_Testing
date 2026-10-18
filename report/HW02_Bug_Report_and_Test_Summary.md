# HW02 -- Domain Testing on EShop

# 3. Báo cáo lỗi (Bug Report)

## 3.1. Quy trình phát hiện và xác nhận lỗi

1. Ghi nhận sai lệch giữa kết quả mong đợi (SRS) và kết quả thực tế khi thực
   thi test case Domain Testing/BVA.
2. Tái hiện lỗi với cùng dữ liệu và tiền điều kiện; DB được re-seed về trạng
   thái gốc sau mỗi đợt chạy.
3. Phân biệt lỗi sản phẩm với lỗi dữ liệu/môi trường bằng cách đối chiếu trực
   tiếp mã nguồn SUT (`server.js`, `database.js`, `App.js`,
   `ForgotPassword.jsx`, ...).
4. Đánh số bug thống nhất theo feature: **BUG-FRxx-nn** (FR-03: 2, FR-09: 7,
   FR-14: 6, FR-07: 8). Mỗi lỗi được xác nhận tạo một GitHub Issue theo template
   *Bug Report* với labels `type: bug`, `status: new`.
5. Cập nhật link GitHub Issue vào từng section (mục "Kết quả thực thi") và vào
   bảng tổng hợp dưới đây.

## 3.2. Danh sách lỗi tổng hợp

| Bug ID      | Feature        | Tiêu đề ngắn                                                        | Severity | Priority | Trạng thái | Test Case liên quan              | GitHub Issue |
| ----------- | -------------- | ------------------------------------------------------------------- | -------- | -------- | ---------- | -------------------------------- | ------------ |
| BUG-FR03-01 | FR-03          | Thiếu chỉ báo bước "Bước 1 / 2" và nút "Quay lại đăng nhập"          | Medium   | P3       | Open       | FR03-DT-01                       | [#1](https://github.com/kiterakun/HW02-Domain_Testing/issues/1) |
| BUG-FR03-02 | FR-03          | Regex mật khẩu khác SRS, từ chối mọi mật khẩu hợp lệ theo SRS        | Critical | P1       | Open       | FR03-DT-03, BVA-02, BVA-03       | [#2](https://github.com/kiterakun/HW02-Domain_Testing/issues/2) |
| BUG-FR09-01 | FR-09          | Công thức percent sai -- discount âm, final tăng ~10 lần             | Critical | P1       | Open       | FR09-DT-01/22/23, BVA-03, BVA-10 | [#3](https://github.com/kiterakun/HW02-Domain_Testing/issues/3) |
| BUG-FR09-02 | FR-09          | C3 dùng `>` thay vì `>=` -- đơn đúng bằng ngưỡng bị từ chối          | High     | P2       | Open       | FR09-DT-09/16, BVA-02, BVA-05    | [#4](https://github.com/kiterakun/HW02-Domain_Testing/issues/4) |
| BUG-FR09-03 | FR-09          | Áp coupon không cần JWT (C4 bỏ trống)                                | Critical | P1       | Open       | FR09-DT-12, DT-13                | [#5](https://github.com/kiterakun/HW02-Domain_Testing/issues/5) |
| BUG-FR09-04 | FR-09          | Bỏ `user_id` bypass giới hạn lượt dùng (C5)                          | High     | P2       | Open       | FR09-DT-12 + probe               | [#6](https://github.com/kiterakun/HW02-Domain_Testing/issues/6) |
| BUG-FR09-05 | FR-09          | `final_amount` cho phép âm                                           | Medium   | P3       | Open       | FR09-DT-21                       | [#7](https://github.com/kiterakun/HW02-Domain_Testing/issues/7) |
| BUG-FR09-06 | FR-09          | Sai thứ tự ưu tiên thông báo khi vi phạm C2+C3                       | Low      | P4       | Open       | FR09-DT-18                       | [#8](https://github.com/kiterakun/HW02-Domain_Testing/issues/8) |
| BUG-FR09-07 | FR-09 (FR-08)  | Checkout tin hoàn toàn tổng tiền client gửi                          | Medium   | P3       | Open       | probe (checkout total=1)         | [#9](https://github.com/kiterakun/HW02-Domain_Testing/issues/9) |
| BUG-FR14-01 | FR-14          | Tạo danh mục chấp nhận tên rỗng và thiếu trường name (NULL)          | Critical | P1       | Open       | FR14-DT-03/04, BVA-01            | [#10](https://github.com/kiterakun/HW02-Domain_Testing/issues/10) |
| BUG-FR14-02 | FR-14          | Tạo danh mục chấp nhận tên chỉ toàn khoảng trắng                     | Medium   | P3       | Open       | FR14-DT-05                       | [#11](https://github.com/kiterakun/HW02-Domain_Testing/issues/11) |
| BUG-FR14-03 | FR-14          | Không kiểm tra kiểu dữ liệu trường name                              | Medium   | P3       | Open       | FR14-DT-08                       | [#12](https://github.com/kiterakun/HW02-Domain_Testing/issues/12) |
| BUG-FR14-04 | FR-14          | POST/PUT/DELETE không kiểm tra `role='admin'`                        | Critical | P1       | Open       | FR14-DT-11/13/17                 | [#13](https://github.com/kiterakun/HW02-Domain_Testing/issues/13) |
| BUG-FR14-05 | FR-14          | Xóa danh mục đang được sản phẩm tham chiếu (dangling reference)      | High     | P2       | Open       | FR14-DT-15                       | [#14](https://github.com/kiterakun/HW02-Domain_Testing/issues/14) |
| BUG-FR14-06 | FR-14          | DELETE id không tồn tại trả 200 thay vì 404                          | Low      | P4       | Open       | FR14-DT-14, BVA-04/05/06         | [#15](https://github.com/kiterakun/HW02-Domain_Testing/issues/15) |
| BUG-FR07-01 | FR-07          | Sửa số lượng trong giỏ luôn cộng thêm 1                              | High     | P2       | Open       | FR07-DT-10, BVA-05, BVA-06       | [#16](https://github.com/kiterakun/HW02-Domain_Testing/issues/16) |
| BUG-FR07-02 | FR-07          | Nhãn tổng tiền "Tổng tạm tính" thay vì "Tổng cộng"                   | Medium   | P3       | Open       | FR07-DT-13/14/22, BVA-08/09/17/18| [#17](https://github.com/kiterakun/HW02-Domain_Testing/issues/17) |
| BUG-FR07-03 | FR-07          | Empty state giỏ trống thiếu hình minh họa                            | Medium   | P3       | Open       | FR07-DT-12/16, BVA-07/16         | [#18](https://github.com/kiterakun/HW02-Domain_Testing/issues/18) |
| BUG-FR07-04 | FR-07          | Xóa sản phẩm không có dialog xác nhận                                | High     | P2       | Open       | FR07-DT-12/21, BVA-16/17/18      | [#19](https://github.com/kiterakun/HW02-Domain_Testing/issues/19) |
| BUG-FR07-05 | FR-07          | Cột Số lượng không có nút +/−                                        | Medium   | P3       | Open       | FR07-DT-14, DT-20                | [#20](https://github.com/kiterakun/HW02-Domain_Testing/issues/20) |
| BUG-FR07-06 | FR-07          | Badge "Giỏ (n)" đếm số dòng; FR-23 mơ hồ cần làm rõ đặc tả           | Low      | P4       | Open       | FR07-DT-19, BVA-13/14/15         | [#21](https://github.com/kiterakun/HW02-Domain_Testing/issues/21) |
| BUG-FR07-07 | FR-07          | Số thực bị cắt cụt âm thầm ("3.5" → thêm 3)                          | Low      | P4       | Open       | FR07-DT-09                       | [#22](https://github.com/kiterakun/HW02-Domain_Testing/issues/22) |
| BUG-FR07-08 | FR-07 (FR-08)  | Checkout gửi `cart.slice(0, -1)` -- mất dòng cuối của giỏ            | High     | P2       | Open       | rà soát mã nguồn (App.js:391)    | [#23](https://github.com/kiterakun/HW02-Domain_Testing/issues/23) |

> Ghi chú: BUG-FR09-07 và BUG-FR07-08 là hai lỗi thuộc luồng Checkout (FR-08)
> được phát hiện trong khi kiểm thử FR-09 và FR-07; đánh số theo feature section
> phát hiện. FR-03 ngoài 2 bug trên còn 2 sai lệch SRS ↔ mã nguồn được xác nhận
> khi thực thi nhưng không cấu thành Fail của test case nào (OTP 4 chữ số thay
> vì 6; thiếu trường xác nhận mật khẩu) -- ghi nhận tại section FR-03, không tạo
> issue riêng.

## 3.3. Chi tiết từng lỗi

### BUG-FR03-01: Thiếu chỉ báo bước "Bước 1 / 2" và nút "Quay lại đăng nhập" ở bước 1

- **Feature:** FR-03 (Forgot password and password reset, two steps)
- **Test Case ID:** FR03-DT-01
- **Severity:** Medium -- **Priority:** P3
- **Môi trường:** frontend-web `http://localhost:5173/forgot-password` (React + Vite, Edge Chromium headless); backend Node/Express + SQLite `http://localhost:3000` (seed sạch); 18-10-2026.
- **Tiền điều kiện:** Đã có tài khoản `test@eshop.com` trong CSDL.
- **Dữ liệu kiểm thử:** `email = "test@eshop.com"`.
- **Các bước tái hiện:**
  1. Truy cập `http://localhost:5173/forgot-password` (bước 1: nhập email).
  2. Quan sát giao diện bước 1.
  3. Nhập email hợp lệ, bấm "Lấy mã OTP" để sang bước 2; quan sát giao diện bước 2.
- **Kết quả mong đợi:** UI hiển thị chỉ báo "Bước 1 / 2" theo từng bước; bước 1 có nút "Quay lại đăng nhập" (SRS FR-03).
- **Kết quả thực tế:** Cả hai bước đều không có chỉ báo "Bước 1 / 2"; bước 1 không có nút "Quay lại đăng nhập"; chỉ có nút "← Quay lại" ở bước 2 (quay về bước 1, hoạt động đúng).
- **Tần suất tái hiện:** 1/1 (FR03-DT-01).
- **Ảnh/log minh chứng:** Không lưu ảnh; chi tiết trong `report/HW02_FR-03_section.md` (2.1.3, 2.1.6) và mã nguồn `ForgotPassword.jsx` (dòng 7, 45–97).
- **GitHub Issue:** https://github.com/kiterakun/HW02-Domain_Testing/issues/1
- **Ghi chú/xác minh:** Sai lệch số 3 trong danh sách 4 sai lệch SRS ↔ mã nguồn (mục 2.1.1).

### BUG-FR03-02: Regex mật khẩu khác SRS, từ chối mọi mật khẩu hợp lệ theo SRS

- **Feature:** FR-03
- **Test Case ID:** FR03-DT-03, FR03-BVA-02, FR03-BVA-03
- **Severity:** Critical -- **Priority:** P1
- **Môi trường:** như BUG-FR03-01.
- **Tiền điều kiện:** Đã nhận mã OTP cho `test@eshop.com`.
- **Dữ liệu kiểm thử:** `newPassword = "Abc12345!"` (hợp lệ theo SRS); đối chứng `Abc 1234` / `Abc 12345` (thỏa regex thực tế).
- **Các bước tái hiện:**
  1. Truy cập trang quên mật khẩu, nhập email `test@eshop.com`, bấm "Lấy mã OTP".
  2. Nhập OTP đúng và mật khẩu mới `Abc12345!`.
  3. Bấm "Đặt lại mật khẩu".
- **Kết quả mong đợi:** Đổi mật khẩu thành công, chuyển về trang đăng nhập (SRS FR-03/FR-01).
- **Kết quả thực tế:** Popup "Mật khẩu quá yếu!..."; không có request nào gửi lên `/api/reset-password`; mật khẩu không đổi. Regex thực tế `(?=.*\s)[A-Za-z\d\s]{8,}` bắt buộc khoảng trắng, từ chối ký tự đặc biệt — mật khẩu có khoảng trắng (`Abc 1234`) lại được chấp nhận.
- **Tần suất tái hiện:** 3/3 test chính thức; bộ 3 điểm bổ sung (BVA-01S/02S/03S) xác nhận biên 8 ký tự hoạt động đúng khi tách khỏi sai lệch regex.
- **Ảnh/log minh chứng:** `report/HW02_FR-03_section.md` (2.1.3, 2.1.5, 2.1.7); `ForgotPassword.jsx` dòng 26–30.
- **GitHub Issue:** https://github.com/kiterakun/HW02-Domain_Testing/issues/2
- **Ghi chú/xác minh:** Sai lệch số 4; hệ quả: không thể hoàn thành luồng đổi mật khẩu bằng mật khẩu nào thỏa mãn SRS.

### BUG-FR09-01: Công thức percent sai hoàn toàn -- discount âm, final tăng ~10 lần

- **Feature:** FR-09 (Discount coupons)
- **Test Case ID:** FR09-DT-01, DT-22, DT-23, FR09-BVA-03, BVA-10
- **Severity:** Critical -- **Priority:** P1
- **Môi trường:** backend Node/Express + SQLite `http://localhost:3000` (DB reset + seed mỗi lượt); kiểm thử qua API trực tiếp; 18-10-2026.
- **Tiền điều kiện:** User id=2 đã đăng nhập; SAVE10 chưa dùng.
- **Dữ liệu kiểm thử:** `{"code":"SAVE10","total_amount":500000,"user_id":2}`.
- **Các bước tái hiện:**
  1. Gọi `POST /api/apply-coupon` với body trên.
  2. Đọc `discount_amount` / `final_amount`.
  3. (DT-23) Đi tiếp checkout và kiểm tra đơn trong `GET /api/orders/my-orders`.
- **Kết quả mong đợi:** Giảm 10% của 500.000₫ = 50.000₫; `final_amount = 450.000` (SRS: `discount = total × value / 100`).
- **Kết quả thực tế:** HTTP 200; `discount_amount = −4.500.000`, `final_amount = 5.000.000` (tăng ~10 lần); đơn hàng lưu `total_amount = 5.000.000`.
- **Tần suất tái hiện:** 5/5 (mọi kịch bản percent).
- **Ảnh/log minh chứng:** `report/HW02_FR-09_section.md` (2.2.3, 2.2.5, 2.2.6); `server.js` dòng 398–401, 418–421 (`Math.floor(total_amount * (1 - discount_value))`).
- **GitHub Issue:** https://github.com/kiterakun/HW02-Domain_Testing/issues/3
- **Ghi chú/xác minh:** Về bản chất là tăng giá 10 lần thay vì giảm 10%; lỗi lan vào đơn hàng (DT-23).

### BUG-FR09-02: C3 dùng `>` thay vì `>=` -- đơn đúng bằng ngưỡng bị từ chối

- **Feature:** FR-09
- **Test Case ID:** FR09-DT-09, DT-16, FR09-BVA-02, BVA-05
- **Severity:** High -- **Priority:** P2
- **Môi trường:** như BUG-FR09-01.
- **Tiền điều kiện:** User id=2 đã đăng nhập; coupon chưa dùng.
- **Dữ liệu kiểm thử:** SAVE10 `total = 300.000` (đúng min); BIGBUY `total = 500.000` (đúng min).
- **Các bước tái hiện:**
  1. Gọi `POST /api/apply-coupon` với `code=SAVE10, total_amount=300000, user_id=2`.
  2. Lặp với `code=BIGBUY, total_amount=500000`.
- **Kết quả mong đợi:** Đơn đúng bằng ngưỡng được chấp nhận (SRS C3: `total >= min_order_amount`).
- **Kết quả thực tế:** HTTP 400 "Đơn hàng chưa đủ giá trị tối thiểu 300,000 ₫" (tương tự với 500,000 ₫).
- **Tần suất tái hiện:** 4/4.
- **Ảnh/log minh chứng:** `report/HW02_FR-09_section.md` (2.2.3, 2.2.5); `server.js` dòng 379.
- **GitHub Issue:** https://github.com/kiterakun/HW02-Domain_Testing/issues/4
- **Ghi chú/xác minh:** Biên dưới (min − 1) bị từ chối đúng; lỗi nằm đúng tại điểm biên.

### BUG-FR09-03: Áp coupon không cần JWT (C4 không được cài đặt)

- **Feature:** FR-09
- **Test Case ID:** FR09-DT-12, DT-13
- **Severity:** Critical -- **Priority:** P1
- **Môi trường:** như BUG-FR09-01.
- **Tiền điều kiện:** Không xác thực (hoặc token rác).
- **Dữ liệu kiểm thử:** `{"code":"SAVE10","total_amount":500000}` không header `Authorization`; sau đó với `Authorization: Bearer abc.xyz.123`.
- **Các bước tái hiện:**
  1. Gọi `POST /api/apply-coupon` không gửi header Authorization, không kèm `user_id`.
  2. Gọi lại với token malformed.
- **Kết quả mong đợi:** 401/403 từ chối theo C4 (SRS FR-09 yêu cầu JWT hợp lệ).
- **Kết quả thực tế:** Cả hai lần HTTP 200 `success:true` — mã được áp "thành công"; endpoint nhận `user_id` từ body client.
- **Tần suất tái hiện:** 2/2.
- **Ảnh/log minh chứng:** `report/HW02_FR-09_section.md` (2.2.3, 2.2.6); `server.js` dòng 363–364.
- **GitHub Issue:** https://github.com/kiterakun/HW02-Domain_Testing/issues/5
- **Ghi chú/xác minh:** Kết hợp BUG-FR09-04 tạo thành lỗ hổng áp mã ẩn danh.

### BUG-FR09-04: Bỏ `user_id` trong body bypass được giới hạn lượt dùng (C5)

- **Feature:** FR-09
- **Test Case ID:** FR09-DT-12 + probe
- **Severity:** High -- **Priority:** P2
- **Môi trường:** như BUG-FR09-01.
- **Tiền điều kiện:** User 2 đã dùng kín lượt SAVE10 (count = max = 1).
- **Dữ liệu kiểm thử:** `{"code":"SAVE10","total_amount":500000}` — không kèm `user_id`.
- **Các bước tái hiện:**
  1. Ghi kín lượt dùng SAVE10 cho user 2 (qua checkout FR09-DT-23).
  2. Gọi `POST /api/apply-coupon` không kèm `user_id`.
- **Kết quả mong đợi:** Từ chối "đã đạt giới hạn" kể cả khi thiếu `user_id`.
- **Kết quả thực tế:** HTTP 200 `success:true` — C5 bị bỏ qua; mã áp được không giới hạn.
- **Tần suất tái hiện:** 1/1 probe.
- **Ảnh/log minh chứng:** `report/HW02_FR-09_section.md` (2.2.6); `server.js` dòng 386–395.
- **GitHub Issue:** https://github.com/kiterakun/HW02-Domain_Testing/issues/6
- **Ghi chú/xác minh:** C5 phụ thuộc giá trị client gửi thay vì định danh phía server.

### BUG-FR09-05: `final_amount` cho phép âm

- **Feature:** FR-09
- **Test Case ID:** FR09-DT-21
- **Severity:** Medium -- **Priority:** P3
- **Môi trường:** như BUG-FR09-01; coupon dựng `FIXBIG` (fixed 500.000₫, min 0) qua Admin API.
- **Tiền điều kiện:** Admin token; coupon `FIXBIG` đã tạo.
- **Dữ liệu kiểm thử:** `{"code":"FIXBIG","total_amount":100000,"user_id":2}`.
- **Các bước tái hiện:**
  1. Tạo coupon `FIXBIG` (fixed 500.000₫, min_order_amount 0).
  2. Áp `FIXBIG` cho đơn 100.000₫.
- **Kết quả mong đợi:** Không được tạo final âm: từ chối hoặc clamp `discount ≤ total`, `final ≥ 0`.
- **Kết quả thực tế:** HTTP 200; `discount_amount = 500.000`, `final_amount = −400.000`.
- **Tần suất tái hiện:** 1/1.
- **Ảnh/log minh chứng:** `report/HW02_FR-09_section.md` (2.2.3, 2.2.4); `server.js` dòng 397–406.
- **GitHub Issue:** https://github.com/kiterakun/HW02-Domain_Testing/issues/7
- **Ghi chú/xác minh:** SRS không quy định tường minh nhưng giao dịch tổng âm vô lý về nghiệp vụ.

### BUG-FR09-06: Sai thứ tự ưu tiên thông báo khi vi phạm C2+C3

- **Feature:** FR-09
- **Test Case ID:** FR09-DT-18
- **Severity:** Low -- **Priority:** P4
- **Môi trường:** như BUG-FR09-01.
- **Tiền điều kiện:** Coupon `EXPIRED` (hết hạn 2020-01-01, min 100.000₫).
- **Dữ liệu kiểm thử:** `{"code":"EXPIRED","total_amount":50000,"user_id":2}`.
- **Các bước tái hiện:**
  1. Áp `EXPIRED` với đơn dưới ngưỡng (vi phạm đồng thời C2 và C3).
  2. Đọc thông báo lỗi; đối chiếu DT-19 (chỉ sai hạn).
- **Kết quả mong đợi:** Ưu tiên "Mã giảm giá đã hết hạn" (mã hết hạn không dùng được bất kể giá trị đơn).
- **Kết quả thực tế:** HTTP 400 "chưa đủ giá trị tối thiểu 100,000 ₫" — lỗi ngưỡng che lỗi hết hạn (C2 lồng trong nhánh thỏa C3).
- **Tần suất tái hiện:** 1/1.
- **Ảnh/log minh chứng:** `report/HW02_FR-09_section.md` (2.2.3); `server.js` dòng 379–384.
- **GitHub Issue:** https://github.com/kiterakun/HW02-Domain_Testing/issues/8
- **Ghi chú/xác minh:** SRS không cố định thứ tự ưu tiên lỗi; ghi nhận hành vi gây hiểu nhầm trạng thái mã.

### BUG-FR09-07: Checkout tin hoàn toàn tổng tiền client gửi (ảnh hưởng FR-08)

- **Feature:** FR-09 (ảnh hưởng FR-08)
- **Test Case ID:** probe trong quá trình test FR-09
- **Severity:** Medium -- **Priority:** P3
- **Môi trường:** như BUG-FR09-01.
- **Tiền điều kiện:** JWT hợp lệ user id=2; giỏ hàng sẵn có.
- **Dữ liệu kiểm thử:** `POST /api/checkout` với `total_amount: 1`.
- **Các bước tái hiện:**
  1. Gọi `POST /api/checkout` với JWT hợp lệ và `total_amount = 1`.
  2. Gọi `GET /api/orders/my-orders` kiểm tra đơn vừa tạo.
- **Kết quả mong đợi:** Server tự tính tổng từ giỏ hàng phía server, không tin `total_amount` client.
- **Kết quả thực tế:** Đơn được tạo và lưu `total_amount = 1`.
- **Tần suất tái hiện:** 1/1 probe.
- **Ảnh/log minh chứng:** `report/HW02_FR-09_section.md` (2.2.6, 2.2.7); `server.js` dòng 297–309.
- **GitHub Issue:** https://github.com/kiterakun/HW02-Domain_Testing/issues/9
- **Ghi chú/xác minh:** Lỗi thuộc luồng Checkout (FR-08), phát hiện trong khi test FR-09; chưa sửa.

### BUG-FR14-01: Tạo danh mục chấp nhận tên rỗng và thiếu trường name (NULL)

- **Feature:** FR-14 (Category management CRUD)
- **Test Case ID:** FR14-DT-03, DT-04, FR14-BVA-01
- **Severity:** Critical -- **Priority:** P1
- **Môi trường:** backend Node/Express + SQLite `http://localhost:3000` (re-seed mỗi lượt chạy); API trực tiếp; token admin; 18-10-2026.
- **Tiền điều kiện:** Token admin hợp lệ; CSDL seed 3 danh mục.
- **Dữ liệu kiểm thử:** `{"name": ""}` và `{}` (thiếu trường name).
- **Các bước tái hiện:**
  1. Đăng nhập `admin@eshop.com` lấy JWT.
  2. `POST /api/categories` với `{"name": ""}`; lặp với `{}`.
  3. `GET /api/categories` đối chiếu.
- **Kết quả mong đợi:** HTTP 400 từ chối (SRS FR-14: "Tên danh mục là bắt buộc, không được để trống").
- **Kết quả thực tế:** HTTP 200 "Category created" cho cả hai; danh mục rỗng và bản ghi `name = NULL` đều được lưu.
- **Tần suất tái hiện:** 3/3 (DT-03, DT-04, BVA-01).
- **Ảnh/log minh chứng:** `report/HW02_FR-14_section.md` (2.3.3, 2.3.5); `server.js` dòng 249–251 (không validate).
- **GitHub Issue:** https://github.com/kiterakun/HW02-Domain_Testing/issues/10
- **Ghi chú/xác minh:** Bảng `categories` không có ràng buộc NOT NULL (`database.js` dòng 23–26).

### BUG-FR14-02: Tạo danh mục chấp nhận tên chỉ toàn khoảng trắng

- **Feature:** FR-14
- **Test Case ID:** FR14-DT-05
- **Severity:** Medium -- **Priority:** P3
- **Môi trường:** như BUG-FR14-01.
- **Tiền điều kiện:** Token admin.
- **Dữ liệu kiểm thử:** `{"name": "   "}` (3 ký tự khoảng trắng).
- **Các bước tái hiện:**
  1. `POST /api/categories` với body trên.
  2. `GET /api/categories` đối chiếu.
- **Kết quả mong đợi:** HTTP 400 — tên rỗng sau trim.
- **Kết quả thực tế:** HTTP 200; lưu nguyên 3 ký tự khoảng trắng làm tên danh mục.
- **Tần suất tái hiện:** 1/1.
- **Ảnh/log minh chứng:** `report/HW02_FR-14_section.md` (2.3.3).
- **GitHub Issue:** https://github.com/kiterakun/HW02-Domain_Testing/issues/11
- **Ghi chú/xác minh:** Thu nhóm "không validate name" cùng BUG-FR14-01/03.

### BUG-FR14-03: Không kiểm tra kiểu dữ liệu trường name

- **Feature:** FR-14
- **Test Case ID:** FR14-DT-08
- **Severity:** Medium -- **Priority:** P3
- **Môi trường:** như BUG-FR14-01.
- **Tiền điều kiện:** Token admin.
- **Dữ liệu kiểm thử:** `{"name": 12345}` (number).
- **Các bước tái hiện:**
  1. `POST /api/categories` với name kiểu số.
  2. `GET /api/categories` đối chiếu.
- **Kết quả mong đợi:** HTTP 400 — name phải là chuỗi.
- **Kết quả thực tế:** HTTP 200; `12345` lưu thẳng cột `name TEXT`, GET trả name dạng số.
- **Tần suất tái hiện:** 1/1.
- **Ảnh/log minh chứng:** `report/HW02_FR-14_section.md` (2.3.3).
- **GitHub Issue:** https://github.com/kiterakun/HW02-Domain_Testing/issues/12
- **Ghi chú/xác minh:** Backend chèn thẳng `req.body.name` vào INSERT.

### BUG-FR14-04: POST/PUT/DELETE danh mục không kiểm tra `role='admin'`

- **Feature:** FR-14
- **Test Case ID:** FR14-DT-11, DT-13, DT-17
- **Severity:** Critical -- **Priority:** P1
- **Môi trường:** như BUG-FR14-01; token user thường (`test@eshop.com`, role=user).
- **Tiền điều kiện:** Token user thường hợp lệ.
- **Dữ liệu kiểm thử:** POST `{"name": "User Token Cat"}`; DELETE id=4; PUT id=2 `{"name": "Hacked"}`.
- **Các bước tái hiện:**
  1. Đăng nhập `test@eshop.com` lấy JWT user thường.
  2. Gọi POST/DELETE/PUT `/api/categories/...` với token user.
  3. Đối chiếu HTTP status và dữ liệu.
- **Kết quả mong đợi:** HTTP 403 (FR-12/SEC-03: API ảnh hưởng dữ liệu yêu cầu `role='admin'` trong token).
- **Kết quả thực tế:** Cả 3 thao tác HTTP 200 — user thường tạo/xóa/sửa danh mục thành công; middleware chỉ `jwt.verify`, không đọc role.
- **Tần suất tái hiện:** 3/3 (POST, DELETE, PUT).
- **Ảnh/log minh chứng:** `report/HW02_FR-14_section.md` (2.3.3, 2.3.7); `server.js` dòng 100–110, 249–278.
- **GitHub Issue:** https://github.com/kiterakun/HW02-Domain_Testing/issues/13
- **Ghi chú/xác minh:** Pattern thiếu kiểm tra role lan sang cả PUT (DT-17, ngoài phạm vi SRS -- tham chiếu).

### BUG-FR14-05: Xóa danh mục đang được sản phẩm tham chiếu (dangling reference)

- **Feature:** FR-14
- **Test Case ID:** FR14-DT-15
- **Severity:** High -- **Priority:** P2
- **Môi trường:** như BUG-FR14-01.
- **Tiền điều kiện:** Danh mục id=1 "Điện thoại" đang được 2 sản phẩm tham chiếu (`category_id = 1`).
- **Dữ liệu kiểm thử:** `DELETE /api/categories/1` (token admin).
- **Các bước tái hiện:**
  1. `GET /api/products` xác nhận 2 sản phẩm trỏ `category_id = 1`.
  2. `DELETE /api/categories/1`.
  3. `GET /api/products` kiểm tra lại.
- **Kết quả mong đợi:** Chặn xóa hoặc xử lý tham chiếu — không để sản phẩm trỏ tới danh mục đã xóa.
- **Kết quả thực tế:** HTTP 200 xóa thành công; 2 sản phẩm vẫn giữ `category_id = 1` (dangling reference).
- **Tần suất tái hiện:** 1/1.
- **Ảnh/log minh chứng:** `report/HW02_FR-14_section.md` (2.3.3); `server.js` dòng 269–278; `products.category_id` không có khóa ngoại.
- **GitHub Issue:** https://github.com/kiterakun/HW02-Domain_Testing/issues/14
- **Ghi chú/xác minh:** Ảnh hưởng tính toàn vẹn dữ liệu gián tiếp qua danh mục.

### BUG-FR14-06: DELETE id không tồn tại/ngoài miền trả 200 thay vì 404

- **Feature:** FR-14
- **Test Case ID:** FR14-DT-14, FR14-BVA-04/05/06
- **Severity:** Low -- **Priority:** P4
- **Môi trường:** như BUG-FR14-01 (max id tồn tại tại thời điểm chạy = 14).
- **Dữ liệu kiểm thử:** id 9999, 0, −1, 15.
- **Các bước tái hiện:**
  1. `DELETE /api/categories/9999` (token admin).
  2. Lặp với id `0`, `−1` (dưới biên) và `15` (trên biên).
- **Kết quả mong đợi:** HTTP 404 Not Found khi id không tồn tại/ngoài miền.
- **Kết quả thực tế:** Cả 4 id trả HTTP 200 "Category deleted" dù không xóa được gì.
- **Tần suất tái hiện:** 4/4.
- **Ảnh/log minh chứng:** `report/HW02_FR-14_section.md` (2.3.5); `server.js` dòng 269–278 (không kiểm tra `this.changes`).
- **GitHub Issue:** https://github.com/kiterakun/HW02-Domain_Testing/issues/15
- **Ghi chú/xác minh:** Thông báo "thành công" sai lệch trạng thái thực của dữ liệu.

### BUG-FR07-01: Sửa số lượng trong giỏ luôn cộng thêm 1

- **Feature:** FR-07 (Shopping cart Mobile)
- **Test Case ID:** FR07-DT-10, FR07-BVA-05, BVA-06
- **Severity:** High -- **Priority:** P2
- **Môi trường:** app mobile RN (`frontend-mobile/App.js`); harness logic Node.js v24.10.0 (`shopping-cart-mobile-feature/execution/harness-fr07.js`) + kiểm tra tĩnh render; không emulator; 18-10-2026.
- **Tiền điều kiện:** Giỏ có dòng AirPods Pro 2 (6.000.000₫) SL=1.
- **Dữ liệu kiểm thử:** nhập `cart_quantity` = "1", "2", "5".
- **Các bước tái hiện:**
  1. Thêm AirPods vào giỏ SL=1.
  2. Mở giỏ, nhập "1" vào ô Số lượng.
  3. Lặp với "2" và "5"; quan sát SL/Thành tiền.
- **Kết quả mong đợi:** SL hiển thị đúng giá trị nhập; Thành tiền = Đơn giá × SL (FR-06/FR-07).
- **Kết quả thực tế:** Nhập "1" → SL=2 (12.000.000₫); "2" → SL=3; "5" → SL=6 — lệch +1 mọi lần; `onChangeText` gán `parsed + 1` (`App.js:617–621`).
- **Tần suất tái hiện:** 3/3.
- **Ảnh/log minh chứng:** `report/HW02_FR-07_section.md` (2.4.3, 2.4.5, 2.4.7); harness-fr07.js.
- **GitHub Issue:** https://github.com/kiterakun/HW02-Domain_Testing/issues/16
- **Ghi chú/xác minh:** Lỗi dữ liệu duy nhất của FR-07; biên min−1 ("0") xử lý an toàn về 1.

### BUG-FR07-02: Nhãn tổng tiền "Tổng tạm tính" thay vì "Tổng cộng"

- **Feature:** FR-07
- **Test Case ID:** FR07-DT-13, DT-14, DT-22, FR07-BVA-08, BVA-09, BVA-17, BVA-18
- **Severity:** Medium -- **Priority:** P3
- **Môi trường:** như BUG-FR07-01.
- **Tiền điều kiện:** Giỏ có sản phẩm.
- **Dữ liệu kiểm thử:** 1–3 dòng giỏ.
- **Các bước tái hiện:**
  1. Thêm sản phẩm vào giỏ.
  2. Mở giỏ, đọc nhãn dòng tổng tiền.
- **Kết quả mong đợi:** Nhãn "Tổng cộng" theo FR-07.
- **Kết quả thực tế:** "Tổng tạm tính: …" (`App.js:638`); chuỗi "Tổng cộng" không tồn tại trong `App.js` (riêng giá trị tổng đúng).
- **Tần suất tái hiện:** 7/7.
- **Ảnh/log minh chứng:** `report/HW02_FR-07_section.md` (2.4.3, 2.4.5).
- **GitHub Issue:** https://github.com/kiterakun/HW02-Domain_Testing/issues/17
- **Ghi chú/xác minh:** Vi phạm điều khoản tường minh của FR-07.

### BUG-FR07-03: Empty state giỏ trống thiếu hình minh họa

- **Feature:** FR-07
- **Test Case ID:** FR07-DT-12, DT-16, FR07-BVA-07, BVA-16
- **Severity:** Medium -- **Priority:** P3
- **Môi trường:** như BUG-FR07-01.
- **Tiền điều kiện:** Giỏ trống (mới mở app hoặc sau khi xóa dòng cuối).
- **Dữ liệu kiểm thử:** không cần.
- **Các bước tái hiện:**
  1. Bấm "Giỏ" khi giỏ trống.
  2. Quan sát khối empty state.
- **Kết quả mong đợi:** Empty state có hình minh họa/icon + thông báo + "Tiếp tục mua sắm" (FR-07/FR-24).
- **Kết quả thực tế:** Chỉ Text "Giỏ hàng của bạn đang trống" + link; không có `<Image>` (`App.js:594–600`).
- **Tần suất tái hiện:** 4/4.
- **Ảnh/log minh chứng:** `report/HW02_FR-07_section.md` (2.4.3, 2.4.5).
- **GitHub Issue:** https://github.com/kiterakun/HW02-Domain_Testing/issues/18
- **Ghi chú/xác minh:** Cùng nhóm UI/UX bị FR-24 tường minh hóa.

### BUG-FR07-04: Xóa sản phẩm không có dialog xác nhận

- **Feature:** FR-07
- **Test Case ID:** FR07-DT-12, DT-21, FR07-BVA-16, BVA-17, BVA-18
- **Severity:** High -- **Priority:** P2
- **Môi trường:** như BUG-FR07-01.
- **Tiền điều kiện:** Giỏ có 1–3 dòng.
- **Dữ liệu kiểm thử:** bấm "Xóa" trên một dòng bất kỳ.
- **Các bước tái hiện:**
  1. Mở giỏ, bấm "Xóa" trên một dòng.
  2. Quan sát có/không hộp thoại xác nhận trước khi dòng biến mất.
- **Kết quả mong đợi:** Dialog xác nhận trước khi xóa (FR-07 + FR-24).
- **Kết quả thực tế:** Xóa ngay không dialog: `onPress={() => removeFromCart(index)}` (`App.js:630`); `removeFromCart` không có Alert/confirm.
- **Tần suất tái hiện:** 5/5.
- **Ảnh/log minh chứng:** `report/HW02_FR-07_section.md` (2.4.3, 2.4.5, 2.4.7).
- **GitHub Issue:** https://github.com/kiterakun/HW02-Domain_Testing/issues/19
- **Ghi chú/xác minh:** Thao tác hủy dữ liệu không thể hoàn tác trên cảm ứng — rủi ro trải nghiệm cao.

### BUG-FR07-05: Cột Số lượng không có nút +/−

- **Feature:** FR-07
- **Test Case ID:** FR07-DT-14, DT-20
- **Severity:** Medium -- **Priority:** P3
- **Môi trường:** như BUG-FR07-01.
- **Tiền điều kiện:** Giỏ có sản phẩm.
- **Dữ liệu kiểm thử:** quan sát cột Số lượng.
- **Các bước tái hiện:**
  1. Thêm sản phẩm vào giỏ, mở giỏ.
  2. Tìm nút +/− trong cột Số lượng.
- **Kết quả mong đợi:** Có nút +/− để chỉnh số lượng (FR-07).
- **Kết quả thực tế:** Chỉ có TextInput (`App.js:609–624`); không có nút +/− nào (kèm BUG-FR07-01 khi nhập tay).
- **Tần suất tái hiện:** 2/2.
- **Ảnh/log minh chứng:** `report/HW02_FR-07_section.md` (2.4.3, 2.4.6).
- **GitHub Issue:** https://github.com/kiterakun/HW02-Domain_Testing/issues/20
- **Ghi chú/xác minh:** Vi phạm điều khoản tường minh của FR-07.

### BUG-FR07-06: Badge "Giỏ (n)" đếm số dòng thay vì tổng số lượng; FR-23 mơ hồ

- **Feature:** FR-07
- **Test Case ID:** FR07-DT-19, FR07-BVA-13/14/15
- **Severity:** Low -- **Priority:** P4
- **Môi trường:** như BUG-FR07-01.
- **Tiền điều kiện:** Giỏ trống ban đầu.
- **Dữ liệu kiểm thử:** chuỗi thao tác: thêm A, thêm B, cộng dồn A, xóa 1 dòng.
- **Các bước tái hiện:**
  1. Thêm A, thêm B (badge "Giỏ (2)").
  2. Thêm lại A (cộng dồn, tổng SL = 3, 2 dòng).
  3. Quan sát badge.
- **Kết quả mong đợi:** Badge phản ánh "số lượng sản phẩm" theo FR-23 (cần làm rõ: đếm dòng hay tổng SL).
- **Kết quả thực tế:** Badge dùng `cart.length` (`App.js:436`) — sau cộng dồn vẫn "Giỏ (2)".
- **Tần suất tái hiện:** 4/4.
- **Ảnh/log minh chứng:** `report/HW02_FR-07_section.md` (2.4.3, 2.4.5, 2.4.7).
- **GitHub Issue:** https://github.com/kiterakun/HW02-Domain_Testing/issues/21
- **Ghi chú/xác minh:** Chưa chốt vi phạm tuyệt đối do FR-23 mơ hồ; cần làm rõ đặc tả trước khi sửa.

### BUG-FR07-07: Số thực nhập vào số lượng bị cắt cụt âm thầm

- **Feature:** FR-07
- **Test Case ID:** FR07-DT-09
- **Severity:** Low -- **Priority:** P4
- **Môi trường:** như BUG-FR07-01.
- **Tiền điều kiện:** Giỏ có hàng; đang ở chi tiết sản phẩm.
- **Dữ liệu kiểm thử:** `add_quantity = "3.5"`.
- **Các bước tái hiện:**
  1. Nhập "3.5" vào ô số lượng, bấm "Thêm vào giỏ hàng".
  2. Mở giỏ, quan sát số lượng.
- **Kết quả mong đợi:** Chặn hoặc báo lỗi khi nhập số thực (FR-06: chỉ nhận số nguyên dương).
- **Kết quả thực tế:** `parseInt` cắt cụt → thêm SL=3, không thông báo; an toàn nhưng thay đổi ý định người dùng.
- **Tần suất tái hiện:** 1/1.
- **Ảnh/log minh chứng:** `report/HW02_FR-07_section.md` (2.4.3); `App.js:129–132` (`normalizeQuantity`).
- **GitHub Issue:** https://github.com/kiterakun/HW02-Domain_Testing/issues/22
- **Ghi chú/xác minh:** Test chính thức Pass (xử lý an toàn); lộ sót đáng chú ý ghi thành bug mức Low.

### BUG-FR07-08: Checkout gửi `cart.slice(0, -1)` -- mất dòng cuối của giỏ (ảnh hưởng FR-08)

- **Feature:** FR-07 (ảnh hưởng FR-08)
- **Test Case ID:** phát hiện qua rà soát mã nguồn trong quá trình test FR-07
- **Severity:** High -- **Priority:** P2
- **Môi trường:** như BUG-FR07-01 (không có emulator để tái hiện end-to-end).
- **Tiền điều kiện:** Giỏ có ≥ 2 dòng.
- **Dữ liệu kiểm thử:** `openCheckout` (`App.js:342–354`).
- **Các bước tái hiện:**
  1. Thêm 2+ dòng vào giỏ, bấm Checkout.
  2. Đối chiếu số dòng giỏ với payload tạo đơn tại `App.js:391`.
- **Kết quả mong đợi:** Toàn bộ dòng giỏ được đưa vào đơn hàng.
- **Kết quả thực tế (từ mã nguồn):** Payload gửi `cart.slice(0, -1)` — dòng cuối bị loại khỏi đơn khi giỏ > 1 dòng.
- **Tần suất tái hiện:** xác nhận qua kiểm tra tĩnh mã nguồn; cần tái hiện trên thiết bị/emulator.
- **Ảnh/log minh chứng:** `report/HW02_FR-07_section.md` (2.4.6, 2.4.7); `App.js:391`.
- **GitHub Issue:** https://github.com/kiterakun/HW02-Domain_Testing/issues/23
- **Ghi chú/xác minh:** Lỗi thuộc Checkout (FR-08); kết hợp BUG-FR09-07 làm luồng tiền sai lệch cả hai phía.

## 3.4. Tổng kết lỗi

- **Tổng số lỗi đã ghi nhận và tạo GitHub Issue: 23** (tất cả ở trạng thái **Open**; chưa có lỗi nào được sửa/đóng).
- **Theo feature:** FR-03: 2 | FR-09: 7 (trong đó BUG-FR09-07 thuộc FR-08) | FR-14: 6 | FR-07: 8 (trong đó BUG-FR07-08 thuộc FR-08).
- **Theo severity:** Critical: 5 (BUG-FR03-02, BUG-FR09-01, BUG-FR09-03, BUG-FR14-01, BUG-FR14-04) | High: 6 (BUG-FR09-02, BUG-FR09-04, BUG-FR14-05, BUG-FR07-01, BUG-FR07-04, BUG-FR07-08) | Medium: 8 | Low: 4.
- **Theo priority:** P1: 5 | P2: 6 | P3: 8 | P4: 4.
- **Nhóm rủi ro lớn nhất:** (1) sai quy tắc/tính toán tiền và giảm giá — BUG-FR09-01/02/03/04/05, BUG-FR09-07, BUG-FR07-01/08; (2) vi phạm kiểm soát truy cập — BUG-FR14-04; (3) mất ràng buộc dữ liệu đầu vào — BUG-FR14-01/02/03.
- **Ghi nhận ngoài issue:** FR-03 xác nhận thêm 2 sai lệch SRS (OTP 4 chữ số, thiếu trường xác nhận mật khẩu) không cấu thành Fail test case; FR-07 ghi nhận 2 điểm nhỏ phụ thuộc locale định dạng tiền và nhãn "← Mua tiếp" (đúng chức năng, khác nhãn SRS).

---

# 6. Test Summary Report

## 6.1. Tóm tắt phạm vi

- **SUT:** EShop (demo thương mại điện tử: backend Node/Express + SQLite, frontend-web React + Vite, frontend-admin React, mobile React Native). Repository SUT: `ttbhanh/eshop-sut`.
- **Số feature được lựa chọn:** 4 / 4 pool (A, B, C, D).
- **Feature:** FR-03 — Forgot password and password reset (two steps); FR-09 — Discount coupons; FR-14 — Category management (CRUD: Thêm/Xem/Xóa); FR-07 — Shopping cart (Mobile).
- **Kỹ thuật:** Domain Testing (phân hoạch lớp tương đương) và Boundary Value Analysis (3 điểm: dưới/tại/trên biên).
- **Khoảng thời gian kiểm thử:** 18-10-2026 (đếm các đợt thực thi, mỗi feature hoàn trong một đợt; DB re-seed sau mỗi đợt).
- **Môi trường:** backend + SQLite tại `http://localhost:3000` (seed sạch); frontend-web `http://localhost:5173` (Edge Chromium headless); mobile qua harness logic Node.js v24.10.0 + kiểm tra tĩnh render; không có emulator Android/iOS, không kiểm thử email/token thật.
- **Quản lý lỗi:** GitHub Issues `kiterakun/HW02-Domain_Testing` — issue #1 → #23, labels `type: bug`, `status: new`.

## 6.2. Thống kê test case

| Chỉ số                                | FR-03 | FR-09 | FR-14 | FR-07 | **Tổng** |
| ------------------------------------- | ----: | ----: | ----: | ----: | -------: |
| Test case thiết kế -- Domain Testing  | 5     | 24    | 18    | 23    | **70**   |
| Test case thiết kế -- BVA             | 3     | 11    | 7     | 18    | **39**   |
| **Tổng test case thiết kế**           | **8** | **35**| **25**| **41**| **109**  |
| Đã thực thi                           | 8     | 35    | 25    | 41    | **109**  |
| Pass                                  | 4     | 23    | 12    | 25    | **64**   |
| Fail                                  | 4     | 12    | 13    | 16    | **45**   |
| Blocked                               | 0     | 0     | 0     | 0     | **0**    |
| Not Executed                          | 0     | 0     | 0     | 0     | **0**    |

> Kiểm tra tính nhất quán: Đã thực thi = Pass + Fail = 64 + 45 = 109 = tổng test case thiết kế; không có Blocked/Not Executed.

Ghi chú số liệu (không cộng trùng):

- FR-03: ngoài 8 test chính thức, chạy thêm 3 test BVA bổ sung (BVA-01S/02S/03S cô lập regex, đều Pass) và 9 probe/kiểm chứng bổ sung — không tính vào số liệu chính thức.
- FR-09: 11 điểm BVA gộp trong 4 nhóm biên (BVA-001…004); 2 probe (bypass C5, checkout total=1) không tính số liệu.
- FR-14: 18 DT bao gồm 2 test PUT "tham chiếu ngoài SRS" (DT-16 Pass tham chiếu); BVA-08 (thăm dò 10.000 ký tự) là Informational, không tính.
- FR-07: 18 điểm BVA trên 6 ranh giới (mỗi ranh giới ≥ 3 điểm).

## 6.3. Thống kê lỗi

| Chỉ số                 | Số lượng |
| ---------------------- | -------: |
| Tổng lỗi đã ghi nhận   | 23       |
| Lỗi đã xác nhận        | 23       |
| Critical               | 5        |
| High                   | 6        |
| Medium                 | 8        |
| Low                    | 4        |
| Open                   | 23       |
| Fixed/Closed           | 0        |

Phân bổ theo feature: FR-03: 2 | FR-09: 7 | FR-14: 6 | FR-07: 8. Chi tiết tại Chương 3.

## 6.4. Đánh giá mức độ hoàn thành

- **Mức độ bao phủ Domain Testing:** FR-03 phủ 5 lớp chính + 4 lớp bổ sung kiểm chứng bằng probe; FR-09 phủ 22/25 lớp (88%, 3 lớp không kiểm được có căn cứ: hết hạn đúng hôm nay, count > max, percent 20–99%); FR-14 phủ 22/22 lớp; FR-07 phủ đủ các lớp đã thiết kế trên 5 biến chính (~96%, phần còn lại N/A có căn cứ).
- **Mức độ bao phủ BVA:** FR-03 3/3 điểm biên 8 ký tự (cộng 3 điểm cô lập regex); FR-09 5/6 nhóm biên (biên ngưỡng EXPIRED bị C2 che); FR-14 đủ 3 điểm biên tên + 4 điểm biên id; FR-07 6/6 ranh giới, mỗi ranh giới ≥ 3 điểm; 2 ranh giới không phủ có lý do (SRS không định nghĩa max; không tạo được giá trị sát mốc 1.000₫ qua UI).
- **Tỷ lệ thực thi:** 109/109 = **100%** (không có Blocked/Not Executed).
- **Kết quả quan trọng:** 45 Fail được xác nhận bằng thực thi (API/trình duyệt/harness logic) và đối chiếu mã nguồn; 100% khớp dự đoán từ rà soát mã nguồn trước khi chạy (FR-03: không có Fail ngoài dự đoán; FR-09 và FR-07: 6/6 điểm nghi ngờ đều thành hiện thực). 5 lỗi Critical liên quan tiền, bảo mật áp coupon và kiểm soát truy cập admin.
- **Yêu cầu chưa kiểm thử:** gửi email OTP thật và thời hạn token (SUT không cài đặt); FR-14 Update trên UI (UI không có chức năng — chỉ kiểm PUT ở mức API tham chiếu); các khía cạnh chỉ đo được trên thiết bị thật của FR-07 (bố cục, bàn phím số, cảm ứng); FR-08 Checkout chưa được kiểm thử trực tiếp như một feature riêng (2 lỗi liên quan được phát hiện qua FR-09/FR-07).

## 6.5. Hạn chế và rủi ro còn lại

1. **Môi trường mobile:** FR-07 thực thi bằng harness logic + kiểm tra tĩnh thay vì thiết bị thật — các khía cạnh UI chạy thật chưa được kiểm chứng; BUG-FR07-08 cần tái hiện trên emulator/thiết bị.
2. **OTP/email demo:** SUT hiển thị OTP trực tiếp trên màn hình, không kiểm thử được việc gửi email thật; token không có thời hạn nên không kiểm thử lớp "OTP hết hạn".
3. **Dữ liệu/lượt dùng phi persistent:** DB re-seed sau mỗi đợt chạy — số liệu đếm lượt coupon chỉ có ý nghĩa trong một phiên; biên "hết hạn đúng hôm nay" và "count > max" không dựng được qua hệ thống thực.
4. **SRS thiếu biên/mơ hồ:** độ dài tên danh mục không có biên trên (chỉ thăm dò); số lượng giỏ không có max (chỉ exploratory 9999); FR-23 "số lượng sản phẩm" mơ hồ khiến BUG-FR07-06 chưa chốt được.
5. **Rủi ro nghiệp vụ ngoài SRS nhưng đã ghi nhận:** bỏ qua `is_active` vẫn từ chối đúng qua C1; coupon_id không lưu trong đơn → không truy vết doanh thu giảm giá; `/api/coupon-usage` phụ thuộc frontend tự gọi — client khác ý muốn có thể bỏ qua để "không bao giờ hết lượt"; locale ảnh hưởng định dạng tiền ("30,000,000 ₫" trên en-US).
6. **Khôi phục dữ liệu:** mật khẩu của user test và cây danh mục quanh các đợt chạy đã được re-seed về trạng thái gốc; đã xác nhận sau từng đợt.

## 6.6. Kết luận và đề xuất

- **Kết luận:** Hai kỹ thuật Domain Testing và BVA được triển khai trọn vẹn trên cả 4 feature: 109/109 test case thiết kế đều được thực thi (100%), phát hiện và xác nhận 23 lỗi có GitHub Issue (#1–#23), trong đó 5 Critical/6 High tập trung ở tính toán giảm giá, kiểm soát truy cập và ràng buộc dữ liệu. BVA chứng minh giá trị riêng ở FR-09 (bắt lỗi đúng tại điểm biên `>=`) và FR-03 (cô lập nguyên nhân regex khỏi biên độ dài).
- **Đóng góp của AI/Agent Skills:** AI đề xuất bộ test case bám sát SRS, phân lớp rõ ràng và bộ giá trị 3 điểm dày đặc; các điểm nghi ngờ từ rà soát AI về mã nguồn đều được người kiểm thử kiểm chứng và 100% xác nhận qua thực thi (FR-03: 4/4 sai lệch; FR-09: 6/6; FR-07: 6/6). AI cũng bỏ sót một số lớp/lặp sai bao phủ — đã được người kiểm thử bổ sung và ghi nhận trong mục "Đánh giá/AI Gap" của từng section.
- **Đề xuất tiếp theo:**
  1. Ưu tiên sửa nhóm Critical: BUG-FR09-01/03 (công thức percent, JWT), BUG-FR14-04 (role admin), BUG-FR03-02 (regex mật khẩu), BUG-FR14-01 (validate name).
  2. Kiểm thử trực tiếp FR-08 Checkout (đã phát hiện 2 lỗi láng giềng BUG-FR09-07, BUG-FR07-08) theo cùng phương pháp.
  3. Bổ sung kiểm thử trên thiết bị/emulator cho FR-07 và tái hiện end-to-end BUG-FR07-08.
  4. Làm rõ đặc tả mơ hồ với đội sản phẩm: FR-23 (định nghĩa badge), giá trị biên chưa xác minh (độ dài tên danh mục, max số lượng giỏ, thứ tự ưu tiên lỗi coupon).
  5. Thiết lập kiểm thử hồi quy cho các đợt re-seed/re-run sau khi sửa lỗi.

---

*File tạo tự động từ 4 section: `HW02_FR-03_section.md`, `HW02_FR-09_section.md`, `HW02_FR-14_section.md`, `HW02_FR-07_section.md` — ngày 18-10-2026.*
