# RUTOP ZALO — Tóm tắt yêu cầu & định hướng xây dựng

## 1. Mục tiêu dự án

Xây dựng phần mềm **Rutop Zalo** theo bản thiết kế/mockup người dùng đã cung cấp.

Mục tiêu là tạo một hệ thống quản lý cộng đồng/CTV dựa trên **Referral**:

> CTV giới thiệu người mới → người mới đăng ký thành viên → Admin kiểm tra/duyệt → thành viên tham gia Group Zalo → thành viên có thể đăng ký trở thành CTV → hệ thống cấp link/QR giới thiệu riêng → tiếp tục phát triển mạng lưới.

Người dùng hiện đang **đẩy code lên Netlify** và muốn tiếp tục xây dựng dự án từ code hiện có.

---

## 2. Hình ảnh thiết kế tham chiếu

Người dùng đã cung cấp một ảnh mockup tổng thể của Rutop.

Mockup gồm:

- Logo RUTOP
- Landing Page
- Form đăng ký người được mời
- Trang đăng ký thành công
- CTV Dashboard
- Admin Dashboard
- Quản lý đăng ký
- Chi tiết đăng ký
- Quản lý CTV
- Referral Tree
- Thiết kế Database
- Quy trình từng module
- Các tính năng bổ sung giai đoạn sau

### Phong cách UI

- Hiện đại, chuyên nghiệp
- Tông trắng + xanh/tím thương hiệu Rutop
- Dashboard dạng SaaS
- Card bo góc
- Sidebar
- Bảng dữ liệu
- Badge trạng thái
- QR Code
- Biểu đồ/thống kê
- Responsive

**Lưu ý:** Ảnh mockup là nguồn tham chiếu UI/UX, không nhất thiết phải sao chép pixel-perfect. Ưu tiên giao diện sạch, dễ dùng và có khả năng mở rộng.

---

# 3. Các Role trong hệ thống

## 3.1 ADMIN

Admin quản lý toàn hệ thống:

- Dashboard tổng quan
- Quản lý đăng ký
- Duyệt / từ chối đăng ký
- Xem chi tiết đăng ký
- Quản lý CTV
- Quản lý thành viên
- Quản lý Group Zalo
- Xem Referral Tree
- Quản lý voucher/chương trình thưởng
- Báo cáo/thống kê
- Export Excel
- Theo dõi logs
- Cấu hình hệ thống

## 3.2 CTV

CTV có:

- Dashboard cá nhân
- Link giới thiệu riêng
- QR Code giới thiệu
- Danh sách người được giới thiệu
- Theo dõi trạng thái từng người
- Số lượng người đã giới thiệu
- Số người đã duyệt
- Theo dõi thành tích/mốc thưởng
- Group Zalo
- Thông tin cá nhân

## 3.3 MEMBER

Member là thành viên thông thường:

- Thông tin cá nhân
- Người giới thiệu
- Group Zalo
- Trạng thái tài khoản

Member có thể đăng ký trở thành CTV.

Luồng:

```text
MEMBER
  ↓
Đăng ký trở thành CTV
  ↓
Admin duyệt / kích hoạt
  ↓
CTV
  ↓
Có link giới thiệu riêng
```

---

# 4. Luồng nghiệp vụ cốt lõi

```text
CTV
 ↓
Có link giới thiệu riêng
 ↓
Người được giới thiệu bấm link
 ↓
Form đăng ký
 ↓
Hệ thống ghi nhận người giới thiệu
 ↓
Tạo mã đăng ký
 ↓
Tạo QR / Link Group Zalo
 ↓
Admin kiểm tra
 ├── Từ chối
 └── Duyệt
       ↓
   Thành viên hợp lệ
       ↓
   Tham gia Group Zalo
       ↓
   Nếu muốn làm CTV
       ↓
   Đăng ký CTV
       ↓
   Có link giới thiệu riêng
       ↓
   Tiếp tục giới thiệu người mới
```

---

# 5. Các module chính

## Module 01 — Landing Page

Trang giới thiệu chương trình Rutop.

Nội dung dự kiến:

- Hero/banner
- Giới thiệu Rutop
- Lợi ích
- Sản phẩm/chương trình
- Cộng đồng
- Hỗ trợ
- CTA đăng ký

CTA chính:

> Tham gia ngay

Có thể nhận referral code từ URL.

Ví dụ:

```text
https://rutop.vn/r/CTV001
```

Khi người dùng vào URL này và đăng ký, hệ thống phải ghi nhận:

```text
referrer = CTV001
```

---

## Module 02 — Form đăng ký thành viên

Form dự kiến:

- Họ và tên
- Số điện thoại
- Zalo
- Người giới thiệu
- Điều khoản/đồng ý
- Submit

Sau khi submit:

```text
Create registration
Create/reference user/member
Attach referrer
Generate registration code
```

Ví dụ:

```text
DK001
DK002
DK003
```

---

## Module 03 — Trang đăng ký thành công

Hiển thị:

- Thông báo đăng ký thành công
- Mã đăng ký
- QR Code Group Zalo
- Link Group Zalo
- Trạng thái đăng ký

Ví dụ:

```text
Đăng ký thành công!

Mã đăng ký: DK001

[ QR CODE ]

[ Tham gia Group Zalo ]
```

Nếu cần duyệt trước khi tham gia:

```text
Trạng thái: Chờ Admin duyệt
```

---

# 6. CTV Dashboard

Sidebar dự kiến:

- Trang chủ
- Thông tin cá nhân
- Link giới thiệu
- Danh sách người giới thiệu
- Thống kê
- Group Zalo

Dashboard hiển thị:

```text
Tổng người giới thiệu
Đã duyệt
Chờ duyệt
Từ chối
```

Ví dụ:

```text
Người giới thiệu: 125
Đã duyệt: 108
Chờ duyệt: 12
Từ chối: 5
```

### Link giới thiệu

Ví dụ:

```text
https://rutop.vn/r/CTV001
```

Có nút:

- Sao chép link
- Hiển thị QR
- Download/hiển thị QR

### Danh sách referral

Các cột:

- Mã đăng ký
- Họ tên
- SĐT
- Ngày đăng ký
- Trạng thái
- Ngày duyệt

---

# 7. Admin Dashboard

Dashboard tổng quan.

Các KPI:

```text
Tổng đăng ký
Chờ duyệt
Tổng thành viên
Tổng CTV
Từ chối
```

Ví dụ:

```text
Đăng ký: 1,250
Chờ duyệt: 83
Thành viên: 1,120
CTV: 145
```

Có thể bổ sung:

- Thành viên mới theo ngày/tuần/tháng
- CTV tăng trưởng
- Referral tăng trưởng
- Top CTV
- Tỷ lệ duyệt
- Biểu đồ

---

# 8. Quản lý đăng ký Admin

Danh sách đăng ký:

```text
Mã đăng ký
Người đăng ký
Người giới thiệu
SĐT
Ngày đăng ký
Trạng thái
Thao tác
```

Trạng thái:

- Chờ duyệt
- Đã duyệt
- Từ chối

Thao tác:

- Xem
- Duyệt
- Từ chối

Có filter:

- Trạng thái
- Khoảng ngày
- Search
- Người giới thiệu

Có thể export Excel.

---

# 9. Chi tiết đăng ký

Hiển thị:

## Thông tin đăng ký

- Mã đăng ký
- Người đăng ký
- SĐT
- Zalo
- Ngày đăng ký
- Người giới thiệu
- Trạng thái

## Group Zalo

- Tên group
- Link
- QR Code
- Trạng thái

## Admin action

```text
[ Duyệt ]
[ Từ chối ]
```

Có thể có ghi chú admin.

---

# 10. Quản lý CTV

Danh sách CTV:

```text
Mã CTV
Họ tên
SĐT
Ngày tham gia
Số referral
Số member hợp lệ
Trạng thái
Thao tác
```

Chức năng:

- Xem
- Khóa/mở khóa
- Xem referral
- Xem cây
- Copy link
- QR

---

# 11. Quản lý Group Zalo

Cần quản lý:

- Group name
- Group ID nếu có
- Link
- QR Code
- Trạng thái
- Ngày tạo
- Số thành viên (nếu có dữ liệu)

Lưu ý:

Việc lấy chính xác dữ liệu thành viên từ Zalo phụ thuộc API/quyền tích hợp thực tế. Không giả định rằng website có thể tự động đọc mọi dữ liệu Group Zalo nếu chưa có API/connector phù hợp.

---

# 12. Referral Tree

Đây là thành phần quan trọng.

Ví dụ:

```text
ADMIN
  │
  └── Nguyễn Văn A
      CTV001
      │
      ├── Trần Thị B
      │   CTV002
      │   │
      │   ├── Phạm D
      │   └── Hoàng E
      │
      └── Lê Văn C
          MEMBER
```

Mỗi node có thể xem:

- Họ tên
- Mã
- Role
- SĐT
- Người giới thiệu
- Ngày tham gia
- Trạng thái
- Số referral

Hệ thống nên hỗ trợ nhiều cấp, không hard-code chỉ 1 hoặc 2 cấp.

---

# 13. Referral Engine

Nên coi Referral Engine là phần lõi của hệ thống.

Logic cơ bản:

```text
referral_link
      ↓
landing page
      ↓
registration
      ↓
referrer_id
      ↓
member
      ↓
referral relationship
      ↓
referral tree
```

Không nên chỉ lưu tổng số referral.

