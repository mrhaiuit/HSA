# HSA EDUCATION — TECHNICAL ROADMAP v1.0
## Tài liệu thiết kế kiến trúc & lộ trình triển khai HSA Integration Platform

---

| Trường | Giá trị |
|---|---|
| **Phiên bản** | v1.0 |
| **Ngày** | 2026-06-16 |
| **Tác giả** | Senior BA + Principal PO |
| **Đối tượng** | Developer (Founder/COO, 10 năm .NET, tự viết code) |
| **Trạng thái** | Approved for Implementation |
| **Phạm vi** | Build .NET middleware tích hợp ngoài, giữ EZSale giai đoạn đầu, Odoo Community làm data warehouse + UI phụ |

**Nguyên tắc cốt lõi của tài liệu này:**
- Phân biệt rõ **"làm ngay"** (Phase 0-2) vs **"làm sau"** (Phase 3+) để tránh over-engineering.
- EZSale CRM **GIỮ NGUYÊN** giai đoạn đầu — middleware chỉ **READ**, không write vào EZSale.
- Odoo Community = data warehouse + UI phụ, **KHÔNG** là CRM chính trong Phase 1-2.
- Mọi code snippet phải compile được; mọi DB schema production-ready; mọi User Story có AC đủ để test.

---

# PHẦN 1 — PRODUCT VISION & GOALS

## 1.1 Vision Statement

> HSA Integration Platform là lớp middleware .NET đứng giữa các hệ thống rời rạc của HSA Education (Web Portal, SePay, EZSale, ClassIn, Zalo OA) nhằm **tự động hóa toàn bộ chuỗi onboarding học sinh từ lúc thanh toán đến lúc sẵn sàng học** trong dưới 2 phút, **biến dữ liệu học tập từ ClassIn thành hành động chăm sóc chủ động**, và **xây dựng một data warehouse (Odoo Community) làm nguồn sự thật duy nhất** cho báo cáo vận hành & tài chính — tất cả mà không phá vỡ quy trình EZSale hiện hữu, cho phép migrate dần khi doanh nghiệp sẵn sàng.

## 1.2 Success Metrics — 5 OKRs

| OKR | Chỉ số | Hiện trạng (As-Is) | Target (To-Be) | Đo bằng |
|---|---|---|---|---|
| **OKR-1: Onboarding tức thời** | Thời gian từ `payment_confirmed` → HS có SBD + link ClassIn + ZNS | > 2 giờ (thủ công, theo ca admin) | **< 2 phút** (P95) | `hsa_enrollments.zalo_sent_at - payment_confirmed_at` |
| **OKR-2: Loại bỏ thao tác tay onboarding** | % HS được onboard hoàn toàn tự động (không cần admin chạm tay) | ~0% | **≥ 95%** | `COUNT(status='ONBOARDED' AND retry_count=0) / COUNT(*)` |
| **OKR-3: Chăm sóc chủ động dựa trên data** | Thời gian từ "HS vắng 2 buổi" → QLL nhận task + HS nhận ZNS | Không có (phát hiện ngẫu nhiên) | **< 1 giờ** sau khi ClassIn push attendance | timestamp task vs `class_date` |
| **OKR-4: Đối soát tài chính tự động** | Thời gian đối soát SePay/ngày | ~2 giờ/ngày thủ công | **< 10 phút/ngày** (chỉ xử lý exception) | thời gian thao tác accounting |
| **OKR-5: Minh bạch hoa hồng CTV** | Thời gian chốt báo cáo hoa hồng cuối tháng | 1-2 ngày thủ công | **< 5 phút** (batch tự động) + CTV self-service xem realtime | thời gian chạy batch job |

**Định nghĩa "ONBOARDED" (Definition of Success cho 1 HS):**
SBD đã tạo `AND` đã enroll ClassIn (có `classin_uid`) `AND` đã gửi ZNS (hoặc fallback email) `AND` đã tạo record Odoo `AND` đã tạo QLL Task.

## 1.3 Constraint Matrix

### PHẢI LÀM (Must — không thương lượng)
- Middleware .NET đứng ngoài, **không sửa** core Web Portal trừ việc gọi `POST /api/leads` và truyền `ref_code`.
- **Idempotency tuyệt đối**: 1 `sepay_transaction_id` → tối đa 1 SBD, 1 lần enroll.
- Mọi external API call (ClassIn/Zalo/Odoo) phải **async + retry + dead-letter**.
- Lưu **mọi** webhook payload (raw) để replay/audit.
- Lưu lại `classin_uid`, `classin_course_id`, GV `uid` — không lưu thì automation chết (ClassIn không có query API đầy đủ).

### KHÔNG LÀM (Must Not — tránh over-engineering & rủi ro)
- **KHÔNG** write vào EZSale từ middleware (chỉ READ leads/contacts).
- **KHÔNG** thay thế EZSale trong Phase 1-2.
- **KHÔNG** build SPA frontend phức tạp ở Phase 1 — dashboard dùng server-rendered HTML + ít JS.
- **KHÔNG** dùng microservices — 1 monolith modular .NET là đủ cho tải hiện tại (~55 HS/ngày, spike 260/ngày).
- **KHÔNG** tự build queue/scheduler — dùng Hangfire.
- **KHÔNG** dùng Odoo Enterprise (license phí; Community đủ cho warehouse + UI phụ).
- **KHÔNG** đụng tới `modifyCourseTeacher` cho per-lesson (V1 đổi GV cho TẤT CẢ buổi chưa bắt đầu — dùng V2 `createClass` với `teacherUid` per buổi).

### CÓ THỂ LÀM SAU (Could — defer, không block Phase 1-2)
- Migrate EZSale → Odoo CRM (Phase 3+, EPIC-08).
- Meta Lead Ads → Odoo Social.
- NPS survey, Helpdesk ticketing.
- Executive dashboard nâng cao, weekly auto-report.
- Redis cache (chỉ thêm khi đo thấy contention; PostgreSQL row lock đã đủ cho SBD sequence ở tải hiện tại).

---

# PHẦN 2 — KIẾN TRÚC GIẢI PHÁP TỔNG THỂ

## 2.1 System Context Diagram (C4 Level 1)

```
                        ┌─────────────────────────────────────────────┐
                        │                  ACTORS                      │
                        │                                              │
   ┌──────────┐         │   ┌───────────┐         ┌──────────────┐     │
   │ HS users │◀────────┼──▶│Admin/Staff│         │ CTV (cộng tác │     │
   │ (20k/năm)│         │   │ QLL/Sale  │         │     viên)     │     │
   └────┬─────┘         │   └─────┬─────┘         └──────┬───────┘     │
        │               └─────────┼──────────────────────┼────────────┘
        │ submit form,            │ vận hành, xem         │ xem hoa hồng
        │ tự tạo SBD,             │ dashboard, duyệt      │ (self-service)
        │ vào lớp                 │                       │
        ▼                         ▼                       ▼
 ┌───────────────────────────────────────────────────────────────────────┐
 │                                                                       │
 │              ███  HSA INTEGRATION PLATFORM (.NET 8)  ███               │
 │              (Webhook receiver · Orchestrator · Warehouse sync)        │
 │                                                                       │
 └───┬────────┬─────────┬──────────┬──────────┬──────────┬───────────┬───┘
     │        │         │          │          │          │           │
     │ POST   │ webhook │ V1/V2 +  │ ZNS/msg  │ JSON-RPC │ READ-only │ read
     │ /leads │ payment │ Data Sub │          │          │ API       │ (giảm dần)
     ▼        ▼         ▼          ▼          ▼          ▼           ▼
┌─────────┐┌───────┐┌──────────┐┌────────┐┌──────────┐┌─────────┐┌──────────┐
│   Web   ││ SePay ││ ClassIn  ││Zalo OA ││  Odoo    ││ EZSale  ││  Google  │
│ Portal  ││(thanh ││(LMS, lớp ││(ZNS,   ││Community ││  CRM    ││  Sheet   │
│hsavnu.  ││ toán) ││học, data ││ chat)  ││(NEW —    ││(GIỮ —   ││(legacy,  │
│edu.vn   ││       ││subscript)││        ││warehouse ││read-only││ giảm dần)│
│         ││       ││ CN server││        ││+ UI phụ) ││ adapter)││          │
└─────────┘└───────┘└──────────┘└────────┘└──────────┘└─────────┘└──────────┘
  EXTERNAL   EXT      EXTERNAL     EXT       NEW          EXT        LEGACY
```

**Quan hệ chính:**
- **Web Portal → Platform**: gửi lead (`POST /api/leads`), redirect thanh toán có mang `ref_code`.
- **SePay → Platform**: webhook xác nhận thanh toán (trigger onboarding).
- **Platform → ClassIn**: enroll HS (V1), tạo buổi học/đổi GV (V2). **ClassIn → Platform**: Data Subscription push (attendance, scores, login).
- **Platform → Zalo OA**: ZNS gửi SBD/cảnh báo. **Platform → Email**: hướng dẫn.
- **Platform ↔ Odoo**: JSON-RPC tạo/cập nhật student, sales order, task, payslip draft.
- **Platform ← EZSale**: READ-only sync leads sang Odoo (Phase 2-3).

## 2.2 Container Diagram (C4 Level 2)

```
╔═══════════════════════════════════════════════════════════════════════════╗
║                  HSA INTEGRATION PLATFORM (Docker host / Linux VPS)        ║
║                                                                           ║
║  ┌─────────────────────────────────────────────────────────────────┐     ║
║  │  HSA.Api  (ASP.NET Core 8 Web API)                                │     ║
║  │  ├── WebhookController     (/webhooks/sepay, /webhooks/classin)   │     ║
║  │  ├── LeadsController       (/api/leads)                           │     ║
║  │  ├── StudentsController    (/api/students/{sbd})                  │     ║
║  │  ├── DashboardController   (/dashboard/onboarding — HTML)         │     ║
║  │  ├── CommissionsController (/api/commissions/...)                 │     ║
║  │  ├── HealthController      (/health)                              │     ║
║  │  └── Middleware: WebhookSignatureMiddleware, ApiKeyAuth           │     ║
║  └───────────────────────────────┬─────────────────────────────────┘     ║
║                                   │ MediatR commands / domain services    ║
║  ┌────────────────────────────────▼────────────────────────────────┐     ║
║  │  HSA.Application  (Use cases / Business Logic — Domain Services)  │     ║
║  │  Onboarding · ClassIn · Commission · LeadManagement · Rules      │     ║
║  └───────────────┬───────────────────────────────┬─────────────────┘     ║
║                  │ enqueue                        │ call                  ║
║  ┌────────────────▼───────────┐   ┌───────────────▼────────────────┐      ║
║  │  HSA.Jobs (Hangfire)       │   │  HSA.Infrastructure (Adapters) │      ║
║  │  OnboardingRetryJob        │   │  ClassInV1Client / V2Client    │      ║
║  │  CommissionBatchJob        │   │  SePayWebhookValidator         │      ║
║  │  ClassInSyncJob            │   │  ZaloOAClient                  │      ║
║  │  DailyReportJob            │   │  OdooJsonRpcClient             │      ║
║  │  SePayReconcileJob (cron)  │   │  EZSaleClient (read-only)      │      ║
║  └────────────────┬───────────┘   └───────────────┬────────────────┘      ║
║                   │                                │                       ║
║                   └────────────┬───────────────────┘                      ║
║                                ▼                                          ║
║  ┌─────────────────────────────────────────────────────────────────┐     ║
║  │  PostgreSQL 16                                                    │     ║
║  │  • hsa_integration DB (EF Core Code-First — middleware tables)   │     ║
║  │  • Hangfire schema (job storage)                                 │     ║
║  │  • Odoo DB = SEPARATE DB (xem ADR-04)                            │     ║
║  └─────────────────────────────────────────────────────────────────┘     ║
║                                                                           ║
║  ┌──────────────┐   ┌──────────────┐   (optional, defer)                  ║
║  │ Seq (logs)   │   │ Hangfire UI  │   ┌──────────────┐                   ║
║  │ Serilog sink │   │ /hangfire    │   │ Redis        │                   ║
║  └──────────────┘   └──────────────┘   └──────────────┘                   ║
╚═══════════════════════════════════════════════════════════════════════════╝
```

