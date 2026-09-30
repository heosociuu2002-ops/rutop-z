# Rutop Zalo — Kế hoạch triển khai

## Luồng lưu trữ và triển khai đã thống nhất

```text
Source code + migrations
        ↓ push
GitHub (source of truth)
        ↓ kết nối repository / build tự động
Netlify (build và hiển thị website)
        ↓ biến môi trường công khai cần thiết
Supabase (PostgreSQL, quan hệ referral; Auth khi có dashboard)
```

- **GitHub** lưu mã nguồn, cấu hình không bí mật, migrations và tài liệu. Không commit mật khẩu, token, service-role key hoặc file `.env` chứa secret.
- **Netlify** lấy code từ GitHub, build và phục vụ website. Build command, thư mục publish và redirect phải được xác định theo framework sau khi có source. Secret được đặt trong Netlify environment variables, không ghi vào `netlify.toml`.
- **Supabase** lưu dữ liệu thành viên, đăng ký và quan hệ người giới thiệu. Schema thay đổi được lưu bằng migrations trong repository để có lịch sử và tái triển khai được. Dùng publishable/anon key phía client cùng RLS phù hợp; tuyệt đối không đưa service-role key vào trình duyệt.
- Supabase Auth được bổ sung khi xây dashboard và mô hình đăng nhập/role đã chốt. Role và quyền thao tác phải được kiểm tra ở backend/database, không chỉ ẩn giao diện.

## Hiện trạng đã kiểm tra

Workspace hiện chỉ có `RUTOP_ZALO_PROJECT.md`. Không tìm thấy source code, `package.json`, `netlify.toml`, thư mục Supabase, Git repository, remote GitHub hoặc cấu hình kết nối dịch vụ. Vì vậy chưa thể xác định framework/build settings hoặc thực hiện push/deploy/database migration.

## Kiến trúc MVP đề xuất

1. Kiểm kê source hiện có trước khi thay đổi stack; xác định framework, routes, build, backend và trạng thái deploy.
2. Tạo repository GitHub cho source, tài liệu và migration; thêm `.gitignore` và `.env.example` không chứa secret.
3. Kết nối repository với Netlify; cấu hình build/publish theo framework, preview deploy và biến môi trường theo từng context.
4. Tạo Supabase schema ban đầu cho `profiles`/members, registrations, referral relationships và audit events. Dùng UUID, khóa ngoại, unique constraint cho số điện thoại/mã referral theo nghiệp vụ; không chỉ lưu bộ đếm tổng.
5. Bật RLS cho các bảng trong schema public và chỉ cấp quyền theo vai trò/use case cụ thể. Thiết kế migration và kiểm tra truy cập trước khi đưa dữ liệu thật vào.
6. Làm MVP theo thứ tự: landing + referral code → form đăng ký → lưu referral relationship → admin duyệt/từ chối → dashboard CTV/Admin → referral tree.
7. Bổ sung Supabase Auth cùng dashboard, liên kết tài khoản Auth với profile bằng ID ổn định; đặc quyền admin không lấy từ user-editable metadata.

## Thứ tự triển khai và kiểm tra

### Giai đoạn A — Source và deploy

- Nhận/khôi phục repository source hiện tại và remote GitHub.
- Xác định framework, build command, publish directory, routing và Node version.
- Thiết lập GitHub → Netlify deploy, trước tiên trên preview.
- Kiểm tra build và các route trực tiếp sau deploy.

### Giai đoạn B — Database referral

- Tạo Supabase project và migration đầu tiên.
- Thiết kế member/profile, registration, referral relationship, trạng thái và audit.
- Bật RLS, grants và chính sách tối thiểu theo access model đã chốt.
- Dùng dữ liệu thử để xác minh tạo đăng ký, gắn referrer và đọc cây referral.

### Giai đoạn C — Đăng nhập và dashboard

- Chốt ai được tạo tài khoản, cách admin được cấp quyền, và luồng quên/đổi mật khẩu.
- Kết nối Supabase Auth, bảo vệ route và kiểm soát quyền cả ở database/API.
- Triển khai các màn hình theo tài liệu `RUTOP_ZALO_PROJECT.md`.

## Các đầu vào còn thiếu để thực hiện push/deploy

- Thư mục hoặc URL repository GitHub đang chứa source Rutop (nếu source nằm ở nơi khác).
- Quyền truy cập GitHub và xác nhận repository đích nếu cần tạo repository mới.
- Netlify site đích (hoặc xác nhận cần tạo site mới) và quyền truy cập tương ứng.
- Supabase project đích (hoặc xác nhận cần tạo project mới); không gửi secret trong chat hoặc commit vào Git.

Sau khi có source và project đích, tiếp tục kiểm kê code, chuẩn bị cấu hình/migrations, review diff, rồi push để Netlify tự build. Không thể suy ra repository hoặc project đích từ tài liệu hiện tại.

