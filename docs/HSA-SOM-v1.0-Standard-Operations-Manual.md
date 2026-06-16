# HSA EDUCATION — STANDARD OPERATIONS MANUAL (SOM)

> Tài liệu vận hành chuẩn quốc tế cho chuỗi luyện thi đánh giá năng lực quốc gia HSA Education.
> Tham chiếu khung: ISO 9001:2015 (Quality Management), ITIL 4 (Service Management), PMI/PMBOK (Project), BPMN 2.0 (Process Modeling).

---

## DOCUMENT CONTROL

| Trường | Giá trị |
|---|---|
| **Tên tài liệu** | HSA Education — Standard Operations Manual (SOM) |
| **Mã tài liệu** | HSA-SOM-v1.0 |
| **Phiên bản** | 1.0 |
| **Trạng thái** | APPROVED |
| **Ngày ban hành** | 2026-06-16 |
| **Ngày hiệu lực** | 2026-07-01 |
| **Chủ sở hữu tài liệu (Owner)** | Operations Director (Giám đốc Vận hành Tập đoàn) |
| **Người phê duyệt (Approver)** | Hội đồng Quản trị (Thầy Hoa, Thầy Khương) |
| **Chu kỳ rà soát (Review Cycle)** | Hàng quý (Q) — bắt buộc rà soát toàn diện mỗi 6 tháng |
| **Phân loại bảo mật** | INTERNAL — CONFIDENTIAL |
| **Phạm vi phân phối** | Toàn bộ cấp Lead trở lên + tất cả nhân sự fulltime |
| **Ngôn ngữ** | Tiếng Việt |

### Change Log

| Phiên bản | Ngày | Tác giả | Mô tả thay đổi | Phê duyệt |
|---|---|---|---|---|
| 0.1 | 2026-05-20 | Operations Director | Bản nháp khung tài liệu, thu thập AS-IS 9 luồng vận hành | — |
| 0.5 | 2026-06-02 | Operations Director + Product Owner | Hoàn thiện 7 Value Streams, 11 SOPs, khung SLA/KPI | — |
| 0.9 | 2026-06-10 | Operations Director | Bổ sung automation design, data governance, risk register | HĐQT review |
| **1.0** | **2026-06-16** | **Operations Director + Principal Product Owner** | **Phát hành chính thức APPROVED. Đầy đủ 12 phần + phụ lục.** | **HĐQT** |

### Quy ước phê duyệt thay đổi (Change Control)
- Thay đổi **minor** (sửa lỗi, làm rõ câu chữ, cập nhật số liệu): Operations Director duyệt, tăng số sau dấu chấm (1.0 → 1.1).
- Thay đổi **major** (thêm/bỏ SOP, đổi cấu trúc tổ chức, đổi SLA/KPI cốt lõi): HĐQT duyệt, tăng số trước dấu chấm (1.x → 2.0).
- Mọi thay đổi phải ghi vào Change Log và thông báo qua kênh nội bộ chính thức trong vòng 48h.

---

## MỤC LỤC

- **PHẦN I** — Tổng quan và Nguyên tắc Vận hành
- **PHẦN II** — Thiết kế Tổ chức và RACI
- **PHẦN III** — Kiến trúc Quy trình (7 Value Streams)
- **PHẦN IV** — SOP Chi tiết (11 SOPs)
- **PHẦN V** — Khung SLA (Service Level Agreement)
- **PHẦN VI** — Thiết kế Tự động hóa
- **PHẦN VII** — Quản trị Dữ liệu
- **PHẦN VIII** — Khung KPI và Đo lường
- **PHẦN IX** — Quản lý Rủi ro và SPOF Elimination
- **PHẦN X** — Kiến trúc Công nghệ
- **PHẦN XI** — Lộ trình Triển khai
- **PHẦN XII** — Phụ lục

---

# PHẦN I — TỔNG QUAN VÀ NGUYÊN TẮC VẬN HÀNH

## 1.1. Executive Summary

HSA Education là chuỗi luyện thi đánh giá năng lực quốc gia hàng đầu Việt Nam, phục vụ **~20.000 học sinh/năm** qua **4 sản phẩm thi** (ĐGNL HSA, BCA — Bộ Công an, BQP — Bộ Quốc phòng, ĐGNL HCM) tại **2 cơ sở** (Hà Nội và TP. Hồ Chí Minh). Quy mô nhân lực vượt **300 người**, trong đó **62 nhân sự fulltime** (HN 50, HCM 12), **~70 giảng viên online**, và mạng lưới **132–137 cộng tác viên (CTV)/Sale**.

Tại thời điểm Q2/2026, vận hành dựa trên **9 luồng thủ công** với mức độ tự động hóa thấp. Hệ thống duy nhất hoạt động tự động ổn định là **webhook thanh toán SePay**. Phân tích hiện trạng đã xác định **14 nút thắt (bottlenecks)** tiêu tốn **~504 giờ công/tháng (~63 ngày công/tháng)** chỉ riêng cho onboarding, đối soát, tính thù lao giảng viên và hoa hồng CTV; đồng thời tồn tại **13 rủi ro trọng yếu**, nổi bật là các điểm lỗi đơn (Single Point of Failure — SPOF): 1 lập trình viên outsource gánh toàn hệ thống, 1 người "duyệt học sinh" thủ công, và dữ liệu lưu trong Google Drive cá nhân.

Tài liệu này thiết kế lại toàn bộ vận hành thành **7 Value Streams** chuẩn hóa, **11 SOP** thực thi được, một **khung SLA/KPI đo lường được**, và một **kiến trúc tự động hóa** lấy **Odoo làm system of record**, **n8n làm middleware**, tích hợp sâu **ClassIn (API V1/V2 + Data Subscription)** và **Zalo OA**. Mục tiêu trọng tâm: biến chuỗi onboarding từ **~15 phút/học sinh thủ công** thành **dưới 5 phút hoàn toàn tự động**, loại bỏ SPOF, và cung cấp **dashboard realtime** cho lãnh đạo theo trục kỳ thi × cơ sở.

**Cam kết kết quả mục tiêu (đến hết 2027):**
- Giảm tải nhân công bottleneck từ ~504h/tháng xuống **< 80h/tháng** (chỉ xử lý ngoại lệ).
- Time-to-SBD **< 2 phút** (target 99%); Time-to-ClassIn-Enroll **< 5 phút** (target 99%).
- Tỷ lệ đối soát SePay tự động **100%**; hoa hồng CTV sai sót **< 0,5%**.
- P&L theo kỳ thi × cơ sở **realtime**.
- Loại bỏ toàn bộ 5 nhóm SPOF trọng yếu.

## 1.2. Phạm vi áp dụng (Scope)

**Trong phạm vi (In-scope):**
- Toàn bộ quy trình vận hành end-to-end từ tạo lead → tư vấn → thanh toán → onboarding → học tập → chăm sóc → đối soát tài chính.
- Cả 2 cơ sở (HN, HCM) và cả 4 sản phẩm thi.
- Toàn bộ nhân sự fulltime, giảng viên online, và mạng lưới CTV/Đại sứ.
- Các hệ thống: hsavnu.edu.vn, SePay, EZSale (chuyển dần sang Odoo), Google Workspace, Zalo OA, ClassIn, Zoom (legacy), Odoo, n8n.

**Ngoài phạm vi (Out-of-scope):**
- Thiết kế nội dung học thuật/đề thi (thuộc Hội đồng Chuyên môn — tài liệu riêng).
- Chiến lược tài chính đầu tư cấp HĐQT.
- Quy trình tuyển dụng chi tiết của HCNS (tham chiếu, không định nghĩa lại).

## 1.3. 10 Nguyên tắc Vận hành Nền tảng (Operating Principles)

Mọi quyết định vận hành, thiết kế quy trình và lựa chọn công nghệ tại HSA Education phải tuân thủ 10 nguyên tắc sau. Khi có xung đột, ưu tiên theo thứ tự số.

**OP-1 — Học sinh là trung tâm (Student-First).**
Mọi quy trình được đo bằng trải nghiệm và kết quả học tập của học sinh. Thời gian từ thanh toán đến khi học sinh học được buổi đầu phải ngắn nhất có thể. Không một bước nội bộ nào được phép làm chậm trải nghiệm học sinh mà không có lý do bắt buộc.

**OP-2 — Tự động hóa phần lặp lại, con người xử lý ngoại lệ (Automate the Repetitive).**
Áp dụng quy tắc 80/20: tự động hóa 80% công việc lặp lại có quy luật; con người tập trung vào 20% ngoại lệ, phán đoán và quan hệ. Mọi tác vụ thủ công lặp lại > 20 lần/tuần phải có kế hoạch tự động hóa hoặc lý do chính đáng vì sao không.

**OP-3 — Một nguồn sự thật duy nhất (Single Source of Truth — SSOT).**
Mỗi loại dữ liệu chỉ có một hệ thống là nguồn chính thức (system of record). Odoo là SSOT cho CRM, đơn hàng, kế toán, nhân sự, task. ClassIn là SSOT cho điểm danh và điểm LMS. SePay là SSOT cho giao dịch thanh toán. Không lưu dữ liệu vận hành chính thức trong Drive cá nhân hay Zalo cá nhân.

**OP-4 — Không có điểm lỗi đơn (No Single Point of Failure).**
Không quy trình trọng yếu nào được phép phụ thuộc vào đúng một con người không thể thay thế. Mỗi vai trò trọng yếu phải có backup được đào tạo và SOP dạng checklist để người khác tiếp quản trong 24h.

**OP-5 — Đo được mới quản được (Measure to Manage).**
Mọi Value Stream phải có KPI với target cụ thể và phương pháp đo. Cấm dùng mục tiêu mơ hồ ("càng nhanh càng tốt"). Mọi cam kết dịch vụ phải là SLA đo được, có ngưỡng vi phạm và đường escalation.

**OP-6 — Mọi việc đều để lại dấu vết kiểm toán (Auditable by Default).**
Mọi tương tác với học sinh, mọi thay đổi dữ liệu cá nhân, mọi giao dịch tài chính phải để lại log truy vết được trong hệ thống (không phải trong đầu nhân viên hay tin nhắn cá nhân). Sự cố phải có ticket; tư vấn phải có lịch sử.

**OP-7 — Chuẩn hóa trước, tối ưu sau (Standardize then Optimize).**
Một quy trình phải được chuẩn hóa thành SOP và chạy ổn định trước khi tự động hóa. Tự động hóa một quy trình lộn xộn chỉ tạo ra hỗn loạn nhanh hơn.

**OP-8 — Thiết kế cho quy mô và spike (Design for Scale & Spike).**
Mọi quy trình phải chịu được tải cao điểm: spike khai giảng HCM ~260 học sinh/ngày. Quy trình không được phụ thuộc vào "làm thêm giờ thủ công" để vượt cao điểm.

**OP-9 — Phòng thủ dữ liệu và quyền riêng tư (Privacy & Security by Design).**
Dữ liệu cá nhân học sinh được phân quyền theo nguyên tắc least-privilege. Truy cập được cấp theo vai trò, thu hồi ngay khi nghỉ việc, và ghi log. Lấy cảm hứng từ GDPR về quyền của chủ thể dữ liệu.

**OP-10 — Cải tiến liên tục dựa trên dữ liệu (Continuous Improvement — Kaizen).**
Tài liệu này là bản sống. Mỗi quý rà soát KPI thực tế so với target, ghi nhận bài học, cập nhật SOP. Quyết định cải tiến phải dựa trên số liệu, không dựa trên cảm tính.

## 1.4. Cấu trúc tài liệu và quan hệ với các tài liệu khác

SOM này là tài liệu vận hành cấp cao nhất (Tier 1). Quan hệ phân cấp:

```
TIER 0 — Charter & Chiến lược (HĐQT)
   └── Chiến lược vận hành v3.0 (đã ban hành)
TIER 1 — Standard Operations Manual (TÀI LIỆU NÀY)  ◄── governs everything below
   ├── TIER 2 — SOPs chi tiết (11 SOP nhúng trong Phần IV)
   ├── TIER 2 — Khung SLA / KPI (Phần V, VIII)
   └── TIER 3 — Work Instructions & Templates (Phụ lục B–D, checklist tác nghiệp)
TIER 4 — Technical Specs (ClassIn API spec, Odoo config, n8n workflow — Phần X + Phụ lục A)
```

**Quan hệ:** SOM định nghĩa "phải làm gì và ai chịu trách nhiệm". SOP định nghĩa "làm như thế nào theo từng bước". Work Instruction định nghĩa "thao tác cụ thể trên hệ thống". Khi có mâu thuẫn, tài liệu cấp cao hơn (số Tier nhỏ hơn) thắng.

---

# PHẦN II — THIẾT KẾ TỔ CHỨC VÀ RACI

## 2.1. Sơ đồ Tổ chức (Organization Chart)

```
                          ┌─────────────────────────────┐
                          │   HỘI ĐỒNG QUẢN TRỊ (HĐQT)   │
                          │  Thầy Hoa  |  Thầy Khương     │
                          └──────────────┬──────────────┘
                                         │
                  ┌──────────────────────┼──────────────────────────┐
                  │                      │                          │
       ┌──────────▼─────────┐  ┌─────────▼──────────┐   ┌───────────▼───────────┐
       │ GĐ VẬN HÀNH BẮC    │  │  GĐ VẬN HÀNH NAM   │   │  KHỐI HỖ TRỢ TẬP ĐOÀN │
       │ (HSA, BCA, BQP)    │  │   (ĐGNL HCM)       │   │  - Kế toán (3)        │
       │ Cơ sở Hà Nội       │  │  Cơ sở HCM         │   │  - HCNS (1)           │
       └──────────┬─────────┘  └─────────┬──────────┘   │  - Tech Ops (TUYỂN)   │
                  │                      │              └───────────────────────┘
   ┌──────────────┼──────────────┐       ├─── Sale HCM (20–25)
   │              │              │       ├─── Marketing HCM (20)
   │              │              │       └─── Vận hành lớp HCM (10)
   ▼              ▼              ▼
┌─────────┐ ┌──────────┐ ┌───────────────┐
│ SALE HN │ │ HỌC VỤ HN│ │ TRUYỀN THÔNG  │
│ TP Sale │ │ - Duyệt  │ │ HN (4 team)   │
│ +11 off │ │   HS (1) │ │ mỗi kỳ thi:   │
│ +1 QL   │ │ - Lead   │ │ 1 lead +2 ct  │
│ +~100   │ │   Trợ    │ │ +1 edit       │
│  CTV    │ │   giảng  │ │ +1 design     │
└─────────┘ │   +CTV   │ │ = 20 người    │
            │ - QLL    │ └───────────────┘
            │   Lead   │
            │   +7 CTV │  ┌──── Tuyến đi trường (2)
            └──────────┘  └──── Đại sứ (8)
```

**Tổng kết biên chế (headcount):**

