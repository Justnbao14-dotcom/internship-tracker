# Kiến trúc và cách chạy dự kiến

## Lựa chọn
Ứng dụng Java MVC duy nhất, Thymeleaf cho giao diện, session authentication. Chưa cần React hoặc JWT.
Maven, Java 21, PostgreSQL, Flyway, Spring Data JPA, Spring Security, Bean Validation.
Thêm REST API ở mốc cuối để thực hành HTTP và cung cấp OpenAPI; giao diện đầu không phụ thuộc API này.

## Cấu trúc do Codex tạo ở M0

```text
pom.xml
mvnw / mvnw.cmd / .mvn/wrapper/
compose.yaml
.env.example
src/main/java/com/nguyenbao/PROJECT/
  config/
  auth/
  FEATURE/
  common/error/
src/main/resources/
  application.yml
  db/migration/
  templates/
  static/css/
src/test/java/com/nguyenbao/PROJECT/
.github/workflows/ci.yml
```

PROJECT và FEATURE là chỗ giữ tên trong tài liệu, phải thay bằng tên package và tính năng thật.
Trong mỗi tính năng tách controller, service, repository, DTO và model theo nhu cầu; tránh tạo lớp rỗng cho đủ cấu trúc.

## Vận hành
- M0: Compose chạy PostgreSQL. Chạy app bằng Maven Wrapper với cấu hình từ biến môi trường.
- Mốc cuối: thêm Dockerfile và Compose profile chạy cả app lẫn DB, healthcheck cho DB và retry/kết nối khởi động hợp lý.
- Mỗi repo có tên database và volume riêng; khi chạy song song dùng host port DB khác nhau. App sport-booking dùng 8080, internship-tracker dùng 8081.
- README phải có lệnh PowerShell copy-paste được, yêu cầu môi trường và cách dừng dịch vụ.
- Dữ liệu demo chỉ bật ở profile demo/local và phải tạo lặp an toàn. Không để mật khẩu mặc định hoạt động trong profile production.
- Migration schema chạy trong CI với PostgreSQL thật; GitHub Actions chạy verify.
- Test nghiệp vụ dùng Clock có thể tiêm vào để không phụ thuộc giờ máy.

## Quy ước lỗi REST ở mốc API
400 dữ liệu sai; 401 chưa đăng nhập (API trả JSON, không redirect HTML); 403 thiếu quyền; 404 không có hoặc không thuộc người dùng; 409 xung đột nghiệp vụ.
Lỗi JSON có code, message, fieldErrors, traceId; không lộ stack trace hay SQL.
Phân trang có giới hạn kích thước, whitelist trường sort. Validation và quyền phải thống nhất giữa web và API qua cùng service.
