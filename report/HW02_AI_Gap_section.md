# Chương 4 -- AI Gap Analysis and Evaluation (nội dung đã điền cho Chương 4)

> **Cách dùng:** copy toàn bộ phần dưới đây (từ `# 4.` đến hết mục 4.5) thay thế cho Chương 4
> tương ứng trong `HW02_Domain_Testing_Report.md`.
>
> **Nguồn nội dung:** tổng hợp từ 4 tài liệu sai sót của AI theo từng feature --
> `forgot-reset-password-feature/ai-mistake.md` (FR-03),
> `discount-coupons-feature/ai-mistake.md` (FR-09),
> `category-management-feature/ai-mistake.md` (FR-14),
> `shopping-cart-mobile-feature/ai-mistake.md` (FR-07) -- kết hợp phần "Đầu ra AI
> chưa chính xác/sai sót" và số liệu thực thi tại 2.1.7, 2.2.7, 2.3.7, 2.4.7
> (`report/HW02_FR-03_section.md`, `report/HW02_FR-09_section.md`,
> `report/HW02_FR-14_section.md`, `report/HW02_FR-07_section.md`).
>
> **Bối cảnh số liệu (2026-10-18):** 109 test case chính thức đã được thiết kế với tham
> vấn AI và thực thi 109/109 (Pass 64 | Fail 45); ngoài ra có **12 kiểm thử bổ
> sung/probe** được người kiểm thử thêm sau rà soát. Tổng **23 bug/sai lệch** được đánh
> số và xác nhận bằng thực thi (chi tiết tại Chương 3).

---

# 4. AI Gap Analysis and Evaluation

## 4.1. Các test case hoặc lỗi AI bỏ sót

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


## 4.2. Đầu ra AI chưa chính xác hoặc chưa đầy đủ

