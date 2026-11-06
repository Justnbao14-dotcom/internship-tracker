# Backlog theo mốc

## M0 — Nền dự án
- Khởi tạo Maven/Spring Boot, wrapper Windows, PostgreSQL Compose, cấu hình biến môi trường và Flyway migration ban đầu.
- Trang chủ Thymeleaf; cấu hình Security cho trang chủ public, các trang chưa triển khai không bị mở bừa.
- README: yêu cầu cài đặt và lệnh PowerShell khởi động.
- Nghiệm thu: build verify và context integration test với PostgreSQL; app mở trang chủ; migration chạy lại không lỗi.

## M1 — Tài khoản và quyền
- Đăng ký CUSTOMER, login/logout session, hash mật khẩu, email chuẩn hóa; lỗi biểu mẫu.
- Seed admin chỉ profile demo/local; URL admin bảo vệ bằng role. CSRF bật.
- Test email trùng sau chuẩn hóa; không tự cấp admin; chưa login không vào trang riêng; CUSTOMER không vào admin; thiếu CSRF bị từ chối.
- Nghiệm thu: đăng ký → login → logout bằng giao diện, quyền được kiểm tra phía server.

## M2 — Sân và lịch
- Admin tạo/sửa thông tin/vô hiệu hóa sân, tạo slot giờ tròn 60 phút; UI public lọc sân và lịch theo ngày.
- Không cho sửa giá/giờ slot đã tạo, không tạo slot trùng; dữ liệu demo giả.
- Test ranh giới ngày theo múi giờ, giá âm, slot trùng, quyền admin.
- Nghiệm thu: admin tạo lịch, khách xem đúng ngày/giá; chưa có booking thật.

## M3 — Đặt/hủy và đồng thời
- BookingService transaction, khóa slot, partial unique index theo SPEC.
- UI đặt, lịch sử, chi tiết và hủy; lỗi conflict dễ hiểu.
- Test: 2 transaction độc lập cùng đặt một slot → đúng 1 BOOKED, bên còn lại xung đột; hai slot khác nhau đều đặt được.
- Test: hủy rồi đặt lại, hủy lặp an toàn, đúng/trước/sau mốc 2 giờ, không đặt quá khứ, không truy cập booking người khác.
- Nghiệm thu: test đồng thời chạy trên PostgreSQL với connection độc lập, không bọc cả test trong một transaction chung; kiểm tra trạng thái DB sau commit.

## M4 — API và portfolio
- Thêm REST API/OpenAPI theo SPEC, phản hồi JSON cho lỗi auth và CSRF; test quyền API.
- Dockerfile, Compose chạy toàn bộ app, GitHub Actions verify; bổ sung hướng dẫn debug trên Windows.
- README có ERD Mermaid, ảnh giao diện tự chụp, lệnh chạy, bộ dữ liệu demo, giải thích chống đặt trùng và giới hạn.
- Nghiệm thu: clone sạch + cấu hình theo README chạy được; CI xanh; demo 3 phút gồm đặt thành công, đặt trùng, hủy và đặt lại.
- CV chỉ ghi kết quả thật. Chưa benchmark thì không ghi số người dùng/request mỗi giây.