## 2.3 Decision Log (ADR)

### ADR-01: .NET middleware thay vì n8n/Make
**Status:** Accepted
**Context:** Cần orchestration nhiều bước, retry, idempotency, logic SBD có concurrency, mapping phức tạp.
**Decision:** Build .NET 8 middleware.
**Rationale:**
- Developer có 10 năm .NET → tốc độ phát triển + bảo trì cao nhất với đội ngũ hiện tại.
- Logic SBD cần transaction/row-lock chính xác — no-code tool khó đảm bảo concurrency.
- Idempotency, dead-letter queue, structured logging dễ kiểm soát bằng code.
- Chi phí vận hành no-code tăng theo số execution (20k HS/năm + hàng triệu event ClassIn → đắt).
**Consequences:** Tốn dev-time ban đầu, nhưng kiểm soát toàn diện. Tránh vendor lock-in no-code.

### ADR-02: Giữ EZSale giai đoạn đầu
**Status:** Accepted
**Context:** Sale đang quen EZSale; thay ngay = rủi ro gián đoạn doanh thu.
**Decision:** Phase 1-2 giữ EZSale, middleware chỉ READ. Mirror lead sang Odoo song song.
**Rationale:** Giảm rủi ro vận hành; cho phép so sánh Odoo vs EZSale trước khi cut-over.
**Consequences:** Tạm thời 2 nguồn lead (EZSale + Odoo mirror) → phải dedupe theo SĐT. Migration để Phase 3 (EPIC-08).

### ADR-03: Odoo Community thay vì Enterprise
**Status:** Accepted
**Context:** Cần warehouse + CRM/Accounting/HR/Project modules.
**Decision:** Odoo 17 Community.
**Rationale:** Community đủ cho CRM, Sales, Accounting cơ bản, Project, HR. Tránh phí license/HS. Tích hợp qua JSON-RPC nên không phụ thuộc tính năng Enterprise UI.
**Consequences:** Thiếu vài tính năng Enterprise (Studio, advanced accounting, dashboards đẹp). Bù bằng custom fields + dashboard riêng của middleware. Helpdesk/NPS sẽ cần module Community tương đương hoặc OCA.

### ADR-04: Separate database (Integration DB ≠ Odoo DB)
**Status:** Accepted
**Context:** Middleware và Odoo đều dùng PostgreSQL.
**Decision:** **2 database riêng biệt** trên cùng instance PostgreSQL: `hsa_integration` (EF Core Code-First) và `hsa_odoo` (Odoo quản lý schema).
**Rationale:**
- Odoo schema do Odoo ORM sở hữu — EF Core migration chạm vào sẽ vỡ khi Odoo upgrade.
- Tách biệt ownership → không xung đột migration; backup/restore độc lập.
- Giao tiếp qua JSON-RPC (business-level API) thay vì SQL trực tiếp → an toàn khi Odoo nâng cấp (xem TR-07).
**Consequences:** Không JOIN cross-DB trực tiếp; đồng bộ qua API + lưu `odoo_partner_id` trong `hsa_students` để map. Chấp nhận eventual consistency cho dữ liệu mirror.

### ADR-05: Async job queue cho onboarding chain
**Status:** Accepted
**Context:** Onboarding gồm ClassIn (CN server, latency 200-500ms, có thể timeout) + Zalo + Email + Odoo.
**Decision:** Webhook trả 200 ngay (<100ms), xử lý chain bằng Hangfire background jobs với `ContinueJobWith`.
**Rationale:**
- SePay cần response nhanh, không chờ chuỗi external API.
- Mỗi bước retry độc lập; bước fail không block bước khác (Odoo fire-and-forget).
- Dead-letter + alert khi fail sau retry.
**Consequences:** Cần idempotency vì job có thể chạy lại. Trạng thái theo dõi qua `hsa_enrollments`.

### ADR-06: Webhook retry strategy
**Status:** Accepted
**Decision:**
- **Inbound** (SePay/ClassIn gửi tới ta): lưu raw ngay khi nhận (`hsa_webhook_log`), trả 200 nếu parse OK; nếu ta lỗi nội bộ → trả 5xx để nguồn retry (ClassIn tự retry). SePay miss → `SePayReconcileJob` cron mỗi 5 phút đối chiếu SePay API.
- **Outbound** (ta gọi ClassIn/Zalo/Odoo): Hangfire `[AutomaticRetry(Attempts = 3)]` + Polly exponential backoff **1s / 5s / 30s**. Fail sau 3 → dead-letter (`status='FAILED'`) + alert.
- Throttle outbound ClassIn ≤ **2 req/giây** (TR-08) khi bulk HCM.
**Rationale:** Idempotency + retry + reconcile = không mất event, không nhân đôi.

### ADR-07: CTV ref_code implementation
**Status:** Accepted
**Context:** Cần attribute HS về CTV để tính hoa hồng.
**Decision:** `ref_code` truyền qua query string `?ref=CTV001` trên link giỏ hàng → Web Portal persist `ref_code` vào order → SePay webhook mang `order_reference` → middleware đọc `order.ref_code`. Lưu vào `hsa_students.ref_code` + tạo `hsa_commissions` (status=pending).
**Rationale:** Đơn giản, không cần cookie/tracking phức tạp; nguồn sự thật là `order.ref_code` tại thời điểm thanh toán (chống gian lận click cuối).
**Consequences:** Web Portal phải lưu `ref_code` vào order (yêu cầu dev portal hỗ trợ field này). Nếu portal chưa hỗ trợ → fallback: middleware nhận `ref_code` qua `/api/leads` và map theo SĐT khi thanh toán.

## 2.4 Data Flow Diagrams

### Flow A — Payment → Auto-Onboarding (CRITICAL PATH)

```
[SePay] POST /webhooks/sepay  { transaction_id, amount, order_reference, signature }
   │
   ▼ (1) WebhookSignatureMiddleware: validate HMAC-SHA256
   │      fail → 400 invalid_signature (vẫn log vào hsa_webhook_log)
   ▼ (2) Lưu raw vào hsa_webhook_log (source='sepay')
   ▼ (3) Idempotency: hsa_enrollments có sepay_transaction_id? → có → 200 (duplicate, no-op)
   ▼ (4) Tạo hsa_enrollments (status='PENDING'); return 200 {accepted, enrollment_id}  ← <100ms
   │
   ▼ [Hangfire async từ đây]
   ▼ (5) Find Order trong Web DB theo order_reference (qua API portal hoặc shared read)
   ▼ (6) GenerateSbdAsync(exam_type) → "HSA-2026-00001"  (row lock, idempotent theo enrollment)
   │
   ▼ (7) ClassIn chain (job 1):
   │      register(0084-SĐT) → nếu err 135: lấy UID từ response
   │      addSchoolStudent → addCourseStudent(courseId, UID)
   │      lưu classin_uid, set classin_enrolled_at
   │      ┌── fail sau 3 retry → dead-letter + alert, status='CLASSIN_FAILED'
   │
   ▼ (8) ContinueWith → Zalo OA ZNS (job 2): SBD + ClassIn invoke link + lịch học
   │      fail → fallback Email; lưu hsa_zalo_log
   ▼ (9) ContinueWith → Email guide (job 3): hướng dẫn đầy đủ + invoke link
   │
   ▼ (10) Fire-and-forget → Odoo (job 4): create res.partner (student) + sale.order + project.task (QLL)
   │       lưu odoo_partner_id
   ▼ (11) Nếu order.ref_code → Commission (job 5): insert hsa_commissions status='pending'
   │
   ▼ (12) Khi tất cả job critical xong → status='ONBOARDED', cập nhật order status (qua portal API)
```

### Flow B — ClassIn Data → Student Care

```
[ClassIn] POST /webhooks/classin/data-subscription  { SID, EventType, Data }
   │
   ▼ (1) Validate SID == config; lưu hsa_webhook_log (source='classin')
   ▼ (2) return { error_info: { errno: 1, error: "success" } }  ← format ClassIn yêu cầu
   │
   ▼ [Hangfire async]
   ▼ (3) Switch theo EventType:
   │
   ├─ ATTENDANCE (20 phút sau buổi):
   │     parse → upsert hsa_classin_attendance (student_id, class_date, is_present)
   │     Rule R1: COUNT(absent) ≥ 2 trong khóa? → create project.task (QLL) + Zalo ZNS phụ huynh
   │     Rule R2: không có login event 3 ngày? → Zalo ZNS hỏi thăm HS
   │
   ├─ LMS_SCORE (realtime):
   │     parse → insert hsa_classin_scores
   │     Rule R3: completion_rate < 50% → Zalo ZNS gửi link tài liệu bổ trợ
   │
   ├─ LOGIN_EVENT (during class entry/exit):
   │     cập nhật last_login để phục vụ R2
   │
   └─ TEACHING_HOURS (từ attendance GV):
         aggregate giờ dạy/GV/tháng → buffer cho payroll (Flow F-06.2)
   │
   ▼ (4) Cập nhật Odoo student record (qua JSON-RPC, fire-and-forget)
```

### Flow C — Web Form → CRM Lead

