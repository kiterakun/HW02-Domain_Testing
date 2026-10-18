# HW02 -- Domain Testing on EShop

## Báo cáo áp dụng Domain Testing và Boundary Value Analysis

---

## Thông tin báo cáo

Mục Thông tin

---

Học phần : Software Testing
Bài tập HW02 -- Domain Testing on EShop
Sinh viên : Phạm Vương Quân
Mã số sinh viên : 23120341

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
  - [2.5. Tổng hợp kết quả kiểm thử bốn feature](#25-tổng-hợp-kết-quả-kiểm-thử-bốn-feature)
- [3. AI Gap Analysis and Evaluation](#3-ai-gap-analysis-and-evaluation)
  - [3.1. Các test case hoặc lỗi AI bỏ sót](#31-các-test-case-hoặc-lỗi-ai-bỏ-sót)
  - [3.2. Đầu ra AI chưa chính xác hoặc chưa đầy đủ](#32-đầu-ra-ai-chưa-chính-xác-hoặc-chưa-đầy-đủ)
  - [3.3. Phân tích nguyên nhân](#33-phân-tích-nguyên-nhân)
  - [3.4. So sánh trước và sau khi rà soát](#34-so-sánh-trước-và-sau-khi-rà-soát)
  - [3.5. Bài học rút ra](#35-bài-học-rút-ra)

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

> Không mặc định hệ thống có một quy tắc coupon cụ thể. Hãy kiểm tra mã
> nguồn, giao diện hoặc tài liệu yêu cầu trước khi xác định các miền và
> biên.

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

- **Mục đích chức năng:** \[Mô tả cách người dùng nhập/áp dụng/gỡ
  coupon.\]
- **Tiền điều kiện:** \[Trạng thái giỏ hàng, tài khoản, coupon và đơn
  hàng.\]
- **Quy tắc giảm giá:** \[Ghi đúng quy tắc đã xác minh.\]
- **Nguồn yêu cầu:** \[Tài liệu/mã nguồn/giao diện.\]
- **Giả định cần xác minh:** \[Điều kiện hiệu lực, giới hạn sử dụng,
  kiểu giảm giá...\]

### 2.2.2. Domain Testing -- phân tích miền

---

Biến/điều kiện Miền/lớp hợp Miền/lớp không Căn cứ Cần xác minh?
lệ hợp lệ

---

Mã coupon \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
nguồn\]

Trạng thái \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
coupon nguồn\]

Thời hạn sử \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
dụng nguồn\]

Giá trị giỏ \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
hàng nguồn\]

Giới hạn sử \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
dụng nguồn\]

Kiểu/mức giảm \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
nguồn\]

Trạng thái áp \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
dụng/gỡ coupon nguồn\]

---

**Giải thích phân hoạch miền:** \[Mô tả lớp coupon hợp lệ/không hợp lệ,
trạng thái giỏ hàng và các tổ hợp điều kiện cần kiểm tra.\]

### 2.2.3. Test case Domain Testing

---

Test Case ID Lớp Tiền điều Dữ liệu Các bước Kết quả Kết quả Trạng Bằng chứng
miền/điều kiện đầu vào thực hiện mong đợi thực tế thái  
 kiện

---

FR09-DT-01 \[Coupon \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
hợp lệ\] khi chạy\]  
 chạy\]

FR09-DT-02 \[Coupon \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
không hợp khi chạy\]  
 lệ\] chạy\]

FR09-DT-03 \[Không \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
thỏa điều khi chạy\]  
 kiện nghiệp chạy\]  
 vụ\]

FR09-DT-04 \[Áp \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
dụng/gỡ khi chạy\]  
 hoặc trạng chạy\]  
 thái khác\]

---

**Đánh giá Domain Testing:** \[Các lớp đã kiểm tra, lớp còn thiếu và lý
do.\]

### 2.2.4. BVA -- phân tích biên

---

Biến/điều Biên dưới Biên trên Căn cứ xác Giá trị cần thử Ghi chú
kiện định biên

---

