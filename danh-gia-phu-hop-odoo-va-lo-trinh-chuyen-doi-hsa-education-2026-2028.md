# HSA Education — Đánh Giá Phù Hợp Odoo & Lộ Trình Chuyển Đổi
## Fit-Gap Analysis · Target Architecture · Implementation Roadmap 2026–2028

---

> ⚠️ **TRẠNG THÁI TÀI LIỆU: RESEARCH / SUPERSEDED (một phần)**
>
> Tài liệu này là nghiên cứu nền (source layer) — **phần fit-gap module Odoo vẫn còn giá trị tham chiếu**.
> Tuy nhiên, các quyết định sau đây đã được **thay thế** bởi [HSA-PLATFORM-VISION-v1.0.md](docs/HSA-PLATFORM-VISION-v1.0.md) và [HSA-TECH-ROADMAP-v1.0.md](docs/HSA-TECH-ROADMAP-v1.0.md):
> - **Odoo edition:** Tài liệu này đề xuất Enterprise/Custom → Đã chốt: **Odoo Community (miễn phí, self-host)**
> - **Middleware:** Tài liệu này đề xuất n8n → Đã chốt: **.NET 10 / Hangfire (tự xây)**
> - **Chi phí license:** 325–565 triệu/năm (Enterprise) → Thực tế: **0 đồng (Community)**
> - **Lộ trình:** 4 GĐ A-D trong file này → Đã được thay bởi **Phase 0–5 trong PLATFORM-VISION §8**

---