| Khối | HN | HCM | Tập đoàn | Ghi chú |
|---|---|---|---|---|
| Lãnh đạo | GĐ Vận hành Bắc | GĐ Vận hành Nam | HĐQT (2) | |
| Sale | 1 TP + 11 offline + 1 QL + ~100 CTV | 20–25 | | Mạng lưới Sale/CTV tổng 132–137 |
| Học vụ | 1 Duyệt HS, 1 Lead trợ giảng + 2–3 CTV/môn, 1 QLL Lead + 7 QLL CTV | 10 vận hành lớp | | |
| Truyền thông/Marketing | 4 team = 20 người | 20 người | | |
| Tài chính | | | Kế toán 3 (thu/chi/tổng hợp) | |
| Quan hệ trường | Tuyến đi trường 2, Đại sứ 8 | | | |
| Nhân sự | | | HCNS 1 | |
| Công nghệ | | | Tech Ops (CẦN TUYỂN — xem SPOF-01) | Đang outsource 1 dev |
| Giảng viên online | ~70 GV (dùng chung 2 cơ sở) | | | |

## 2.2. RACI Master Matrix

**Quy ước RACI:**
- **R (Responsible)** — Người trực tiếp thực hiện công việc.
- **A (Accountable)** — Người chịu trách nhiệm cuối cùng, người duyệt. Mỗi quy trình chỉ có **đúng một A**.
- **C (Consulted)** — Người được hỏi ý kiến trước khi thực hiện (giao tiếp hai chiều).
- **I (Informed)** — Người được thông báo kết quả (giao tiếp một chiều).

**Vai trò (cột):** HĐQT | GĐVH (GĐ Vận hành Bắc/Nam) | TP-Sale | Sale | CTV | DuyetHS (Học vụ) | QLL-Lead | QLL | GV | KT (Kế toán) | TT (Truyền thông) | TechOps | HCNS

| Quy trình chính | HĐQT | GĐVH | TP-Sale | Sale | CTV | DuyetHS | QLL-Lead | QLL | GV | KT | TT | TechOps | HCNS |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| VS1 Lead Acquisition & CRM | I | A | R | R | R | | | | | | C | C | |
| VS2 Nurture, Close & QA tư vấn | I | A | R | R | R | | | | | | | I | |
| VS3 Payment → Auto-Onboarding | I | A | | I | | R | C | I | | C | | R | |
| VS4 Learning Operations (ClassIn) | I | A | | | | | R | R | R | | | C | |
| VS5 Student Care & Retention | I | A | | | | | R | R | C | | | I | |
| VS6 Instructor & Content Ops | I | A | | | | | R | C | R | C | C | I | |
| VS7 Partner & Financial Ops | A | C | C | | C | | | | | R | | I | |
| Quản lý sự cố (Incident) | I | A | C | C | | C | C | C | C | C | C | R | |
| Onboarding nhân sự mới | I | A | C | | | | | | | | | C | R |
| Quản trị dữ liệu & phân quyền | A | R | | | | | | | | C | | R | C |
| Báo cáo & Dashboard | A | R | I | | | | I | I | | C | I | C | |
| Phê duyệt thay đổi SOM | A | R | I | | | | | | | I | | I | I |

## 2.3. Định nghĩa chi tiết từng Vai trò (Role Definition Cards)

### RDC-01 — Operations Director (Giám đốc Vận hành Tập đoàn)
- **Báo cáo cho:** HĐQT.
- **Mục tiêu chính:** Vận hành toàn chuỗi đạt SLA/KPI, loại bỏ SPOF, dẫn dắt tự động hóa.
- **Trách nhiệm:** Sở hữu tài liệu SOM; chủ trì rà soát KPI hàng quý; phê duyệt thay đổi minor; điều phối liên cơ sở HN–HCM.
- **Quyền hạn:** Duyệt SOP, phân bổ ngân sách vận hành theo hạn mức, dừng quy trình khi rủi ro cao.
- **KPI cá nhân:** Tổng tải bottleneck/tháng, % SLA đạt toàn chuỗi, tiến độ lộ trình triển khai.

### RDC-02 — Giám đốc Vận hành Bắc / Nam (GĐVH)
- **Báo cáo cho:** HĐQT (đường nét đứt: Operations Director điều phối).
- **Phạm vi:** GĐVH Bắc quản 3 kỳ thi (HSA, BCA, BQP) tại HN; GĐVH Nam quản ĐGNL HCM tại HCM.
- **Trách nhiệm:** Accountable cho toàn bộ Value Stream tại cơ sở; đạt KPI doanh thu/onboarding/retention theo cơ sở; quản lý spike khai giảng (HCM).
- **KPI cá nhân:** P&L cơ sở, conversion rate, onboarding error rate, attendance rate cơ sở.

### RDC-03 — Trưởng phòng Sale (TP-Sale)
- **Báo cáo cho:** GĐVH.
- **Trách nhiệm:** Quản đội Sale offline + CTV; phân bổ lead; giám sát SLA phản hồi lead Hot; vận hành playbook tư vấn và QA.
- **KPI:** Lead-to-Payment conversion, Lead Response Time < 15 phút (95%), doanh số đội.

### RDC-04 — Sale (Nhân viên kinh doanh)
- **Báo cáo cho:** TP-Sale.
- **Trách nhiệm:** Gọi/chốt lead Hot trong SLA; nhập liệu đúng; tuân thủ playbook; chăm Warm/Cold theo chuỗi nurture.
- **KPI:** Conversion cá nhân, response time, tuân thủ QA checklist.

### RDC-05 — CTV / Đại sứ (Cộng tác viên)
- **Báo cáo cho:** Quản lý CTV (thuộc Sale).
- **Trách nhiệm:** Giới thiệu học sinh qua **ref link cá nhân (?ref=CTVxxx)**; tư vấn theo playbook; tuân thủ chuẩn branding.
- **KPI:** Số học sinh attributed qua ref link, attribution rate, doanh số.

### RDC-06 — Chuyên viên Duyệt học sinh / Học vụ (DuyetHS)
- **Báo cáo cho:** GĐVH.
- **Trách nhiệm (TO-BE):** Sau tự động hóa, chuyển từ "tạo SBD + add nhóm thủ công" sang **xử lý ngoại lệ onboarding** (sai dữ liệu, thanh toán bất thường, mapping thiếu) và QA chất lượng dữ liệu học sinh.
- **KPI:** Onboarding error rate < 1%, thời gian xử lý exception.

### RDC-07 — QLL Lead (Trưởng nhóm Quản lý lớp)
- **Báo cáo cho:** GĐVH.
- **Trách nhiệm:** Lập lịch dạy; quản 7 QLL CTV; vận hành SOP-05/06; giám sát attendance dashboard; điều phối at-risk intervention.
- **KPI:** Attendance rate > 80%, at-risk catch rate trong 24h, tỷ lệ GV vào đúng giờ.

### RDC-08 — QLL (Quản lý lớp / Vận hành lớp)
- **Báo cáo cho:** QLL Lead.
- **Trách nhiệm:** Quản nhóm lớp được giao; xác nhận học sinh đăng nhập; gửi hướng dẫn; can thiệp học sinh vắng/at-risk; xử lý câu hỏi trong SLA.
- **KPI:** % học sinh đăng nhập trong 24h, SLA trả lời câu hỏi học sinh, NPS lớp.

### RDC-09 — Giảng viên (GV)
- **Báo cáo cho:** QLL Lead.
- **Trách nhiệm:** Dạy đúng lịch trên ClassIn; vào lớp đúng giờ; giao bài/chấm điểm LMS; báo cáo timesheet (tự động hóa dần).
- **KPI:** Tỷ lệ vào đúng giờ, NPS GV từ học sinh, assignment completion của lớp.

### RDC-10 — Kế toán (KT — thu / chi / tổng hợp)
- **Báo cáo cho:** HĐQT (đường điều phối Operations Director).
- **Trách nhiệm:** Đối soát SePay; xử lý hoàn tiền; duyệt batch thù lao GV và hoa hồng CTV; lập P&L theo kỳ thi × cơ sở.
- **KPI:** Auto-reconciliation rate 100%, payroll processing time < 30 phút, commission error rate < 0,5%.

### RDC-11 — Truyền thông / Marketing (TT)
- **Báo cáo cho:** GĐVH.
- **Trách nhiệm:** Chạy quảng cáo đa kênh; quản landing page; đảm bảo **auto-tag nguồn** và **ref link** hoạt động; tuân thủ brand guideline thống nhất 4 team.
- **KPI:** Lead volume by channel, Cost per Lead, brand consistency score.

### RDC-12 — Tech Ops (CẦN TUYỂN — xem SPOF-01)
- **Báo cáo cho:** Operations Director.
- **Trách nhiệm:** Quản trị Odoo/n8n/ClassIn integration; vận hành automation; xử lý sự cố P0/P1 kỹ thuật; document toàn bộ technical stack; quản outsource dev.
- **KPI:** Uptime automation, MTTR sự cố kỹ thuật, % technical stack được document.

### RDC-13 — HCNS (Hành chính Nhân sự)
- **Báo cáo cho:** HĐQT.
- **Trách nhiệm:** Onboarding nhân sự mới (SOP-11); cấp/thu hồi quyền truy cập; quản hồ sơ nhân sự; chấm công.
- **KPI:** Thời gian onboarding nhân sự mới, % thu hồi quyền đúng hạn khi nghỉ việc.

## 2.4. Ma trận Phân quyền (Access Rights Matrix)

Nguyên tắc: **Least Privilege** — chỉ cấp quyền tối thiểu cần để làm việc. Quyền cấp theo vai trò (role-based), thu hồi trong **24h** khi đổi vai trò/nghỉ việc (do HCNS + TechOps thực thi).

| Hệ thống / Dữ liệu | HĐQT | GĐVH | TP-Sale | Sale | CTV | DuyetHS | QLL-Lead | QLL | GV | KT | TT | TechOps | HCNS |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Odoo CRM (lead) | R | RW | RW | RW(own) | — | R | — | — | — | — | R | Admin | — |
| Odoo Sales (đơn) | R | RW | R | R(own) | — | R | — | — | — | RW | — | Admin | — |
| Odoo Accounting | R | R | — | — | — | — | — | — | — | RW | — | Admin | — |
| Odoo HR/Payroll | R | R | — | — | — | — | R(own team) | — | R(own) | RW | — | Admin | RW |
| Odoo Project (task QLL) | R | RW | — | — | — | RW | RW | RW(own) | — | — | — | Admin | — |
| Odoo Helpdesk (ticket) | R | RW | RW | R | — | RW | RW | RW | R | RW | — | Admin | — |
| ClassIn (admin) | — | R | — | — | — | — | RW | RW(own class) | RW(own class) | — | — | Admin | — |
| SePay dashboard | R | R | — | — | — | — | — | — | — | RW | — | Admin | — |
| Zalo OA (gửi thông báo) | — | R | — | — | — | — | RW | RW | — | — | RW | Admin | — |
| Google Workspace (Drive tổ chức) | RW | RW | RW | R(scoped) | — | RW | RW | R | R | RW | RW | Admin | RW |
| Dữ liệu cá nhân HS (PII) | R(masked) | R | R(scoped) | R(own lead) | — | RW | R | R(own class) | R(own class, masked) | R(billing) | — | Admin | — |
| n8n workflows | — | R | — | — | — | — | — | — | — | — | — | Admin | — |
| Dashboard COO/QLL/GV | RW | RW | R | — | — | — | RW(QLL) | R(QLL) | R(GV) | R | R | Admin | — |

> **Ghi chú:** RW = đọc+ghi; R = chỉ đọc; (own) = chỉ dữ liệu của mình; (masked) = PII bị che một phần; Admin = quản trị toàn quyền. CTV **không** được cấp tài khoản hệ thống nội bộ — chỉ làm việc qua ref link và cổng đối tác giới hạn.

---

# PHẦN III — KIẾN TRÚC QUY TRÌNH (7 VALUE STREAMS)

## 3.1. Value Chain Tổng thể

Chuỗi giá trị HSA Education biến nhu cầu thị trường thành kết quả học tập và doanh thu, được tái thiết kế từ 9 luồng AS-IS thành 7 Value Streams (VS) tinh gọn:

```
[THỊ TRƯỜNG] 
     │
     ▼
┌──────────┐   ┌──────────┐   ┌──────────────┐   ┌──────────────┐
│   VS1    │──▶│   VS2    │──▶│     VS3      │──▶│     VS4      │
│  Lead    │   │ Nurture  │   │  Payment →   │   │  Learning    │
│ Acq.&CRM │   │ Close&QA │   │ Auto-Onboard │   │ Ops(ClassIn) │
└──────────┘   └──────────┘   └──────┬───────┘   └──────┬───────┘
                                     │                  │
                                     ▼                  ▼
                              ┌──────────────┐   ┌──────────────┐
                              │     VS5      │   │     VS6      │
                              │ Student Care │   │ Instructor & │
                              │ & Retention  │   │ Content Ops  │
                              └──────────────┘   └──────────────┘
                                     │
                                     ▼
                              ┌──────────────────────────────┐
                              │            VS7               │
                              │ Partner (CTV) & Financial Ops│
                              │  (xuyên suốt — cross-cutting) │
                              └──────────────────────────────┘
```

**Bản đồ chuyển đổi 9 luồng AS-IS → 7 Value Stream TO-BE:**

| Luồng AS-IS | Value Stream TO-BE |
|---|---|
| Luồng 1 (Marketing & tạo lead) | VS1 |
| Luồng 2 (CRM, Sale, Nurture) | VS2 |
| Luồng 3 (Thanh toán) | VS3 |
| Luồng 4 (Onboarding) | VS3 (core) |
| Luồng 5 (Học tập) | VS4 |
| Luồng 6 (Chăm sóc học viên) | VS5 |
| Luồng 7 (Quản lý GV) | VS6 |
| Luồng 8 (Quản lý CTV & Đại sứ) | VS7 |
| Luồng 9 (Đối soát Kế toán) | VS7 |

**Ký hiệu BPMN dùng trong tài liệu:** `[ ]` activity/task | `< >` gateway/quyết định | `(( ))` event/trigger | `==>` luồng tự động (automation) | `-->` luồng thủ công (manual) | `[A]` actor.

---

## VS1 — Lead Acquisition & CRM

**Mục tiêu:** Thu hút lead chất lượng và đưa 100% lead vào Odoo CRM tự động, không sót/trùng, có gắn nguồn và CTV ref.

**Triggers:** ((Học sinh/phụ huynh submit form trên landing page)) | ((Click ref link CTV)).

**Actors:** [TT] Truyền thông, [Sale], [CTV], [TP-Sale], [TechOps] (vận hành automation).

**Tools:** FB/TikTok/Google Ads → Landing page (hsavnu.edu.vn) → n8n webhook → Odoo CRM.

**Flow (BPMN text):**
```
((Ad click)) ==> [Landing page hsavnu.edu.vn]
   --> [HS điền form: tên, SĐT, email, kỳ thi, cơ sở]
   == ?ref=CTVxxx được capture vào hidden field ==>
((Form submit)) ==> [n8n webhook nhận payload]
   ==> [Odoo CRM: auto-create Lead]
       ==> auto-tag: exam_type, source_channel, cơ sở
       ==> nếu có ref_code: gắn tag CTV_code
   ==> <Lead trùng SĐT/email?>
        ├─ Có ==> [merge vào lead cũ + ghi log] 
        └─ Không ==> [tạo lead mới]
   ==> [auto-assign Sales Team theo exam_type + cơ sở]
   ==> ((Lead sẵn sàng trong CRM — SLA < 5 phút))
```

