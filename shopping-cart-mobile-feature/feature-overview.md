# Feature Overview

**Feature Name:** FR-07 — Giỏ hàng (Shopping Cart) — Mobile App
**Description:** Domain Testing & Boundary Value Analysis (BVA) cho tính năng Giỏ hàng trên ứng dụng Mobile (React Native + Expo) của hệ thống EShop.
**Version:** SRS v2.0 — Cập nhật 2026-05-14

## Requirements
(nguyên văn FR-07 từ `eshop-sut/README.md` — SRS EShop)

### FR-07: Giỏ hàng (Shopping Cart)
- Hiển thị danh sách sản phẩm với các cột: **Sản phẩm**, **Đơn giá**, **Số lượng** (có nút +/- để chỉnh), **Thành tiền**, **Thao tác**.
- Thêm cùng một sản phẩm vào giỏ sẽ tăng số lượng, không tạo dòng mới.
- Nút **Xóa sản phẩm** phải có dialog xác nhận trước khi thực hiện.
- Có nút **Tiếp tục mua sắm** để quay về trang chủ.
- Tổng tiền hiển thị nhãn chính xác: **"Tổng cộng"** (không phải "Tổng tạm tính").
- Giỏ hàng trống phải có hình minh họa và thông báo rõ ràng.

### Yêu cầu liên quan (giáp ranh)
- **FR-06**: Ô nhập Số lượng (chỉ nhận số nguyên dương, tối thiểu là 1); nút Thêm vào giỏ hàng có phản hồi trực quan (toast/badge).
- **FR-23**: Link "Giỏ hàng" phải hiển thị badge số lượng sản phẩm trong giỏ.
- **FR-24**: Khi xóa item khỏi giỏ phải có dialog xác nhận; Empty state phải có icon/hình minh họa và message thân thiện.
- **FR-21**: Đơn vị tiền dùng ký hiệu ₫ với định dạng phân cách hàng nghìn.

## User Flows
- Flow 1: Thêm nhanh sản phẩm vào giỏ từ trang chủ (số lượng mặc định 1)
- Flow 2: Chi tiết sản phẩm → nhập số lượng → bấm "Thêm vào giỏ hàng"
- Flow 3: Thêm lại cùng một sản phẩm → số lượng tăng dồn, không tạo dòng mới
- Flow 4: Xem giỏ hàng có sản phẩm (Sản phẩm / Đơn giá / Số lượng / Thành tiền / Thao tác)
- Flow 5: Chỉnh sửa số lượng trong giỏ
- Flow 6: Xóa sản phẩm khỏi giỏ (có dialog xác nhận)
- Flow 7: Xem giỏ hàng trống (hình minh họa + thông báo rõ ràng)
- Flow 8: Bấm "Tiếp tục mua sắm" → quay về trang chủ
- Flow 9: Badge "Giỏ (n)" trên header cập nhật theo giỏ hàng

## Preconditions
- Ứng dụng Mobile đã chạy và kết nối được Backend API (`API_URL` trỏ IP LAN của máy chủ, cổng 3000).
- CSDL có ít nhất 1 sản phẩm (seed data).
- Giỏ hàng được lưu ở state phía client — không yêu cầu đăng nhập để thêm/xem giỏ hàng. Đăng nhập chỉ cần ở bước Thanh toán (FR-08 — ngoài phạm vi chính của feature này).

## Input Variables
- `add_quantity`: số lượng nhập tại màn chi tiết sản phẩm khi bấm "Thêm vào giỏ hàng" (mặc định "1")
- `product_id`: sản phẩm được thêm vào giỏ (cùng sản phẩm đã có / khác sản phẩm)
- `cart_quantity`: số lượng hiển thị và chỉnh sửa trực tiếp trong giỏ (TextInput)
- `cart_state`: trạng thái giỏ — trống / có 1 dòng / có nhiều dòng
- `delete_action`: thao tác xóa dòng sản phẩm trong giỏ

## Expected Outputs
- Danh sách dòng giỏ hàng: Sản phẩm, Đơn giá (₫, phân cách hàng nghìn), Số lượng (có nút +/-), Thành tiền (₫), Thao tác (Xóa)
- Nhãn tổng tiền chính xác: "Tổng cộng: X ₫"
- Giỏ hàng trống: hình minh họa + thông báo rõ ràng + "Tiếp tục mua sắm"
- Dialog xác nhận trước khi xóa sản phẩm
- Chuyển về trang chủ khi bấm "Tiếp tục mua sắm"

## Business Rules (testable)
- BR-1: Thêm cùng sản phẩm → số lượng của dòng tăng, không tạo dòng mới (FR-07)
- BR-2: Số lượng chỉ nhận số nguyên dương, tối thiểu 1 (FR-06)
- BR-3: Thành tiền = Đơn giá × Số lượng; Tổng cộng = Σ Thành tiền (FR-07, FR-21)
- BR-4: Xóa sản phẩm phải có dialog xác nhận trước khi thực hiện (FR-07, FR-24)
- BR-5: Có nút "Tiếp tục mua sắm" quay về trang chủ (FR-07)
- BR-6: Nhãn tổng tiền phải là "Tổng cộng", không phải "Tổng tạm tính" (FR-07)
- BR-7: Giỏ trống phải có hình minh họa + thông báo rõ ràng (FR-07, FR-24)
- BR-8: Cột hiển thị đầy đủ, Số lượng phải có nút +/- (FR-07)
- BR-9: Badge "Giỏ (n)" hiển thị số lượng sản phẩm trong giỏ (FR-23)

## Implementation Evidence (Mobile SUT — `eshop-sut/frontend-mobile/App.js`)
- Màn hình giỏ hàng: `renderCart()` — tiêu đề "Giỏ Hàng".
- Giỏ trống: hiển thị text "Giỏ hàng của bạn đang trống" + link "Tiếp tục mua sắm" (chưa thấy hình minh họa).
- Dòng giỏ: tên sản phẩm, "Giá: …", ô TextInput số lượng (không thấy nút +/-), "Thành tiền: …", liên kết "Xóa" gọi `removeFromCart(index)` trực tiếp (không thấy dialog xác nhận).
- Nhãn tổng tiền: "Tổng tạm tính: …" (khác với "Tổng cộng" theo SRS).
- `addToCart()`: trùng `id` → cộng dồn `quantity`; hiển thị `Alert.alert("Thành công", "Đã thêm vào giỏ hàng")`.
- `normalizeQuantity()`: `parseInt > 0 ? parsed : 1` → giá trị nhỏ nhất hợp lệ là 1.
- Header: nút "Giỏ (n)" hiển thị `cart.length` (số dòng, chưa chắc là tổng số lượng).

## Scope
- **In scope:** FR-07 trên Mobile App + các điểm giáp ranh FR-06 (thêm vào giỏ, số lượng), FR-23/FR-24 (badge, dialog xác nhận, empty state).
- **Out of scope:** FR-08 Checkout (đặt hàng, coupon FR-09), các nền tảng Web/frontend-admin.
