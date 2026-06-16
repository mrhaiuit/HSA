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
| **Phạm vi** | Toàn bộ nền tảng web, 7 nhóm người dùng, kiến trúc tích hợp .NET 10 + Odoo + ClassIn |

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
| **INTEGRATE (tích hợp, KHÔNG xây lại)** | ClassIn (live class), Odoo Community (ERP/kế toán), SePay (thanh toán), EZSale (CRM giai đoạn đầu), Zalo OA + Email, VNPT/Viettel (e-invoice). |
| **REPLACE (thay thế dần)** | Google Sheet → migrate vào platform; Google Drive rời rạc → file storage (DO Spaces/S3). |

**Triết lý:** Không phát minh lại những gì SaaS đã làm tốt (live class, payment, ERP core). Tập trung nguồn lực kỹ thuật vào **"chất keo" và nghiệp vụ đặc thù** mà không sản phẩm nào trên thị trường phủ được.

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
| **Accounting / ERP** | **INTEGRATE** Odoo Community | Free, mạnh, kết nối JSON-RPC. |
| **Payment gateway** | **INTEGRATE** SePay | Đang dùng, webhook ổn định. |
| **CRM (giai đoạn đầu)** | **INTEGRATE** EZSale | Không gián đoạn đội Sale; migrate sang Odoo CRM sau. |
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
       │  Hangfire QUEUE │    │  PostgreSQL       │   │  Odoo Community    │
       │  background job │    │  INTEGRATION DB   │   │  Finance + CRM     │
       │  retry · cron   │    │  (HSA sở hữu —     │◄─►│  (JSON-RPC)         │
       │  scheduling     │    │  KHÔNG phải Odoo)  │   │                    │
       └─────────────────┘    └───────────────────┘   └────────────────────┘
                                        │
        ┌───────────────────────────────┴────────────────────────────────────┐
        │                  EXTERNAL INTEGRATIONS                              │
        │  SePay (payment) · ClassIn (live class + data) · Zalo OA (ZNS)      │
        │  EZSale (CRM Phase 1) · VNPT/Viettel (e-invoice) · S3/DO Spaces     │
        └─────────────────────────────────────────────────────────────────────┘
```

**Nguyên tắc kiến trúc:**
- **PostgreSQL Integration DB** là nguồn sự thật do **HSA sở hữu** — KHÔNG dùng Odoo DB làm DB chính, tránh khóa cứng vào Odoo.
- **.NET API Gateway** là "hệ thần kinh trung ương" — mọi luồng dữ liệu đi qua đây, không cho phép frontend gọi thẳng external service.
- **Odoo** kết nối qua **JSON-RPC**; ClassIn/SePay/Zalo qua **webhook + REST**; tất cả bọc trong Domain Service tương ứng.
- **Hangfire** xử lý onboarding chain, đối soát batch, tính hoa hồng/thù lao, đẩy ZNS với retry tự động.

---

# 6. QUYẾT ĐỊNH: ERP + LMS

> **HSA không mua ERP-LMS all-in-one.** Quyết định: dùng **Odoo Community** (miễn phí, self-host) làm ERP + giữ **ClassIn** làm lớp trực tuyến + tự xây **HSA Learning Module** (mock exam, ngân hàng câu hỏi, tiến trình) + kết nối tất cả qua **.NET Integration Platform**.

| Lớp | Cấu thành | Chi phí license |
|---|---|---|
| **ERP** | Odoo Community (kế toán, CRM, nhân sự, báo cáo) | **0 đồng** |
| **LMS — live class** | ClassIn (giữ nguyên) | License đang có |
| **LMS — ôn luyện** | HSA Learning Module (tự xây, nằm trong platform) | Chi phí nhân sự CTO |
| **Connector** | HSA Integration Platform (.NET) — kết nối toàn bộ | Chi phí nhân sự CTO |

---

# 7. RESOURCE PLAN — CẦN BAO NHIÊU NGƯỜI ĐỂ XÂY?

## 7.1 Thực tế với 1 CTO thuần backend

- **Có thể xây:** API, integrations, database, business logic, Odoo connector.
- **KHÔNG thể xây một mình:** 7 portal web (UX/UI + frontend code) song song với tất cả integrations.
- **Timeline nếu chỉ 1 người:** 36–48 tháng — **quá dài**, mất cơ hội mở rộng HCM/Đà Nẵng.

## 7.2 Team tối thiểu để hoàn thành trong 24 tháng

| Vị trí | Hình thức | Mức chi phí | Nhiệm vụ chính |
|---|---|---|---|
| CTO / Backend Lead (.NET) | Fulltime nội bộ | **50–100 triệu/tháng** | API, business logic, integrations, database, Odoo |
| Frontend Developer (Next.js) | **Freelancer theo phase** | Theo khối lượng/giai đoạn | Web portals cho tất cả 7 vai trò |
| UI/UX Designer | Freelancer/part-time | Theo project | Design system, wireframes, mockups |
| QA/Tester | Part-time hoặc CTO kiêm | Tùy | Test tự động + manual |

> **Lưu ý Freelancer FE:** Phù hợp cho Phase 0–2 khi deliverable rõ ràng (từng portal). Rủi ro: thay người giữa project mất context. Giảm thiểu bằng cách: spec chi tiết trước khi thuê, handoff code có tài liệu, CTO review mọi PR.

## 7.3 Tổng chi phí nhân sự phát triển

- **Năm 1 (T8/2026 – T7/2027):** CTO (fulltime) + FE Freelancer (theo phase) + UI/UX (project-based) — **thấp hơn đáng kể so với 2 fulltime**, chỉ chi khi có deliverable.
- **Năm 2 (T8/2027 – T7/2028):** CTO (fulltime) + FE Freelancer tiếp tục hoặc chuyển fulltime nếu khối lượng đủ lớn.

## 7.4 So sánh với thuê agency

| Phương án | Chi phí ước tính | Ghi chú |
|---|---|---|
| **Tự xây (CTO nội bộ + FE Freelancer)** | ~thấp hơn fulltime team | Làm chủ sản phẩm, linh hoạt chi theo phase |
| **Thuê agency xây tương đương** | **2.000–5.000 triệu** | Quy mô platform này, chưa kể chi phí maintain |

> **Kết luận:** Tự xây vẫn **rẻ hơn 10–20 lần** so với agency, đồng thời giữ được quyền sở hữu mã nguồn và năng lực kỹ thuật trong nội bộ.

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
- Odoo deep integration: dashboard BGĐ realtime.
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
| **3** | Parent portal + Mock exam engine + Finance module + Odoo realtime + Family CRM (Flow D) | PH tự xem tiến trình; GV tự xem thù lao; e-invoice tự động | T5–9/2027 |
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
| **R7** | **Odoo upgrade/lock-in** — phụ thuộc Odoo cho dữ liệu tài chính | Trung bình | PostgreSQL Integration DB là nguồn sự thật, KHÔNG phải Odoo DB; Odoo chỉ là module kế toán; có thể thay thế mà không mất dữ liệu lõi. |
| **R8** | **Quá tải khi scale ×2 (2026) / ×1.5 (2027)** | Trung bình | Hangfire queue + horizontal scaling; CDN cho static/media; load test trước mỗi mùa tuyển sinh cao điểm. |

---

> **Phê duyệt:** Tài liệu này cần BGĐ phê duyệt **định hướng (Section 1, 4, 6, 7, 8)** và CTO xác nhận **tính khả thi kỹ thuật (Section 5, 9, 10)** trước khi khởi động Phase 0.

*— Hết tài liệu HSA-PV-v1.0 —*