**Outputs:** Lead record trong Odoo (đã tag nguồn, exam_type, cơ sở, CTV ref, đã phân công Sale).

**Automation level:** **Cao (90%)** — toàn bộ capture → CRM tự động. Con người chỉ xử lý lead lỗi format.

**Khắc phục bottleneck:** N5 (lead nhập tay), N8 (tracking CTV), R8 (sót/trùng/chậm).

---

## VS2 — Nurture, Close & QA Tư vấn

**Mục tiêu:** Chuyển lead thành học sinh trả phí với tư vấn nhất quán, có QA, có lịch sử audit.

**Triggers:** ((Lead mới được phân công)) | ((Lead chuyển stage → Hot)) | ((Lead Warm/Cold đến lịch nurture)).

**Actors:** [Sale], [CTV], [TP-Sale] (QA + escalation).

**Tools:** Odoo CRM (pipeline + activity), chuỗi nurture tự động (Odoo Marketing/n8n → Zalo OA), Playbook tư vấn (Phụ lục C).

**Flow (BPMN text):**
```
((Lead phân công)) --> <Phân loại nhiệt độ>
   ├─ HOT ==> [Odoo notify Sale: SLA timer 15 phút]
   │          --> [Sale gọi điện theo playbook]
   │          --> <Chốt?>
   │               ├─ Có --> [chuyển VS3 thanh toán]
   │               └─ Chưa --> [đặt activity follow-up + ghi note]
   ├─ WARM ==> [Auto-nurture sequence: Zalo OA day 1/3/7 nội dung giá trị]
   └─ COLD ==> [Auto-nurture dài hạn + tái kích hoạt theo mùa khai giảng]

[Case khó] --> [Sale tra Playbook taxonomy] 
   --> <Giải quyết được?>
        ├─ Có --> [xử lý + log vào CRM]
        └─ Không --> [escalate TP-Sale review + cập nhật playbook]

[QA] --> [TP-Sale sample-check note/ghi âm theo QA checklist hàng tuần]
```

**Outputs:** Lead chuyển trạng thái (Won/Lost), note tư vấn lưu CRM, playbook được cập nhật theo case mới.

**Automation level:** **Trung bình (50%)** — nurture tự động; tư vấn/chốt là con người; QA bán tự động.

**Khắc phục bottleneck:** N6 (nurture thủ công), N14 (giám sát tư vấn), R9 (lịch sử trong Zalo cá nhân), R13 (tư vấn thiếu QA). Taxonomy case bắt buộc: học phí, lịch học, chọn khóa, hoàn tiền, phụ huynh phản đối, so sánh đối thủ, kỹ thuật học (xem Phụ lục C).

---

## VS3 — Payment → Auto-Onboarding (CORE VALUE STREAM)

**Mục tiêu:** Từ khoảnh khắc thanh toán thành công đến khi học sinh có SBD, được enroll ClassIn, nhận đầy đủ thông tin qua Zalo OA — **hoàn toàn tự động dưới 5 phút**. Đây là chuỗi cốt lõi loại bỏ bottleneck lớn nhất (Luồng 4 AS-IS ~14h/ngày).

**Triggers:** ((SePay webhook: payment_success)).

**Actors:** [SePay], [Odoo], [n8n], [ClassIn], [Zalo OA], [DuyetHS] (chỉ xử lý exception), [QLL] (xác nhận đăng nhập).

**Tools:** SePay webhook → Odoo (SSOT) → n8n middleware → ClassIn API V1 → Zalo OA API.

**Flow (BPMN text):**
```
((SePay webhook payment_success))
 ==> [Odoo: ghi nhận đơn + map học sinh/khóa]
 ==> [B1: Auto-generate SBD = exam-năm-SEQ5  (vd HSA-2026-08421)]
 ==> [B2: ClassIn API V1]
       ==> action=register (tạo classin_uid)
       ==> addSchoolStudent
       ==> [lookup bảng mapping: hsa_course_code → classin_course_id → gv_uid → qll_user_id]
       ==> <Mapping tồn tại?>
            ├─ Có ==> addCourseStudent
            └─ Không ==> [HALT bước này + tạo Task exception cho DuyetHS] (xem fallback)
 ==> [B3: Zalo OA qua n8n] gửi: SBD + link ClassIn + lịch học + tên GV  (SLA < 2 phút)
 ==> [B4: Email] gửi guide đầy đủ + link cài ClassIn
 ==> [B5: Odoo Project Task cho QLL] stage="Chờ xác nhận HS đăng nhập"
 ==> [B6: <có ref_code?>] 
        └─ Có ==> [cộng commission_pending cho CTV]
 ==> [B7: Log toàn bộ chuỗi vào Odoo (audit trail)]
 ==> ((Học sinh onboarded — sẵn sàng học))

[QLL] --> [theo dõi Task: HS đăng nhập ClassIn trong 24h?]
        --> <Chưa?> ==> [Zalo OA nhắc + QLL liên hệ]
```

**Outputs:** Học sinh có SBD, được enroll vào lớp ClassIn đúng, nhận thông tin qua Zalo+Email, task QLL được tạo, commission CTV pending (nếu có), audit log đầy đủ.

**Automation level:** **Rất cao (95%)** — chỉ exception (mapping thiếu, dữ liệu lỗi) cần người.

**Khắc phục bottleneck:** N1 (SBD thủ công), N2 (Zalo OA thủ công), N3 (duyệt HS 1 người), N4 (add nhóm thủ công), N7 (ClassIn pipeline), N10 (đối soát SePay), R3 (SPOF duyệt HS), R7 (spike HCM).

---

## VS4 — Learning Operations (ClassIn)

**Mục tiêu:** Tổ chức lớp học live ổn định, đo lường được điểm danh và kết quả học tập theo thời gian thực.

**Triggers:** ((Mở khóa học mới)) | ((Lịch buổi học đến giờ)) | ((ClassIn Data Subscription push: attendance/score 20 phút sau buổi)).

**Actors:** [QLL-Lead], [QLL], [GV], [ClassIn], [TechOps].

**Tools:** ClassIn API V2 LMS (createClass với teacherUid per lesson), ClassIn Data Subscription (PUSH), Odoo Project, dashboard.

**Flow (BPMN text):**
```
((Mở khóa)) --> [QLL-Lead chuẩn bị bảng mapping khóa↔ClassIn↔GV↔QLL (SOP-05)]
   ==> [ClassIn API V2: createClass với teacherUid cho từng lesson]
   ==> [Sinh classin_course_id ghi vào bảng mapping Odoo]  ◄── prerequisite cho VS3
((Buổi học đến giờ)) ==> [Link lớp đã sẵn trong nhóm Zalo lớp + ClassIn]
   --> [GV dạy ClassIn (Zoom dự phòng)]
((Data Subscription push, ~20' sau buổi)) 
   ==> [Attendance sync về Odoo tự động]
   ==> [LMS scores sync realtime về Odoo]
   ==> [Login activity sync]
   ==> [Cập nhật Attendance Dashboard QLL]
```

**Outputs:** Lớp ClassIn được tạo đúng mapping; dữ liệu điểm danh/điểm/đăng nhập đồng bộ về Odoo; dashboard realtime.

**Automation level:** **Cao (80%)** sau khi bật Data Subscription — tạo lớp bán tự động, đồng bộ dữ liệu tự động.

**Khắc phục bottleneck:** N7 (ClassIn pipeline), điểm danh không đo lường (Luồng 5 AS-IS).

---

## VS5 — Student Care & Retention

**Mục tiêu:** Phát hiện sớm học sinh có nguy cơ bỏ học, can thiệp kịp thời, đo NPS định kỳ — tất cả có SLA và lịch sử audit.

**Triggers:** ((HS đặt câu hỏi)) | ((Data Subscription: vắng/điểm thấp/không login)) | ((Lịch khảo sát NPS)).

**Actors:** [QLL], [QLL-Lead], [GV] (consulted), [Odoo Helpdesk].

**Tools:** Odoo Helpdesk (ticket), Zalo OA (ZNS), dashboard at-risk, Data Subscription triggers.

**Flow (BPMN text):**
```
((HS hỏi trong nhóm Zalo lớp)) --> [QLL/GV/trợ giảng trả lời trong SLA 30' giờ học]
   --> [câu hỏi quan trọng/khiếu nại ==> tạo Helpdesk ticket có lịch sử]

((Vắng 1 buổi)) ==> [log + flag dashboard QLL]
((Vắng 2+ buổi liên tiếp)) ==> [Task QLL priority cao + Zalo OA nhắc HS]  (SLA liên hệ < 24h)
((Không login 3 ngày)) ==> [Zalo OA ZNS hỏi thăm tự động]
((completion_rate < 50%)) ==> [Zalo OA gợi ý tài liệu bổ trợ theo môn]
((điểm thấp 2 lần liên tiếp)) ==> [Alert QLL + Task "Tư vấn học tập HS [tên]"]

((Lịch NPS)) ==> [Khảo sát NPS định kỳ qua Zalo OA] --> [tổng hợp về dashboard]
```

**Outputs:** Ticket chăm sóc có lịch sử; at-risk student được catch trong 24h; NPS đo định kỳ; can thiệp học tập được ghi nhận.

**Automation level:** **Trung bình-cao (65%)** — phát hiện at-risk tự động, can thiệp con người.

**Khắc phục bottleneck:** N12 (sự cố qua Zalo mất lịch sử), R12 (sự cố mất khi đóng Zalo); thêm SLA + ticket + NPS (vốn không tồn tại ở Luồng 6 AS-IS).

---

## VS6 — Instructor & Content Operations

**Mục tiêu:** Lập lịch GV minh bạch, đo tỷ lệ vào đúng giờ, tự động tổng hợp giờ dạy thành payroll, thu thập NPS GV.

**Triggers:** ((Lập lịch dạy kỳ mới)) | ((Cuối tháng — chốt payroll)) | ((Data Subscription: GV login/attendance)).

**Actors:** [QLL-Lead], [GV], [KT], [TT] (content — consulted), [TechOps].

**Tools:** ClassIn timesheet, Odoo HR/Payroll, Odoo Project, dashboard GV.

**Flow (BPMN text):**
```
((Lập lịch)) --> [QLL-Lead tạo lịch dạy trong Odoo + map teacherUid per lesson]
   ==> [GV nhận + xác nhận qua Odoo activity/Zalo]
   --> [GV dạy ClassIn]
((Data Subscription)) ==> [đo GV vào đúng giờ (login time vs lesson start)]
((Cuối tháng)) 
   ==> [tổng hợp giờ dạy từ ClassIn timesheet × teaching_rate]
   ==> [draft payslip trong Odoo HR]
   ==> [KT review + duyệt batch]  (SLA: trước ngày 5 tháng sau)
((Định kỳ)) ==> [NPS GV từ HS qua khảo sát Zalo OA]
```

**Outputs:** Lịch dạy minh bạch; chỉ số đúng giờ; payslip draft tự động; NPS GV.

**Automation level:** **Cao (75%)** sau Data Subscription + Odoo HR — tổng hợp payroll tự động, duyệt là con người.

**Khắc phục bottleneck:** N9 (tổng hợp thù lao GV thủ công ~1 ngày/tháng); thêm đo đúng giờ + NPS GV (không có ở Luồng 7 AS-IS).

---

## VS7 — Partner (CTV/Ambassador) & Financial Operations

**Mục tiêu:** Attribution CTV chính xác qua ref link, tính hoa hồng tự động không tranh chấp, đối soát SePay tự động, P&L realtime theo kỳ thi × cơ sở.

**Triggers:** ((Đơn hàng có ref_code confirmed)) | ((SePay webhook)) | ((Cuối tháng — chốt commission)) | ((Lịch báo cáo P&L)).

**Actors:** [KT] (thu/chi/tổng hợp), [CTV], [HĐQT] (accountable cho tài chính), [TechOps].

**Tools:** ref link tracking (?ref=CTVxxx), Odoo Accounting, SePay, dashboard P&L.

**Flow (BPMN text):**
```
((HS thanh toán có ref_code)) ==> [Odoo gắn CTV vào order + commission_pending]
((SePay webhook)) ==> [auto-reconcile: match giao dịch ↔ đơn Odoo]
   ==> <Khớp?>
        ├─ Có ==> [đánh dấu reconciled]
        └─ Không ==> [tạo exception cho KT thu xử lý]

((Cuối tháng)) 
   ==> [gom confirmed orders có ref_code → commission batch theo commission_rate]
   ==> [KT review batch]  (SLA: trước ngày 7 tháng sau)
   ==> [chi hoa hồng vào bank_account CTV + ghi sổ]
((Realtime)) ==> [Dashboard P&L theo exam_type × cơ sở cập nhật liên tục]
```

**Outputs:** Attribution CTV chính xác; commission batch; SePay reconciled tự động; P&L realtime.

**Automation level:** **Cao (80%)** — attribution + reconcile + batch tự động, duyệt chi là con người.

**Khắc phục bottleneck:** N8 (tracking CTV thủ công), N9 (hoa hồng ~2 ngày/tháng), N10 (đối soát SePay ~2h/ngày), N11 (P&L không realtime), N13 (báo cáo theo cơ sở), R4 (132–137 CTV thủ công).

---

## 3.2. Bảng tổng hợp Automation Level theo Value Stream

| VS | Tên | Owner (A) | Automation hiện tại | Automation mục tiêu | Bottleneck giải quyết |
|---|---|---|---|---|---|
| VS1 | Lead Acquisition & CRM | GĐVH | ~10% | 90% | N5, N8, R8 |
| VS2 | Nurture, Close & QA | GĐVH | ~5% | 50% | N6, N14, R9, R13 |
| VS3 | Payment → Auto-Onboarding | GĐVH | ~30% (chỉ SePay) | 95% | N1,N2,N3,N4,N7,N10,R3,R7 |
| VS4 | Learning Operations | GĐVH | ~15% | 80% | N7 |
| VS5 | Student Care & Retention | GĐVH | ~5% | 65% | N12, R12 |
| VS6 | Instructor & Content Ops | GĐVH | ~10% | 75% | N9 |
| VS7 | Partner & Financial Ops | HĐQT | ~15% | 80% | N8,N9,N10,N11,N13,R4 |

---

# PHẦN IV — SOP CHI TIẾT (11 SOPs)

> Mỗi SOP tuân thủ format chuẩn quốc tế: Mã | Mục đích | Phạm vi | Trigger | Pre-conditions | Quy trình từng bước | Outputs | Owner | SLA | Exception handling | Revision history.

---

## SOP-01 — Lead Intake & CRM Auto-Capture

