# HSA EDUCATION — PLATFORM VISION & ARCHITECTURE v1.0
## Tài liệu định hướng sản phẩm & kiến trúc nền tảng thống nhất HSA Platform

---

| Trường | Giá trị |
|---|---|
| **Mã tài liệu** | HSA-PV-v1.0 |
| **Phiên bản** | 1.0 |
| **Ngày** | 2026-06-16 |
| **Loại tài liệu** | Product Vision & Platform Architecture |
| **Người soạn** | COO — Product Owner |
| **Đối tượng đọc** | BGĐ (phê duyệt định hướng) + CTO (căn cứ thiết kế) |
| **Trạng thái** | Draft for Approval |
| **Phạm vi** | Toàn bộ nền tảng web, 7 nhóm người dùng, kiến trúc tích hợp .NET 10 + MISA SME Online + ClassIn |

---

# 1. TÓM TẮT ĐIỀU HÀNH

> **Vision (1 câu):** Xây dựng **HSA Platform** — nền tảng vận hành và học tập thống nhất cho toàn hệ sinh thái HSA Education, phục vụ **7 nhóm người dùng**, thay thế **7 công cụ rời rạc** bằng **một sản phẩm duy nhất do HSA làm chủ**.

## 1.1 Bối cảnh

HSA Education hiện là trung tâm ôn luyện thi hàng đầu quốc gia (**20.000 HS/năm**, dự kiến **~37.000 HS/năm vào 2027** khi HCM ×2 năm 2026 và ×1.5 năm 2027). Với **4 kỳ thi** (ĐGNL HSA – ĐHQG HN, ĐGNL HCM, BCA – Bộ Công An, BQP – Bộ Quốc Phòng), **2 cơ sở** (Hà Nội 50 FT + TP.HCM 12 FT) và **>300 nhân lực** (62 FT, ~70 GV, 132–137 CTV), toàn bộ vận hành đang chạy trên các công cụ rời rạc: **ClassIn, SePay, EZSale CRM, Google Sheet, Zalo OA, Google Drive**.

Hệ quả: dữ liệu phân mảnh, thao tác tay nhiều, không có nguồn sự thật duy nhất, khó scale khi quy mô tăng gần gấp đôi.

## 1.2 Scope sản phẩm

- **Web portal** (Next.js, responsive, PWA-ready) — phục vụ cả 7 vai trò trên một codebase chung.

## 1.3 Ba giá trị cốt lõi

| Giá trị | Ý nghĩa |
|---|---|
| **1. Thống nhất dữ liệu** | Một nguồn sự thật duy nhất (PostgreSQL do HSA sở hữu) — chấm dứt việc đối chiếu chéo giữa 7 hệ thống. |
| **2. Tự động hóa quy trình** | Onboarding, đối soát, hoa hồng, thù lao, chăm sóc vắng học chạy tự động — giảm >80% thao tác tay. |
| **3. Trải nghiệm cá nhân hóa** | Mỗi vai trò có cổng riêng; học sinh nhận lộ trình ôn luyện cá nhân hóa dựa trên kết quả mock exam. |

## 1.4 BUILD / INTEGRATE / REPLACE — quyết định chiến lược

| Hành động | Đối tượng |
|---|---|
| **BUILD (tự xây)** | 7 portal web, mock exam engine, learning module, business logic nghiệp vụ HSA (SBD, onboarding chain, CTV attribution). |
| **INTEGRATE (tích hợp, KHÔNG xây lại)** | ClassIn (live class), MISA SME Online (kế toán chính thức), SePay (thanh toán), EZSale (CRM giai đoạn đầu), Zalo OA + Email, VNPT/Viettel (e-invoice). |
| **REPLACE (thay thế dần)** | Google Sheet → migrate vào platform; Google Drive rời rạc → file storage (DO Spaces/S3). |

**Triết lý:** Không phát minh lại những gì SaaS đã làm tốt (live class, payment, kế toán chuẩn pháp lý VN). Tập trung nguồn lực kỹ thuật vào **"chất keo" và nghiệp vụ đặc thù** (CRM, hoa hồng, thù lao, đối soát, onboarding) mà không sản phẩm nào trên thị trường phủ được.

---

# 2. BẢN ĐỒ ACTOR — 7 NHÓM NGƯỜI DÙNG

## A. BGĐ / Admin (Ban lãnh đạo)

- **Dashboard thời gian thực:** doanh thu hôm nay, học sinh mới, tỉ lệ chốt, tỉ lệ điều chỉnh.
- **KPI tracking:** theo tháng/quý/năm, theo cơ sở (HN vs HCM), theo kỳ thi (HSA/HCM/BCA/BQP).
- **Báo cáo tài chính:** P&L, dòng tiền, công nợ (phải thu – học phí; phải trả – lương GV + hoa hồng CTV).
- **Alert quan trọng:** lỗ hổng vận hành, học sinh vắng nhiều, CTV top performer.
- **Phân quyền theo scope:** xem toàn hệ thống hoặc giới hạn từng cơ sở.

## B. Sale (Nhân viên kinh doanh)