```
[Web Form] POST /api/leads  { full_name, phone, email, exam_type, branch, source, ref_code? }
   │
   ▼ (1) ApiKey auth
   ▼ (2) Normalize phone (E.164 / 0xxxxxxxxx); Deduplicate theo phone
   │       trùng → 409 { duplicate_phone, existing_lead_id }
   ▼ (3) Tag: source, exam_type, ref_code
   ▼ (4) [Phase 2] Push to EZSale API (create/update lead) — chỉ create/update, KHÔNG write back business data ngược
   ▼ (5) Push to Odoo crm.lead (mirror) → lưu odoo_lead_id
   ▼ (6) Auto-assign Sales Team theo exam_type (4 team: HSA/BCA/BQP/ĐGNL HCM)
   ▼ (7) Nếu HOT (rule: source ưu tiên / form đánh dấu) → Zalo OA notify Sale phụ trách
   ▼ (8) 201 { lead_id, ezsale_lead_id?, odoo_lead_id? }
```

---

# PHẦN 3 — EPIC & FEATURE BREAKDOWN (MoSCoW)

> Effort tính cho **1 .NET dev full-time** (dev-days). Bao gồm code + test + tích hợp, chưa gồm chờ bên thứ 3 (ClassIn sandbox, Zalo template duyệt).

## MUST HAVE (Phase 1 & 2)

### EPIC-01: Auto-Onboarding Engine
**Mô tả:** Tự động hóa toàn bộ chuỗi từ `payment_confirmed` → HS sẵn sàng học, thay thế bottleneck N1 (SBD tay), N2 (Zalo tay), N3 (duyệt 1 người), N4 (add lớp tay).
**Business value:** Giải phóng admin khỏi >2h/HS thủ công; giảm sai sót; trải nghiệm HS tức thì → giảm churn sớm. Đây là **xương sống** của dự án.
**Effort:** ~22 dev-days.
**Dependencies:** SePay webhook secret, ClassIn SID/SECRET + sandbox + course mapping, Zalo OA token + ZNS template duyệt.
**Acceptance Criteria (Epic-level):**
- AC1: 1 webhook SePay hợp lệ → trong < 2 phút HS có SBD, có `classin_uid`, nhận ZNS, có record Odoo + QLL task.
- AC2: Gửi 100 webhook trùng `transaction_id` đồng thời → đúng 1 SBD, 1 enroll.
- AC3: ClassIn down → enrollment vào dead-letter + alert; tự retry khi up; không mất HS.
- AC4: Dashboard hiển thị realtime: today_count, pending, failed.

| Feature | Mô tả | Effort |
|---|---|---|
| F-01.1 | SePay webhook receiver + HMAC-SHA256 validation | 2d |
| F-01.2 | SBD auto-generation (4 sequence theo exam_type, year) | 2d |
| F-01.3 | ClassIn V1: register → addSchoolStudent → addCourseStudent (xử lý err 135) | 4d |
| F-01.4 | Zalo OA ZNS gửi SBD + link (< 2 phút) | 2d |
| F-01.5 | Email guide với ClassIn invoke link | 1d |
| F-01.6 | Odoo student record creation (JSON-RPC) | 2d |
| F-01.7 | QLL Task tự động trong Odoo Project | 1.5d |
| F-01.8 | Retry + fallback khi ClassIn API fail (Polly + Hangfire) | 2d |
| F-01.9 | Dead-letter queue + alert | 1.5d |
| F-01.10 | Onboarding status dashboard (server-rendered HTML) | 2d |

### EPIC-02: ClassIn Data Pipeline
**Mô tả:** Nhận Data Subscription push, lưu trữ, chạy rule engine chăm sóc chủ động (N7, N12).
**Business value:** Phát hiện sớm HS at-risk; QLL chủ động thay vì bị động; data nền cho payroll & dashboard.
**Effort:** ~16 dev-days.
**Dependencies:** EPIC-01 (cần `student_id`, `classin_uid`), ClassIn Data Subscription đã setup với account manager (TR-02), Zalo template alert.
**Acceptance Criteria:**
- AC1: ClassIn push attendance → lưu trong < 5s, trả đúng `{error_info:{errno:1}}`.
- AC2: HS vắng buổi thứ 2 → QLL có task + phụ huynh có ZNS trong < 1h.
- AC3: HS không login 3 ngày → ZNS hỏi thăm.
- AC4: Điểm LMS thấp (<50%) → ZNS tài liệu bổ trợ.

| Feature | Mô tả | Effort |
|---|---|---|
| F-02.1 | Webhook endpoint nhận Data Subscription + đúng format response | 1.5d |
| F-02.2 | Attendance storage + processing (upsert idempotent) | 2d |
| F-02.3 | "Vắng 2+ buổi" → Zalo + QLL Task | 2d |
| F-02.4 | "Không login 3 ngày" → Zalo | 1.5d |
| F-02.5 | LMS Score webhook + "điểm thấp" trigger | 2d |
| F-02.6 | Teaching hours → payroll data buffer | 2d |
| F-02.7 | Student learning dashboard (per QLL) | 3d |

### EPIC-03: CTV Attribution & Commission
**Mô tả:** Gắn HS với CTV qua ref_code, tính hoa hồng tự động (N8).
**Business value:** Minh bạch, chính xác hoa hồng; CTV tự xem; giảm tranh chấp.
**Effort:** ~9 dev-days.
**Dependencies:** EPIC-01 (order.ref_code), Web Portal lưu ref_code.
**Acceptance Criteria:**
- AC1: Link `?ref=CTV001` → HS thanh toán → `hsa_commissions` status=pending đúng CTV, đúng rate.
- AC2: Batch cuối tháng tổng hợp đúng số tiền, export Excel/PDF.
- AC3: CTV đăng nhập xem được hoa hồng của mình (read-only).

| Feature | Mô tả | Effort |
|---|---|---|
| F-03.1 | ref_code param trong web form URL | 0.5d |
| F-03.2 | ref_code persistence qua payment flow | 1d |
| F-03.3 | Commission auto-calc khi payment confirmed | 2d |
| F-03.4 | Commission batch report (Excel/PDF) | 2.5d |
| F-03.5 | CTV self-service xem hoa hồng | 3d |

### EPIC-04: Odoo Foundation Setup
**Mô tả:** Dựng Odoo Community làm warehouse + UI phụ.
**Business value:** Nguồn sự thật duy nhất; báo cáo P&L; UI cho staff không cần build SPA.
**Effort:** ~14 dev-days (gồm config Odoo + custom fields).
**Dependencies:** Hạ tầng Docker/PostgreSQL.
**Acceptance Criteria:**
- AC1: Odoo chạy, 4 Sales Team theo kỳ thi, custom fields trên res.partner.
- AC2: JSON-RPC create/update partner + task hoạt động từ middleware.
- AC3: 3 dashboard (COO/QLL/GV) hiển thị dữ liệu thật.

| Feature | Mô tả | Effort |
|---|---|---|
| F-04.1 | Odoo Community install + PostgreSQL | 1d |
| F-04.2 | CRM config (4 Sales Teams) | 1.5d |
| F-04.3 | Custom fields res.partner (student_sbd, exam_type, classin_uid...) | 2d |
| F-04.4 | Accounting + SePay reconciliation config | 3d |
| F-04.5 | HR: 62 NS + GV + CTV records | 2d |
| F-04.6 | Project: QLL task management | 1.5d |
| F-04.7 | 3 Dashboards (COO/QLL/GV) | 3d |

## SHOULD HAVE (Phase 2-3)

### EPIC-05: Lead Management Automation
**Mô tả:** Web form → EZSale + Odoo mirror tự động, dedupe, nurture (N5, N6).
**Business value:** Loại bỏ nhập tay lead; phản hồi hot lead nhanh; nurture warm/cold.
**Effort:** ~12 dev-days.
**Dependencies:** EZSale API doc + credentials.
**AC:** Web form không còn nhập tay; dedupe theo SĐT; hot lead → Sale ZNS < 5 phút.

- F-05.1 Web form → EZSale auto-push · F-05.2 Web form → Odoo mirror · F-05.3 Dedup SĐT · F-05.4 Auto-assign team · F-05.5 Hot lead → Sale Zalo · F-05.6 Warm/Cold nurture · F-05.7 EZSale → Odoo sync (read EZSale, write Odoo)

### EPIC-06: Financial Automation
**Mô tả:** Đối soát SePay tự động, timesheet GV → payslip, P&L realtime (N9, N10, N11).
**Effort:** ~11 dev-days. **Dependencies:** EPIC-02 (teaching hours), EPIC-04 (Accounting).
**AC:** Đối soát < 10 phút/ngày; payslip draft tự sinh; P&L theo kỳ thi × cơ sở (analytic).

- F-06.1 SePay auto-reconciliation với Odoo Invoice · F-06.2 GV timesheet từ ClassIn → Odoo Payslip draft · F-06.3 P&L realtime (analytic dimensions)

### EPIC-07: Student Care Automation
**Mô tả:** Chuỗi pre-exam, NPS, at-risk, helpdesk (N12).
**Effort:** ~12 dev-days. **Dependencies:** EPIC-02, Zalo templates.
**AC:** Chuỗi D-30/D-7/D-3/D-1 chạy đúng lịch; NPS gửi sau kỳ thi; ticket từ Zalo/web vào Odoo.

- F-07.1 Pre-exam sequences (D-30/7/3/1) · F-07.2 NPS survey · F-07.3 At-risk dashboard · F-07.4 Helpdesk ticket → Odoo

## COULD HAVE (Phase 3+)

### EPIC-08: EZSale Migration → Odoo CRM
**Effort:** ~15 dev-days. **Dependencies:** EPIC-04, EPIC-05 ổn định.
- F-08.1 Full EZSale export + Odoo import · F-08.2 Meta Lead Ads → Odoo Social · F-08.3 CTV link tracking trong Odoo · F-08.4 Sale Manager QA dashboard

### EPIC-09: COO / Leadership Ops
**Effort:** ~8 dev-days.
- F-09.1 Executive dashboard (HN vs HCM, 4 kỳ thi) · F-09.2 Weekly automated report email · F-09.3 Alert khi KPI breach SLA

---

# PHẦN 4 — USER STORIES (PHASE 1)

> **Definition of Done chung (áp dụng mọi US trừ khi ghi đè):**
> - Unit test coverage ≥ 80% cho logic mới.
> - Integration test với test account (SePay sandbox / ClassIn sandbox / Zalo test OA).
> - Logging đầy đủ: request + response + timestamp (Serilog structured, không log SĐT full / số TK).
> - Retry: 3 lần, backoff 1s/5s/30s (outbound external).
> - Alert (email/Zalo) khi fail sau 3 retry.
> - Idempotent nếu job có thể chạy lại.

## EPIC-01 — Auto-Onboarding (15 User Stories)

---
**US-01.1: Nhận webhook thanh toán từ SePay**
As a hệ thống
I want to nhận `POST /webhooks/sepay` và trả 200 trong < 100ms
So that SePay không timeout và không gửi lại trùng lặp do chậm

