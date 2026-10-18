# HW02 – AI Domain Testing on EShop

## 1. Thông tin bài nộp

| Trường            | Thông tin                        |
| ----------------- | -------------------------------- |
| Họ và tên         | `Phạm Vương Quân`                |
| MSSV              | `23120341`                       |
| Môn học / bài tập | `HW02 – Domain Testing on EShop` |

## 2. Self-assessment table

| No. | Criteria                              | Maximum Grade | Self-Assessed Grade |
| --: | ------------------------------------- | ------------: | ------------------: |
|   1 | Feature A (Domain + Boundary)         |            25 |                `25` |
|   2 | Feature B (Domain + Boundary)         |            25 |                `25` |
|   3 | Feature C (Domain + Boundary)         |            25 |                `25` |
|   4 | Feature D (Mobile, Domain + Boundary) |            15 |                `15` |
|   5 | Agent Skills                          |            10 |                `10` |
|     | **Total**                             |       **100** |             **100** |

# 3. Test Summary Report

## 3.1. Tóm tắt phạm vi

- **SUT:** EShop (demo thương mại điện tử: backend Node/Express + SQLite, frontend-web React + Vite, frontend-admin React, mobile React Native). Repository SUT: `ttbhanh/eshop-sut`.
- **Số feature được lựa chọn:** 4 / 4 pool (A, B, C, D).
- **Feature:** FR-03 — Forgot password and password reset (two steps); FR-09 — Discount coupons; FR-14 — Category management (CRUD: Thêm/Xem/Xóa); FR-07 — Shopping cart (Mobile).
- **Kỹ thuật:** Domain Testing (phân hoạch lớp tương đương) và Boundary Value Analysis (3 điểm: dưới/tại/trên biên).
- **Môi trường:** backend + SQLite tại `http://localhost:3000` (seed sạch); frontend-web `http://localhost:5173` (Edge Chromium); mobile qua harness logic Node.js v24.10.0 + kiểm tra tĩnh render; không có emulator Android/iOS, không kiểm thử email/token thật.
- **Quản lý lỗi:** GitHub Issues `kiterakun/HW02-Domain_Testing` — issue #1 → #23, labels `type: bug`, `status: new`.

## 3.2. Thống kê test case

| Chỉ số                               | FR-03 |  FR-09 |  FR-14 |  FR-07 | **Tổng** |
| ------------------------------------ | ----: | -----: | -----: | -----: | -------: |
| Test case thiết kế -- Domain Testing |     5 |     24 |     18 |     23 |   **70** |
| Test case thiết kế -- BVA            |     3 |     11 |      7 |     18 |   **39** |
| **Tổng test case thiết kế**          | **8** | **35** | **25** | **41** |  **109** |
| Đã thực thi                          |     8 |     35 |     25 |     41 |  **109** |
| Pass                                 |     4 |     23 |     12 |     25 |   **64** |
| Fail                                 |     4 |     12 |     13 |     16 |   **45** |
| Blocked                              |     0 |      0 |      0 |      0 |    **0** |
| Not Executed                         |     0 |      0 |      0 |      0 |    **0** |

Ghi chú số liệu (không cộng trùng):

- FR-03: ngoài 8 test chính thức, chạy thêm 3 test BVA bổ sung (BVA-01S/02S/03S cô lập regex, đều Pass) và 9 probe/kiểm chứng bổ sung — không tính vào số liệu chính thức.
- FR-09: 11 điểm BVA gộp trong 4 nhóm biên (BVA-001…004); 2 probe (bypass C5, checkout total=1) không tính số liệu.
- FR-14: 18 DT bao gồm 2 test PUT "tham chiếu ngoài SRS" (DT-16 Pass tham chiếu); BVA-08 (thăm dò 10.000 ký tự) là Informational, không tính.
- FR-07: 18 điểm BVA trên 6 ranh giới (mỗi ranh giới ≥ 3 điểm).

## 3.3. Thống kê lỗi

| Chỉ số               | Số lượng |
| -------------------- | -------: |
| Tổng lỗi đã ghi nhận |       23 |
| Lỗi đã xác nhận      |       23 |
| Critical             |        5 |
| High                 |        6 |
| Medium               |        8 |
| Low                  |        4 |
| Open                 |       23 |
| Fixed/Closed         |        0 |

Phân bổ theo feature: FR-03: 2 | FR-09: 7 | FR-14: 6 | FR-07: 8. Chi tiết tại mục 2.

## 3.4. Đánh giá mức độ hoàn thành

