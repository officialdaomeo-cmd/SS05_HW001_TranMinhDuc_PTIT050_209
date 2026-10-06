# Báo cáo: Bài 1 - Khôi phục commit đã mất bằng Git Reflog

## 1. Bối cảnh
Trong quá trình làm việc, tôi đã vô tình chạy lệnh `git reset --hard HEAD~1` khiến cho commit quan trọng nhất chứa mã nguồn vừa chỉnh sửa bị biến mất và không còn hiển thị trên lịch sử của `git log`.

## 2. Các bước thực hiện khôi phục

### Bước 1: Kiểm tra lịch sử commit (xác nhận lỗi)
Đầu tiên, tôi chạy lệnh kiểm tra log để xác nhận rằng commit đã bị mất:
```bash
git log --oneline
```
Kết quả cho thấy commit cần thiết không còn tồn tại trên nhánh hiện tại.

### Bước 2: Tra cứu nhật ký Reflog
Để tìm lại commit đã mất, tôi sử dụng lệnh `git reflog` để xem lại toàn bộ lịch sử các thao tác thay đổi con trỏ HEAD cục bộ:
```bash
git reflog
```
Kết quả hiển thị danh sách các tham chiếu, ví dụ:
```text
a1b2c3d HEAD@{0}: reset: moving to HEAD~1
e3a5b2c HEAD@{1}: commit: Them tinh nang quan trong
...
```
Tại đây, tôi đã tìm thấy mã hash của commit vừa bị mất (ví dụ `e3a5b2c` với thông điệp "Them tinh nang quan trong").

### Bước 3: Khôi phục commit
Sau khi đã xác định được mã hash của commit cần khôi phục, tôi sử dụng lệnh `git reset --hard` để đưa nhánh hiện tại trở về trạng thái của commit đó:
```bash
git reset --hard e3a5b2c
```
*(Trong đó `e3a5b2c` là mã hash tìm được từ bước 2)*

### Bước 4: Kiểm tra lại kết quả
Cuối cùng, tôi chạy lại lệnh `git log` để xác nhận commit đã được khôi phục thành công:
```bash
git log --oneline
```
Lịch sử đã hiển thị lại đúng commit "Them tinh nang quan trong" và toàn bộ nội dung mã nguồn ban đầu đã được khôi phục.