Phải lưu quan hệ:

```text
parent/referrer
child/referred user
```

để có thể xây Referral Tree và thống kê về sau.

---

# 14. Voucher / Milestone Engine

Mockup có định hướng chương trình thưởng theo số người giới thiệu.

Ví dụ:

```text
CTV đạt 10 người
      ↓
Voucher A

CTV đạt 20 người
      ↓
Voucher B

CTV đạt 50 người
      ↓
Voucher C
```

Không nên hard-code:

```text
if count == 10
```

Nên có cấu hình:

```text
voucher_rules
```

Ví dụ:

```text
rule:
  threshold = 10
  reward_type = voucher
  reward_value = ...
```

Lưu lịch sử đạt mốc:

```text
CTV
Milestone
Achieved At
Voucher
Status
```

---

# 15. Database định hướng

Mockup ban đầu có:

```text
users
referral_links
registrations
groups
referrals
logs
```

Đề xuất mở rộng:

```text
users
roles
referral_links
registrations
groups
group_members
referrals
voucher_rules
vouchers
voucher_claims
notifications
logs
system_settings
```

## users

Các field dự kiến:

```text
id
name
phone
zalo_id
role
status
created_at
updated_at
```

## referral_links

```text
id
user_id
referral_code
url
status
created_at
updated_at
```

## registrations

```text
id
registration_code
user_id
referrer_user_id
status
created_at
updated_at
```

## groups

```text
id
name
link
qr_code
status
created_at
updated_at
```

## referrals

```text
id
referrer_user_id
referred_user_id
level
status
created_at
```

Có thể điều chỉnh schema theo database/stack thực tế sau khi kiểm tra source code.

---

# 16. Logs

Cần log các hành động quan trọng:

```text
Admin approve registration
Admin reject registration
Create CTV
Create referral link
Update user
Generate voucher
Change group
Login
Logout
```

Field dự kiến:

```text
id
user_id
action
entity_type
entity_id
content
ip_address
created_at
```

---

# 17. Quy trình phát triển

Không nên xây tất cả cùng lúc.

Đề xuất:

## Phase 1 — Core

```text
Database
↓
User / Role
↓
Registration
↓
Referral
↓
Status
```

## Phase 2 — CTV

```text
CTV Dashboard
↓
Referral Link
↓
QR
↓
Referral List
↓
Statistics
```

## Phase 3 — Admin

```text
Admin Dashboard
↓
Registration Management
↓
CTV Management
↓
Member Management
↓
Group Management
```

## Phase 4 — Referral Tree

```text
Referral relationship
↓
Tree visualization
↓
Multi-level tracking
```

## Phase 5 — Voucher

```text
Referral count
↓
Milestone Engine
↓
Voucher Rule
↓
Voucher
↓
Notification
```

## Phase 6 — Zalo

```text
Group
↓
Link
↓
QR
↓
Integration/API nếu khả thi
```

## Phase 7 — Reporting

```text
Dashboard
↓
Statistics
↓
Charts
↓
Excel export
```

---

# 18. Yêu cầu kỹ thuật quan trọng

Người dùng đang triển khai trên **Netlify**.

Khi kiểm tra source code cần xác định:

- Framework
- Build command
- Publish directory
- Node version
- Environment variables
- API/backend hiện tại
- Database hiện tại
- Netlify Functions nếu sử dụng
- Routing/SPA redirect
- Authentication

Không tự ý thay đổi stack nếu chưa kiểm tra code hiện tại.

Nếu backend/database chưa có, đề xuất giải pháp phù hợp với kiến trúc Netlify và khả năng mở rộng.

---

# 19. Nguyên tắc khi code

1. Ưu tiên tái sử dụng code hiện có.
2. Không phá vỡ chức năng đang chạy.
3. UI bám sát mockup nhưng ưu tiên UX tốt.
4. Responsive desktop/tablet/mobile.
5. Tách component rõ ràng.
6. Không hard-code nghiệp vụ có khả năng cấu hình.
7. Referral phải là quan hệ dữ liệu thật, không chỉ là counter.
8. Role/permission phải được kiểm soát ở backend/API, không chỉ ẩn menu frontend.
9. Validation form đầy đủ.
10. Có trạng thái loading/error/empty.
11. Có audit/log cho thao tác quan trọng.
12. Thiết kế để sau này tích hợp Zalo/API.
13. Build phải chạy được trên Netlify.
14. Không commit secret/API key.
15. Environment variables phải được tách riêng.

---

# 20. Những gì đã thống nhất với người dùng

Người dùng muốn:

- Xây phần mềm tên **Rutop Zalo**
- Lấy ảnh mockup đã cung cấp làm định hướng tổng thể
- Xây theo từng module
- Không chỉ làm mockup mà hướng tới hệ thống có thể chạy thật
- Người dùng đang đẩy code lên Netlify
- Muốn giảm thao tác thủ công, hệ thống hóa quy trình
- Ưu tiên cấu trúc rõ ràng, dễ mở rộng