| Trường | Nội dung |
|---|---|
| **Mã SOP** | HSA-SOP-01 |
| **Value Stream** | VS1 |
| **Mục đích** | Đảm bảo 100% lead từ mọi kênh được đưa vào Odoo CRM tự động, không sót/trùng, gắn đúng nguồn, exam_type, cơ sở và CTV ref. |
| **Phạm vi** | Mọi lead phát sinh từ landing page hsavnu.edu.vn, ads FB/TikTok/Google, và ref link CTV. Áp dụng cả 2 cơ sở, 4 kỳ thi. |
| **Trigger** | Form submit trên landing page (webhook). |
| **Owner** | TP-Sale (vận hành); TechOps (kỹ thuật webhook). |
| **SLA** | Lead → CRM < 5 phút (tự động). |

**Pre-conditions:**
1. Landing page có hidden field capture `?ref=CTVxxx`, `utm_source`, `utm_campaign`, `exam_type`, `cơ sở`.
2. n8n webhook giữa landing page và Odoo hoạt động.
3. Odoo CRM có sẵn Sales Team theo exam_type × cơ sở và rule auto-assign.
4. Danh mục CTV (ctv_code) tồn tại trong Odoo.

**Quy trình từng bước:**
1. Học sinh/phụ huynh điền form (tên, SĐT, email, kỳ thi quan tâm, cơ sở).
2. Khi submit, landing page gửi payload (gồm UTM + ref_code) tới n8n webhook.
3. n8n chuẩn hóa dữ liệu: định dạng SĐT (`+84`/`0`), trim tên, lowercase email.
4. n8n gọi Odoo API tạo Lead với tags: `exam_type`, `source_channel`, `cơ sở`.
5. Odoo kiểm tra trùng theo SĐT/email; nếu trùng → merge và ghi log; nếu mới → tạo lead.
6. Nếu có `ref_code` hợp lệ → gắn tag `CTV_code` vào lead.
7. Odoo auto-assign lead cho Sales Team đúng exam_type + cơ sở (round-robin trong team).
8. Odoo ghi `created_at`, `source`, audit log.

**Outputs:** Lead record đầy đủ tag, đã phân công, có audit trail.

**Exception handling:**
- **Webhook fail:** n8n retry 3 lần (cách 2 phút); nếu vẫn fail → ghi vào queue dự phòng + alert TechOps; lead form lưu tạm trên landing page DB để re-sync.
- **Dữ liệu thiếu (SĐT sai):** tạo lead với tag `data_incomplete` → Sale bổ sung thủ công.
- **ref_code không tồn tại:** tạo lead bình thường, tag `ref_invalid` để TP-Sale rà soát.

**Revision history:** v1.0 (2026-06-16) — ban hành.

---

## SOP-02 — Nurture Campaign & QA Tư vấn Sale/CTV

| Trường | Nội dung |
|---|---|
| **Mã SOP** | HSA-SOP-02 |
| **Value Stream** | VS2 |
| **Mục đích** | Chuyển lead thành học sinh trả phí với tư vấn nhất quán theo playbook, có chuỗi nurture tự động, có QA và lịch sử audit. |
| **Phạm vi** | Toàn bộ lead Hot/Warm/Cold; Sale offline + CTV; cả 2 cơ sở. |
| **Trigger** | Lead được phân công; lead chuyển stage; lịch nurture. |
| **Owner** | TP-Sale. |
| **SLA** | Lead Hot → Sale gọi < 15 phút (giờ làm việc). |

**Pre-conditions:**
1. Playbook tư vấn với taxonomy case (Phụ lục C) đã ban hành.
2. Chuỗi nurture (Odoo Marketing/n8n → Zalo OA) đã cấu hình theo exam_type.
3. QA checklist hàng tuần đã có.

**Quy trình từng bước:**
1. Phân loại nhiệt độ lead (Hot/Warm/Cold) theo tiêu chí: Hot = chủ động hỏi giá/lịch; Warm = quan tâm chưa quyết; Cold = chỉ để lại thông tin.
2. **Hot:** Odoo notify Sale + bật SLA timer 15 phút → Sale gọi theo playbook → ghi note bắt buộc vào CRM (vấn đề, cam kết, bước tiếp).
3. **Warm:** kích hoạt nurture sequence (nội dung giá trị ngày 1/3/7 qua Zalo OA) → Sale theo dõi engagement → gọi lại khi có tín hiệu.
4. **Cold:** nurture dài hạn + tái kích hoạt theo mùa khai giảng.
5. **Case khó:** Sale tra playbook taxonomy (học phí, lịch học, chọn khóa, hoàn tiền, phụ huynh phản đối, so sánh đối thủ). Nếu không giải quyết → escalate TP-Sale.
6. TP-Sale review case mới → cập nhật playbook (không để case lặp lại review từ đầu).
7. **QA:** TP-Sale sample-check note/ghi âm tối thiểu 5 case/Sale/tuần theo QA checklist; chấm điểm tuân thủ.

**Outputs:** Lead chuyển Won/Lost; note tư vấn lưu CRM; playbook cập nhật; điểm QA.

**Exception handling:**
- **Sale không gọi trong SLA 15':** Odoo auto-escalate sang Sale backup + notify TP-Sale.
- **Khiếu nại/yêu cầu hoàn tiền:** chuyển Helpdesk ticket (SOP-10) + tag `refund_request`.
- **Lead phụ huynh phản đối:** áp dụng playbook "phụ huynh phản đối" + có thể escalate GĐVH.

**Revision history:** v1.0 (2026-06-16) — ban hành.

---

## SOP-03 — Payment Confirmation & Reconciliation

| Trường | Nội dung |
|---|---|
| **Mã SOP** | HSA-SOP-03 |
| **Value Stream** | VS3, VS7 |
| **Mục đích** | Xác nhận thanh toán và đối soát SePay tự động 100%, loại bỏ đối soát tay ~2h/ngày. |
| **Phạm vi** | Mọi giao dịch qua SePay trên hsavnu.edu.vn. |
| **Trigger** | SePay webhook `payment_success`. |
| **Owner** | Kế toán thu (KT). |
| **SLA** | Auto-reconciliation 100%; exception xử lý < 4h. |

**Pre-conditions:**
1. SePay webhook trỏ về Odoo (qua n8n nếu cần) hoạt động.
2. Mỗi đơn hàng Odoo có mã tham chiếu khớp nội dung chuyển khoản (order_ref).
3. Quy ước nội dung chuyển khoản chuẩn hóa (chứa order_ref/SĐT).

**Quy trình từng bước:**
1. SePay phát webhook `payment_success` kèm số tiền, nội dung CK, thời gian.
2. Odoo nhận → match giao dịch với đơn hàng theo order_ref/số tiền/SĐT.
3. Nếu khớp → đánh dấu `paid` + `reconciled` → **trigger SOP-04 (auto-onboarding)**.
4. Nếu không khớp (thừa/thiếu tiền, sai nội dung) → tạo exception cho KT thu.
5. KT thu xử lý exception trong SLA 4h: liên hệ HS, đối chiếu sao kê, điều chỉnh.
6. Cuối ngày: Odoo tự sinh báo cáo đối soát (đã match / chờ xử lý) — KT thu chỉ review danh sách exception (thay vì đối soát toàn bộ tay).

**Outputs:** Đơn hàng `paid + reconciled`; trigger onboarding; báo cáo đối soát hằng ngày.

**Exception handling:**
- **Thanh toán không match:** giữ ở trạng thái `unreconciled`, KT xử lý, **không** trigger onboarding cho đến khi xác nhận.
- **Thanh toán trùng:** đánh dấu `duplicate` + hoàn tiền theo quy trình hoàn tiền.
- **Webhook không nhận được:** TechOps kiểm tra log SePay, re-pull giao dịch theo lịch đối soát dự phòng (cuối ngày).

**Revision history:** v1.0 (2026-06-16) — ban hành.

---

## SOP-04 — Auto-Onboarding Chain (SePay → SBD → ClassIn → Zalo OA) [CORE]

| Trường | Nội dung |
|---|---|
| **Mã SOP** | HSA-SOP-04 |
| **Value Stream** | VS3 (CORE) |
| **Mục đích** | Tự động hóa toàn bộ chuỗi onboarding sau thanh toán, đạt Time-to-SBD < 2 phút và Time-to-ClassIn-Enroll < 5 phút, loại bỏ ~14h/ngày thủ công và SPOF "duyệt học sinh". |
| **Phạm vi** | Mọi đơn hàng đã `paid + reconciled`. Cả 2 cơ sở, 4 kỳ thi, chịu được spike HCM ~260 HS/ngày. |
| **Trigger** | Đơn hàng chuyển trạng thái `paid + reconciled` (từ SOP-03). |
| **Owner** | DuyetHS (xử lý exception); TechOps (vận hành chuỗi). |
| **SLA** | SBD gửi HS < 2 phút; ClassIn enroll < 5 phút; onboarding error rate < 1%. |

**Pre-conditions (BẮT BUỘC trước khi automation chạy):**
1. **Bảng mapping prerequisite tồn tại:** `hsa_course_code → classin_course_id → gv_uid → qll_user_id`.
2. ClassIn API V1 credentials hoạt động (register, addSchoolStudent, addCourseStudent).
3. n8n → Zalo OA API hoạt động.
4. SBD sequence generator cấu hình theo format `[KỲ_THI]-[NĂM]-[SEQ_5]`.
5. Template Zalo OA + Email đã duyệt.

**Quy trình từng bước (automation):**
1. **B1 — Sinh SBD:** Odoo cấp SBD tự tăng theo `exam-năm-SEQ5` (vd `HSA-2026-08421`), khóa SEQ để tránh trùng khi spike đồng thời.
2. **B2 — ClassIn enroll (API V1):**
   - `action=register` → tạo `classin_uid` cho học sinh.
   - `addSchoolStudent` → thêm HS vào trường ClassIn.
   - Lookup bảng mapping theo `hsa_course_code` của đơn.
   - `addCourseStudent` → thêm HS vào đúng khóa ClassIn.
3. **B3 — Zalo OA (qua n8n):** gửi SBD + link ClassIn + lịch học + tên GV (< 2 phút).
4. **B4 — Email:** gửi guide đầy đủ + link cài ClassIn/Zoom.
5. **B5 — Tạo Task QLL** trong Odoo Project, stage `Chờ xác nhận HS đăng nhập`, gán `qll_user_id` theo mapping.
6. **B6 — Commission:** nếu đơn có `ref_code` → cộng `commission_pending` cho CTV.
7. **B7 — Log** toàn bộ chuỗi vào Odoo (audit trail: timestamp từng bước).
8. **Theo dõi:** QLL xác nhận HS đăng nhập ClassIn trong 24h; nếu chưa → Zalo OA nhắc + QLL liên hệ.

**Outputs:** HS có SBD, enrolled ClassIn, nhận Zalo+Email, Task QLL, commission pending (nếu có), audit log.

**Exception handling (fallback):**
- **Mapping thiếu (B2):** HALT enroll, tạo Task priority cao cho DuyetHS + alert; **vẫn gửi SBD tạm + thông báo "đang xếp lớp"** để không treo HS; sau khi mapping bổ sung → chạy lại enroll.
- **ClassIn API lỗi:** retry 3 lần (cách 1 phút); nếu fail → queue + alert TechOps; DuyetHS enroll tay tạm theo checklist dự phòng.
- **Zalo OA gửi fail:** retry; nếu fail → fallback gửi Email + Task QLL gọi điện trực tiếp.
- **SBD trùng (race condition khi spike):** generator dùng lock; nếu vẫn trùng → reject + sinh lại + log lỗi nghiêm trọng.

**Revision history:** v1.0 (2026-06-16) — ban hành.

---

## SOP-05 — Class Setup & ClassIn Mapping

| Trường | Nội dung |
|---|---|
| **Mã SOP** | HSA-SOP-05 |
| **Value Stream** | VS4, VS6 |
| **Mục đích** | Tạo lớp ClassIn đúng chuẩn và hoàn thiện bảng mapping prerequisite TRƯỚC khi automation onboarding chạy. |
| **Phạm vi** | Mọi khóa học mới mở (theo đợt khai giảng). |
| **Trigger** | Quyết định mở khóa học mới (kế hoạch khai giảng). |
| **Owner** | QLL-Lead. |
| **SLA** | Mapping hoàn tất ≥ 48h trước ngày mở bán khóa. |

**Pre-conditions:**
1. Có kế hoạch khai giảng (kỳ thi, đợt, môn, lịch).
2. GV đã được phân công (gv_uid, classin_gv_uid).
3. QLL phụ trách lớp đã xác định (qll_user_id).

**Quy trình từng bước:**
1. Tạo `hsa_course_code` theo chuẩn `[KỲ_THI]-[NĂM]-[ĐỢT]-[MÔN]`.
2. Tạo lớp trên ClassIn qua **API V2 LMS `createClass`** với `teacherUid` cho từng lesson.
3. Ghi `classin_course_id` trả về vào bảng mapping Odoo.
4. Hoàn thiện hàng mapping: `hsa_course_code → classin_course_id → gv_uid → qll_user_id`.
5. Tạo nhóm Zalo lớp theo chuẩn `[KỲ_THI]-[NĂM]-[ĐỢT]-[LỚP]` (vd `HSA-2026-D1-L01`).
6. QLL-Lead kiểm tra checklist mở khóa (Phụ lục D) → xác nhận sẵn sàng.
7. Đánh dấu khóa `ready_for_enrollment` → cho phép VS3 enroll.

**Outputs:** Lớp ClassIn tạo xong; bảng mapping đầy đủ; nhóm Zalo đúng chuẩn; khóa sẵn sàng nhận HS.

**Exception handling:**
- **createClass lỗi:** TechOps kiểm tra credentials API V2; tạo tay tạm + ghi mapping.
- **Thiếu GV/QLL:** không đánh dấu `ready` (chặn enroll lỗi) → escalate QLL-Lead/GĐVH.

**Revision history:** v1.0 (2026-06-16) — ban hành.

---

## SOP-06 — Learning Operations & Attendance Monitoring

