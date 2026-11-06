# Hướng dẫn cho Codex

## Mục tiêu và phạm vi
Đọc README.md, docs/SPEC.md, docs/ARCHITECTURE.md, docs/BACKLOG.md và docs/PROGRESS.md trước khi sửa.
Chỉ triển khai mốc được yêu cầu. Nếu yêu cầu đã rõ, tự giải quyết lựa chọn thông thường và thực hiện đến khi nghiệm thu được.
Không tự thêm microservices, Kafka, Redis, thanh toán thật, đăng nhập mạng xã hội hoặc AI chatbot.
Không triển khai lên dịch vụ bên ngoài nếu chưa được yêu cầu.

## Quy ước
- Java 21, Maven Wrapper; Spring Boot bản ổn định còn được hỗ trợ và tương thích Java 21. Khi khởi tạo, kiểm tra nguồn chính thức và ghi phiên bản đã chọn vào README. Không dùng snapshot.
- Ứng dụng MVC duy nhất với Thymeleaf, PostgreSQL, Spring Data JPA, Spring Security, Bean Validation, Flyway.
- Đặt lớp theo tính năng; Controller → Service → Repository. DTO cho đầu vào web/API; không trả Entity trực tiếp ở API.
- Constructor injection, transaction ở Service; không dùng Lombok ở bản đầu để người học thấy rõ Java.
- Tiền dùng BigDecimal và NUMERIC; thời gian lưu bằng Instant/timestamptz, hiển thị theo Asia/Ho_Chi_Minh.
- Session cookie HttpOnly; giữ CSRF cho thao tác thay đổi dữ liệu. BCrypt qua PasswordEncoder. Không dùng JWT cho giao diện server-rendered này.
- Không nhận userId/role từ form để xác định danh tính/quyền. Lấy principal phía server. Kiểm tra quyền với từng tài nguyên.
- Flyway quản lý schema, Hibernate ddl-auto=validate. Không sửa migration đã áp dụng; thêm migration mới.
- Không commit secret, .env, target, dữ liệu DB, log hoặc thông tin thật. .env.example chỉ có giá trị mẫu.
- Compose cấp biến cho container; không giả định Spring tự đọc .env khi chạy trực tiếp trên máy. Hướng dẫn cách đặt biến trong PowerShell và IDE.
- Giữ giao diện đơn giản, responsive, có thông báo lỗi nhập liệu và trạng thái trống.

## Kiểm tra và báo cáo
- Test nghiệp vụ quan trọng bằng JUnit, kiểm tra quyền và persistence bằng integration test trên PostgreSQL/Testcontainers khi phù hợp.
- Không viết test chỉ xác nhận getter/setter hoặc lặp nguyên logic triển khai.
- Dùng Maven Wrapper, có mvnw.cmd cho Windows. Chạy verify trước khi kết thúc mốc; ghi chính xác pass/fail/chưa chạy và lý do.
- Không tuyên bố build thành công hoặc test pass nếu chưa chạy. Không đánh dấu hoàn tất chỉ vì đã sinh file.
- Cập nhật docs/PROGRESS.md: phần đã làm, lệnh đã chạy, kết quả, giới hạn và mốc tiếp theo.
- Kết thúc bằng: thay đổi chính, cách chạy thử trên Windows, kiểm thử và một câu hỏi tự ôn kèm gợi ý ngắn.
- Giải thích bằng tiếng Việt; identifier và commit message dùng tiếng Anh.