Giá trị đơn \[Điền\] \[Điền\] \[Yêu \[Ngay \[Điền\]
hàng tối cầu/mã dưới/tại/ngay  
 thiểu (nếu nguồn\] trên\]  
 có)

Phần \[Điền\] \[Điền\] \[Yêu \[Ngay \[Điền\]
trăm/mức cầu/mã dưới/tại/ngay  
 giảm nguồn\] trên\]

Mức giảm \[Điền\] \[Điền\] \[Yêu \[Ngay \[Điền\]
tối đa (nếu cầu/mã dưới/tại/ngay  
 có) nguồn\] trên\]

Thời hạn \[Điền\] \[Điền\] \[Yêu \[Trước/tại/sau \[Điền\]
coupon cầu/mã biên thời gian\]  
 nguồn\]

Số lần sử \[Điền\] \[Điền\] \[Yêu \[Dưới/tại/vượt \[Điền\]
dụng (nếu cầu/mã giới hạn\]  
 có) nguồn\]

---

### 2.2.5. Test case BVA

---

Test Case ID Biến/biên Giá trị Tiền điều Các bước Kết quả Kết quả Trạng Bằng chứng
kiểm thử kiện thực hiện mong đợi thực tế thái

---

FR09-BVA-01 \[Ngay dưới \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
biên\] khi chạy\]  
 chạy\]

FR09-BVA-02 \[Tại \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
biên\] khi chạy\]  
 chạy\]

FR09-BVA-03 \[Ngay trên \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
biên\] khi chạy\]  
 chạy\]

---

### 2.2.6. Kết quả thực thi

- Tổng test case Domain Testing: \[ \]
- Tổng test case BVA: \[ \]
- Pass: \[ \] \| Fail: \[ \] \| Blocked: \[ \] \| Not Executed: \[ \]
- Bug liên quan: \[ID/link\]
- Bằng chứng: \[Link/thư mục\]
- Nhận xét: \[Kết quả thực tế, cách xác minh số tiền và giới hạn.\]

### 2.2.7. Đánh giá feature

\[Đánh giá độ bao phủ miền/biên, rủi ro về tính toán tiền, trạng thái
coupon và các trường hợp còn thiếu.\]

---

## 2.3. FR-14 -- Category management (CRUD)

### 2.3.1. Mô tả chức năng và yêu cầu

- **Mục đích chức năng:** \[Mô tả CRUD danh mục.\]
- **Vai trò/quyền truy cập:** \[Vai trò được phép thao tác.\]
- **Các trường dữ liệu:** \[Tên và các trường khác thực tế có trên
  SUT.\]
- **Nguồn yêu cầu:** \[Tài liệu/mã nguồn/giao diện.\]
- **Quy tắc xóa/cập nhật:** \[Ghi theo hành vi đã xác minh.\]

### 2.3.2. Domain Testing -- phân tích miền

---

Biến/điều kiện Miền/lớp hợp Miền/lớp không Căn cứ Cần xác minh?
lệ hợp lệ

---

Tên danh mục \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
nguồn\]

Độ dài tên \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
nguồn\]

Tính duy \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
nhất/tên trùng nguồn\]

Thao tác CRUD \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
nguồn\]

Trạng thái \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
danh mục/sản nguồn\]  
 phẩm liên quan

Quyền người \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
dùng nguồn\]

---

**Giải thích phân hoạch miền:** \[Nêu các lớp dữ liệu và trạng thái cho
Create, Read, Update, Delete; giải thích lớp nào có thể gây ra kết quả
khác nhau.\]

### 2.3.3. Test case Domain Testing

---

Test Case ID Thao Tiền điều Dữ liệu Các bước Kết quả Kết quả Trạng Bằng chứng
tác/lớp kiện đầu vào thực hiện mong đợi thực tế thái  
 miền

---

FR14-DT-01 Create -- \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
dữ liệu hợp khi chạy\]  
 lệ chạy\]

FR14-DT-02 Create -- \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
dữ liệu khi chạy\]  
 không hợp chạy\]  
 lệ