| Trường | Nội dung |
|---|---|
| **Mã SOP** | HSA-SOP-06 |
| **Value Stream** | VS4, VS5 |
| **Mục đích** | Vận hành buổi học ổn định và giám sát điểm danh/đăng nhập tự động qua ClassIn Data Subscription. |
| **Phạm vi** | Mọi lớp đang hoạt động. |
| **Trigger** | Buổi học đến giờ; Data Subscription push (~20' sau buổi). |
| **Owner** | QLL-Lead (backup: 2 QLL được đào tạo — xem SPOF-03). |
| **SLA** | Câu hỏi HS trong giờ học < 30 phút; at-risk catch < 24h. |

**Pre-conditions:**
1. ClassIn Data Subscription (PUSH) đã bật: attendance, LMS scores, login activity.
2. Attendance Dashboard QLL đã có.
3. Lớp đã setup (SOP-05).

**Quy trình từng bước:**
1. Trước buổi: xác nhận link lớp ClassIn + GV sẵn sàng (nhắc tự động qua Odoo activity).
2. Trong buổi: GV dạy ClassIn; QLL trực hỗ trợ; câu hỏi HS được trả lời < 30 phút.
3. ~20' sau buổi: Data Subscription đẩy attendance về Odoo tự động.
4. LMS scores đồng bộ realtime; login activity đồng bộ.
5. Dashboard QLL cập nhật: tỷ lệ tham dự lớp, HS vắng, HS không login.
6. QLL rà soát flag at-risk hằng ngày (xem SOP-07 cho can thiệp).
7. Zoom chỉ dùng dự phòng khi ClassIn sự cố (ghi log lý do).

**Outputs:** Dữ liệu điểm danh/điểm/đăng nhập trong Odoo; dashboard realtime; attendance rate đo được (target > 80%).

**Exception handling:**
- **Data Subscription không push:** TechOps kiểm tra endpoint; pull tay qua API tạm; alert.
- **ClassIn sập giữa buổi:** chuyển Zoom dự phòng, QLL thông báo nhóm Zalo lớp, ghi incident (SOP-10).

**Revision history:** v1.0 (2026-06-16) — ban hành.

---

## SOP-07 — Student Care & At-Risk Intervention

| Trường | Nội dung |
|---|---|
| **Mã SOP** | HSA-SOP-07 |
| **Value Stream** | VS5 |
| **Mục đích** | Phát hiện sớm và can thiệp học sinh có nguy cơ bỏ học; đo NPS định kỳ; mọi tương tác có ticket/lịch sử. |
| **Phạm vi** | Mọi học sinh đang học. |
| **Trigger** | Data Subscription (vắng/điểm thấp/không login); câu hỏi HS; lịch NPS. |
| **Owner** | QLL (vận hành); QLL-Lead (giám sát). |
| **SLA** | Vắng 2+ buổi → QLL liên hệ < 24h. |

**Pre-conditions:** Data Subscription + dashboard at-risk hoạt động; Odoo Helpdesk cấu hình; Zalo OA ZNS template duyệt.

**Quy trình từng bước:**
1. **Vắng 1 buổi:** hệ thống log + flag dashboard (chưa cần can thiệp).
2. **Vắng 2+ buổi liên tiếp:** tạo Task QLL priority cao + Zalo OA nhắc HS → QLL liên hệ < 24h, ghi kết quả vào ticket.
3. **Không login 3 ngày:** trigger Zalo OA ZNS hỏi thăm tự động.
4. **completion_rate < 50%:** Zalo OA gợi ý tài liệu bổ trợ theo môn.
5. **Điểm thấp 2 lần liên tiếp:** alert QLL + Task "Tư vấn học tập HS [tên]".
6. **Câu hỏi/khiếu nại quan trọng:** tạo Helpdesk ticket (không xử lý chỉ trong Zalo).
7. **NPS định kỳ:** khảo sát qua Zalo OA → tổng hợp dashboard (target NPS > 50).

**Outputs:** At-risk catch trong 24h; ticket chăm sóc có lịch sử; NPS đo định kỳ; can thiệp học tập ghi nhận.

**Exception handling:**
- **HS không phản hồi sau can thiệp:** escalate QLL-Lead → cân nhắc liên hệ phụ huynh.
- **Yêu cầu hoàn tiền/nghỉ học:** chuyển ticket tài chính + ghi lý do churn (phục vụ phân tích).

**Revision history:** v1.0 (2026-06-16) — ban hành.

---

## SOP-08 — Instructor Scheduling & Payroll

| Trường | Nội dung |
|---|---|
| **Mã SOP** | HSA-SOP-08 |
| **Value Stream** | VS6 |
| **Mục đích** | Lập lịch GV minh bạch, đo tỷ lệ vào đúng giờ, tự động tổng hợp giờ dạy thành payroll draft, loại bỏ tổng hợp tay ~1 ngày/tháng. |
| **Phạm vi** | ~70 GV online, cả 2 cơ sở. |
| **Trigger** | Lập lịch kỳ mới; cuối tháng chốt payroll. |
| **Owner** | QLL-Lead (lịch); KT chi (payroll). |
| **SLA** | Tính thù lao GV xong trước ngày 5 tháng sau; payroll processing < 30 phút (batch). |

**Pre-conditions:** ClassIn timesheet + Data Subscription bật; Odoo HR/Payroll cấu hình; `teaching_rate` mỗi GV trong Odoo HR.

**Quy trình từng bước:**
1. QLL-Lead lập lịch dạy trong Odoo, map `teacherUid` per lesson.
2. GV nhận + xác nhận qua Odoo activity/Zalo.
3. GV dạy ClassIn; Data Subscription ghi login time → đo "vào đúng giờ" (so với lesson start).
4. Cuối tháng: Odoo tổng hợp giờ dạy từ ClassIn timesheet × `teaching_rate` → **draft payslip tự động**.
5. KT chi review batch payslip + đối chiếu ngoại lệ.
6. Duyệt + chi → trước ngày 5 tháng sau.
7. NPS GV từ HS thu thập định kỳ → dashboard GV.

**Outputs:** Lịch dạy minh bạch; chỉ số GV đúng giờ; payslip draft; thù lao chi đúng hạn; NPS GV.

**Exception handling:**
- **Timesheet ClassIn lệch (GV dạy bù/đổi giờ):** GV báo QLL-Lead → điều chỉnh thủ công có phê duyệt + ghi log.
- **GV không vào đúng giờ lặp lại:** alert QLL-Lead → đánh giá GV.

**Revision history:** v1.0 (2026-06-16) — ban hành.

---

## SOP-09 — CTV Commission Attribution & Payment

| Trường | Nội dung |
|---|---|
| **Mã SOP** | HSA-SOP-09 |
| **Value Stream** | VS7 |
| **Mục đích** | Attribution CTV chính xác qua ref link, tính hoa hồng tự động không tranh chấp, loại bỏ đối chiếu tay ~2 ngày/tháng cho 132–137 CTV. |
| **Phạm vi** | Toàn mạng lưới CTV/Đại sứ (132–137 người). |
| **Trigger** | Đơn hàng có ref_code confirmed; cuối tháng chốt commission. |
| **Owner** | KT chi; Quản lý CTV (backup owner — SPOF-04). |
| **SLA** | Tính hoa hồng CTV xong trước ngày 7 tháng sau; commission error rate < 0,5%. |

**Pre-conditions:** Mỗi CTV có `ctv_code` + `ref_link` (?ref=CTVxxx) + `bank_account` + `commission_rate` trong Odoo; ref tracking gắn vào form URL.

**Quy trình từng bước:**
1. CTV chia sẻ ref link cá nhân (?ref=CTVxxx).
2. HS đăng ký qua link → ref_code lưu vào lead → giữ qua đến order.
3. Khi order confirmed (paid) → Odoo gắn CTV + `commission_pending` (SOP-04 B6).
4. Cuối tháng: Odoo gom confirmed orders có ref_code → tính commission theo `commission_rate` → **commission batch**.
5. KT chi review batch (đối chiếu ngoại lệ, xử lý tranh chấp attribution dựa trên ref log).
6. Chi vào `bank_account` CTV + ghi sổ → trước ngày 7 tháng sau.
7. CTV xem trạng thái hoa hồng qua cổng đối tác/Zalo OA (minh bạch).

**Outputs:** Attribution chính xác; commission batch; chi đúng hạn; lịch sử attribution audit được.

**Exception handling:**
- **Tranh chấp attribution (2 CTV cùng claim):** quyết theo ref_code đầu tiên trong lead log (first-touch) — quy tắc cố định, không thương lượng.
- **HS không qua ref nhưng CTV claim:** không có ref_code → không tính commission (chống gian lận).
- **CTV thiếu bank_account:** giữ `pending` → Quản lý CTV bổ sung thông tin.

**Revision history:** v1.0 (2026-06-16) — ban hành.

---

## SOP-10 — Incident Management & Escalation

| Trường | Nội dung |
|---|---|
| **Mã SOP** | HSA-SOP-10 |
| **Value Stream** | Cross-cutting (ITIL Incident Management) |
| **Mục đích** | Mọi sự cố được ghi nhận thành ticket có lịch sử, phân mức nghiêm trọng, xử lý trong SLA, escalate đúng đường — thay cho xử lý qua Zalo (mất lịch sử). |
| **Phạm vi** | Mọi sự cố kỹ thuật, vận hành, tài chính, chăm sóc HS. |
| **Trigger** | Phát hiện sự cố (con người hoặc hệ thống alert). |
| **Owner** | TechOps (kỹ thuật); GĐVH (vận hành). |
| **SLA** | Ticket kỹ thuật: phản hồi < 15 phút, giải quyết < 2h. Ticket tài chính: phản hồi < 4h. |

**Pre-conditions:** Odoo Helpdesk cấu hình với severity levels và escalation rules; kênh alert automation tới TechOps.

**Quy trình từng bước:**
1. Ghi nhận sự cố → tạo Helpdesk ticket (bắt buộc, không xử lý chỉ trong Zalo cá nhân).
2. Phân loại severity: **P0** (hệ thống cốt lõi sập, ảnh hưởng thanh toán/onboarding diện rộng) | **P1** (chức năng quan trọng lỗi, có workaround) | **P2** (lỗi cục bộ, ảnh hưởng giới hạn) | **P3** (yêu cầu thông thường).
3. Gán owner theo loại; bật SLA timer.
4. Xử lý theo runbook; cập nhật ticket từng bước (audit).
5. Nếu vượt SLA → escalate theo Escalation Matrix (Phần V).
6. Đóng ticket khi resolved + ghi root cause; sự cố P0/P1 làm post-mortem.

**Outputs:** Ticket có lịch sử đầy đủ; root cause; post-mortem cho P0/P1.

**Exception handling:**
- **P0 ngoài giờ:** kích hoạt on-call TechOps + thông báo GĐVH/Operations Director ngay.
- **Sự cố lặp lại:** mở problem record → khắc phục căn nguyên (ITIL Problem Management).

**Revision history:** v1.0 (2026-06-16) — ban hành.

---

## SOP-11 — New Staff Onboarding & System Access

| Trường | Nội dung |
|---|---|
| **Mã SOP** | HSA-SOP-11 |
| **Value Stream** | Cross-cutting (HR/Security) |
| **Mục đích** | Onboard nhân sự mới nhanh, cấp quyền theo least-privilege, đảm bảo thu hồi quyền khi nghỉ việc. |
| **Phạm vi** | Mọi nhân sự fulltime, GV, CTV (cấp ref link). |
| **Trigger** | Quyết định tuyển dụng/cộng tác. |
| **Owner** | HCNS (quy trình); TechOps (cấp quyền hệ thống). |
| **SLA** | Onboarding hoàn tất ≤ 3 ngày làm việc; thu hồi quyền khi nghỉ ≤ 24h. |

**Pre-conditions:** Access Rights Matrix (Phần 2.4) là chuẩn cấp quyền; template onboarding (Phụ lục B).

**Quy trình từng bước:**
1. HCNS tạo hồ sơ nhân sự trong Odoo HR.
2. Xác định vai trò → tra Access Rights Matrix → TechOps cấp quyền đúng mức (Odoo, ClassIn, Google Workspace, Zalo OA, dashboard...).
3. Cấp tài khoản Google Workspace tổ chức (không dùng Drive cá nhân — R2).
4. Giao SOM + SOP liên quan vai trò + đọc cam kết bảo mật dữ liệu HS (OP-9).
5. Đào tạo theo checklist vai trò; backup được đào tạo cho vai trò SPOF.
6. CTV: cấp `ctv_code` + ref link + cập nhật `bank_account` + `commission_rate`.
7. **Offboarding:** khi nghỉ việc, HCNS trigger → TechOps thu hồi toàn bộ quyền ≤ 24h + chuyển giao dữ liệu/khách hàng phụ trách (chống R9, R12).

**Outputs:** Nhân sự sẵn sàng làm việc; quyền đúng vai trò; backup cho SPOF; offboarding sạch.

**Exception handling:**
- **Nghỉ việc đột ngột:** ưu tiên thu hồi quyền + đổi mật khẩu hệ thống dùng chung ngay (< 4h).
- **Vai trò mới chưa có trong Matrix:** Operations Director duyệt quyền tạm + cập nhật Matrix (change control).

**Revision history:** v1.0 (2026-06-16) — ban hành.

---

# PHẦN V — KHUNG SLA (SERVICE LEVEL AGREEMENT)

## 5.1. SLA Framework

**Định nghĩa:** SLA là cam kết dịch vụ đo lường được giữa các bộ phận (và với học sinh), gồm: chỉ tiêu (target), phương pháp đo (measurement), ngưỡng vi phạm (breach threshold), và hành động khi vi phạm (penalty/escalation).

**Nguyên tắc:**
- Mọi SLA phải **đo được tự động** (ưu tiên) hoặc có phương pháp đo rõ ràng.
- "Giờ làm việc" mặc định: 08:00–21:00 hằng ngày (chăm sóc HS theo lịch học buổi tối).
- SLA tự động (automation) đo bằng timestamp hệ thống; SLA con người đo bằng activity/ticket Odoo.
- Vi phạm SLA lặp lại được đưa vào đánh giá hiệu suất bộ phận hằng quý.

**Phân loại Penalty:**
- **Cảnh báo mềm:** lần đầu vi phạm → notify owner + ghi nhận.
- **Escalation:** vi phạm liên tục hoặc nghiêm trọng → đẩy lên cấp trên theo Escalation Matrix.
- **Review hiệu suất:** vi phạm có hệ thống → đưa vào KPI cá nhân/bộ phận.

## 5.2. Bảng SLA theo Value Stream

| Mã | Sự kiện | Target SLA | Đo bằng | Owner | Penalty/Escalation |
|---|---|---|---|---|---|
| SLA-01 | Lead → CRM | < 5 phút (auto) | Timestamp webhook → Odoo create | TechOps | Alert TechOps nếu > 5'; queue retry |
| SLA-02 | Lead Hot → Sale gọi | < 15 phút (giờ làm việc) | Odoo activity "called" vs assign time | TP-Sale | Auto-reassign + notify TP-Sale |
| SLA-03 | Payment → SBD gửi HS | < 2 phút (auto) | Timestamp paid → Zalo OA sent | TechOps | Alert + fallback Email + Task QLL |
| SLA-04 | Payment → ClassIn enroll | < 5 phút (auto) | Timestamp paid → addCourseStudent OK | TechOps | Retry 3x → exception DuyetHS |
| SLA-05 | Câu hỏi HS trong giờ học | < 30 phút | Ticket/Zalo response time | QLL | Escalate QLL-Lead |
| SLA-06 | Ticket sự cố kỹ thuật | phản hồi < 15', giải quyết < 2h | Helpdesk timestamps | TechOps | Escalate Operations Director |
| SLA-07 | Ticket tài chính | phản hồi < 4h | Helpdesk timestamps | KT | Escalate GĐVH |
| SLA-08 | Hoàn tiền | < 3 ngày làm việc | Refund ticket created → done | KT chi | Escalate GĐVH/HĐQT |
| SLA-09 | Vắng 2+ buổi → QLL liên hệ | < 24h | Data Subscription flag → QLL contact log | QLL | Escalate QLL-Lead |
| SLA-10 | Tính thù lao GV | trước ngày 5 tháng sau | Payroll batch approved date | KT chi | Escalate GĐVH |
| SLA-11 | Tính hoa hồng CTV | trước ngày 7 tháng sau | Commission batch approved date | KT chi | Escalate GĐVH |
| SLA-12 | Không login 3 ngày → ZNS | < 3 ngày (auto) | Login activity → ZNS sent | TechOps | Alert + manual ZNS |
| SLA-13 | Thu hồi quyền khi nghỉ việc | < 24h | HR offboard → access revoked | HCNS+TechOps | Escalate Operations Director (rủi ro bảo mật) |

## 5.3. Escalation Matrix

| Cấp | Vai trò xử lý | Khi nào escalate lên | Escalate tới |
|---|---|---|---|
| L1 | Người thực hiện (Sale/QLL/GV/KT/TechOps) | SLA breach lần đầu hoặc ngoài năng lực | Lead trực tiếp |
| L2 | Lead (TP-Sale/QLL-Lead/KT tổng hợp) | Breach lặp lại / ảnh hưởng nhiều HS / liên bộ phận | GĐVH cơ sở |
| L3 | GĐVH (Bắc/Nam) | Ảnh hưởng cơ sở / xung đột liên bộ phận / P0-P1 | Operations Director |
| L4 | Operations Director | Ảnh hưởng toàn chuỗi / quyết định ngân sách lớn / rủi ro chiến lược | HĐQT |
| Tài chính | KT → GĐVH → HĐQT | Hoàn tiền > hạn mức, tranh chấp commission lớn | HĐQT |
| Kỹ thuật P0 | TechOps on-call → Operations Director | Hệ thống cốt lõi sập | HĐQT (thông báo) |

**Quy tắc escalation:** Escalate kèm context (ticket, dữ liệu, đã thử gì). Cấp nhận escalation phải phản hồi trong nửa SLA gốc. P0 escalate ngay không chờ SLA.

## 5.4. Phương pháp đo (Measurement Method)

- **SLA tự động (01, 03, 04, 12):** Odoo/n8n ghi timestamp từng bước; dashboard tính % đạt theo ngày/tuần; cảnh báo realtime khi vượt ngưỡng.
- **SLA con người (02, 05, 09):** đo qua Odoo activity/Helpdesk ticket timestamp; báo cáo % đạt theo Sale/QLL hằng tuần.
- **SLA tài chính (07, 08, 10, 11):** đo theo ngày phê duyệt batch/ticket trong Odoo Accounting.
- **Báo cáo SLA:** Operations Director review % đạt toàn bộ SLA hằng tháng; mục tiêu tổng thể ≥ 95% SLA đạt.

---

# PHẦN VI — THIẾT KẾ TỰ ĐỘNG HÓA

## 6.1. Automation Philosophy (80/20)

Tự động hóa tại HSA tuân thủ OP-2: **tự động hóa 80% việc lặp lại có quy luật, con người xử lý 20% ngoại lệ**. Nguyên tắc thiết kế:
1. **Idempotent:** mỗi automation chạy lại nhiều lần cho cùng input không gây tác dụng phụ (chống double-enroll, double-commission).
2. **Fail-safe:** khi automation fail, hệ thống **không** treo học sinh — luôn có fallback thủ công có checklist.
3. **Observable:** mọi bước log timestamp; mọi failure phát alert.
4. **Prerequisite-gated:** automation chỉ chạy khi điều kiện tiên quyết (mapping) tồn tại.
5. **Human-in-the-loop cho tiền:** mọi giao dịch chi (payroll, commission, refund) cần người duyệt batch — automation chỉ chuẩn bị draft.

## 6.2. Automation Dependency Map

```
┌─────────────────────────────────────────────────────────────┐
│ PREREQUISITES (phải tồn tại TRƯỚC khi automation chạy)        │
│  • Bảng mapping: hsa_course_code→classin_course_id→gv_uid→qll │
│  • ClassIn API V1/V2 credentials hoạt động                    │
│  • SePay webhook → Odoo hoạt động                             │
│  • n8n → Zalo OA API hoạt động                               │
│  • SBD sequence generator cấu hình                           │
│  • Templates Zalo OA + Email đã duyệt                        │
│  • Sales Team + auto-assign rule trong Odoo CRM              │
└──────────────────────────┬──────────────────────────────────┘
                           │ nếu thiếu bất kỳ ↓
                    [Automation HALT + tạo exception + fallback thủ công]

LUỒNG PHỤ THUỘC:
  SOP-05 (mapping sẵn sàng) ──prerequisite──▶ SOP-04 (auto-onboarding)
  SOP-03 (payment reconciled) ──trigger──▶ SOP-04
  SOP-04 (enroll) ──enables──▶ Data Subscription (VS4/VS5/VS6 triggers)
```

## 6.3. Chi tiết Automation theo Trigger

### TRIGGER 1 — SePay Webhook (payment_success)
**Điều kiện chạy:** order `paid + reconciled`; mapping tồn tại.
```
((SePay payment_success))
 ==> B1: Auto-generate SBD ([KỲ_THI]-[NĂM]-[SEQ_5], vd HSA-2026-08421)
 ==> B2: ClassIn API V1: register → addSchoolStudent → lookup mapping → addCourseStudent
 ==> B3: Zalo OA (qua n8n): gửi SBD + link ClassIn + lịch + GV  (< 2 phút)
 ==> B4: Email: guide đầy đủ + link cài ClassIn
 ==> B5: Odoo Project Task cho QLL (stage "Chờ xác nhận HS đăng nhập")
 ==> B6: <ref_code?> → cộng commission_pending cho CTV
 ==> B7: Log toàn bộ vào Odoo (audit trail)
```
**Fallback:** mapping thiếu → gửi SBD tạm + "đang xếp lớp" + Task DuyetHS; ClassIn fail → retry 3x → enroll tay; Zalo fail → Email + Task gọi điện.

### TRIGGER 2 — ClassIn Data Subscription: Attendance
```
((attendance push ~20' sau buổi))
 ==> <vắng 1 buổi?>   → log + flag dashboard QLL
 ==> <vắng 2+ liên tiếp?> → Task QLL priority cao + Zalo OA nhắc HS  (SLA-09 < 24h)
 ==> <không login 3 ngày?> → Zalo OA ZNS hỏi thăm  (SLA-12)
```
**Fallback:** không nhận push → TechOps pull tay qua API + alert.

### TRIGGER 3 — ClassIn Data Subscription: LMS Score
```
((LMS score push realtime))
 ==> <completion_rate < 50%?> → Zalo OA gợi ý tài liệu bổ trợ theo môn
 ==> <điểm thấp 2 lần liên tiếp?> → Alert QLL + Task "Tư vấn học tập HS [tên]"
```

### TRIGGER 4 — Web Form Submit (Lead mới)
```
((form submit))
 ==> Odoo CRM auto-create Lead (tags: exam_type, source, cơ sở)
 ==> auto-assign Sales Team theo exam_type × cơ sở
 ==> <ref_code?> → tag CTV_code vào Lead
```
**Fallback:** webhook fail → n8n retry 3x → queue dự phòng + alert.

### TRIGGER 5 — Lead stage change → Hot
```
((stage → Hot))
 ==> Odoo activity notify Sale phụ trách
 ==> bật SLA timer 15 phút (SLA-02)
 ==> <quá 15' chưa gọi?> → auto-reassign backup + notify TP-Sale
```

### TRIGGER 6 — Cuối tháng (Payroll & Commission)
```
((cuối tháng))
 ==> GV Payroll: tổng hợp giờ dạy ClassIn timesheet × teaching_rate → draft payslip → KT review (SLA-10 trước ngày 5)
 ==> CTV Commission: confirmed orders có ref_code → commission batch theo commission_rate → KT review (SLA-11 trước ngày 7)
```
**Fallback:** timesheet lệch → điều chỉnh thủ công có phê duyệt; mọi batch cần người duyệt (human-in-the-loop cho tiền).

## 6.4. Fallback Procedures khi Automation Fail

| Automation | Chế độ fail | Fallback |
|---|---|---|
| SBD generation | Lock fail / trùng | Sinh lại + log lỗi nghiêm trọng |
| ClassIn enroll | API down / mapping thiếu | Retry 3x → queue → DuyetHS enroll tay theo checklist |
| Zalo OA gửi | API down | Retry → fallback Email + Task QLL gọi điện |
| SePay webhook | Không nhận | Đối soát dự phòng cuối ngày (re-pull) |
| Data Subscription | Không push | TechOps pull tay qua API |
| Lead webhook | Fail | n8n queue dự phòng + landing DB lưu tạm |

**Nguyên tắc vàng:** automation fail **không bao giờ** được làm học sinh kẹt — luôn có đường thủ công có người chịu trách nhiệm và checklist.

## 6.5. Monitoring & Alert cho Automation Failures

- **Health check:** n8n + Odoo scheduler kiểm tra endpoint (SePay, ClassIn, Zalo OA) mỗi 5 phút.
- **Alert channel:** failure → notify TechOps qua kênh on-call (P0/P1 → ngay; P2/P3 → tổng hợp).
- **Dead-letter queue:** mọi message fail vào queue để re-process, không mất dữ liệu.
- **Automation dashboard:** hiển thị success rate từng trigger, số exception đang chờ, MTTR.
- **Daily automation report:** Operations Director nhận tổng hợp số onboarding tự động/exception/fail hằng ngày.

---

# PHẦN VII — QUẢN TRỊ DỮ LIỆU

## 7.1. Data Governance Framework

**Mục tiêu:** Mỗi loại dữ liệu có owner rõ ràng, định nghĩa chuẩn (master data), naming convention thống nhất, chuẩn chất lượng, chính sách lưu trữ và audit — chấm dứt tình trạng dữ liệu phân mảnh trong Google Sheet và Drive cá nhân (R2, R11).

**Nguyên tắc (theo OP-3, OP-6, OP-9):**
1. SSOT: mỗi dữ liệu một nguồn chính thức (Odoo/ClassIn/SePay).
2. Mọi thay đổi PII có audit log.
3. Phân quyền least-privilege (Phần 2.4).
4. Không lưu dữ liệu chính thức trong tài khoản cá nhân.

## 7.2. Data Ownership Matrix

| Loại dữ liệu | System of Record (SSOT) | Data Owner (vai trò) | Người dùng chính |
|---|---|---|---|
| Lead / CRM | Odoo CRM | GĐVH | Sale, TP-Sale |
| Học sinh (PII, SBD, enrollment) | Odoo | DuyetHS / GĐVH | QLL, KT |
| Khóa học mapping | Odoo (mapping table) | QLL-Lead | TechOps, automation |
| Điểm danh / điểm LMS | ClassIn → Odoo | QLL-Lead | QLL, GV |
| Giao dịch thanh toán | SePay → Odoo | KT thu | KT tổng hợp |
| CTV (code, ref, bank) | Odoo | Quản lý CTV | KT chi |
| GV (uid, rate, specialization) | Odoo HR | QLL-Lead / HCNS | KT chi |
| Tài chính / P&L | Odoo Accounting | KT tổng hợp / HĐQT | GĐVH, HĐQT |
| Tài liệu nội bộ | Google Workspace (tổ chức) + Odoo Documents | Operations Director | Toàn bộ theo quyền |
| Ticket / sự cố | Odoo Helpdesk | TechOps / GĐVH | Toàn bộ liên quan |

## 7.3. Master Data Definitions

**Học sinh (Student):**
| Field | Kiểu | Mô tả | Bắt buộc |
|---|---|---|---|
| `student_sbd` | string | Số báo danh, format chuẩn | Có |
| `exam_type` | enum | HSA / BCA / BQP / ĐGNL HCM | Có |
| `cohort` | string | Đợt/khóa | Có |
| `classin_uid` | string | UID trên ClassIn | Có (sau enroll) |
| `qll_assigned` | ref | QLL phụ trách | Có |
| `enrollment_date` | datetime | Ngày nhập học | Có |
| `branch` | enum | HN / HCM | Có |
| `phone`, `email`, `name` | PII | Liên hệ | Có |
| `ref_code` | string | CTV ref (nếu có) | Không |

**Khóa học mapping (Course Mapping):**
| Field | Mô tả |
|---|---|
| `hsa_course_code` | Mã khóa HSA: `[KỲ_THI]-[NĂM]-[ĐỢT]-[MÔN]` |
| `classin_course_id` | ID lớp trên ClassIn (từ createClass) |
| `classin_gv_uid` | UID GV trên ClassIn |
| `gv_uid` | Mã GV nội bộ |
| `qll_user_id` | QLL phụ trách lớp |

**CTV:**
| Field | Mô tả |
|---|---|
| `ctv_code` | Mã CTV (vd CTV001) |
| `ref_link` | URL ref: `...?ref=CTV001` |
| `bank_account` | Tài khoản nhận hoa hồng |
| `commission_rate` | Tỷ lệ hoa hồng |

**GV (Instructor):**
| Field | Mô tả |
|---|---|
| `gv_uid` | Mã GV nội bộ |
| `teaching_rate` | Đơn giá giờ dạy |
| `classin_gv_uid` | UID GV trên ClassIn |
| `specialization` | Môn/kỳ thi chuyên |

## 7.4. Naming Conventions

| Đối tượng | Format | Ví dụ |
|---|---|---|
| Số báo danh (SBD) | `[KỲ_THI]-[NĂM]-[SEQ_5_DIGIT]` | `HSA-2026-08421` |
| Zalo nhóm lớp | `[KỲ_THI]-[NĂM]-[ĐỢT]-[LỚP]` | `HSA-2026-D1-L01` |
| Mã khóa học | `[KỲ_THI]-[NĂM]-[ĐỢT]-[MÔN]` | `HSA-2026-D1-TOAN` |
| Mã CTV | `CTV` + số 3 chữ số | `CTV001` |
| Ticket Helpdesk | `[LOẠI]-[YYYYMM]-[SEQ]` | `TECH-202606-014` |

**Quy ước KỲ_THI:** `HSA` (ĐGNL HSA), `BCA` (Bộ Công an), `BQP` (Bộ Quốc phòng), `HCM` (ĐGNL HCM).

## 7.5. Data Quality Standards

- **Số điện thoại:** chuẩn hóa về `0xxxxxxxxx` (10 số) hoặc `+84xxxxxxxxx`; validate khi nhập; loại ký tự thừa.
- **Tên:** trim khoảng trắng, viết hoa chuẩn (title case), không ký tự đặc biệt bất thường.
- **Email:** lowercase, validate định dạng RFC; cảnh báo email tạm/giả.
- **Chống trùng:** dedupe theo SĐT + email khi tạo lead/HS.
- **Bắt buộc field:** không cho tạo record thiếu field bắt buộc (Phần 7.3).
- **Data quality score:** đo % record đạt chuẩn; mục tiêu ≥ 98%.

## 7.6. Data Retention Policy

| Loại dữ liệu | Thời gian lưu | Sau đó |
|---|---|---|
| Lead không chuyển đổi | 24 tháng | Ẩn danh hóa hoặc xóa |
| Hồ sơ học sinh (active) | Trong suốt thời gian học + 36 tháng | Lưu trữ/ẩn danh |
| Giao dịch tài chính | Theo quy định kế toán VN (tối thiểu 10 năm) | Lưu trữ |
| Ticket/sự cố | 24 tháng | Lưu trữ tổng hợp |
| Audit log PII | ≥ 24 tháng | Lưu trữ bảo mật |
| Dữ liệu CTV (sau ngừng cộng tác) | 24 tháng (đối soát hoa hồng) | Ẩn danh |

## 7.7. Privacy & Audit Log (GDPR-inspired)

- **Quyền chủ thể dữ liệu:** học sinh/phụ huynh có thể yêu cầu xem/sửa/xóa dữ liệu cá nhân (theo SLA hoàn tất ≤ 7 ngày làm việc, trừ dữ liệu phải lưu theo luật kế toán).
- **Audit log bắt buộc:** mọi truy cập/sửa/xóa PII ghi log (ai, khi nào, làm gì) — đáp ứng R11.
- **Phân quyền PII:** masked cho vai trò không cần PII đầy đủ (Phần 2.4).
- **Đồng ý (consent):** form đăng ký có điều khoản xử lý dữ liệu.
- **Phòng rò rỉ:** không export PII ra file cá nhân; cảnh báo export bất thường.

---

# PHẦN VIII — KHUNG KPI VÀ ĐO LƯỜNG

## 8.1. KPI Framework (5 chiều)

KPI tổ chức theo 5 chiều của hành trình giá trị: **Acquisition → Conversion → Onboarding → Learning/Retention → Finance**. Mọi KPI có **target cụ thể**, **threshold cảnh báo**, và **cách đo** (OP-5). Cấm mục tiêu mơ hồ.

## 8.2. ACQUISITION KPIs

| KPI | Target | Threshold cảnh báo | Cách đo |
|---|---|---|---|
| Lead Volume by Channel & Exam Type | Theo kế hoạch tháng từng kênh/kỳ thi | < 80% kế hoạch | Đếm lead Odoo theo tag source × exam_type (daily/weekly) |
| Lead Response Time (Hot → gọi < 15') | ≥ 95% đạt | < 90% | Odoo activity time vs assign time (SLA-02) |
| Lead-to-Call Rate | ≥ 90% lead được liên hệ | < 80% | % lead có activity "called" |
| Cost per Lead by Channel | Theo ngân sách (giảm dần QoQ) | Vượt 120% mục tiêu | Chi phí ads / số lead theo kênh |

## 8.3. CONVERSION KPIs

| KPI | Target | Threshold | Cách đo |
|---|---|---|---|
| Lead-to-Payment Conversion (by exam × cơ sở) | ≥ 20% (chuẩn hóa theo baseline từng kỳ) | < 15% | Orders paid / leads (Odoo) |
| Average Sales Cycle Length | ≤ 7 ngày | > 14 ngày | Trung bình (paid_date − lead_created) |
| CTV Attribution Rate | ≥ 95% order qua CTV có ref hợp lệ | < 90% | Orders có ref_code hợp lệ / orders gắn CTV |

## 8.4. ONBOARDING KPIs

| KPI | Target | Threshold | Cách đo |
|---|---|---|---|
| Time-to-SBD | < 2 phút | 99% đạt | Timestamp paid → SBD sent |
| Time-to-ClassIn-Enroll | < 5 phút | 99% đạt | Timestamp paid → addCourseStudent OK |
| HS đăng nhập ClassIn trong 24h | ≥ 85% | < 75% | Login activity (Data Subscription) trong 24h/enrollment |
| Onboarding Error Rate | < 1% | > 2% | Số onboarding exception / tổng onboarding |

## 8.5. LEARNING OPERATIONS KPIs

| KPI | Target | Threshold | Cách đo |
|---|---|---|---|
| Class Attendance Rate | > 80% | < 70% | Attendance ClassIn / tổng HS đăng ký buổi |
| At-risk Identification Rate | 100% (vắng ≥ 2 buổi catch < 24h) | < 95% | Flag time vs vắng buổi thứ 2 |
| Assignment Completion Rate | ≥ 70% | < 50% | LMS completion (Data Subscription) |
| Student Satisfaction (NPS) | > 50 | < 30 | Khảo sát NPS định kỳ qua Zalo OA |

## 8.6. FINANCE KPIs

| KPI | Target | Threshold | Cách đo |
|---|---|---|---|
| SePay Auto-reconciliation Rate | 100% | < 98% | Giao dịch auto-matched / tổng giao dịch |
| GV Payroll Processing Time | < 30 phút (batch) | > 1h | Thời gian sinh draft payslip batch |
| CTV Commission Error Rate | < 0,5% | > 1% | Số commission sai / tổng commission |
| P&L by Exam Type × Branch | Realtime available | Không realtime | Dashboard Odoo Accounting cập nhật liên tục |

## 8.7. Dashboard Definitions

### Dashboard 1 — COO Dashboard (Operations Director / GĐVH / HĐQT)
- **Mục đích:** Tổng quan toàn chuỗi theo thị trường/kỳ thi/cơ sở.
- **Thành phần:** P&L realtime (exam × cơ sở); lead volume & conversion; onboarding error rate & time-to-SBD; attendance trung bình; % SLA đạt; số automation exception/fail; doanh thu vs kế hoạch.
- **Tần suất:** realtime + báo cáo tuần/tháng.

### Dashboard 2 — QLL Dashboard (QLL-Lead / QLL)
- **Mục đích:** Quản theo lớp/học sinh/task.
- **Thành phần:** danh sách lớp phụ trách; attendance từng lớp; danh sách at-risk (vắng/không login/điểm thấp); task "chờ xác nhận đăng nhập"; SLA câu hỏi HS; NPS lớp.
- **Tần suất:** realtime.

### Dashboard 3 — GV Dashboard (Giảng viên)
- **Mục đích:** Lớp GV dạy + hiệu suất học sinh.
- **Thành phần:** lịch dạy + tỷ lệ vào đúng giờ của chính GV; attendance lớp mình; điểm/assignment completion HS lớp; NPS GV; giờ dạy tích lũy (đối chiếu payroll).
- **Tần suất:** realtime.

## 8.8. KPI Review Cadence

| Tần suất | Ai | Nội dung |
|---|---|---|
| Hằng ngày | GĐVH, QLL-Lead | Onboarding error, at-risk, SLA breach, automation fail |
| Hằng tuần | TP-Sale, QLL-Lead | Conversion, response time, attendance, QA tư vấn |
| Hằng tháng | Operations Director | Toàn bộ 5 chiều KPI vs target, payroll/commission accuracy |
| Hằng quý | HĐQT | P&L, chiến lược, rà soát SOM (OP-10) |

---

# PHẦN IX — QUẢN LÝ RỦI RO VÀ SPOF ELIMINATION

## 9.1. Risk Register (13 rủi ro + controls)

**Quy ước:** Mức độ = Khả năng (L) × Tác động (I), thang 1–5. Mức rủi ro = Cao/Trung/Thấp.

| Mã | Rủi ro | L | I | Mức | Control mới (TO-BE) | Owner |
|---|---|---|---|---|---|---|
| R1 | 1 outsource dev gánh toàn hệ thống | 4 | 5 | **Cao** | Tuyển Tech Ops nội bộ; document toàn bộ stack (Phụ lục A); outsource chỉ hỗ trợ (SPOF-01) | Operations Director |
| R2 | Dữ liệu trong Drive cá nhân | 5 | 4 | **Cao** | Google Workspace tổ chức + Odoo Documents; cấm Drive cá nhân (SOP-11) | TechOps |
| R3 | 1 người duyệt học sinh | 5 | 5 | **Cao** | Tự động hóa 100% (SOP-04); người chỉ xử lý exception; backup đào tạo (SPOF-02) | DuyetHS/GĐVH |
| R4 | CTV tracking thủ công 132–137 người | 4 | 4 | **Cao** | ref link automation (SOP-09); backup owner (SPOF-04) | Quản lý CTV |
| R5 | Mở rộng thị trường không có SOP/KPI | 3 | 5 | **Cao** | Bộ SOM/SOP/KPI chuẩn hóa (tài liệu này) áp dụng mọi cơ sở mới | Operations Director |
| R6 | Lãnh đạo không có dashboard | 4 | 4 | **Cao** | 3 dashboard realtime (Phần 8.7) | TechOps |
| R7 | Spike khai giảng HCM làm thủ công | 4 | 5 | **Cao** | Auto-onboarding chịu spike (SOP-04, OP-8) | GĐVH Nam |
| R8 | Lead nhập sót/trùng/chậm | 4 | 3 | **Trung** | Auto-capture + dedupe (SOP-01) | TP-Sale |
| R9 | Lịch sử tư vấn trong Zalo cá nhân | 4 | 4 | **Cao** | CRM note bắt buộc; offboarding chuyển giao (SOP-02, SOP-11) | TP-Sale |
| R10 | Branding lệch giữa 4 team truyền thông | 3 | 3 | **Trung** | Brand guideline thống nhất + duyệt tập trung | GĐVH |
| R11 | Không có audit log PII | 4 | 5 | **Cao** | Audit log bắt buộc (Phần 7.7) | TechOps |
| R12 | Sự cố mất khi đóng Zalo | 4 | 4 | **Cao** | Helpdesk ticket bắt buộc (SOP-10) | GĐVH |
| R13 | Tư vấn không nhất quán, thiếu QA | 4 | 3 | **Trung** | Playbook + QA checklist (SOP-02, Phụ lục C) | TP-Sale |

## 9.2. SPOF Elimination Plan

| SPOF | Hiện trạng | Kế hoạch loại bỏ | Trạng thái mục tiêu |
|---|---|---|---|
| **SPOF-01** | 1 outsource dev gánh toàn hệ thống | Tuyển **Tech Ops nội bộ**; document toàn bộ technical stack (Phụ lục A); outsource chuyển sang vai trò hỗ trợ có hợp đồng SLA | Stack có ≥ 2 người hiểu; document 100% |
| **SPOF-02** | 1 người duyệt học sinh | **Tự động hóa 100%** (SOP-04); người chỉ xử lý exception; đào tạo backup | Không ai là điểm chặn onboarding |
| **SPOF-03** | QLL Lead | **SOP-06 thành checklist**; đào tạo **2 QLL backup** tiếp quản trong 24h | 2 backup sẵn sàng |
| **SPOF-04** | 1 quản lý CTV | **ref_code automation** (SOP-09); chỉ định backup owner | Commission chạy không phụ thuộc 1 người |
| **SPOF-05** | Google Drive cá nhân | **Google Workspace tổ chức + Odoo Documents**; di trú dữ liệu; cấm Drive cá nhân | 0 dữ liệu chính thức trong tài khoản cá nhân |

## 9.3. Business Continuity Plan (BCP)

| Kịch bản sự cố | Tác động | Phương án dự phòng |
|---|---|---|
| ClassIn sập | Không dạy được | Chuyển **Zoom dự phòng** (legacy giữ sẵn); QLL thông báo nhóm Zalo lớp; ghi incident |
| SePay gián đoạn | Không nhận thanh toán | Hướng dẫn chuyển khoản thủ công + đối soát tay tạm; bật lại auto khi khôi phục |
| Odoo down | Mất SSOT vận hành | Read-replica/backup; quy trình thủ công có checklist; ưu tiên khôi phục P0 |
| n8n down | Automation ngừng | Fallback thủ công cho onboarding (SOP-04); TechOps khôi phục |
| Zalo OA lỗi | Không gửi thông báo HS | Fallback Email + QLL gọi điện |
| Mất nhân sự SPOF | Gián đoạn vai trò | Backup đào tạo tiếp quản trong 24h (SPOF plan) |

**Backup & Recovery:** Odoo + dữ liệu được backup định kỳ (tối thiểu hằng ngày); RPO mục tiêu ≤ 24h, RTO mục tiêu ≤ 4h cho hệ thống cốt lõi.

## 9.4. Incident Severity Levels

| Mức | Định nghĩa | Ví dụ | SLA phản hồi | SLA giải quyết |
|---|---|---|---|---|
| **P0** | Hệ thống cốt lõi sập, ảnh hưởng diện rộng | SePay/Odoo down, không onboarding được | < 15 phút | < 2h (huy động toàn lực) |
| **P1** | Chức năng quan trọng lỗi, có workaround | ClassIn enroll fail hàng loạt | < 30 phút | < 4h |
| **P2** | Lỗi cục bộ, ảnh hưởng giới hạn | 1 lớp lỗi điểm danh | < 2h | < 1 ngày |
| **P3** | Yêu cầu thông thường | Sửa thông tin 1 HS | < 4h | < 2 ngày |

---

# PHẦN X — KIẾN TRÚC CÔNG NGHỆ

## 10.1. Technology Stack Map (Approved Tools by Function)

| Chức năng | Công cụ chuẩn | Vai trò | Ghi chú |
|---|---|---|---|
| System of Record / Workflow | **Odoo** | CRM, Sales, Accounting, HR, Project, Helpdesk, Documents | SSOT trung tâm |
| Middleware / Automation | **n8n** | Cầu nối Odoo ↔ Zalo OA ↔ landing ↔ ClassIn | Đặc biệt cho Zalo (không thay bằng Email/SMS) |
| Lớp học live | **ClassIn** | Dạy live + LMS + điểm danh | Non-replaceable |
| Thanh toán | **SePay** | Cổng thanh toán nội địa + webhook | Non-replaceable |
| Giao tiếp HS | **Zalo OA** | Thông báo + ZNS | Non-replaceable |
| Landing/Web | **hsavnu.edu.vn** | Form đăng ký + thanh toán + ref capture | |
| Tài liệu/cộng tác | **Google Workspace (tổ chức)** | Drive tổ chức, Docs | Thay Drive cá nhân (R2) |
| Lớp dự phòng | **Zoom** | Dự phòng khi ClassIn sự cố | Legacy |
| CRM cũ | EZSale | Đang chuyển sang Odoo CRM | Loại bỏ dần |

## 10.2. Integration Architecture (text-based)

```
                         ┌──────────────────────────┐
   FB/TikTok/Google Ads ─▶ hsavnu.edu.vn (Landing)  │
                         │  form + ?ref=CTVxxx + UTM │
                         └────────────┬─────────────┘
                                      │ webhook
                                      ▼
   ┌─────────┐   webhook   ┌────────────────────┐   API   ┌──────────────┐
   │  SePay  │────────────▶│        n8n         │────────▶│   ClassIn    │
   │payment_ │             │   (middleware)     │ V1/V2   │ API V1/V2    │
   │success  │             │                    │◀────────│ Data Subscr. │
   └─────────┘             └─────────┬──────────┘  PUSH   │ (attendance, │
                                     │                    │  score,login)│
                                     │ API                └──────────────┘
                                     ▼
                          ┌────────────────────┐
                          │       ODOO         │  ◄── SSOT
                          │ CRM | Sales | Acct │
                          │ HR | Project |     │
                          │ Helpdesk | Docs    │
                          └─────────┬──────────┘
                                    │ via n8n
                                    ▼
                          ┌────────────────────┐
                          │     Zalo OA        │──▶ Học sinh (thông báo, ZNS)
                          └────────────────────┘
                                    │
                          ┌────────────────────┐
                          │  Dashboards        │──▶ COO / QLL / GV
                          └────────────────────┘
```

## 10.3. Non-replaceable Systems và cách tích hợp

1. **ClassIn** — nền tảng lớp học live chuyên biệt. Tích hợp qua API V1 (enroll), V2 (createClass), Data Subscription (PUSH attendance/score/login).
2. **SePay** — cổng thanh toán nội địa. Tích hợp qua webhook payment_success → Odoo (SSOT).
3. **Zalo OA** — kênh giao tiếp chính với HS Việt Nam. **Không thể thay bằng Email/SMS**; tích hợp qua n8n → Zalo OA API.

## 10.4. Odoo Module Configuration Overview

| Module | Cấu hình chính |
|---|---|
| CRM | Sales Team theo exam × cơ sở; auto-assign rule; lead tags (source, exam, ref); pipeline stage (New→Warm→Hot→Won/Lost) |
| Sales | Sản phẩm = khóa học (hsa_course_code); order ref khớp SePay |
| Accounting | Đối soát SePay; P&L analytic theo exam × cơ sở; batch payroll/commission |
| HR/Payroll | GV (gv_uid, teaching_rate); draft payslip từ timesheet |
| Project | Task QLL onboarding ("Chờ xác nhận HS đăng nhập"); at-risk task |
| Helpdesk | Ticket severity P0–P3; SLA policy; escalation |
| Documents | Tài liệu tổ chức thay Drive cá nhân |

## 10.5. ClassIn Integration Spec

| Layer | Endpoint/Action | Dùng cho | SOP |
|---|---|---|---|
| API V1 | `action=register` | Tạo classin_uid cho HS | SOP-04 B2 |
| API V1 | `addSchoolStudent` | Thêm HS vào trường | SOP-04 B2 |
| API V1 | `addCourseStudent` | Thêm HS vào khóa (sau lookup mapping) | SOP-04 B2 |
| API V2 LMS | `createClass` (teacherUid per lesson) | Tạo lớp khi mở khóa | SOP-05 |
| Data Subscription (PUSH) | attendance (~20' sau buổi) | Trigger 2 — điểm danh/at-risk | SOP-06/07 |
| Data Subscription (PUSH) | LMS scores (realtime) | Trigger 3 — can thiệp học tập | SOP-07 |
| Data Subscription (PUSH) | login activity | Đo đăng nhập 24h, không login 3 ngày | SOP-06/07 |

## 10.6. n8n Workflow Overview

| Workflow | Trigger | Hành động chính |
|---|---|---|
| WF-Lead-Capture | Landing webhook | Chuẩn hóa → Odoo create lead |
| WF-Onboarding | Odoo paid+reconciled | SBD → ClassIn enroll → Zalo OA → Email → Task |
| WF-Zalo-Notify | Odoo event | Gửi Zalo OA (SBD, nhắc vắng, ZNS) |
| WF-DataSub-Ingest | ClassIn push | Ghi attendance/score/login vào Odoo + trigger at-risk |
| WF-Health-Check | Cron 5 phút | Kiểm tra endpoint + alert |
| WF-Monthly-Batch | Cron cuối tháng | Hỗ trợ payroll/commission batch |

## 10.7. Technical Prerequisites Checklist (trước khi bật automation)

- [ ] Bảng mapping `hsa_course_code → classin_course_id → gv_uid → qll_user_id` đầy đủ cho mọi khóa đang mở bán.
- [ ] ClassIn API V1/V2 credentials test thành công.
- [ ] ClassIn Data Subscription endpoint nhận được push test.
- [ ] SePay webhook trỏ đúng + test payment_success.
- [ ] n8n → Zalo OA API gửi test thành công.
- [ ] SBD sequence generator chạy đúng + chống trùng khi concurrent.
- [ ] Templates Zalo OA + Email đã duyệt nội dung.
- [ ] Odoo Sales Team + auto-assign rule cấu hình.
- [ ] Audit log PII bật.
- [ ] Dead-letter queue + alert channel hoạt động.
- [ ] Fallback thủ công có checklist cho từng bước.

---

# PHẦN XI — LỘ TRÌNH TRIỂN KHAI

## 11.1. 30-60-90 Day Quick Wins (không cần Odoo — làm ngay giảm rủi ro)

**30 ngày:**
- Di trú dữ liệu từ Drive cá nhân → Google Workspace tổ chức (giảm R2/SPOF-05).
- Áp dụng naming convention (SBD, Zalo nhóm, khóa học) ngay trên Google Sheet hiện tại.
- Gắn `?ref=CTVxxx` vào form URL + bắt đầu tracking CTV thủ công có cấu trúc (giảm R4).
- Viết playbook tư vấn v1 + QA checklist (giảm R13).

**60 ngày:**
- Ban hành 11 SOP; đào tạo backup cho SPOF-03/04.
- Bắt buộc ghi note tư vấn vào CRM (EZSale tạm) thay Zalo cá nhân (giảm R9).
- Bắt buộc ticket sự cố (bảng/sheet tạm trước khi có Helpdesk) (giảm R12).

**90 ngày:**
- Tuyển Tech Ops nội bộ + bắt đầu document technical stack (giảm R1/SPOF-01).
- Chuẩn hóa bảng mapping prerequisite cho toàn bộ khóa (chuẩn bị automation).

## 11.2. Phase 0 — Foundation (Q2–Q3/2026)
- Chuẩn hóa SOP + data + naming convention.
- Google Workspace tổ chức go-live; cấm Drive cá nhân.
- Hoàn thiện bảng mapping prerequisite.
- Tuyển Tech Ops; bắt đầu document stack.
- **Exit criteria:** 11 SOP ban hành; data chuẩn hóa ≥ 95%; mapping đầy đủ.

## 11.3. Phase 1 — Core Automation (Q3/2026)
- Bật **SePay → Auto-onboarding** (SOP-04): SBD → ClassIn enroll → Zalo OA → Email → Task.
- Tích hợp ClassIn API V1/V2; n8n → Zalo OA.
- **Exit criteria:** Time-to-SBD < 2', Time-to-ClassIn-Enroll < 5', onboarding error < 1%.

## 11.4. Phase 2 — Odoo Foundation (Q4/2026–Q1/2027)
- Odoo CRM go-live (thay EZSale): auto-capture lead, auto-assign, nurture.
- Odoo Accounting go-live: đối soát SePay tự động, batch payroll/commission.
- **Exit criteria:** Lead→CRM < 5'; auto-reconciliation 100%; commission error < 0,5%.

## 11.5. Phase 3 — Full Integration (Q2–Q3/2027)
- ClassIn **Data Subscription** pipeline (attendance/score/login) → at-risk automation.
- Zalo OA middleware đầy đủ (nhắc vắng, ZNS, NPS).
- **3 dashboard** (COO/QLL/GV) realtime.
- **Exit criteria:** attendance đo được > 80%; at-risk catch 100% < 24h; P&L realtime; lãnh đạo có dashboard (giảm R6).

## 11.6. Phase 4 — Optimization (Q4/2027+)
- Tối ưu conversion/retention dựa dữ liệu (OP-10).
- Mở rộng quy mô hướng 2028; áp SOM cho cơ sở/kỳ thi mới (giảm R5).
- AI hỗ trợ tư vấn/phân loại lead, dự báo churn (đánh giá).

## 11.7. Bảng tổng hợp lộ trình

| Phase | Thời gian | Trọng tâm | Rủi ro/Bottleneck giải quyết |
|---|---|---|---|
| Quick Wins | 30-60-90 ngày | Data org, naming, ref, playbook, ticket | R2,R4,R9,R12,R13 |
| Phase 0 | Q2–Q3/2026 | SOP + data + Workspace + Tech Ops | R1,R2,R5,SPOF |
| Phase 1 | Q3/2026 | Auto-onboarding + ClassIn API | N1,N2,N3,N4,N7,R3,R7 |
| Phase 2 | Q4/2026–Q1/2027 | Odoo CRM + Accounting | N5,N6,N10,N8,N9,R8 |
| Phase 3 | Q2–Q3/2027 | Data pipeline + Zalo + dashboards | N11,N12,N13,R6,R11,R12 |
| Phase 4 | Q4/2027+ | Optimization + scale | R5 + cải tiến liên tục |

---

# PHẦN XII — PHỤ LỤC

## Phụ lục A — Technical Inventory (tài khoản & webhook cần document)

| Hạng mục | Cần document | Owner | Bảo mật |
|---|---|---|---|
| SePay | API key, webhook URL, secret, IP whitelist | TechOps | Vault |
| ClassIn API V1 | credentials, base URL, rate limit | TechOps | Vault |
| ClassIn API V2 LMS | credentials, createClass spec | TechOps | Vault |
| ClassIn Data Subscription | endpoint nhận push, schema payload | TechOps | Vault |
| Zalo OA | OA ID, access token, ZNS template ID | TechOps | Vault |
| Odoo | admin, DB backup, module list, API key | TechOps | Vault |
| n8n | workflow export, credentials store | TechOps | Vault |
| hsavnu.edu.vn | hosting, landing form config, ref capture | TechOps | Vault |
| Google Workspace | admin, domain, sharing policy | TechOps | Vault |
| DNS/domain | registrar, records | TechOps | Vault |

> Mục tiêu: **0 tài khoản chỉ 1 người biết**. Mọi credential lưu vault tổ chức, có ≥ 2 người truy cập được (chống SPOF-01).

## Phụ lục B — Template Onboarding Nhân sự mới

```
[ ] Tạo hồ sơ Odoo HR (vai trò, cơ sở, ngày bắt đầu)
[ ] Cấp Google Workspace tổ chức (không Drive cá nhân)
[ ] Cấp quyền hệ thống theo Access Rights Matrix (Phần 2.4)
[ ] Giao SOM + SOP liên quan vai trò
[ ] Ký cam kết bảo mật dữ liệu HS (OP-9)
[ ] Đào tạo theo checklist vai trò
[ ] (Nếu vai trò SPOF) đào tạo làm backup cho vai trò khác
[ ] (CTV) cấp ctv_code + ref_link + bank_account + commission_rate
[ ] Xác nhận hoàn tất onboarding (≤ 3 ngày làm việc)
--- OFFBOARDING ---
[ ] Thu hồi toàn bộ quyền ≤ 24h
[ ] Đổi mật khẩu tài khoản dùng chung
[ ] Chuyển giao khách hàng/dữ liệu phụ trách
```

## Phụ lục C — Playbook Tư vấn Sale/CTV (taxonomy + case handling)

**Taxonomy case (bắt buộc phân loại mọi case):**
| Mã | Loại case | Hướng xử lý chuẩn |
|---|---|---|
| C-01 | Học phí | Trình bày giá trị/lộ trình; chính sách trả góp/ưu đãi đợt |
| C-02 | Lịch học | Tra lịch khóa; tư vấn lớp phù hợp thời gian HS |
| C-03 | Chọn khóa | Hỏi mục tiêu/kỳ thi → đề xuất khóa theo exam_type |
| C-04 | Hoàn tiền | Áp chính sách hoàn tiền; chuyển ticket tài chính (SLA-08) |
| C-05 | Phụ huynh phản đối | Lắng nghe lo ngại; số liệu kết quả; mời tư vấn trực tiếp; escalate GĐVH nếu cần |
| C-06 | So sánh đối thủ | Nhấn điểm khác biệt HSA (live ClassIn, GV, tỷ lệ đỗ); không nói xấu đối thủ |
| C-07 | Kỹ thuật học (cài ClassIn) | Hướng dẫn theo guide; chuyển QLL nếu phức tạp |

**Cấu trúc 1 cuộc tư vấn (script khung):** Mở đầu (xác nhận nhu cầu) → Khám phá (mục tiêu, kỳ thi, thời gian) → Giải pháp (đề xuất khóa) → Xử lý phản đối (theo taxonomy) → Chốt (kêu gọi hành động + hỗ trợ thanh toán) → Ghi note CRM bắt buộc.

**QA checklist (TP-Sale chấm ≥ 5 case/Sale/tuần):** đúng SLA gọi; đúng script; phân loại đúng taxonomy; ghi note đầy đủ; không cam kết sai chính sách; thái độ chuẩn brand.

## Phụ lục D — Checklist Mở khóa học mới (hỗ trợ SOP-05)

```
[ ] Tạo hsa_course_code chuẩn [KỲ_THI]-[NĂM]-[ĐỢT]-[MÔN]
[ ] Phân công GV (gv_uid, classin_gv_uid)
[ ] Phân công QLL (qll_user_id)
[ ] ClassIn createClass (API V2, teacherUid per lesson) → classin_course_id
[ ] Ghi đầy đủ hàng mapping vào Odoo
[ ] Tạo nhóm Zalo lớp chuẩn [KỲ_THI]-[NĂM]-[ĐỢT]-[LỚP]
[ ] Tạo sản phẩm khóa trên web + Odoo Sales
[ ] Kiểm tra lịch học + link lớp
[ ] Đánh dấu khóa ready_for_enrollment (cho phép VS3 enroll)
[ ] Hoàn tất ≥ 48h trước ngày mở bán
```

## Phụ lục E — Tài liệu liên quan và phiên bản

| Tài liệu | Mã | Phiên bản | Quan hệ |
|---|---|---|---|
| HSA Education — Chiến lược vận hành | v3.0 | 3.0 | Tier 0 — định hướng chiến lược (đầu vào cho SOM) |
| Phân tích & đánh giá quy trình HSA Education | — | — | Tier 0 — phân tích AS-IS, bottlenecks, rủi ro |
| **Standard Operations Manual (tài liệu này)** | **HSA-SOM-v1.0** | **1.0** | **Tier 1 — chi phối toàn bộ vận hành** |
| ClassIn API Integration Spec | (sẽ ban hành) | — | Tier 4 — chi tiết kỹ thuật |
| Odoo Configuration Guide | (sẽ ban hành) | — | Tier 4 — cấu hình hệ thống |
| n8n Workflow Documentation | (sẽ ban hành) | — | Tier 4 — automation |
| Brand Guideline (4 team truyền thông) | (sẽ ban hành) | — | Tier 3 — chuẩn branding (giảm R10) |

---

## KẾT THÚC TÀI LIỆU

**HSA-SOM-v1.0 — Standard Operations Manual.** Trạng thái: **APPROVED**. Hiệu lực từ 2026-07-01.
Đây là tài liệu sống (living document); rà soát hằng quý theo OP-10. Mọi đề xuất thay đổi gửi Operations Director theo quy trình Change Control (mục Document Control).

> *"Chuẩn hóa trước, tự động hóa sau, đo lường liên tục, loại bỏ điểm lỗi đơn — để mỗi học sinh được phục vụ nhanh và nhất quán dù quy mô tăng gấp nhiều lần."*
