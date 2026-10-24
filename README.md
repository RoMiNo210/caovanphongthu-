PHẦN MỀM.STORE — BỘ KHỞI ĐỘNG TỪ A–Z
==========================================

BỘ FILE
- index.html: website (cần dán URL + publishable key Supabase vào 2 dòng cấu hình).
- setup.sql: bảng dữ liệu, RLS, trigger tạo profile, chức năng đặt hàng.
- README.txt: hướng dẫn này.

PHẦN 1 — TẠO SUPABASE MỚI
1. Vào https://supabase.com/dashboard và đăng nhập.
2. Chọn New project.
3. Đặt tên ví dụ: phanmem-store-prod.
4. Tạo mật khẩu database mạnh và lưu ở nơi riêng tư. Chọn region gần Việt Nam nếu có.
5. Chờ dự án khởi tạo xong.
6. Vào SQL Editor > New query.
7. Mở file setup.sql, sao chép toàn bộ, dán vào SQL Editor, bấm Run.
8. Chờ thông báo thành công. Nếu báo lỗi, dừng lại và gửi nguyên văn lỗi; không chạy các lệnh xóa dữ liệu ngẫu nhiên.

PHẦN 2 — LẤY URL VÀ KHÓA CÔNG KHAI
1. Trong dashboard dự án Supabase, mở Project Settings > API (hoặc mục Connect / API Keys tùy giao diện).
2. Sao chép Project URL.
3. Sao chép publishable key (thường bắt đầu sb_publishable_...) hoặc anon public key nếu giao diện dự án dùng tên cũ.
4. KHÔNG dùng secret key, service_role key hoặc mật khẩu database trong website/GitHub.
5. Mở index.html bằng trình soạn thảo văn bản.
6. Tìm:
   const SUPABASE_URL = "DAN_SUPABASE_PROJECT_URL_VAO_DAY";
   const SUPABASE_PUBLISHABLE_KEY = "DAN_SUPABASE_PUBLISHABLE_KEY_VAO_DAY";
7. Thay phần chữ DAN_... bằng thông tin vừa sao chép, giữ dấu ngoặc kép. Ví dụ:
   const SUPABASE_URL = "https://xxxx.supabase.co";
   const SUPABASE_PUBLISHABLE_KEY = "sb_publishable_...";
8. Lưu file.

PHẦN 3 — TẠO WEBSITE GITHUB PAGES
1. Vào https://github.com/new
2. Repository name: phanmem-store (hoặc tên khác).
3. Chọn Public, bấm Create repository.
4. Trong repository, Add file > Upload files.
5. Tải lên index.html đã sửa. (Có thể tải README.txt và setup.sql lên để lưu tài liệu, nhưng không bắt buộc.)
6. Bấm Commit changes.
7. Vào Settings > Pages.
8. Trong Build and deployment chọn Deploy from a branch.
9. Branch: main; folder: / (root); bấm Save.
10. Chờ vài phút, mở URL GitHub Pages hiển thị trong Settings > Pages.
Nếu dùng repo tên phanmem-store, thường URL dạng https://TEN-TAI-KHOAN.github.io/phanmem-store/ .

PHẦN 4 — TẠO TÀI KHOẢN VÀ CẤP QUYỀN ADMIN AN TOÀN
1. Mở website GitHub Pages, đăng ký bằng email bạn muốn dùng làm quản trị.
2. Nếu Supabase yêu cầu xác nhận email, mở hộp thư và xác nhận. Nếu không thấy email, kiểm tra Spam.
3. Đăng nhập website ít nhất một lần để trigger tạo profile.
4. Vào Supabase > SQL Editor > New query, chạy lệnh dưới đây, thay email nếu cần:

update public.profiles p
set role = 'admin', display_name = 'CAO VĂN PHONG THƯ'
from auth.users u
where p.id = u.id
  and lower(u.email) = lower('caovanphongthu@gmail.com');

5. Kiểm tra kết quả bằng:
select u.email, p.display_name, p.role
from auth.users u join public.profiles p on p.id = u.id
where lower(u.email) = lower('caovanphongthu@gmail.com');

Kết quả cần có role = admin. Nếu không có dòng nào, tài khoản chưa được tạo/xác nhận hoặc email khác. Đăng ký trước rồi chạy lại câu UPDATE.
Không cho người khác quyền admin.

PHẦN 5 — KIỂM TRA
- Mở website và thấy thông báo “Đã kết nối Supabase”.
- Thấy sản phẩm demo “Windows 11 Pro - Demo”.
- Đăng ký tài khoản phụ để thử.
- Đặt thử đơn hàng; kiểm tra đơn ở tài khoản và trang quản trị.
- Dùng admin để thêm/sửa sản phẩm, đổi trạng thái đơn, xem yêu cầu nạp.
- Xóa hoặc chỉnh sản phẩm demo trước khi mở bán thật.

LƯU Ý VỀ THANH TOÁN
Đây chỉ là hệ thống ghi nhận đơn hàng và yêu cầu nạp tiền. Nó không tự tích hợp cổng thanh toán, không tự chuyển tiền, và không có ví/số dư thực. Chỉ xác nhận nạp tiền sau khi kiểm tra giao dịch thực tế. Không yêu cầu khách gửi mật khẩu ngân hàng, mã OTP hoặc thông tin thẻ đầy đủ.

KHẮC PHỤC NHANH
- “Chưa cấu hình Supabase”: chưa thay 2 dòng trong index.html hoặc chưa deploy file đã sửa.
- “Invalid API key”: dán sai key hoặc key không thuộc đúng dự án.
- “relation does not exist”: chưa chạy hết setup.sql trong dự án Supabase mới.
- Đăng ký được nhưng không vào được: kiểm tra email confirmation trong Supabase Authentication.
- Không thấy admin: chạy lại SQL cấp quyền admin sau khi đăng ký đúng email.
- Lỗi khi chạy SQL: gửi nguyên văn thông báo lỗi để sửa đúng chỗ; đừng gửi secret key.