FR14-DT-03 Read -- \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
có/không có khi chạy\]  
 dữ liệu chạy\]

FR14-DT-04 Update -- \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
dữ liệu hợp khi chạy\]  
 lệ/không chạy\]  
 hợp lệ

FR14-DT-05 Delete -- \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
điều kiện khi chạy\]  
 xóa chạy\]

FR14-DT-06 Quyền truy \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
cập/trạng khi chạy\]  
 thái liên chạy\]  
 quan

---

### 2.3.4. BVA -- phân tích biên

---

Biến/điều Biên dưới Biên trên Căn cứ xác Giá trị cần Ghi chú
kiện định biên thử

---

Độ dài tên \[Điền nếu xác \[Điền nếu \[Yêu \[min−1, \[Điền\]
danh mục minh được\] xác minh cầu/mã min,  
 được\] nguồn\] min+1...\]

Trường bắt \[Rỗng/không \[Không áp \[Yêu \[Rỗng, tối \[Điền\]
buộc rỗng theo quy dụng nếu cầu/mã thiểu hợp  
 tắc\] không có nguồn\] lệ...\]  
 biên trên\]

Số lượng \[Nếu có quy \[Nếu có \[Yêu \[Giá trị \[Điền\]
bản tắc\] quy tắc\] cầu/mã sát biên\]  
 ghi/trạng nguồn\]  
 thái liên  
 quan

---

### 2.3.5. Test case BVA

---

Test Case ID Biến/biên Giá trị Tiền điều Các bước Kết quả Kết quả Trạng Bằng chứng
kiểm thử kiện thực hiện mong đợi thực tế thái

---

FR14-BVA-01 \[Dưới \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
biên\] khi chạy\]  
 chạy\]

FR14-BVA-02 \[Tại \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
biên\] khi chạy\]  
 chạy\]

FR14-BVA-03 \[Trên \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
biên\] khi chạy\]  
 chạy\]

---

### 2.3.6. Kết quả thực thi

- Tổng test case Domain Testing: \[ \]
- Tổng test case BVA: \[ \]
- Pass: \[ \] \| Fail: \[ \] \| Blocked: \[ \] \| Not Executed: \[ \]
- Bug liên quan: \[ID/link\]
- Bằng chứng: \[Link/thư mục\]
- Nhận xét: \[Kết quả CRUD, kiểm tra dữ liệu lưu và trạng thái sau
  thao tác.\]

### 2.3.7. Đánh giá feature

\[Đánh giá độ bao phủ các thao tác CRUD, miền dữ liệu, biên độ dài và
các điều kiện xóa/cập nhật.\]

---

## 2.4. FR-07 -- Shopping cart (Mobile)

### 2.4.1. Mô tả chức năng và yêu cầu

- **Nền tảng được kiểm thử:** \[Ứng dụng mobile/web responsive/thiết
  bị giả lập.\]
- **Mục đích chức năng:** \[Mô tả phạm vi giỏ hàng.\]
- **Tiền điều kiện:** \[Trạng thái đăng nhập, sản phẩm và dữ liệu ban
  đầu.\]
- **Các trường/trạng thái:** \[Sản phẩm, số lượng, đơn giá, tổng tiền,
  giỏ rỗng...\]
- **Nguồn yêu cầu:** \[Tài liệu/mã nguồn/giao diện.\]
- **Giới hạn nền tảng:** \[Thiết bị, kích thước màn hình, trình
  duyệt/OS.\]

### 2.4.2. Domain Testing -- phân tích miền

---

Biến/điều kiện Miền/lớp hợp Miền/lớp không Căn cứ Cần xác minh?
lệ hợp lệ

---

Trạng thái giỏ \[Rỗng/có sản \[Điền\] \[Yêu cầu/mã \[Có/Không\]
hàng phẩm...\] nguồn\]

Số lượng sản \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
phẩm nguồn\]

Trạng thái tồn \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
kho nguồn\]

Giá và tổng \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
tiền nguồn\]