| AI output ID | Feature | Nội dung AI đưa ra | Vấn đề phát hiện đối chiếu | Bằng chứng | Chỉnh sửa của người kiểm thử |
| --- | --- | --- | --- | --- | --- |
| AI-01 | FR-03 | Coverage report khai báo email 3/3, resetToken 3/3, newPassword 4/5 lớp | **Khai báo bao phủ vượt thực tế (thiếu):** 4 lớp khai báo đã phủ (email sai định dạng, OTP rỗng, mật khẩu rỗng, thiếu độ phức tạp) không có test case tương ứng trong danh sách 8 test case | Đối chiếu coverage report với 2.1.3; `forgot-reset-password-feature/ai-mistake.md` | Người kiểm thử đặt 4 dữ liệu kiểm chứng bổ sung, thực thi khớp 100% dự đoán; sai sót khai báo được ghi nhận nguyên trạng trước khi thực thi |
| AI-02 | FR-03 | FR03-DT-03 yêu cầu dữ liệu `confirmPassword = "Abc12345!"` | **Thiếu đồng bộ thiết kế ↔ khảo sát:** AI đã xác minh UI không có trường xác nhận mật khẩu nhưng vẫn giữ dữ liệu `confirmPassword` trong test case; đồng thời mật khẩu "hợp lệ theo SRS" không thỏa regex thực tế khiến kết quả happy path trộn hai sai lệch | FR03-DT-03 ghi chú 1; `ForgotPassword.jsx` dòng 26--30 | Bỏ qua `confirmPassword` khi thực thi (ghi chú rõ vào test case); bổ sung bộ BVA-01S/02S/03S (mật khẩu thỏa regex thực tế) để cô lập riêng biên 8 ký tự khỏi sai lệch regex |
| AI-03 | FR-09 | Bảng "Giá trị đại diện cho công thức (T1--T3)" ở bước 6 | **Sai và loạn trình bày:** bảng bị lặp 3 lần với nội dung lệch nhau; ký tự lỗi soạn thảo (`50. (000`, cột `suggested`, cụm "need xác minh", `270.0 0`); sai bản chất: `BIGBUY × 500.000` bị ghi thành percent 10% giảm 25.000 (đúng: fixed 50.000); sinh mã `SAVE4` không tồn tại trong dữ liệu | `discount-coupons-feature/ai-mistake.md` (Step 6) | AI tự phát hiện, trình bày lại bảng chuẩn ngay sau đó; các dòng lặp/sai không được dùng để thiết kế test case; người kiểm thử duyệt lại giá trị đại diện đối chiếu seed trước khi dùng |
| AI-04 | FR-09 | File `discount-coupons-feature/domain-testing/test-cases.md` (28 test case) | **Sinh text thừa chèn sai vị trí:** `**ID:** TC-0010 sẽ nói ở Test Case 10.` (trước TC-09) và `## Test Case  test 18 được mô tả dưới đây.` (trước TC-18) | `discount-coupons-feature/ai-mistake.md` (Step 7) | AI tự phát hiện, sửa bằng edit (lần đầu thất bại vì oldString không khớp chính xác -- đọc lại file rồi sửa thành công); nội dung 28 test case không bị ảnh hưởng |
| AI-05 | FR-09 | Bản nháp `report/HW02_FR-09_section.md` | **Lỗi soạn thảo trong báo cáo:** ký tự rác "vì ắt Fah điểm" chèn giữa câu tại 2.2.2; hàng FR09-DT-23 trong bảng 2.2.3 thiếu cột "Dữ liệu đầu vào" (nội dung steps dồn vào cột sai) | `discount-coupons-feature/ai-mistake.md` (Bổ sung 18-10-2026) | AI tự soát ngay sau khi ghi và sửa cả hai bằng edit; người kiểm thử duyệt lại bảng trước khi tổng hợp số liệu |
| AI-06 | FR-14 | Bước thực thi/khảo sát gọi trực tiếp POST/DELETE lên SUT | **Probe không cô lập dữ liệu:** làm bẩn CSDL seed -- sinh bản ghi rác (id 4--13) và mất danh mục id=1, lệch tiền đề cho các test sau | `category-management-feature/ai-mistake.md` (Summary) | Khởi động lại backend để re-seed (`DROP TABLE` + seed) và chạy lại toàn bộ lượt chính thức trên dữ liệu sạch theo đúng thứ tự test case; ghi quy trình dọn môi trường trong Notes của feature |
| AI-07 | FR-14 | Kỳ vọng mã lỗi HTTP cho các lớp token | **Giả định sai:** mặc định token sai định dạng trả 401; thực tế middleware `jwt.verify` trả 403 cho token sai định dạng và 401 cho token thiếu | FR14-DT-10 (kỳ vọng "401/403", thực tế 403); `category-management-feature/ai-mistake.md` | Điều chỉnh kỳ vọng dựa trên bằng chứng thực thi thay vì giả định; sau đó áp dụng nguyên tắc "expected theo SRS, quan sát triển khai tách riêng" cho các feature còn lại |
| AI-08 | FR-14 | Đặt 2 test PUT (DT-16, DT-17) vào danh sách chính do đề bài ghi "Category management (CRUD)" | **Nhầm phạm vi:** SRS FR-14 chỉ yêu cầu Thêm/Xem/Xóa (không có Update) dù backend có API PUT; bộ test ban đầu chưa tách bạch phạm vi theo SRS | `category-management-feature/ai-mistake.md`; `category-management-feature/feature-overview.md` | Giữ 2 test nhưng gắn nhãn "ngoài phạm vi SRS -- tham chiếu"; tách bạch khi thống kê để không nhận sai hành vi là bug của FR-14 theo đặc tả |
| AI-09 | FR-14 | Giá trị biên trên của miền id DELETE | **Biên động có nguy cơ hard-code:** max id tồn tại thay đổi theo dữ liệu tại thời điểm chạy; nếu hard-code sẽ sai tiền đề cho BVA-06/07 | `category-management-feature/ai-mistake.md`; FR14-BVA-06 (id=15 khi max=14) | Bổ sung bước GET lấy max id ngay trước khi chạy BVA-06/07 (max=14 tại thời điểm chạy); ghi chú "miền động" trong bảng biên |
| AI-10 | FR-07 | Trình bày phân tích trong chat (bước 1--4) | **Lỗi mã hóa và Markdown:** chữ lạ thay "code" (bước 1), cụm SRS lẫn ký tự lạ (bước 2); bước 4: bảng lớp tương đương V2 (`product_id`), V4 (`cart_state`), V5 (`repeat_add`), V6 (`delete_action`) vỡ cấu trúc (thiếu/dư dòng phân cách, cột lệch, "TC-B01" rơi sai vị trí); lỗi gõ rải rác: "ting tự đặc biệt" (đúng: "ký tự đặc biệt"), "Mini-1 boundary" (đúng: "min-1"), mô tả invalid class của V2 bị cụt | `shopping-cart-mobile-feature/ai-mistake.md` | Người dùng duyệt nội dung -- phân hoạch EC1--EC16 giữ nguyên không đổi; từ bước sau áp self-check sau khi ghi file |
| AI-11 | FR-07 | Các file output đầu: `ai-mistake.md`, `bva/bva-values.md` | **Lỗi sót trong file:** ai-mistake.md còn ký tự lỗi CJK sau 2 lần ghi ("tu的还是", "đ切尔"); bva/bva-values.md có 2 lỗi chính tả ("Ranhr giới", cụm tiếng Anh trộn lẫn "only exploratory") | `shopping-cart-mobile-feature/ai-mistake.md` (Xử lý trong các file output) | Viết lại toàn bộ ai-mistake.md rồi edit từng dòng làm sạch; sửa 2 lỗi chính tả trước khi trình duyệt; các file khác (feature-overview, domain-testing/test-cases, bva/test-cases, 2 coverage-report) self-check đạt |
| AI-12 | FR-07 | Harness thực thi tái lập các handler giỏ hàng | **Nguy cơ lệch mã gốc:** bản nháp đầu tái lập handler sửa số lượng bằng cách tách biểu thức -- kết quả harness có thể nghiêng so với `App.js` thật | `shopping-cart-mobile-feature/ai-mistake.md` (Bước thực thi 2026-10-18) | Viết lại nguyên văn đúng `App.js:617-621` (`parsed + 1` trong cùng một biểu thức, kèm chú thích số dòng) và **chạy lại toàn bộ 41 test** trước khi lấy số liệu chính thức |
| AI-13 | FR-07 | Bản nháp `report/HW02_FR-07_section.md` | **Lỗi soạn thảo trong báo cáo:** "Ba dí 0 dòng" trong bảng BVA (FR07-BVA-13); câu "Các trường hợp chưa kiểm" viết sai ngữ nghĩa lần đầu ("duplicate-dùng giỏ khi đổi IP") kèm dấu gạch dư ";-percent" | `shopping-cart-mobile-feature/ai-mistake.md` (Bước thực thi + viết báo cáo) | Sửa cả hai ngay khi self-check sau khi ghi; viết lại danh sách trường hợp chưa kiểm cho đúng phạm vi; quét tự động ký tự CJK/lệch mã hóa bằng PowerShell trên file báo cáo và execution-results -- kết quả sạch |