- **Pipeline CRM:** lead mới → đang tư vấn → sắp chốt → đã chốt.
- **Lead inbox:** tự động nhận lead từ Facebook Ads, Landing page, CTV referral.
- **Hồ sơ học sinh:** lịch sử liên hệ, ghi chú, lịch hẹn.
- **Quota & performance:** chỉ tiêu tháng, đã đạt bao nhiêu %.
- **Click-to-call** / tích hợp gọi điện.
- **Chia sẻ link giỏ hàng** cho học sinh.
- **Hoa hồng sale preview** (nếu có cơ chế).

## C. Cộng tác viên (CTV / Đại lý)

- **Link referral cá nhân** (unique per CTV).
- **Dashboard:** số lượt click, số HS đã đăng ký, tỉ lệ chuyển đổi.
- **Hoa hồng realtime:** đang tích lũy → đã xác nhận → đã thanh toán.
- **Lịch sử thanh toán hoa hồng.**
- **Tài liệu marketing:** banner, content mẫu, video giới thiệu để share.
- **Leaderboard** (top CTV tháng này).
- **Đăng ký học sinh thay mặt** (khi phụ huynh cần hỗ trợ).

## D. Học sinh (Student)

- **Trạng thái đăng ký:** SBD, gói học, trạng thái thanh toán.
- **Lịch học:** theo tuần, tích hợp reminder (Zalo + push notification).
- **Tham gia lớp:** nút "Vào lớp" (deeplink ClassIn hoặc join URL).
- **Điểm danh & tiến trình:** bao nhiêu buổi đã học / còn lại.
- **Ôn luyện / đề thi thử:**
  - Ngân hàng câu hỏi phân loại theo môn / độ khó.
  - Đề thi thử có đếm giờ (timed mock exam).
  - Kết quả: phân tích đúng/sai, điểm yếu theo chủ đề.
  - Lịch sử bài làm và tiến bộ theo thời gian.
- **Tài liệu học:** slide bài giảng, video ghi lại (nếu có), tài liệu PDF.
- **Thanh toán:** lịch sử giao dịch, tải hóa đơn.
- **Thông báo:** tin quan trọng từ trung tâm, nhắc lịch thi thật, kết quả thi.
- **Hỗ trợ:** chat với QLL / tư vấn học vụ.

## E. Phụ huynh (Parent)

- **Dashboard con:** học sinh nào, gói học nào, SBD.
- **Điểm danh:** buổi nào vắng/có mặt → cảnh báo vắng nhiều.
- **Tiến trình học:** điểm bài thi thử, so sánh với mục tiêu.
- **Lịch sử thanh toán** và hóa đơn.
- **Thông báo:** cảnh báo vắng học, thông tin khai giảng đợt mới.
- **Đăng ký cho con em khác** (family account).
- **Liên hệ** QLL / giáo viên.
- **Referral phụ huynh:** giới thiệu → nhận ưu đãi.

## F. Giáo viên / Giảng viên (Teacher)

- **Lịch dạy:** các lớp phụ trách, theo tuần.
- **Danh sách học sinh** mỗi lớp.
- **Điểm danh đầu vào** mỗi buổi (hoặc đồng bộ từ ClassIn).
- **Upload tài liệu** bài giảng cho từng buổi.
- **Bài tập / kiểm tra:** giao bài, xem kết quả nộp.
- **Thù lao:** số buổi đã dạy, đơn giá, tổng thù lao tháng (preview trước khi kế toán duyệt).
- **Thông báo** từ ban quản lý.

## G. Quản lý nội bộ / Admin (QLL, Kế toán, HR)

- **Quản lý học sinh:** tìm kiếm, xem hồ sơ, sửa thông tin.
- **Quản lý lớp học:** tạo lớp, phân giáo viên, xếp lịch.
- **Quản lý đăng ký:** danh sách HS theo lớp / kỳ thi.
- **Hàng chờ ngoại lệ:** các trường hợp onboarding lỗi cần xử lý tay.
- **Thanh toán:** đối soát SePay tự động, danh sách ngoại lệ (lệch nội dung CK, hoàn tiền).
- **Kế toán thu:** xem tất cả giao dịch, xuất báo cáo.
- **Kế toán chi:** phê duyệt thù lao GV, phê duyệt hoa hồng CTV.
- **Mã khuyến mãi:** tạo, quản lý, giới hạn số lần dùng, thống kê.
- **Hóa đơn điện tử:** tạo và gửi e-invoice (tích hợp VNPT/Viettel).
- **Phân quyền người dùng nội bộ.**

---

# 3. MA TRẬN TÍNH NĂNG (FEATURE MATRIX)

Ký hiệu: ✅ có quyền/sử dụng · ❌ không · 🔜 giai đoạn sau.