- **Mức độ bao phủ Domain Testing:** FR-03 phủ 5 lớp chính + 4 lớp bổ sung kiểm chứng bằng probe; FR-09 phủ 22/25 lớp (88%, 3 lớp không kiểm được có căn cứ: hết hạn đúng hôm nay, count > max, percent 20–99%); FR-14 phủ 22/22 lớp; FR-07 phủ đủ các lớp đã thiết kế trên 5 biến chính (~96%, phần còn lại N/A có căn cứ).
- **Mức độ bao phủ BVA:** FR-03 3/3 điểm biên 8 ký tự (cộng 3 điểm cô lập regex); FR-09 5/6 nhóm biên (biên ngưỡng EXPIRED bị C2 che); FR-14 đủ 3 điểm biên tên + 4 điểm biên id; FR-07 6/6 ranh giới, mỗi ranh giới ≥ 3 điểm; 2 ranh giới không phủ có lý do (SRS không định nghĩa max; không tạo được giá trị sát mốc 1.000₫ qua UI).
- **Tỷ lệ thực thi:** 109/109 = **100%** (không có Blocked/Not Executed).
- **Kết quả quan trọng:** 45 Fail được xác nhận bằng thực thi (API/trình duyệt/harness logic) và đối chiếu mã nguồn; 100% khớp dự đoán từ rà soát mã nguồn trước khi chạy (FR-03: không có Fail ngoài dự đoán; FR-09 và FR-07: 6/6 điểm nghi ngờ đều thành hiện thực). 5 lỗi Critical liên quan tiền, bảo mật áp coupon và kiểm soát truy cập admin.
- **Yêu cầu chưa kiểm thử:** gửi email OTP thật và thời hạn token (SUT không cài đặt); FR-14 Update trên UI (UI không có chức năng — chỉ kiểm PUT ở mức API tham chiếu); các khía cạnh chỉ đo được trên thiết bị thật của FR-07 (bố cục, bàn phím số, cảm ứng); FR-08 Checkout chưa được kiểm thử trực tiếp như một feature riêng (2 lỗi liên quan được phát hiện qua FR-09/FR-07).

## 3.5. Hạn chế và rủi ro còn lại

1. **Môi trường mobile:** FR-07 thực thi bằng harness logic + kiểm tra tĩnh thay vì thiết bị thật — các khía cạnh UI chạy thật chưa được kiểm chứng; BUG-FR07-08 cần tái hiện trên emulator/thiết bị.
2. **OTP/email demo:** SUT hiển thị OTP trực tiếp trên màn hình, không kiểm thử được việc gửi email thật; token không có thời hạn nên không kiểm thử lớp "OTP hết hạn".
3. **Dữ liệu/lượt dùng phi persistent:** DB re-seed sau mỗi đợt chạy — số liệu đếm lượt coupon chỉ có ý nghĩa trong một phiên; biên "hết hạn đúng hôm nay" và "count > max" không dựng được qua hệ thống thực.
4. **SRS thiếu biên/mơ hồ:** độ dài tên danh mục không có biên trên (chỉ thăm dò); số lượng giỏ không có max (chỉ exploratory 9999); FR-23 "số lượng sản phẩm" mơ hồ khiến BUG-FR07-06 chưa chốt được.
5. **Rủi ro nghiệp vụ ngoài SRS nhưng đã ghi nhận:** bỏ qua `is_active` vẫn từ chối đúng qua C1; coupon_id không lưu trong đơn → không truy vết doanh thu giảm giá; `/api/coupon-usage` phụ thuộc frontend tự gọi — client khác ý muốn có thể bỏ qua để "không bao giờ hết lượt"; locale ảnh hưởng định dạng tiền ("30,000,000 ₫" trên en-US).
6. **Khôi phục dữ liệu:** mật khẩu của user test và cây danh mục quanh các đợt chạy đã được re-seed về trạng thái gốc; đã xác nhận sau từng đợt.

## 3.6. Kết luận và đề xuất

- **Kết luận:** Hai kỹ thuật Domain Testing và BVA được triển khai trọn vẹn trên cả 4 feature: 109/109 test case thiết kế đều được thực thi (100%), phát hiện và xác nhận 23 lỗi có GitHub Issue (#1–#23), trong đó 5 Critical/6 High tập trung ở tính toán giảm giá, kiểm soát truy cập và ràng buộc dữ liệu. BVA chứng minh giá trị riêng ở FR-09 (bắt lỗi đúng tại điểm biên `>=`) và FR-03 (cô lập nguyên nhân regex khỏi biên độ dài).
- **Đóng góp của AI/Agent Skills:** AI đề xuất bộ test case bám sát SRS, phân lớp rõ ràng và bộ giá trị 3 điểm dày đặc; các điểm nghi ngờ từ rà soát AI về mã nguồn đều được người kiểm thử kiểm chứng và 100% xác nhận qua thực thi (FR-03: 4/4 sai lệch; FR-09: 6/6; FR-07: 6/6). AI cũng bỏ sót một số lớp/lặp sai bao phủ — đã được người kiểm thử bổ sung và ghi nhận trong mục "Đánh giá/AI Gap" của từng section.
- **Đề xuất tiếp theo:**
  1. Ưu tiên sửa nhóm Critical: BUG-FR09-01/03 (công thức percent, JWT), BUG-FR14-04 (role admin), BUG-FR03-02 (regex mật khẩu), BUG-FR14-01 (validate name).
  2. Kiểm thử trực tiếp FR-08 Checkout (đã phát hiện 2 lỗi láng giềng BUG-FR09-07, BUG-FR07-08) theo cùng phương pháp.
  3. Bổ sung kiểm thử trên thiết bị/emulator cho FR-07 và tái hiện end-to-end BUG-FR07-08.
  4. Làm rõ đặc tả mơ hồ với đội sản phẩm: FR-23 (định nghĩa badge), giá trị biên chưa xác minh (độ dài tên danh mục, max số lượng giỏ, thứ tự ưu tiên lỗi coupon).
  5. Thiết lập kiểm thử hồi quy cho các đợt re-seed/re-run sau khi sửa lỗi.

---