**Loại tài liệu:** Phân tích đánh giá phù hợp Odoo, kiến trúc chuyển đổi và lộ trình triển khai chi tiết
**Tài liệu đầu vào:** [phan-tich-hien-trang-van-hanh-hsa-education-q2-2026.md](phan-tich-hien-trang-van-hanh-hsa-education-q2-2026.md)
**Phạm vi:** Chuyển các điểm nghẽn, rủi ro và nợ vận hành từ báo cáo hiện trạng thành target architecture, fit-gap Odoo, kế hoạch dữ liệu, tích hợp và rollout
**Quy mô hệ thống:** ~20.000 học sinh/năm | 62 fulltime/offline | ~70 GV online + 15 GV chính HCM | >170 CTV/freelance | 4 kỳ thi | 2 cơ sở
**Không thay thế:** ClassIn, SePay, Zalo OA — các hệ thống này cần tích hợp vào lõi quản trị
**Nguồn Odoo cập nhật:** [Odoo Pricing](https://www.odoo.com/pricing) và [Odoo 19 Documentation](https://www.odoo.com/documentation/19.0/), kiểm tra ngày 18/05/2026
**Người soạn:** Giám đốc vận hành (COO) / Product Owner
**Phiên bản:** 1.1 — Q2/2026

---

## 0. VAI TRÒ CỦA TÀI LIỆU NÀY

Báo cáo hiện trạng đã xác định 5 nhóm vấn đề chính: dữ liệu phân mảnh, onboarding sau thanh toán thủ công, thiếu dữ liệu học tập có cấu trúc, phụ thuộc cá nhân, và không có dashboard vận hành theo thời gian thực. Tài liệu này không lặp lại phần mô tả hiện trạng; nhiệm vụ của nó là trả lời câu hỏi tiếp theo:

> Odoo có phải lõi quản trị phù hợp để xử lý các điểm nghẽn đó không, nếu có thì triển khai theo thứ tự nào để không làm gián đoạn vận hành HSA?

Kết luận điều hành: **Odoo phù hợp cao, nhưng là quyết định "go có điều kiện".** HSA nên triển khai Odoo khi đã có SOP tối thiểu, dữ liệu đầu vào được chuẩn hóa, Tech Ops/Odoo Champion nội bộ, và lịch rollout tránh các spike khai giảng.

## I. ĐÁNH GIÁ TỔNG QUAN — Odoo có phải lựa chọn đúng không?

### 1.1 Vì sao cần hệ thống quản trị tập trung?

Hiện tại HSA Education đang vận hành với **5–7 hệ thống song song không kết nối**:

```
EZSale (CRM)         ←→  không tự động    ←→  Web portal
Google Sheet (dữ liệu)   ←→  thủ công nhập   ←→  SePay (thanh toán)
Zalo (nội bộ + HS)   ←→  copy-paste        ←→  Google Drive (file)
ClassIn (học tập)    ←→  không sync        ←→  Kế toán thủ công
```

Kết quả: **dữ liệu phân mảnh, công việc trùng lặp, không có single source of truth.**

Ở quy mô ~20.000 học sinh/năm với 4 kỳ thi và 2 cơ sở, bài toán này sẽ chỉ tệ hơn nếu không có một lõi quản trị tập trung. Tuy nhiên, lõi quản trị không có nghĩa là thay toàn bộ công cụ hiện hữu. Odoo chỉ nên giữ vai trò **system of record + workflow engine + reporting layer**.

### 1.2 Tại sao Odoo?

| Tiêu chí | Odoo | HubSpot | Bitrix24 | Custom Build |
|---|---|---|---|---|
| CRM tích hợp với kế toán | ✅ Native | ❌ Riêng | Hạn chế | Cần build |
| Quản lý nhân sự + lương | ✅ Native | ❌ | Hạn chế | Cần build |
| Phân quyền theo vai trò | ✅ Chi tiết | Hạn chế | ✅ | Phức tạp |
| Tùy chỉnh cho đặc thù GD | ✅ Module | Hạn chế | Hạn chế | Hoàn toàn |
| Cộng đồng VN & đối tác | ✅ Lớn | Nhỏ | Trung bình | N/A |
| Chi phí quy mô ~30 user | Trung bình | Cao | Thấp-Trung | Cao nhất |
| Scale lên 2028 | ✅ Tốt | Tốt | Hạn chế | Tốt |
| Open source / tự host | ✅ Community | ❌ | Hạn chế | ✅ |

**Kết luận:** Odoo là lựa chọn phù hợp cao cho quy mô và bài toán của HSA, nhưng không phải vì Odoo hoàn hảo. Odoo phù hợp vì nó gom được CRM, Sales, Accounting, HR, Project, Helpdesk, Documents và reporting vào cùng một database, trong khi vẫn cho phép tích hợp ClassIn, SePay và Zalo OA.

### 1.3 Fit-gap cấp điều hành

| Nhu cầu từ hiện trạng | Mức phù hợp Odoo | Nhận định |
|---|---|---|
| Single source of truth cho học sinh, đơn hàng, thanh toán | **Cao** | Odoo Contacts + Sales + Accounting là lõi phù hợp. |
| CRM thay EZSale và chống rơi lead | **Cao** | Odoo CRM phù hợp để thay EZSale nếu form web được tích hợp tự động. |
| Giám sát, hỗ trợ và QA tư vấn Sale/CTV | **Cao nhưng cần thiết kế quy trình** | Odoo CRM Activities/Tasks/Tags có thể tạo review queue, checklist QA và playbook case; cần cấu hình taxonomy và quyền quản lý rõ. |
| Tự động hóa payment → SBD → onboarding | **Cao nhưng cần custom** | Cần SePay connector, sequence SBD và workflow Zalo/ClassIn. |
| Dữ liệu học tập và trigger chăm sóc | **Trung bình-cao, phụ thuộc ClassIn** | Odoo không tạo data học tập; ClassIn là nguồn dữ liệu, Odoo xử lý workflow. |
| Zalo OA/ZNS | **Thấp nếu dùng native, tốt nếu qua middleware** | Odoo Email/SMS không thay Zalo OA; cần n8n/Make hoặc custom connector. |
| Tính thù lao GV và hoa hồng CTV | **Trung bình-cao** | Native Accounting/Payroll hỗ trợ nền tảng; logic đặc thù cần custom. |
| Quản trị HN-HCM và phân quyền | **Cao** | Single company + branches/tags phù hợp hơn multi-company giai đoạn đầu. |
| Triển khai nhanh toàn tổ chức | **Thấp** | Không nên big-bang. Cần rollout theo module và readiness gate. |

### 1.4 Ba điều Odoo KHÔNG thay thế được — phải tích hợp

> Đây là điểm quan trọng nhất cần hiểu trước khi bắt đầu.

| Hệ thống | Lý do không thể thay thế | Cách xử lý |
|---|---|---|
| **ClassIn** | Nền tảng học trực tiếp (live class) chuyên biệt; không có sản phẩm nào thay thế được cho thị trường VN/CN | Tích hợp ClassIn API → Odoo; ClassIn là nguồn data, Odoo là trung tâm quản lý |
| **SePay** | Cổng thanh toán nội địa; Odoo không có sẵn connector | Custom payment provider module |
| **Zalo OA** | Kênh giao tiếp chính với học sinh VN; Odoo SMS/Email không thay thế được Zalo trong bối cảnh VN | Middleware (n8n hoặc Make) bridge Odoo trigger → Zalo OA API |

---

## II. BẢN ĐỒ CHUYỂN ĐỔI — Quy trình hiện tại → Odoo

Báo cáo hiện trạng mô tả 9 luồng nghiệp vụ đang vận hành. Trong kiến trúc mục tiêu, 9 luồng này được gom lại thành 7 value streams để giảm trùng lặp và làm rõ ownership: acquisition, nurture/close, payment/onboarding, learning data, student care, instructor operations, và collaborator/commission.

### 2.1 Mapping tổng thể

| Công cụ/Quy trình hiện tại | Module Odoo tương ứng | Mức độ phù hợp | Cần tùy chỉnh / tích hợp |
|---|---|---|---|
| EZSale CRM | **Odoo CRM** | ★★★★★ | Ít |
| Review tư vấn Sale/CTV thủ công trên CRM | **Odoo CRM Activities + Knowledge/Helpdesk** | ★★★★☆ | Cấu hình QA checklist, case taxonomy, review queue |
| Google Sheet (học sinh) | **Odoo Contacts + Sales** | ★★★★☆ | Thêm custom fields |
| Google Sheet (GV giờ dạy) | **Odoo HR + Timesheets** | ★★★★☆ | Kết nối ClassIn data |
| Google Sheet (CTV hoa hồng) | **Odoo Accounting + custom** | ★★★☆☆ | Module hoa hồng riêng |
| Thanh toán SePay | **Odoo Accounting** | ★★★☆☆ | Custom SePay connector |
| Kế toán thủ công (3 người) | **Odoo Accounting** | ★★★★★ | Localization VN |
| Google Drive (file cá nhân) | **Odoo Documents** | ★★★★☆ | Ít |
| Zalo nội bộ | **Odoo Discuss** | ★★★☆☆ | Thay đổi thói quen |
| Zalo OA (học sinh) | Không thay thế — Middleware | ★★☆☆☆ | n8n/Make connector |
| Dashboard (3 loại) | **Odoo Reporting + BI** | ★★★★☆ | Cấu hình views |
| Ticket sự cố | **Odoo Helpdesk** | ★★★★★ | Ít |
| Lịch dạy GV | **Odoo Project + Calendar** | ★★★☆☆ | Kết nối ClassIn |
| ClassIn (học tập) | Không thay thế — tích hợp | Tích hợp | ClassIn connector |

**Điểm cần chốt:** Nếu HSA cần custom module và API hai chiều với ClassIn/SePay/Zalo, phương án đánh giá chính phải là **Odoo Custom/Enterprise trên Odoo.sh hoặc on-premise**, không phải gói Standard thuần Odoo Online.

### 2.2 Luồng vận hành → Module Odoo

```
LUỒNG 1: Lead Acquisition
Form web → Odoo CRM (Lead) → Sales Team theo kỳ thi
                [Hiện tại: form → EZSale một phần thủ công]

LUỒNG 2: Nurture & Close
Odoo CRM Pipeline → Odoo Email Marketing (sequence theo kỳ thi)
→ Zalo OA qua Middleware
→ CRM QA Queue: case khó / tư vấn sai / cần coaching → quản lý review
                [Hiện tại: EZSale + Zalo thủ công; review tư vấn làm tay từng case]

LUỒNG 3: Payment → Onboarding
SePay webhook → Odoo Accounting (Invoice paid)
→ Odoo Automation: tạo SBD, trigger Zalo OA, trigger ClassIn API
→ Odoo Contact: tạo học sinh record đầy đủ
                [Hiện tại: thủ công hoàn toàn]

LUỒNG 4: Học tập (ClassIn)
ClassIn Data Subscription → Custom Connector → Odoo
→ Odoo stores: attendance, scores, login activity
→ Odoo Dashboard: 3 views (QLL / Ban ĐH / GV)
                [Hiện tại: ClassIn đang thay Zoom, nhưng chưa tích hợp API/data sâu]

LUỒNG 5: Chăm sóc học viên
Odoo Automation (từ ClassIn data) → Trigger Email/Zalo OA
→ Odoo Helpdesk (nếu cần QLL xử lý)
                [Hiện tại: thủ công, phản ứng]

LUỒNG 6: Giảng viên
Odoo HR (hồ sơ GV) + Timesheets (từ ClassIn data)
→ Odoo Payroll: tự động từ giờ ClassIn × đơn giá
                [Hiện tại: Sheet thủ công]

LUỒNG 7: CTV & Đại sứ
Odoo Contacts (hồ sơ CTV) + Custom Commission Module
→ SePay webhook có ref_code → Odoo tự cộng hoa hồng
→ Odoo Payroll/Purchase: thanh toán cuối tháng
                [Hiện tại: Zalo + Sheet thủ công]
```

---

## III. PHÂN TÍCH CHI TIẾT TỪNG MODULE

### 3.1 Odoo CRM — Thay thế EZSale

**Đánh giá phù hợp: ★★★★★ — Thay thế hoàn toàn, không cần EZSale nữa**

EZSale là CRM nội địa có tính năng cơ bản. Odoo CRM vượt trội ở tích hợp với toàn bộ hệ thống còn lại (Accounting, Sales, Email Marketing, Helpdesk cùng một database).

**Cấu hình cho HSA:**

```
Sales Teams (theo kỳ thi):
├── Team HSA — Sale phụ trách ĐGNL HSA
├── Team BCA — Sale phụ trách ĐGNL Bộ Công An
├── Team BQP — Sale phụ trách ĐGNL Bộ Quốc Phòng
└── Team HCM — Sale phụ trách ĐGNL HCM

Pipeline stages (chuẩn cho tất cả teams):
New Lead → Đã liên hệ → Đang tư vấn → Hot → Chốt đơn → [Thất bại]

Tags bắt buộc trên Lead:
├── Kỳ thi: [HSA | BCA | BQP | HCM]
├── Cơ sở: [HN | HCM]
├── Nguồn: [Organic | CTV | Tuyến đi trường | Đại sứ]
├── CTV_code: [ref code nếu có]
├── Case tư vấn: [Học phí | Chọn khóa | Lịch học | So sánh đối thủ | Hoàn tiền | Phụ huynh phản đối | Khác]
└── QA_status: [Chưa review | Cần quản lý hỗ trợ | Đã review | Đưa vào playbook]
```

**Automation trong Odoo CRM:**
- Khi lead mới vào → tự assign vào đúng Sales Team theo kỳ thi + cơ sở
- Khi chuyển stage → trigger Email Marketing sequence
- Khi ở "Đã liên hệ" > 48h không hoạt động → cảnh báo Sale phụ trách
- Khi lead được tag "Cần quản lý hỗ trợ" → tạo Activity cho Sale Manager/CTV Manager review
- Khi case tư vấn lặp lại nhiều lần → tạo task cập nhật playbook / FAQ nội bộ
- Khi "Chốt đơn" → tự tạo Sales Order → trigger invoice

**QA tư vấn & coaching Sale/CTV:**

| Thành phần | Thiết kế trong Odoo |
|---|---|
| Review queue | CRM filter: lead/case có `QA_status = Cần quản lý hỗ trợ` hoặc lead giá trị cao chưa có next activity |
| Checklist QA | Trường bắt buộc: nhu cầu HS, kỳ thi, học lực, phản đối chính, hướng xử lý, kết quả tư vấn |
| Case taxonomy | Tag chuẩn cho các tình huống lặp lại: học phí, lịch học, chọn khóa, phụ huynh phản đối, so sánh đối thủ, hoàn tiền |
| Coaching note | Manager ghi feedback ngay trên lead/activity, có owner và deadline follow-up |
| Playbook | Odoo Knowledge/Documents lưu hướng dẫn xử lý case; link playbook gắn lại vào lead/case tương ứng |
| Dashboard | Số case cần review, số case đã review, lỗi tư vấn lặp lại theo Sale/CTV, thời gian manager phản hồi |

**Tùy chỉnh cần thiết:**
- Thêm custom field `exam_type` và `ref_code` vào Lead form
- Thêm custom fields `case_type`, `qa_status`, `manager_review_note`, `playbook_link`
- Webhook nhận lead từ web portal HSA
- Tích hợp form `hsavnu.edu.vn` → Odoo API

---

### 3.2 Odoo Sales — Quản lý Học viên & Đơn hàng

**Đánh giá phù hợp: ★★★★☆**

Odoo Sales quản lý Sales Order (đơn hàng) = enrollment. Mỗi học sinh có 1 Customer record và 1+ Sales Orders (mỗi khóa đăng ký là 1 order).

**Cấu trúc sản phẩm (Products) cho HSA:**

```
Product Categories:
├── ĐGNL HSA
│   ├── Khóa Toán Nâng Cao HSA (giá X)
│   ├── Khóa Văn Cơ Bản HSA (giá Y)
│   └── Khóa Combo HSA (giá Z)
├── ĐGNL Bộ Công An
│   ├── Khóa Toán BCA
│   └── Khóa Combo BCA
├── ĐGNL Bộ Quốc Phòng
│   └── ...
└── ĐGNL HCM
    └── ...
```

**Custom fields trên Customer (Partner) cho học sinh:**

| Field | Kiểu dữ liệu | Ghi chú |
|---|---|---|
| `student_sbd` | Char | Auto-generated sau khi payment |
| `exam_type` | Selection | HSA / BCA / BQP / HCM |
| `cohort` | Char | Đợt học (VD: HSA-2026-D1) |
| `classin_uid` | Integer | UID trong ClassIn |
| `classin_course_id` | Integer | Lớp đang học |
| `qll_assigned` | Many2one (HR) | QLL phụ trách |
| `enrollment_date` | Date | Ngày thanh toán |
| `zalo_phone` | Char | SĐT Zalo (để gửi ZNS) |

**Automation sau khi Sales Order confirmed (= học sinh đã thanh toán):**
1. Auto-generate `student_sbd` (theo format `[KỲ_THI]-[NĂM]-[SEQ]`)
2. Tạo invoice → mark paid (từ SePay webhook)
3. Trigger: gửi Zalo OA qua middleware
4. Trigger: gọi ClassIn API (register + enroll)
5. Trigger: tạo task trong Odoo Project cho QLL

---

### 3.3 Odoo Accounting — Kế toán 3 người

**Đánh giá phù hợp: ★★★★★ — Thay đổi lớn nhất, lợi ích lớn nhất**

Hiện tại kế toán 3 người đang xử lý thủ công: đối soát SePay, tính thù lao GV từ Sheet, tính hoa hồng CTV từ Zalo. Odoo Accounting tích hợp tất cả vào 1 hệ thống.

**Cấu hình cho HSA:**

*Analytic Accounting (bắt buộc — để phân tích P&L theo chiều):*
```
Analytic Dimensions:
├── Theo Kỳ thi: HSA / BCA / BQP / HCM
├── Theo Cơ sở: HN / HCM
└── Theo Loại chi phí: GV / CTV / Marketing / Vận hành
```

*Journals (sổ nhật ký):*
- SePay Bank Journal (kết nối webhook)
- Payroll Journal (lương nhân sự + thù lao GV)
- CTV Commission Journal (hoa hồng cuối tháng)

*SePay Integration (custom module):*
```
SePay Webhook → Odoo API endpoint:
  POST /api/sepay/payment_confirm
  {transaction_id, amount, student_order_id}
  → Odoo: tìm invoice → mark paid → trigger automation chain
```

**Lợi ích cụ thể cho kế toán 3 người:**

| Công việc hiện tại | Với Odoo |
|---|---|
| Đối soát SePay với đơn hàng: ~2h/ngày | Auto-match: ~5 phút review |
| Tổng hợp thù lao GV từ nhiều Sheet: ~1 ngày/tháng | Payroll batch: ~30 phút review |
| Tính hoa hồng mạng lưới Sale/CTV ~132–137 người: ~2 ngày/tháng | Auto-tính từ confirmed orders: ~1 giờ review |
| Báo cáo tài chính theo kỳ thi: không có | Dashboard realtime |

**Lưu ý:** Cần cài module kế toán Việt Nam (Vietnamese Chart of Accounts + tax configuration). Các Odoo partner VN đều cung cấp module localization này.

---

### 3.4 Odoo HR & Payroll — Quản lý Nhân sự

**Đánh giá phù hợp: ★★★★☆**

**Phân loại nhân sự trong Odoo HR:**

| Loại | Odoo Record | Quản lý qua |
|---|---|---|
| 62 nhân sự fulltime/offline | **Employee** (có hợp đồng) | Odoo HR + Payroll |
| ~70 GV online + 15 GV chính HCM | **Employee** hoặc **Vendor** | Odoo HR (nếu dài hạn) hoặc Purchase (nếu invoice) |
| Mạng lưới Sale/CTV ~132–137 người | **Vendor** hoặc **Employee** tùy loại hợp đồng | Odoo Purchase + custom Commission; cần tách fulltime/CTV HCM |
| Marketing HCM 20 người | **Employee** hoặc **Vendor** | Odoo HR/Documents/Project; gồm cả fulltime và CTV |
| 8 Đại sứ | **Vendor** hoặc **Contact** | Tương tự CTV |

**Payroll cho GV — cấu hình đặc thù:**

Odoo Payroll chuẩn tính lương theo tháng cố định. Với GV thỉnh giảng tính theo giờ, cần:

```
Odoo Payslip Rule (custom):
  Thù lao GV = Số buổi đã dạy (từ ClassIn data) × Đơn giá/buổi
               + Phụ cấp (nếu có)

Source of truth: ClassIn teaching log → Odoo Timesheet → Payslip
```

Hoặc đơn giản hơn: GV submit invoice (Purchase Bill) dựa trên ClassIn data, kế toán review và approve → tránh phải custom phức tạp.

**Recruitment module (tùy chọn, hữu ích cho HCM đang tuyển):**
- Job positions theo department
- Application pipeline: CV → Phỏng vấn → Offer → Onboard
- Onboarding checklist tự động cho nhân sự mới

---

### 3.5 Odoo Project — Quản lý QLL & Vận hành lớp

**Đánh giá phù hợp: ★★★★☆**

8 QLL hiện đang quản lý công việc qua Zalo và Google Sheet. Odoo Project thay thế bằng task management có cấu trúc, có deadline, có người chịu trách nhiệm rõ ràng.

**Cấu trúc Project cho QLL:**

```
Projects:
├── [QLL] Onboarding HN — 2026
│   Tasks (1 task = 1 học sinh mới):
│   ├── Stage: Chờ xử lý
│   ├── Stage: Zalo OA đã gửi
│   ├── Stage: ClassIn đã tạo
│   ├── Stage: Đã đăng nhập ClassIn
│   └── Stage: Hoàn tất ✓ (hoặc Flag đỏ)
│
├── [QLL] Onboarding HCM — 2026
│   (Tương tự HN)
│
├── [QLL] Chăm sóc học viên — HSA
├── [QLL] Chăm sóc học viên — BCA
├── [QLL] Chăm sóc học viên — BQP
└── [QLL] Chăm sóc học viên — HCM
```

**Automation khi học sinh thanh toán:**
- Odoo tự tạo Task trong Project "Onboarding HN/HCM" với stage "Chờ xử lý"
- Gán QLL phụ trách theo mapping `lớp → QLL`
- Deadline: 24h để hoàn tất onboarding
- QLL nhận thông báo trong Odoo Discuss

**Tích hợp ClassIn data vào Project:**
- Khi ClassIn push "student không login 3 ngày" → Odoo auto-tạo Task cho QLL: "Gọi điện [tên HS]"
- Khi "vắng 2+ buổi" → Task khẩn với priority cao

---

### 3.6 Odoo Helpdesk — Hỗ trợ học viên & Xử lý sự cố

**Đánh giá phù hợp: ★★★★★ — Thay thế hoàn toàn việc theo dõi sự cố qua Zalo**

**Cấu hình Helpdesk cho HSA:**

```
Teams:
├── Support Học viên (QLL xử lý — SLA: 2h phản hồi, 24h giải quyết)
│   Loại ticket: Không vào ClassIn | Hỏi lịch học | Khiếu nại
├── Kỹ thuật (Tech Ops xử lý — SLA: 15 phút phản hồi, 2h giải quyết)
│   Loại ticket: ClassIn lỗi | SePay lỗi | Web portal lỗi
├── Tài chính (Kế toán xử lý — SLA: 4h phản hồi)
│   Loại ticket: Hoàn tiền | Sai học phí | Hoa hồng CTV
└── HCM Sự cố (GĐ VH Nam xử lý)
    Loại ticket: Sự cố tại HCM
```

**Tại sao Helpdesk quan trọng hơn nghĩ:**
- Hiện tại sự cố báo qua Zalo → không có lịch sử → không biết mẫu nào lặp lại nhiều nhất
- Odoo Helpdesk: mọi sự cố có ticket, có thời gian xử lý, có báo cáo → COO biết SLA có được tuân thủ không, vấn đề nào cần cải thiện SOP

---

### 3.7 Odoo Email Marketing — Chuỗi chăm sóc tự động

**Đánh giá phù hợp: ★★★★☆ (cho Email) — ★★☆☆☆ (không thay được Zalo OA)**

Odoo Email Marketing mạnh cho automated email sequences. Nhưng học sinh VN phản hồi tốt hơn với Zalo. Giải pháp: **dùng cả hai** — Odoo quản lý logic trigger, Zalo OA thực thi giao tiếp với học sinh.

**Mailing Lists trong Odoo:**

```
Danh sách:
├── Lead HSA — HN (nurture trước khi mua)
├── Lead BCA — HN
├── Lead BQP — HN
├── Lead HCM — HCM
├── Học sinh đang học — HSA (chăm sóc trong khóa)
├── Học sinh đang học — BCA
├── Học sinh đang học — BQP
├── Học sinh đang học — HCM
└── Phụ huynh (nhận báo cáo tiến độ hàng tháng)
```

**Automated mailings (Odoo native):**
- Khi lead chuyển stage → email nurture theo kỳ thi
- Khi thanh toán thành công → email onboarding guide
- D-30 trước kỳ thi → email kế hoạch ôn tập
- D-7 → email thông tin phòng thi
- Sau kết quả → email báo cáo phân tích

**Zalo OA cho real-time communication (qua middleware):**
- Odoo trigger (từ automation) → n8n/Make → Zalo OA API
- Các tin nhắn ZNS (xác nhận đăng ký, SBD, nhắc nhở) vẫn qua Zalo OA
- Odoo ghi log: đã trigger gửi Zalo OA hay chưa

---

### 3.8 Odoo Documents — Thay thế Drive cá nhân

**Đánh giá phù hợp: ★★★★☆**

Odoo Documents là document management system với phân quyền theo folder, workflow approve, và tích hợp với các module khác (ký hợp đồng GV từ Odoo HR, lưu tài liệu học từ ClassIn).

**Lưu ý quan trọng:** Không nên migrate toàn bộ khỏi Google Workspace nếu Workspace vẫn là lớp cộng tác tài liệu hằng ngày. Thay vào đó:
- **Google Drive**: vẫn dùng cho collaboration nội bộ (editing real-time, Google Docs/Sheets)
- **Odoo Documents**: quản lý hợp đồng, SOP, tài liệu cần workflow approve, file gắn vào record (hồ sơ GV, hợp đồng CTV)
- **Tích hợp**: Google Drive connector của Odoo (Community/Enterprise đều có)

---

## IV. KIẾN TRÚC TÍCH HỢP — ClassIn, SePay, Zalo OA

### 4.1 ClassIn Integration — Trung tâm của toàn bộ data học tập

```
ClassIn (nguồn dữ liệu)
       │
       │ 1. ClassIn Data Subscription
       │    (push tới HSA endpoint khi có sự kiện)
       │
       ▼
Custom Odoo Module: classin_connector
       │
       │ Nhận và parse các sự kiện:
       │  - Điểm danh: entry_time, exit_time, student_uid
       │  - Bài tập: score, total, submitted_at, student_uid
       │  - Login: last_login, student_uid
       │  - Hoàn thành module: completion_rate, student_uid
       │
       ▼
Odoo Database:
  model: classin.attendance (log điểm danh)
  model: classin.score (log bài tập)
  model: classin.activity (log login)
       │
       │ Odoo Automation Rules (triggered từ data mới):
       │  "Nếu student không login 3 ngày → tạo Task QLL"
       │  "Nếu score < threshold → trigger Zalo OA"
       │  "Nếu vắng buổi → trigger Zalo OA + tạo Task"
       │
       ▼
3 Dashboard (Odoo BI / Looker Studio):
  QLL View | Ban ĐH View | GV View
```

**ClassIn API calls từ Odoo (chiều ngược lại):**
```
Khi Sales Order confirmed (học sinh đã thanh toán):
Odoo → classin_connector → ClassIn API:
  1. action=register (tạo tài khoản)
  2. action=addSchoolStudent
  3. action=addCourseStudent (lookup từ bảng mapping)
```

**Bảng mapping trong Odoo (thay vì Google Sheet):**
```
Model: hsa.class.mapping
  fields:
    hsa_course_code   → Char (mã khóa học trong Odoo)
    classin_course_id → Integer
    classin_gv_uid    → Integer (GV chính)
    qll_user_id       → Many2one(res.users)
    exam_type         → Selection
    branch            → Selection (HN/HCM)
```

**Ước tính công phát triển ClassIn connector:** 3–5 tuần developer.

---

### 4.2 SePay Integration

```
Học sinh thanh toán trên web portal (SePay)
       │
       ▼ SePay Webhook POST
Custom endpoint: /api/sepay/webhook
  Payload: {transaction_id, amount, order_reference, timestamp}
       │
       ▼ Odoo xử lý:
  1. Tìm Sales Order theo order_reference
  2. Tạo Payment record
  3. Match với Invoice → mark paid
  4. Trigger Odoo Automation: onboarding chain
       │
       ▼ Onboarding chain:
  - Auto-generate SBD
  - Call ClassIn API (register + enroll)
  - Trigger Zalo OA (qua middleware)
  - Trigger Email Marketing
  - Create Project Task cho QLL
  - Nếu ref_code: cộng commission pending cho CTV
```

**Ước tính công phát triển SePay connector:** 3–5 ngày developer.

---

### 4.3 Zalo OA Integration — Thách thức lớn nhất

Zalo OA không có Odoo module native. Cần middleware layer.

**Option A — n8n (khuyến nghị):**

```
Odoo Webhook (khi trigger condition đạt)
       │
       ▼
n8n (self-hosted hoặc cloud)
  Workflow: nhận Odoo event → call Zalo OA API → log kết quả về Odoo
       │
       ▼
Zalo OA API → ZNS hoặc message tới học sinh
```

- n8n: open source, self-hosted được, chi phí thấp (~5-10 triệu/năm nếu dùng cloud)
- Không cần custom code nhiều — dùng GUI workflow builder
- Nhược điểm: thêm 1 layer phụ thuộc; n8n phải uptime 24/7

**Option B — Make (Integromat):**
- Tương tự n8n nhưng cloud-only, đắt hơn (~15-25 triệu/năm)
- Dễ dùng hơn, ít cần kỹ thuật

**Option C — Custom Odoo module gọi Zalo OA API trực tiếp:**
- Chỉ cần Odoo → Zalo OA, không cần middleware
- Phức tạp hơn về dev, nhưng ít layer
- Ước tính: 2–3 tuần developer

**Khuyến nghị:** Option A (n8n self-hosted). Linh hoạt, chi phí thấp, cộng đồng lớn.

**Log Zalo OA trong Odoo:**
```
Mỗi lần trigger Zalo OA → ghi vào Odoo:
  model: hsa.zalo.log
    student_id, message_type, sent_at, status (success/failed)
→ QLL Dashboard hiển thị: "Zalo OA đã gửi lúc HH:MM"
```

---

### 4.4 Lưu ý cập nhật Odoo 19 về API, webhook và hosting

Các tích hợp ClassIn, SePay và Zalo khiến lựa chọn gói/hosting trở thành quyết định kiến trúc, không chỉ là quyết định chi phí.

| Điểm cập nhật từ Odoo chính thức | Tác động với HSA |
|---|---|
| External API chỉ khả dụng trên gói **Custom**, không phải Standard. | HSA không nên lập ngân sách theo Standard nếu muốn tích hợp hai chiều với ClassIn/SePay/Zalo hoặc dùng custom module. |
| Odoo 19 có External JSON-2 API; XML-RPC/JSON-RPC cũ được Odoo thông báo sẽ bị loại bỏ ở các phiên bản tương lai. | Connector mới nên thiết kế theo API mới/đường nâng cấp rõ, tránh viết mới dựa hoàn toàn vào RPC cũ. |
| Odoo Studio hỗ trợ webhooks/automated actions, nhưng tài liệu Odoo khuyến nghị test trên database duplicate trước khi dùng live. | SePay webhook, Zalo trigger và automation onboarding phải có sandbox/staging, logging và rollback plan. |
| Odoo.sh hosting không nằm trong giá subscription tiêu chuẩn. | Nếu triển khai custom module production trên Odoo.sh, cần tách chi phí Odoo subscription và Odoo.sh/hosting. |

**Kết luận kiến trúc:** Giai đoạn đầu có thể dùng Odoo Online Custom cho sandbox và cấu hình business process. Khi đưa custom connector ClassIn/SePay/Zalo vào production, cần đánh giá nghiêm túc Odoo.sh hoặc on-premise Enterprise để kiểm soát module, môi trường staging và release.

---

## V. THIẾT LẬP ODOO CHO 4 KỲ THI + 2 CƠ SỞ

### 5.1 Cấu trúc công ty và chi nhánh

**Khuyến nghị: Single Company, Multiple Branches (không phải Multi-company)**

| Cách tiếp cận | Single Company | Multi-company |
|---|---|---|
| Setup | Đơn giản | Phức tạp |
| Báo cáo tổng hợp | Dễ | Cần inter-company |
| Phân tách dữ liệu | Qua Analytic | Hoàn toàn riêng |
| Phù hợp khi | HN và HCM cùng chính sách | Muốn 2 pháp nhân riêng |
| **Chọn cho HSA** | **✅ Phù hợp** | Không cần thiết |

**Analytic Accounts (phân tích theo chiều):**
```
Company: HSA Education
  Analytic Dimensions:
  ├── Branch: HN / HCM
  ├── Exam Type: HSA / BCA / BQP / HCM
  └── Cost Center: Sale / QLL / GV / Truyền thông / Admin
```

### 5.2 Phân quyền người dùng (User Access Rights)

| Vai trò | Quyền trong Odoo |
|---|---|
| HĐQT | Read-only tất cả modules + Full Reporting |
| GĐ Vận hành | Full access CRM + Sales + HR + Project + Helpdesk + Reporting |
| Kế toán trưởng | Full Accounting + Payroll; Read HR |
| Kế toán thu/chi | Accounting (giới hạn) |
| Sale Manager | Full CRM + Read Sales |
| Sale HN/HCM (HN 11 sale + HCM 20–25 người) | CRM — chỉ xem lead của mình |
| QLL Lead | Full Project + Read Sales + Helpdesk |
| QLL CTV | Project — chỉ task được assign |
| Trưởng phòng truyền thông | Documents + Email Marketing (kỳ thi của mình) |
| Tech Ops | System Admin |

### 5.3 SBD Auto-generation

```python
# Odoo sequence configuration
# Tạo 4 sequences (1 per exam type):

Sequence HSA:
  prefix: HSA-%(year)s-
  padding: 5
  → HSA-2026-00001, HSA-2026-00002, ...

Sequence BCA:
  prefix: BCA-%(year)s-
  → BCA-2026-00001, ...

Sequence BQP:
  prefix: BQP-%(year)s-
  → BQP-2026-00001, ...

Sequence HCM:
  prefix: HCM-%(year)s-
  → HCM-2026-00001, ...
```

Khi Sales Order confirmed: `student_sbd = sequence.next_by_code('hsa.sbd.' + exam_type)`

---

## VI. DỮ LIỆU CẦN DI CHUYỂN (Data Migration)

### 6.1 Inventory dữ liệu hiện tại

| Nguồn dữ liệu | Khối lượng ước tính | Chất lượng | Độ khó migrate |
|---|---|---|---|
| Danh sách học sinh (Google Sheet) | ~20.000 records/năm | Trung bình (thiếu chuẩn) | ★★★ |
| Hồ sơ GV (Zalo/Sheet) | ~70 GV online + 15 GV chính HCM | Thấp (rải rác) | ★★ |
| Hồ sơ Sale/CTV (Sheet) | ~132–137 người | Trung bình | ★★ |
| Lịch sử thanh toán (SePay) | Theo năm | Tốt (SePay API) | ★★ |
| Lịch sử liên lạc (Zalo) | Không chuẩn hóa | Xấu | Không migrate — bỏ |
| EZSale leads | Tùy thuộc EZSale export | Trung bình | ★★ |
| Google Drive files | Hàng nghìn file | Hỗn loạn | ★★★★ |

### 6.2 Quy trình chuẩn bị data trước khi import

```
Bước 1 — Chuẩn hóa dữ liệu học sinh:
  - Thống nhất format SĐT: 10 số, không dấu cách
  - Chuẩn hóa tên: UPPERCASE họ tên
  - Bổ sung trường: exam_type, cohort, enrollment_date
  - Loại bỏ duplicate (theo SĐT)

Bước 2 — Chuẩn hóa hồ sơ GV:
  - Tạo template Google Sheet chuẩn
  - Thu thập: CCCD, tài khoản ngân hàng, email, môn dạy, kỳ thi phụ trách

Bước 3 — Chuẩn hóa hồ sơ CTV:
  - Tạo ref_code duy nhất cho từng CTV
  - Thu thập: CCCD, tài khoản ngân hàng, lịch sử giới thiệu (nếu có)

Bước 4 — Import vào Odoo:
  - Dùng Odoo import CSV (built-in)
  - Kiểm tra: count records trước và sau import
  - Validate: chạy duplicate check sau import
```

**Lưu ý nghiêm trọng:** Không import dữ liệu bẩn vào Odoo. Dữ liệu bẩn vào ERP sẽ ảnh hưởng accounting, payroll, báo cáo — cực kỳ khó sửa sau này. Thà mất thêm 2 tuần chuẩn hóa data trước.

---

## VII. RỦI RO TRIỂN KHAI

| # | Rủi ro | Mức độ | Biện pháp |
|---|---|---|---|
| R1 | **Không có người dùng** — Nhân viên không dùng Odoo, quay về Zalo/Sheet (bài học Myxteam) | **Nghiêm trọng** | Mandate từ lãnh đạo; training đầy đủ; quick wins sớm; không chấp nhận "chạy song song mãi mãi" |
| R2 | **Partner Odoo yếu** — Chọn sai nhà triển khai → configure sai, customize sai, tốn tiền không hiệu quả | **Cao** | Vetting kỹ (xem portfolio, nói chuyện với khách hàng cũ); hợp đồng có milestone rõ ràng |
| R3 | **Over-customization** — Custom quá nhiều → không thể nâng cấp Odoo version sau | **Cao** | Ưu tiên dùng native features; custom chỉ khi thực sự cần; document tất cả custom code |
| R4 | **Data migration thất bại** — Import dữ liệu bẩn → accounting sai, báo cáo sai | **Cao** | Chuẩn hóa data TRƯỚC import; validation nghiêm ngặt; không migrate lịch sử Zalo |
| R5 | **ClassIn integration phức tạp** — Dev ước tính sai → delay toàn bộ lịch | **Trung bình** | Buffer 50% thời gian cho ClassIn connector; bắt đầu từ data export thủ công nếu cần |
| R6 | **Zalo OA ngắt kết nối** — ZNS policy Zalo thay đổi → middleware bị ảnh hưởng | **Trung bình** | Không phụ thuộc 100% vào Zalo ZNS; luôn có fallback Email |
| R7 | **Triển khai trong khi operations đang chạy** — Go-live ngay lúc khai giảng HCM → rủi ro cao | **Cao** | Lên lịch go-live tránh spike khai giảng; chạy parallel tối thiểu 4 tuần |
| R8 | **Chi phí vượt dự toán** — Custom modules + import + training + server tốn hơn kế hoạch | **Trung bình** | Scope rõ ràng trong hợp đồng; agile từng module (không big-bang) |

---

## VIII. LỘ TRÌNH TRIỂN KHAI ODOO — 4 GIAI ĐOẠN

> **Nguyên tắc quan trọng nhất:** Không triển khai Odoo TRƯỚC khi quy trình được chuẩn hóa (SOP đã viết, ClassIn đã ổn định, dữ liệu đã sạch). Odoo không giải quyết quy trình xấu — nó phóng đại vấn đề lên.

```
Readiness track:  Baseline/SOP     Automation + ClassIn stabilization/API design    Data cleanup/SOP stable    Operating maturity
                  (Q2/2026)        (Q3/2026)                     (Q4/2026-Q1/2027)          (Q2-Q4/2027)
                       │                    │                              │                         │
Odoo timeline:         │          Giai đoạn A                    Giai đoạn B              Giai đoạn C     Giai đoạn D
                       │          (Planning)                     (Foundation)             (Tích hợp)      (Tối ưu)
                       │          Q3/2026                        Q4/2026-Q1/2027          Q2-Q3/2027      Q4/2027+
```

---

### GIAI ĐOẠN A — Lập kế hoạch & Chuẩn bị (Q3/2026, 6–8 tuần)

**Không động đến production, chỉ chuẩn bị.**

- [ ] Chọn Odoo partner: vetting 3 nhà cung cấp, xem demo, ký hợp đồng
- [ ] Chọn phiên bản/hosting: Odoo Custom/Enterprise; sandbox có thể dùng Odoo Online Custom, production custom module nên đánh giá Odoo.sh hoặc on-premise
- [ ] Setup Odoo sandbox: cấu hình thử nghiệm, không dữ liệu thật
- [ ] Define exact configuration: products, teams, pipelines, user roles (dựa trên SOP đã viết)
- [ ] Chốt integration approach: ưu tiên JSON-2 API / webhook có logging; không thiết kế mới phụ thuộc hoàn toàn vào RPC cũ
- [ ] Chuẩn bị data migration: bắt đầu chuẩn hóa dữ liệu học sinh, GV, CTV
- [ ] Phân công nội bộ: 1 người HSA là "Odoo Champion" (thường là Tech Ops)

**Output:** Sandbox đang chạy đúng theo quy trình thực tế; dữ liệu đã chuẩn; team đã được demo

---

### GIAI ĐOẠN B — Foundation Modules (Q4/2026–Q1/2027, 10–12 tuần)

**Go-live từng module theo thứ tự, không go-live tất cả một lúc.**

**Tuần 1–4: CRM (thay thế EZSale)**
- Import leads hiện có từ EZSale → Odoo CRM
- Sales Teams theo 4 kỳ thi
- Web portal form → Odoo webhook (tự động tạo lead)
- CTV link tracking → Odoo CRM ref_code
- CRM QA workflow: case taxonomy, review queue cho Sale/CTV, checklist tư vấn, playbook link
- KPI: 100% lead mới vào Odoo, không vào EZSale nữa
- KPI: 100% case cần quản lý hỗ trợ có owner, deadline và kết quả review trên CRM

**Tuần 4–8: Accounting + SePay**
- Cài Localization VN (chart of accounts, thuế)
- Custom SePay connector: webhook → auto match payment
- Import lịch sử thanh toán (tối thiểu 3 tháng gần nhất)
- Kế toán 3 người: training 2 ngày
- KPI: 100% invoice trong Odoo, đối soát SePay tự động

**Tuần 8–10: HR cơ bản**
- Import hồ sơ 62 nhân sự fulltime/offline vào Odoo HR
- Import hồ sơ ~70 GV online + 15 GV chính HCM (Employee hoặc Vendor)
- Import hồ sơ mạng lưới Sale/CTV ~132–137 người (Vendor/Employee theo loại hợp đồng)
- Hợp đồng số hóa trong Odoo

**Tuần 10–12: Documents + Email Marketing**
- Cấu trúc folder Odoo Documents cho 4 kỳ thi + 2 cơ sở
- Tích hợp Google Drive connector (không bỏ Google Drive)
- Setup mailing lists theo kỳ thi
- Automated email sequences: onboarding + D-30/D-7/D-1

---

### GIAI ĐOẠN C — Tích hợp chuyên sâu (Q2–Q3/2027, 12–16 tuần)

**Đây là giai đoạn phức tạp nhất — cần developer có kinh nghiệm.**

**Tuần 1–5: ClassIn Connector**
- Build custom module `classin_connector`
- ClassIn Data Subscription → Odoo endpoint
- Store attendance, scores, login activity
- Bảng mapping `hsa.class.mapping` trong Odoo
- Test: dữ liệu ClassIn có vào đúng Odoo record không?

**Tuần 5–8: Odoo Automation (trigger từ ClassIn data)**
- Rule: 3 ngày không login → tạo Task QLL + trigger Zalo OA
- Rule: vắng buổi → trigger Zalo OA + Task
- Rule: điểm thấp → trigger Zalo OA
- Pre-exam sequences: D-30, D-7, D-3, D-1

**Tuần 8–10: Zalo OA Middleware**
- Setup n8n (self-hosted)
- Build workflows: Odoo webhook → Zalo OA API
- Test: Zalo ZNS gửi thành công, log về Odoo
- Fallback: nếu Zalo fail → email tự động

**Tuần 10–12: Payroll tự động GV + CTV Commission**
- GV Payroll: ClassIn timesheet → Odoo Payslip
- CTV Commission: ref_code → confirmed orders → commission batch
- Kế toán review dashboard: 30 phút/tháng thay vì 2 ngày

**Tuần 12–16: 3 Dashboards hoàn chỉnh**
- QLL Dashboard: onboarding pipeline + ClassIn alerts
- Ban điều hành: tổng quan 4 kỳ thi × 2 cơ sở
- GV Dashboard: lớp mình dạy + student performance

---

### GIAI ĐOẠN D — Tối ưu & Scale (Q4/2027–2028)

- Helpdesk: SLA monitoring, sự cố tự tạo ticket
- Advanced reporting: P&L theo kỳ thi × cơ sở
- Chatbot FAQ (tích hợp Zalo OA chatbot với Odoo backend)
- Odoo version upgrade (nếu cần)
- Load test cho quy mô 2028 (x2 học sinh)

---

## IX. CHI PHÍ ƯỚC TÍNH

> Số dưới đây là ước tính để lập ngân sách, chưa phải báo giá mua hàng. Giá Odoo cần xác nhận lại tại thời điểm ký hợp đồng, theo số user trả phí, tỷ giá USD/VND, thuế, chi phí Odoo.sh/on-premise và scope custom module. Theo trang pricing chính thức Odoo kiểm tra ngày 18/05/2026, gói **Custom** là base case phù hợp hơn Standard vì HSA cần External API/custom integration.

### 9.1 Chi phí vận hành (recurring)

**Base case khuyến nghị — Odoo Custom/Enterprise + Odoo.sh hoặc hosting kiểm soát được custom module:**

| Hạng mục | Cơ sở tính | Ước tính/năm |
|---|---|---|
| Odoo Custom subscription | ~30 paid backend users × khoảng US$25.50/user/tháng nếu trả năm | ~US$9.180/năm, tương đương ~230–245 triệu VND trước VAT/tỷ giá thực tế |
| Odoo.sh / hosting production + staging | Phụ thuộc worker, storage, môi trường staging | ~20–80 triệu VND |
| n8n hoặc middleware Zalo OA | Cloud hoặc self-hosted | ~10–40 triệu VND |
| Monitoring, backup ngoài, domain/email kỹ thuật | Tùy chính sách IT | ~5–20 triệu VND |
| Bảo trì custom code / partner support | Retainer sau go-live, bugfix, minor change request | ~60–180 triệu VND |
| **Tổng recurring tham chiếu** | | **~325–565 triệu VND/năm** |

**Không nên dùng làm base case:** Odoo Standard có chi phí user thấp hơn nhưng không phù hợp nếu HSA cần External API/custom module cho ClassIn, SePay, Zalo OA hoặc commission logic.

**Option thay thế — Community/self-hosted:** Có thể giảm subscription nhưng tăng rủi ro vận hành. Chỉ nên xem xét sau khi có Head of Technology/Tech Ops đủ năng lực quản trị server, bảo mật, backup, upgrade và custom code.

### 9.2 Chi phí triển khai (one-time)

| Hạng mục | Chi phí ước tính |
|---|---|
| Discovery + solution design + fit-gap workshop | 60–120 triệu VND |
| Configuration core modules: CRM, Sales, Accounting, HR, Project/Helpdesk, Documents | 120–250 triệu VND |
| Custom module: ClassIn connector | 80–160 triệu VND |
| Custom module: SePay connector + payment reconciliation | 20–50 triệu VND |
| Middleware setup: Odoo → n8n/Make → Zalo OA + logging | 30–80 triệu VND |
| Custom logic: CTV commission + GV timesheet/payroll review | 40–100 triệu VND |
| Data migration + cleanup + duplicate handling | 40–120 triệu VND |
| Training + change management + SOP update | 30–80 triệu VND |
| UAT, staging, go-live support, rollback plan | 40–100 triệu VND |
| **Tổng one-time full scope** | **~460 triệu – 1,06 tỷ VND** |

**MVP ngân sách thấp hơn:** Nếu chỉ làm CRM + Accounting + SePay basic + onboarding automation tối thiểu trong năm đầu, ngân sách one-time có thể nằm khoảng **250–450 triệu VND**, nhưng chưa nên kỳ vọng có đủ ClassIn dashboard, Zalo automation sâu, commission tự động và payroll/timesheet hoàn chỉnh.

### 9.3 Tổng chi phí Năm 1

```
Kịch bản MVP:
  One-time:        ~250–450 triệu
  Recurring:       ~325–565 triệu/năm
  Tổng năm 1:      ~575 triệu – 1,015 tỷ VND

Kịch bản full transformation:
  One-time:        ~460 triệu – 1,06 tỷ
  Recurring:       ~325–565 triệu/năm
  Tổng năm 1:      ~785 triệu – 1,625 tỷ VND

Năm 2+:
  Recurring + bảo trì + minor enhancements: ~325–650 triệu/năm
```

### 9.4 ROI ước tính

| Tiết kiệm | Hiện tại | Sau Odoo | Tiết kiệm/năm |
|---|---|---|---|
| Nhân công onboarding thủ công | ~14h/ngày × 250 ngày × 150k/h = ~525 triệu | ~2–4h/ngày review ngoại lệ | ~375–450 triệu VND |
| Kế toán đối soát + tổng hợp | ~2h/ngày đối soát SePay + báo cáo tháng | ~15–30 phút/ngày review | ~80–120 triệu VND |
| Tính hoa hồng CTV + thù lao GV | ~3 ngày/tháng | ~0,5 ngày/tháng review batch | ~30–60 triệu VND |
| Sự cố do thiếu hệ thống | Không đo được | Giảm đáng kể | — |
| **Tổng tiết kiệm định lượng sơ bộ** | | | **~485–630 triệu/năm** |

> Payback period ước tính: **12–24 tháng với MVP đúng trọng tâm**, và **18–30 tháng với full transformation**. Con số này chỉ đáng tin sau khi HSA đo baseline chính thức cho onboarding time, reconciliation time, ticket volume, lỗi dữ liệu và tỷ lệ chuyển đổi lead.

---

## X. SO SÁNH TRƯỚC / SAU KHI CÓ ODOO

| Bài toán vận hành | Trước Odoo | Sau Odoo |
|---|---|---|
| Lead tracking | EZSale (riêng lẻ) + Zalo | Odoo CRM — 1 nơi, đầy đủ lịch sử |
| Thanh toán → SBD | 1–2 giờ (thủ công) | < 5 phút (tự động) |
| Hồ sơ học sinh | Google Sheet (cá nhân) | Odoo Contact — phân quyền, backup |
| Thù lao GV | 1 ngày/tháng (kế toán) | 30 phút review (tự động từ ClassIn) |
| Hoa hồng CTV | 2 ngày/tháng, dễ tranh chấp | Tự động từ ref_code, audit trail rõ |
| Dashboard vận hành | Không có | 3 dashboard realtime |
| Sự cố tracking | Zalo (mất lịch sử) | Odoo Helpdesk ticket, SLA monitor |
| Báo cáo tài chính | Cuối tháng (thủ công) | Realtime, theo kỳ thi × cơ sở |
| Khi nhân sự nghỉ | Mất dữ liệu theo người | Dữ liệu ở trong Odoo, không mất |
| Visibility HCM | Phải vào từng Zalo | Dashboard COO nhìn thấy ngay |

---

## XI. ĐIỀU KIỆN THÀNH CÔNG

Dưới góc nhìn của các dự án ERP thất bại phổ biến, đây là 5 điều kiện không thể thiếu:

**1. Mandate từ lãnh đạo cao nhất**
Nếu HĐQT không chỉ đạo toàn tổ chức dùng Odoo thì Odoo sẽ bị bypass như Myxteam. Không có "tự nguyện chuyển đổi" với hệ thống mới.

**2. Process trước, system sau**
Triển khai Odoo sau khi SOP tối thiểu đã viết xong và owner quy trình đã rõ, không phải trước. Odoo configure theo quy trình đã thống nhất, không dùng Odoo để ép tổ chức tự đoán lại quy trình trong lúc go-live.

**3. Người chịu trách nhiệm nội bộ**
Tech Ops (đang tuyển) phải trở thành Odoo Champion — người duy nhất có thể answer câu hỏi của nhân viên, raise bug với partner, và maintain cấu hình. Không có người này, dự án chết sau 3 tháng.

**4. Data migration nghiêm túc**
Chuẩn hóa data trước khi import. Không import vội → dữ liệu bẩn → báo cáo sai → mất niềm tin vào hệ thống.

**5. Rollout từng bước, đo lường từng bước**
Không go-live tất cả module cùng lúc. Mỗi module go-live → đo KPI → stable → mới đến module tiếp theo. Có thể kéo dài hơn nhưng tỷ lệ thành công cao hơn nhiều.

---

## XII. KHUYẾN NGHỊ CUỐI CÙNG

### Quyết định 1: Có nên dùng Odoo không?
**Có điều kiện.** Odoo là kiến trúc phù hợp cao cho HSA Education ở quy mô hiện tại và lộ trình 2028, nhưng chỉ nên triển khai khi HSA coi đây là chương trình chuyển đổi vận hành, không phải dự án cài phần mềm. Điều kiện tối thiểu: SOP đủ rõ, dữ liệu đầu vào được chuẩn hóa, có Odoo Champion nội bộ, có partner đủ năng lực, và rollout tránh các đợt khai giảng cao điểm.

### Quyết định 2: Bắt đầu khi nào?
**Không phải ngay bây giờ.** Thứ tự đúng:
```
Q2/2026: Baseline hiện trạng, SOP tối thiểu, ownership dữ liệu, chuẩn hóa tài liệu
Q3/2026: Automation onboarding hẹp, ổn định vận hành ClassIn + thiết kế API/data sync, data cleanup bắt đầu
         + ĐỒNG THỜI: Odoo Giai đoạn A (Planning, sandbox, chọn partner)
Q4/2026: Odoo Giai đoạn B (CRM go-live, Accounting)
Q2/2027: Odoo Giai đoạn C (ClassIn integration, Zalo middleware, dashboards)
```

Lý do: Nếu bắt đầu Odoo trước khi baseline, SOP, data ownership và lớp vận hành/ClassIn API đủ rõ, tổ chức sẽ gánh quá nhiều thay đổi lớn cùng lúc: chuẩn hóa tài liệu, ClassIn, automation, data cleanup và ERP. Mỗi thay đổi đều cần adaptation time. Làm song song quá rộng sẽ làm giảm xác suất go-live thành công.

### Quyết định 3: Bắt đầu với module nào?
**CRM trước tiên.** Lý do:
- Thay thế EZSale 1:1 → ít xáo trộn nhất
- Tạo quick win sớm (sale team thấy Odoo tốt hơn EZSale)
- Là foundation cho tất cả module sau (mọi thứ bắt đầu từ CRM)

### Quyết định 4: Enterprise hay Community?
**Odoo Custom/Enterprise là base case.** Standard không phù hợp làm phương án chính vì HSA cần API/custom integration. Community/self-hosted chỉ nên xem xét khi đã có năng lực Tech Ops mạnh, vì chi phí subscription thấp hơn có thể bị bù lại bằng chi phí bảo trì, bảo mật, backup và upgrade.

### Năm việc cần làm ngay:
1. **Chốt owner nội bộ** — Tech Ops/Odoo Champion phải được phân công trước khi ký hợp đồng triển khai.
2. **Đo baseline 30 ngày** — onboarding time, đối soát SePay, lỗi add lớp, lead response time, ticket/sự cố.
3. **Chuẩn hóa data học sinh** — Đây là task dài nhất và không ai làm thay được.
4. **Vetting 3 Odoo partner** — Yêu cầu demo theo đúng use case HSA: SePay, ClassIn, Zalo OA, 4 kỳ thi, 2 cơ sở.
5. **Làm sandbox trước production** — Không go-live module nào nếu chưa qua UAT, rollback plan và training.

---

## XIII. NGUỒN THAM KHẢO ODOO CHÍNH THỨC

| Nguồn | Điểm dùng trong tài liệu |
|---|---|
| [Odoo Pricing](https://www.odoo.com/pricing) | Cập nhật gói Standard/Custom, External API, định nghĩa paid user, chi phí chưa bao gồm Odoo.sh/custom code |
| [Odoo 19 — Webhooks](https://www.odoo.com/documentation/19.0/applications/studio/automated_actions/webhooks.html) | Cập nhật khả năng webhook/automated actions và yêu cầu test trước production |
| [Odoo 19 — External RPC API](https://www.odoo.com/documentation/19.0/developer/reference/external_rpc_api.html) | Cập nhật điều kiện External API và cảnh báo deprecation RPC cũ |
| [Odoo 19 — External JSON-2 API](https://www.odoo.com/documentation/19.0/developer/reference/external_api.html) | Định hướng connector mới cho tích hợp dài hạn |

---

*Phiên bản 1.1 — Q2/2026 — Fit-gap Odoo & transformation roadmap*
*Review tiếp theo: Sau khi chọn được Odoo partner và xác nhận ngân sách*
*Người chịu trách nhiệm: Giám đốc vận hành + Tech Ops (khi tuyển được)*
