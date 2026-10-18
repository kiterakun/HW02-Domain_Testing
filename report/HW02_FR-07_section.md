# FR-07 -- Shopping cart (Mobile) (nội dung đã điền cho mục 2.4 -- ĐÃ THỰC THI)

> **Cách dùng:** copy toàn bộ phần dưới đây (từ `## 2.4.` đến hết mục 2.4.7) thay thế cho mục 2.4 tương ứng trong `HW02_Domain_Testing_Report.md`.
>
> **Nguồn nội dung:** tài liệu trong `shopping-cart-mobile-feature/` (feature-overview.md, domain-testing/, bva/, execution/, ai-mistake.md), đã đối chiếu với mã nguồn SUT: `eshop-sut/frontend-mobile/App.js` (`renderCart` dòng 590--657; `addToCart` dòng 134--153; `removeFromCart` dòng 155--159; `normalizeQuantity` dòng 129--132; badge dòng 436; `openCheckout` dòng 342--354), `eshop-sut/backend/database.js` (seed sản phẩm dòng 96--103), SRS FR-07/FR-06/FR-21/FR-23/FR-24 (`eshop-sut/README.md` dòng 93--100, 86--88, 243--269).
>
> **Thực thi:** đã chạy toàn bộ 41 test case (23 Domain Testing + 18 BVA) ngày 18-10-2026. Giỏ hàng mobile là state phía client trong `App.js` (không có API giỏ hàng ở backend) và môi trường chạy không có emulator Android/iOS, nên việc thực thi dùng hai phương pháp bổ sung cho nhau: (1) **harness logic** -- tái lập NGUYÊN VĂN các handler giỏ hàng (`addToCart`, `removeFromCart`, `normalizeQuantity`, `onChangeText` số lượng, `cartTotal`, badge) từ `App.js` kèm chú thích số dòng, chạy bằng Node.js v24.10.0 (kịch bản `shopping-cart-mobile-feature/execution/harness-fr07.js`); (2) **kiểm tra tĩnh render** -- đọc trực tiếp chính tệp `App.js` (mã chạy thật trên thiết bị) để xác minh các khía cạnh giao diện (nhãn tổng, hình minh họa, nút +/−, dialog xóa). Mọi kết luận Fail đều dựa trên mã nguồn ứng dụng mobile thực tế.

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

## Số liệu dùng cho các bảng tổng hợp (không copy vào mục 2.4)

Khi cập nhật bảng 2.5 và bảng 6.2 của báo cáo, dùng các số sau cho FR-07:

- Test case thiết kế -- Domain Testing: **23**
- Test case thiết kế -- BVA: **18** (6 ranh giới, 18 điểm 3-point)
- Tổng test case thiết kế: **41**
- Đã thực thi: **41** | Pass: **25** | Fail: **16** | Blocked: **0** | Not Executed: **0**
- Bug xác nhận (FR-07): **6** (BUG-FR07-01 High, BUG-FR07-02 Medium, BUG-FR07-03 Medium, BUG-FR07-04 High, BUG-FR07-05 Medium, BUG-FR07-07 Low); thêm **BUG-FR07-06 Low** (FR-23 mơ hồ -- ghi nhận, không tính vi phạm tuyệt đối) và **BUG-FR07-08 High** (phát hiện trong khi test FR-07 nhưng thuộc FR-08).
