# HW02 -- Domain Testing on EShop

## Báo cáo áp dụng Domain Testing và Boundary Value Analysis

> **Trạng thái tài liệu:** Sườn báo cáo để hoàn thiện. Các phần trong
> dấu `[ ... ]` cần được thay bằng thông tin và kết quả thực tế. Không
> điền trước kết quả kiểm thử, lỗi, ảnh minh chứng hoặc số liệu khi chưa
> thực hiện.

---

## Thông tin báo cáo

Mục Thông tin

---

Học phần \[Tên học phần\]
Bài tập HW02 -- Domain Testing on EShop
Sinh viên \[Họ và tên\]
Mã số sinh viên \[MSSV\]
Nhóm \[Tên/số nhóm\]
SUT EShop -- ứng dụng thương mại điện tử demo
Repository SUT https://github.com/ttbhanh/eshop-sut

## Các feature được lựa chọn

---

Feature Pool theo đề bài Phạm vi kiểm thử dự kiến

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
  - [1.3. Môi trường và công cụ](#13-môi-trường-và-công-cụ)
  - [1.4. Quy trình Domain Testing](#14-quy-trình-domain-testing)
  - [1.5. Quy trình Boundary Value Analysis (3 điểm)](#15-quy-trình-boundary-value-analysis-3-điểm)
  - [1.6. Quy trình thực thi và quản lý bằng chứng](#16-quy-trình-thực-thi-và-quản-lý-bằng-chứng)
  - [1.7. Quy ước trạng thái kiểm thử](#17-quy-ước-trạng-thái-kiểm-thử)
- [2. Thiết kế và thực thi kiểm thử](#2-thiết-kế-và-thực-thi-kiểm-thử)
  - [2.1. FR-03 -- Forgot password and password reset](#21-fr-03----forgot-password-and-password-reset)
    - [2.1.1. Mô tả chức năng và yêu cầu](#211-mô-tả-chức-năng-và-yêu-cầu)
    - [2.1.2. Domain Testing -- phân tích miền](#212-domain-testing----phân-tích-miền)
    - [2.1.3. Test case Domain Testing](#213-test-case-domain-testing)
    - [2.1.4. BVA -- phân tích biên](#214-bva----phân-tích-biên)
    - [2.1.5. Test case BVA](#215-test-case-bva)
    - [2.1.6. Kết quả thực thi và bằng chứng](#216-kết-quả-thực-thi-và-bằng-chứng)
    - [2.1.7. Đánh giá feature](#217-đánh-giá-feature)
  - [2.2. FR-09 -- Discount coupons](#22-fr-09----discount-coupons)
    - [2.2.1. Mô tả chức năng và yêu cầu](#221-mô-tả-chức-năng-và-yêu-cầu)
    - [2.2.2. Domain Testing -- phân tích miền](#222-domain-testing----phân-tích-miền)
    - [2.2.3. Test case Domain Testing](#223-test-case-domain-testing)
    - [2.2.4. BVA -- phân tích biên](#224-bva----phân-tích-biên)
    - [2.2.5. Test case BVA](#225-test-case-bva)
    - [2.2.6. Kết quả thực thi và bằng chứng](#226-kết-quả-thực-thi-và-bằng-chứng)
    - [2.2.7. Đánh giá feature](#227-đánh-giá-feature)
  - [2.3. FR-14 -- Category management (CRUD)](#23-fr-14----category-management-crud)
    - [2.3.1. Mô tả chức năng và yêu cầu](#231-mô-tả-chức-năng-và-yêu-cầu)
    - [2.3.2. Domain Testing -- phân tích miền](#232-domain-testing----phân-tích-miền)
    - [2.3.3. Test case Domain Testing](#233-test-case-domain-testing)
    - [2.3.4. BVA -- phân tích biên](#234-bva----phân-tích-biên)
    - [2.3.5. Test case BVA](#235-test-case-bva)
    - [2.3.6. Kết quả thực thi và bằng chứng](#236-kết-quả-thực-thi-và-bằng-chứng)
    - [2.3.7. Đánh giá feature](#237-đánh-giá-feature)
  - [2.4. FR-07 -- Shopping cart (Mobile)](#24-fr-07----shopping-cart-mobile)
    - [2.4.1. Mô tả chức năng và yêu cầu](#241-mô-tả-chức-năng-và-yêu-cầu)
    - [2.4.2. Domain Testing -- phân tích miền](#242-domain-testing----phân-tích-miền)
    - [2.4.3. Test case Domain Testing](#243-test-case-domain-testing)
    - [2.4.4. BVA -- phân tích biên](#244-bva----phân-tích-biên)
    - [2.4.5. Test case BVA](#245-test-case-bva)
    - [2.4.6. Kết quả thực thi và bằng chứng](#246-kết-quả-thực-thi-và-bằng-chứng)
    - [2.4.7. Đánh giá feature](#247-đánh-giá-feature)
  - [2.5. Tổng hợp kết quả kiểm thử bốn feature](#25-tổng-hợp-kết-quả-kiểm-thử-bốn-feature)
- [3. Báo cáo lỗi (Bug Report)](#3-báo-cáo-lỗi-bug-report)
  - [3.1. Quy trình phát hiện và xác nhận lỗi](#31-quy-trình-phát-hiện-và-xác-nhận-lỗi)
  - [3.2. Danh sách lỗi tổng hợp](#32-danh-sách-lỗi-tổng-hợp)
  - [3.3. Chi tiết từng lỗi](#33-chi-tiết-từng-lỗi)
    - [BUG-\[ID\]: \[Tiêu đề ngắn, mô tả đúng hiện tượng\]](#bug-id-tiêu-đề-ngắn-mô-tả-đúng-hiện-tượng)
  - [3.4. Tổng kết lỗi](#34-tổng-kết-lỗi)
- [4. AI Gap Analysis and Evaluation](#4-ai-gap-analysis-and-evaluation)
  - [4.1. Các test case hoặc lỗi AI bỏ sót](#41-các-test-case-hoặc-lỗi-ai-bỏ-sót)
  - [4.2. Đầu ra AI chưa chính xác hoặc chưa đầy đủ](#42-đầu-ra-ai-chưa-chính-xác-hoặc-chưa-đầy-đủ)
  - [4.3. Phân tích nguyên nhân](#43-phân-tích-nguyên-nhân)
  - [4.4. So sánh trước và sau khi rà soát](#44-so-sánh-trước-và-sau-khi-rà-soát)
  - [4.5. Bài học rút ra](#45-bài-học-rút-ra)
- [5. Agent Skills và tự động hóa](#5-agent-skills-và-tự-động-hóa)
  - [5.1. Mục tiêu](#51-mục-tiêu)
  - [5.2. Thiết kế Skill cho Domain Testing](#52-thiết-kế-skill-cho-domain-testing)
  - [5.3. Thiết kế Skill cho BVA](#53-thiết-kế-skill-cho-bva)
  - [5.4. Cách sử dụng và đầu vào/đầu ra](#54-cách-sử-dụng-và-đầu-vàođầu-ra)
  - [5.5. Minh họa chạy Skill trên feature](#55-minh-họa-chạy-skill-trên-feature)
  - [5.6. Đánh giá hiệu quả và giới hạn](#56-đánh-giá-hiệu-quả-và-giới-hạn)
  - [5.7. Video demo và liên kết](#57-video-demo-và-liên-kết)
- [6. Test Summary Report](#6-test-summary-report)
  - [6.1. Tóm tắt phạm vi](#61-tóm-tắt-phạm-vi)
  - [6.2. Thống kê test case](#62-thống-kê-test-case)
  - [6.3. Thống kê lỗi](#63-thống-kê-lỗi)
  - [6.4. Đánh giá mức độ hoàn thành](#64-đánh-giá-mức-độ-hoàn-thành)
  - [6.5. Hạn chế và rủi ro còn lại](#65-hạn-chế-và-rủi-ro-còn-lại)
  - [6.6. Kết luận và đề xuất](#66-kết-luận-và-đề-xuất)
- [Tài liệu tham khảo](#tài-liệu-tham-khảo)
- [Phụ lục](#phụ-lục)
  - [A. AI Audit Report](#a-ai-audit-report)
  - [B. AI Critique](#b-ai-critique)
  - [C. Git Commit Log](#c-git-commit-log)
  - [D. Bộ test case đầy đủ](#d-bộ-test-case-đầy-đủ)
  - [E. Ảnh và bằng chứng kiểm thử](#e-ảnh-và-bằng-chứng-kiểm-thử)
  - [F. Agent Skills và hướng dẫn sử dụng](#f-agent-skills-và-hướng-dẫn-sử-dụng)
- [Checklist trước khi nộp](#checklist-trước-khi-nộp)

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

## 1.3. Môi trường và công cụ

Hạng mục Thông tin cần ghi

---

Phiên bản/mã commit SUT \[Commit hash hoặc phiên bản\]
Hệ điều hành \[Windows/macOS/Linux/Android...\]
Trình duyệt/thiết bị \[Tên và phiên bản\]
Cơ sở dữ liệu/dữ liệu thử nghiệm \[Thông tin phù hợp\]
Công cụ kiểm thử \[Thủ công/automation và tên công cụ\]
Công cụ AI \[Tên mô hình/công cụ và phiên bản nếu biết\]
Quản lý lỗi GitHub Issues: \[URL repository/issues\]
Quản lý mã nguồn \[URL repository/branch\]

## 1.4. Quy trình Domain Testing

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

## 1.5. Quy trình Boundary Value Analysis (3 điểm)

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

## 1.6. Quy trình thực thi và quản lý bằng chứng

- Chuẩn bị tài khoản, dữ liệu và trạng thái hệ thống cần thiết.
- Xác nhận tiền điều kiện trước mỗi test case.
- Thực hiện đúng các bước đã ghi; không thay đổi dữ liệu giữa chừng mà
  không ghi lại.
- Ghi kết quả thực tế, trạng thái Pass/Fail/Blocked/Not Executed
- Khi phát hiện sai lệch, thử tái hiện để phân biệt lỗi sản phẩm với
  lỗi môi trường hoặc dữ liệu thử.
- Tạo GitHub Issue cho mỗi lỗi được xác nhận; đính kèm ảnh minh chứng
  trực tiếp trong issue.
- Cập nhật bảng thống kê sau mỗi đợt chạy kiểm thử.
- Tạo Git commit cho từng bước của quy trình kiểm thử từng feature
  theo yêu cầu bài tập.

## 1.7. Quy ước trạng thái kiểm thử

---

Trạng thái Ý nghĩa

---

Pass Kết quả thực tế phù hợp với kết quả
mong đợi

Fail Kết quả thực tế khác kết quả mong
đợi và đã ghi nhận bằng chứng

Blocked Không thể chạy do tiền điều kiện,
môi trường hoặc phụ thuộc bị chặn

Not Executed Chưa thực thi

N/A Không áp dụng; phải ghi rõ lý do

---

---

# 2. Thiết kế và thực thi kiểm thử

> Dùng cùng cấu trúc cho cả bốn feature. Hoàn thiện Domain Testing và
> BVA thành hai phần riêng biệt, sau đó ghi kết quả thực thi. Các bảng
> dưới đây là mẫu trống, không phải test case đã được xác nhận.

## 2.1. FR-03 -- Forgot password and password reset

### 2.1.1. Mô tả chức năng và yêu cầu

- **Mục đích chức năng:** \[Mô tả ngắn gọn luồng quên mật khẩu hai
  bước.\]
- **Điểm bắt đầu/kết thúc:** \[Trang bắt đầu và trạng thái kết thúc.\]
- **Tiền điều kiện:** \[Điều kiện cần có trước khi chạy.\]
- **Luồng nghiệp vụ:** \[Bước 1, bước 2 và các nhánh lỗi.\]
- **Nguồn yêu cầu:** \[Tài liệu/mã nguồn/giao diện và vị trí tham
  chiếu.\]
- **Giả định cần xác minh:** \[Liệt kê; không xem giả định là yêu cầu
  chính thức.\]

### 2.1.2. Domain Testing -- phân tích miền

---

Biến/điều kiện Miền/lớp hợp Miền/lớp không Căn cứ Cần xác minh?
lệ hợp lệ

---

Email/định \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
danh nguồn\]

Trạng thái tài \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
khoản nguồn\]

Mã xác \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
minh/token nguồn\]  
 (nếu có)

Mật khẩu mới \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
(nếu có) nguồn\]

Xác nhận mật \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
khẩu (nếu có) nguồn\]

Trạng thái \[Điền\] \[Điền\] \[Yêu cầu/mã \[Có/Không\]
luồng hai bước nguồn\]

---

**Giải thích phân hoạch miền:** \[Giải thích vì sao các giá trị được xếp
vào từng lớp, lớp nào có hành vi khác nhau và những trường hợp nào được
ưu tiên.\]

### 2.1.3. Test case Domain Testing

---

Test Case ID Lớp miền/điều Tiền điều Dữ liệu Các bước Kết quả Kết quả Trạng Bằng chứng
kiện kiện đầu vào thực hiện mong đợi thực tế thái

---

FR03-DT-01 \[Lớp hợp \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link/đường
lệ\] khi chạy\] dẫn\]
chạy\]

FR03-DT-02 \[Lớp không \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link/đường
hợp lệ\] khi chạy\] dẫn\]
chạy\]

FR03-DT-03 \[Lớp/trạng \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link/đường
thái khác\] khi chạy\] dẫn\]
chạy\]

---

**Đánh giá Domain Testing:** \[Số lớp đã xác định, số lớp đã được kiểm
tra, lớp còn thiếu và lý do.\]

### 2.1.4. BVA -- phân tích biên

---

Biến/điều Biên dưới Biên trên Căn cứ xác Giá trị cần thử Ghi chú
kiện định biên

---

Email/định \[Nếu có\] \[Nếu có\] \[Yêu \[Dưới/tại/trên \[Điền\]
danh cầu/mã biên phù hợp\]  
 nguồn\]

Mật khẩu mới \[Nếu có\] \[Nếu có\] \[Yêu \[Dưới/tại/trên \[Điền\]
cầu/mã biên phù hợp\]  
 nguồn\]

Thời hạn \[Nếu có\] \[Nếu có\] \[Yêu \[Trước/tại/sau \[Điền\]
token/mã cầu/mã thời điểm biên\]  
 (nếu có) nguồn\]

---

**Giải thích BVA:** \[Nêu từng biên, cách chọn giá trị ngay
dưới/tại/ngay trên biên và kết quả mong đợi. Nếu không có giới hạn được
xác minh, ghi rõ không áp dụng/chưa xác định.\]

### 2.1.5. Test case BVA

---

Test Case ID Biến/biên Giá trị Tiền điều Các bước Kết quả Kết quả Trạng Bằng chứng
kiểm thử kiện thực hiện mong đợi thực tế thái

---

FR03-BVA-01 \[Biên \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
dưới\] khi chạy\]  
 chạy\]

FR03-BVA-02 \[Tại \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
biên\] khi chạy\]  
 chạy\]

FR03-BVA-03 \[Biên \[Điền\] \[Điền\] \[Điền\] \[Điền\] \[Sau \[Chưa \[Link\]
trên\] khi chạy\]  
 chạy\]

---

### 2.1.6. Kết quả thực thi và bằng chứng

- Tổng test case Domain Testing: \[ \]
- Tổng test case BVA: \[ \]
- Pass: \[ \] \| Fail: \[ \] \| Blocked: \[ \] \| Not Executed: \[ \]
- Bug liên quan: \[ID/link hoặc "Chưa phát hiện sau khi thực thi"\]
- Bằng chứng: \[Thư mục ảnh/video/log\]
- Nhận xét: \[Kết quả nổi bật và phạm vi chưa kiểm tra.\]

### 2.1.7. Đánh giá feature

\[Đánh giá mức bao phủ lớp miền và biên, các rủi ro còn lại, những điều
AI đề xuất hữu ích/chưa chính xác và phần người kiểm thử đã bổ sung.\]

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

### 2.2.6. Kết quả thực thi và bằng chứng

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

### 2.3.6. Kết quả thực thi và bằng chứng

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

### 2.4.6. Kết quả thực thi và bằng chứng

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

# 3. Báo cáo lỗi (Bug Report)

## 3.1. Quy trình phát hiện và xác nhận lỗi

1.  Ghi nhận sai lệch giữa kết quả mong đợi và kết quả thực tế.
2.  Tái hiện lỗi với cùng dữ liệu và tiền điều kiện.
3.  Kiểm tra lỗi có thể do dữ liệu, cấu hình hoặc môi trường hay không.
4.  Thu thập bước tái hiện, ảnh chụp màn hình, log và thông tin môi
    trường.
5.  Tạo GitHub Issue với tiêu đề rõ ràng, mức độ nghiêm trọng và liên
    kết test case.
6.  Cập nhật trạng thái lỗi và bảng tổng hợp sau khi có kết quả xác
    minh.

## 3.2. Danh sách lỗi tổng hợp

---

Bug ID Feature Tiêu đề Severity Priority Trạng thái Test GitHub
ngắn Case Issue
liên  
 quan

---

BUG-001 \[FR-xx\] \[Tiêu \[Critical/High/Medium/Low\] \[P1/P2/P3/P4\] \[Open/...\] \[TC \[URL\]
đề\] ID\]

\[Thêm  
 dòng khi  
 có lỗi  
 thực tế\]

---

> Nếu không phát hiện lỗi, ghi rõ số test case đã thực thi và phạm vi
> kiểm thử; không tạo bug giả để lấp bảng.

## 3.3. Chi tiết từng lỗi

### BUG-\[ID\]: \[Tiêu đề ngắn, mô tả đúng hiện tượng\]

- **Feature:** \[FR-03/FR-09/FR-14/FR-07\]
- **Test Case ID:** \[ID\]
- **Severity:** \[Mức độ ảnh hưởng\]
- **Priority:** \[Mức ưu tiên xử lý\]
- **Môi trường:** \[Thiết bị, trình duyệt, phiên bản\]
- **Tiền điều kiện:** \[Điều kiện trước khi tái hiện\]
- **Dữ liệu kiểm thử:** \[Dữ liệu\]
- **Các bước tái hiện:**
  1.  \[Bước 1\]
  2.  \[Bước 2\]
  3.  \[Bước 3\]
- **Kết quả mong đợi:** \[Hành vi đúng theo yêu cầu\]
- **Kết quả thực tế:** \[Hành vi quan sát được\]
- **Tần suất tái hiện:** \[Ví dụ: x/y lần đã thử\]
- **Ảnh/log minh chứng:** \[Đường dẫn hoặc nhúng ảnh\]
- **GitHub Issue:** \[URL\]
- **Ghi chú/xác minh:** \[Thông tin bổ sung\]

## 3.4. Tổng kết lỗi

\[Tổng số lỗi xác nhận; phân loại theo feature, severity và trạng thái;
lỗi đã sửa/chưa sửa nếu có.\]

---

# 4. AI Gap Analysis and Evaluation

## 4.1. Các test case hoặc lỗi AI bỏ sót

---

Gap ID Feature Đề xuất Trường hợp Cách phát Tác động Cách bổ
ban đầu bị bỏ sót hiện sung
của AI

---

GAP-01 \[FR-xx\] \[Tóm \[Mô tả\] \[Rà \[Ảnh \[Cách
tắt\] soát/thực hưởng\] sửa\]
thi\]

---

## 4.2. Đầu ra AI chưa chính xác hoặc chưa đầy đủ

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

## 4.3. Phân tích nguyên nhân

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

## 4.4. So sánh trước và sau khi rà soát

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

## 4.5. Bài học rút ra

\[Trình bày bài học cụ thể về cách đặt prompt theo từng bước kỹ thuật,
kiểm chứng giới hạn từ nguồn có căn cứ, rà soát đầu ra AI và giữ trách
nhiệm cuối cùng của người kiểm thử.\]

---

# 5. Agent Skills và tự động hóa

## 5.1. Mục tiêu

\[Mô tả mục tiêu tạo Skill để hỗ trợ quy trình Domain Testing/BVA có cấu
trúc và có thể tái sử dụng. Nếu không xây dựng Skill, nêu trung thực
phạm vi đã làm và không khai báo tính năng chưa có.\]

## 5.2. Thiết kế Skill cho Domain Testing

- **Tên Skill:** \[Tên\]
- **Mục đích:** \[Mục đích\]
- **Đầu vào:** \[Yêu cầu, feature, biến, quy tắc nghiệp vụ, thông tin
  SUT\]
- **Các bước xử lý:** \[Phân tích yêu cầu → liệt kê biến → phân lớp
  miền → đề xuất test case → kiểm tra tính đầy đủ\]
- **Đầu ra:** \[Bảng miền, lớp tương đương, test case và giả định cần
  xác minh\]
- **Quy tắc an toàn/chất lượng:** \[Không tự tạo quy tắc nghiệp vụ;
  đánh dấu thông tin chưa xác minh\]
- **Tệp Skill:** \[Đường dẫn hoặc tên tệp\]

## 5.3. Thiết kế Skill cho BVA

- **Tên Skill:** \[Tên\]
- **Mục đích:** \[Mục đích\]
- **Đầu vào:** \[Biến, giới hạn, kiểu dữ liệu và căn cứ\]
- **Các bước xử lý:** \[Xác định biên → tạo giá trị sát biên → xác
  định kết quả mong đợi → rà soát\]
- **Đầu ra:** \[Bảng biên và test case BVA\]
- **Quy tắc chất lượng:** \[Không tự đoán min/max; ghi nguồn và các
  biên chưa xác minh\]
- **Tệp Skill:** \[Đường dẫn hoặc tên tệp\]

## 5.4. Cách sử dụng và đầu vào/đầu ra

1.  \[Cách cài đặt/kích hoạt Skill.\]
2.  \[Thông tin người dùng phải cung cấp.\]
3.  \[Cách Skill xử lý.\]
4.  \[Cách đọc kết quả.\]
5.  \[Các bước người dùng phải rà soát trước khi thực thi.\]

**Ví dụ đầu vào:** \[Prompt/dữ liệu đã loại bỏ thông tin nhạy cảm.\]

**Ví dụ đầu ra:** \[Trích đoạn kết quả thực tế, kèm đường dẫn tệp nếu
có.\]

## 5.5. Minh họa chạy Skill trên feature

- **Feature demo:** \[Chọn một feature\]
- **Mục tiêu demo:** \[Domain Testing/BVA hoặc cả hai\]
- **Các bước end-to-end:** \[Đầu vào → chạy Skill → rà soát → sửa →
  tạo test case → thực thi\]
- **Kết quả:** \[Kết quả thực tế\]
- **Bằng chứng:** \[Ảnh/log/video\]

## 5.6. Đánh giá hiệu quả và giới hạn

Tiêu chí Quan sát/kết quả Bằng chứng

---

Tính đầy đủ của test case \[Điền\] \[Link\]
Tính đúng đắn của miền/biên \[Điền\] \[Link\]
Khả năng tái sử dụng \[Điền\] \[Link\]
Thời gian/độ thuận tiện \[Điền nếu có đo lường\] \[Link\]
Lỗi hoặc giới hạn còn lại \[Điền\] \[Link\]
Vai trò rà soát của con người \[Điền\] \[Link\]

## 5.7. Video demo và liên kết

- **Video 1 -- Domain Testing:** \[YouTube URL\]
- **Video 2 -- BVA:** \[YouTube URL\]
- **Video end-to-end trên feature:** \[YouTube URL\]
- **Repository/tệp Skill:** \[URL\]
- **Ghi chú:** \[Nội dung video, feature và phạm vi được trình diễn\]

---

# 6. Test Summary Report

## 6.1. Tóm tắt phạm vi

- SUT: EShop.
- Số feature được lựa chọn: 4.
- Feature: FR-03, FR-09, FR-14 và FR-07 (phạm vi mobile cần xác nhận
  theo yêu cầu Pool D).
- Kỹ thuật: Domain Testing và Boundary Value Analysis.
- Khoảng thời gian kiểm thử: \[Ngày bắt đầu -- ngày kết thúc\].
- Môi trường: \[Tóm tắt\].

## 6.2. Thống kê test case

Chỉ số FR-03 FR-09 FR-14 FR-07 Mobile Tổng

---

Test case thiết kế -- Domain Testing \[ \] \[ \] \[ \] \[ \] \[ \]
Test case thiết kế -- BVA \[ \] \[ \] \[ \] \[ \] \[ \]
Tổng test case thiết kế \[ \] \[ \] \[ \] \[ \] \[ \]
Đã thực thi \[ \] \[ \] \[ \] \[ \] \[ \]
Pass \[ \] \[ \] \[ \] \[ \] \[ \]
Fail \[ \] \[ \] \[ \] \[ \] \[ \]
Blocked \[ \] \[ \] \[ \] \[ \] \[ \]
Not Executed \[ \] \[ \] \[ \] \[ \] \[ \]

> Kiểm tra tính nhất quán số liệu: `Đã thực thi = Pass + Fail` nếu chỉ
> dùng hai trạng thái này cho các test case đã chạy; nếu có Blocked, hãy
> thống nhất rõ cách tính và tránh cộng trùng. Tổng các trạng thái phải
> khớp với tổng số test case thiết kế.

## 6.3. Thống kê lỗi

Chỉ số Số lượng

---

Tổng lỗi đã ghi nhận \[ \]
Lỗi đã xác nhận \[ \]
Critical \[ \]
High \[ \]
Medium \[ \]
Low \[ \]
Open \[ \]
Fixed/Closed \[ \]

## 6.4. Đánh giá mức độ hoàn thành

- **Mức độ bao phủ Domain Testing:** \[Tóm tắt.\]
- **Mức độ bao phủ BVA:** \[Tóm tắt.\]
- **Tỷ lệ thực thi:** \[Cách tính và kết quả.\]
- **Kết quả quan trọng:** \[Nêu các kết quả đã có bằng chứng.\]
- **Yêu cầu chưa kiểm thử:** \[Liệt kê và lý do.\]

## 6.5. Hạn chế và rủi ro còn lại

\[Ví dụ về giới hạn môi trường, dữ liệu thử, không thể kiểm tra
email/token thật, thiếu quy tắc biên chính thức hoặc hạn chế thiết bị.
Chỉ nêu những điều thực sự xảy ra.\]

## 6.6. Kết luận và đề xuất

\[Tổng kết việc áp dụng hai kỹ thuật, mức độ đạt mục tiêu, lỗi quan
trọng, đóng góp của AI/Agent Skills và đề xuất kiểm thử tiếp theo.\]

---

# Tài liệu tham khảo

> Ghi thông tin xuất bản và ngày truy cập theo tài liệu thực tế đã đọc.
> Không đưa tài liệu vào danh mục nếu chưa sử dụng.

1.  ISTQB. _Foundation Level Syllabus_ (ấn bản mới nhất phù hợp).
2.  Hardman, P. (2025). _A Post-AI Learning Taxonomy_.
3.  Fuster Rabella (2025). _OECD Education Working Paper No. 338_.
4.  Anthropic (2025). _Building Reliable AI Test Agents_.
5.  DeepEval documentation: \[URL và ngày truy cập\].
6.  Promptfoo documentation: \[URL và ngày truy cập\].
7.  EShop SUT repository: https://github.com/ttbhanh/eshop-sut (ngày
    truy cập: \[ngày\]).
8.  Các tài liệu/nguồn khác đã sử dụng: \[Bổ sung\].

---

# Phụ lục

## A. AI Audit Report

**Phụ lục này phải được hoàn thiện và nộp thành tài liệu Markdown + PDF
riêng theo yêu cầu bài tập.**

Ghi lại toàn bộ tương tác AI có liên quan đến bài tập. Mỗi lần tương tác
cần có:

Trường Nội dung

---

Interaction ID \[AI-001\]
Công cụ/mô hình AI \[Tên công cụ và model nếu biết\]
Ngày giờ \[Ngày giờ thực tế\]
Feature/kỹ thuật \[FR-xx; Domain Testing/BVA/Gap Analysis/Skill...\]
Prompt đầy đủ \[Dán prompt đã dùng\]
Đầu ra AI \[Dán đầu ra hoặc tham chiếu tệp; lưu đủ theo yêu cầu\]
Rà soát của người thực hiện \[Điểm đúng/sai/thiếu và cách xử lý\]
Đầu ra sau chỉnh sửa \[Liên kết/tệp kết quả đã được kiểm chứng\]

Lặp lại bảng/nhật ký cho tất cả tương tác. Nếu không sử dụng AI cho một
phần nào đó, khai báo rõ theo đúng thực tế.

## B. AI Critique

**Phụ lục này phải được hoàn thiện và nộp thành tài liệu Markdown + PDF
riêng. Độ dài bắt buộc: 200--300 từ.**

Viết bài phê bình dựa trên trải nghiệm thực tế, trả lời:

1.  AI đã đưa ra nội dung sai, thiên lệch hoặc chưa đầy đủ ở đâu?
2.  Vì sao AI không phát hiện được vấn đề đó?
3.  Bạn đã dùng bằng chứng nào để xác minh hoặc bác bỏ đầu ra AI?
4.  Bạn rút ra nguyên tắc gì khi cộng tác với AI trong kiểm thử phần
    mềm?

**Bản nháp cần hoàn thiện:** \[Viết 200--300 từ dựa trên các trường hợp
thực tế trong Chương 4; không dùng đoạn văn chung chung hoặc khẳng định
chưa có bằng chứng.\]

## C. Git Commit Log

Lưu log ở tệp văn bản riêng, ví dụ `git-commit-log.txt`.

Ngày giờ Feature Bước kiểm thử Commit hash Nội dung commit

---

\[Ngày giờ\] \[FR-xx\] \[Phân tích yêu cầu\] \[Hash\] \[Thông điệp\]
\[Ngày giờ\] \[FR-xx\] \[Domain Testing\] \[Hash\] \[Thông điệp\]
\[Ngày giờ\] \[FR-xx\] \[BVA\] \[Hash\] \[Thông điệp\]
\[Ngày giờ\] \[FR-xx\] \[Thực thi/báo cáo\] \[Hash\] \[Thông điệp\]

## D. Bộ test case đầy đủ

\[Đặt các bảng test case chi tiết hoặc liên kết tới tệp
CSV/XLSX/Markdown chứa toàn bộ test case.\]

## E. Ảnh và bằng chứng kiểm thử

- \[Tên ảnh/video/log\]
- \[Test Case ID liên quan\]
- \[Feature\]
- \[Mô tả bằng chứng\]
- \[Đường dẫn\]

Mỗi GitHub Issue báo lỗi cần có ảnh minh chứng đính kèm theo yêu cầu bài
tập.

## F. Agent Skills và hướng dẫn sử dụng

- Tệp Skill Domain Testing: \[Đường dẫn\]
- Tệp Skill BVA: \[Đường dẫn\]
- Hướng dẫn cài đặt/sử dụng: \[Đường dẫn\]
- Ví dụ đầu vào/đầu ra: \[Đường dẫn\]
- Video demo end-to-end: \[URL\]

---

# Checklist trước khi nộp

- [ ] Báo cáo chính đã có cả Domain Testing và BVA cho đủ bốn feature.
- [ ] Mỗi feature có phân tích từng bước, miền/lớp giá trị, biên và
      test case.
- [ ] Test case có kết quả mong đợi, kết quả thực tế, trạng thái và
      bằng chứng.
- [ ] Mọi lỗi được ghi trong báo cáo Markdown và GitHub Issues; issue
      có ảnh minh chứng.
- [ ] AI Gap Analysis phản ánh các thiếu sót/sai sót thực tế của AI và
      lý do.
- [ ] AI Audit Report đầy đủ prompt, đầu ra, công cụ và ngày giờ; có
      bản Markdown + PDF riêng.
- [ ] AI Critique dài 200--300 từ; có bản Markdown + PDF riêng.
- [ ] Agent Skills và video demo được đính kèm nếu đã xây dựng; đối
      chiếu yêu cầu chấm điểm.
- [ ] Git commit log dạng văn bản được đính kèm.
- [ ] `README.md` có bảng tự đánh giá và Test Summary Report.
- [ ] Các con số trong báo cáo, README và bảng tổng hợp nhất quán.
- [ ] Xác nhận phạm vi Pool D/Mobile với giảng viên nếu cần.
- [ ] Tạo ZIP theo mẫu:
      `<StudentID>_HW02_AI_DomainTesting_<SelfAssessedGrade>.zip`.
- [ ] Kiểm tra đầy đủ các tệp bắt buộc trước khi nộp Moodle.
