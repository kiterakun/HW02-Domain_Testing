# Feature Overview

**Feature Name:** Mã Giảm Giá (Discount Coupons) — FR-09
**Description:** Tại bước Checkout, người dùng nhập mã giảm giá. Hệ thống kiểm tra 5 điều kiện (C1–C5) và áp dụng giảm giá theo công thức percent/fixed để tính `final_amount`.
**Version:** SRS v2.0 (2026-05-14)

## Requirements
> **FR-09: Mã Giảm Giá (Coupon)**
>
> Tại bước Checkout, người dùng có thể nhập mã giảm giá. Hệ thống áp dụng giảm giá dựa trên **5 điều kiện** sau, tất cả phải thỏa mãn:
>
> | #   | Điều kiện              | Mô tả                                                       |
> | --- | ---------------------- | ----------------------------------------------------------- |
> | C1  | **Mã tồn tại**         | Mã phải có trong CSDL và đang hoạt động (`is_active = 1`)   |
> | C2  | **Còn hạn sử dụng**    | Ngày hiện tại phải trước `expired_at`                       |
> | C3  | **Đủ ngưỡng đơn hàng** | Tổng đơn hàng **>= (lớn hơn hoặc bằng)** `min_order_amount` |
> | C4  | **Đã đăng nhập**       | Người dùng phải có JWT Token hợp lệ                         |
> | C5  | **Chưa dùng hết lượt** | Số lần đã dùng mã này của user < `max_uses_per_user`        |
>
> **Công thức tính giảm giá:**
>
> - Loại `percent`: `discount_amount = total × discount_value / 100`
> - Loại `fixed`: `discount_amount = discount_value`
> - `final_amount = total - discount_amount`
>
> **Mã giảm giá mẫu trong hệ thống:**
>
> | Mã        | Loại    | Giá trị   | Ngưỡng tối thiểu | Hạn dùng   | Số lần/người |
> | --------- | ------- | --------- | ---------------- | ---------- | ------------ |
> | `SAVE10`  | percent | 10%       | 300,000 ₫        | 2099-12-31 | 1            |
> | `BIGBUY`  | fixed   | 50,000 ₫  | 500,000 ₫        | 2099-12-31 | 1            |
> | `VIP100`  | fixed   | 100,000 ₫ | 300,000 ₫        | 2099-12-31 | 2            |
> | `EXPIRED` | percent | 20%       | 100,000 ₫        | 2020-01-01 | 1            |

## User Flows
- Flow 1 (thành công): Đăng nhập → thêm hàng vào giỏ → Checkout → nhập mã → bấm "Áp dụng" → hệ thống kiểm tra C1–C5 → hiển thị tiền giảm + thành tiền mới → hoàn tất thanh toán → đơn hàng được tạo với tổng đã giảm.
- Flow 2 (thất bại): Nhập mã vi phạm bất kỳ điều kiện C1–C5 → hiển thị thông báo lỗi tương ứng, không áp dụng giảm giá.

## Input Variables
- `code` (string): mã giảm giá người dùng nhập tại Checkout
- `total_amount` (number): tổng tiền giỏ hàng tại thời điểm áp dụng
- JWT token (header `Authorization: Bearer`) — định danh người dùng cho C4/C5

## Expected Outputs
- Thành công: `discount_amount`, `final_amount = total − discount_amount`, thông báo áp dụng thành công + hiển thị "Tiết kiệm" / "Thành tiền" trên UI
- Thất bại: thông báo lỗi tương ứng điều kiện vi phạm (không tồn tại/bị vô hiệu, hết hạn, chưa đủ ngưỡng, đã dùng hết lượt, chưa đăng nhập), không giảm giá

## Business Rules
- R1 (C1): mã phải tồn tại và `is_active = 1`
- R2 (C2): ngày hiện tại < `expired_at`
- R3 (C3): tổng đơn hàng ≥ `min_order_amount`
- R4 (C4): bắt buộc JWT hợp lệ
- R5 (C5): số lần dùng của user < `max_uses_per_user`
- R6: công thức percent — `discount = total × discount_value / 100`
- R7: công thức fixed — `discount = discount_value`
- R8: `final_amount = total − discount_amount`
- R9: cả 5 điều kiện phải thỏa mãn đồng thời — vi phạm bất kỳ điều kiện nào → từ chối

## Key Implementation Evidence (SUT)
- Endpoint áp dụng mã: `backend/server.js:363-441` (`POST /api/apply-coupon`)
- Ghi nhận lượt dùng: `POST /api/coupon-usage` (`backend/server.js:443-454`), bảng `coupon_usage` (`backend/database.js:41-46`)
- UI nhập mã: `frontend-web/src/pages/Checkout.jsx` — ô input "Nhập mã giảm giá..." + nút "Áp dụng", khối kết quả "Tiết kiệm" / "Thành tiền"
- Schema coupon: `backend/database.js:29-38`; seed: `backend/database.js:106-111`
- Tài khoản test: `test@eshop.com / Test1234!` (user, id=2); admin: `admin@eshop.com / Admin123!`
- Lưu ý thực thi: backend DROP toàn bộ bảng và seed lại mỗi lần restart (`backend/database.js:117`) → usage count/đơn hàng bị reset sau restart.

> Nguyên tắc thiết kế: Expected results của mọi test case dựa trên SRS (đặc tả nghiệp vụ đúng) — mục tiêu là kiểm thử hệ thống thực để phát hiện điểm triển khai không tuân thủ đặc tả.
