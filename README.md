KỸ TÍNH ĐỒNG BỘ VÀ XỬ LÝ SỰ CỐ MÃ NGUỒN Phù hợp với GIT & GITHUB
Môn học: CS073
Mục tiêu: Sinh viên làm quen với quy trình làm việc chuyên nghiệp trên GitHub, thành viên các kỹ năng giải quyết xung đột (Xung đột) và phục hồi dữ liệu khi xảy ra sai sót trong quá trình phát triển nhóm.
 THANG ĐIỂM ĐÁNH GIÁ (10 ĐIỂM)
Giảng viên sẽ thực nghiệm dựa trên lịch sử cam kết trên GitHub và sơ đồ nhánh (Graph).

STT	Tình xử lý	Bắc Thái yêu cầu	Điểm
1	Hợp nhất xung đột	Giải quyết xung đột và pushthành côngmain	3.0
2	Đầu tách rời	Phục hồi nguồn mã hóa mới và pushlên GitHub	2.0
3	Khôi phục mềm	Hủy cam kết, che giấu thông tin cảm ứng và gửi pushlại	2.0
4	Đẩy bài viết bị từ chối	Thực hiện pull, mã nguồn từ các thành viên khác vàpush	3.0
Lưu ý: Mọi hành động sử dụng git push --forcehoặc xóa Repo để sao chép lại từ đầu sẽ trừ 2.0 điểm .

II. CHUẨN BỊ MÔI TRƯỜNG (LOCAL)
Sinh viên thực hiện khởi tạo Repository tại máy cục bộ và kết nối với GitHub:

# Khởi tạo thư mục và repo
mkdir Git_Professional_Lab
cd Git_Professional_Lab
git init

# Tạo file ban đầu và commit
echo "Tài liệu hướng dẫn dự án" > README.md
git add README.md
git commit -m "init: Khởi tạo dự án"

# Kết nối với GitHub (Thay URL bằng link repo của bạn trên Classroom)
git remote add origin <URL_REPO_CỦA_BẠN>
git push -u origin main
##️III. NỘI DUNG THỰC HÀNH

TÌNH HUỐNG 1: XỬ LÝ XUNG ĐỘT MÃ NGUỒN (XUNG ĐỘT MERGE)
Yêu cầu: Giải quyết xung đột khi hai nhánh cùng tác động vào một dòng mã.

Tạo đột phá:
git checkout -b dev1
echo "Nội dung từ Dev 1" > app.txt
git add . && git commit -m "feat: Dev 1 cập nhật app.txt"

git checkout main
git checkout -b dev2
echo "Nội dung từ Dev 2" > app.txt
git add . && git commit -m "feat: Dev 2 cập nhật app.txt"
Thực hiện hợp nhất:
git checkout main
git merge dev1   # Thành công
git merge dev2   # Xuất hiện CONFLICT
Khắc phục: Mở app.txt, xóa các ký hiệu <<<<, ====, >>>>, giữ lại nội dung mong muốn.
Đồng danh:
git add app.txt
git commit -m "fix: Giải quyết xung đột giữa dev1 và dev2"
git push origin main
TÌNH HUỐNG 2: PHỤC HỒI TỪ TRẠNG THÁI "TÁO ĐẦU"
Yêu cầu: Lấy một cam kết cũ trong quá khứ và đưa nó trở thành một nhánh phát triển mới trên GitHub.

Tạo ra những điều này:
echo "Update 1" >> README.md && git commit -am "chore: Update 1"
echo "Update 2" >> README.md && git commit -am "chore: Update 2"
Quay về quá khứ:
git log --oneline
git checkout <mã-hash-của-commit-đầu-tiên>
Khắc phục và Đồng bộ:
# Tạo nhánh mới để giữ lại trạng thái này
git checkout -b feature/recovery-point
# Đẩy nhánh mới này lên GitHub
git push -u origin feature/recovery-point
TÌNH HUỐNG 3: HOÀN TÁC VÀ LÀM SẠCH DỮ LIỆU (SOFT RESET)
Yêu cầu: Hủy cam kết chứa thông tin nhạy cảm nhưng không làm mất dữ liệu viết lách.

Mô phỏng lỗi:
git checkout main
echo "API_KEY=secret_12345" > .env
git add .env && git commit -m "feat: Cấu hình API Key"
Khắc phục:
# Hủy commit nhưng giữ lại file trong Staging
git reset --soft HEAD~1
# Chỉnh sửa lại file cho an toàn
echo "API_KEY=********" > .env
git add .env
git commit -m "fix: Bảo mật thông tin API Key"
Đồng danh:
git push origin main
TÌNH HUỐNG 4: QUY TRÌNH PULL - MERGE - PUSH (LÀM VIỆC NHÓM)
Yêu cầu: Xử lý lỗi từ chối lệnh pushkhi kho lưu trữ Remote có thay đổi mới từ người dùng khác.

Mô phỏng lỗi từ Remote: Truy cập GitHub.com, chỉnh sửa tệp trực tiếp README.mdtrên web giao diện và Commit.
Tạo các thay đổi tại Local:
git checkout main
echo "Thay đổi mới tại máy cục bộ" >> README.md
git add . && git commit -m "feat: Cập nhật nội dung tại local"
Thực hiện nguồn mã đẩy:
git push origin main # Sẽ bị báo lỗi [rejected]
Khắc phục (chuẩn quy trình):
# Tải code mới nhất về
git pull origin main
# (Nếu có xung đột, xử lý như Tình huống 1)
# Đẩy lại lên GitHub
git push origin main
NGHIỆM THU
Sau khi hoàn tất, sinh viên giữ màn hình Terminal bằng lệnh sau để kiểm tra sinh viên:

git log --oneline --graph --all