---

# 21. Việc cần làm tiếp theo

Khi đưa file này sang Codex, hãy yêu cầu Codex:

### Bước 1

Kiểm tra repository/source code hiện tại:

```text
- package.json
- framework
- src/
- components/
- pages/
- app/
- public/
- netlify.toml
- functions/
- database config
- env references
```

### Bước 2

Xác định:

```text
Đã có gì?
Thiếu gì?
Phần nào đang chạy?
Phần nào cần sửa?
```

### Bước 3

Không xây lại toàn bộ ngay.

Trước tiên lập:

```text
IMPLEMENTATION_PLAN.md
```

với:

- Architecture
- Database
- Routes
- Components
- API
- Authentication
- Roles
- Referral logic
- Netlify deployment
- Phases

### Bước 4

Sau khi xác định stack, triển khai MVP theo thứ tự:

```text
Landing Page
→ Registration
→ Referral
→ Admin Approval
→ CTV Dashboard
→ Admin Dashboard
→ Referral Tree
```

### Bước 5

Chạy:

```text
npm install
npm run build
```

và kiểm tra lỗi trước khi deploy Netlify.

---

# 22. Prompt giao cho Codex

Có thể copy nguyên phần dưới đây:

> Bạn là developer chính của dự án Rutop Zalo.
>
> Hãy đọc toàn bộ repository hiện tại trước khi sửa code.
>
> Mục tiêu là xây dựng phần mềm quản lý CTV/member/referral theo tài liệu RUTOP_ZALO_PROJECT.md và ảnh mockup tham chiếu.
>
> Không tự ý thay đổi framework/stack nếu chưa kiểm tra.
>
> Trước tiên:
> 1. Phân tích cấu trúc repo.
> 2. Xác định framework, database, backend/API và cấu hình Netlify.
> 3. Xác định những phần đã có và những phần còn thiếu.
> 4. Tạo IMPLEMENTATION_PLAN.md.
>
> Sau đó triển khai MVP theo thứ tự:
> 1. Landing Page.
> 2. Registration.
> 3. Referral Link/Referral Engine.
> 4. Registration status.
> 5. Admin approval/rejection.
> 6. CTV Dashboard.
> 7. Admin Dashboard.
> 8. Referral Tree.
>
> Yêu cầu:
> - UI bám sát mockup.
> - Responsive.
> - Role: ADMIN / CTV / MEMBER.
> - Referral phải lưu quan hệ referrer → referred user.
> - Không hard-code milestone/voucher.
> - Có validation/loading/error/empty state.
> - Không để secret trong source.
> - Phù hợp Netlify.
> - Không phá vỡ code hiện tại.
> - Sau mỗi phase phải chạy build/test và sửa lỗi.
>
> Nếu backend/database chưa có, đề xuất kiến trúc phù hợp trước khi triển khai.
>
> Không giả định rằng Zalo API có thể cung cấp dữ liệu mà chưa xác minh quyền/API thực tế.
>
> Ưu tiên MVP chạy được trước, sau đó mở rộng.

---

# 23. Trạng thái hiện tại

```text
[✓] Ý tưởng Rutop Zalo
[✓] Mockup tổng thể
[✓] Xác định Role
[✓] Xác định luồng nghiệp vụ
[✓] Xác định module
[✓] Định hướng Database
[✓] Định hướng Referral Engine
[✓] Định hướng Voucher Engine
[✓] Định hướng triển khai Netlify
[ ] Kiểm tra source code hiện tại
[ ] Chốt stack
[ ] Chốt database
[ ] Xây Core
[ ] Xây CTV
[ ] Xây Admin
[ ] Xây Referral Tree
[ ] Tích hợp Zalo
[ ] Test
[ ] Production
```

## Kết luận

Rutop Zalo nên được xây như một **Referral Management Platform**, trong đó Referral Engine là lõi.

Các module khác như CTV Dashboard, Admin Dashboard, Group Zalo, Voucher và báo cáo đều sử dụng dữ liệu từ lõi này.

Kiến trúc mục tiêu:

```text
                    RUTOP ZALO
                        │
              ┌─────────┴─────────┐
              │  REFERRAL ENGINE  │
              └─────────┬─────────┘
                        │
       ┌────────────────┼────────────────┐
       ▼                ▼                ▼
    MEMBER             CTV             ADMIN
       │                │                │
       └────────────────┼────────────────┘
                        ▼
                  REFERRAL TREE
                        │
              ┌─────────┴─────────┐
              ▼                   ▼
          GROUP ZALO          MILESTONE
                                  │
                                  ▼
                               VOUCHER
```