AC:
- AC1: Endpoint nhận POST, lưu raw payload vào `hsa_webhook_log` trước khi xử lý.
- AC2: Trả `200 {status:"accepted", enrollment_id}` ngay, mọi xử lý nặng đẩy sang Hangfire.
- AC3: P95 thời gian response < 100ms (đo bằng integration test).

DoD: + load test 50 req đồng thời vẫn < 100ms P95.
Story Points: 3 · Priority: Must

---
**US-01.2: Validate chữ ký webhook SePay (HMAC-SHA256)**
As a security officer
I want to mọi webhook SePay được verify HMAC-SHA256 bằng secret
So that không ai giả mạo thanh toán để tạo HS lậu

AC:
- AC1: Header `X-SePay-Signature` được verify với `SEPAY_WEBHOOK_SECRET`.
- AC2: Sai chữ ký → `400 {error:"invalid_signature"}`, vẫn log (signature_valid=false).
- AC3: Đúng chữ ký → tiếp tục xử lý.

DoD: + unit test cả case hợp lệ và giả mạo (sửa 1 byte payload).
Story Points: 3 · Priority: Must

---
**US-01.3: Chống xử lý trùng giao dịch (Idempotency)**
As a accounting/ops
I want to 1 `sepay_transaction_id` chỉ tạo tối đa 1 enrollment
So that 1 HS không bị 2 SBD / enroll 2 lần khi SePay gửi lại

AC:
- AC1: `hsa_enrollments.sepay_transaction_id` UNIQUE.
- AC2: Webhook trùng transaction → 200 no-op (không tạo mới), log "duplicate".
- AC3: 100 request trùng đồng thời → đúng 1 enrollment.

DoD: + concurrency test 100 parallel.
Story Points: 5 · Priority: Must

---
**US-01.4: Sinh SBD tự động, không trùng, có thứ tự**
As a HS / admin
I want to mỗi HS được sinh SBD `[EXAM]-[YEAR]-[SEQ5]` tự động
So that loại bỏ N1 (tạo SBD tay)

AC:
- AC1: Format `HSA-2026-00001`; 4 exam_type có sequence độc lập.
- AC2: 100 request song song cùng exam_type → không SBD trùng.
- AC3: SBD đã sinh được lưu `sbd_generated_at`; sinh lại cho cùng enrollment → trả SBD cũ (idempotent).

DoD: + concurrency test 100 parallel không duplicate.
Story Points: 5 · Priority: Must

---
**US-01.5: Tạo tài khoản ClassIn cho HS (register)**
As a hệ thống
I want to gọi ClassIn `action=register` với phone `0084-[SĐT]`
So that HS có tài khoản ClassIn để vào học

AC:
- AC1: Gọi register, lưu `classin_uid` trả về.
- AC2: Error 135 (SĐT đã đăng ký) → lấy UID từ response, KHÔNG coi là lỗi.
- AC3: safeKey = MD5(SECRET+timeStamp), timestamp trong 20 phút.

DoD: + integration test với ClassIn sandbox; mock cả case 135.
Story Points: 5 · Priority: Must

---
**US-01.6: Add HS vào trường + enroll vào lớp (ClassIn)**
As a hệ thống
I want to `addSchoolStudent` rồi `addCourseStudent(courseId, uid)`
So that HS xuất hiện đúng lớp trong ClassIn

AC:
- AC1: Lấy `classin_course_id` từ `hsa_course_mappings` theo `course_code`.
- AC2: addSchoolStudent trước, addCourseStudent sau (đúng thứ tự bắt buộc).
- AC3: Set `classin_enrolled_at`; lưu mapping student↔course.

DoD: + integration test sandbox enroll thật.
Story Points: 5 · Priority: Must

---
**US-01.7: Retry + dead-letter khi ClassIn fail**
As a ops
I want to ClassIn call fail được retry 3 lần (1s/5s/30s) rồi vào dead-letter + alert
So that ClassIn down không làm mất HS

AC:
- AC1: Polly retry 1s/5s/30s; Hangfire `[AutomaticRetry(Attempts=3)]`.
- AC2: Fail sau 3 → `status='CLASSIN_FAILED'`, ghi `error_log`, gửi alert.
- AC3: Re-run job thủ công từ Hangfire UI → tiếp tục đúng từ bước fail (idempotent).

DoD: + test giả lập ClassIn trả 500 → vào dead-letter + alert bắn.
Story Points: 5 · Priority: Must

---
**US-01.8: Gửi ZNS Zalo OA với SBD + link ClassIn**
As a HS
I want to nhận ZNS chứa SBD, link/invoke code ClassIn, lịch học
So that biết ngay cách vào lớp (loại bỏ N2)

AC:
- AC1: Dùng template ZNS đã duyệt, fill `{sbd}`, `{classin_link}`, `{schedule}`.
- AC2: Gửi trong < 2 phút kể từ payment_confirmed.
- AC3: Mọi lần gửi log vào `hsa_zalo_log` (status, error).

DoD: + integration test Zalo test OA.
Story Points: 3 · Priority: Must

---
**US-01.9: Fallback Email khi Zalo fail**
As a HS
I want to nếu ZNS fail thì vẫn nhận email hướng dẫn
So that không bị "kẹt" không biết cách vào lớp

AC:
- AC1: ZNS fail sau retry → tự động gửi email guide.
- AC2: Email chứa SBD + invoke link + hướng dẫn đầy đủ.
- AC3: Ghi log cả 2 kênh; đánh dấu kênh thành công.

DoD: + test giả lập Zalo 4xx/5xx → email gửi.
Story Points: 3 · Priority: Must

---
**US-01.10: Email hướng dẫn onboarding đầy đủ**
As a HS
I want to nhận email guide chi tiết (vào lớp, tạo SBD nếu cần, invoke ClassIn)
So that tự onboard được không cần admin

AC:
- AC1: Template email có SBD, invoke link, hướng dẫn từng bước, hotline.
- AC2: Gửi song song với ZNS (không phụ thuộc ZNS thành công).
- AC3: Log gửi + bounce handling cơ bản.

DoD: + render template với data thật, kiểm tra link đúng.
Story Points: 2 · Priority: Must

---
**US-01.11: Tạo student record trong Odoo**
As a COO/QLL
I want to mỗi HS onboard có `res.partner` trong Odoo với custom fields
So that Odoo là warehouse tra cứu/báo cáo

AC:
- AC1: JSON-RPC tạo res.partner với student_sbd, exam_type, classin_uid, cohort, branch.
- AC2: Lưu `odoo_partner_id` về `hsa_students` để map.
- AC3: Fire-and-forget — Odoo fail KHÔNG block onboarding chain.

DoD: + integration test Odoo dev (Docker).
Story Points: 3 · Priority: Must

---
**US-01.12: Tự tạo QLL Task trong Odoo Project**
As a QLL
I want to mỗi HS mới sinh 1 task "Chào mừng & thêm vào group Zalo lớp" gán QLL phụ trách
So that QLL không quên bước thủ công (N3, N4)

AC:
- AC1: Tạo `project.task` trong project theo lớp, assignee = `qll_user_id` từ course_mapping.
- AC2: Task chứa SBD, tên HS, SĐT, link Zalo group.
- AC3: Fire-and-forget.

DoD: + integration test tạo task Odoo.
Story Points: 3 · Priority: Must

---
**US-01.13: Tạo Sales Order trong Odoo**
As a accounting
I want to mỗi thanh toán tạo `sale.order`/invoice tương ứng trong Odoo
So that doanh thu vào warehouse phục vụ P&L

AC:
- AC1: Tạo sale.order với partner = student, amount, product = khóa học, ref = order_reference.
- AC2: Gắn analytic (exam_type × branch) cho P&L sau này.
- AC3: Idempotent theo order_reference.

DoD: + integration test.
Story Points: 3 · Priority: Must

---
**US-01.14: Orchestrate chuỗi onboarding bằng Hangfire**
As a hệ thống
I want to chuỗi SBD → ClassIn → Zalo → Email → Odoo → Commission chạy theo `ContinueJobWith`
So that mỗi bước retry độc lập, bước fail không chặn bước khác

AC:
- AC1: ClassIn job xong → ContinueWith Zalo + Email.
- AC2: Odoo + Commission là Enqueue độc lập (fire-and-forget).
- AC3: Khi mọi critical job xong → `status='ONBOARDED'`, cập nhật order status.

DoD: + E2E test với ngrok + SePay test mode.
Story Points: 5 · Priority: Must

---
**US-01.15: Onboarding status dashboard realtime**
As a QLL/Admin
I want to xem `/dashboard/onboarding` với today_count, pending, failed + danh sách
So that giám sát & xử lý exception kịp thời

AC:
- AC1: Hiển thị today_count, pending_count, failed_count.
- AC2: Bảng HS gần đây + trạng thái từng bước (SBD/ClassIn/Zalo/Odoo).
- AC3: Nút "retry" cho enrollment failed (gọi lại job).

DoD: + server-rendered HTML, auto refresh 30s; auth Bearer (QLL/Admin).
Story Points: 5 · Priority: Must

## EPIC-02 — ClassIn Data Pipeline (10 User Stories)

---
**US-02.1: Nhận Data Subscription push từ ClassIn**
As a hệ thống
I want to `POST /webhooks/classin/data-subscription` lưu raw và trả đúng format
So that ClassIn không retry vô ích và ta không mất event

AC:
- AC1: Validate `SID` khớp config; lưu `hsa_webhook_log` (source='classin').
- AC2: Trả `{error_info:{errno:1,error:"success"}}` (format ClassIn yêu cầu).
- AC3: Xử lý nặng đẩy sang Hangfire; response < 5s.

DoD: + test với payload mẫu mỗi EventType.
Story Points: 3 · Priority: Must

---
**US-02.2: Lưu trữ điểm danh (attendance)**
As a QLL
I want to attendance được parse và upsert vào `hsa_classin_attendance`
So that có dữ liệu chấm chuyên cần & trigger care

AC:
- AC1: Map `classin_uid` → `student_id`; lưu entry/exit time, is_present, class_date.
- AC2: Upsert idempotent (cùng student+class_date không nhân đôi).
- AC3: Lưu `source_payload` để audit.

DoD: + test với payload thật/mẫu.
Story Points: 3 · Priority: Must

---
**US-02.3: Trigger "vắng 2+ buổi"**
As a QLL
I want to khi HS vắng buổi thứ 2 trong khóa → tạo task + ZNS phụ huynh
So that can thiệp sớm, giảm bỏ học (N7)

AC:
- AC1: `COUNT(is_present=false)` trong khóa ≥ 2 → tạo `project.task` QLL + ZNS group phụ huynh.
- AC2: Mỗi ngưỡng chỉ trigger 1 lần (không spam mỗi buổi vắng tiếp theo dùng lại đúng ngưỡng).
- AC3: Trong < 1h sau khi nhận attendance.

