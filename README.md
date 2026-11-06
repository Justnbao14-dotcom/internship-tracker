# Sport Booking

Sports court booking application built with Java and Spring Boot.

Ứng dụng đặt sân thể thao cho khách hàng và một đơn vị vận hành sân, tập trung vào chống đặt trùng bằng ràng buộc database và transaction.

**Trạng thái:** bộ khung tài liệu; chưa có ứng dụng chạy được. Codex bắt đầu từ docs/PROMPTS.md → M0.

## MVP
- Khách đăng ký, đăng nhập, xem sân và khung giờ trống, đặt/hủy sân, xem lịch sử của mình.
- ADMIN quản lý sân, tạo khung giờ, xem các lượt đặt.
- Chỉ dùng giá tiền tham khảo và trạng thái đặt sân; chưa thanh toán thật, chưa có nhiều chủ sân.

## Stack dự kiến
Java 21, Spring Boot, Thymeleaf, PostgreSQL, Spring Data JPA, Spring Security, Flyway, Maven, JUnit, Testcontainers.
Package gốc: com.nguyenbao.sportbooking. Cổng app: 8080.

## Tài liệu
- docs/SPEC.md: chức năng, dữ liệu, quy tắc và API dự kiến.
- docs/ARCHITECTURE.md: cấu trúc và quy ước triển khai.
- docs/BACKLOG.md: mốc công việc và nghiệm thu.
- docs/PROMPTS.md: prompt cho Codex.
- docs/PROGRESS.md: bằng chứng tiến độ.

Khi triển khai, cập nhật README này với cách chạy thực tế, biến môi trường, tài khoản demo, ảnh giao diện và kết quả test. Chỉ ghi chức năng đã hoàn thành.