## 4.3. Phân tích nguyên nhân

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

## 4.4. So sánh trước và sau khi rà soát

| Chỉ số | Trước rà soát AI | Sau rà soát của người thực hiện | Giải thích thay đổi |
| --- | --- | --- | --- |
| Số test case được đề xuất | 109 (FR-03: 8; FR-09: 35; FR-14: 25; FR-07: 41) | 109 test chính thức + 12 kiểm thử bổ sung/probe (FR-03: 9; FR-09: 2; FR-14: 1) | Thiết kế lõi của AI được giữ nguyên 100%; phần bổ sung phủ các lớp AI khai báo sẵn mà không có test case (GAP-01), biên chưa xác minh (GAP-05) và probe xác nhận bypass/lỗi lan (GAP-08) |
| Số test case được giữ lại | 109 | 109 (100%), trong đó 2 test PUT FR-14 giữ lại nhưng đổi nhãn "ngoài phạm vi SRS -- tham chiếu" | Không có test case nào bị loại: sai sót của AI nằm ở trình bày, giả định và khai báo coverage, không nằm ở chất lượng phân lớp miền/biên |
| Số test case bổ sung | --- | 12 (FR-03: 3 BVA bổ sung cô lập regex + 4 lớp khai báo thừa + 2 probe dùng lại token/quay lại bước 1; FR-09: 2 probe bypass C5 và checkout `total=1`; FR-14: 1 thăm dò 10.000 ký tự) | Bổ sung để chứng minh gap được lấp; toàn bộ 12 dự đoán và kết quả bổ sung đều khớp khi thực thi |
| Số test case bị sửa/loại | --- | Sửa 3, loại 0: FR03-DT-03 (bỏ dữ liệu `confirmPassword` khi thực thi); FR14-DT-16/17 (đổi nhãn phạm vi) | Đồng bộ thiết kế với kết quả khảo sát mã nguồn (UI không có trường xác nhận) và phạm vi SRS thật (không yêu cầu Update) |
| Số lỗi được xác nhận | 0 bug chính thức -- 22 điểm nghi ngờ được AI đánh dấu trước khi chạy (FR-03: 4 sai lệch SRS↔mã nguồn; FR-09: 6 điểm "Quan sát khi chạy"; FR-14: 6 nhóm vi phạm dự đoán; FR-07: 6 điểm "Cần xác minh") | 23 bug/sai lệch được đánh số và xác nhận bằng thực thi: FR-03: 2 (BUG-03, BUG-04) + 2 sai lệch xác nhận không cấu thành Fail; FR-09: 6 (BUG-001..006) + BUG-007; FR-14: 6 (BUG-01..06); FR-07: 6 (BUG-101..105, BUG-107) + BUG-106 + BUG-108 | 100% điểm nghi ngờ đánh dấu từ mã nguồn đều thành hiện thực khi thực thi; probe bổ sung phát hiện thêm BUG-007; BUG-108 được phát hiện nhờ rà soát chéo ngoài thiết kế test (GAP-08) |

