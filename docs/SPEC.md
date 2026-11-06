# Sport Booking — Đặc tả MVP

## Người dùng và màn hình
- Public: trang chủ, danh sách sân, chi tiết sân/lịch theo ngày, đăng ký, đăng nhập.
- CUSTOMER: các trang public; đặt một khung giờ, danh sách đặt của mình, chi tiết và hủy đặt.
- ADMIN: dashboard, danh sách/form sân, danh sách/form tạo khung giờ, danh sách lượt đặt.
- Chỉ một đơn vị vận hành. Đăng ký luôn tạo CUSTOMER; không có trường chọn ADMIN. ADMIN tạo bằng seed demo có kiểm soát.

## Quy tắc
- Một lượt đặt tương ứng đúng một slot có thời lượng 60 phút. Slot bắt đầu vào giờ tròn theo Asia/Ho_Chi_Minh; không hỗ trợ giờ lẻ hoặc đặt nhiều slot trong một đơn ở MVP.
- Slot là khoảng [start, end), end = start + 60 phút. Sân không hoạt động hoặc slot đã bắt đầu thì không được đặt.
- ADMIN tạo trước slot, giá không âm; UNIQUE(court_id, starts_at). Không thay đổi thời gian/giá slot đã phát hành; vô hiệu hóa sân chỉ chặn lượt đặt mới, giữ lịch sử.
- Chỉ đặt slot ở tương lai. Giá được sao chép vào booking tại lúc đặt, không tính lại theo giá hiện tại.
- Khách hủy booking của mình nếu còn ít nhất 2 giờ trước lúc bắt đầu (đúng 2 giờ được phép); ADMIN không có quyền vượt quy tắc này trong MVP.
- BOOKED → CANCELLED; chưa hỗ trợ hoàn tất/no-show/thanh toán. Hủy lại booking đã hủy của chính mình trả trạng thái hiện tại, không tạo lỗi hay thay đổi thêm.
- Sau khi hủy hợp lệ, người khác được đặt lại slot. Người dùng không xem được booking của người khác.
- Tên sân trống, ngày sai, giá âm, email trùng phải có phản hồi rõ ràng.

## Mô hình dữ liệu

| Bảng | Cột chính / ràng buộc |
| --- | --- |
| users | id UUID PK, email chuẩn hóa UNIQUE, password_hash, display_name, role CUSTOMER/ADMIN, created_at |
| courts | id UUID PK, name, sport_type, location, description, active |
| slots | id UUID PK, court_id FK, starts_at timestamptz, ends_at timestamptz, price numeric(12,0), UNIQUE(court_id, starts_at), CHECK thời lượng và giá |
| bookings | id UUID PK, user_id FK, slot_id FK, status BOOKED/CANCELLED, price_snapshot numeric(12,0), created_at, cancelled_at nullable |

CHECK trạng thái/thời gian hợp lệ, NOT NULL và FK tại DB. Không xóa cứng sân/slot có lịch sử.
Index truy vấn: slots(court_id, starts_at), bookings(user_id, created_at), bookings(slot_id).

## Chống đặt trùng — yêu cầu bắt buộc
Tạo PostgreSQL partial unique index trên bookings(slot_id) WHERE status='BOOKED'.
Mọi thao tác đặt/hủy khóa cùng hàng slot bằng SELECT FOR UPDATE trong transaction, rồi đọc lại và kiểm tra trạng thái/giờ bằng Clock.
Đặt sân phải ghi booking và commit nguyên tử. Ràng buộc DB là lớp bảo vệ cuối cùng khi nhiều instance cùng chạy.
Chuyển vi phạm unique sau rollback thành lỗi nghiệp vụ SLOT_UNAVAILABLE (HTTP 409); không tiếp tục truy vấn trong transaction đã hỏng.
Hủy cập nhật đúng booking và trạng thái trong transaction, sau khi kiểm tra chủ sở hữu.
Không dùng synchronized trong Java làm cơ chế bảo vệ duy nhất. Kiểm thử đồng thời trên PostgreSQL thật.

## API dự kiến ở M4
Session authentication dùng chung ứng dụng; các POST vẫn yêu cầu CSRF token. API chưa có trước M4.

| Method | Route | Quyền / hành vi |
| --- | --- | --- |
| GET | /api/courts | Public, lọc môn thể thao và phân trang |
| GET | /api/courts/{id}/slots?date=YYYY-MM-DD | Public, tính khoảng ngày địa phương sang Instant, trả giờ và khả dụng |
| POST | /api/bookings | CUSTOMER, body chỉ có slotId, trả 201 |
| GET | /api/me/bookings | CUSTOMER, chỉ dữ liệu của principal |
| POST | /api/bookings/{id}/cancel | CUSTOMER chủ sở hữu, trả 200; 404 nếu của người khác |

Thao tác admin qua MVC ở MVP. Thông tin khách không xuất hiện ở API public.

## Ngoài phạm vi
Nhiều chủ sân, thanh toán thật, email/SMS, khuyến mại, rating, websocket, microservices.