| Tính năng | BGĐ | Sale | CTV | HS | PH | GV | QLL |
|---|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| Dashboard điều hành (doanh thu, KPI) | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | 🔜 |
| Báo cáo tài chính P&L / dòng tiền | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |
| Pipeline CRM / Lead inbox | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ |
| Hồ sơ học sinh (xem/sửa) | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ |
| Quota & performance cá nhân | ❌ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Click-to-call / chia sẻ giỏ hàng | ❌ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Link referral cá nhân | ❌ | 🔜 | ✅ | ❌ | 🔜 | ❌ | ❌ |
| Hoa hồng realtime / lịch sử | ✅ | 🔜 | ✅ | ❌ | ❌ | ❌ | ✅ |
| Leaderboard CTV | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ | ✅ |
| Tài liệu marketing để share | ❌ | ✅ | ✅ | ❌ | ❌ | ❌ | ✅ |
| Trạng thái đăng ký / SBD | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |
| Lịch học + reminder | ❌ | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ |
| Nút "Vào lớp" (ClassIn) | ❌ | ❌ | ❌ | ✅ | ❌ | ✅ | ❌ |
| Điểm danh & tiến trình | ✅ | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ |
| Mock exam engine (làm đề) | ❌ | ❌ | ❌ | ✅ | ❌ | 🔜 | ✅ |
| Phân tích kết quả / điểm yếu | ✅ | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ |
| Tài liệu học (slide/video/PDF) | ❌ | ❌ | ❌ | ✅ | 🔜 | ✅ | ✅ |
| Thanh toán & hóa đơn cá nhân | ❌ | ❌ | ❌ | ✅ | ✅ | ❌ | ✅ |
| Cảnh báo vắng học | ✅ | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ |
| Family account / đăng ký anh em | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ✅ |
| Lịch dạy / DS học sinh lớp | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ |
| Upload tài liệu / giao bài tập | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ |
| Thù lao GV preview | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ |
| Quản lý lớp / xếp lịch / phân GV | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |
| Hàng chờ ngoại lệ onboarding | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |
| Đối soát SePay tự động | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |
| Phê duyệt thù lao / hoa hồng | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |
| Mã khuyến mãi (tạo/thống kê) | ✅ | 🔜 | ❌ | ❌ | ❌ | ❌ | ✅ |
| E-invoice (VNPT/Viettel) | ❌ | ❌ | ❌ | 🔜 | 🔜 | ❌ | ✅ |
| Phân quyền người dùng | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |
| Chat / hỗ trợ học vụ | 🔜 | ✅ | 🔜 | ✅ | ✅ | ✅ | ✅ |

---

# 4. BUILD vs INTEGRATE vs REPLACE

| Thành phần | Quyết định | Lý do |
|---|---|---|
| **Actor portals (web)** | **BUILD** (Next.js) | Đặc thù nghiệp vụ HSA, không SaaS nào phủ đủ 7 vai trò. |
| **Mock exam engine** | **BUILD** | Cần kiểm soát câu hỏi, kết quả, chống gian lận, phân tích điểm yếu. |
| **Online classroom (live)** | **INTEGRATE** ClassIn | Đã có hợp đồng, HS quen; lấy data về qua Data Subscription. |
| **Kế toán chính thức** | **INTEGRATE** MISA SME Online | Chuẩn kế toán VN, kế toán HSA đã quen, có REST API; nhận sync 1 chiều từ .NET. |
| **Nghiệp vụ tài chính đặc thù** | **BUILD** .NET Finance Service | Hoa hồng CTV, thù lao GV, đối soát SePay, mã khuyến mãi — push journal entries lên MISA. |
| **Payment gateway** | **INTEGRATE** SePay | Đang dùng, webhook ổn định. |
| **CRM (giai đoạn đầu)** | **INTEGRATE** EZSale | Không gián đoạn đội Sale; migrate sang .NET CRM module sau. |
| **Communication** | **INTEGRATE** Zalo OA + Email | Không tự xây messaging. |
| **E-invoice** | **INTEGRATE** VNPT/Viettel | Bắt buộc pháp lý, API có sẵn. |
| **Google Sheet** | **REPLACE** dần | Migrate vào platform sau khi ổn định. |
| **Google Drive rời rạc** | **REPLACE** bằng file storage | Upload vào platform, lưu DO Spaces hoặc S3. |

---

# 5. KIẾN TRÚC KỸ THUẬT TỔNG QUAN

