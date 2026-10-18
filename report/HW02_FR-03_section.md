# FR-03 -- Forgot password and password reset (nội dung đã điền cho mục 2.1)

> **Cách dùng:** copy toàn bộ phần dưới đây (từ `## 2.1.` đến hết mục 2.1.7) thay thế cho mục 2.1 tương ứng trong `HW02_Domain_Testing_Report.md`.
>
> **Nguồn nội dung:** tài liệu trong `forgot-reset-password-feature/` (feature-overview.md, domain-testing/, bva/, ai-mistake.md), đã đối chiếu với mã nguồn SUT: `eshop-sut/backend/server.js` (dòng 68--98), `eshop-sut/frontend-web/src/pages/ForgotPassword.jsx`, `eshop-sut/backend/database.js` (bảng `users`, dòng 50--63, dữ liệu mẫu dòng 92--93).
>
> **Lưu ý:** các cột "Kết quả thực tế", "Trạng thái", "Bằng chứng" giữ nguyên chỗ trống theo đúng quy tắc ở đầu báo cáo -- không điền trước kết quả kiểm thử khi chưa thực thi.

---

## 2.1. FR-03 -- Forgot password and password reset

### 2.1.1. Mô tả chức năng và yêu cầu

- **Mục đích chức năng:** Cho phép người dùng khôi phục tài khoản khi quên mật khẩu qua hai bước: (1) nhập email đã đăng ký để nhận mã OTP -- trong môi trường demo, mã OTP hiển thị trực tiếp trên màn hình thay vì gửi email; (2) nhập OTP và mật khẩu mới để đặt lại mật khẩu.
- **Điểm bắt đầu/kết thúc:** Bắt đầu tại trang "Quên Mật Khẩu" (`/forgot-password`, frontend-web). Kết thúc: thông báo "Đổi mật khẩu thành công!" và chuyển hướng về trang đăng nhập (`/login`); hoặc kết thúc sớm bằng thông báo lỗi tương ứng.
- **Tiền điều kiện:** Bước 1 -- đã có tài khoản đăng ký với email tồn tại trong CSDL (dữ liệu mẫu: `test@eshop.com`, `admin@eshop.com`). Bước 2 -- đã nhận mã OTP hợp lệ cho email vừa yêu cầu.
- **Luồng nghiệp vụ:**
  - Flow 1 (thành công): Nhập email hợp lệ → nhận mã OTP và chuyển sang bước 2 → nhập OTP đúng + mật khẩu mới hợp lệ → "Đổi mật khẩu thành công!" → chuyển về trang đăng nhập.
  - Flow 2 (lỗi bước 1): Email không tồn tại / sai định dạng → lỗi "User not found" (HTTP 404).
  - Flow 3 (lỗi bước 2): OTP sai → lỗi "Mã OTP không đúng hoặc có lỗi xảy ra." (backend trả "Invalid token or email", HTTP 400); mật khẩu yếu → cảnh báo phía client "Mật khẩu quá yếu!" và từ chối submit.
- **Nguồn yêu cầu:**
  - SRS FR-03 (Version 2.0): `forgot-reset-password-feature/feature-overview.md`.
  - API backend: `eshop-sut/backend/server.js` -- `POST /api/forgot-password` (dòng 68--85), `POST /api/reset-password` (dòng 87--98).
  - Giao diện: `eshop-sut/frontend-web/src/pages/ForgotPassword.jsx`.
- **Giả định cần xác minh** (chưa xem là yêu cầu chính thức; đã đối chiếu mã nguồn):
  1. SRS yêu cầu OTP 6 chữ số, nhưng mã nguồn sinh 4 chữ số (1000--9999, `server.js` dòng 72) và nhãn UI ghi "Mã OTP (4 số)".
  2. SRS yêu cầu trường "Xác nhận mật khẩu mới", nhưng UI thực tế không có trường này.
  3. SRS yêu cầu chỉ báo bước "Bước 1 / 2" và nút "Quay lại đăng nhập", nhưng UI không hiển thị chỉ báo bước; bước 2 có nút "← Quay lại" chỉ quay về bước 1, bước 1 không có nút về trang đăng nhập.
  4. SRS cho phép mật khẩu có "ký tự đặc biệt/khoảng trắng", nhưng regex thực tế bắt buộc khoảng trắng (`(?=.*\s)`) và không chấp nhận ký tự đặc biệt (chỉ `[A-Za-z\d\s]`).
  5. SRS không nêu thời hạn OTP; mã nguồn không cài đặt thời hạn cho `reset_token` (lưu đến khi được dùng hoặc cấp lại).
  6. Token chỉ dùng một lần: `reset_token` bị xóa (`NULL`) sau khi reset thành công -- đã thấy trong mã nguồn (`server.js` dòng 90), cần xác nhận thêm qua thực thi.