Thao tác \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
thêm/cập nguồn\]  
 nhật/xóa

Trạng thái \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
phiên/tài nguồn\]  
 khoản (nếu ảnh  
 hưởng)

Hiển thị trên \[Điền\] \[Điền\] \[Yêu cầu/giao \[Có/Không\]
mobile diện\]

---

**Giải thích phân hoạch miền:** \[Mô tả các lớp đại diện cho trạng thái
giỏ hàng, số lượng, tồn kho và thao tác; nêu điều kiện nào được kiểm tra
riêng trên mobile.\]

### 2.4.3. Test case Domain Testing

---

Test Case ID Lớp miền/điều Tiền điều Dữ liệu Các bước Kết quả Kết quả Trạng Bằng chứng
kiện kiện đầu vào thực hiện mong đợi thực tế thái

---

FR07-DT-01 Giỏ hàng rỗng \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
khi chạy\]  
 chạy\]

FR07-DT-02 Thêm sản phẩm \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
hợp lệ khi chạy\]  
 chạy\]

FR07-DT-03 Cập nhật số \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
lượng khi chạy\]  
 chạy\]

FR07-DT-04 Số \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
lượng/trạng khi chạy\]  
 thái không chạy\]  
 hợp lệ

FR07-DT-05 Xóa sản phẩm \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
khi chạy\]  
 chạy\]

FR07-DT-06 Tồn kho/giá \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
thay đổi (nếu khi chạy\]  
 hỗ trợ) chạy\]

FR07-DT-07 Hiển thị và \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
thao tác trên khi chạy\]  
 mobile chạy\]

---

### 2.4.4. BVA -- phân tích biên

---

Biến/điều Biên dưới Biên trên Căn cứ xác Giá trị cần thử Ghi chú
kiện định biên

---

Số lượng sản \[min\] \[max nếu \[Yêu \[min−1, min, \[Điền\]
phẩm có\] cầu/mã min+1, max−1,  
 nguồn\] max, max+1\]

Tồn kho \[0 hoặc \[Số lượng \[Yêu \[Sát biên tồn \[Điền\]
giới hạn tồn nếu cầu/mã kho\]  
 thực tế\] có\] nguồn\]

Tổng \[Nếu có \[Nếu có \[Yêu \[Sát biên\] \[Điền\]
tiền/giá trị giới hạn\] giới hạn\] cầu/mã  
 số lượng nguồn\]

Kích thước \[Nếu có \[Nếu có \[Thiết \[Ngay \[Điền\]
màn yêu cầu\] yêu cầu\] kế/giao dưới/tại/ngay  
 hình/điểm diện\] trên điểm gãy\]
gãy  
 responsive

---

### 2.4.5. Test case BVA

---

Test Case ID Biến/biên Giá trị Tiền điều Các bước Kết quả Kết quả Trạng Bằng chứng
kiểm thử kiện thực hiện mong đợi thực tế thái

---

FR07-BVA-01 \[Số lượng \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
dưới biên\] khi chạy\]  
 chạy\]

FR07-BVA-02 \[Số lượng \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
tại biên\] khi chạy\]  
 chạy\]

FR07-BVA-03 \[Số lượng \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
trên biên\] khi chạy\]  
 chạy\]

FR07-BVA-04 \[Biên tồn \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
kho hoặc khi chạy\]  
 màn hình\] chạy\]

---

### 2.4.6. Kết quả thực thi

- Tổng test case Domain Testing: \[ \]
- Tổng test case BVA: \[ \]
- Pass: \[ \] \| Fail: \[ \] \| Blocked: \[ \] \| Not Executed: \[ \]
- Bug liên quan: \[ID/link\]
- Thiết bị/kích thước màn hình đã thử: \[ \]
- Bằng chứng: \[Link/thư mục\]
- Nhận xét: \[Kết quả nghiệp vụ và hiển thị/thao tác trên mobile.\]

### 2.4.7. Đánh giá feature

\[Đánh giá độ bao phủ miền/biên, tính đúng đắn của số lượng/tổng tiền,
trạng thái giỏ và các vấn đề riêng của mobile.\]

