# HSA Education — Tài liệu dự án

Kho tài liệu chiến lược, vận hành và kỹ thuật của **HSA Education** — trung tâm ôn luyện thi hàng đầu quốc gia (20.000+ HS/năm, 4 kỳ thi, 2 cơ sở HN + HCM).

---

## Cây thư mục

```
hsa/
├── README.md                          ← File này — chỉ mục toàn dự án
│
├── docs/                              ← Tài liệu chính thức (phê duyệt / thực thi)
│   ├── 01-HSA-BUSINESS-CASE-v1.0.md
│   ├── 02-HSA-PLATFORM-VISION-v1.0.md
│   ├── 03-HSA-SOM-v1.0-Standard-Operations-Manual.md
│   ├── 04-HSA-TECH-ROADMAP-v1.0.md
│   └── 05-HSA-CLASSIN-API-REFERENCE-v1.0.md
│
├── phan-tich-hien-trang-van-hanh-hsa-education-q2-2026.md   ← Nghiên cứu nguồn As-Is
├── danh-gia-phu-hop-odoo-va-lo-trinh-chuyen-doi-...md       ← Nghiên cứu nguồn (SUPERSEDED)
│
├── hsa-as-is-models-png/              ← Diagram PNG hiện trạng vận hành
└── HSA Education – Quy trình vận hành thực tế & lộ trình nâng cấp.html
```

---

## Tài liệu chính thức (`docs/`)

| # | Tài liệu | Mục đích | Đối tượng đọc | Trạng thái |
|---|---|---|---|---|
| 01 | [Business Case v1.0](docs/HSA-BUSINESS-CASE-v1.0.md) | Luận cứ đầu tư — thị trường, vấn đề, giải pháp, ROI | BGĐ / HĐQT | Draft for Approval |
| 02 | [Platform Vision v1.0](docs/HSA-PLATFORM-VISION-v1.0.md) | Kiến trúc nền tảng — 7 actor, Feature Matrix, lộ trình 24 tháng, AI/BigData | BGĐ + CTO | Draft for Approval |
| 03 | [SOM v1.0](docs/HSA-SOM-v1.0-Standard-Operations-Manual.md) | Sổ tay vận hành chuẩn — 7 Value Streams, 11 SOP, SLA/KPI | TP Lead + Fulltime | Approved |
| 04 | [Tech Roadmap v1.0](docs/HSA-TECH-ROADMAP-v1.0.md) | Thiết kế kỹ thuật — ADR, API contract, DB schema, EPIC/Story | CTO + Dev team | Approved for Implementation |
| 05 | [ClassIn API Reference v1.0](docs/HSA-CLASSIN-API-REFERENCE-v1.0.md) | Tài liệu kỹ thuật ClassIn API — V1/V2, Data Subscription, error codes | CTO + Dev team | Reference |

---

## Tài liệu nghiên cứu nền (root)

| File | Mục đích | Trạng thái |
|---|---|---|
| [phan-tich-hien-trang...q2-2026.md](phan-tich-hien-trang-van-hanh-hsa-education-q2-2026.md) | Phân tích As-Is Q2/2026 — 9 luồng, 14 bottleneck, headcount chi tiết | **Tham chiếu** — nguồn dữ liệu gốc cho SOM + BC |
| [danh-gia-phu-hop-odoo...md](danh-gia-phu-hop-odoo-va-lo-trinh-chuyen-doi-hsa-education-2026-2028.md) | Fit-gap Odoo (2026) | **SUPERSEDED** — quyết định không dùng Odoo; xem Platform Vision §4 |

---

## Nguyên tắc quản lý tài liệu

- **Tài liệu trong `docs/`** = tài liệu chính thức, đã qua review, đánh số thứ tự (01–05).
- **Tài liệu root** = nghiên cứu nguồn / phân tích nền, không phải tài liệu chỉ đạo.
- Mọi thay đổi quyết định chiến lược cập nhật vào `docs/` trước, sau đó commit git.
- Version control: git `main` branch — mọi thay đổi commit với message rõ ràng.

---

## Trạng thái dự án (cập nhật 2026-06-16)

| Mốc | Thời gian | Trạng thái |
|---|---|---|
| Phê duyệt Business Case | T7/2026 | Chờ BGĐ phê duyệt |
| Tuyển CTO | T7–8/2026 | Chưa bắt đầu |
| Phase 0 — Security & Foundation | T8–9/2026 | Chưa bắt đầu |
| Phase 1 — Core Automation | T10–12/2026 | Chưa bắt đầu |
| Phase 2 — Teacher + CTV + Sale Portal | T1–4/2027 | Chưa bắt đầu |
| Phase 3 — Parent + Mock Exam + Finance | T5–9/2027 | Chưa bắt đầu |
| Phase 4 — BGĐ Dashboard + Đà Nẵng* | T10/2027–T3/2028 | Chưa bắt đầu |
| Phase 5 — AI & Personalization | T4–9/2028 | Chưa bắt đầu |

> *Đà Nẵng là thị trường **giả định** — cần nghiên cứu thực địa trước khi đưa vào kế hoạch chính thức.