### 2.1.2. Domain Testing -- phân tích miền

| Biến/điều kiện | Miền/lớp hợp lệ | Miền/lớp không hợp lệ | Căn cứ | Cần xác minh? |
| --- | --- | --- | --- | --- |
| Email/định danh | Email đúng định dạng và đã đăng ký trong CSDL (VD: `test@eshop.com`) | (a) Đúng định dạng nhưng chưa đăng ký (VD: `notfound@eshop.com`); (b) sai định dạng (VD: `abc`, thiếu `@`). Lưu ý: ô nhập là `type="text"` nên trình duyệt không kiểm tra định dạng; mọi chuỗi không khớp CSDL đều trả về "User not found" (HTTP 404) -- lớp (b) thực tế hành xử giống lớp (a) | SRS FR-03 (`feature-overview.md`); `server.js` dòng 68--71 (tra cứu chính xác email, trả 404); `ForgotPassword.jsx` dòng 50--55 | Có (hành vi lớp sai định dạng khi thực thi) |
| Trạng thái tài khoản | Tài khoản tồn tại trong bảng `users` | Tài khoản không tồn tại (không có bản ghi) | `server.js` dòng 70--71; `database.js` dòng 50--63 (bảng `users`), dòng 92--93 (dữ liệu mẫu) | Không (đã xác minh qua mã nguồn; sẽ đối chiếu khi thực thi) |
| Mã xác minh/token (resetToken) | OTP trùng khớp `reset_token` đang lưu của email đã yêu cầu; theo mã nguồn gồm 4 chữ số (VD: `1234`), theo SRS là 6 chữ số | OTP sai (VD: `9999`); OTP rỗng; token đã bị xóa (`NULL`) sau lần reset thành công trước đó | SRS FR-03; `server.js` dòng 72 (sinh token 4 chữ số), dòng 87--94 (đối chiếu email + token); `ForgotPassword.jsx` dòng 69--77 | Có (độ dài OTP khác SRS; token dùng một lần) |
| Mật khẩu mới | Theo SRS: tối thiểu 8 ký tự, có chữ hoa, chữ thường, số và ký tự đặc biệt/khoảng trắng (VD: `Abc12345!`) | Dưới 8 ký tự (VD: `Ab1!`); rỗng; đủ dài nhưng thiếu độ phức tạp. Lưu ý: regex thực tế `/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*\s)[A-Za-z\d\s]{8,}$/` bắt buộc khoảng trắng, không nhận ký tự đặc biệt -- mâu thuẫn SRS | SRS FR-03 (quy tắc mật khẩu theo FR-01); `ForgotPassword.jsx` dòng 26--30 | Có (regex thực tế khác SRS) |
| Xác nhận mật khẩu (confirmPassword) | Trùng khớp hoàn toàn mật khẩu mới | Không khớp mật khẩu mới | SRS FR-03 (`feature-overview.md`) | Có -- UI thực tế không có trường xác nhận (`ForgotPassword.jsx`) nên lớp này hiện không thể kiểm thử qua giao diện; sai lệch so với SRS |
| Trạng thái luồng hai bước | Bước 1: nhập email → nhận OTP; bước 2: nhập OTP + mật khẩu mới; có nút "← Quay lại" về bước 1 | Thao tác ở bước 2 khi chưa có OTP hợp lệ (yêu cầu reset bị từ chối vì email + token không khớp CSDL) | `ForgotPassword.jsx` (state `step` 1/2 -- dòng 7, 45--97); `server.js` dòng 87--94 | Có (SRS yêu cầu chỉ báo "Bước 1 / 2" và nút quay lại đăng nhập -- UI chưa có) |