```
┌────────────────────────────────────────────────────────────────────────────┐
│                          NGƯỜI DÙNG — 7 VAI TRÒ                              │
│   BGĐ · Sale · CTV · Học sinh · Phụ huynh · Giáo viên · QLL/Kế toán/HR       │
└───────────────────────────────┬────────────────────────────────────────────┘
                                │ HTTPS / JWT
                │ HTTPS / JWT
┌───────────────▼─────────────────────────────────────────────────────────┐
│  FRONTEND — WEB                                                           │
│  Next.js (responsive, PWA-ready) · 1 codebase · 7 layout/role            │
│  SSR + RBAC-aware routing                                                 │
└───────────────┬─────────────────────────────────────────────────────────┘
                │ REST / JSON
        ┌──────────────────────────▼─────────────────────────────────────┐
        │           BACKEND — .NET 10 API GATEWAY (Clean Architecture)     │
        │  ┌───────────────────────────────────────────────────────────┐  │
        │  │  Auth Service     — JWT, RBAC (7 vai trò, phân quyền chi tiết) │
        │  │  Enrollment Svc   — SBD, ClassIn enroll, Zalo onboarding   │  │
        │  │  Learning Service — classes, attendance, content, mock exam│  │
        │  │  Finance Service  — payments, invoices, commissions, payroll│ │
        │  │  CRM Service      — leads, pipeline, CTV attribution       │  │
        │  │  Notification Svc — Zalo, Email, Push                      │  │
        │  │  Report Service   — aggregation, dashboard data           │  │
        │  └───────────────────────────────────────────────────────────┘  │
        └───────┬───────────────────────┬───────────────────────┬─────────┘
                │                       │                       │
       ┌────────▼────────┐    ┌─────────▼─────────┐   ┌─────────▼──────────┐
       │  Hangfire QUEUE │    │  PostgreSQL       │   │  MISA SME Online   │
       │  background job │    │  HSA PLATFORM DB  │   │  Kế toán chính thức│
       │  retry · cron   │    │  (HSA sở hữu —     │──►│  MISA API (sync    │
       │  scheduling     │    │  SSOT duy nhất)   │   │  1 chiều .NET→MISA)│
       └─────────────────┘    └───────────────────┘   └────────────────────┘
                                        │
        ┌───────────────────────────────┴────────────────────────────────────┐
        │                  EXTERNAL INTEGRATIONS                              │
        │  SePay (payment) · ClassIn (live class + data) · Zalo OA (ZNS)      │
        │  EZSale (CRM Phase 1) · VNPT/Viettel (e-invoice) · S3/DO Spaces     │
        └─────────────────────────────────────────────────────────────────────┘
```

**Nguyên tắc kiến trúc:**
- **PostgreSQL HSA Platform DB** là nguồn sự thật duy nhất (SSOT) do **HSA sở hữu** — chỉ có 1 database; **MISA SME** nhận dữ liệu kế toán qua sync định kỳ từ **.NET Finance Service**.
- **.NET API Gateway** là "hệ thần kinh trung ương" — mọi luồng dữ liệu đi qua đây, không cho phép frontend gọi thẳng external service.
- **MISA SME Online** nhận sync **1 chiều (.NET → MISA)** qua **MISA API** (push journal entries định kỳ); ClassIn/SePay/Zalo qua **webhook + REST**; tất cả bọc trong Domain Service tương ứng.
- **Hangfire** xử lý onboarding chain, đối soát batch, tính hoa hồng/thù lao, đẩy ZNS với retry tự động.

---

---

# 7. RESOURCE PLAN — CẦN BAO NHIÊU NGƯỜI ĐỂ XÂY?

## 7.1 Lưu ý quan trọng: CTO cần tuyển mới

> HSA hiện **chưa có CTO**. Toàn bộ lộ trình phụ thuộc vào việc tuyển được CTO phù hợp. Đây là **điều kiện tiên quyết** — không tuyển được CTO thì Phase 0 không thể khởi động. COO đóng vai Product Owner, KHÔNG đóng vai kỹ thuật.

## 7.2 Cấu hình team đề xuất

| Vị trí | Hình thức | Mức lương/chi phí | Nhiệm vụ chính |
|---|---|---|---|
| **CTO** (SA + Backend .NET) | Fulltime nội bộ — **tuyển mới** | **50–100 triệu/tháng** | Kiến trúc hệ thống, API, business logic, integrations, DB, MISA sync, dẫn dắt team |
| **Fullstack Developer** (.NET + Next.js) | Fulltime nội bộ | **30–40 triệu/tháng** | Backend support + toàn bộ web portal (7 vai trò) |
| **Fresher Developer** | Fulltime nội bộ | **15 triệu/tháng** | Task nhỏ, test, bug fix, hỗ trợ senior |
| **UI/UX Designer** | Freelancer theo project | Theo deliverable | Design system, wireframes, mockups từng Phase |
| **QC / Tester** | Freelancer theo sprint | Theo deliverable | Test manual + viết test case |

## 7.3 Chi phí nhân sự phát triển (tháng/năm)

| | Chi phí/tháng | Chi phí/năm |
|---|---|---|
| CTO | 50–100 triệu | 600–1.200 triệu |
| Fullstack Dev | 30–40 triệu | 360–480 triệu |
| Fresher | 15 triệu | 180 triệu |
| UI/UX + QC (freelance) | ~10–20 triệu | ~120–240 triệu |
| **Tổng nhân sự/năm** | **105–175 triệu/tháng** | **~1.260–2.100 triệu/năm** |

> **Lưu ý:** Chi phí nhân sự tech cao hơn đáng kể so với các ước tính ban đầu. Đây là chi phí thực tế để có đội ngũ đủ năng lực. Cần đưa con số này vào ROI calculation đầy đủ.

## 7.4 So sánh với thuê agency