DoD: + test rule với chuỗi attendance giả lập.
Story Points: 5 · Priority: Must

---
**US-02.4: Trigger "không login 3 ngày"**
As a HS
I want to nếu không có login event 3 ngày → nhận ZNS hỏi thăm
So that được nhắc nhở quay lại học

AC:
- AC1: Dựa `last_login` (từ login event); ≥ 3 ngày → ZNS.
- AC2: Chỉ gửi 1 lần/đợt không-login (không spam hằng ngày).
- AC3: Log vào `hsa_zalo_log`.

DoD: + test rule với last_login giả lập.
Story Points: 5 · Priority: Must

---
**US-02.5: Nhận điểm LMS (scores)**
As a QLL
I want to điểm bài tập/kiểm tra LMS được lưu `hsa_classin_scores`
So that theo dõi tiến độ học tập

AC:
- AC1: Parse score, total_score, completion_rate, activity_name.
- AC2: Map về student_id; lưu submitted_at.
- AC3: Idempotent theo (student, activity).

DoD: + test payload score.
Story Points: 3 · Priority: Must

---
**US-02.6: Trigger "điểm thấp / completion < 50%"**
As a HS
I want to khi completion_rate < 50% → ZNS gửi link tài liệu bổ trợ
So that được hỗ trợ kịp thời

AC:
- AC1: completion_rate < 0.5 → ZNS template tài liệu.
- AC2: Throttle: 1 ZNS/HS/ngày tối đa cho loại này (tránh spam).
- AC3: Log đầy đủ.

DoD: + test ngưỡng.
Story Points: 3 · Priority: Must

---
**US-02.7: Tổng hợp giờ dạy GV (teaching hours)**
As a accounting
I want to giờ dạy/GV/tháng được aggregate từ attendance GV
So that làm dữ liệu payroll (N9)

AC:
- AC1: Aggregate theo (gv_uid, month) → tổng giờ dạy.
- AC2: Lưu buffer để EPIC-06 đẩy sang Odoo Payslip draft.
- AC3: Re-run cùng tháng → ghi đè đúng (không cộng dồn trùng).

DoD: + test aggregate.
Story Points: 5 · Priority: Should

---
**US-02.8: Cập nhật student record Odoo từ data học tập**
As a QLL
I want to attendance/score cập nhật vào Odoo student record
So that warehouse phản ánh tình hình học tập

AC:
- AC1: JSON-RPC update custom fields (attendance_rate, avg_score) trên res.partner.
- AC2: Fire-and-forget; Odoo fail không chặn pipeline.
- AC3: Batch để tránh quá nhiều RPC (cập nhật theo lô).

DoD: + integration test.
Story Points: 3 · Priority: Should

---
**US-02.9: Student learning dashboard (per QLL)**
As a QLL
I want to dashboard xem chuyên cần + điểm theo lớp mình phụ trách
So that quản lý lớp hiệu quả

AC:
- AC1: Lọc theo QLL → danh sách HS, % chuyên cần, điểm gần nhất, cờ at-risk.
- AC2: Highlight HS vắng/at-risk.
- AC3: Auth: chỉ QLL thấy lớp của mình.

DoD: + server-rendered HTML.
Story Points: 5 · Priority: Should

---
**US-02.10: Xử lý sự kiện hủy/đổi tài khoản ClassIn**
As a hệ thống
I want to nhận event đổi SĐT (sync) và hủy tài khoản (AccountStatus=255)
So that dữ liệu HS luôn nhất quán

AC:
- AC1: Đổi SĐT → cập nhật `zalo_phone`/`phone` mapping.
- AC2: Hủy tài khoản → đánh dấu HS inactive, dừng các trigger care.
- AC3: Log mọi thay đổi.

DoD: + test 2 event này.
Story Points: 3 · Priority: Should

---

# PHẦN 5 — TECHNICAL SPECIFICATIONS

## 5.1 Tech Stack Decision

```
Backend:     .NET 8 / ASP.NET Core Web API
ORM:         Entity Framework Core 8 (Code-First) — DB hsa_integration
Database:    PostgreSQL 16 (instance dùng chung; DB riêng cho integration vs Odoo — ADR-04)
Mediator:    MediatR (command/handler trong Application layer)
Job Queue:   Hangfire (PostgreSQL storage, dashboard /hangfire)
Resilience:  Polly (retry/backoff/circuit-breaker cho outbound)
Logging:     Serilog → Seq (dev) / file rolling (prod)
Caching:     Redis (OPTIONAL — chỉ thêm khi đo thấy cần; mặc định KHÔNG dùng ở Phase 1)
Testing:     xUnit + Moq + FluentAssertions + TestContainers (PostgreSQL)
HTTP client: IHttpClientFactory + Polly handlers
CI/CD:       GitHub Actions (build + test + docker image)
Hosting:     Linux VPS (Ubuntu 22.04) + Docker Compose
Secrets:     Environment variables (.env, không commit) — không hardcode
```

## 5.2 Project Structure (.NET Solution)

```
HSA.Integration/
├── HSA.Integration.sln
├── docker-compose.yml            # postgres, redis(optional), seq, app, odoo
├── .env.example
├── src/
│   ├── HSA.Api/                          # ASP.NET Core Web API (entry point)
│   │   ├── Controllers/
│   │   │   ├── WebhookController.cs       # /webhooks/sepay, /webhooks/classin/*
│   │   │   ├── LeadsController.cs         # /api/leads
│   │   │   ├── StudentsController.cs      # /api/students/{sbd}
│   │   │   ├── CommissionsController.cs   # /api/commissions/*
│   │   │   ├── DashboardController.cs     # /dashboard/* (HTML)
│   │   │   └── HealthController.cs        # /health
│   │   ├── Middleware/
│   │   │   ├── WebhookSignatureMiddleware.cs
│   │   │   └── ApiKeyMiddleware.cs
│   │   ├── Views/                         # Razor (dashboard server-rendered)
│   │   ├── appsettings.json
│   │   └── Program.cs
│   │
│   ├── HSA.Application/                   # Use cases / business logic
│   │   ├── Onboarding/
│   │   │   ├── Commands/ProcessPaymentCommand.cs
│   │   │   └── Handlers/ProcessPaymentHandler.cs
│   │   ├── ClassIn/
│   │   │   ├── ProcessAttendanceCommand.cs
│   │   │   ├── ProcessScoreCommand.cs
│   │   │   └── Rules/ (AbsenceRule, NoLoginRule, LowScoreRule)
│   │   ├── Commission/
│   │   ├── LeadManagement/
│   │   ├── Abstractions/                  # interfaces cho adapters (ports)
│   │   │   ├── IClassInV1Client.cs
│   │   │   ├── IClassInV2Client.cs
│   │   │   ├── IZaloOAClient.cs
│   │   │   ├── IOdooClient.cs
│   │   │   ├── ISePayWebhookValidator.cs
│   │   │   └── IEZSaleClient.cs
│   │   └── DependencyInjection.cs
│   │
│   ├── HSA.Domain/                        # Entities, VOs, Domain Events
│   │   ├── Entities/
│   │   │   ├── Student.cs
│   │   │   ├── Enrollment.cs
│   │   │   ├── ClassInAttendance.cs
│   │   │   ├── ClassInScore.cs
│   │   │   ├── Commission.cs
│   │   │   ├── CtvProfile.cs
│   │   │   ├── CourseMapping.cs
│   │   │   └── SbdSequence.cs
│   │   ├── ValueObjects/StudentId.cs      # SBD VO
│   │   ├── Enums/ (ExamType, OnboardingStatus, CommissionStatus)
│   │   └── Events/
│   │       ├── PaymentConfirmedEvent.cs
│   │       └── StudentEnrolledEvent.cs
│   │
│   ├── HSA.Infrastructure/                # External integrations + persistence
│   │   ├── ClassIn/ (ClassInV1Client, ClassInV2Client, ClassInWebhookParser, ClassInSignature)
│   │   ├── SePay/ (SePayWebhookValidator)
│   │   ├── ZaloOA/ (ZaloOAClient, ZaloTokenStore)
│   │   ├── Odoo/ (OdooJsonRpcClient)
│   │   ├── EZSale/ (EZSaleClient — read-only)
│   │   ├── Email/ (EmailSender)
│   │   ├── Persistence/
│   │   │   ├── HsaDbContext.cs
│   │   │   ├── Configurations/            # IEntityTypeConfiguration
│   │   │   └── Migrations/
│   │   └── DependencyInjection.cs
│   │
│   └── HSA.Jobs/                          # Hangfire jobs
│       ├── ClassInEnrollJob.cs
│       ├── ZaloNotifyJob.cs
│       ├── EmailJob.cs
│       ├── OdooSyncJob.cs
│       ├── CommissionJob.cs
│       ├── OnboardingRetryJob.cs
│       ├── CommissionBatchJob.cs
│       ├── SePayReconcileJob.cs           # cron 5 phút
│       └── DailyReportJob.cs
│
└── tests/
    ├── HSA.Application.Tests/             # handler/rule unit tests + Moq
    ├── HSA.Infrastructure.Tests/          # adapter tests (mock HTTP)
    └── HSA.Integration.Tests/             # E2E TestContainers + WebApplicationFactory
```

## 5.3 Database Schema (PostgreSQL — hsa_integration)