**Giải thích phân hoạch miền:** Miền được phân theo từng biến đầu vào của hai bước; mỗi biến tách thành lớp hợp lệ/không hợp lệ vì chúng tạo ra kết quả đầu ra khác nhau: thông báo thành công và chuyển trang ("Đổi mật khẩu thành công!") so với các thông báo lỗi riêng ("User not found", "Mã OTP không đúng hoặc có lỗi xảy ra.", "Mật khẩu quá yếu!"). Giá trị đại diện ưu tiên happy path (email hợp lệ → OTP đúng → mật khẩu hợp lệ) và các lớp lỗi có thông báo riêng để đối chiếu trực tiếp khi thực thi. Các sai lệch SRS ↔ mã nguồn (OTP 4 chữ số thay vì 6; thiếu trường xác nhận mật khẩu; thiếu chỉ báo bước; regex mật khẩu bắt buộc khoảng trắng) được đánh dấu "Cần xác minh" và sẽ được xác nhận bằng thực thi. Lớp "email sai định dạng" về hành vi bị gộp chung với lớp "email không tồn tại" (đều trả về 404 "User not found") nên khi thực thi có thể dùng chung một giá trị đại diện để xác nhận hành vi gộp này.

### 2.1.3. Test case Domain Testing

| Test Case ID | Lớp miền/điều kiện | Tiền điều kiện | Dữ liệu đầu vào | Các bước thực hiện | Kết quả mong đợi | Kết quả thực tế | Trạng thái | Bằng chứng |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| FR03-DT-01 | Email hợp lệ (đã đăng ký) | User đã đăng ký tài khoản | `email = "test@eshop.com"` | 1. Truy cập trang Quên mật khẩu<br>2. Nhập email hợp lệ<br>3. Bấm "Lấy mã OTP" | Hiển thị thông báo chứa mã OTP ("Mã OTP của bạn là: ..."), giao diện chuyển sang bước nhập OTP & mật khẩu (SRS: kèm chỉ báo bước "Bước 1 / 2") | [Sau khi chạy] | Chưa chạy | [Link/đường dẫn] |
| FR03-DT-02 | Email không hợp lệ (chưa đăng ký) | Không có | `email = "notfound@eshop.com"` | 1. Truy cập trang Quên mật khẩu<br>2. Nhập email chưa đăng ký<br>3. Bấm "Lấy mã OTP" | Hiển thị lỗi "User not found" (HTTP 404) | [Sau khi chạy] | Chưa chạy | [Link/đường dẫn] |
| FR03-DT-03 | Happy path bước 2 (OTP đúng, mật khẩu hợp lệ, xác nhận khớp) | Đã nhận mã OTP cho email đã đăng ký (VD: `1234`) | `email = "test@eshop.com"`, `resetToken = "1234"`, `newPassword = "Abc12345!"`, `confirmPassword = "Abc12345!"` (xem ghi chú 1) | 1. Nhập mã OTP chính xác<br>2. Nhập mật khẩu mới hợp lệ (và xác nhận khớp nếu có trường)<br>3. Bấm "Đặt lại mật khẩu" | Thông báo "Đổi mật khẩu thành công!", chuyển hướng về trang đăng nhập; đăng nhập được bằng mật khẩu mới | [Sau khi chạy] | Chưa chạy | [Link/đường dẫn] |
| FR03-DT-04 | OTP không hợp lệ (sai OTP) | Đã nhận mã OTP | `resetToken = "9999"` (sai OTP), email hợp lệ, mật khẩu hợp lệ | 1. Nhập mã OTP sai<br>2. Nhập mật khẩu hợp lệ<br>3. Bấm "Đặt lại mật khẩu" | Hiển thị lỗi "Mã OTP không đúng hoặc có lỗi xảy ra." (backend trả 400 "Invalid token or email") | [Sau khi chạy] | Chưa chạy | [Link/đường dẫn] |
| FR03-DT-05 | Mật khẩu mới không hợp lệ (quá ngắn) | Đã nhận mã OTP | `newPassword = "Ab1!"` (4 ký tự, dưới 8) | 1. Nhập OTP đúng<br>2. Nhập mật khẩu quá ngắn<br>3. Bấm "Đặt lại mật khẩu" | Hiển thị cảnh báo "Mật khẩu quá yếu!...", từ chối submit (không gửi yêu cầu lên server) | [Sau khi chạy] | Chưa chạy | [Link/đường dẫn] |

Ghi chú:

1.  Test case giữ dữ liệu `confirmPassword` theo thiết kế dựa trên SRS; tuy nhiên UI thực tế không có trường này (đã xác minh trong `ForgotPassword.jsx`) -- khi thực thi bỏ qua dữ liệu xác nhận và ghi nhận sai lệch (xem 2.1.7). Ngoài ra, `Abc12345!` thỏa mãn SRS nhưng không thỏa mãn regex thực tế (thiếu khoảng trắng, chứa ký tự đặc biệt) nên dự kiến bị chặn khi thực thi (xem 2.1.4).

**Đánh giá Domain Testing:** 5 test case thiết kế phủ được các lớp: email hợp lệ, email chưa đăng ký, happy path bước 2 (OTP đúng + mật khẩu hợp lệ), OTP sai, mật khẩu quá ngắn. Sau khi rà soát, coverage report của AI (khai báo email 3/3 lớp, resetToken 3/3 lớp, newPassword 4/5 lớp) cao hơn thực tế -- có 4 lớp được khai báo nhưng chưa có test case riêng: (1) email sai định dạng; (2) OTP rỗng; (3) mật khẩu rỗng; (4) mật khẩu đủ 8 ký tự nhưng thiếu độ phức tạp. Đề xuất bổ sung trước khi thực thi: `email = "abc"` (thiếu `@`, dự kiến 404 "User not found" do không có kiểm tra định dạng phía client), `resetToken = ""` (dự kiến trình duyệt chặn submit do `required`), `newPassword = ""` (chặn submit do `required`), `newPassword = "abcdefgh"` (dự kiến cảnh báo mật khẩu yếu). Lớp "xác nhận mật khẩu không khớp" không thể kiểm thử qua UI do thiếu trường (sai lệch SRS, xem 2.1.7).

### 2.1.4. BVA -- phân tích biên

| Biến/điều kiện | Biên dưới | Biên trên | Căn cứ xác định biên | Giá trị cần thử | Ghi chú |
| --- | --- | --- | --- | --- | --- |
| Email/định danh | Không có | Không có | SRS FR-03 không quy định giới hạn độ dài/định dạng; `server.js` chỉ tra cứu chính xác chuỗi email; ô nhập `type="text"` không kiểm tra | Không áp dụng | Không có biên nào được xác minh cho email |
| Mật khẩu mới | 8 ký tự (độ dài tối thiểu) | Không có (không ràng buộc độ dài tối đa) | SRS FR-03/FR-01 (tối thiểu 8 ký tự); regex `{8,}` trong `ForgotPassword.jsx` dòng 26 | Min-1: 7 ký tự (`Abc12!`); Min: 8 ký tự (`Abc1234!`); Min+1: 9 ký tự (`Abc12345!`) | Giá trị chọn theo SRS. Regex thực tế bắt buộc khoảng trắng và không nhận ký tự đặc biệt nên cả 3 giá trị dự kiến bị chặn khi thực thi; cần bộ bổ sung thỏa mãn regex thực tế để cô lập biên độ dài: `Abc 123` (7), `Abc 1234` (8), `Abc 12345` (9) |
| Thời hạn token/mã | Không có | Không có | `server.js` không cài đặt thời hạn cho `reset_token` (lưu đến khi dùng hoặc cấp lại); `database.js` chỉ có cột `reset_token TEXT`, không có cột hết hạn; SRS không quy định | Không áp dụng (chưa xác minh) | SUT không có thời hạn token nên không thể áp dụng BVA 3 điểm cho biên này |

**Giải thích BVA:** Biên duy nhất xác minh được là độ dài tối thiểu 8 ký tự của mật khẩu mới, chọn đúng 3 điểm: Min-1 = 7 ký tự (`Abc12!`) → hệ thống báo lỗi "Mật khẩu quá yếu!"; Min = 8 ký tự (`Abc1234!`) → đổi mật khẩu thành công; Min+1 = 9 ký tự (`Abc12345!`) → đổi mật khẩu thành công (kỳ vọng theo SRS/FR-01). Điểm cần lưu ý sau khi rà soát mã nguồn: regex frontend bắt buộc khoảng trắng (`(?=.*\s)`) và chỉ chấp nhận `[A-Za-z\d\s]` nên cả 3 giá trị theo SRS ở trên đều chứa `!` và không có khoảng trắng -- dự kiến bị chặn với cảnh báo "Mật khẩu quá yếu!"; khi thực thi, FR03-BVA-02/03 được dự kiến Fail và xác nhận sai lệch regex (xem 2.1.7). Đồng thời chạy bổ sung bộ `Abc 123` / `Abc 1234` / `Abc 12345` (thỏa mãn regex thực tế, chỉ khác độ dài) để kiểm chứng riêng biên 8 ký tự. Biên tiềm năng khác chưa được thiết kế test case: độ dài OTP (SRS 6 chữ số, mã nguồn 4 chữ số -- biên 3/4/5 chữ số theo hành vi thực tế), cần xác nhận quy tắc thực tế trước khi thiết kế. Email và thời hạn token không có biên được xác minh (không áp dụng).