---

## 2.5. Tổng hợp kết quả kiểm thử bốn feature

---

Feature Domain BVA test Đã thực Pass Fail Blocked Not Bug xác
test case thi Executed nhận
case thiết kế  
 thiết kế

---

FR-03 -- Forgot \[ \] \[ \] \[ \] \[ \] \[ \] \[ \] \[ \] \[ \]
password/reset

FR-09 -- \[ \] \[ \] \[ \] \[ \] \[ \] \[ \] \[ \] \[ \]
Discount coupons

FR-14 -- \[ \] \[ \] \[ \] \[ \] \[ \] \[ \] \[ \] \[ \]
Category CRUD

FR-07 -- \[ \] \[ \] \[ \] \[ \] \[ \] \[ \] \[ \] \[ \]
Shopping cart  
 (Mobile)

**Tổng** **\[ **\[ **\[ **\[ **\[ **\[ \]\*\* **\[ \]** **\[
\]** \]** \]** \]** \]** \]\*\*

---

**Nhận xét tổng hợp:** \[So sánh khả năng phát hiện trường hợp giữa
Domain Testing và BVA; nêu feature có độ bao phủ thấp và lý do.\]

---

# 3. AI Gap Analysis and Evaluation

## 3.1. Các test case hoặc lỗi AI bỏ sót

---

Gap ID Feature Đề xuất Trường hợp Cách phát Tác động Cách bổ
ban đầu bị bỏ sót hiện sung
của AI

---

GAP-01 \[FR-xx\] \[Tóm \[Mô tả\] \[Rà \[Ảnh \[Cách
tắt\] soát/thực hưởng\] sửa\]
thi\]

---

## 3.2. Đầu ra AI chưa chính xác hoặc chưa đầy đủ

---

AI output Feature Nội dung AI Vấn đề phát hiện Bằng chứng Chỉnh sửa
ID đưa ra đối chiếu của người
kiểm thử

---

AI-01 \[FR-xx\] \[Tóm tắt, \[Sai/thiếu/giả \[Yêu \[Cách
không cần định\] cầu/mã sửa\]
chép dài\] nguồn/kết  
 quả chạy\]

---

## 3.3. Phân tích nguyên nhân

Phân tích từng khoảng trống theo các khía cạnh phù hợp:

- **Prompt:** yêu cầu chưa đủ rõ, thiếu ràng buộc hoặc chưa cung cấp
  quy tắc nghiệp vụ.
- **Ngữ cảnh:** AI không được cung cấp mã nguồn, dữ liệu hoặc trạng
  thái SUT cần thiết.
- **Giới hạn công cụ/mô hình:** AI suy luận từ mẫu phổ biến thay vì
  bằng chứng cụ thể.
- **Độ phức tạp của feature:** nhiều điều kiện tương tác, trạng thái
  hoặc bước chuyển tiếp.
- **Rà soát của con người:** trường hợp nào đáng lẽ có thể phát hiện
  sớm hơn và cách cải thiện quy trình.

\[Viết phân tích dựa trên các gap thực tế đã ghi ở mục 4.1 và 4.2; không
dùng nhận định chung thay cho bằng chứng.\]

## 3.4. So sánh trước và sau khi rà soát

---

Chỉ số Trước rà soát AI Sau rà soát của Giải thích thay
người thực hiện đổi

---

Số test case \[ \] \[ \] \[ \]
được đề xuất

Số test case \[ \] \[ \] \[ \]
được giữ lại

Số test case bổ --- \[ \] \[ \]
sung

Số test case bị --- \[ \] \[ \]
sửa/loại

Số lỗi được xác \[ \] \[ \] \[ \]
nhận

---

## 3.5. Bài học rút ra

\[Trình bày bài học cụ thể về cách đặt prompt theo từng bước kỹ thuật,
kiểm chứng giới hạn từ nguồn có căn cứ, rà soát đầu ra AI và giữ trách
nhiệm cuối cùng của người kiểm thử.\]

---