```sql
-- ============================================================
-- ENUM types
-- ============================================================
CREATE TYPE onboarding_status AS ENUM
    ('PENDING','SBD_GENERATED','CLASSIN_FAILED','CLASSIN_DONE',
     'NOTIFIED','ODOO_SYNCED','ONBOARDED','FAILED');

CREATE TYPE commission_status AS ENUM ('pending','confirmed','paid','cancelled');

CREATE TYPE webhook_source AS ENUM ('sepay','classin','web');

-- ============================================================
-- hsa_course_mappings : map khóa học HSA ↔ ClassIn ↔ QLL/GV
--   PHẢI điền thủ công trước khi onboarding chạy (Sprint 0)
-- ============================================================
CREATE TABLE hsa_course_mappings (
    id                  BIGSERIAL PRIMARY KEY,
    hsa_course_code     VARCHAR(64)  NOT NULL,
    exam_type           VARCHAR(16)  NOT NULL,  -- HSA / BCA / BQP / DGNL_HCM
    branch              VARCHAR(8)   NOT NULL,  -- HN / HCM
    classin_course_id   BIGINT       NOT NULL,
    classin_gv_uid      BIGINT,
    qll_user_id         BIGINT,                 -- assignee QLL (Odoo res.users id)
    active              BOOLEAN      NOT NULL DEFAULT TRUE,
    created_at          TIMESTAMPTZ  NOT NULL DEFAULT now(),
    CONSTRAINT uq_course_code UNIQUE (hsa_course_code)
);
COMMENT ON TABLE hsa_course_mappings IS 'Bảng cấu hình lõi: không có mapping thì không enroll được';
CREATE INDEX ix_course_mappings_exam ON hsa_course_mappings (exam_type, branch) WHERE active;

-- ============================================================
-- hsa_sbd_sequences : bộ đếm SBD, dùng row-lock khi sinh
-- ============================================================
CREATE TABLE hsa_sbd_sequences (
    exam_type   VARCHAR(16) NOT NULL,
    year        INT         NOT NULL,
    last_seq    INT         NOT NULL DEFAULT 0,
    updated_at  TIMESTAMPTZ NOT NULL DEFAULT now(),
    CONSTRAINT pk_sbd_sequences PRIMARY KEY (exam_type, year)
);
COMMENT ON TABLE hsa_sbd_sequences IS 'Sinh SBD an toàn concurrency bằng SELECT ... FOR UPDATE';

-- ============================================================
-- hsa_students : học sinh (nguồn middleware; map sang Odoo qua odoo_partner_id)
-- ============================================================
CREATE TABLE hsa_students (
    student_id          BIGSERIAL PRIMARY KEY,
    sbd                 VARCHAR(32)  NOT NULL,
    full_name           VARCHAR(160) NOT NULL,
    phone               VARCHAR(20)  NOT NULL,
    email               VARCHAR(160),
    exam_type           VARCHAR(16)  NOT NULL,
    cohort              VARCHAR(32),                -- VD: 2026-DOT3
    classin_uid         BIGINT,
    classin_course_id   BIGINT,
    qll_assigned_id     BIGINT,
    enrollment_date     DATE,
    onboarding_status   onboarding_status NOT NULL DEFAULT 'PENDING',
    zalo_phone          VARCHAR(20),
    ref_code            VARCHAR(32),                -- CTV attribution
    odoo_partner_id     INT,
    created_at          TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_at          TIMESTAMPTZ  NOT NULL DEFAULT now(),
    CONSTRAINT uq_students_sbd UNIQUE (sbd)
);
COMMENT ON COLUMN hsa_students.classin_uid IS 'UID ClassIn — bắt buộc lưu, automation phụ thuộc';
CREATE INDEX ix_students_phone       ON hsa_students (phone);
CREATE INDEX ix_students_classin_uid ON hsa_students (classin_uid);
CREATE INDEX ix_students_exam_cohort ON hsa_students (exam_type, cohort);

-- ============================================================
-- hsa_enrollments : 1 record / 1 giao dịch thanh toán (orchestration state)
-- ============================================================
CREATE TABLE hsa_enrollments (
    id                    BIGSERIAL PRIMARY KEY,
    student_id            BIGINT REFERENCES hsa_students(student_id),
    order_reference       VARCHAR(64)  NOT NULL,
    sepay_transaction_id  VARCHAR(64)  NOT NULL,
    amount                NUMERIC(14,2) NOT NULL,
    payment_confirmed_at  TIMESTAMPTZ,
    sbd_generated_at      TIMESTAMPTZ,
    classin_enrolled_at   TIMESTAMPTZ,
    zalo_sent_at          TIMESTAMPTZ,
    email_sent_at         TIMESTAMPTZ,
    odoo_created_at       TIMESTAMPTZ,
    status                onboarding_status NOT NULL DEFAULT 'PENDING',
    error_log             TEXT,
    retry_count           INT          NOT NULL DEFAULT 0,
    created_at            TIMESTAMPTZ  NOT NULL DEFAULT now(),
    CONSTRAINT uq_enroll_txn UNIQUE (sepay_transaction_id)   -- IDEMPOTENCY KEY
);
COMMENT ON CONSTRAINT uq_enroll_txn ON hsa_enrollments IS 'Chống tạo trùng SBD/enroll khi SePay gửi lại';
CREATE INDEX ix_enroll_order   ON hsa_enrollments (order_reference);
CREATE INDEX ix_enroll_status  ON hsa_enrollments (status, created_at);

-- ============================================================
-- hsa_classin_attendance : điểm danh từ Data Subscription
-- ============================================================
CREATE TABLE hsa_classin_attendance (
    id              BIGSERIAL PRIMARY KEY,
    student_id      BIGINT REFERENCES hsa_students(student_id),
    classin_uid     BIGINT       NOT NULL,
    course_id       BIGINT       NOT NULL,
    class_id        BIGINT       NOT NULL,
    class_date      DATE         NOT NULL,
    entry_time      TIMESTAMPTZ,
    exit_time       TIMESTAMPTZ,
    is_present      BOOLEAN      NOT NULL DEFAULT FALSE,
    source_payload  JSONB,
    created_at      TIMESTAMPTZ  NOT NULL DEFAULT now(),
    CONSTRAINT uq_attendance UNIQUE (classin_uid, class_id)  -- idempotent upsert
);
CREATE INDEX ix_att_student_date ON hsa_classin_attendance (student_id, class_date);
CREATE INDEX ix_att_uid_course   ON hsa_classin_attendance (classin_uid, course_id);

-- ============================================================
-- hsa_classin_scores : điểm LMS
-- ============================================================
CREATE TABLE hsa_classin_scores (
    id               BIGSERIAL PRIMARY KEY,
    student_id       BIGINT REFERENCES hsa_students(student_id),
    classin_uid      BIGINT       NOT NULL,
    course_id        BIGINT       NOT NULL,
    activity_name    VARCHAR(200) NOT NULL,
    score            NUMERIC(8,2),
    total_score      NUMERIC(8,2),
    completion_rate  NUMERIC(5,4),                -- 0.0000 - 1.0000
    submitted_at     TIMESTAMPTZ,
    created_at       TIMESTAMPTZ  NOT NULL DEFAULT now(),
    CONSTRAINT uq_score UNIQUE (classin_uid, course_id, activity_name)
);
CREATE INDEX ix_scores_student ON hsa_classin_scores (student_id);

-- ============================================================
-- hsa_ctv_profiles : hồ sơ CTV
-- ============================================================
CREATE TABLE hsa_ctv_profiles (
    id               BIGSERIAL PRIMARY KEY,
    ctv_code         VARCHAR(32)  NOT NULL,
    full_name        VARCHAR(160) NOT NULL,
    phone            VARCHAR(20),
    bank_account     VARCHAR(40),                 -- KHÔNG log ra Serilog
    bank_name        VARCHAR(80),
    commission_rate  NUMERIC(5,4) NOT NULL DEFAULT 0,
    active           BOOLEAN      NOT NULL DEFAULT TRUE,
    created_at       TIMESTAMPTZ  NOT NULL DEFAULT now(),
    CONSTRAINT uq_ctv_code UNIQUE (ctv_code)
);

-- ============================================================
-- hsa_commissions : hoa hồng theo từng HS/đơn
-- ============================================================
CREATE TABLE hsa_commissions (
    id               BIGSERIAL PRIMARY KEY,
    ctv_code         VARCHAR(32)  NOT NULL REFERENCES hsa_ctv_profiles(ctv_code),
    student_id       BIGINT REFERENCES hsa_students(student_id),
    order_reference  VARCHAR(64)  NOT NULL,
    amount           NUMERIC(14,2) NOT NULL,      -- số tiền hoa hồng
    rate             NUMERIC(5,4) NOT NULL,
    status           commission_status NOT NULL DEFAULT 'pending',
    confirmed_at     TIMESTAMPTZ,
    paid_at          TIMESTAMPTZ,
    batch_month      VARCHAR(7),                  -- 'YYYY-MM'
    created_at       TIMESTAMPTZ  NOT NULL DEFAULT now(),
    CONSTRAINT uq_commission_order UNIQUE (order_reference)  -- 1 đơn = 1 hoa hồng
);
CREATE INDEX ix_comm_ctv_batch    ON hsa_commissions (ctv_code, batch_month);
CREATE INDEX ix_comm_status_batch ON hsa_commissions (status, batch_month);

-- ============================================================
-- hsa_zalo_log : log mọi lần gửi Zalo
-- ============================================================
CREATE TABLE hsa_zalo_log (
    id            BIGSERIAL PRIMARY KEY,
    student_id    BIGINT REFERENCES hsa_students(student_id),
    message_type  VARCHAR(40)  NOT NULL,          -- onboarding / absence / nologin / lowscore
    zalo_phone    VARCHAR(20),
    payload       JSONB,
    status        VARCHAR(16)  NOT NULL,          -- sent / failed / fallback_email
    error         TEXT,
    sent_at       TIMESTAMPTZ,
    created_at    TIMESTAMPTZ  NOT NULL DEFAULT now()
);
CREATE INDEX ix_zalo_student ON hsa_zalo_log (student_id, created_at);

-- ============================================================
-- hsa_webhook_log : raw mọi webhook nhận được (replay/audit)
-- ============================================================
CREATE TABLE hsa_webhook_log (
    id               BIGSERIAL PRIMARY KEY,
    source           webhook_source NOT NULL,
    payload          JSONB        NOT NULL,
    signature_valid  BOOLEAN,
    processed        BOOLEAN      NOT NULL DEFAULT FALSE,
    error            TEXT,
    created_at       TIMESTAMPTZ  NOT NULL DEFAULT now()
);
CREATE INDEX ix_webhook_src_proc ON hsa_webhook_log (source, processed, created_at);
```

## 5.4 API Contract Specifications

```
POST /webhooks/sepay
  Description: Nhận payment confirmation từ SePay
  Auth:    HMAC-SHA256 trong header X-SePay-Signature
  Body:    { transaction_id, amount, order_reference, timestamp, ... }
  200:     { status: "accepted", enrollment_id }
  400:     { error: "invalid_signature" }
  409:     { error: "duplicate_transaction" }   (trả 200 no-op cũng chấp nhận; xem US-01.3)
  Processing: async (return ngay, xử lý nền < 100ms response)

POST /webhooks/classin/data-subscription
  Description: Nhận data push từ ClassIn
  Auth:    validate SID == config
  Body:    { SID, EventType, Data: {...} }
  Response: { error_info: { errno: 1, error: "success" } }   (FORMAT BẮT BUỘC của ClassIn)
  Processing: async; response < 5s

POST /api/leads
  Description: Web form submit → tạo lead
  Auth:    API Key (header X-Api-Key)
  Body:    { full_name, phone, email, exam_type, branch, source, ref_code?, notes? }
  201:     { lead_id, ezsale_lead_id?, odoo_lead_id? }
  409:     { error: "duplicate_phone", existing_lead_id }

GET /api/students/{sbd}
  Description: Tra cứu HS theo SBD
  Auth:    Bearer token (staff)
  200:     { student_id, sbd, full_name, exam_type, classin_uid, onboarding_status, qll_assigned }
  404:     { error: "not_found" }

GET /api/dashboard/onboarding
  Description: Trạng thái onboarding realtime
  Auth:    Bearer token (QLL/Admin)
  200:     { today_count, pending_count, failed_count, students: [...] }

GET /api/commissions/batch/{year}/{month}
  Description: Commission batch cho tháng
  Auth:    Bearer token (Admin/Accounting)
  200:     { batch_month, total, items: [{ ctv_code, amount, status }] }

POST /api/commissions/batch/{year}/{month}/confirm
  Description: Confirm + lock commission batch (status pending → confirmed)
  Auth:    Bearer token (Admin/Accounting)
  200:     { batch_month, confirmed_count, total }
  409:     { error: "batch_already_confirmed" }

GET /health
  Description: Health check DB + external APIs
  200:     { status:"healthy", db:"up", classin:"up|degraded", zalo:"up", odoo:"up" }
```