### 2.1.5. Test case BVA

| Test Case ID | Biến/biên kiểm thử | Giá trị | Tiền điều kiện | Các bước thực hiện | Kết quả mong đợi | Kết quả thực tế | Trạng thái | Bằng chứng |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| FR03-BVA-01 | Độ dài mật khẩu -- Min-1 | `newPassword = "Abc12!"` (7 ký tự) | Đã nhận mã OTP | 1. Nhập OTP hợp lệ<br>2. Nhập mật khẩu 7 ký tự<br>3. Bấm "Đặt lại mật khẩu" | Hệ thống báo lỗi "Mật khẩu quá yếu!" (chưa đạt tối thiểu 8 ký tự), từ chối submit | [Sau khi chạy] | Chưa chạy | [Link] |
| FR03-BVA-02 | Độ dài mật khẩu -- Min | `newPassword = "Abc1234!"` (8 ký tự) | Đã nhận mã OTP | 1. Nhập OTP hợp lệ<br>2. Nhập mật khẩu đúng 8 ký tự có đủ chữ hoa/thường/số/ký tự đặc biệt<br>3. Bấm "Đặt lại mật khẩu" | Đổi mật khẩu thành công (kỳ vọng theo SRS -- xem ghi chú 1) | [Sau khi chạy] | Chưa chạy | [Link] |
| FR03-BVA-03 | Độ dài mật khẩu -- Min+1 | `newPassword = "Abc12345!"` (9 ký tự) | Đã nhận mã OTP | 1. Nhập OTP hợp lệ<br>2. Nhập mật khẩu 9 ký tự hợp lệ<br>3. Bấm "Đặt lại mật khẩu" | Đổi mật khẩu thành công (kỳ vọng theo SRS -- xem ghi chú 1) | [Sau khi chạy] | Chưa chạy | [Link] |

Ghi chú:

1.  Kỳ vọng đặt theo SRS. Qua rà soát mã nguồn, regex thực tế bắt buộc khoảng trắng và không chấp nhận ký tự đặc biệt (xem 2.1.4) nên khi thực thi dự kiến cả hai TC Fail với cảnh báo "Mật khẩu quá yếu!"; chạy bổ sung `Abc 1234` (8 ký tự) và `Abc 12345` (9 ký tự) để xác nhận biên độ dài với mật khẩu thỏa mãn regex thực tế.

### 2.1.6. Kết quả thực thi và bằng chứng

- Tổng test case Domain Testing: 5
- Tổng test case BVA: 3
- Pass: 0 | Fail: 0 | Blocked: 0 | Not Executed: 8 (toàn bộ -- chưa thực thi)
- Bug liên quan: Chưa thực thi. Qua rà soát mã nguồn đã ghi nhận 4 sai lệch SRS ↔ mã nguồn (OTP 4 chữ số thay vì 6; thiếu trường xác nhận mật khẩu; thiếu chỉ báo bước "Bước 1 / 2"; regex mật khẩu bắt buộc khoảng trắng, không nhận ký tự đặc biệt) -- sẽ được xác nhận bằng thực thi và tạo GitHub Issue (xem 2.1.7).
- Bằng chứng: [Chưa có -- thu thập ảnh/log khi thực thi]
- Nhận xét: Bộ test case thiết kế phủ được happy path và các lớp lỗi chính ở cả hai bước. Khi thực thi cần chú ý: (1) các giá trị mật khẩu "hợp lệ" theo SRS dự kiến bị regex thực tế từ chối; (2) mã OTP ví dụ phải dùng 4 chữ số theo hành vi thực tế của SUT.

### 2.1.7. Đánh giá feature