| Phương án | Chi phí xây dựng | Chi phí vận hành/năm | Ghi chú |
|---|---|---|---|
| **Tự xây (team nội bộ)** | ~1.260–2.100 triệu/năm nhân sự | ~200–300 triệu/năm (duy trì) | Làm chủ code, kiến thức ở lại |
| **Thuê agency** | **2.000–5.000 triệu** (một lần) | ~300–600 triệu/năm (maintain) | Phụ thuộc vendor, black box |

> **Kết luận:** Tự xây vẫn có lợi thế về **kiểm soát, sở hữu và linh hoạt** — nhưng chi phí nhân sự cần được hoạch định đầy đủ, không thể tính thấp.

---

# 8. LỘ TRÌNH PHÂN KỲ 24 THÁNG

## Phase 0 — Security & Foundation (T8–9/2026, 2 tháng)
- Tuyển CTO + FE Dev.
- Fix bảo mật website (B0–B7).
- Dựng hạ tầng: server, domain, CI/CD pipeline.
- Design system (UI component library cơ bản).

## Phase 1 — Core Automation + Admin Portal (T10–12/2026, 3 tháng)
- Onboarding tự động (SePay → SBD → ClassIn → Zalo) — **Flow A**.
- Admin portal: quản lý học sinh, hàng chờ ngoại lệ, đối soát thanh toán cơ bản.
- Student portal (web): SBD, lịch học, trạng thái gói — đơn giản.
- **KPI:** onboarding < 2 phút, giảm 80% việc tay.

## Phase 2 — Teacher + CTV + Sale Portal (T1–4/2027, 4 tháng)
- Teacher portal: lịch dạy, điểm danh, upload tài liệu, xem thù lao.
- CTV portal: link ref, hoa hồng realtime, leaderboard.
- Sale portal: pipeline CRM đơn giản, lead inbox, chia sẻ giỏ hàng.
- Hoa hồng CTV tự động (**Flow C**).
- Chăm sóc chủ động vắng học (**Flow B** — ClassIn Data Subscription).
- **KPI:** CTV tự xem hoa hồng, tranh chấp → 0.

## Phase 3 — Parent Portal + Mock Exam + Finance (T5–9/2027, 5 tháng)
- Parent portal: xem con, điểm danh, alert, đăng ký anh em.
- Mock exam engine: ngân hàng câu hỏi, đề thi thử timed, phân tích kết quả.
- Finance module: kế toán thu/chi đầy đủ, thù lao GV tự động, e-invoice.
- MISA sync + .NET Finance module hoàn chỉnh: push journal entries lên MISA SME, dashboard BGĐ realtime từ PostgreSQL HSA.
- Family CRM (**Flow D**): nhận diện gia đình, nurture sequence.
- **KPI:** phụ huynh tự xem tiến trình, GV tự xem thù lao.

## Phase 4 — BGĐ Dashboard + Đà Nẵng (T10/2027–T3/2028, 6 tháng)
- BGĐ dashboard nâng cao: P&L realtime, marketing ROI, cohort analysis.
- Cloudflare CDN migration.
- Google Workspace email thay Gmail cá nhân.
- Chuẩn hóa quy trình → sẵn sàng mở Đà Nẵng "plug and play".
- **KPI:** Đà Nẵng go-live, BGĐ có đủ số liệu ra quyết định real-time.

## Phase 5 — AI & Personalization (T4–9/2028, 6 tháng)
- Gợi ý học tập cá nhân hóa dựa trên kết quả mock exam.
- Predictive analytics: học sinh nào có nguy cơ drop-out.
- Marketing automation: nurture sequence tự động theo behavior.
- Advanced reporting: cohort retention, LTV per kỳ thi/kênh.

---

# 9. MILESTONE & KPI THEO GIAI ĐOẠN

| Phase | Milestone deliverable | KPI đo được | Timeline |
|---|---|---|---|
| **0** | CTO+FE onboard, hạ tầng + CI/CD, design system, fix bảo mật B0–B7 | 100% lỗ hổng B0–B7 đóng; pipeline build xanh | T8–9/2026 |
| **1** | Onboarding chain (Flow A) + Admin portal + Student portal cơ bản | Onboarding < 2 phút (P95); giảm ≥80% thao tác tay | T10–12/2026 |
| **2** | Teacher + CTV + Sale portal; hoa hồng tự động (Flow C); chăm sóc vắng (Flow B) | Tranh chấp hoa hồng = 0; CTV self-service 100% | T1–4/2027 |
| **3** | Parent portal + Mock exam engine + Finance module + MISA sync realtime + Family CRM (Flow D) | PH tự xem tiến trình; GV tự xem thù lao; e-invoice tự động | T5–9/2027 |
| **4** | BGĐ dashboard nâng cao + Đà Nẵng go-live | Đà Nẵng vận hành plug-and-play; BGĐ dashboard real-time đầy đủ | T10/2027–T3/2028 |
| **5** | AI personalization + predictive drop-out + marketing automation | Gợi ý ôn luyện cá nhân hóa; cảnh báo drop-out có độ chính xác đo được | T4–9/2028 |

---

# 10. RỦI RO VÀ CÁCH GIẢM THIỂU (PLATFORM-LEVEL)