## 4.5. Bài học rút ra

1. **Tách bạch "kỳ vọng theo SRS" và "quan sát theo triển khai" ngay trong thiết kế:**
   mỗi test case giữ expected theo SRS và đưa các điểm bất thường của mã nguồn vào ghi
   chú riêng (bài học gốc FR-09). Nhờ vậy 22 điểm nghi ngờ được đánh dấu trước khi chạy
   và khớp 100% khi thực thi; kỳ vọng không bị "nhiễm" bới hành vi sai của sản phẩm.
2. **Khai báo coverage phải đi kèm test case tương ứng 1--1:** sau khi phân lớp cần một
   bước đối chiếu coverage report ↔ danh sách test case; lớp nào không có test case phải
   ghi Missing có căn cứ (GAP-01 của FR-03 là ví dụ điển hình -- khai báo thừa chỉ bị
   phát hiện khi rà soát thủ công).
3. **Đừng tin môi trường sẵn có:** luôn xác nhận trạng thái backend/DB trước khi chạy;
   restart để re-seed trước mỗi lượt chính thức; mọi probe phải cô lập dữ liệu hoặc kèm
   bước dọn (bài học CSDL bẩn FR-14 và cổng 3000 của FR-09).
4. **Biên động phải được tính tại thời điểm chạy:** các giá trị như max id tồn tại thay
   đổi theo dữ liệu -- đọc bằng GET ngay trước khi chạy, không hard-code (FR-14).
5. **Giá trị đại diện phải tồn tại và đúng loại trong dữ liệu thật:** mã `SAVE4` do AI
   "sinh ra" và việc nhầm BIGBUY percent/fixed cho thấy bảng giá trị luôn phải đối chiếu
   với seed/DB trước khi dùng làm dữ liệu test (FR-09).
