# Báo Cáo Bài 2: Tái Cấu Trúc Lịch Sử Commit Bằng Interactive Rebase

- **Thực hiện:** Học viên  
- **Thư mục bài tập:** `homework/session_05/ex2/`  
- **Mục tiêu:** Sử dụng `git rebase -i` để gộp nhiều commit vụn vặt (squash), sửa lại commit message (reword) và loại bỏ commit rác (drop) nhằm làm sạch lịch sử commit trước khi push lên Remote.

---

## 1. Bối cảnh & Mô phỏng chuỗi commit rác

Trong quá trình phát triển tính năng, nhánh làm việc sinh ra 4 commit phụ/rác như sau:

| Mã Commit (Ví dụ) | Thông điệp Commit (Message) | Mục đích / Xử lý mong muốn |
|---|---|---|
| `c1a2b3c` | `feat: khoi tao module auth` | Commit gốc -> Giữ lại làm nền |
| `d2e3f4g` | `fix typo` | Commit nhỏ -> **Squash** vào commit gốc |
| `h5i6j7k` | `adds utility functions` | Commit nhỏ -> **Squash** vào commit gốc |
| `l8m9n0p` | `add temp file for debug` | File rác `temp.txt` -> **Drop** (xóa bỏ) |

---

## 2. Các bước thực hiện

### Bước 1: Khởi tạo dữ liệu & giả lập lịch sử
Thực hiện tạo lần lượt 4 commit theo gợi ý:
```bash
# Commit 1
echo "// Auth module" > auth.js
git add auth.js
git commit -m "feat: khoi tao module auth"

# Commit 2
echo "// Auth module fixed" > auth.js
git add auth.js
git commit -m "fix typo"

# Commit 3
echo "// Auth module with utilities" >> auth.js
git add auth.js
git commit -m "adds utility functions"

# Commit 4
echo "debug log" > temp.txt
git add temp.txt
git commit -m "add temp file for debug"

```

Kiểm tra lịch sử commit trước khi rebase (`git log --oneline`):

```text
l8m9n0p (HEAD -> main) add temp file for debug
h5i6j7k adds utility functions
d2e3f4g fix typo
c1a2b3c feat: khoi tao module auth

```

---

### Bước 2: Chạy lệnh Interactive Rebase

Mở trình tương tác rebase cho 4 commit gần nhất:

```bash
git rebase -i HEAD~4

```

---

### Bước 3: Cấu hình lệnh Rebase trong Vim / Code Editor

Màn hình cấu hình rebase mở ra, chỉnh sửa các keyword đầu dòng như sau:

```text
pick c1a2b3c feat: khoi tao module auth
squash d2e3f4g fix typo
squash h5i6j7k adds utility functions
drop l8m9n0p add temp file for debug

# Rebase 7f8e9d0..l8m9n0p onto 7f8e9d0 (4 commands)
#
# Commands:
# p, pick <commit> = use commit
# s, squash <commit> = use commit, but meld into previous commit
# d, drop <commit> = remove commit

```

**Giải thích:**

* `pick`: Giữ commit đầu tiên làm commit chính.
* `squash` (hoặc `s`): Gộp commit 2 và 3 vào commit 1.
* `drop` (hoặc `d`): Xóa hoàn toàn commit 4 chứa file `temp.txt`.

*(Lưu lại và đóng trình soạn thảo)*.

---

### Bước 4: Đổi tên thông điệp commit sau khi gộp (Reword)

Trình soạn thảo tiếp theo xuất hiện để nhập thông điệp gộp chung. Xóa các thông điệp cũ và đổi thành:

```text
feat: hoan thien module authentication

```

*(Lưu và đóng trình soạn thảo để hoàn tất Rebase)*.

---

## 3. Kiểm tra kết quả

### 1. Kiểm tra lịch sử commit sau khi Rebase:

```bash
git log --oneline

```

**Kết quả:**

```text
a9b8c7d (HEAD -> main) feat: hoan thien module authentication
7f8e9d0 Initial commit

```

> Lịch sử đã được thu gọn chỉ còn **01 commit sạch duy nhất** đúng với nội dung tính năng.

### 2. Kiểm tra thư mục làm việc:

```bash
ls

```

**Kết quả:**

* File `auth.js` tồn tại và chứa toàn bộ mã nguồn cập nhật từ 3 commit.
* File `temp.txt` đã bị xóa hoàn toàn khỏi Working Directory và Git Tree.