| # | Rủi ro | Mức độ | Cách giảm thiểu |
|---|---|:--:|---|
| **R1** | **Over-engineering** — xây quá nhiều tính năng trước khi validate | Cao | Phân kỳ chặt theo phase; mỗi phase chỉ release tính năng đã có actor xác nhận cần; bám Feature Matrix, không nhảy cóc 🔜 lên ✅. |
| **R2** | **Team dependency** — CTO nghỉ khi platform đang dở | Cao | Code Clean Architecture + tài liệu hóa; commit thường xuyên; FE Dev nắm domain; tránh "kiến thức ngầm" trong đầu 1 người. |
| **R3** | **User adoption** — 7 nhóm người dùng = 7 thách thức change management | Cao | Roll-out theo phase từng nhóm; đào tạo + tài liệu hướng dẫn; chạy song song công cụ cũ trong giai đoạn chuyển đổi; thu feedback sớm. |
| **R4** | **Data migration** từ Google Sheet, EZSale, Drive → lỗi dữ liệu | Trung bình–Cao | Migrate theo lô có đối soát; giữ bản gốc read-only; script idempotent + dry-run; validate trước khi cutover. |
| **R5** | **Scope creep** — BGĐ liên tục thêm yêu cầu | Cao | COO làm Product Owner gác cổng backlog; mọi yêu cầu mới vào backlog, ưu tiên theo phase; "freeze scope" trong mỗi sprint. |
| **R6** | **ClassIn API thay đổi** ảnh hưởng Learning module | Trung bình | Bọc ClassIn sau Learning Service (anti-corruption layer); không để frontend phụ thuộc trực tiếp; theo dõi changelog ClassIn; có fallback điểm danh tay. |
| **R7** | **MISA lock-in** — phụ thuộc MISA cho dữ liệu kế toán | Trung bình–Thấp | PostgreSQL HSA Platform là nguồn sự thật duy nhất, KHÔNG phải MISA DB; MISA chỉ nhận sync 1 chiều dữ liệu kế toán; có thể thay phần mềm kế toán mà không mất dữ liệu lõi. |
| **R8** | **Quá tải khi scale ×2 (2026) / ×1.5 (2027)** | Trung bình | Hangfire queue + horizontal scaling; CDN cho static/media; load test trước mỗi mùa tuyển sinh cao điểm. |

---

# 11. AI & BIGDATA — CƠ HỘI VÀ LỘ TRÌNH

HSA Education không chỉ vận hành một trung tâm ôn luyện — với quy mô 20.000 học sinh/năm (2025) tăng lên ~37.000 vào 2027, qua 4 kỳ thi (ĐGNL HSA, ĐGNL HCM, BCA, BQP) và 2 cơ sở Hà Nội + TP.HCM, HSA đang ngồi trên một mỏ dữ liệu hành vi học tập và tuyển sinh thuộc loại lớn nhất thị trường EdTech ôn luyện Việt Nam. HSA Platform chính là công cụ biến mỏ dữ liệu đó thành lợi thế cạnh tranh bền vững thông qua AI và BigData.

Phần này trả lời 4 yêu cầu của BGĐ từ cuộc họp: (1) tự động hóa review Sale & CTV bằng AI, (2) chăm sóc khách hàng tự động bằng AI, (3) BigData phân tích hành vi học sinh, (4) phân tích sâu cơ hội AI+BigData trong business model HSA.

## 11.1 Dữ liệu HSA sở hữu — tài sản chiến lược

Khi HSA Platform đi vào hoạt động, mỗi tương tác của học sinh, phụ huynh, Sale và CTV đều để lại dấu vết số. Đây là các nhóm dữ liệu HSA sẽ thu thập một cách có hệ thống (thay vì rải rác trên Google Sheet, EZSale, Drive như hiện tại):

- **Behavioral data (hành vi sử dụng):** thời điểm học sinh login (sáng/trưa/tối, ngày thường/cuối tuần), tổng thời gian ôn luyện mỗi phiên, số câu hỏi làm/ngày, tốc độ làm bài (giây/câu), pattern đúng/sai theo dạng câu, tỉ lệ bỏ dở giữa chừng một đề.
- **Learning data (kết quả học tập):** điểm mock exam tách theo chủ đề/môn con (Toán, Văn, Khoa học, Tiếng Anh…), tiến bộ điểm số theo tuần, điểm yếu cố hữu theo dạng bài, độ ổn định điểm qua nhiều lần thi thử.
- **Conversion data (chuyển đổi tuyển sinh):** nguồn lead (Zalo, Facebook, CTV, referral, walk-in), thời gian từ lead → chốt (lead time), số lần tương tác với Sale trước khi chốt, kênh nào có chi phí/chuyển đổi tốt nhất, tỉ lệ rơi ở từng bước phễu.
- **CTV data (cộng tác viên):** conversion rate của từng CTV trong 132–137 CTV, loại nội dung CTV share hiệu quả nhất, thời điểm CTV active, referral chain (CTV nào giới thiệu CTV nào), độ bền hoạt động.
- **Payment data (thanh toán — qua SePay/.NET Finance Service):** gói học bán chạy nhất theo mùa, tỉ lệ hoàn tiền theo gói, pattern thanh toán trả góp vs trả thẳng, độ trễ thanh toán, gói nào hay bị huỷ.
- **Engagement data (tương tác Zalo OA):** tỉ lệ mở tin nhắn, tỉ lệ click link, tỉ lệ phản hồi; buổi học nào học sinh vắng nhiều nhất (qua điểm danh ClassIn), khung giờ phụ huynh phản hồi nhanh nhất.
- **Family data (quan hệ gia đình):** gia đình nào có nhiều con em cùng học HSA, tỉ lệ siblings convert (anh/chị học rồi giới thiệu em), giá trị vòng đời theo hộ gia đình.