6. **Soạn thảo theo từng khối nhỏ + self-check hai lớp:** trình bày từng bảng một và
   kiểm tra block vừa viết trước khi viết block kế; sau khi ghi file phải đọc lại
   (self-check); với nội dung tiếng Việt ưu tiên write toàn file thay vì edit nhỏ để
   tránh sót ký tự lỗi; kết hợp quét tự động ký tự CJK/lệch mã hóa (PowerShell) như đã
   làm cho FR-07.
7. **Read trước khi edit:** lần sửa đầu của FR-09 thất bại vì oldString không khớp chính
   xác -- đọc lại file để lấy nội dung chính xác luôn rẻ hơn đoán.
8. **Khi tái lập mã (harness), copy nguyên văn kèm số dòng:** diễn lại/tách biểu thức
   tạo sai lệch vô hình với mã gốc (AI-12); nguyên văn `parsed + 1` (`App.js:617-621`)
   phải được giữ và phải chạy lại toàn bộ test sau khi sửa harness.
9. **Đọc SRS để chốt phạm vi trước khi thiết kế:** mâu thuẫn "CRUD" (đề bài) ↔
   "Thêm/Xem/Xóa" (SRS FR-14) được xử lý đúng bằng cách gắn nhãn test ngoài phạm vi thành
   "tham chiếu" thay vì tính nhầm thành bug hoặc bỏ âm thầm.
10. **Rà soát chéo trong khi thực thi:** BUG-108 không nằm trong bất kỳ test case nào
    nhưng được phát hiện nhờ quan sát lân cận phạm vi khi test FR-07 -- nên dành thời
    gian kiểm tra biên giới giữa các feature (giỏ → checkout, coupon → checkout).
11. **Trách nhiệm cuối cùng thuộc về người kiểm thử:** mọi số liệu của AI được tái xác
    nhận bằng thực thi trên dữ liệu sạch/harness nguyên văn (109/109 test chính thức,
    23/23 bug có bằng chứng); mọi điểm chưa kiểm chứng được ghi rõ "chưa xác minh" thay
    vì trình bày như kết quả -- từ GAP-01 đến AI-13, không có sai sót nào của AI lọt
    đến số liệu chính thức của báo cáo.

---

## Số liệu dùng cho các bảng tổng hợp (không copy vào Chương 4)

- Test case chính thức thiết kế với tham vấn AI và thực thi: **109/109**
  (FR-03: 8 -- Pass 4/Fail 4; FR-09: 35 -- Pass 23/Fail 12; FR-14: 25 -- Pass 12/Fail 13;
  FR-07: 41 -- Pass 25/Fail 16); kiểm thử bổ sung/probe sau rà soát: **12**.
- Bug/sai lệch được đánh số và xác nhận: **23** (FR-03: 2; FR-09: 6 + BUG-007; FR-14: 6;
  FR-07: 6 + BUG-106 + BUG-108); độ khớp dự đoán mã nguồn ↔ thực thi: **100%**
  (FR-09: 6/6; FR-07: 6/6; FR-03: 4/4 sai lệch; FR-14: các lớp dự đoán đều thành bug).
- Sai sót của AI đã ghi nhận: **13 mục (AI-01..AI-13)** -- chia theo lỗi trình bày/soạn
  thảo (7), giả định/khai báo (4), quy trình thực thi (2); **0 test case bị loại**,
  3 test được điều chỉnh nhãn/dữ liệu; phần lớn do AI tự phát hiện qua self-check,
  phần còn lại do người kiểm thử bắt trong rà soát.
- Khoảng trống bao phủ đã ghi (GAP-01..GAP-08): 4 lớp FR-03, 3 lớp DT + 2 biên BVA
  FR-09, 2 điểm FR-14, 2 biên + nhiều tình huống FR-07 -- đã được lấp hoặc ghi
  "chưa xác minh/Missing" có căn cứ.