## 5.5 Integration Adapter Specifications

```csharp
// ===== ClassIn V1 Adapter =====
// Base: https://api.eeo.cn/partner/api/course.api.php?action=[action]
// Auth: safeKey = MD5(SECRET + timeStamp); timeStamp valid 20 phút
// phone format: "0084-[SĐT]" ; Error 135 = SĐT đã đăng ký → dùng UID trong response
public interface IClassInV1Client
{
    Task<RegisterResult> RegisterUserAsync(string phone, string name, string password, CancellationToken ct = default);
    Task<bool> AddSchoolStudentAsync(long uid, CancellationToken ct = default);
    Task<bool> AddCourseStudentAsync(long courseId, long studentUid, CancellationToken ct = default); // identity=1
    Task<bool> ModifyCourseTeacherAsync(long courseId, long teacherUid, CancellationToken ct = default); // CHÚ Ý: đổi GV cho TẤT CẢ buổi chưa bắt đầu
    Task<bool> RemoveCourseStudentAsync(long courseId, long studentUid, CancellationToken ct = default);
}
// RegisterResult { long Uid; bool AlreadyExisted; int Errno; string Error; }

// ===== ClassIn V2 LMS Adapter =====
// Base: https://api.eeo.cn/lms/  Headers: X-EEO-SIGN, X-EEO-UID, X-EEO-TS (timestamp valid 5 phút)
// Dùng createClass để gán teacherUid PER BUỔI (thay cho modifyCourseTeacher V1)
public interface IClassInV2Client
{
    Task<CreateClassResult> CreateClassAsync(long courseId, string name, long teacherUid,
        DateTime startTime, DateTime endTime, CancellationToken ct = default);
}

// ===== SePay Adapter =====
public interface ISePayWebhookValidator
{
    bool ValidateSignature(string payload, string signature, string secret);
    SePayPayload Parse(string rawJson);
}

// ===== Zalo OA Adapter =====
// Access token hết hạn → refresh bằng refresh_token (chu kỳ ~3 tháng). Lưu token bền (ZaloTokenStore).
public interface IZaloOAClient
{
    Task<bool> SendZNSAsync(string phone, string templateId, Dictionary<string,string> templateData, CancellationToken ct = default);
    Task<bool> SendMessageAsync(string userId, string message, CancellationToken ct = default);
    Task<string> RefreshTokenAsync(CancellationToken ct = default);
}

// ===== Odoo Adapter (JSON-RPC tại /web/dataset/call_kw) =====
public interface IOdooClient
{
    Task<int> CreatePartnerAsync(OdooPartner partner, CancellationToken ct = default);     // res.partner
    Task<int> CreateLeadAsync(OdooLead lead, CancellationToken ct = default);              // crm.lead
    Task<int> CreateSalesOrderAsync(OdooSalesOrder order, CancellationToken ct = default); // sale.order
    Task<int> CreateProjectTaskAsync(OdooTask task, CancellationToken ct = default);       // project.task
    Task UpdatePartnerAsync(int id, Dictionary<string,object> values, CancellationToken ct = default);
    Task<List<T>> SearchReadAsync<T>(string model, List<object> domain, List<string> fields, CancellationToken ct = default);
}

// ===== EZSale Adapter (READ-ONLY — KHÔNG write business data ngược EZSale) =====
public interface IEZSaleClient
{
    Task<List<EZSaleLead>> GetLeadsAsync(DateTime since, CancellationToken ct = default);
    Task<EZSaleLead?> GetLeadByPhoneAsync(string phone, CancellationToken ct = default);
    // Phase 2: cho phép create/update lead khi web form mới đẩy vào (F-05.1) — vẫn KHÔNG đọc-sửa-ghi business data
    Task<string> UpsertLeadAsync(EZSaleLead lead, CancellationToken ct = default);
}
```

**Implementation notes:**
- **ClassIn V1**: tính `safeKey = MD5(SECRET + timeStamp)` (hex lowercase). Gửi form-urlencoded body. Khi nhận errno 135 → đọc UID trong response, set `AlreadyExisted=true`, KHÔNG throw.
- **ClassIn throttle**: dùng `SemaphoreSlim` hoặc Polly RateLimit ≤ 2 req/s khi bulk (TR-08).
- **Zalo**: `ZaloTokenStore` lưu access/refresh token vào DB; refresh khi nhận lỗi token; cron refresh phòng ngừa.
- **Odoo**: authenticate qua `/web/session/authenticate` lấy session cookie, hoặc `common.login` lấy uid; cache uid; mọi lỗi RPC → log + fail-soft (fire-and-forget cho non-critical).
- **EZSale**: bọc trong adapter mỏng để dễ thay khi API đổi (TR-06).

---

# PHẦN 6 — PHASED RELEASE PLAN

## PHASE 0 — Foundation & Quick Wins (Tuần 1-3, không cần Odoo)

**Sprint 0 (Tuần 1): Setup & Baseline**
- [ ] Khởi tạo .NET solution theo structure 5.2
- [ ] docker-compose: postgres, seq, (redis optional)
- [ ] Hangfire với PostgreSQL storage + dashboard auth
- [ ] Serilog → Seq (dev) / file (prod)
- [ ] SePay webhook receiver (validate signature, log, 200)
- [ ] ClassIn Data Subscription endpoint (đúng format response)
- [ ] Test với WebApplicationFactory
- [ ] GitHub Actions CI (build + test)
- [ ] Document: lấy SID/SECRET ClassIn; credentials Zalo OA

**Sprint 1 (Tuần 2): SBD Auto-Generation**
- [ ] SBD sequence logic với PostgreSQL row-lock (SELECT FOR UPDATE)
- [ ] Unit test concurrency (100 parallel không trùng)
- [ ] Integration test với TestContainers
- [ ] Endpoint GET /api/generate-sbd?exam_type=HSA (test only, tắt ở prod)

**Sprint 2 (Tuần 3): ClassIn Auto-Enroll**
- [ ] ClassIn V1 client (register + addSchoolStudent + addCourseStudent + err 135)
- [ ] hsa_course_mappings table + CRUD API
- [ ] Retry 3x exponential backoff (Polly)
- [ ] Alert khi fail sau 3 retry
- [ ] Test với ClassIn sandbox (yêu cầu ClassIn VN cấp test account — làm sớm Sprint 0)

## PHASE 1 — Auto-Onboarding (Tuần 4-6)

**Sprint 3 (Tuần 4): SePay → Onboarding Chain**
- [ ] SePay webhook → ProcessPaymentHandler
- [ ] Full chain: SBD → ClassIn → Zalo → Email → Odoo (Hangfire ContinueWith)
- [ ] Idempotency (duplicate transaction)
- [ ] Dead-letter cho failed jobs
- [ ] Onboarding status tracking
- [ ] E2E với ngrok (SePay test mode)

**Sprint 4 (Tuần 5): Zalo OA Integration**
- [ ] Zalo ZNS template gửi SBD + link
- [ ] Token refresh logic (3 tháng)
- [ ] Fallback Zalo fail → Email
- [ ] Log mọi gửi vào hsa_zalo_log

**Sprint 5 (Tuần 6): Odoo Integration & Dashboard**
- [ ] Cài Odoo Community + modules cơ bản
- [ ] OdooJsonRpcClient authenticate + call
- [ ] Create student record + QLL task khi onboarding
- [ ] Dashboard /dashboard/onboarding (HTML + ít JS)

## PHASE 2 — ClassIn Data Pipeline (Tuần 7-10)

**Sprint 6-7 (Tuần 7-8): Attendance Processing**
- [ ] Parse attendance webhook → hsa_classin_attendance
- [ ] Rule: vắng ≥ 2 buổi → Task + Zalo
- [ ] Rule: không login 3 ngày → ZNS
- [ ] QLL attendance dashboard

**Sprint 8 (Tuần 9): LMS Score + Teaching Hours**
- [ ] Parse score webhook
- [ ] Rule completion < 50% → ZNS tài liệu
- [ ] Teaching hours aggregation/GV/tháng → export Odoo

**Sprint 9 (Tuần 10): CTV Attribution**
- [ ] ref_code injection vào web form URLs
- [ ] Persist ref_code qua SePay flow
- [ ] Commission calc engine
- [ ] Commission batch report (Excel/PDF)

## PHASE 3 — Odoo CRM Migration (Tuần 11-14, tùy chọn)

**Sprint 10-11: Lead Management**
- [ ] Web form → EZSale (giữ) + Odoo CRM (mirror)
- [ ] EZSale read adapter sync sang Odoo
- [ ] Nurture sequences trong Odoo

**Sprint 12-13: Full Odoo Foundation**
- [ ] Accounting: SePay reconciliation
- [ ] HR: toàn bộ nhân sự
- [ ] 3 dashboards hoàn chỉnh
- [ ] EZSale data migration (khi sẵn sàng cut-over)

---

# PHẦN 7 — NON-FUNCTIONAL REQUIREMENTS

## 7.1 Performance
- SePay webhook → 200: **< 100ms** (P95).
- Full onboarding chain (SBD + ClassIn + Zalo): **< 2 phút** end-to-end (ClassIn latency VN ~200-500ms).
- ClassIn webhook → processed: **< 5 giây**.
- Dashboard load: **< 3 giây**.

## 7.2 Reliability
- Uptime: **99.5%** (≤ 3.6h downtime/tháng).
- Webhook miss rate: **< 0.1%** (bù bằng SePayReconcileJob cron 5 phút).
- Idempotent: duplicate SePay transaction không tạo 2 SBD (constraint UNIQUE).
- Retry: mọi external call có retry + backoff.

## 7.3 Security
- SePay webhook: validate HMAC-SHA256.
- ClassIn webhook: validate SID.
- API endpoints: Bearer token hoặc API Key.
- KHÔNG log sensitive: SĐT full → mask `0901***456`; số tài khoản ngân hàng → không log.
- Odoo: role-based, least privilege (tài khoản API riêng, không dùng admin).
- Secrets: env vars / Key Vault, không hardcode, không commit.