> **Định hướng:** Khi Platform đủ trưởng thành (12–18 tháng vận hành thực tế), HSA sẽ có một **data warehouse** tập trung trên PostgreSQL với khối lượng và độ sạch đủ để **train model riêng** (cho bài toán dự đoán) hoặc **fine-tune LLM** (cho chăm sóc và tư vấn theo ngữ cảnh HSA). Dữ liệu là tài sản tích lũy — bắt đầu thu thập sạch ngay từ Phase 1 sẽ quyết định năng lực AI ở Phase 4–5.

## 11.2 AI cho Sale & CTV — tự động hóa review và coaching

### A. AI Review Sale

- **Call analysis (phân tích cuộc gọi):** tích hợp ghi âm cuộc gọi tư vấn → AI transcribe và phân tích: tỉ lệ % thời gian Sale nói vs nghe (talk-to-listen ratio), các từ khóa phủ nhận/do dự của học sinh ("đắt", "để suy nghĩ", "hỏi bố mẹ đã"), Sale có chủ động hỏi về nhu cầu/kỳ thi mục tiêu không, có tạo cảm giác deadline (sắp hết ưu đãi, sắp đầy lớp) không → tự động cho điểm theo **rubric chuẩn** → gửi báo cáo tuần cho Sale Manager kèm trích đoạn cần cải thiện.
- **Pipeline intelligence:** AI phát hiện deal sắp "chết lạnh" (không có activity 3+ ngày), tự động nhắc Sale và **gợi ý next best action** dựa trên các deal tương tự đã thắng trong lịch sử (ví dụ: "lead nguồn CTV, quan tâm BCA, đã làm mock — nên gửi điểm chuẩn năm trước + mời thi thử miễn phí").
- **Conversion predictor:** dự đoán xác suất chốt của từng lead dựa trên nguồn lead, thời gian phản hồi đầu tiên, kỳ thi quan tâm, điểm mock (nếu đã làm), số lần tương tác → Sale ưu tiên thời gian cho lead xác suất cao, không lãng phí vào lead "đông lạnh".

### B. AI Review CTV

- Phân tích nội dung CTV share để xác định **loại nào dẫn đến conversion cao hơn** (video vs text vs hình ảnh, kỳ thi nào, thông điệp gì) → đúc kết "content playbook" và phân phối lại cho toàn đội CTV.
- Tự động **phân tier CTV theo performance thực tế** (conversion rate, doanh thu mang về) thay vì chỉ đếm số lượng referral → gợi ý uplift plan riêng cho từng tier (đào tạo, thưởng, cấp tài nguyên).
- Phát hiện **CTV inactive sắp churn** (giảm hoạt động, dừng share) → tự động trigger re-engagement campaign (nhắc thưởng, gửi content mới, mời sự kiện).

### C. AI Chăm sóc khách hàng tự động

- **AI Chatbot học vụ (Zalo OA):** trả lời ~80% câu hỏi thường gặp (lịch thi, số báo danh, link ClassIn, lịch học, học phí) bằng ngôn ngữ tự nhiên, chỉ escalate lên Quản lý lớp (QLL) khi câu hỏi phức tạp/nhạy cảm — train trên FAQ HSA + lịch sử ticket. Đây là bước nhảy so với rule-based template hiện tại.
- **Proactive care (chăm sóc chủ động):** AI phát hiện học sinh có nguy cơ drop-out (vắng 2+ buổi liên tiếp theo điểm danh ClassIn, điểm mock đi xuống, không login platform 5+ ngày) → tự động gửi Zalo cá nhân hóa động viên + alert QLL/Sale để can thiệp kịp thời trước khi học sinh bỏ hẳn.
- **Parent engagement (gắn kết phụ huynh):** AI tự tóm tắt tuần học của con và gửi Zalo cho phụ huynh mỗi cuối tuần (không cần QLL soạn tay) — ví dụ: *"Tuần này con học 4/5 buổi, điểm mock trung bình 78/100 (tăng 6 điểm), môn yếu nhất là Khoa học tự nhiên, gợi ý con ôn thêm chuyên đề Vật lý sóng cuối tuần này."*

## 11.3 BigData phân tích hành vi học sinh

Với quy mô hàng chục nghìn học sinh, BigData cho phép HSA nhìn ra những quy luật không thể thấy bằng mắt thường:

1. **Cohort retention analysis:** học sinh từ kênh nào (Zalo, Facebook, CTV, referral) giữ chân tốt nhất, không bỏ giữa chừng? Kỳ thi nào (HSA/HCM/BCA/BQP) có drop-out cao nhất? → điều chỉnh ngân sách tuyển sinh và thiết kế chương trình giữ chân.
2. **Learning pattern clustering:** phân nhóm học sinh theo hành vi học (sáng sớm vs tối muộn; đều đặn vs dồn sát ngày thi; tự học cao vs phụ thuộc lớp trực tiếp) → thiết kế gói học và lịch nhắc phù hợp từng nhóm.
3. **Exam readiness prediction:** dựa trên kết quả mock + tỉ lệ điểm danh + tổng thời gian ôn → dự đoán xác suất pass kỳ thi thật → alert sớm cho học sinh và giảng viên những trường hợp "đang trượt khỏi đường ray".
4. **Content effectiveness:** bài giảng nào / giảng viên nào khiến học sinh làm mock tốt nhất sau đó? → tối ưu phân công 70 giảng viên và nội dung theo dữ liệu thay vì cảm tính.
5. **Seasonal demand forecasting:** dự đoán số lượng đăng ký theo tháng (dựa trên 3–5 năm lịch sử + lịch thi ĐGNL chính thức) → chủ động mở lớp, tuyển thêm CTV, tăng capacity hạ tầng trước mùa cao điểm thay vì chữa cháy.
6. **Price elasticity:** gói học nào, mức giá nào, thời điểm khuyến mãi nào cho conversion cao nhất → tối ưu pricing strategy và lịch chạy ưu đãi theo dữ liệu thực.

## 11.4 Lộ trình AI/BigData theo Phase

AI không phải thứ "bật công tắc là có" — nó phụ thuộc vào lượng dữ liệu tích lũy. Lộ trình dưới đây đồng bộ với các Phase phát triển Platform và gắn yêu cầu dữ liệu tối thiểu cho từng deliverable:

| Phase | Thời gian | AI/BigData deliverable | Yêu cầu dữ liệu tối thiểu |
|---|---|---|---|
| **Phase 1–2** | T10/2026–T4/2027 | Thu thập dữ liệu sạch (không thiếu, không trùng) vào PostgreSQL; báo cáo KPI cơ bản cho BGĐ | 3 tháng data |
| **Phase 3** | T5–9/2027 | Dashboard BigData: cohort analysis, conversion funnel, mock exam analytics; Zalo chatbot FAQ cơ bản (rule-based + GPT API) | 6–9 tháng data |
| **Phase 4** | T10/2027–T3/2028 | Proactive care AI (drop-out predictor), pipeline intelligence cho Sale, CTV performance analytics, parent weekly summary tự động | 12 tháng data |
| **Phase 5** | T4–9/2028 | Call analysis AI, conversion predictor, exam readiness score, learning path personalization, seasonal forecasting | 18–24 tháng data |

## 11.5 Công nghệ AI — Build vs API

Quyết định công nghệ phải tối ưu chi phí và tốc độ triển khai — không phải mọi bài toán đều cần LLM, và không bài toán nào cần HSA tự xây model nền tảng:

| Ứng dụng | Phương án | Lý do |
|---|---|---|
| Chatbot FAQ học vụ | **OpenAI API / Claude API** (gọi từ .NET 10) | Không cần train; context HSA đưa vào system prompt; triển khai nhanh ngay Phase 3 |
| Proactive care / drop-out predictor | **Scikit-learn / XGBoost** (Python microservice) | Dữ liệu tabular (điểm danh, mock score, login frequency) → classical ML đủ, không cần LLM |
| Phân tích cohort / funnel | **SQL + dbt + Metabase/Superset** | Không cần AI — BigData analytics thuần, đủ mạnh và rẻ |
| Call analysis (Sale) | **OpenAI Whisper + GPT-4** | Transcription + phân tích hội thoại; gọi API theo batch sau mỗi ngày |
| Conversion predictor | **Logistic regression → Gradient Boosting** | Train trên lịch sử CRM; retrain mỗi tháng tự động qua Hangfire |
| Learning path personalization | **Collaborative filtering (Phase 5)** | Cần 12+ tháng data đủ lớn để collaborative filtering có ý nghĩa |

> **Nguyên tắc cốt lõi:** KHÔNG tự xây model LLM. Gọi OpenAI/Anthropic API cho các tác vụ NLP (chatbot, call analysis, tóm tắt); tự xây model nhỏ (scikit-learn/XGBoost) cho các tác vụ dự đoán trên dữ liệu tabular. Đầu tư vào **data pipeline sạch** trước, AI sau — không có dữ liệu sạch thì mọi model đều vô nghĩa (rác vào, rác ra).

---

> **Phê duyệt:** Tài liệu này cần BGĐ phê duyệt **định hướng (Section 1, 4, 6, 7, 8, 11)** và CTO xác nhận **tính khả thi kỹ thuật (Section 5, 9, 10)** trước khi khởi động Phase 0.

*— Hết tài liệu HSA-PV-v1.0 —*