- **Độ bao phủ lớp miền và biên:** Domain Testing phủ được 5 lớp chính (email hợp lệ, email chưa đăng ký, happy path bước 2, OTP sai, mật khẩu quá ngắn); BVA phủ đủ 3 điểm (Min-1/Min/Min+1) của biên duy nhất được xác minh (độ dài tối thiểu 8 ký tự của mật khẩu). Các lớp chưa có test case riêng: email sai định dạng, OTP rỗng, mật khẩu rỗng, mật khẩu thiếu độ phức tạp, xác nhận mật khẩu không khớp (UI thiếu trường), OTP đã dùng sau lần reset trước (token bị xóa `NULL`) -- đã nêu dữ liệu đề xuất bổ sung ở 2.1.3.
- **Rủi ro còn lại:** môi trường demo hiển thị OTP trực tiếp trên màn hình nên không kiểm thử được việc gửi email thật; SUT không cài đặt thời hạn token nên không thể kiểm thử lớp "OTP hết hạn"; OTP 4 chữ số theo mã nguồn có không gian giá trị nhỏ -- rủi ro bảo mật nằm ngoài phạm vi kiểm thử chức năng nhưng đáng ghi nhận.
- **Đóng góp hữu ích của AI:** bộ test case bám sát SRS, phân lớp biến rõ ràng (email, resetToken, newPassword, confirmPassword, trạng thái luồng hai bước); happy path và error path có thông báo lỗi cụ thể, dễ đối chiếu khi thực thi.
- **Đầu ra AI chưa chính xác/sai sót** (tổng hợp từ `forgot-reset-password-feature/ai-mistake.md`, đã đối chiếu mã nguồn):
  1. **OTP length mismatch:** SRS yêu cầu OTP 6 chữ số, mã nguồn sinh 4 chữ số (`Math.floor(1000 + Math.random() * 9000)` -- `server.js` dòng 72); nhãn UI cũng ghi "Mã OTP (4 số)".
  2. **Missing confirm password field:** UI thực tế (`ForgotPassword.jsx`) không có trường xác nhận mật khẩu mới so với SRS; FR03-DT-03 vẫn yêu cầu dữ liệu `confirmPassword` nên phải điều chỉnh khi thực thi.
  3. **Missing step indicator:** thiếu hiển thị "Bước 1 / 2" trên giao diện; ngoài ra bước 1 không có nút "Quay lại đăng nhập" như SRS (bước 2 có nút "← Quay lại" nhưng chỉ quay về bước 1).
  4. **Strict whitespace regex:** regex kiểm tra mật khẩu trong code bắt buộc khoảng trắng (`(?=.*\s)`) và chỉ cho phép `[A-Za-z\d\s]` (từ chối ký tự đặc biệt), khác SRS (yêu cầu ký tự đặc biệt hoặc khoảng trắng). Hệ quả: các giá trị mật khẩu "hợp lệ" theo SRS dùng trong test case (`Abc12345!`, `Abc1234!`) đều chứa `!` và không có khoảng trắng nên dự kiến bị từ chối khi thực thi.
  5. **Khai báo bao phủ vượt thực tế (phát hiện thêm khi rà soát):** coverage report của AI khai báo đã phủ các lớp (email sai định dạng, OTP rỗng, mật khẩu rỗng, thiếu độ phức tạp) mà test case chưa có tương ứng (xem 2.1.3).
- **Phần người kiểm thử đã bổ sung:** đối chiếu từng khai báo của AI với mã nguồn (`server.js`, `ForgotPassword.jsx`, `database.js`); đánh dấu các mục "Cần xác minh" trong bảng phân tích miền; đề xuất bộ giá trị mật khẩu thỏa mãn regex thực tế (`Abc 123`, `Abc 1234`, `Abc 12345`) để cô lập biên độ dài khỏi ảnh hưởng của sai lệch regex; liệt kê các lớp miền còn thiếu kèm dữ liệu đề xuất bổ sung.

---

## Số liệu dùng cho các bảng tổng hợp (không copy vào mục 2.1)

Khi cập nhật bảng 2.5 và bảng 6.2 của báo cáo, dùng các số sau cho FR-03:

- Test case thiết kế -- Domain Testing: **5**
- Test case thiết kế -- BVA: **3**
- Tổng test case thiết kế: **8**
- Đã thực thi: **0** | Pass: **0** | Fail: **0** | Blocked: **0** | Not Executed: **8**
- Bug xác nhận: **0** (chưa thực thi; 4 sai lệch SRS ↔ mã nguồn chờ xác nhận khi chạy)
