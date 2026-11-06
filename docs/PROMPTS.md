# Prompt dùng với Codex

## Bắt đầu — M0

```text
Hãy đọc AGENTS.md và toàn bộ docs của repo này. Triển khai chỉ mốc M0 trong BACKLOG.md.
Đây là dự án portfolio Java cho sinh viên chuẩn bị thực tập. Hiện repo chỉ có tài liệu.
Kiểm tra môi trường trước, chọn phiên bản Spring Boot ổn định tương thích Java 21 từ tài liệu chính thức.
Tạo mã, Maven Wrapper gồm mvnw.cmd, cấu hình PostgreSQL, Flyway, trang chủ và hướng dẫn chạy trên Windows.
Hoàn thành các kiểm tra nghiệm thu M0 có thể chạy trong môi trường này. Nếu bị thiếu công cụ hoặc mạng, ghi rõ và cung cấp lệnh kiểm tra ở máy tôi.
Cập nhật PROGRESS.md; chưa triển khai M1. Cuối cùng giải thích các file chính và luồng chạy ứng dụng bằng tiếng Việt.
```

## Tiếp tục một mốc

```text
Đọc AGENTS.md, SPEC.md, BACKLOG.md và PROGRESS.md. Triển khai mốc M1.
Kiểm tra trạng thái repo trước; giữ các thay đổi của tôi. Hoàn thiện chức năng và test có ý nghĩa theo tiêu chí nghiệm thu.
Chạy kiểm tra phù hợp, cập nhật README và PROGRESS.md rồi báo cách demo trên Windows.
Không tự chuyển sang mốc tiếp theo. Giải thích một quyết định kỹ thuật để tôi có thể trả lời phỏng vấn.
```

Thay M1 bằng M2, M3 hoặc M4 khi mốc trước đã nghiệm thu.

## Review trước khi commit

```text
Review diff hiện tại với SPEC.md. Tập trung lỗi nghiệp vụ, phân quyền, transaction, migration và secret.
Nêu phát hiện theo mức độ, vị trí file, cách tái hiện và đề xuất sửa. Nếu không có phát hiện, nói rõ giới hạn kiểm tra.
Chưa sửa code trong lượt review này.
```

## Học lại code

```text
Chọn một luồng nghiệp vụ quan trọng đã triển khai. Giải thích request đi từ giao diện qua controller/service/repository tới database thế nào.
Chỉ ra validation, kiểm tra quyền, ranh giới transaction và tình huống lỗi. Dẫn tới file thực tế.
Hỏi tôi 3 câu phỏng vấn về luồng này rồi chờ tôi trả lời.
```

## Khi gặp lỗi

```text
Đây là lệnh tôi vừa chạy và log lỗi: [dán nội dung, bỏ mật khẩu/token].
Hãy xác định nguyên nhân từ bằng chứng, sửa tối thiểu và chạy lại kiểm tra liên quan.
Không đổi công nghệ, tắt bảo mật hay bỏ test để làm lỗi biến mất. Cập nhật PROGRESS.md.
```