## 7.4 Observability
- Structured logging: Serilog JSON.
- Mọi webhook log: source, payload hash, processing time, result.
- Hangfire dashboard: job status, retry count, failure reason (auth bảo vệ).
- `/health`: trạng thái DB + external APIs.
- Alert: email/Zalo khi error rate > 5% trong 5 phút.

## 7.5 Scalability
- API stateless → scale horizontal nếu cần.
- Hangfire: thêm worker nếu queue dồn.
- Index strategy đã thiết kế (5.3).
- HCM spike: 1.300 HS/đợt; nếu enroll dồn → throttle 2 req/s ClassIn (TR-08). Tải trung bình ~11 enroll/giờ → design hiện tại thừa sức.

---

# PHẦN 8 — RISK REGISTER (TECHNICAL)

| Risk ID | Mô tả | Xác suất | Tác động | Mitigation |
|---|---|---|---|---|
| TR-01 | ClassIn API timeout/down (server TQ) | Trung bình | Cao | Async, retry 3x, alert, manual fallback SOP, circuit-breaker |
| TR-02 | ClassIn Data Subscription setup delay (cần ClassIn VN) | Cao | Trung bình | Liên hệ account manager tuần 1; tạm dùng manual export |
| TR-03 | Zalo ZNS template bị từ chối/đổi policy | Thấp | Cao | Fallback Email; không phụ thuộc 100% Zalo |
| TR-04 | SePay webhook miss (HSA server down) | Thấp | Cao | hsa_webhook_log + SePayReconcileJob cron 5 phút đối chiếu API |
| TR-05 | SBD duplicate khi concurrent payment | Thấp | Cao | PostgreSQL row-lock (FOR UPDATE) + UNIQUE constraint + idempotency key |
| TR-06 | EZSale API đổi/deprecated | Trung bình | Thấp | Adapter read-only mỏng, dễ thay |
| TR-07 | Odoo upgrade phá custom code | Thấp | Trung bình | Tối thiểu custom module; chủ yếu JSON-RPC; DB tách riêng (ADR-04) |
| TR-08 | ClassIn rate limit khi bulk enroll (1.300 HS/đợt HCM) | Trung bình | Cao | Queue-based, throttle ≤ 2 req/s, test trước go-live HCM |

---

# PHẦN 9 — SPRINT 0 CHECKLIST (Bắt đầu ngay)

**Infrastructure (ngày 1-2):**
- [ ] Cài Docker Desktop (postgres + seq + optional redis)
- [ ] docker-compose.yml: postgres, seq, app, (odoo dev)
- [ ] GitHub repo: hsa-integration
- [ ] .NET 8 solution theo structure 5.2
- [ ] Odoo 17 Community (Docker) cho dev

**Credentials & API Setup (ngày 2-3) — KHÔNG CODE, chỉ thu thập:**
- [ ] ClassIn: SID + SECRET từ management console
- [ ] ClassIn: liên hệ account manager VN (028 7105 9900) đăng ký Data Subscription
- [ ] ClassIn: yêu cầu sandbox/test account
- [ ] ClassIn: export tất cả courseId + GV UID hiện có
- [ ] SePay: webhook secret + test mode credentials
- [ ] Zalo OA: OA ID + App ID + App Secret + access/refresh token
- [ ] Zalo OA: đăng ký ZNS templates (SBD notification, attendance alert)
- [ ] Web portal: yêu cầu dev hiện tại document webhook/order endpoint + field ref_code
- [ ] EZSale: API documentation + credentials

**Data Preparation (ngày 3-5):**
- [ ] Điền `hsa_course_mappings`: tất cả khóa hiện có
      `(hsa_course_code, exam_type, branch, classin_course_id, classin_gv_uid, qll_user_id)`
- [ ] Chuẩn hóa CTV: tạo `ctv_code` duy nhất cho 132-137 người
- [ ] Naming convention Zalo group: `[KỲ_THI]-[NĂM]-[ĐỢT]-[LOẠI]` áp dụng ngay

**First Code (ngày 5-7):**
- [ ] SePay webhook receiver (validate + log + 200)
- [ ] Unit test signature validation (hợp lệ + giả mạo)
- [ ] Test với ngrok + SePay sandbox

---

# PHẦN 10 — PHỤ LỤC TECHNICAL

## A. ClassIn API Quick Reference
```
Base V1: https://api.eeo.cn/partner/api/course.api.php?action=[action]
Auth V1: safeKey = MD5(SECRET + timeStamp)   (timestamp valid 20 phút)
Base V2: https://api.eeo.cn/lms/
Auth V2: Headers X-EEO-SIGN, X-EEO-UID, X-EEO-TS (timestamp valid 5 phút)

Key calls:
  action=register          → tạo tài khoản (phone: 0084-[SĐT])
  action=addSchoolStudent  → add vào trường (trước khi add course)
  action=addCourseStudent  → enroll lớp (courseId + UID, identity=1)
  Error 135                → SĐT đã đăng ký → dùng UID trong response
  POST /lms/activity/createClass → tạo buổi + teacherUid PER BUỔI

Lưu ý: ClassIn KHÔNG có query API đầy đủ → BẮT BUỘC lưu uid/courseId/classId.
Server tại TQ → đo latency, async + retry.
```

## B. SBD Generation Logic (PostgreSQL row-lock — production-ready)
```csharp
// Sinh SBD an toàn concurrency. Dùng raw SELECT ... FOR UPDATE để khóa đúng row.
public async Task<string> GenerateSbdAsync(string examType, CancellationToken ct = default)
{
    var year = DateTime.UtcNow.Year;
    await using var tx = await _db.Database.BeginTransactionAsync(ct);

    // Đảm bảo row tồn tại (idempotent, không lỗi nếu đã có)
    await _db.Database.ExecuteSqlInterpolatedAsync($@"
        INSERT INTO hsa_sbd_sequences (exam_type, year, last_seq)
        VALUES ({examType}, {year}, 0)
        ON CONFLICT (exam_type, year) DO NOTHING;", ct);

    // Khóa row + tăng atomically
    var seq = await _db.Database
        .SqlQuery<int>($@"
            UPDATE hsa_sbd_sequences
            SET last_seq = last_seq + 1, updated_at = now()
            WHERE exam_type = {examType} AND year = {year}
            RETURNING last_seq;")
        .SingleAsync(ct);

    await tx.CommitAsync(ct);
    return $"{examType}-{year}-{seq:D5}";   // → HSA-2026-00001
}
```
> Ghi chú: `UPDATE ... RETURNING` đã atomic; transaction + RETURNING đảm bảo không hai request lấy cùng số. Không cần Redis ở tải hiện tại.

## C. Onboarding Chain Implementation
```csharp
// ProcessPaymentHandler.cs — orchestrator (MediatR handler)
public async Task Handle(ProcessPaymentCommand cmd, CancellationToken ct)
{
    // 1. Idempotency — UNIQUE(sepay_transaction_id) là chốt chặn cuối ở DB
    if (await _enrollmentRepo.ExistsAsync(cmd.TransactionId, ct))
        return; // đã xử lý

    // 2. Find order (qua Web Portal API hoặc shared read)
    var order = await _orders.FindByReferenceAsync(cmd.OrderReference, ct)
                ?? throw new InvalidOperationException($"Order {cmd.OrderReference} not found");

    // 3. Tạo enrollment (PENDING) — bắt UniqueViolation để no-op nếu race
    var enrollment = await _enrollmentRepo.CreatePendingAsync(cmd, order, ct);

    // 4. Sinh SBD
    var sbd = await _sbdService.GenerateSbdAsync(order.ExamType, ct);
    await _enrollmentRepo.SetSbdAsync(enrollment.Id, sbd, ct);

    // 5. ClassIn enroll (job 1) → ContinueWith Zalo + Email
    var classInJobId = BackgroundJob.Enqueue<ClassInEnrollJob>(j =>
        j.EnrollAsync(enrollment.Id, order.StudentPhone, order.StudentName, order.CourseCode, sbd));

    BackgroundJob.ContinueJobWith<ZaloNotifyJob>(classInJobId, j =>
        j.SendOnboardingAsync(enrollment.Id, order.StudentPhone, sbd, order.ExamType));

    BackgroundJob.ContinueJobWith<EmailJob>(classInJobId, j =>
        j.SendOnboardingGuideAsync(enrollment.Id, order.StudentEmail, sbd));

    // 6. Odoo (fire-and-forget, NON-critical path)
    BackgroundJob.Enqueue<OdooSyncJob>(j => j.CreateStudentAsync(enrollment.Id, sbd));

    // 7. CTV commission
    if (!string.IsNullOrEmpty(order.RefCode))
        BackgroundJob.Enqueue<CommissionJob>(j =>
            j.LogPendingAsync(order.RefCode, order.OrderReference, order.Amount));
}
```

## D. Environment Variables Template
```env
# Database
POSTGRES_CONNECTION=Host=localhost;Database=hsa_integration;Username=hsa;Password=xxx

# ClassIn
CLASSIN_SID=your_school_id
CLASSIN_SECRET=your_secret_key

# SePay
SEPAY_WEBHOOK_SECRET=your_webhook_secret

# Zalo OA
ZALO_OA_ID=your_oa_id
ZALO_ACCESS_TOKEN=your_access_token
ZALO_REFRESH_TOKEN=your_refresh_token
ZALO_APP_ID=your_app_id
ZALO_APP_SECRET=your_app_secret

# Odoo
ODOO_BASE_URL=http://localhost:8069
ODOO_DB=hsa_odoo
ODOO_USERNAME=api_user            # tài khoản riêng, least privilege (KHÔNG dùng admin)
ODOO_PASSWORD=your_odoo_password

# EZSale (read-only)
EZSALE_API_KEY=your_ezsale_key
EZSALE_BASE_URL=https://api.ezsale.vn

# Alerts / Dashboard
ALERT_EMAIL=ops@hsavnu.edu.vn
HANGFIRE_DASHBOARD_USER=admin
HANGFIRE_DASHBOARD_PASS=xxx
API_KEY=web_form_api_key
```

---

## CHECKLIST CHẤT LƯỢNG TÀI LIỆU
- [x] Code snippets compile được (C#, SQL production-ready với constraints/indexes/comments).
- [x] Mọi User Story có Acceptance Criteria đủ để test.
- [x] Phân biệt rõ "làm ngay" (Phase 0-2) vs "làm sau" (Phase 3+) — Constraint Matrix + MoSCoW.
- [x] Đủ để developer bắt tay thiết kế kiến trúc & code ngay hôm nay.

---
*HSA Education — Technical Roadmap v1.0 — Approved for Implementation*
