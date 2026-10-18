# HW02 -- Domain Testing on EShop

## Báo cáo áp dụng Domain Testing và Boundary Value Analysis

---

## Thông tin báo cáo

| Trường | Thông tin |
|---|---|
| Họ và tên | `Phạm Vương Quân` |
| MSSV | `23120341` |
| Môn học / bài tập | `HW02 – Domain Testing on EShop` |
| System Under Test (SUT) | [EShop SUT](https://github.com/ttbhanh/eshop-sut) |

## Các feature được lựa chọn

---

FR-03: Forgot password Pool A Quy trình quên mật khẩu and password reset (two và đặt lại mật khẩu qua steps) hai bước

FR-09: Discount coupons Pool B Áp dụng mã giảm giá trong luồng mua hàng/giỏ hàng theo hành vi thực tế của SUT

FR-14: Category Pool C Tạo, xem, cập nhật và management (CRUD) xóa danh mục

FR-07: Shopping cart Pool D Kiểm thử giỏ hàng trên (Mobile) ứng dụng mobile: xem giỏ, thêm/cập nhật/xóa sản phẩm và các trạng thái liên quan theo phạm vi SUT

---

# Mục lục

- [HW02 -- Domain Testing on EShop](#hw02----domain-testing-on-eshop)
  - [Báo cáo áp dụng Domain Testing và Boundary Value Analysis](#báo-cáo-áp-dụng-domain-testing-và-boundary-value-analysis)
  - [Thông tin báo cáo](#thông-tin-báo-cáo)
  - [Các feature được lựa chọn](#các-feature-được-lựa-chọn)
- [Mục lục](#mục-lục)
- [1. Phạm vi và quy trình kiểm thử](#1-phạm-vi-và-quy-trình-kiểm-thử)
  - [1.1. Mục tiêu](#11-mục-tiêu)
  - [1.2. Phạm vi và feature được chọn](#12-phạm-vi-và-feature-được-chọn)
    - [1.2.1. FR-03 -- Forgot password and password reset (two steps)](#121-fr-03----forgot-password-and-password-reset-two-steps)
    - [1.2.2. FR-09 -- Discount coupons](#122-fr-09----discount-coupons)
    - [1.2.3. FR-14 -- Category management (CRUD)](#123-fr-14----category-management-crud)
    - [1.2.4. FR-07 -- Shopping cart (Mobile)](#124-fr-07----shopping-cart-mobile)
  - [1.3. Quy trình Domain Testing](#13-quy-trình-domain-testing)
  - [1.4. Quy trình Boundary Value Analysis (3 điểm)](#14-quy-trình-boundary-value-analysis-3-điểm)
- [2. Thiết kế và thực thi kiểm thử](#2-thiết-kế-và-thực-thi-kiểm-thử)
  - [2.1. FR-03 -- Forgot password and password reset](#21-fr-03----forgot-password-and-password-reset)
    - [2.1.1. Mô tả chức năng và yêu cầu](#211-mô-tả-chức-năng-và-yêu-cầu)
    - [2.1.2. Domain Testing -- phân tích miền](#212-domain-testing----phân-tích-miền)
    - [2.1.3. Test case Domain Testing](#213-test-case-domain-testing)
    - [2.1.4. BVA -- phân tích biên](#214-bva----phân-tích-biên)
    - [2.1.5. Test case BVA](#215-test-case-bva)
    - [2.1.6. Kết quả thực thi](#216-kết-quả-thực-thi)
    - [2.1.7. Đánh giá feature](#217-đánh-giá-feature)
  - [2.2. FR-09 -- Discount coupons](#22-fr-09----discount-coupons)
    - [2.2.1. Mô tả chức năng và yêu cầu](#221-mô-tả-chức-năng-và-yêu-cầu)
    - [2.2.2. Domain Testing -- phân tích miền](#222-domain-testing----phân-tích-miền)
    - [2.2.3. Test case Domain Testing](#223-test-case-domain-testing)
    - [2.2.4. BVA -- phân tích biên](#224-bva----phân-tích-biên)
    - [2.2.5. Test case BVA](#225-test-case-bva)
    - [2.2.6. Kết quả thực thi](#226-kết-quả-thực-thi)
    - [2.2.7. Đánh giá feature](#227-đánh-giá-feature)
  - [2.3. FR-14 -- Category management (CRUD)](#23-fr-14----category-management-crud)
    - [2.3.1. Mô tả chức năng và yêu cầu](#231-mô-tả-chức-năng-và-yêu-cầu)
    - [2.3.2. Domain Testing -- phân tích miền](#232-domain-testing----phân-tích-miền)
    - [2.3.3. Test case Domain Testing](#233-test-case-domain-testing)
    - [2.3.4. BVA -- phân tích biên](#234-bva----phân-tích-biên)
    - [2.3.5. Test case BVA](#235-test-case-bva)
    - [2.3.6. Kết quả thực thi](#236-kết-quả-thực-thi)
    - [2.3.7. Đánh giá feature](#237-đánh-giá-feature)
  - [2.4. FR-07 -- Shopping cart (Mobile)](#24-fr-07----shopping-cart-mobile)
    - [2.4.1. Mô tả chức năng và yêu cầu](#241-mô-tả-chức-năng-và-yêu-cầu)
    - [2.4.2. Domain Testing -- phân tích miền](#242-domain-testing----phân-tích-miền)
    - [2.4.3. Test case Domain Testing](#243-test-case-domain-testing)
    - [2.4.4. BVA -- phân tích biên](#244-bva----phân-tích-biên)
    - [2.4.5. Test case BVA](#245-test-case-bva)
    - [2.4.6. Kết quả thực thi](#246-kết-quả-thực-thi)
    - [2.4.7. Đánh giá feature](#247-đánh-giá-feature)
- [3. AI Gap Analysis and Evaluation](#3-ai-gap-analysis-and-evaluation)
  - [3.1. Các test case hoặc lỗi AI bỏ sót](#31-các-test-case-hoặc-lỗi-ai-bỏ-sót)
  - [3.2. Phân tích nguyên nhân](#32-phân-tích-nguyên-nhân)
  - [3.3. So sánh trước và sau khi rà soát](#33-so-sánh-trước-và-sau-khi-rà-soát)
  - [3.4. Bài học rút ra](#34-bài-học-rút-ra)

---

# 1. Phạm vi và quy trình kiểm thử

## 1.1. Mục tiêu

Báo cáo trình bày cách áp dụng **Domain Testing** và **Boundary Value Analysis (BVA)** để thiết kế, rà soát và thực thi test case cho bốn feature đã chọn của EShop. AI được sử dụng như công cụ hỗ trợ phân tích và tạo đề xuất; mọi đầu ra phải được người thực hiện kiểm tra, chỉnh sửa và xác minh bằng hành vi thực tế của SUT.

Mục tiêu cụ thể:

- Phân tích điều kiện đầu vào, quy tắc nghiệp vụ và miền giá trị của từng feature.
- Chia miền giá trị thành các lớp tương đương/miền hợp lệ và không hợp lệ phù hợp với yêu cầu.
- Xác định các giá trị biên và thiết kế test case cho các điểm ngay dưới, tại và ngay trên biên khi áp dụng được.
- Thực thi test case, đối chiếu kết quả thực tế với kết quả mong đợi và lưu bằng chứng.
- Phát hiện, xác nhận, ghi nhận lỗi; phân tích các khoảng trống trong đề xuất của AI.
- Xây dựng hoặc mô tả Agent Skills có thể tái sử dụng cho Domain Testing và BVA.

## 1.2. Phạm vi và feature được chọn

### 1.2.1. FR-03 -- Forgot password and password reset (two steps)

**Mục tiêu kiểm thử:** xác minh luồng quên mật khẩu gồm hai bước theo thiết kế của SUT, bao gồm dữ liệu đầu vào, điều kiện chuyển bước, xác thực thông tin và kết quả đặt lại mật khẩu.

**Phạm vi cần xác minh từ SUT/yêu cầu:**

- Trường dữ liệu và điều kiện bắt buộc ở từng bước.
- Định dạng email hoặc định danh tài khoản.
- Hành vi khi tài khoản tồn tại/không tồn tại.
- Mã xác minh/token, thời hạn và trạng thái sử dụng lại (nếu SUT có).
- Quy tắc mật khẩu mới và xác nhận mật khẩu (nếu có).
- Điều hướng, thông báo lỗi/thành công và khả năng đăng nhập bằng mật khẩu mới.

### 1.2.2. FR-09 -- Discount coupons

**Mục tiêu kiểm thử:** xác minh quy tắc áp dụng coupon/mã giảm giá và ảnh hưởng của coupon lên tổng tiền đơn hàng theo hành vi được cài đặt trong SUT.

**Phạm vi cần xác minh từ SUT/yêu cầu:**

- Mã coupon hợp lệ, không hợp lệ, trống hoặc sai định dạng.
- Trạng thái hoạt động, thời hạn hiệu lực và giới hạn sử dụng (nếu có).
- Điều kiện giá trị đơn hàng tối thiểu/tối đa (nếu có).
- Kiểu giảm giá, mức giảm, giới hạn giảm tối đa (nếu có).
- Quy tắc kết hợp coupon, áp dụng lại hoặc gỡ coupon.
- Cách cập nhật tổng tiền và thông báo cho người dùng.


### 1.2.3. FR-14 -- Category management (CRUD)

**Mục tiêu kiểm thử:** xác minh các thao tác tạo (Create), xem/đọc (Read), cập nhật (Update) và xóa (Delete) danh mục.

**Phạm vi cần xác minh từ SUT/yêu cầu:**

- Trường bắt buộc, độ dài tối thiểu/tối đa và định dạng tên danh mục.
- Tên trùng lặp, khoảng trắng đầu/cuối và ký tự đặc biệt.
- Khả năng xem danh sách/chi tiết và xử lý khi không có dữ liệu.
- Quy tắc cập nhật danh mục và xử lý dữ liệu không thay đổi.
- Điều kiện xóa danh mục đang được sản phẩm tham chiếu (nếu có).
- Quyền truy cập, thông báo xác nhận và phản hồi thành công/thất bại.

### 1.2.4. FR-07 -- Shopping cart (Mobile)

**Mục tiêu kiểm thử:** xác minh hành vi giỏ hàng trên giao diện/ứng dụng mobile trong phạm vi thực tế của SUT.

**Phạm vi cần xác minh:**

- Giỏ hàng rỗng và giỏ hàng có sản phẩm.
- Thêm sản phẩm vào giỏ; tăng/giảm số lượng.
- Giá trị số lượng hợp lệ và không hợp lệ, nếu giao diện cho phép nhập trực tiếp.
- Xóa sản phẩm và cập nhật tổng tiền.
- Sản phẩm hết hàng/không còn khả dụng hoặc giá thay đổi (nếu SUT hỗ trợ tình huống này).
- Trạng thái giỏ hàng khi điều hướng, tải lại hoặc đăng nhập/đăng xuất, tùy thiết kế.
- Khả năng sử dụng trên kích thước màn hình mobile và thông báo phản hồi.

## 1.3. Quy trình Domain Testing

Áp dụng quy trình dưới đây riêng cho từng feature và lưu lại kết quả ở Chương 2.

1.  **Đọc yêu cầu và khảo sát SUT:** xác định luồng chính, điều kiện
    tiền đề, dữ liệu đầu vào, kết quả đầu ra và quy tắc nghiệp vụ có thể
    kiểm chứng.
2.  **Xác định biến/điều kiện ảnh hưởng:** liệt kê từng trường dữ liệu
    và trạng thái có thể làm thay đổi kết quả.
3.  **Xác định miền giá trị:** mô tả các giá trị hợp lệ và không hợp lệ
    dựa trên bằng chứng từ yêu cầu, giao diện hoặc mã nguồn.
4.  **Chia miền thành các lớp tương đương:** nhóm giá trị dự kiến tạo ra
    hành vi giống nhau; tách các lớp có thể dẫn đến kết quả khác nhau.
5.  **Xác định tổ hợp điều kiện quan trọng:** chọn tổ hợp cần kiểm tra,
    chú ý các điều kiện nghiệp vụ tương tác với nhau.
6.  **Chọn giá trị đại diện:** chọn ít nhất một giá trị đại diện cho mỗi
    lớp quan trọng; bổ sung trường hợp âm tính và trường hợp trạng thái.
7.  **Hoàn thiện test case:** ghi mã, tiền điều kiện, dữ liệu, bước thực
    hiện, kết quả mong đợi và nguồn căn cứ.
8.  **Thực thi và cập nhật:** chạy test case, lưu kết quả thực tế/bằng
    chứng và tạo bug report nếu phát hiện sai lệch.
9.  **Đánh giá độ bao phủ:** ghi rõ lớp miền nào đã được kiểm tra, lớp
    nào chưa và lý do.

## 1.4. Quy trình Boundary Value Analysis (3 điểm)

Áp dụng quy trình Boundary Value Analysis theo đúng 3 điểm (giá trị ngay dưới biên, tại biên và ngay trên biên) cho các biến có miền giới hạn.

1.  Xác định biến/điều kiện có biên giới (độ dài chuỗi, số lượng, giá trị tiền, phần trăm giảm, thời hạn...) từ yêu cầu/mã nguồn SUT. Nếu chưa xác định được, ghi là **chưa xác minh**.
2.  Với mỗi biên xác định được, chọn đúng 3 giá trị đại diện:
    - Giá trị ngay dưới biên (ví dụ: min-1, length-1)
    - Giá trị tại biên (ví dụ: min, exact length)
    - Giá trị ngay trên biên (ví dụ: min+1, length+1)

    Chọn giá trị có ý nghĩa thực tế và có thể nhập được.

3.  Xác định kết quả mong đợi cho từng giá trị dựa trên quy tắc nghiệp vụ.
4.  Hoàn thiện test case BVA: ghi mã, tiền điều kiện, dữ liệu, bước thực hiện, kết quả mong đợi và nguồn căn cứ.
5.  Thực thi test case BVA (3 điểm); ghi kết quả thực tế, trạng thái và bằng chứng.
6.  Đánh giá độ bao phủ các biên đã kiểm tra và ghi rõ biên nào chưa thể kiểm tra cùng lý do.

# 2. Thiết kế và thực thi kiểm thử

## 2.1. FR-03 -- Forgot password and password reset

### 2.1.1. Mô tả chức năng và yêu cầu

- **Mục đích chức năng:** Cho phép người dùng khôi phục tài khoản khi quên mật khẩu qua hai bước: (1) nhập email đã đăng ký để nhận mã OTP -- trong môi trường demo, mã OTP hiển thị trực tiếp trên màn hình thay vì gửi email; (2) nhập OTP và mật khẩu mới để đặt lại mật khẩu.
- **Điểm bắt đầu/kết thúc:** Bắt đầu tại trang "Quên Mật Khẩu". Kết thúc: thông báo "Đổi mật khẩu thành công!" và chuyển hướng về trang đăng nhập; hoặc kết thúc sớm bằng thông báo lỗi tương ứng.
- **Tiền điều kiện:** Bước 1 -- đã có tài khoản đăng ký với email tồn tại trong CSDL (dữ liệu mẫu: `test@eshop.com`, `admin@eshop.com`). Bước 2 -- đã nhận mã OTP hợp lệ cho email vừa yêu cầu.
- **Luồng nghiệp vụ:**
  - Flow 1 (thành công): Nhập email hợp lệ → nhận mã OTP và chuyển sang bước 2 → nhập OTP đúng + mật khẩu mới hợp lệ → "Đổi mật khẩu thành công!" → chuyển về trang đăng nhập.
  - Flow 2 (lỗi bước 1): Email không tồn tại / sai định dạng → lỗi "User not found" (HTTP 404).
  - Flow 3 (lỗi bước 2): OTP sai → lỗi "Mã OTP không đúng hoặc có lỗi xảy ra." (backend trả "Invalid token or email", HTTP 400); mật khẩu yếu → cảnh báo phía client "Mật khẩu quá yếu!" và từ chối submit.
- **Giả định cần xác minh** (chưa xem là yêu cầu chính thức; đã đối chiếu mã nguồn):
  1. SRS yêu cầu OTP 6 chữ số, nhưng mã nguồn sinh 4 chữ số (1000--9999, `server.js` dòng 72) và nhãn UI ghi "Mã OTP (4 số)".
  2. SRS yêu cầu trường "Xác nhận mật khẩu mới", nhưng UI thực tế không có trường này.
  3. SRS yêu cầu chỉ báo bước "Bước 1 / 2" và nút "Quay lại đăng nhập", nhưng UI không hiển thị chỉ báo bước; bước 2 có nút "← Quay lại" chỉ quay về bước 1, bước 1 không có nút về trang đăng nhập.
  4. SRS cho phép mật khẩu có "ký tự đặc biệt/khoảng trắng", nhưng regex thực tế bắt buộc khoảng trắng (`(?=.*\s)`) và không chấp nhận ký tự đặc biệt (chỉ `[A-Za-z\d\s]`).
  5. SRS không nêu thời hạn OTP; mã nguồn không cài đặt thời hạn cho `reset_token` (lưu đến khi được dùng hoặc cấp lại).
  6. Token chỉ dùng một lần: `reset_token` bị xóa (`NULL`) sau khi reset thành công -- **đã xác nhận bằng thực thi**: dùng lại OTP cũ ngay sau khi đổi mật khẩu thành công bị từ chối (HTTP 400 "Invalid token or email").

### 2.1.2. Domain Testing -- phân tích miền

| Biến/điều kiện | Miền/lớp hợp lệ | Miền/lớp không hợp lệ | Căn cứ | Cần xác minh? |
| --- | --- | --- | --- | --- |
| Email/định danh | Email đúng định dạng và đã đăng ký trong CSDL (VD: `test@eshop.com`) | (a) Đúng định dạng nhưng chưa đăng ký (VD: `notfound@eshop.com`); (b) sai định dạng (VD: `abc`, thiếu `@`). Lưu ý: ô nhập là `type="text"` nên trình duyệt không kiểm tra định dạng; mọi chuỗi không khớp CSDL đều trả về "User not found" (HTTP 404) -- lớp (b) thực tế hành xử giống lớp (a) | SRS FR-03 ; `server.js` dòng 68--71 (tra cứu chính xác email, trả 404); `ForgotPassword.jsx` dòng 50--55 | Có (hành vi lớp sai định dạng khi thực thi) -- **đã xác nhận bằng thực thi**: `abc` trả 404 "User not found" giống hệt email chưa đăng ký |
| Trạng thái tài khoản | Tài khoản tồn tại trong bảng `users` | Tài khoản không tồn tại (không có bản ghi) | `server.js` dòng 70--71; `database.js` dòng 50--63 (bảng `users`), dòng 92--93 (dữ liệu mẫu) | Không (đã đối chiếu khi thực thi: dữ liệu mẫu tồn tại, đăng nhập được) |
| Mã xác minh/token (resetToken) | OTP trùng khớp `reset_token` đang lưu của email đã yêu cầu; theo mã nguồn gồm 4 chữ số (VD: `1234`), theo SRS là 6 chữ số | OTP sai (VD: `9999`); OTP rỗng; token đã bị xóa (`NULL`) sau lần reset thành công trước đó | SRS FR-03; `server.js` dòng 72 (sinh token 4 chữ số), dòng 87--94 (đối chiếu email + token); `ForgotPassword.jsx` dòng 69--77 | Có (độ dài OTP khác SRS; token dùng một lần) -- **đã xác nhận bằng thực thi**: OTP thực tế 4 chữ số; OTP sai và OTP đã dùng đều bị từ chối (HTTP 400); OTP rỗng bị `required` chặn submit |
| Mật khẩu mới | Theo SRS: tối thiểu 8 ký tự, có chữ hoa, chữ thường, số và ký tự đặc biệt/khoảng trắng (VD: `Abc12345!`) | Dưới 8 ký tự (VD: `Ab1!`); rỗng; đủ dài nhưng thiếu độ phức tạp. Lưu ý: regex thực tế `/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*\s)[A-Za-z\d\s]{8,}$/` bắt buộc khoảng trắng, không nhận ký tự đặc biệt -- mâu thuẫn SRS | SRS FR-03 (quy tắc mật khẩu theo FR-01); `ForgotPassword.jsx` dòng 26--30 | Có (regex thực tế khác SRS) -- **đã xác nhận bằng thực thi**: mọi mật khẩu chứa `!` và thiếu khoảng trắng đều bị chặn với cảnh báo "Mật khẩu quá yếu!"; mật khẩu thỏa regex thực tế (`Abc 1234`) được chấp nhận |
| Xác nhận mật khẩu (confirmPassword) | Trùng khớp hoàn toàn mật khẩu mới | Không khớp mật khẩu mới | SRS FR-03 (`feature-overview.md`) | Có -- UI thực tế không có trường xác nhận (`ForgotPassword.jsx`) nên lớp này hiện không thể kiểm thử qua giao diện; sai lệch so với SRS -- **đã xác nhận khi thực thi**: giao diện bước 2 chỉ có 1 ô mật khẩu, không có nhãn "Xác nhận mật khẩu" |
| Trạng thái luồng hai bước | Bước 1: nhập email → nhận OTP; bước 2: nhập OTP + mật khẩu mới; có nút "← Quay lại" về bước 1 | Thao tác ở bước 2 khi chưa có OTP hợp lệ (yêu cầu reset bị từ chối vì email + token không khớp CSDL) | `ForgotPassword.jsx` (state `step` 1/2 -- dòng 7, 45--97); `server.js` dòng 87--94 | Có (SRS yêu cầu chỉ báo "Bước 1 / 2" và nút quay lại đăng nhập -- UI chưa có) -- **đã xác nhận bằng thực thi**: không có chỉ báo bước ở cả 2 bước; bước 1 không có nút về trang đăng nhập; nút "← Quay lại" hoạt động đúng (về bước 1) |

**Giải thích phân hoạch miền:** Miền được phân theo từng biến đầu vào của hai bước; mỗi biến tách thành lớp hợp lệ/không hợp lệ vì chúng tạo ra kết quả đầu ra khác nhau: thông báo thành công và chuyển trang ("Đổi mật khẩu thành công!") so với các thông báo lỗi riêng ("User not found", "Mã OTP không đúng hoặc có lỗi xảy ra.", "Mật khẩu quá yếu!"). Giá trị đại diện ưu tiên happy path (email hợp lệ → OTP đúng → mật khẩu hợp lệ) và các lớp lỗi có thông báo riêng để đối chiếu trực tiếp khi thực thi. Các sai lệch SRS ↔ mã nguồn (OTP 4 chữ số thay vì 6; thiếu trường xác nhận mật khẩu; thiếu chỉ báo bước; regex mật khẩu bắt buộc khoảng trắng) được đánh dấu "Cần xác minh" và sẽ được xác nhận bằng thực thi. Lớp "email sai định dạng" về hành vi bị gộp chung với lớp "email không tồn tại" (đều trả về 404 "User not found") nên khi thực thi có thể dùng chung một giá trị đại diện để xác nhận hành vi gộp này.

### 2.1.3. Test case Domain Testing

| Test Case ID | Lớp miền/điều kiện | Tiền điều kiện | Dữ liệu đầu vào | Các bước thực hiện | Kết quả mong đợi | Kết quả thực tế | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FR03-DT-01 | Email hợp lệ (đã đăng ký) | User đã đăng ký tài khoản | `email = "test@eshop.com"` | 1. Truy cập trang Quên mật khẩu<br>2. Nhập email hợp lệ<br>3. Bấm "Lấy mã OTP" | Hiển thị thông báo chứa mã OTP ("Mã OTP của bạn là: ..."), giao diện chuyển sang bước nhập OTP & mật khẩu (SRS: kèm chỉ báo bước "Bước 1 / 2") | Luồng chính đúng: HTTP 200, hiện "Mã OTP của bạn là: 4197" (4 chữ số, hiển thị trực tiếp trên màn hình), chuyển sang bước 2 với ô OTP + ô mật khẩu và nút "← Quay lại" -- nhưng **không có chỉ báo bước "Bước 1 / 2"** (và bước 1 không có nút "Quay lại đăng nhập") như SRS yêu cầu | **Fail → BUG-FR03-01** (thiếu step indicator) |
| FR03-DT-02 | Email không hợp lệ (chưa đăng ký) | Không có | `email = "notfound@eshop.com"` | 1. Truy cập trang Quên mật khẩu<br>2. Nhập email chưa đăng ký<br>3. Bấm "Lấy mã OTP" | Hiển thị lỗi "User not found" (HTTP 404) | HTTP 404 `{"error":"User not found"}`; dialog "Lỗi: User not found"; ở lại trang /forgot-password | **Pass** |
| FR03-DT-03 | Happy path bước 2 (OTP đúng, mật khẩu hợp lệ, xác nhận khớp) | Đã nhận mã OTP cho email đã đăng ký (VD: `1234`) | `email = "test@eshop.com"`, `resetToken = "1234"`, `newPassword = "Abc12345!"`, `confirmPassword = "Abc12345!"` (xem ghi chú 1) | 1. Nhập mã OTP chính xác<br>2. Nhập mật khẩu mới hợp lệ (và xác nhận khớp nếu có trường)<br>3. Bấm "Đặt lại mật khẩu" | Thông báo "Đổi mật khẩu thành công!", chuyển hướng về trang đăng nhập; đăng nhập được bằng mật khẩu mới | UI không có trường xác nhận mật khẩu (bỏ qua dữ liệu `confirmPassword`, ghi chú 1). OTP đúng + mật khẩu `Abc12345!` (hợp lệ theo SRS) → popup "Mật khẩu quá yếu! Phải dài tối thiểu 8 ký tự, gồm chữ hoa, chữ thường, số và KÝ TỰ ĐẶC BIỆT."; **không gửi yêu cầu** lên server (regex frontend chặn); ở lại trang, mật khẩu không đổi (đăng nhập bằng mật khẩu cũ `Test1234!` vẫn HTTP 200) | **Fail → BUG-FR03-02** (regex khác SRS) |
| FR03-DT-04 | OTP không hợp lệ (sai OTP) | Đã nhận mã OTP | `resetToken = "9999"` (sai OTP), email hợp lệ, mật khẩu hợp lệ | 1. Nhập mã OTP sai<br>2. Nhập mật khẩu hợp lệ<br>3. Bấm "Đặt lại mật khẩu" | Hiển thị lỗi "Mã OTP không đúng hoặc có lỗi xảy ra." (backend trả 400 "Invalid token or email") | Nhập OTP thật (`9359`) nhận được ở bước 1, OTP submit = `9999` (sai), mật khẩu hợp lệ theo regex thực tế (`Abc 1234`) → HTTP 400 `{"error":"Invalid token or email"}`; dialog "Mã OTP không đúng hoặc có lỗi xảy ra."; ở lại trang | **Pass** |
| FR03-DT-05 | Mật khẩu mới không hợp lệ (quá ngắn) | Đã nhận mã OTP | `newPassword = "Ab1!"` (4 ký tự, dưới 8) | 1. Nhập OTP đúng<br>2. Nhập mật khẩu quá ngắn<br>3. Bấm "Đặt lại mật khẩu" | Hiển thị cảnh báo "Mật khẩu quá yếu!...", từ chối submit (không gửi yêu cầu lên server) | Popup "Mật khẩu quá yếu! Phải dài tối thiểu 8 ký tự, gồm chữ hoa, chữ thường, số và KÝ TỰ ĐẶC BIỆT."; không có request nào gửi lên `/api/reset-password`; ở lại trang | **Pass** |

Ghi chú:

1.  Test case giữ dữ liệu `confirmPassword` theo thiết kế dựa trên SRS; tuy nhiên UI thực tế không có trường này (đã xác minh trong `ForgotPassword.jsx`) -- khi thực thi bỏ qua dữ liệu xác nhận và ghi nhận sai lệch (xem 2.1.7). Ngoài ra, `Abc12345!` thỏa mãn SRS nhưng không thỏa mãn regex thực tế (thiếu khoảng trắng, chứa ký tự đặc biệt) nên dự kiến bị chặn khi thực thi (xem 2.1.4) -- **thực thi đã xác nhận đúng**: bị chặn ở client, FAIL → BUG-FR03-02.

**Đánh giá Domain Testing:** 5 test case thiết kế phủ được các lớp: email hợp lệ, email chưa đăng ký, happy path bước 2 (OTP đúng + mật khẩu hợp lệ), OTP sai, mật khẩu quá ngắn. Sau khi rà soát, coverage report của AI (khai báo email 3/3 lớp, resetToken 3/3 lớp, newPassword 4/5 lớp) cao hơn thực tế -- có 4 lớp được khai báo nhưng chưa có test case riêng: (1) email sai định dạng; (2) OTP rỗng; (3) mật khẩu rỗng; (4) mật khẩu đủ 8 ký tự nhưng thiếu độ phức tạp. Đề xuất bổ sung trước khi thực thi: `email = "abc"` (thiếu `@`, dự kiến 404 "User not found" do không có kiểm tra định dạng phía client), `resetToken = ""` (dự kiến trình duyệt chặn submit do `required`), `newPassword = ""` (chặn submit do `required`), `newPassword = "abcdefgh"` (dự kiến cảnh báo mật khẩu yếu). Lớp "xác nhận mật khẩu không khớp" không thể kiểm thử qua UI do thiếu trường (sai lệch SRS, xem 2.1.7). **Kết quả sau khi thực thi:** 4 bổ sung đề xuất đã chạy kiểm chứng bổ sung và khớp đúng dự đoán: `abc` → 404 "User not found"; OTP rỗng → chặn bởi `required` ("Please fill out this field.", không gửi request); mật khẩu rỗng → chặn bởi `required`; `abcdefgh` → popup "Mật khẩu quá yếu!". Như vậy toàn bộ các lớp đã khai báo trong coverage report đều có dữ liệu thực thi tương ứng (nhưng các lớp này vẫn không được AI thiết kế thành test case riêng -- ghi nhận vào 2.1.7).

### 2.1.4. BVA -- phân tích biên

| Biến/điều kiện | Biên dưới | Biên trên | Căn cứ xác định biên | Giá trị cần thử | Ghi chú |
| --- | --- | --- | --- | --- | --- |
| Email/định danh | Không có | Không có | SRS FR-03 không quy định giới hạn độ dài/định dạng; `server.js` chỉ tra cứu chính xác chuỗi email; ô nhập `type="text"` không kiểm tra | Không áp dụng | Không có biên nào được xác minh cho email |
| Mật khẩu mới | 8 ký tự (độ dài tối thiểu) | Không có (không ràng buộc độ dài tối đa) | SRS FR-03/FR-01 (tối thiểu 8 ký tự); regex `{8,}` trong `ForgotPassword.jsx` dòng 26 | Min-1: 7 ký tự (`Abc12!`); Min: 8 ký tự (`Abc1234!`); Min+1: 9 ký tự (`Abc12345!`) | Giá trị chọn theo SRS. Regex thực tế bắt buộc khoảng trắng và không nhận ký tự đặc biệt nên cả 3 giá trị dự kiến bị chặn khi thực thi; cần bộ bổ sung thỏa mãn regex thực tế để cô lập biên độ dài: `Abc 123` (7), `Abc 1234` (8), `Abc 12345` (9) |
| Thời hạn token/mã | Không có | Không có | `server.js` không cài đặt thời hạn cho `reset_token` (lưu đến khi dùng hoặc cấp lại); `database.js` chỉ có cột `reset_token TEXT`, không có cột hết hạn; SRS không quy định | Không áp dụng (chưa xác minh) | SUT không có thời hạn token nên không thể áp dụng BVA 3 điểm cho biên này |

**Giải thích BVA:** Biên duy nhất xác minh được là độ dài tối thiểu 8 ký tự của mật khẩu mới, chọn đúng 3 điểm: Min-1 = 7 ký tự (`Abc12!`) → hệ thống báo lỗi "Mật khẩu quá yếu!"; Min = 8 ký tự (`Abc1234!`) → đổi mật khẩu thành công; Min+1 = 9 ký tự (`Abc12345!`) → đổi mật khẩu thành công (kỳ vọng theo SRS/FR-01). Điểm cần lưu ý sau khi rà soát mã nguồn: regex frontend bắt buộc khoảng trắng (`(?=.*\s)`) và chỉ chấp nhận `[A-Za-z\d\s]` nên cả 3 giá trị theo SRS ở trên đều chứa `!` và không có khoảng trắng -- dự kiến bị chặn với cảnh báo "Mật khẩu quá yếu!"; khi thực thi, FR03-BVA-02/03 được dự kiến Fail và xác nhận sai lệch regex (xem 2.1.7). Đồng thời chạy bổ sung bộ `Abc 123` / `Abc 1234` / `Abc 12345` (thỏa mãn regex thực tế, chỉ khác độ dài) để kiểm chứng riêng biên 8 ký tự. Biên tiềm năng khác chưa được thiết kế test case: độ dài OTP (SRS 6 chữ số, mã nguồn 4 chữ số -- biên 3/4/5 chữ số theo hành vi thực tế), cần xác nhận quy tắc thực tế trước khi thiết kế. Email và thời hạn token không có biên được xác minh (không áp dụng). **Kết quả thực thi (2026-10-18):** BVA-02/03 Fail đúng như dự đoán (BUG-FR03-02); bộ bổ sung 3 điểm `Abc 123`/`Abc 1234`/`Abc 12345` cho kết quả 7 ký tự bị chặn / 8 và 9 ký tự thành công (có điều hướng về `/login`) → biên dưới 8 ký tự được xác nhận chính xác khi cô lập khỏi sai lệch regex. OTP quan sát được luôn 4 chữ số (khớp mã nguồn, khác SRS 6 chữ số).

### 2.1.5. Test case BVA

| Test Case ID | Biến/biên kiểm thử | Giá trị | Tiền điều kiện | Các bước thực hiện | Kết quả mong đợi | Kết quả thực tế | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FR03-BVA-01 | Độ dài mật khẩu -- Min-1 | `newPassword = "Abc12!"` (7 ký tự) | Đã nhận mã OTP | 1. Nhập OTP hợp lệ<br>2. Nhập mật khẩu 7 ký tự<br>3. Bấm "Đặt lại mật khẩu" | Hệ thống báo lỗi "Mật khẩu quá yếu!" (chưa đạt tối thiểu 8 ký tự), từ chối submit | Popup "Mật khẩu quá yếu! Phải dài tối thiểu 8 ký tự, gồm chữ hoa, chữ thường, số và KÝ TỰ ĐẶC BIỆT."; không gửi request; ở lại trang. **Lưu ý:** từ chối do vi phạm đồng thời cả độ dài (7 < 8) lẫn regex thực tế (chứa `!`, thiếu khoảng trắng) -- biên độ dài bị che khuất, cần bộ bổ sung S-BVA-01 để cô lập | **Pass** |
| FR03-BVA-02 | Độ dài mật khẩu -- Min | `newPassword = "Abc1234!"` (8 ký tự) | Đã nhận mã OTP | 1. Nhập OTP hợp lệ<br>2. Nhập mật khẩu đúng 8 ký tự có đủ chữ hoa/thường/số/ký tự đặc biệt<br>3. Bấm "Đặt lại mật khẩu" | Đổi mật khẩu thành công (kỳ vọng theo SRS -- xem ghi chú 1) | Popup "Mật khẩu quá yếu!..."; mật khẩu 8 ký tự hợp lệ theo SRS bị regex thực tế từ chối vì chứa `!` và không có khoảng trắng; không gửi request; mật khẩu không đổi | **Fail → BUG-FR03-02** |
| FR03-BVA-03 | Độ dài mật khẩu -- Min+1 | `newPassword = "Abc12345!"` (9 ký tự) | Đã nhận mã OTP | 1. Nhập OTP hợp lệ<br>2. Nhập mật khẩu 9 ký tự hợp lệ<br>3. Bấm "Đặt lại mật khẩu" | Đổi mật khẩu thành công (kỳ vọng theo SRS -- xem ghi chú 1) | Popup "Mật khẩu quá yếu!..."; mật khẩu 9 ký tự hợp lệ theo SRS bị regex thực tế từ chối vì chứa `!` và không có khoảng trắng; không gửi request; mật khẩu không đổi | **Fail → BUG-FR03-02** |
| FR03-BVA-01S | Bổ sung -- biên độ dài với mật khẩu thỏa regex thực tế: Min-1 | `newPassword = "Abc 123"` (7 ký tự) | Đã nhận mã OTP (kiểm thử bổ sung, không tính số liệu chính thức) | 1. Nhập OTP hợp lệ<br>2. Nhập mật khẩu 7 ký tự thỏa regex thực tế (`[A-Za-z\d\s]`, có khoảng trắng)<br>3. Bấm "Đặt lại mật khẩu" | Báo lỗi "Mật khẩu quá yếu!" -- bị chặn do chỉ thiếu độ dài | Popup "Mật khẩu quá yếu!..."; không gửi request → biên dưới 8 ký tự hoạt động đúng khi tách khỏi ảnh hưởng regex | Pass (bổ sung) |
| FR03-BVA-02S | Bổ sung -- Min | `newPassword = "Abc 1234"` (8 ký tự) | Đã nhận mã OTP | 1. Nhập OTP hợp lệ<br>2. Nhập mật khẩu 8 ký tự thỏa regex thực tế<br>3. Bấm "Đặt lại mật khẩu" | Đổi mật khẩu thành công | Popup "Đổi mật khẩu thành công!"; HTTP 200 `{"message":"Password reset successfully"}`; **điều hướng về `/login`**; đăng nhập bằng `Abc 1234` HTTP 200 → biên Min hoạt động đúng | Pass (bổ sung) |
| FR03-BVA-03S | Bổ sung -- Min+1 | `newPassword = "Abc 12345"` (9 ký tự) | Đã nhận OTP mới sau lần reset thành công trước | 1. Nhập OTP hợp lệ mới<br>2. Nhập mật khẩu 9 ký tự thỏa regex thực tế<br>3. Bấm "Đặt lại mật khẩu" | Đổi mật khẩu thành công | Popup "Đổi mật khẩu thành công!"; HTTP 200; điều hướng về `/login`; đăng nhập bằng `Abc 12345` HTTP 200 → biên Min+1 hoạt động đúng | Pass (bổ sung) |

Ghi chú:

1.  Kỳ vọng đặt theo SRS. Qua rà soát mã nguồn, regex thực tế bắt buộc khoảng trắng và không chấp nhận ký tự đặc biệt (xem 2.1.4) nên khi thực thi dự kiến cả hai TC Fail với cảnh báo "Mật khẩu quá yếu!"; chạy bổ sung `Abc 1234` (8 ký tự) và `Abc 12345` (9 ký tự) để xác nhận biên độ dài với mật khẩu thỏa mãn regex thực tế. **Kết quả thực thi khớp đúng dự đoán:** FR03-BVA-02/03 Fail do regex; bộ bổ sung BVA-01S/02S/03S đều Pass chứng tỏ biên 8 ký tự bản thân hoạt động đúng -- nguyên nhân Fail nằm trọn ở sai lệch regex SRS ↔ mã nguồn (BUG-FR03-02).

### 2.1.6. Kết quả thực thi

- **Cách thức thực thi:** 2026-10-18, chạy trực tiếp trên giao diện frontend-web (`http://localhost:5173/forgot-password`, React + Vite) bằng trình duyệt Edge (Chromium) chế độ headless; backend Node/Express + SQLite chạy tại `http://localhost:3000` với CSDL seed sạch. Kết quả HTTP và popup (alert) được ghi nhận tự động theo từng bước của test case; sau mỗi đợt chạy, backend được khởi động lại để re-seed CSDL về trạng thái gốc (`test@eshop.com / Test1234!`).
- Tổng test case Domain Testing: 5 -- đã thực thi **5/5** (Pass 3 | Fail 2)
- Tổng test case BVA: 3 -- đã thực thi **3/3** (Pass 1 | Fail 2); ngoài ra 3 test bổ sung (BVA-01S/02S/03S, mật khẩu thỏa regex thực tế) đều Pass -- không tính vào số liệu chính thức
- Tổng kết cả hai kỹ thuật: **8 test case chính thức, thực thi 8/8: Pass 4 | Fail 4 | Blocked 0 | Not Executed 0**
- Bug xác nhận qua thực thi (2/4 sai lệch SRS ↔ mã nguồn liệt kê tại 2.1.1 cấu thành Fail của test case; mỗi bug có GitHub Issue tương ứng):
  - **BUG-FR03-01 -- Thiếu chỉ báo bước "Bước 1 / 2" và nút "Quay lại đăng nhập" ở bước 1** (sai lệch số 3, SRS FR-03): xác nhận qua FR03-DT-01 -- giao diện chỉ có 2 bước chuyển qua lại bằng nút "← Quay lại", không hiển thị bước hiện tại. Severity: Medium. GitHub Issue: [#1](https://github.com/kiterakun/HW02-Domain_Testing/issues/1)
  - **BUG-FR03-02 -- Regex mật khẩu khác SRS, từ chối mọi mật khẩu hợp lệ theo SRS** (sai lệch số 4; Critical): xác nhận qua FR03-DT-03, FR03-BVA-02, FR03-BVA-03 -- `Abc12345!`/`Abc1234!`/`Abc12345!` đều bị chặn ở client với popup "Mật khẩu quá yếu!..." và không có request nào gửi lên server; ngược lại `Abc 1234` / `Abc 12345` (có khoảng trắng) được chấp nhận. Hệ quả nặng nhất: **không thể hoàn thành luồng đổi mật khẩu bằng bất kỳ mật khẩu nào thỏa mãn SRS** (mật khẩu hợp lệ duy nhất phải chứa khoảng trắng, không được chứa ký tự đặc biệt). GitHub Issue: [#2](https://github.com/kiterakun/HW02-Domain_Testing/issues/2)
  - (Sai lệch số 1 -- OTP 4 chữ số, và sai lệch số 2 -- thiếu trường xác nhận mật khẩu, cũng đã được xác nhận khi thực thi nhưng không cấu thành Fail của test case nào: OTP 4 chữ số là quy ước demo hiển thị ngay trên màn hình, trường xác nhận mật khẩu bị bỏ qua trong FR03-DT-03.)
- Các xác nhận khác từ thực thi: (1) token dùng một lần -- dùng lại OTP cũ sau reset thành công trả HTTP 400 "Invalid token or email", ngay sau đó OTP mới cùng phiên vẫn đổi thành công; (2) email sai định dạng `abc` hành xử như email chưa đăng ký (HTTP 404 "User not found", không có kiểm tra format phía client); (3) OTP rỗng/mật khẩu rỗng bị thuộc tính `required` của trình duyệt chặn submit ("Please fill out this field.", không gửi request); (4) `abcdefgh` (8 ký tự, thiếu độ phức tạp) bị cảnh báo "Mật khẩu quá yếu!"; (5) OTP thực tế 4 chữ số, hiển thị trực tiếp trên màn hình như mô tả demo; (6) reset thành công điều hướng về trang đăng nhập (`/login`); nút "← Quay lại" về bước 1 hoạt động đúng.
- Nhận xét: Luồng "email hợp lệ → nhận OTP" và "OTP sai → báo lỗi" hoạt động đúng như thiết kế; các lỗi Fail đều bắt nguồn từ 2 sai lệch SRS ↔ mã nguồn đã được báo trước ở 2.1.2/2.1.4 (bây giờ đã được chứng minh bằng thực thi thay vì chỉ đọc mã). Không có test case nào Fail ngoài dự đoán. Sau thực thi, mật khẩu `Test1234!` tại thời điểm chạy bị thay đổi tạm bởi test bổ sung (reset thành công về `Abc 12345`) và đã được khôi phục bằng re-seed CSDL.

### 2.1.7. Đánh giá feature

- **Độ bao phủ lớp miền và biên:** Domain Testing phủ được 5 lớp chính (email hợp lệ, email chưa đăng ký, happy path bước 2, OTP sai, mật khẩu quá ngắn) -- thực thi 5/5; BVA phủ đủ 3 điểm (Min-1/Min/Min+1) của biên duy nhất được xác minh (độ dài tối thiểu 8 ký tự của mật khẩu) -- thực thi 3/3, cộng 3 điểm bổ sung cô lập regex đều Pass. Các lớp còn lại (email sai định dạng, OTP rỗng, mật khẩu rỗng, mật khẩu thiếu độ phức tạp) đã được kiểm chứng bằng 4 kiểm thử bổ sung theo dữ liệu đề xuất ở 2.1.3 và khớp đúng dự đoán; lớp "xác nhận mật khẩu không khớp" xác nhận không thể kiểm thử qua UI do thiếu trường; lớp "OTP đã dùng" được kiểm chứng qua probe dùng lại token sau reset thành công (bị từ chối HTTP 400). Không còn lớp nào đã định nghĩa mà chưa có dữ liệu thực thi tương ứng.
- **Rủi ro còn lại:** môi trường demo hiển thị OTP trực tiếp trên màn hình nên không kiểm thử được việc gửi email thật; SUT không cài đặt thời hạn token nên không thể kiểm thử lớp "OTP hết hạn"; OTP 4 chữ số theo mã nguồn có không gian giá trị nhỏ -- rủi ro bảo mật nằm ngoài phạm vi kiểm thử chức năng nhưng đáng ghi nhận. Ngoài ra, việc kiểm tra mật khẩu chỉ nằm ở frontend (frontend-only regex) đồng nghĩa API `POST /api/reset-password` không có kiểm tra độ phức tạp phía server (chỉ từ chối khi email + token không khớp) -- chưa đưa vào test case do nằm ngoài phạm vi 2 bước của SRS nhưng là rủi ro đáng ghi nhận.
- **Đóng góp hữu ích của AI:** bộ test case bám sát SRS, phân lớp biến rõ ràng (email, resetToken, newPassword, confirmPassword, trạng thái luồng hai bước); happy path và error path có thông báo lỗi cụ thể, dễ đối chiếu khi thực thi.
- **Đầu ra AI chưa chính xác/sai sót**:
  1. **OTP length mismatch:** SRS yêu cầu OTP 6 chữ số, mã nguồn sinh 4 chữ số (`Math.floor(1000 + Math.random() * 9000)` -- `server.js` dòng 72); nhãn UI cũng ghi "Mã OTP (4 số)".
  2. **Missing confirm password field:** UI thực tế (`ForgotPassword.jsx`) không có trường xác nhận mật khẩu mới so với SRS; FR03-DT-03 vẫn yêu cầu dữ liệu `confirmPassword` nên phải điều chỉnh khi thực thi.
  3. **Missing step indicator:** thiếu hiển thị "Bước 1 / 2" trên giao diện; ngoài ra bước 1 không có nút "Quay lại đăng nhập" như SRS (bước 2 có nút "← Quay lại" nhưng chỉ quay về bước 1).
  4. **Strict whitespace regex:** regex kiểm tra mật khẩu trong code bắt buộc khoảng trắng (`(?=.*\s)`) và chỉ cho phép `[A-Za-z\d\s]` (từ chối ký tự đặc biệt), khác SRS (yêu cầu ký tự đặc biệt hoặc khoảng trắng). Hệ quả: các giá trị mật khẩu "hợp lệ" theo SRS dùng trong test case (`Abc12345!`, `Abc1234!`) đều chứa `!` và không có khoảng trắng nên dự kiến bị từ chối khi thực thi.
  5. **Khai báo bao phủ vượt thực tế (phát hiện thêm khi rà soát):** coverage report của AI khai báo đã phủ các lớp (email sai định dạng, OTP rỗng, mật khẩu rỗng, thiếu độ phức tạp) mà test case không có tương ứng -- 4 lớp này đã được người kiểm thử bổ sung dữ liệu và thực thi (khớp dự đoán), nhưng sai sót khai báo của AI vẫn được ghi nhận nguyên trạng.
- **Kết luận sau thực thi:** 4/4 sai lệch SRS ↔ mã nguồn nêu ở đầu báo cáo đều được xác nhận bằng thực thi (OTP 4 chữ số -- quan sát trực tiếp; thiếu trường xác nhận mật khẩu -- kiểm đếm trường bước 2; thiếu chỉ báo bước -- không tìm thấy text "Bước" ở cả 2 bước; regex mật khẩu -- BUG-FR03-02). Kết quả thực thi không có điểm nào trái với dự đoán từ rà soát mã nguồn.

---

## 2.2. FR-09 -- Discount coupons

### 2.2.1. Mô tả chức năng và yêu cầu

- **Mục đích chức năng:** Tại bước Checkout, người dùng nhập mã giảm giá và bấm "Áp dụng". Hệ thống kiểm tra đồng thời 5 điều kiện (C1--C5); nếu tất cả thỏa mãn thì tính tiền giảm theo công thức percent/fixed, hiển thị "Tiết kiệm" / "Thành tiền", và khi thanh toán, đơn hàng được tạo với tổng đã giảm.
- **Tiền điều kiện:** Người dùng đã đăng nhập (JWT hợp lệ); giỏ hàng có tổng tiền xác định; mã giảm giá tồn tại trong CSDL (`coupons`); bản ghi lượt dùng lưu trong bảng `coupon_usage`.
- **Quy tắc giảm giá (đã xác minh từ SRS + mã nguồn):**
  - C1: mã có trong CSDL và `is_active = 1`; C2: ngày hiện tại < `expired_at`; C3: `total_amount >= min_order_amount`; C4: JWT hợp lệ; C5: số lần dùng của user < `max_uses_per_user`.
  - `percent`: `discount_amount = total × discount_value / 100`; `fixed`: `discount_amount = discount_value`; `final_amount = total − discount_amount`.
  - Dữ liệu seed: `SAVE10` (percent 10%, min 300.000, max 1), `BIGBUY` (fixed 50.000, min 500.000, max 1), `VIP100` (fixed 100.000, min 300.000, max 2), `EXPIRED` (percent 20%, min 100.000, exp 2020-01-01, max 1).
- **Nguồn yêu cầu:** SRS FR-09 (v2.0, `eshop-sut/README.md` dòng 110--136); API `eshop-sut/backend/server.js` dòng 363--454; giao diện `eshop-sut/frontend-web/src/pages/Checkout.jsx` (ô input "Nhập mã giảm giá...", nút "Áp dụng", khối "Tiết kiệm"/"Thành tiền").
- **Giả định cần xác minh (đã đối chiếu mã nguồn trước khi chạy, xác nhận qua thực thi):**
  1. Endpoint `POST /api/apply-coupon` không yêu cầu JWT và nhận `user_id` từ body của client (`server.js:363-364`) -- nghi ngờ vi phạm C4/C5.
  2. Điều kiện C3 so sánh `total_amount > min_order_amount` (strict, `server.js:379`) -- nghi ngờ sai so với SRS (`>=`).
  3. Công thức percent trong mã là `Math.floor(total_amount * (1 - discount_value))` (`server.js:398-401, 418-421`) -- nghi ngờ sai hoàn toàn so với `total × value / 100`.
  4. Kiểm tra C2 lồng bên trong nhánh thỏa C3 (`server.js:379-384`) -- nghi ngờ sai thứ tự ưu tiên thông báo lỗi.
  5. `final_amount` không được clamp ≥ 0; checkout lưu nguyên `total_amount` client gửi (`server.js:297-309`) -- ảnh hưởng R8.

### 2.2.2. Domain Testing -- phân tích miền

| Biến/điều kiện | Miền/lớp hợp lệ | Miền/lớp không hợp lệ | Căn cứ | Cần xác minh? |
| --- | --- | --- | --- | --- |
| Mã coupon (`code`) | Khớp chính xác coupon trong CSDL, `is_active=1`, còn hạn: `SAVE10`, `BIGBUY`, `VIP100` | (a) Không tồn tại (`INVALIDCODE`); (b) tồn tại nhưng `is_active=0`; (c) rỗng/toàn khoảng trắng; (d) sai chữ hoa/thường (`save10`); (e) tồn tại + active nhưng hết hạn (`EXPIRED` -- pass C1, fail C2) | SRS FR-09 C1; `server.js:370` (`WHERE code = ? AND is_active = 1`); seed `database.js:106-111` | Có -- đã xác minh qua thực thi |
| Trạng thái coupon | `is_active = 1` | `is_active = 0` (dựng qua Admin API + UPDATE SQLite) | `server.js:370`; FR-17 | Có -- đã xác minh |
| Thời hạn sử dụng (`expired_at` vs `now`) | `now < expired_at` (SAVE10/BIGBUY/VIP100: 2099-12-31) | `now >= expired_at` (EXPIRED: 2020-01-01 → fail C2) | SRS FR-09 C2; `server.js:382` | Có (thứ tự ưu tiên với C3) -- đã xác minh |
| Giá trị giỏ hàng (`total_amount` vs `min_order_amount` M) | `total ≥ M` (300.000 / 500.000 / 100.000 tùy mã) | `total < M` (299.999; 499.999; 100.000 − 1₫...) | SRS FR-09 C3; `server.js:379` | Có (nghi ngờ `>` thay vì `>=`) -- đã xác minh, sai |
| Giới hạn sử dụng (`usage_count` vs `max_uses_per_user`) | `count < max` (SAVE10 max 1: count 0; VIP100 max 2: count 0, 1) | `count ≥ max` (SAVE10: 1; VIP100: 2) | SRS FR-09 C5; `server.js:386-395`; bảng `coupon_usage` | Có (per-user; bypass khi thiếu `user_id`) -- đã xác minh |
| Trạng thái đăng nhập (JWT) | JWT hợp lệ kèm yêu cầu (C4) | Không có token; token sai (`Bearer abc.xyz.123`) | SRS FR-09 C4; `server.js:363` | Có (nghi ngờ endpoint bỏ check) -- đã xác minh, sai |
| Kiểu/mức giảm (`type`, `discount_value`) | percent 1--99% (`SAVE10` 10%); fixed > 0, nhỏ hơn total (`BIGBUY` 50.000) | fixed lớn hơn total → final âm (dựng `FIXBIG`); value ngoài miền chỉ tạo được qua Admin API không validate | SRS FR-09 công thức; `server.js:397-406` | Có (công thức percent; final âm) -- đã xác minh, sai |
| Trạng thái áp dụng/ghi nhận | Áp mã (không trừ lượt) → checkout → `POST /api/coupon-usage` ghi lượt → lần sau bị chặn nếu đạt max | Áp mã lặp lại không tạo lượt; bỏ gọi endpoint ghi lượt → cũng như chưa dùng | `Checkout.jsx:40-66` (gọi usage sau checkout); `server.js:443-454` | Có -- đã xác minh |

**Giải thích phân hoạch miền:** Mỗi mã coupon là một "profile" điều kiện (type, value, min_order_amount, expired_at, max_uses), nên `code` vừa là biến vừa quyết định tham số của các điều kiện còn lại. Các lớp được tách khi kỳ vọng đầu ra khác nhau: success JSON (discount/final) vs từng loại thông báo lỗi (không tồn tại / hết hạn / chưa đủ ngưỡng / đã đạt giới hạn). Lớp biên "total = min" được tách riêng khỏi "total > min" vì SRS quy định `>=` -- đây là điểm nghi ngờ triển khai được kiểm tra riêng qua BVA. Tổ hợp được ưu tiên: (1) single-fault từng điều kiện C1--C5 để xác định thông báo lỗi riêng; (2) biên 3 điểm quanh min_order_amount và max_uses_per_user; (3) tương tác giữa hai lỗi (C2+C3, C1+C3) để quan sát thứ tự ưu tiên; (4) e2e áp mã → checkout → ghi lượt.

### 2.2.3. Test case Domain Testing

| Test Case ID | Lớp miền/điều kiện | Tiền điều kiện | Dữ liệu đầu vào | Các bước thực hiện | Kết quả mong đợi | Kết quả thực tế | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FR09-DT-01 | Coupon percent hợp lệ (all-pass) | Đăng nhập (user 2); SAVE10 chưa dùng | `code=SAVE10, total=500.000` | 1. POST /api/apply-coupon `{code, total_amount, user_id:2}` (payload như frontend) | Thành công; giảm 50.000; thành 450.000; UI "Tiết kiệm 50.000 ₫" | 200 `success:true` nhưng `discount_amount = −4.500.000`, `final_amount = 5.000.000` (tăng ~10 lần), msg "Áp dụng thành công! Giảm 10%" | **Fail (BUG-FR09-01)** |
| FR09-DT-02 | Coupon fixed hợp lệ | User 2; BIGBUY chưa dùng | `code=BIGBUY, total=600.000` | Như DT-01 | Giảm 50.000; thành 550.000 | 200; discount 50.000; final 550.000, msg "Giảm 50,000 ₫" | Pass |
| FR09-DT-03 | Lượt cuối (count = max−1) | User 2 đã dùng VIP100 1 lần (insert qua /api/coupon-usage) | `code=VIP100, total=400.000` | 1. Ghi 1 lượt dùng 2. Áp VIP100 | Đậu (1 < 2); giảm 100.000; thành 300.000 | 200; discount 100.000; final 300.000 | Pass |
| FR09-DT-04 | Mã không tồn tại (C1) | User 2 | `code=INVALIDCODE, total=500.000` | Áp mã | Từ chối "Mã giảm giá không tồn tại hoặc đã bị vô hiệu hóa" | 404 "Mã giảm giá không tồn tại hoặc đã bị vô hiệu hóa" | Pass |
| FR09-DT-05 | Coupon bị vô hiệu (C1) | Tạo `SAVE10OFF` qua Admin API; UPDATE SQLite `is_active=0` (changes=1) | `code=SAVE10OFF, total=500.000` | 1. Xác nhận `is_active=0` qua GET /api/coupons 2. Áp mã | Từ chối như mã không tồn tại | 404 (cùng thông báo) | Pass |
| FR09-DT-06 | Mã rỗng / khoảng trắng | User 2 | `code=""` và `code="   "` | Áp mã (API) | Từ chối, yêu cầu nhập mã | `""`→400 "Vui lòng nhập mã giảm giá" (khớp); `"   "`→404 "không tồn tại" (vẫn từ chối đúng C1, thông điệp khác) | Pass (ghi chú) |
| FR09-DT-07 | Sai chữ hoa/thường | User 2 | `code=save10` (API raw) | Áp mã | Từ chối (không khớp CSDL) | 404 "không tồn tại" | Pass |
| FR09-DT-08 | Mã hết hạn (C2) | User 2; EXPIRED chưa dùng | `code=EXPIRED, total=200.000` (≥ min 100.000) | Áp mã | Từ chối "Mã giảm giá đã hết hạn" | 400 "Mã giảm giá đã hết hạn" | Pass |
| FR09-DT-09 | Đơn đúng bằng ngưỡng (C3 biên) | User 2; SAVE10 chưa dùng | `code=SAVE10, total=300.000` | Áp mã (API) | **Đậu theo SRS `>=`**, giảm 30.000, thành 270.000 | 400 "Đơn hàng chưa đủ giá trị tối thiểu 300,000 ₫" | **Fail (BUG-FR09-02)** |
| FR09-DT-10 | Thiếu đúng 1₫ | User 2 | `code=SAVE10, total=299.999` | Áp mã | Từ chối ngưỡng | 400 "chưa đủ 300,000 ₫" | Pass |
| FR09-DT-11 | Thấp hơn ngưỡng nhiều | User 2 | `code=SAVE10, total=100.000` | Áp mã | Từ chối | 400 "chưa đủ 300,000 ₫" | Pass |
| FR09-DT-12 | Không có JWT (C4) | Chưa xác thực | `POST /api/apply-coupon` `{code=SAVE10, total=500.000}` -- không header Authorization, không user_id | Áp mã (API trực tiếp) | 401 / từ chối theo C4 | 200 `success:true` -- mã được áp "thành công" (C5 cũng bị bỏ qua vì thiếu user_id) | **Fail (BUG-FR09-03, BUG-FR09-04)** |
| FR09-DT-13 | JWT sai (C4) | Token malformed | `Authorization: Bearer abc.xyz.123` | Áp mã | 401/403 | 200 `success:true` (header bị bỏ qua hoàn toàn) | **Fail (BUG-FR09-03)** |
| FR09-DT-14 | Hết lượt max=1 (C5) | User 2 đã dùng SAVE10 1 lần (sau checkout DT-23) | `code=SAVE10, total=500.000, user_id=2` | Áp mã lần nữa | Từ chối "đã đạt giới hạn" | 400 "Bạn đã sử dụng mã này 1 lần (đã đạt giới hạn)" | Pass |
| FR09-DT-15 | Hết lượt max=2 (C5) | User 2 đã dùng VIP100 2 lần (2 insert) | `code=VIP100, total=400.000, user_id=2` | Áp mã | Từ chối "đã đạt giới hạn" | 400 "Bạn đã sử dụng mã này 2 lần (đã đạt giới hạn)" | Pass |
| FR09-DT-16 | BIGBUY đúng bằng ngưỡng | User 2; BIGBUY chưa dùng | `code=BIGBUY, total=500.000` | Áp mã | Đậu theo SRS `>=`; giảm 50.000, thành 450.000 | 400 "chưa đủ giá trị tối thiểu 500,000 ₫" | **Fail (BUG-FR09-02)** |
| FR09-DT-17 | BIGBUY thiếu 1₫ | User 2 | `code=BIGBUY, total=499.999` | Áp mã | Từ chối | 400 "chưa đủ 500,000 ₫" | Pass |
| FR09-DT-18 | 2 lỗi cùng lúc (C2+C3), quan sát ưu tiên | User 2 | `code=EXPIRED, total=50.000` (< min) | Áp mã | Từ chối; ghi nhận thông báo (SRS không cố định ưu tiên) | 400 "chưa đủ giá trị tối thiểu 100,000 ₫" -- **trả lỗi ngưỡng thay vì "hết hạn"** (C2 nằm trong nhánh C3) | Pass (từ chối đúng; sai lệch ưu tiên → BUG-FR09-06) |
| FR09-DT-19 | Chỉ sai hạn (C2) | User 2 | `code=EXPIRED, total=200.000` | Áp mã | 400 "hết hạn" (phân biệt với DT-18) | 400 "Mã giảm giá đã hết hạn" | Pass |
| FR09-DT-20 | C1 ưu tiên (không tồn tại + dưới ngưỡng) | User 2 | `code=INVALIDCODE, total=100.000` | Áp mã | Lỗi C1 (mã không tồn tại), không bị lỗi ngưỡng che | 404 "không tồn tại" | Pass |
| FR09-DT-21 | Fixed > total → final âm | Admin tạo `FIXBIG` (fixed 500.000, min 0); user 2 | `code=FIXBIG, total=100.000` | 1. Tạo coupon 2. Áp mã | Hệ thống không được tạo final âm (từ chối hoặc clamp `discount ≤ total`, final ≥ 0) | 200 `success:true`, `discount_amount=500.000`, `final_amount=−400.000` | **Fail (BUG-FR09-05)** |
| FR09-DT-22 | C5 per-user (user khác) | User A (id 2) đã dùng SAVE10; User B (id 3, đăng ký `couponuser2@eshop.com`) chưa dùng | `code=SAVE10, total=500.000, user_id=3` | Áp mã với user B | Đậu: lượt dùng của B = 0 < 1; giảm 50.000, thành 450.000 | 200 `success:true` (C5 per-user đúng) nhưng amounts sai như DT-01 (discount −4.500.000 / final 5.000.000) | **Fail (BUG-FR09-01; phần C5 đúng)** |
| FR09-DT-23 | e2e: áp mã → checkout → đơn lưu tổng đã giảm | User 2; SAVE10 chưa dùng | `code=SAVE10, total=500.000, user_id=2` | 1. Áp SAVE10 lấy `final_amount` 2. POST /api/checkout (JWT) `{total_amount: final}` 3. GET /api/orders/my-orders 4. POST /api/coupon-usage | Đơn được tạo với tổng 450.000; lượt dùng +1 | Đơn #1 lưu `total_amount=5.000.000` (bằng `final_amount` sai trả về); usage SAVE10 ghi nhận → DT-14 bị chặn đúng | **Fail (BUG-FR09-01 lan vào đơn; cơ chế ghi lượt đúng)** |
| FR09-DT-24 | Dùng lại mã đã dùng sau checkout | Sau DT-23 (count=1) | `code=SAVE10, total=500.000, user_id=2` | Áp mã lần 2 | Từ chối "đã đạt giới hạn" | 400 "…1 lần (đã đạt giới hạn)" | Pass |

**Đánh giá Domain Testing:** 24/24 lớp miền đã thiết kế đều được kiểm tra: Pass 16, Fail 8. Các lớp fail đều do 2 nhóm lỗi SUT: (1) công thức percent sai (DT-01/22/23); (2) C3 dùng `>` thay vì `>=` (DT-09/16); ngoài ra C4 không được thực thi (DT-12/13) và final âm (DT-21). Lớp "hết hạn đúng hôm nay" và "count > max" không đưa vào danh sách chính (lý do tại 2.2.7); lớp "percent > 10% còn hạn" chỉ phủ gián tiếp.

### 2.2.4. BVA -- phân tích biên

| Biến/điều kiện | Biên dưới | Biên trên | Căn cứ xác định biên | Giá trị cần thử | Ghi chú |
| --- | --- | --- | --- | --- | --- |
| Giá trị đơn hàng tối thiểu (SAVE10, min 300.000) | 300.000 | Không có | SRS C3 (`>=`); seed `database.js:107` | min−1 = 299.999; min = 300.000; min+1 = 300.001 | min+1 kèm kiểm tra hành vi làm tròn (10% của 300.001 = 30.000,1) |
| Giá trị đơn hàng tối thiểu (BIGBUY, min 500.000) | 500.000 | Không có | SRS C3; seed `database.js:108` | 499.999; 500.000; 500.001 | -- |
| Giá trị đơn tối thiểu (EXPIRED, min 100.000) | 100.000 | Không có | seed `database.js:110` | Không thử riêng | Mọi total của EXPIRED đều fail C2 trước khi chạm C3 → biên không quan sát được qua hệ thống thực |
| Số lần sử dụng (VIP100, max 2) | count 0 | count 2 | SRS C5 (`count < max`); seed `database.js:109` | count = 0; 1 (max−1); 2 (max) | Dựng count qua 2 lần `POST /api/coupon-usage` |
| Số lần sử dụng (SAVE10, max 1) | count 0 (= max−1 vì max=1) | count 1 | SRS C5 | count = 0; 1 | count=1 dựng tự nhiên qua checkout FR09-DT-23 |
| Thời hạn coupon (`expired_at` vs now) | `now` (còn hạn/hết hạn) | -- | SRS C2 | now ≪ 2099-12-31 (đậu); now ≫ 2020-01-01 (trượt) | Biên `expired_at = now` bỏ qua (ngoài tầm kiểm soát đồng hồ/múi giờ hệ thống) |
| Mức giảm tối đa / final ≥ 0 | final = 0 | -- | Logic nghiệp vụ (final = total − discount không âm) | fixed 500.000 trên đơn 100.000 (`FIXBIG`) | SRS không quy định tường minh; kỳ vọng nghiệp vụ không cho final âm |

**Giải thích BVA:** Hai nhóm biên chính được chọn 3 điểm: ngưỡng đơn hàng (min−1/min/min+1 cho SAVE10 và BIGBUY -- kỳ vọng: min−1 bị từ chối, min và min+1 được chấp nhận theo SRS `>=`) và giới hạn lượt dùng (max−1/max cho VIP100 max=2 và SAVE10 max=1 -- kỳ vọng: max−1 đậu, max bị chặn). Các giá trị biên được đưa trực tiếp qua API vì giao diện chỉ nhận tổng giỏ tổng hợp. Biên thời gian chỉ thử nổi 2 điểm đại diện 2 bên do dữ liệu seed; biên "hết hạn đúng ngày" và biên ngưỡng của EXPIRED bị che bởi điều kiện C2 nên không thiết kế test case, đã ghi Missing trong coverage.

### 2.2.5. Test case BVA

| Test Case ID | Biến/biên kiểm thử | Giá trị | Tiền điều kiện | Các bước thực hiện | Kết quả mong đợi | Kết quả thực tế | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FR09-BVA-01 | SAVE10 ngưỡng -- dưới biên | 299.999 | User 2; count=0 | Áp `SAVE10` total 299.999 | Từ chối "chưa đủ 300.000 ₫" | 400 "chưa đủ giá trị tối thiểu 300,000 ₫" | Pass |
| FR09-BVA-02 | SAVE10 ngưỡng -- tại biên | 300.000 | User 2; count=0 | Áp `SAVE10` total 300.000 | Đậu (`>=`): giảm 30.000, thành 270.000 | 400 "chưa đủ 300,000 ₫" | **Fail (BUG-FR09-02)** |
| FR09-BVA-03 | SAVE10 ngưỡng -- trên biên | 300.001 | User 2; count=0 | Áp `SAVE10` total 300.001 | Đậu; giảm ≈30.000,1 (ghi nhận làm tròn); thành ≈270.001 | 200 nhưng discount **−2.700.009**, final **3.000.010** | **Fail (BUG-FR09-01)** |
| FR09-BVA-04 | BIGBUY ngưỡng -- dưới biên | 499.999 | User 2; count=0 | Áp `BIGBUY` | Từ chối "chưa đủ 500.000 ₫" | 400 "chưa đủ 500,000 ₫" | Pass |
| FR09-BVA-05 | BIGBUY ngưỡng -- tại biên | 500.000 | User 2; count=0 | Áp `BIGBUY` | Đậu (`>=`): giảm 50.000, thành 450.000 | 400 "chưa đủ 500,000 ₫" | **Fail (BUG-FR09-02)** |
| FR09-BVA-06 | BIGBUY ngưỡng -- trên biên | 500.001 | User 2; count=0 | Áp `BIGBUY` | Đậu: giảm 50.000, thành 450.001 | 200; discount 50.000; final 450.001 | Pass |
| FR09-BVA-07 | VIP100 lượt -- dưới biên (count=0) | count 0, total 400.000 | User 2 chưa dùng VIP100 | Áp `VIP100` | Đậu: giảm 100.000, thành 300.000 | 200; discount 100.000; final 300.000 | Pass |
| FR09-BVA-08 | VIP100 lượt -- tại max−1 (count=1) | count 1 | Đã ghi 1 lượt (coupon-usage) | Áp `VIP100` | Đậu (1 < 2): giảm 100.000, thành 300.000 | 200; discount 100.000; final 300.000 | Pass |
| FR09-BVA-09 | VIP100 lượt -- tại max (count=2) | count 2 | Đã ghi 2 lượt | Áp `VIP100` | Từ chối "đã đạt giới hạn" | 400 "Bạn đã sử dụng mã này 2 lần (đã đạt giới hạn)" | Pass |
| FR09-BVA-10 | SAVE10 lượt -- dưới biên (count=0, max=1) | count 0, total 500.000 | User 2 chưa dùng SAVE10 | Áp `SAVE10` | Đậu: giảm 50.000, thành 450.000 | 200 nhưng discount **−4.500.000**, final **5.000.000** | **Fail (BUG-FR09-01)** |
| FR09-BVA-11 | SAVE10 lượt -- tại max (count=1) | count 1 | Sau checkout FR09-DT-23 | Áp `SAVE10` | Từ chối "đã đạt giới hạn" | 400 "…1 lần (đã đạt giới hạn)" | Pass |

> Ánh xạ kiểm thử → thiết kế: FR09-BVA-01…03 = BVA-001; 04…06 = BVA-002; 07…09 = BVA-003; 10…11 = BVA-004 (trong `bva/test-cases.md`).

### 2.2.6. Kết quả thực thi

- Tổng test case Domain Testing: **24** (đã thực thi 24/24)
- Tổng điểm kiểm thử BVA: **11** (gộp trong 4 nhóm BVA-001…004; đã thực thi 11/11)
- Pass: **23** | Fail: **12** | Blocked: **0** | Not Executed: **0**
  - DT: Pass 16 / Fail 8 (DT-01, 09, 12, 13, 16, 21, 22, 23)
  - BVA: Pass 7 / Fail 4 (BVA-02, 03, 05, 10)
- Bug liên quan (chi tiết tại Chương 3):
  - **BUG-FR09-01 (Critical):** công thức percent sai hoàn toàn → discount âm, final tăng ~10× (DT-01/22/23, BVA-03/10) -- GitHub Issue: [#3](https://github.com/kiterakun/HW02-Domain_Testing/issues/3)
  - **BUG-FR09-02 (High):** C3 dùng `>` thay vì `>=` → đơn đúng bằng ngưỡng bị từ chối (DT-09/16, BVA-02/05) -- GitHub Issue: [#4](https://github.com/kiterakun/HW02-Domain_Testing/issues/4)
  - **BUG-FR09-03 (Critical):** C4 không được cài đặt -- áp mã không cần JWT (DT-12/13) -- GitHub Issue: [#5](https://github.com/kiterakun/HW02-Domain_Testing/issues/5)
  - **BUG-FR09-04 (High):** C5 bypass được khi bỏ `user_id` trong body (DT-12 + probe) -- GitHub Issue: [#6](https://github.com/kiterakun/HW02-Domain_Testing/issues/6)
  - **BUG-FR09-05 (Medium):** `final_amount` âm không bị chặn (DT-21) -- GitHub Issue: [#7](https://github.com/kiterakun/HW02-Domain_Testing/issues/7)
  - **BUG-FR09-06 (Low):** sai thứ tự ưu tiên thông báo khi vi phạm C2+C3 (DT-18) -- GitHub Issue: [#8](https://github.com/kiterakun/HW02-Domain_Testing/issues/8)
  - **BUG-FR09-07 (Medium, FR-08 liên quan):** checkout tin tổng tiền client gửi (probe: tạo đơn total=1) -- phát hiện trong quá trình test FR-09 -- GitHub Issue: [#9](https://github.com/kiterakun/HW02-Domain_Testing/issues/9)
- Nhận xét: Kết quả thực tế khớp **100%** với các điểm "Quan sát khi chạy" đã đánh dấu từ bước phân tích mã nguồn (6/6 điểm nghi ngờ đều thành hiện thực). Cơ chế hoạt động đúng: toàn bộ nhánh từ chối C1/C2/C5, công thức fixed, biên max−1/max, tính per-user của C5, thứ tự ưu tiên C1, và việc ghi lượt dùng sau checkout. Sai lệch nghiêm trọng nhất là công thức percent: SAVE10 trên đơn 500.000₫ trả về `discount_amount = −4.500.000` và `final_amount = 5.000.000` -- về bản chất là **tăng giá 10 lần** thay vì giảm 10%, và con số này được ghi nguyên vào đơn hàng (DT-23).

### 2.2.7. Đánh giá feature

- **Độ bao phủ lớp miền và biên:** Domain Testing phủ 22/25 lớp (88%): thiếu "hết hạn đúng hôm nay" (ngoài tầm kiểm soát đồng hồ/múi giờ), "count > max" (chỉ dựng được bằng ghi trực tiếp CSDL; hành vi trùng count = max), "percent 20--99% còn hạn" (không có seed; phủ gián tiếp qua EXPIRED). BVA phủ 5/6 nhóm biên: thiếu biên ngưỡng EXPIRED (bị C2 che) và biên `expired_at = now` (đã nêu).
- **Rủi ro còn lại:**
  1. Bug percent (BUG-FR09-01) là lỗi nghiêm trọng của tính năng: mọi mã percent đang seed (SAVE10, EXPIRED) đều không thể dùng đúng; rủi ro tài chính và uy tín nếu lên production.
  2. Kết hợp BUG-FR09-03 + BUG-FR09-04 + không verify `total_amount`: một caller ẩn danh có thể áp mã bất kỳ, bỏ qua giới hạn lượt, và checkout với tổng tùy ý (probe tạo đơn total=1) -- lỗ hổng kinh doanh/bảo mật ngoài phạm vi SRS nhưng cần ghi nhận.
  3. `coupon_id` không được lưu vào đơn hàng (`orders` không có cột) → không thể truy vết doanh thu giảm giá theo mã; lượt dùng phụ thuộc frontend tự gọi `/api/coupon-usage` sau checkout -- một client khác ý muốn có thể bỏ qua endpoint này để "không bao giờ hết lượt".
  4. DB reset + seed lại mọi khi restart backend: số liệu đếm lượt chỉ có ý nghĩa trong một phiên chạy; không ảnh hưởng test case nhưng ảnh hưởng vận hành demo.
- **Các trường hợp chưa kiểm:** percent 20--99% còn hạn; coupon hết hạn đúng ngày; count > max; áp nhiều mã trong cùng đơn (SRS chỉ hỗ trợ 1 mã/đơn -- không có sẵn UI, bỏ qua); làm tròn chính xác của percent (không thể chạy do BUG-FR09-01 chồng lên mọi kịch bản percent).
- **Đóng góp hữu ích của AI:** phân tích 5 điều kiện C1--C5, bảng lớp tương đương theo "single-fault" và bộ giá trị 3 điểm dày đặc (11 điểm biên/4 nhóm) phát hiện trọn bộ 6 bug FR-09 trong một phiên chạy; các cảnh báo "Quan sát khi chạy" từ rà soát mã nguồn khớp 100% kết quả thực thi, giúp kỳ vọng kiểm thử tách bạch SRS-so-với-SUT.
- **Đầu ra AI chưa chính xác/sai sót** (tổng hợp từ `discount-coupons-feature/ai-mistake.md`):
  1. Step 6: trình bày lặp 3 bản bảng "giá trị đại diện công thức" với ký tự lỗi và nhầm `BIGBUY × 500.000` thành percent 10% -- đã phát hiện, thay bằng bảng chuẩn trước khi thiết kế.
  2. Step 7: file test-cases.md bị 2 dòng text thừa (trước TC-09 và TC-18) -- đã phát hiện và sửa; nội dung 28 test case không bị ảnh hưởng.
- **Phần người kiểm thử đã bổ sung:** xác minh trực tiếp mã nguồn (`server.js`, `database.js`, `Checkout.jsx`) trước khi chạy; thêm 2 probe (bypass C5 bằng bỏ `user_id`; checkout total=1) để xác nhận BUG-FR09-04 và ghi nhận BUG-FR09-07; thiết lập dữ liệu đặc biệt (coupon `is_active=0` qua SQLite; `FIXBIG` qua Admin API) cho các lớp không có seed.

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
| FR14-DT-03 | Create -- tên rỗng (V1-N2) | Có token admin | `POST /api/categories`, body `{"name": ""}` | 1. Gửi POST với tên là chuỗi rỗng | **Từ chối (HTTP 400)** -- FR-14: "không được để trống" | HTTP 200, `{"message": "Category created", "id": 5}` -- danh mục rỗng được lưu | **Fail → BUG-FR14-01** |
| FR14-DT-04 | Create -- thiếu trường name (V1-N3) | Có token admin | `POST /api/categories`, body `{}` | 1. Gửi POST không có trường name | Từ chối (HTTP 400) | HTTP 200, id=6, bản ghi `name = NULL` được lưu | **Fail → BUG-FR14-01** |
| FR14-DT-05 | Create -- tên chỉ toàn khoảng trắng (V1-N4) | Có token admin | `POST /api/categories`, body `{"name": "   "}` | 1. Gửi POST với 3 ký tự khoảng trắng | Từ chối (HTTP 400) -- thực chất là tên rỗng sau trim | HTTP 200, id=7, lưu nguyên chuỗi khoảng trắng | **Fail → BUG-FR14-02** |
| FR14-DT-06 | Create -- tên trùng lặp (V1-N8) | Danh mục "Điện thoại" (id=1) đã tồn tại | `POST /api/categories`, body `{"name": "Điện thoại"}` | 1. Tạo danh mục trùng tên có sẵn<br>2. Gọi GET đối chiếu | Ghi nhận hành vi -- SRS không quy định UNIQUE | HTTP 200, id=8; 2 bản ghi cùng tên "Điện thoại" tồn tại song song | **Pass (theo SRS)** -- ghi nhận rủi ro |
| FR14-DT-07 | Create -- ký tự đặc biệt/emoji (V1-N6) | Có token admin | `POST /api/categories`, body `{"name": "Laptop-Gaming!@# 🎮"}` | 1. Tạo danh mục tên chứa ký tự đặc biệt + emoji<br>2. GET đối chiếu | Chấp nhận (SRS chỉ bắt buộc không rỗng, không cấm ký tự) | HTTP 200, id=9, lưu và hiển thị đúng | **Pass** |
| FR14-DT-08 | Create -- sai kiểu dữ liệu (V1-N7) | Có token admin | `POST /api/categories`, body `{"name": 12345}` (number) | 1. Gửi POST với name là kiểu số | Từ chối (HTTP 400) -- name phải là chuỗi | HTTP 200, id=10; giá trị 12345 lưu thẳng cột TEXT, GET trả name dạng số | **Fail → BUG-FR14-03** |
| FR14-DT-09 | Access Control -- POST không có token (V3-A3) | Không gửi header Authorization | `POST /api/categories`, body `{"name": "No Token Cat"}` | 1. Gửi POST không kèm token | HTTP 401 Unauthorized | HTTP 401, `{"error": "Unauthorized"}` | **Pass** |
| FR14-DT-10 | Access Control -- token sai định dạng (V3-A4) | Token rác `abc.def.ghi` | `POST /api/categories`, token không verify được | 1. Gửi POST với token sai | HTTP 401/403 | HTTP 403, `{"error": "Forbidden"}` | **Pass** |
| FR14-DT-11 | Access Control -- token user thường, POST (V3-A2) | Đăng nhập `test@eshop.com` (role=user) lấy token | `POST /api/categories`, body `{"name": "User Token Cat"}`, token user | 1. Đăng nhập tài khoản user thường<br>2. Gửi POST tạo danh mục bằng token user | **HTTP 403** -- FR-12: POST /api/categories yêu cầu `role='admin'` trong Token (SEC-03) | HTTP 200, `{"message": "Category created", "id": 11}` -- **user thường tạo danh mục thành công** | **Fail → BUG-FR14-04 (Critical)** |
| FR14-DT-12 | Access Control -- DELETE không có token (V3-A3) | Danh mục id=4 đang tồn tại | `DELETE /api/categories/4`, không token | 1. Gửi DELETE không kèm token | HTTP 401 Unauthorized; danh mục còn nguyên | HTTP 401; id=4 không bị xóa | **Pass** |
| FR14-DT-13 | Access Control -- DELETE bằng token user (V3-A2) | Danh mục id=4 tồn tại; token user thường | `DELETE /api/categories/4`, token user | 1. Gửi DELETE danh mục bằng token user thường | HTTP 403 (FR-12 yêu cầu role admin) | HTTP 200, `{"message": "Category deleted"}` -- **danh mục bị user thường xóa thành công** | **Fail → BUG-FR14-04 (Critical)** |
| FR14-DT-14 | Delete -- id không tồn tại (V2-I2) | Có token admin; id 9999 không có trong CSDL | `DELETE /api/categories/9999`, token admin | 1. Xóa danh mục id không tồn tại | HTTP 404 Not Found, thông báo phù hợp | HTTP 200, `{"message": "Category deleted"}` -- báo xóa thành công dù không xóa được gì | **Fail → BUG-FR14-06** |
| FR14-DT-15 | Delete -- danh mục có sản phẩm tham chiếu (V4-S2) | Danh mục id=1 "Điện thoại" có 2 sản phẩm (iPhone 15 Pro Max, Samsung Galaxy S24 Ultra) trỏ tới `category_id = 1` | `DELETE /api/categories/1`, token admin | 1. GET /api/products đếm sản phẩm có category_id=1 (→ 2)<br>2. DELETE danh mục id=1<br>3. GET /api/products kiểm tra lại | Chặn xóa hoặc xử lý tham chiếu -- không để sản phẩm trỏ tới danh mục đã xóa | HTTP 200 xóa thành công; **2 sản phẩm vẫn giữ category_id=1** (dangling reference) | **Fail → BUG-FR14-05** |
| FR14-DT-16 | Update -- admin (ngoài phạm vi SRS, tham chiếu) | Có token admin; SRS FR-14 không yêu cầu Update, API PUT tồn tại ở backend, UI không có | `PUT /api/categories/2`, body `{"name": "Laptop & Notebook"}` | 1. Gửi PUT đổi tên danh mục id=2<br>2. Khôi phục tên "Laptop" sau test | HTTP 200, chỉ danh mục id=2 thay đổi (tham chiếu -- không kết luận theo SRS) | HTTP 200, `{"message": "Category updated"}`; đúng id=2, đã khôi phục | **Pass (tham chiếu)** |
| FR14-DT-17 | Update -- token user (ngoài phạm vi SRS, tham chiếu) | Token user thường | `PUT /api/categories/2`, body `{"name": "Hacked"}` | 1. Gửi PUT bằng token user thường<br>2. Khôi phục tên sau test | HTTP 403 (FR-12: PUT /api/categories yêu cầu role admin) | HTTP 200, `{"message": "Category updated"}` -- **user thường sửa được tên danh mục**; đã khôi phục | **Fail → BUG-FR14-04 (Critical, áp dụng cả PUT)** |
| FR14-DT-18 | Read -- danh sách rỗng (V5-L2) | CSDL seed sạch 3 danh mục (re-start backend để re-seed) | Xóa lần lượt id 1, 2, 3 bằng admin → `GET /api/categories` | 1. Xóa cả 3 danh mục seed<br>2. Gọi GET /api/categories | HTTP 200, mảng rỗng `[]`; UI hiển thị empty state (FR-24) | HTTP 200, `[]` -- API trả mảng rỗng đúng. **Lưu ý UI:** bảng hiển thị trống, không có empty-state thân thiện (icon + message) -- cần đối chiếu bằng mắt trên trình duyệt | **Pass (mức API)** |

**Đánh giá Domain Testing:** 18 test case phủ đủ 22 lớp tương đương đã xác định trên 5 biến (name: 8 lớp; id: 4 lớp; token: 4 lớp; trạng thái tham chiếu sản phẩm: 2 lớp; trạng thái danh sách: 2 lớp). Kết quả: 9 Pass, 9 Fail (trong đó DT-16 Pass tham chiếu ngoài SRS). Tất cả Fail đều là vi phạm thật của SUT so với SRS, tập trung ở 2 nhóm: (1) không validate `name` (rỗng/khoảng trắng/kiểu dữ liệu) vi phạm FR-14; (2) không kiểm tra role admin trên POST/PUT/DELETE vi phạm FR-12/SEC-03 -- nghiêm trọng nhất. Chi tiết bao phủ từng lớp:.

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
| FR14-BVA-01 | Độ dài tên -- Min−1 | `name = ""` (0 ký tự) | Có token admin | 1. Gửi `POST /api/categories` với tên rỗng | HTTP 400 -- từ chối (FR-14: không được để trống) | HTTP 200, `{"message": "Category created", "id": 12}` | **Fail → BUG-FR14-01** |
| FR14-BVA-02 | Độ dài tên -- Min | `name = "X"` (1 ký tự) | Có token admin | 1. Gửi `POST /api/categories` với tên 1 ký tự | HTTP 200 -- chấp nhận (tên ngắn nhất hợp lệ) | HTTP 200, id=13 | **Pass** |
| FR14-BVA-03 | Độ dài tên -- Min+1 | `name = "XY"` (2 ký tự) | Có token admin | 1. Gửi `POST /api/categories` với tên 2 ký tự | HTTP 200 -- chấp nhận | HTTP 200, id=14 | **Pass** |
| FR14-BVA-04 | Miền id DELETE -- dưới biên dưới | `id = 0` | Có token admin | 1. Gửi `DELETE /api/categories/0` | HTTP 404 Not Found | HTTP 200, `{"message": "Category deleted"}` | **Fail → BUG-FR14-06** |
| FR14-BVA-05 | Miền id DELETE -- dưới biên dưới (xa) | `id = −1` | Có token admin | 1. Gửi `DELETE /api/categories/-1` | HTTP 404 Not Found | HTTP 200, `{"message": "Category deleted"}` | **Fail → BUG-FR14-06** |
| FR14-BVA-06 | Miền id DELETE -- trên biên trên | `id = 15` (max tồn tại = 14, nên 15 không tồn tại) | Có token admin; đã GET danh sách xác nhận max id hiện thời | 1. Gửi `DELETE /api/categories/15` | HTTP 404 Not Found | HTTP 200, `{"message": "Category deleted"}` | **Fail → BUG-FR14-06** |
| FR14-BVA-07 | Miền id DELETE -- tại biên trên | `id = 14` (bản ghi cuối đang tồn tại) | Có token admin; danh mục id=14 (`"XY"`) tồn tại | 1. Gửi `DELETE /api/categories/14` | HTTP 200 -- xóa đúng bản ghi cuối | HTTP 200, `{"message": "Category deleted"}` -- xóa đúng bản ghi tồn tại | **Pass** |
| FR14-BVA-08 | Độ dài tên -- biên trên (thăm dò) | `name = "Z" × 10.000` | Có token admin; SRS không định nghĩa max | 1. Gửi POST với tên 10.000 ký tự<br>2. Xóa bản ghi sau test | Chưa xác minh (SRS không quy định) -- ghi nhận hành vi | HTTP 200, chấp nhận tên 10.000 ký tự (SQLite TEXT không giới hạn; UI không có `maxlength`) | **Informational** |

### 2.3.6. Kết quả thực thi

- Tổng test case Domain Testing: **18** (trong đó 2 test tham chiếu ngoài SRS: DT-16, DT-17)
- Tổng test case BVA: **7** chính thức (3-point) + **1** thăm dò (BVA-08)
- Pass: **12** | Fail: **13** | Blocked: 0 | Not Executed: 0
  - Domain Testing: 9 Pass / 9 Fail (DT-16 Pass ở dạng tham chiếu)
  - BVA: 3 Pass / 4 Fail
- Bug liên quan: **6 nhóm lỗi xác nhận thực thi** (BUG-FR14-01 → BUG-FR14-06, chi tiết 2.3.7 và `category-management-feature/ai-mistake.md`):
  - BUG-FR14-01 (Critical): chấp nhận tên rỗng `""` và thiếu trường `name` (NULL) -- DT-03, DT-04, BVA-01 -- GitHub Issue: [#10](https://github.com/kiterakun/HW02-Domain_Testing/issues/10)
  - BUG-FR14-02: chấp nhận tên chỉ toàn khoảng trắng -- DT-05 -- GitHub Issue: [#11](https://github.com/kiterakun/HW02-Domain_Testing/issues/11)
  - BUG-FR14-03: không kiểm tra kiểu dữ liệu (`name = 12345`) -- DT-08 -- GitHub Issue: [#12](https://github.com/kiterakun/HW02-Domain_Testing/issues/12)
  - BUG-FR14-04 (Critical): POST/PUT/DELETE không kiểm tra `role='admin'` -- user thường thao tác được -- DT-11, DT-13, DT-17 -- GitHub Issue: [#13](https://github.com/kiterakun/HW02-Domain_Testing/issues/13)
  - BUG-FR14-05: xóa danh mục đang được sản phẩm tham chiếu, để lại dangling reference -- DT-15 -- GitHub Issue: [#14](https://github.com/kiterakun/HW02-Domain_Testing/issues/14)
  - BUG-FR14-06: DELETE id không tồn tại/ngoài miền trả 200 thay vì 404 -- DT-14, BVA-04/05/06 -- GitHub Issue: [#15](https://github.com/kiterakun/HW02-Domain_Testing/issues/15)
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

---

## 2.4. FR-07 -- Shopping cart (Mobile)

### 2.4.1. Mô tả chức năng và yêu cầu
 
- **Mục đích chức năng:** Người dùng thêm sản phẩm vào giỏ (từ thẻ sản phẩm ở trang chủ hoặc từ màn chi tiết kèm số lượng), xem danh sách dòng giỏ (Sản phẩm, Đơn giá, Số lượng, Thành tiền, Thao tác), chỉnh sửa/xóa dòng, theo dõi tổng tiền và badge "Giỏ (n)" trên header; giỏ trống có empty state.
- **Tiền điều kiện:** App kết nối được back-end; CSDL có sản phẩm seed (iPhone 15 Pro Max 30.000.000 ₫; AirPods Pro 2 6.000.000 ₫; Keychron Q1 4.000.000 ₫). Giỏ hàng là state phía client (`App.js` dòng 62: `const [cart, setCart] = useState([])`) -- **không yêu cầu đăng nhập**; đăng nhập chỉ cần ở Checkout (FR-08).
- **Các trường/trạng thái:** Số lượng thêm (`add_quantity`, mặc định "1"), số lượng sửa trong giỏ (`cart_quantity`), trạng thái giỏ (0/1/≥2 dòng), lặp thêm cùng sản phẩm, thao tác xóa, trạng thái đăng nhập (guest/đã đăng nhập).
- **Nguồn yêu cầu:** SRS v2.0 FR-07 (giỏ hàng: 5 cột, +/−, cộng dồn, dialog xóa, "Tiếp tục mua sắm", nhãn "Tổng cộng", empty state có hình minh họa); FR-06 (số lượng chỉ nhận số nguyên dương, min 1; phản hồi khi thêm); FR-21 (₫ + phân cách hàng nghìn); FR-23 (badge số lượng giỏ); FR-24 (dialog xác nhận xóa, empty state có icon/hình).

**Giả định cần xác minh (đã đối chiếu mã nguồn trước khi chạy -- 6/6 điểm nghi ngờ đều xác nhận qua thực thi):**

1. Nhãn tổng tiền đang là "Tổng tạm tính" (`App.js:638`), không có chuỗi "Tổng cộng" nào trong tệp -- nghi ngờ vi phạm FR-07.
2. `onChangeText` của ô số lượng trong giỏ gán `parsed + 1` (`App.js:617-621`) -- nghi ngờ số lượng luôn lệch +1 so với nhập.
3. Cột Số lượng không có nút +/−, chỉ có TextInput (`App.js:609-624`) -- nghi ngờ vi phạm FR-07.
4. `removeFromCart` (`App.js:155-159`) không có Alert/dialog, nút "Xóa" gọi trực tiếp (`App.js:630`) -- nghi ngờ vi phạm FR-07/FR-24.
5. Khối giỏ trống (`App.js:594-600`) chỉ có Text + link, không có `<Image>` -- nghi ngờ vi phạm FR-07/FR-24.
6. Badge header dùng `cart.length` (số dòng, `App.js:436`) -- FR-23 dùng cụm "số lượng sản phẩm" mơ hồ (đếm dòng hay tổng số lượng).

### 2.4.2. Domain Testing -- phân tích miền

| Biến/điều kiện | Miền/lớp hợp lệ | Miền/lớp không hợp lệ | Căn cứ | Cần xác minh? |
| --- | --- | --- | --- | --- |
| Số lượng thêm `add_quantity` (chi tiết SP / trang chủ) | Số nguyên dương ≥ 1: "1", "2", "5" | (a) "0"; (b) "-1"; (c) rỗng; (d) chữ "abc"; (e) số thực "3.5"; (f) 9999 (không có max trong SRS -- exploratory) | FR-06 "chỉ nhận số nguyên dương, tối thiểu là 1"; `normalizeQuantity` `App.js:129-132` (ép về 1) | Có -- đã xác minh qua thực thi |
| Sản phẩm thêm (`product_id`) | (a) SP mới khi giỏ trống; (b) SP **trùng** dòng đã có (cộng dồn, không tạo dòng mới); (c) SP **khác** (tạo dòng mới) | SP không tồn tại -- không phát sinh qua UI (chỉ thêm từ danh sách API trả về) | FR-07 "Thêm cùng một sản phẩm... tăng số lượng, không tạo dòng mới"; `addToCart` theo `id` (`App.js:136-150`) | Có -- đã xác minh |
| Số lượng sửa trong giỏ `cart_quantity` | Số nguyên dương ≥ 1 | (a) "0"; (b) âm; (c) rỗng; (d) chữ | FR-06 (cùng quy tắc số lượng) | **Có (quan trọng)** -- mã nguồn gán `parsed + 1` (`App.js:617-621`), dự đoán mọi giá trị hợp lệ bị +1 |
| Trạng thái giỏ `cart_state` | 0 dòng (trống) / 1 dòng / ≥2 dòng | Không có (mọi trạng thái đều render) | FR-07 (giỏ có hàng vs empty state); `App.js:594` (`cart.length === 0 ?`) | Có -- đã xác minh |
| Lặp thêm cùng SP `repeat_add` | Lần 1 tạo dòng; lần 2+ cộng dồn | Không có | FR-07; `App.js:140-147` | Có -- đã xác minh |
| Xóa sản phẩm `delete_action` | Xóa khi còn ≥2 dòng (danh sách giữ nguyên còn lại); xóa dòng cuối → empty state | Xóa khi giỏ trống -- UI không hiển thị nút Xóa ở empty state (N/A) | FR-07, FR-24; `removeFromCart` `App.js:155-159` | Có -- đã xác minh |
| Giá và tổng tiền | Thành tiền = Đơn giá × SL; Tổng = Σ Thành tiền; nhãn **"Tổng cộng"**; ₫ + phân cách hàng nghìn | Nhãn sai ("Tổng tạm tính"); sai công thức/currency | FR-07, FR-21; `App.js:75-77, 638`, `formatMoney` dòng 18 | **Có** -- mã nguồn dùng "Tổng tạm tính", dự đoán sai nhãn |
| Trạng thái tồn kho | Không áp dụng | Không áp dụng | Bảng `products` không có cột tồn kho (`database.js:64-71`); SRS không có yêu cầu tồn kho | N/A |
| Trạng thái phiên/tài khoản | Giỏ hoạt động ở cả guest lẫn đã đăng nhập; chỉ Checkout chặn (`App.js:342-347`) | -- | FR-08; giỏ là state client | Có -- đã xác minh |
| Hiển thị trên mobile | Native RN: SafeAreaView, ScrollView danh sách dòng, TextInput số lượng, TouchableOpacity thao tác | Thiếu thành phần bắt buộc theo SRS: nút +/−, hình minh họa empty state, dialog xóa | FR-07/FR-24; mã render `App.js:590-657` | Có -- đã xác minh |

**Giải thích phân hoạch miền:** Miền được phân theo 4 trục hành vi của giỏ (thêm, sửa số lượng, xóa, hiển thị) kết hợp trạng thái giỏ. Các lớp được tách khi kỳ vọng đầu ra khác nhau: tạo dòng mới vs cộng dồn (FR-07 tường minh); giá trị số lượng hợp lệ vs bốn nhóm không hợp lệ (SRS không nói rõ "chặn" hay "ép về min" -- mã nguồn chọn ép về 1, được coi là xử lý an toàn đạt yêu cầu); xóa giữa danh sách vs xóa về 0 dòng (biểu hiện khác nhau khi chuyển empty state). Ưu tiên kiểm tra: (1) các điểm nghi ngờ triển khai có sẵn trong mã (nhãn tổng, +1 khi sửa, thiếu +/−/dialog/hình) vì chạm trực tiếp điều khoản tường minh của FR-07; (2) biên số lượng min−1/min/min+1; (3) chuỗi cộng dồn nhiều lần; (4) chuyển trạng thái 1→0 dòng.

### 2.4.3. Test case Domain Testing

| Test Case ID | Lớp miền/điều kiện | Tiền điều kiện | Dữ liệu đầu vào | Các bước thực hiện | Kết quả mong đợi | Kết quả thực tế | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FR07-DT-01 | Giỏ trống + thêm hợp lệ từ trang chủ (SP-A, qty mặc định 1) | App kết nối API; giỏ trống | SP-A iPhone 15 Pro Max 30.000.000 ₫ | 1. Bấm "Thêm vào giỏ" trên thẻ SP-A<br>2. Mở giỏ | Alert "Đã thêm vào giỏ hàng"; 1 dòng SL=1; Đơn giá/Thành tiền "30.000.000 ₫"; badge cập nhật | Alert "Thành công -- Đã thêm vào giỏ hàng"; Giỏ (1); 1 dòng SL=1; TT 30.000.000 ₫; badge "Giỏ (1)" | **Pass** |
| FR07-DT-02 | Thêm hợp lệ từ chi tiết SP (SP-B, qty=2) | Giỏ trống | SP-B AirPods 6.000.000 ₫; SL "2" | 1. Mở chi tiết SP-B<br>2. Nhập SL=2, bấm "Thêm vào giỏ hàng"<br>3. Mở giỏ | 1 dòng SL=2; Thành tiền "12.000.000 ₫" | 1 dòng SL=2; TT 12.000.000 ₫ | **Pass** |
| FR07-DT-03 | Thêm lại cùng SP (cộng dồn) | Giỏ có SP-A SL=1 | Thêm SP-A lần 2 (qty=1) | 1. Bấm "Thêm vào giỏ" trên thẻ SP-A<br>2. Mở giỏ | **Vẫn 1 dòng**, SL=2; không tạo dòng mới (BR-1) | 1 dòng, SL=2, TT 60.000.000 ₫ | **Pass** |
| FR07-DT-04 | Thêm SP khác (tạo dòng mới độc lập) | Giỏ có SP-A SL=2 | Thêm SP-B | 1. Thêm SP-B từ trang chủ<br>2. Mở giỏ | 2 dòng: A SL=2 giữ nguyên, B SL=1 | 2 dòng đúng; A giữ nguyên SL=2 | **Pass** |
| FR07-DT-05 | `add_quantity` = "0" (invalid: min−1) | Giỏ có SP-B | Chi tiết SP-C; SL "0" | 1. Nhập SL=0<br>2. Thêm vào giỏ | Không thêm 0 sản phẩm -- chặn hoặc xử lý an toàn (theo mã: ép về 1) | Thêm với SL=1 (normalizeQuantity ép về 1, `App.js:129-132`) -- an toàn, không crash | **Pass** |
| FR07-DT-06 | `add_quantity` = "-1" (âm) | Giỏ có SP-B | SL "-1" | Nhập; thêm | Không chèn SL âm; an toàn | SL=1 -- an toàn | **Pass** |
| FR07-DT-07 | `add_quantity` rỗng | Giỏ có SP-B | SL "" | Xóa trắng ô; thêm | Không thêm "0 sản phẩm"; an toàn | parseInt("") = NaN → SL=1 | **Pass** |
| FR07-DT-08 | `add_quantity` = "abc" | Giỏ có SP-B | SL "abc" | Nhập; thêm | Từ chối/an toàn | SL=1 | **Pass** |
| FR07-DT-09 | `add_quantity` = "3.5" (số thực) | Giỏ có SP-B | SL "3.5" | Nhập; thêm | Chỉ nhận số nguyên -- cần chặn hoặc báo lỗi; không thêm theo ý người dùng | parseInt cắt cụt → thêm SL=3 ("3.5" thành 3) -- an toàn nhưng thay đổi ý định người dùng | **Pass (an toàn); lộ sót đáng chú ý → BUG-FR07-07 (Low)** |
| FR07-DT-10 | Cập nhật `cart_quantity` = "5" trong giỏ | Giỏ có SP-B SL=1 | Sửa ô SL thành "5" | 1. Mở giỏ<br>2. Nhập 5<br>3. Quan sát SL/Thành tiền/Tổng | SL=5; Thành tiền "30.000.000 ₫"; tổng cập nhật | **SL=6; Thành tiền 36.000.000 ₫** -- lệch +1 mọi lần sửa | **Fail → BUG-FR07-01** |
| FR07-DT-11 | `cart_quantity` invalid chuỗi ("abc", rỗng, "0", "-1") | Giỏ có SP-B SL=1 | Nhập lần lượt 4 giá trị | Sau mỗi lần nhập kiểm SL/Thành tiền/Tổng | Mỗi giá trị xử lý an toàn; SL không âm/không rỗng lâu dài; không crash | Cả 4 giá trị rơi về SL=1; luôn hiển thị "Thành tiền 6.000.000 ₫" | **Pass** |
| FR07-DT-12 | Xóa dòng cuối (1 → 0 dòng): dialog + empty state | Giỏ có duy nhất dòng SP-C | Bấm "Xóa" dòng SP-C | 1. Mở giỏ<br>2. Bấm "Xóa"<br>3. Quan sát | **Dialog xác nhận trước khi xóa** (FR-24); sau xác nhận: giỏ trống có hình minh họa + thông báo + "Tiếp tục mua sắm"; badge (0) | **Xóa NGAY không dialog (BUG-FR07-04)**; giỏ trống có Text "Giỏ hàng của bạn đang trống" + link "Tiếp tục mua sắm" nhưng **KHÔNG có hình minh họa (BUG-FR07-03)**; badge "Giỏ (0)" | **Fail → BUG-FR07-04, BUG-FR07-03** |
| FR07-DT-13 | Xóa 1 dòng khi giỏ 3 dòng | Giỏ [A SP=1, B SP=1, C SP=1], tổng 40.000.000 ₫ | Xóa dòng SP-B | 1. Xóa B (qua xác nhận nếu có)<br>2. Đối chiếu còn lại + tổng | Còn [A, C] nguyên SL; tổng "34.000.000 ₫" với nhãn "Tổng cộng" | Còn đúng 2 dòng, tổng số 34.000.000 ₫ **đúng**; nhưng nhãn là **"Tổng tạm tính"**; xóa không dialog | **Fail → BUG-FR07-02, BUG-FR07-04** |
| FR07-DT-14 | 5 mục của dòng + công thức Thành tiền/Tổng + nhãn | Giỏ: SP-B SL=2, SP-C SL=1 | Kiểm tra hiển thị từng dòng và tổng | 1. Mở giỏ<br>2. Đối chiếu 2×6.000.000 + 1×4.000.000 = 16.000.000 | Mỗi dòng đủ 5 mục; TT dòng đúng (12.000.000 / 4.000.000); nhãn **"Tổng cộng"** "16.000.000 ₫" | Đủ 5 mục/dòng; TT từng dòng đúng; nhãn sai "Tổng tạm tính"; cột SL không có nút +/− | **Fail → BUG-FR07-02, BUG-FR07-05** |
| FR07-DT-15 | Định dạng tiền giá trị lớn (FR-21) | Giỏ có SP-A SL=1 | Quan sát Đơn giá/Thành tiền/Tổng | Mở giỏ | "30.000.000 ₫" (₫ + phân cách hàng nghìn) | "30,000,000 ₫" theo locale môi trường chạy (en-US) -- đúng ký hiệu ₫ + phân cách nghìn (thiết bị vi-VN sẽ "30.000.000 ₫") | **Pass** |
| FR07-DT-16 | Empty state khi mới mở app (0 dòng) | Giỏ trống chưa thêm gì | -- | 1. Bấm "Giỏ" ngay khi mở app | Hình minh họa + thông báo rõ + "Tiếp tục mua sắm" (FR-07, FR-24) | Chỉ Text + link; **KHÔNG hình minh họa** | **Fail → BUG-FR07-03** |
| FR07-DT-17 | Guest (chưa đăng nhập) dùng giỏ | Không đăng nhập | Thêm SP-A; sửa SL=2; xóa | Toàn bộ thao tác giỏ không token | Giỏ thêm/sửa/xóa bình thường (chỉ Checkout yêu cầu đăng nhập) | Mọi thao tác hoạt động -- giỏ là state client, không kiểm tra token | **Pass** |
| FR07-DT-18 | `add_quantity` = 9999 (exploratory, SRS không có max) | Giỏ có SP-C | Chi tiết SP-A; SL "9999" | Nhập; thêm; mở giỏ | Hệ thống chấp nhận (SRS không chặn) và không crash | Dòng A SL=9999, TT 299.970.000.000 ₫; tổng hiển thị 299.974.000.000 ₫; không crash | **Pass (exploratory)** |
| FR07-DT-19 | Badge "Giỏ (n)" cập nhật sau mỗi thao tác | Giỏ trống | Chuỗi thao tác: thêm A, thêm B, cộng dồn A, xóa 1 dòng | Thực hiện từng bước, quan sát badge sau mỗi bước | Badge phản ánh đúng giỏ sau **mỗi** thao tác (FR-23) | "Giỏ (0)→(1)→(2)→(2)→(1)": cập nhật sau mỗi thao tác nhưng đếm **SỐ DÒNG** -- sau khi cộng dồn A (tổng SL=3) badge vẫn (2) | **Pass (cập nhật có lực sống; độ nghĩa "số lượng" mơ hồ) → BUG-FR07-06 (Low)** |
| FR07-DT-20 | Cột Số lượng có nút +/− (yêu cầu tường minh FR-07) | Giỏ có hàng | Quan sát mã render + UI | Xem cột Số lượng của dòng giỏ | Có nút +/− để chỉnh số lượng | Chỉ có TextInput (`App.js:609-624`); **không có bất kỳ nút +/− nào** | **Fail → BUG-FR07-05** |
| FR07-DT-21 | Xóa phải có dialog xác nhận (FR-07 + FR-24) | Giỏ có hàng | Bấm "Xóa" một dòng | Bấm Xóa; quan sát có hộp thoại trước khi dòng biến mất | Có dialog xác nhận trước khi thực hiện | `onPress={() => removeFromCart(index)}` (`App.js:630`) gọi xóa ngay; trong `removeFromCart` không có Alert/confirm | **Fail → BUG-FR07-04** |
| FR07-DT-22 | Nhãn tổng tiền = "Tổng cộng" (FR-07) | Giỏ có hàng | Đọc nhãn dòng tổng | Mở giỏ; đọc nhãn | Nhãn chính xác "Tổng cộng" (không phải "Tổng tạm tính") | Thực tế "Tổng tạm tính: …" (`App.js:638`); chuỗi "Tổng cộng" không tồn tại trong `App.js` | **Fail → BUG-FR07-02** |
| FR07-DT-23 | "Tiếp tục mua sắm" về trang chủ | Giỏ trống (và giỏ có hàng) | Bấm nút chính về trang chủ | 1. Ở giỏ bấm nút<br>2. Quan sát màn hình | Về trang chủ danh sách sản phẩm | Giỏ trống: "Tiếp tục mua sắm" → `goHome()` đúng; khi giỏ có hàng nút ghi "← Mua tiếp" (cùng hành vi) -- chức năng đúng, nhãn khác SRS (informational) | **Pass** |

**Đánh giá Domain Testing:** 23/23 lớp đã thiết kế đều được thực thi: Pass 15, Fail 8 (DT-10, 12, 13, 14, 16, 20, 21, 22). Các Fail dồn vào 4 nhóm vi phạm điều khoản tường minh của FR-07/FR-24: sửa số lượng bị +1 (BUG-FR07-01), thiếu dialog xóa (BUG-FR07-04), thiếu nút +/− (BUG-FR07-05), nhãn tổng sai + empty state thiếu hình (BUG-FR07-02, BUG-FR07-03). Nhóm đầu vào không hợp lệ (DT-05→08, DT-11) được mã nguồn xử lý an toàn toàn bộ. Lớp "SP không tồn tại" và "xóa khi giỏ trống" không phát sinh qua UI (ghi N/A có căn cứ).

### 2.4.4. BVA -- phân tích biên

| Biến/điều kiện | Biên dưới | Biên trên | Căn cứ xác định biên | Giá trị cần thử | Ghi chú |
| --- | --- | --- | --- | --- | --- |
| B1 -- `add_quantity` (thêm vào giỏ) | min = 1 | **Không có** -- SRS không định nghĩa max; input không chặn | FR-06; `normalizeQuantity` `App.js:129-132` | min−1 = "0"; min = "1"; min+1 = "2"; exploratory "9999" | ⚠️ Mã dự đoán: "0" về 1; "1"/"2" đúng |
| B2 -- `cart_quantity` (sửa trong giỏ) | min = 1 | **Không có** | FR-06; `App.js:615-623` | min−1 = "0"; min = "1"; min+1 = "2" | ⚠️ Mã gán `parsed + 1` → dự đoán: "1"→2, "2"→3, "0"→1 |
| B3 -- `cart_state` (số dòng giỏ) | 0 (trống) | -- (biến rời rạc) | FR-07; `App.js:594` | 0 dòng / 1 dòng / 2 dòng | Biên chuyển đổi 2 nhánh render |
| B4 -- `repeat_add` (số lần thêm cùng SP) | lần 1 (tạo dòng) | -- (biến rời rạc) | FR-07 BR-1; `App.js:140-150` | lần 1 / lần 2 / lần 3 | Biên chuyển "tạo mới" ↔ "cộng dồn" |
| B5 -- Badge "Giỏ (n)" | 0 | -- (biến rời rạc) | FR-23; `App.js:436` | 0 / 1 / 2 | FR-23 mơ hồ: "số lượng sản phẩm" = đếm dòng hay tổng SL |
| B6 -- Xóa -- số dòng còn lại | 0 (giỏ trống) | -- (biến rời rạc) | FR-07, FR-24 | 1→0 / 2→1 / 3→2 | 1→0 phải rơi empty state |
| Định dạng tiền quanh mốc 1.000 ₫ | -- | -- | FR-21 | Không thử | Giá thấp nhất seed 4.000.000 ₫ → thành tiền luôn ≥ 4.000.000: không tạo được 999/1.000/1.001 ₫ qua UI -- ghi Missing trong coverage |

**Giải thích BVA:** Ranh giới số lượng là biên dưới thực chất (min = 1 theo FR-06): tại B1 kỳ vọng "0" bị xử lý an toàn và "1"/"2" được thêm đúng; thực tế cả ba điểm đều đạt vì điểm min−1 được ép về min. Tại B2 (sửa số lượng trong giỏ) kỳ vọng min−1 = "0" vẫn xử lý an toàn (đạt), nhưng min = "1" hiển thị 2 và min+1 = "2" hiển thị 3 -- **sai lệch đúng tại điểm biên và ngay trên biên** do biểu thức `parsed + 1`, xác nhận lỗi bằng phương pháp 3 điểm độc lập. B3/B4/B5/B6 là biến trạng thái rời rạc, phủ bằng 3 trạng thái kề nhau quanh điểm chuyển (0/1/2, lần 1/2/3, 1→0/2→1/3→2). Biên trên số lượng không được SRS định nghĩa nên chỉ thăm dò 9999 (chấp nhận, không crash) và ghi "chưa xác minh" thay vì tự đặt quy tắc.

### 2.4.5. Test case BVA

| Test Case ID | Biến/biên kiểm thử | Giá trị | Tiền điều kiện | Các bước thực hiện | Kết quả mong đợi | Kết quả thực tế | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FR07-BVA-01 | B1 -- min−1 | `add_quantity = "0"` (SP-C 4.000.000) | Giỏ có SP-B | Nhập 0 → "Thêm vào giỏ hàng" → mở giỏ | Không thêm 0 SP -- chặn hoặc an toàn (mã: về 1) | SL=1, TT 4.000.000 ₫ | **Pass** |
| FR07-BVA-02 | B1 -- min | `add_quantity = "1"` | Giỏ trống | Nhập 1, thêm; mở giỏ | SL=1; TT "4.000.000 ₫" | SL=1; TT 4.000.000 ₫ | **Pass** |
| FR07-BVA-03 | B1 -- min+1 | `add_quantity = "2"` | Giỏ trống | Nhập 2, thêm; mở giỏ | SL=2; TT "8.000.000 ₫" | SL=2; TT 8.000.000 ₫ | **Pass** |
| FR07-BVA-04 | B2 -- min−1 | `cart_quantity = "0"` (giỏ có SP-B) | Giỏ có dòng SP-B SL=1 | Nhập 0 vào ô SL trong giỏ | Không chấp nhận 0; an toàn (về 1) | SL=1 | **Pass** |
| FR07-BVA-05 | B2 -- min | `cart_quantity = "1"` | Giỏ có dòng SP-B (6.000.000) | Nhập 1 vào ô SL trong giỏ | SL=1; Thành tiền "6.000.000 ₫" | **SL=2; Thành tiền 12.000.000 ₫** (+1) | **Fail → BUG-FR07-01** |
| FR07-BVA-06 | B2 -- min+1 | `cart_quantity = "2"` | Giỏ có dòng SP-B | Nhập 2 vào ô SL trong giỏ | SL=2; Thành tiền "12.000.000 ₫" | **SL=3; Thành tiền 18.000.000 ₫** (+1) | **Fail → BUG-FR07-01** |
| FR07-BVA-07 | B3 -- điểm 0 (giỏ trống) | 0 dòng | Mới mở app, chưa thêm | Bấm "Giỏ"; quan sát empty state | Hình minh họa + thông báo + "Tiếp tục mua sắm" | Text + link; **không hình minh họa** | **Fail → BUG-FR07-03** |
| FR07-BVA-08 | B3 -- điểm 1 | 1 dòng (SP-A SL=1) | Giỏ có đúng 1 dòng | Mở giỏ | Render 1 dòng đủ 5 mục; "Tổng cộng" = "30.000.000 ₫" | Render + tổng số 30.000.000 ₫ đúng; nhãn "Tổng tạm tính" sai | **Fail → BUG-FR07-02** |
| FR07-BVA-09 | B3 -- điểm 2 | 2 dòng (SP-A + SP-B) | Giỏ có 2 dòng | Mở giỏ | Render 2 dòng; "Tổng cộng" = "36.000.000 ₫" | Render + tổng số 36.000.000 ₫ đúng; nhãn sai | **Fail → BUG-FR07-02** |
| FR07-BVA-10 | B4 -- lần 1 | Thêm SP-B lần 1 | Giỏ trống | Thêm từ trang chủ; mở giỏ | Tạo 1 dòng mới SL=1 | 1 dòng, SL=1 | **Pass** |
| FR07-BVA-11 | B4 -- lần 2 | Thêm SP-B lần 2 | Giỏ có SP-B SL=1 | Thêm lại; mở giỏ | Vẫn 1 dòng, SL=2 (cộng dồn) | 1 dòng, SL=2 | **Pass** |
| FR07-BVA-12 | B4 -- lần 3 | Thêm SP-B lần 3 | Giỏ có SP-B SL=2 | Thêm lần nữa; mở giỏ | Vẫn 1 dòng, SL=3 | 1 dòng, SL=3 | **Pass** |
| FR07-BVA-13 | B5 -- điểm 0 | Giỏ 0 dòng | Giỏ trống | Quan sát badge | "Giỏ (0)" | "Giỏ (0)" | **Pass** |
| FR07-BVA-14 | B5 -- điểm 1 | 1 dòng | Giỏ có SP-A SL=1 | Quan sát badge | Badge phản ánh giỏ -- "Giỏ (1)" | "Giỏ (1)" | **Pass** |
| FR07-BVA-15 | B5 -- điểm 2 | 2 dòng | Giỏ có 2 dòng | Quan sát badge | "Giỏ (2)" | "Giỏ (2)" | **Pass** |
| FR07-BVA-16 | B6 -- 1→0 | Xóa dòng duy nhất (SP-C) | Giỏ đúng 1 dòng | Bấm Xóa; xác nhận nếu có | Dialog rồi empty state có hình; badge (0) | Xóa ngay không dialog (BUG-FR07-04); empty state không hình (BUG-FR07-03); badge (0) | **Fail → BUG-FR07-03, BUG-FR07-04** |
| FR07-BVA-17 | B6 -- 2→1 | Xóa SP-B khỏi [A, B] | Giỏ 2 dòng | Bấm Xóa SP-B | Còn [A]; "Tổng cộng" "30.000.000 ₫" | Còn đúng 1 dòng, tổng số 30.000.000 ₫ đúng; nhãn sai; xóa không dialog | **Fail → BUG-FR07-02, BUG-FR07-04** |
| FR07-BVA-18 | B6 -- 3→2 | Xóa SP-B khỏi [A, B, C] | Giỏ 3 dòng | Bấm Xóa SP-B | Còn [A, C]; "Tổng cộng" "34.000.000 ₫" | Còn đúng 2 dòng, tổng số 34.000.000 ₫ đúng; nhãn sai | **Fail → BUG-FR07-02** |


### 2.4.6. Kết quả thực thi

- Tổng test case Domain Testing: **23** (đã thực thi 23/23)
- Tổng test case BVA: **18** (6 ranh giới, mỗi ranh giới ≥ 3 điểm; đã thực thi 18/18)
- Pass: **25** | Fail: **16** | Blocked: **0** | Not Executed: **0**
  - Domain Testing: Pass 15 / Fail 8 (DT-10, 12, 13, 14, 16, 20, 21, 22)
  - BVA: Pass 10 / Fail 8 (BVA-05→09, 16, 17, 18)
- Bug liên quan (chi tiết tại Chương 3):
  - **BUG-FR07-01 (High):** sửa số lượng trong giỏ luôn cộng thêm 1 (`parsed + 1`) -- nhập 1 ra 2, nhập 5 ra 6; diễn lại tổng tiền sai theo (DT-10, BVA-05, BVA-06) -- GitHub Issue: [#16](https://github.com/kiterakun/HW02-Domain_Testing/issues/16)
  - **BUG-FR07-04 (High):** xóa sản phẩm không có dialog xác nhận (DT-12, DT-21, BVA-16→18) -- GitHub Issue: [#19](https://github.com/kiterakun/HW02-Domain_Testing/issues/19)
  - **BUG-FR07-02 (Medium):** nhãn tổng tiền "Tổng tạm tính" thay vì "Tổng cộng" (DT-13, DT-14, DT-22, BVA-08, BVA-09, BVA-17, BVA-18) -- GitHub Issue: [#17](https://github.com/kiterakun/HW02-Domain_Testing/issues/17)
  - **BUG-FR07-05 (Medium):** cột Số lượng không có nút +/− (DT-14, DT-20) -- GitHub Issue: [#20](https://github.com/kiterakun/HW02-Domain_Testing/issues/20)
  - **BUG-FR07-03 (Medium):** empty state thiếu hình minh họa (DT-12, DT-16, BVA-07, BVA-16) -- GitHub Issue: [#18](https://github.com/kiterakun/HW02-Domain_Testing/issues/18)
  - **BUG-FR07-06 (Low):** badge "Giỏ (n)" đếm số dòng thay vì tổng số lượng; đồng thời FR-23 dùng cụm "số lượng sản phẩm" mơ hồ -- cần làm rõ đặc tả (DT-19, BVA-13→15) -- GitHub Issue: [#21](https://github.com/kiterakun/HW02-Domain_Testing/issues/21)
  - **BUG-FR07-07 (Low):** số thực bị cắt cụt âm thầm ("3.5" → thêm 3) (DT-09) -- GitHub Issue: [#22](https://github.com/kiterakun/HW02-Domain_Testing/issues/22)
  - **BUG-FR07-08 (High -- thuộc FR-08, phát hiện khi rà soát trong quá trình test FR-07):** Checkout gửi `cart.slice(0, -1)` nên khi giỏ > 1 dòng, dòng cuối bị loại khỏi đơn hàng (`App.js:391`) -- GitHub Issue: [#23](https://github.com/kiterakun/HW02-Domain_Testing/issues/23)
- Thiết bị/nền kiểm thử: không dùng thiết bị thật/emulator -- harness logic Node.js v24.10.0 tái lập nguyên văn handler giỏ hàng từ `App.js` + kiểm tra tĩnh render đọc chính `App.js`
- Nhận xét: Kết quả thực tế khớp **100%** với 6/6 điểm "Cần xác minh" đã đánh dấu từ rà soát mã nguồn trước khi chạy. Phần logic số học và luồng thao tác (cộng dồn, xóa, ép số lượng không hợp lệ về 1, tổng tiền = Σ thành tiền, badge có cập nhật) hoạt động đúng và an toàn; toàn bộ 16 Fail nằm ở lớp yêu cầu giao diện/UX được FR-07 và FR-24 tường minh hóa (nhãn, +/−, dialog, hình minh họa) cộng một lỗi dữ liệu duy nhất là BUG-FR07-01 (+1 khi sửa số lượng). Hai điểm nhỏ ghi nhận thêm: tiền đúng định dạng ₫ nhưng phụ thuộc locale thiết bị ("30,000,000" ở locale en-US); nhãn về trang chủ khi giỏ có hàng là "← Mua tiếp" khác SRS nhưng đúng chức năng.

### 2.4.7. Đánh giá feature

- **Độ bao phủ lớp miền và biên:** Domain Testing phủ đủ các lớp đã thiết kế theo 5 biến chính (add_quantity 7 lớp, product_id 3, cart_quantity 5, cart_state 3, repeat_add 2, delete_action 2, auth 1 tường minh + 1 phủ ngầm -- ~96% lớp tương đương, phần còn lại là lớp N/A có căn cứ). BVA phủ 6/6 ranh giới xác định được, mỗi ranh giới đủ 3 điểm; 2 ranh giới không phủ có lý do: (1) biên trên số lượng -- SRS không định nghĩa max, chỉ exploratory 9999; (2) mốc định dạng tiền 1.000 ₫ -- không tạo được giá trị qua UI với dữ liệu seed (giá nhỏ nhất 4.000.000 ₫).
- **Rủi ro còn lại:**
  1. BUG-FR07-01 làm số lượng và tổng tiền sai ngay khi người dùng chỉnh trực tiếp trong giỏ; kết hợp BUG-FR07-08 (FR-08) khi thanh toán còn bị bớt dòng cuối -- luồng tiền có thể sai lệch cả hai phía.
  2. Xóa không xác nhận (BUG-FR07-04) là thao tác hủy dữ liệu không thể hoàn tác trên mobile (màn cảm ứng, dễ chạm nhầm) -- rủi ro trải nghiệm cao dù dữ liệu chỉ tồn tại trong phiên.
  3. Thực thi bằng harness logic + kiểm tra tĩnh thay vì chạy trên thiết bị thật: các khía cạnh chỉ đo được trên UI chạy thật (bố cục, bàn phím số, cảm ứng) chưa được kiểm chứng; nền logic đã được đối chiếu nguyên văn với mã nguồn.
  4. FR-23 mơ hồ ("số lượng sản phẩm") khiến BUG-FR07-06 chưa thể chốt là vi phạm tuyệt đối -- cần làm rõ đặc tả.
- **Các trường hợp chưa kiểm:** badge khi tổng số lượng lớn (> 9, hiển thị tràn hay không); sửa số lượng trong khi Alert còn hiển thị; hành vi giỏ khi mất kết nối API giữa chừng (giỏ là state client nên dự kiến không ảnh hưởng, chưa kiểm chứng); đếm badge theo tổng số lượng (không thể xây qua UI vì mã nguồn cố ý dùng `cart.length`); percent trên mobile (thuộc FR-09, không nằm trong phạm vi FR-07).

---

# 3. AI Gap Analysis and Evaluation

## 3.1. Các test case hoặc lỗi AI bỏ sót

| Gap ID | Feature | Đề xuất ban đầu của AI | Trường hợp bị bỏ sót | Cách phát hiện | Tác động | Cách bổ sung |
| --- | --- | --- | --- | --- | --- | --- |
| GAP-01 | FR-03 | 8 test case (5 DT + 3 BVA) và coverage report khai báo phủ các lớp (email 3/3, resetToken 3/3, newPassword 4/5) | 4 lớp được khai báo phủ nhưng **không có test case riêng**: (1) email sai định dạng; (2) OTP rỗng; (3) mật khẩu rỗng; (4) mật khẩu đủ 8 ký tự nhưng thiếu độ phức tạp | Rà soát thủ công: đối chiếu coverage report với danh sách 8 test case sau khi trình bày | Danh sách test case không phủ trọn các lớp đã phân tích; độ bao phủ bị khai báo vượt thực tế | Người kiểm thử đặt 4 dữ liệu kiểm chứng (`abc`; `resetToken=""`; `newPassword=""`; `"abcdefgh"`) và thực thi bổ sung -- cả 4 khớp dự đoán (404 "User not found"; chặn `required` hai lần; popup "Mật khẩu quá yếu!") |
| GAP-02 | FR-03 | BVA chỉ thiết kế biên độ dài mật khẩu (min = 8 ký tự) | Biên độ dài OTP (SRS: 6 chữ số -- mã nguồn: 4 chữ số, 1000--9999) không được thiết kế 3 điểm (3/4/5 chữ số) | Rà soát phần giải thích BVA (2.1.4) của AI | Vùng giá trị OTP nhỏ liên quan rủi ro bảo mật chưa được kiểm chứng bằng test case | Ghi "chưa xác minh" có căn cứ; xác nhận hành vi bằng quan sát khi thực thi (OTP luôn 4 chữ số); đánh dấu cần làm rõ đặc tả trước khi thiết kế biên |
| GAP-03 | FR-09 | Domain Testing phủ 22/25 lớp (88%), phần còn lại được AI đánh dấu Missing | 3 lớp không có test case: (1) coupon hết hạn đúng hôm nay; (2) `count > max`; (3) percent 20--99% còn hạn (không có seed, mức giảm chỉ được phủ gián tiếp qua EXPIRED) | Rà soát coverage ngay sau bước thiết kế lớp tương đương | Hành vi tại các lớp này chưa được xác minh; đặc biệt mức giảm > 10% chưa từng chạy qua kịch bản còn hạn | Ghi Missing kèm lý do có căn cứ (không kiểm soát được đồng clock/múi giờ; chỉ dựng được bằng ghi trực tiếp CSDL; không có seed) thay vì bỏ qua âm thầm; không tự đặt quy tắc mới |
| GAP-04 | FR-09 | BVA phủ 5/6 nhóm biên (11 điểm 3-point trong 4 nhóm) | 2 biên không phủ: (1) ngưỡng `min_order_amount` của EXPIRED (bị điều kiện C2 che nên không quan sát được); (2) biên `expired_at = now` (ngoài tầm kiểm soát đồng clock hệ thống) | Rà soát bảng phân tích biên trước khi thực thi | Biên thời gian thực của hệ thống không được chấm điểm qua 3 điểm chuẩn | Chỉ thử 2 điểm đại diện hai phía của biên thời gian (2099-12-31: đậu; 2020-01-01: từ chối); ghi Missing có căn cứ trong coverage |
| GAP-05 | FR-14 | BVA chỉ thiết kế biên dưới của độ dài tên (min = 1 ký tự) | Biên trên độ dài tên **chưa xác minh** -- SRS và SUT đều không định nghĩa max (thăm dò 10.000 ký tự vẫn được chấp nhận; UI không có `maxlength`) | Rà soát bảng biên (2.3.4) | Không chốt được hành vi đúng/sai tại mức tối đa -- là khoảng trống đặc tả, không phải lỗi sản phẩm | Thực hiện thăm dò BVA-08 (10.000 ký tự) ở mức Informational; ghi "chưa xác minh" thay vì tự đặt quy tắc max |
| GAP-06 | FR-14 | 18 test DT + 7 test BVA bọc 22 lớp trên 5 biến (name/id/token/tham chiếu/danh sách) | 3 trường hợp ngoài thiết kế: (1) token hết hạn chưa test riêng; (2) `DELETE /api/categories/abc` (id không phải số); (3) UI Admin chưa kiểm bằng mắt trên trình duyệt (form thêm không có `required`, nút Xóa không dialog, empty state khi bảng trống) | Mục "Rủi ro còn lại" của đánh giá feature sau thực thi (2.3.7) | Vùng xác thực mở rộng và trải nghiệm UI thực tế chưa có bằng chứng trực quan | Ghi nhận là rủi ro còn lại có căn cứ; đề xuất bổ sung test id không hợp lệ và chụp màn hình UI trong đợt kiểm thử tiếp theo |
| GAP-07 | FR-07 | BVA phủ 6/6 ranh giới (18 điểm), mỗi ranh giới ≥ 3 điểm | Biên không phủ có căn cứ: (1) biên trên số lượng (SRS không định nghĩa max -- chỉ exploratory 9999); (2) mốc định dạng tiền 1.000 ₫ (giá seed nhỏ nhất 4.000.000 ₫ không tạo được 999/1.000/1.001 ₫ qua UI). Ngoài ra chưa kiểm: badge khi tổng SL > 9; sửa số lượng khi Alert còn hiển thị; giỏ khi mất kết nối API giữa chừng | Rà soát coverage BVA và danh sách "trường hợp chưa kiểm" sau thực thi (2.4.7) | Biên trên và một số hành vi ngoại lệ không được chốt bằng test case | Ghi Missing có căn cứ; chạy exploratory `add_quantity = 9999` (chấp nhận, không crash); phần còn lại đánh dấu chờ kiểm trên thiết bị thật |
| GAP-08 | FR-07 | Thiết kế tập trung vào giỏ hàng; không có test case nối giỏ → checkout | **BUG-108 (High, thuộc FR-08):** Checkout gửi `cart.slice(0, -1)` -- khi giỏ > 1 dòng, dòng cuối bị loại khỏi đơn hàng | Rà soát chéo mã nguồn trong khi thực thi FR-07; không thuộc thiết kế test nào của FR-07 hay FR-08 | Đơn hàng có thể thiếu sản phẩm -- sai cả hiển thị lẫn dữ liệu đơn | Đánh số BUG-108 và ghi nhận thuộc FR-08; đề xuất bổ sung test case luồng giỏ → checkout trong phạm vi FR-08 |

## 3.2. Phân tích nguyên nhân

Phân tích dựa trên các khoảng trống thực tế đã ghi ở 4.1 (GAP-01 → GAP-08) và các sai
sót đã ghi ở 4.2 (AI-01 → AI-13), không dùng nhận định chung:

- **Prompt:**
  - Mâu thuẫn nằm ngay đầu vào: đề bài gọi feature là "Category management (**CRUD**)"
    trong khi SRS FR-14 chỉ yêu cầu Thêm/Xem/Xóa -- chính điểm này khiến AI đặt 2 test
    PUT vào danh sách chính (AI-08) và phải điều chỉnh về "tham chiếu" sau rà soát.
  - SRS dùng từ mơ hồ hoặc im lặng: FR-23 "số lượng sản phẩm" không xác định đếm dòng
    hay tổng số lượng (liên quan BUG-106); không định nghĩa max số lượng, max độ dài
    tên, thời hạn OTP, hành vi "hết hạn đúng ngày" -- khiến một phần biên chỉ có thể
    exploratory hoặc ghi "chưa xác minh" (GAP-02, GAP-05, GAP-07).
  - Yêu cầu trình bày nhiều bảng giá trị trong một bước (FR-09 bước 6) là điều kiện
    trực tiếp sinh lỗi lặp block và dàn trang (AI-03).
- **Ngữ cảnh:**
  - AI hai lần bị trạng thái môi trường đánh lừa: backend sẵn process trên cổng 3000
    với DB không xác định (FR-09) và probe gọi thẳng SUT đang chạy (FR-14) làm chệch
    dữ liệu tiền đề (AI-06) -- phải dừng/khởi động lại để seed sạch và chạy lại.
  - Biên trên của miền id là giá trị **động theo dữ liệu**: AI phải được đưa vào quy
    trình việc đọc max id ngay trước khi chạy (AI-09) thay vì chốt số liệu tĩnh.
  - Không có thiết bị/emulator thật cho FR-07 -- không phải sai sót của AI nhưng là
    giới hạn ngữ cảnh khiến việc thực thi phải dùng harness logic + kiểm tra tĩnh;
    giới hạn này được ghi trung thực trong báo cáo.
  - Mặt tích cực của ngữ cảnh: việc cung cấp SRS **và** mã nguồn ngay từ đầu giúp AI
    đánh dấu trước 22 điểm nghi ngờ ("Cần xác minh"/"Quan sát khi chạy") -- 100% khớp
    kết quả thực thi, kỳ vọng không bị "nhiễm" bởi hành vi sai của sản phẩm.
- **Giới hạn công cụ/mô hình:**
  - Lớp lỗi tái diễn xuyên suốt 4 feature là **soạn thảo khi sinh chuỗi/Markdown dài**:
    bảng vỡ cấu trúc (FR-07 bước 4), lặp block với nội dung lệch nhau (FR-09 bước 6),
    chèn câu thừa giữa các test case (FR-09 bước 7), ký tự CJK rác chèn câu tiếng Việt
    ("tu的还是", "đ切尔", "vì ắt Fah điểm", "Ba dí 0 dòng", "ting tự đặc biệt"). Sửa một
    file tiếng Việt bằng edit nhỏ còn để sót ký tự lỗi (AI-11).
  - "Ảo hóa" dữ liệu khi trình bày bảng giá trị: sinh mã `SAVE4` không tồn tại và nhầm
    loại mã BIGBUY (AI-03) -- mô hình mô phỏng "dạng bảng đẹp" thay vì đọc lại seed.
  - Không có lỗi nào thuộc nhóm này lọt vào nội dung thiết kế hoặc kết quả chính thức
    -- nhờ cơ chế self-check sau khi ghi và rà soát của người kiểm thử.
- **Độ phức tạp của feature:**
  - FR-09: 5 điều kiện C1--C5 tương tác (C2 lồng trong nhánh C3) và mỗi coupon là một
    "profile" tham số -- mật độ bảng giá trị cao vừa là nơi phát sinh lỗi trình bày
    (AI-03), vừa là nơi ẩn 6 bug (BUG-001..006).
  - FR-03: 4 sai lệch SRS ↔ mã nguồn **chồng chéo** (regex chặn cả mật khẩu đúng độ dài)
    che khuất biên độ dài -- phải thiết kế bộ bổ sung để cô lập biến (AI-02).
  - FR-14: nhiều trục trạng thái (name × id × token × tham chiếu) phụ thuộc thứ tự chạy
    -- một probe bẩn làm sai cả đợt thực thi (AI-06).
  - FR-07: nhiều điều khoản UI tường minh cần đối chiếu song song với logic số lượng --
    harness nếu không tái lập nguyên văn sẽ nghiêng kết quả mà không ai thấy (AI-12).
- **Rà soát của con người:**
  - Gap đáng lẽ phát hiện sớm nhất là khai báo coverage vượt thực tế của FR-03
    (GAP-01/AI-01) -- chỉ bắt được ở bước đối chiếu thủ công; quy trình cần thêm bước
    đối chiếu coverage ↔ test case ngay sau thiết kế (kiểm tra 1--1 lớp ↔ test).
  - Giả định 401/403 (AI-07) bị chính bằng chứng thực thi chỉnh lại -- nếu chạy probe
    xác thực sớm trước khi chốt kỳ vọng toàn bộ, có thể tránh điều chỉnh giữa chừng.
  - Cơ chế rà soát hai lớp phát huy tốt: AI self-check bắt phần lớn lỗi soạn thảo
    (AI-04, AI-05, AI-11, AI-13), người kiểm thử duyệt nội dung phân tích (giữ nguyên
    EC1--EC16, thay bảng lặp bằng bảng chuẩn) và bổ sung các kiểm thử mà AI chỉ khai
    báo chứ chưa thiết kế (GAP-01). Hồ sơ FR-09 ghi nhận không có bước nào bị người
    dùng chỉnh yêu cầu -- sai sót đều thuộc phía AI và được phát hiện trong quy trình.

## 3.3. So sánh trước và sau khi rà soát

| Chỉ số | Trước rà soát AI | Sau rà soát của người thực hiện | Giải thích thay đổi |
| --- | --- | --- | --- |
| Số test case được đề xuất | 109 (FR-03: 8; FR-09: 35; FR-14: 25; FR-07: 41) | 109 test chính thức + 12 kiểm thử bổ sung/probe (FR-03: 9; FR-09: 2; FR-14: 1) | Thiết kế lõi của AI được giữ nguyên 100%; phần bổ sung phủ các lớp AI khai báo sẵn mà không có test case (GAP-01), biên chưa xác minh (GAP-05) và probe xác nhận bypass/lỗi lan (GAP-08) |
| Số test case được giữ lại | 109 | 109 (100%), trong đó 2 test PUT FR-14 giữ lại nhưng đổi nhãn "ngoài phạm vi SRS -- tham chiếu" | Không có test case nào bị loại: sai sót của AI nằm ở trình bày, giả định và khai báo coverage, không nằm ở chất lượng phân lớp miền/biên |
| Số test case bổ sung | --- | 12 (FR-03: 3 BVA bổ sung cô lập regex + 4 lớp khai báo thừa + 2 probe dùng lại token/quay lại bước 1; FR-09: 2 probe bypass C5 và checkout `total=1`; FR-14: 1 thăm dò 10.000 ký tự) | Bổ sung để chứng minh gap được lấp; toàn bộ 12 dự đoán và kết quả bổ sung đều khớp khi thực thi |
| Số test case bị sửa/loại | --- | Sửa 3, loại 0: FR03-DT-03 (bỏ dữ liệu `confirmPassword` khi thực thi); FR14-DT-16/17 (đổi nhãn phạm vi) | Đồng bộ thiết kế với kết quả khảo sát mã nguồn (UI không có trường xác nhận) và phạm vi SRS thật (không yêu cầu Update) |
| Số lỗi được xác nhận | 0 bug chính thức -- 22 điểm nghi ngờ được AI đánh dấu trước khi chạy (FR-03: 4 sai lệch SRS↔mã nguồn; FR-09: 6 điểm "Quan sát khi chạy"; FR-14: 6 nhóm vi phạm dự đoán; FR-07: 6 điểm "Cần xác minh") | 23 bug/sai lệch được đánh số và xác nhận bằng thực thi: FR-03: 2 (BUG-03, BUG-04) + 2 sai lệch xác nhận không cấu thành Fail; FR-09: 6 (BUG-001..006) + BUG-007; FR-14: 6 (BUG-01..06); FR-07: 6 (BUG-101..105, BUG-107) + BUG-106 + BUG-108 | 100% điểm nghi ngờ đánh dấu từ mã nguồn đều thành hiện thực khi thực thi; probe bổ sung phát hiện thêm BUG-007; BUG-108 được phát hiện nhờ rà soát chéo ngoài thiết kế test (GAP-08) |

## 3.4. Bài học rút ra

1. **Khai báo coverage phải đi kèm test case tương ứng 1--1:** sau khi phân lớp cần một
   bước đối chiếu coverage report ↔ danh sách test case; lớp nào không có test case phải
   ghi Missing có căn cứ (GAP-01 của FR-03 là ví dụ điển hình -- khai báo thừa chỉ bị
   phát hiện khi rà soát thủ công).
2. **Đừng tin môi trường sẵn có:** luôn xác nhận trạng thái backend/DB trước khi chạy;
   restart để re-seed trước mỗi lượt chính thức; mọi probe phải cô lập dữ liệu hoặc kèm
   bước dọn (bài học CSDL bẩn FR-14 và cổng 3000 của FR-09).
3. **Đọc SRS để chốt phạm vi trước khi thiết kế:** mâu thuẫn "CRUD" (đề bài) ↔
   "Thêm/Xem/Xóa" (SRS FR-14) được xử lý đúng bằng cách gắn nhãn test ngoài phạm vi thành
   "tham chiếu" thay vì tính nhầm thành bug hoặc bỏ âm thầm.
4.  **Rà soát chéo trong khi thực thi:** BUG-108 không nằm trong bất kỳ test case nào
    nhưng được phát hiện nhờ quan sát lân cận phạm vi khi test FR-07 -- nên dành thời
    gian kiểm tra biên giới giữa các feature (giỏ → checkout, coupon → checkout).
5.  **Trách nhiệm cuối cùng thuộc về người kiểm thử:** mọi số liệu của AI được tái xác
    nhận bằng thực thi trên dữ liệu sạch/harness nguyên văn (109/109 test chính thức,
    23/23 bug có bằng chứng); mọi điểm chưa kiểm chứng được ghi rõ "chưa xác minh" thay
    vì trình bày như kết quả -- từ GAP-01 đến AI-13, không có sai sót nào của AI lọt
    đến số liệu chính thức của báo cáo.
---