# Tài Liệu Vận Hành & Lộ Trình Hệ Thống
## HSA Education — 2026–2028

---

**Loại tài liệu:** Kế hoạch vận hành tổng thể
**Phạm vi:** Vận hành thuần túy — không bao gồm chiến lược kinh doanh & marketing
**Quy mô 2026:** ~28.000–30.000 học sinh/năm | 60 nhân sự offline | ~70 GV online | ~170 CTV
**Cơ sở:** Hà Nội (chính) + Hồ Chí Minh (vận hành song song từ 2026)
**Sản phẩm:** 4 kỳ thi — ĐGNL HSA · ĐGNL Bộ Công An · ĐGNL Bộ Quốc Phòng · ĐGNL HCM
**Người soạn:** Giám đốc vận hành (COO)
**Phiên bản:** 3.0 — Q2/2026 — Rewrite toàn diện

---

## I. TÓM TẮT ĐIỀU HÀNH

HSA Education đang vận hành một chuỗi luyện thi 4 kỳ thi quốc gia với quy mô ~30.000 học sinh/năm trên 2 cơ sở, bằng đội ngũ ~300 người (fulltime + GV + CTV) — nhưng với hạ tầng vận hành chưa tương xứng: mọi bước sau thanh toán đều thủ công, không có dashboard, không có dữ liệu học tập tập trung, CTV tracking bằng Zalo.

**Thực trạng Q2/2026:** Hệ thống đang chạy được là nhờ con người bù vào chỗ mà quy trình và công nghệ chưa đến. Đây không bền vững khi scale lên 2028.

**3 ưu tiên vận hành không thể trì hoãn:**

1. **Automation onboarding sau thanh toán** — Toàn bộ chuỗi tạo SBD → gửi Zalo OA → gửi email → enroll ClassIn đang làm tay. Với 77–82 học sinh nhập học mỗi ngày, đây là điểm nghẽn nghiêm trọng nhất.

2. **Triển khai ClassIn thay Zoom + Zalo** — Không có ClassIn đồng nghĩa không có dữ liệu học tập, không có trigger chăm sóc tự động, không có dashboard vận hành. ClassIn là xương sống kỹ thuật của toàn bộ tầng chăm sóc học viên.

3. **Hạ tầng dữ liệu tổ chức** — Dữ liệu đang nằm trong Drive cá nhân nhân viên ở cả HN lẫn HCM. Mỗi lần nhân sự nghỉ là một lần rủi ro mất data không lấy lại được.

**Tầm nhìn vận hành 2027:** Một engine vận hành duy nhất phục vụ 4 kỳ thi, 2 cơ sở — tự động hóa >70% tác vụ lặp lại, mọi quyết định vận hành dựa trên dữ liệu từ ClassIn.

**Lộ trình tăng trưởng 2028:** x2 nhân sự fulltime, x1.5 CTV. Hệ thống xây năm 2026 phải được thiết kế để chịu được tải 2028 mà không cần xây lại.

---

## II. CƠ CẤU TỔ CHỨC HSA EDUCATION 2026

### 2.1 Sơ đồ tổ chức

```
                    HĐQT / Đồng sáng lập
                    (Hoa · Thầy Khương)
                            │
              ┌─────────────┴─────────────┐
              │                           │
    GĐ Vận hành Bắc              GĐ Vận hành Nam
    3 kỳ thi:                    1 kỳ thi:
    HSA · BCA · BQP              ĐGNL HCM
              │
    ┌─────────┼──────────┬──────────┬──────────┐
    │         │          │          │          │
  Kế toán   Sale       Học vụ   Truyền    Hành chính
  (chung)              & QLL     thông      & NS
```

### 2.2 Chi tiết từng bộ phận

#### Lãnh đạo

| Vị trí | Số lượng | Phạm vi |
|---|---|---|
| Đồng sáng lập / HĐQT | 2 | Hoa, Thầy Khương — quản lý toàn tổ chức |
| GĐ Vận hành Bắc | 1 | 3 kỳ thi: HSA, Bộ Công An, Bộ Quốc Phòng |
| GĐ Vận hành Nam | 1 | 1 kỳ thi: ĐGNL HCM |

#### Tài chính — Kế toán (chung toàn tổ chức)

| Vị trí | Số lượng |
|---|---|
| Kế toán thu | 1 |
| Kế toán chi | 1 |
| Kế toán tổng hợp | 1 |
| **Tổng** | **3** |

#### Sale

```
MIỀN BẮC (HÀ NỘI)
├── Phòng Sale Offline: 1 Trưởng phòng + 11 sale = 12 người
└── Đội Sale Online: 1 quản lý + ~100 CTV

MIỀN NAM (HỒ CHÍ MINH)
└── 2 sale offline + 1 quản lý vận hành = 3 người + ~30 CTV

TỔNG CTV SALE: ~130 người (100 HN + 30 HCM)
```

#### Học vụ & Quản lý lớp học

| Bộ phận | Vị trí | SL | Ghi chú |
|---|---|---|---|
| Duyệt học sinh | Chuyên viên | 1 | Xác nhận thanh toán + tạo SBD + add Zalo lớp |
| Trợ giảng | Lead | 1 | Điều phối đội trợ giảng toàn tổ chức |
| Trợ giảng | CTV / môn | 2–3 | Q&A online theo môn học |
| Quản lý lớp (QLL) | Lead | 1 | Quản lý vận hành toàn bộ lớp HN + HCM |
| Quản lý lớp (QLL) | CTV | 7 | Chạy lớp hàng ngày |

> **Tỷ lệ thực tế:** 8 QLL (1 lead + 7 CTV) quản lý ~30.000 học sinh → ~3.750 HS/người. Automation không phải tùy chọn — là điều kiện sống còn.

#### Giảng viên

| | Số lượng | Ghi chú |
|---|---|---|
| GV online (2 miền) | ~70 | Dạy remote, phục vụ cả HN và HCM |
| GV làm đề thi | Chưa xác định | Vận hành riêng biệt, không tính vào GV dạy |

#### Truyền thông — Tổ chức theo kỳ thi

> Mỗi kỳ thi: 1 Lead + 2 Content + 1 Edit + 1 Design = **5 người/kỳ thi**

| Kỳ thi | Cơ sở | Team |
|---|---|---|
| ĐGNL HSA | HN | 5 người |
| ĐGNL Bộ Công An | HN | 5 người |
| ĐGNL Bộ Quốc Phòng | HN | 5 người |
| ĐGNL HCM | HCM | 5 người |
| **Tổng** | | **20 người** |

**Tuyến đi trường:** 2 người — phụ trách hoạt động tại trường THPT, hội thảo, tư vấn trực tiếp

#### Đại sứ & Hành chính

| Vị trí | Số lượng | Ghi chú |
|---|---|---|
| Đại sứ HSA (Ambassador) | 8 | Active cả 2 miền — cựu học sinh xuất sắc |
| Hành chính — Nhân sự | 1 | Chung toàn tổ chức |

### 2.3 Tổng kết nhân sự

```
NHÂN SỰ FULLTIME / OFFLINE
─────────────────────────────────────────
HÀ NỘI                        : 50 người
HỒ CHÍ MINH                   : 10 người
Tổng offline                   : 60 người

NHÂN SỰ CTV / FREELANCE
─────────────────────────────────────────
CTV Sale HN                    : ~100
CTV Sale HCM                   :  ~30
CTV Trợ giảng (2–3/môn)       :  ~25
CTV Vận hành lớp (QLL)        :    7
Đại sứ                         :    8
Tổng CTV                       : ~170

GIÁO VIÊN ONLINE
─────────────────────────────────────────
GV online (không tính GV đề)   :  ~70

TỔNG TOÀN TỔ CHỨC              : ~300 người
```

---

## III. HIỆN TRẠNG VẬN HÀNH Q2/2026

### 3.1 Luồng vận hành thực tế — Từng bước hiện tại

**Luồng học sinh (từ tiếp cận đến vào lớp):**

```
Học sinh tiếp cận qua: Organic / CTV giới thiệu / Tuyến đi trường / Đại sứ
         │
         ▼
Web portal: hsavnu.edu.vn
  Điền form đăng ký
         │
         ▼
Thanh toán (SePay) ← Webhook đang chạy ổn định
         │
         ▼   [TẤT CẢ BƯỚC SAU LÀ THỦ CÔNG]
         │
  Nhân sự tạo SBD (số báo danh)
  → ghi vào Google Sheet
         │
  Gửi SBD + link nhóm Zalo lớp qua Zalo OA
         │
  "Duyệt học sinh": kiểm tra sheet có SBD + mã đơn hàng
  → Add học sinh vào nhóm Zalo lớp
         │
  Học sinh học qua Zoom + nhóm Zalo lớp
```

**Luồng CTV sale (attribution hiện tại):**

```
CTV giới thiệu học sinh → nhắn tin vào nhóm Zalo CTV
→ Quản lý CTV ghi nhận vào Google Sheet
→ Cuối tháng: tính hoa hồng thủ công từ Sheet
```

**Luồng giảng viên (hiện tại):**

```
GV nhận lịch dạy qua Zalo / Google Sheet
→ Dạy qua Zoom (link gửi trong nhóm Zalo lớp)
→ Cuối tháng: tổng hợp giờ dạy thủ công → Kế toán chi
```

**Luồng kế toán (hiện tại):**

```
SePay webhook ghi nhận thanh toán
→ Kế toán thu đối soát thủ công
→ Kế toán chi: xử lý thù lao GV + hoa hồng CTV thủ công
→ Kế toán tổng hợp: báo cáo thủ công
```

### 3.2 Điểm nghẽn đang hoạt động

| # | Điểm nghẽn | Tác động thực tế | Có thể automation |
|---|---|---|---|
| N1 | SBD tạo thủ công sau thanh toán | Học sinh chờ nhiều giờ mới có SBD | **Có — 100%** |
| N2 | Zalo OA gửi thủ công (SBD + link nhóm) | Delay, phụ thuộc giờ hành chính | **Có — 100%** |
| N3 | "Duyệt học sinh": 1 người, thủ công | Nghỉ phép → toàn bộ tắc | **Có — ~80%** (exception handling còn lại) |
| N4 | Add vào nhóm Zalo lớp thủ công | QLL mất thời gian, dễ nhầm lớp | Có — 1-click từ dashboard |
| N5 | CTV attribution qua Zalo + Sheet | Tranh chấp hoa hồng, tính tay sai sót | **Có — link tracking** |
| N6 | Không có dữ liệu học tập (dùng Zoom) | Không biết ai học ai nghỉ | Cần deploy ClassIn trước |
| N7 | Thù lao GV tính thủ công | 70 GV × cuối tháng = nhiều giờ kế toán | Có — ClassIn data |
| N8 | Dashboard vận hành chưa tồn tại | QLL check từng nhóm Zalo để biết tình trạng | Có — Phase 1–2 |

### 3.3 Quy mô và áp lực hậu cần

```
HÀ NỘI
├── ~20.000–22.000 học sinh/năm
├── ~55–60 học sinh nhập học mới mỗi ngày
├── 3 kỳ thi → lịch học, nội dung, chu kỳ thi thử khác nhau
└── ~600–700 lớp học/năm (ước tính)

HỒ CHÍ MINH
├── ~8.000 học sinh/năm (mục tiêu 2026)
├── 6 đợt khai giảng — ~1.300 học sinh/đợt → spike tải lớn mỗi 2 tháng
├── ~22 học sinh nhập học mới mỗi ngày (bình quân)
└── Mỗi đợt khai giảng cần tạo 40–50 lớp ClassIn trong 1 tuần

TỔNG HỢP
├── ~77–82 học sinh nhập học mới mỗi ngày
└── Nếu mỗi ca onboarding mất 15 phút thủ công:
    HN: 55 HS × 15 phút = ~14 giờ nhân công/ngày
    HCM: 22 HS × 15 phút = ~5.5 giờ nhân công/ngày
    Tổng: ~20 giờ nhân công/ngày chỉ riêng onboarding
```

### 3.4 Cấu hình vận hành theo 4 kỳ thi

| Chiều | ĐGNL HSA | ĐGNL BCA | ĐGNL BQP | ĐGNL HCM |
|---|---|---|---|---|
| Cơ sở chủ yếu | HN | HN | HN | HCM |
| Team truyền thông | HSA team (5) | BCA team (5) | BQP team (5) | HCM team (5) |
| Đặc điểm học sinh | Rộng, đại trà | Niche, tiêu chuẩn khắt khe | Niche, tiêu chuẩn khắt khe | Rộng, đại trà |
| Độ phức tạp onboarding | Thấp | Cao (cần verify hồ sơ) | Cao (cần verify hồ sơ) | Thấp |
| Nội dung chăm sóc HS | Chung + kỳ thi HSA | Kỳ thi BCA + thể lực | Kỳ thi BQP + thể lực | Chung + kỳ thi HCM |
| Khai giảng | Rải đều | Theo chu kỳ thi | Theo chu kỳ thi | 6 đợt/năm |
| Tiền tố SBD | `HSA-26-XXXXX` | `BCA-26-XXXXX` | `BQP-26-XXXXX` | `HCM-26-XXXXX` |

---

## IV. VISION END-STATE & PHÂN TÍCH GAP

### 4.1 Vòng lặp vận hành mục tiêu (8 tầng)

```
① Marketing ──▶ ② CRM & Sale ──▶ ③ Thanh toán
                                          │
                                          ▼
⑧ Upsell/       ⑦ Retarget  ◀── ⑥ Chăm sóc ◀── ⑤ Học tập ◀── ④ Onboarding
   Cross-sell                    Học viên        ClassIn        Tự động
      │                              │
      └──────────────────────────────┘
              Vòng lặp mới ↺
```

| Tầng | Vision end-state | Trạng thái Q2/2026 |
|---|---|---|
| ① Marketing | Auto-tag nguồn + kỳ thi + CTV vào EZSale | Nhập tay một phần |
| ② CRM & Sale | Hot/Warm/Cold tự động; Zalo OA nurture theo kỳ thi | EZSale deployed, chưa full automation |
| ③ Thanh toán | SePay webhook; Zalo OA nhắc nếu thất bại | **Đang chạy ổn định** |
| ④ Onboarding | Auto-SBD; auto Zalo OA + Email; ClassIn API enroll; QLL 1-click | **Thủ công hoàn toàn** |
| ⑤ Học tập | ClassIn; điểm danh tự động; data pipeline → 3 dashboard | **Chưa có ClassIn — dùng Zoom + Zalo** |
| ⑥ Chăm sóc | Trigger từ hành vi ClassIn; Zalo OA tự động; chatbot 24/7 | **Thủ công, phản ứng** |
| ⑦ Retarget | Ngoài phạm vi vận hành | — |
| ⑧ Upsell | Ngoài phạm vi vận hành | — |

### 4.2 Phân tích gap — Theo mức độ ưu tiên vận hành

| Khoảng cách | Mức độ | Phase xử lý |
|---|---|---|
| Onboarding thủ công (SBD, Zalo OA, Email) | **Nghiêm trọng** | Phase 1 |
| ClassIn chưa triển khai | **Nghiêm trọng** | Phase 1 |
| CTV tracking thủ công (130 CTV) | **Nghiêm trọng** | Phase 1 |
| Dashboard QLL chưa tồn tại | **Cao** | Phase 1 |
| ClassIn data pipeline → 3 dashboard chưa có | **Cao** | Phase 2 |
| Trigger chăm sóc từ ClassIn data chưa thiết lập | **Cao** | Phase 2 |
| Thù lao GV tính thủ công | **Cao** | Phase 2 |
| Chatbot 24/7 (Zalo OA) chưa có | Trung bình | Phase 3 |
| Tài liệu tự học trên web chưa tổ chức | Trung bình | Phase 3 |

### 4.3 Nhận định vận hành quan trọng

**ClassIn là điều kiện tiên quyết của tầng 5 và 6.** Mọi trigger chăm sóc học viên đều phụ thuộc vào dữ liệu từ ClassIn. Không có ClassIn → không có dữ liệu → không có trigger → chăm sóc vẫn thủ công dù Zalo OA đã sẵn sàng. Đây là lý do ClassIn rollout là ưu tiên kỹ thuật số 1.

**Bảng mapping là điều kiện tiên quyết của ClassIn API.** Automation gán lớp + gán GV yêu cầu: `kỳ thi + khóa học → classin_course_id → gv_uid → qll_uid`. Nếu bảng này không được maintain trước khi khai giảng, automation sẽ enroll sai lớp hoặc không chạy được.

---

## V. LUỒNG VẬN HÀNH TỐI ƯU — KIẾN TRÚC 7 LUỒNG

### 5.0 Nguyên tắc kiến trúc

> **Trục tổ chức chính: Kỳ thi — không phải địa phương.**
> HN/HCM là tham số trong flow, không phải nhánh riêng.
> Mỗi kỳ thi có cấu hình riêng nhưng chạy chung 1 engine.

Lý do: team truyền thông, lịch học, nội dung chăm sóc học viên, chu kỳ khai giảng đều khác nhau theo kỳ thi. Tổ chức flow theo địa phương buộc phải maintain 2+ bản song song và không scale khi thêm kỳ thi mới.

**Kiến trúc 7 luồng:**

```
┌───────────────────────────────────────────────────────────┐
│          TRỤC SẢN PHẨM: 4 KỲ THI                         │
│   HSA · Bộ Công An · Bộ Quốc Phòng · ĐGNL HCM            │
│   Mỗi kỳ thi: config riêng — chạy chung 1 engine         │
└──────────────────────────┬────────────────────────────────┘
                           │
         ┌─────────────────┼─────────────────┐
         ▼                 ▼                 ▼
   [5 LUỒNG CHÍNH]  [LUỒNG 6: GV]   [LUỒNG 7: CTV/ĐS]
   Hành trình HS    Quản lý GV       Hoa hồng tự động
```

---

### 5.1 Luồng 1 — Lead Acquisition & Attribution

**Mục tiêu:** Mọi lead vào hệ thống phải có đầy đủ: nguồn + kỳ thi + cơ sở + CTV_code (nếu có).

```
4 kênh vào:
┌─────────────────┬──────────────────┬────────────────┬────────────┐
│   Organic       │   CTV Sale       │ Tuyến đi trường│  Đại sứ   │
│  (Ads/SEO/      │  130 người       │  2 người       │  8 người  │
│   Social)       │                  │                │           │
│      │          │  [Link riêng:    │  [QR tại       │  [Link ĐS │
│      │          │  ?ref=CTV001]    │   sự kiện]     │   riêng]  │
└──────┬──────────┴────────┬─────────┴───────┬────────┴─────┬─────┘
       │                   │                 │              │
       └───────────────────┴─────────────────┴──────────────┘
                                   │
                    [Web portal: hsavnu.edu.vn]
              Form tự động tag: kỳ thi + nguồn + CTV_code
                                   │
                               → EZSale
```

**Yêu cầu kỹ thuật:**
- Mỗi landing page URL tương ứng 1 kỳ thi để auto-tag `exam_type`
- Link CTV format: `hsavnu.edu.vn/dang-ky?ref=CTV001&exam=HSA`
- Form field ẩn tự điền: `utm_source`, `exam_type`, `ref_code`, `region`
- EZSale nhận webhook từ form → tạo lead tự động với đầy đủ tags

**Trạng thái hiện tại → Mục tiêu:**

| Kênh | Hiện tại | Mục tiêu Phase 1 |
|---|---|---|
| Organic | Form web → EZSale thủ công | Auto-push với tags |
| CTV | Báo qua Zalo → ghi Sheet tay | Link tracking → auto-tag CTV_code |
| Tuyến đi trường | Thu thập danh sách → nhập tay | QR code → form → auto EZSale |
| Đại sứ | Không có hệ thống | Link riêng + tracking giống CTV |

---

### 5.2 Luồng 2 — Lead Nurture & Close (EZSale)

**Mục tiêu:** Đúng Sale chăm đúng lead, đúng nội dung theo kỳ thi, không bỏ sót.

```
Lead vào EZSale (với tags: kỳ thi + HN/HCM + nguồn)
        │
        ▼
Auto-phân loại: Hot / Warm / Cold
(dựa trên: điền form đầy đủ, thời gian tương tác, hành vi web)
        │
   ┌────┴────┐
   ▼         ▼
 HOT        WARM / COLD
   │              │
Sale HN off    CTV Sale follow-up
(12 người)     + Zalo OA nurture
Sale HCM off   sequence theo kỳ thi
(3 người)
   │              │
Gọi điện        Chuỗi 5–7 tin:
trong 30 phút   nội dung khác theo
                kỳ thi đã tag
        │
        ▼
Chốt đơn → điền form web → thanh toán
```

**Chuỗi nurture theo kỳ thi (Zalo OA — định hướng nội dung):**

| Kỳ thi | Nội dung chuỗi nurture |
|---|---|
| HSA (ĐGNL HN) | Tỷ lệ đỗ các trường top, phân tích đề năm trước, lợi thế của học sinh thi ĐGNL |
| BCA | Điều kiện hồ sơ cụ thể, lịch thi, cách chuẩn bị thể lực song song học văn hóa |
| BQP | Tương tự BCA nhưng nội dung đặc thù các trường quân sự |
| ĐGNL HCM | Tỷ lệ đỗ các trường ĐHQG HCM, điểm sàn, chiến lược thi ĐGNL so với THPT QG |

**SLA Sale:**
- Lead Hot: phản hồi trong 30 phút (giờ hành chính)
- Lead Warm: vào chuỗi Zalo OA trong 2 giờ
- Lead Cold: Zalo OA auto; CTV follow-up trong 24 giờ

---

### 5.3 Luồng 3 — Payment → Auto-Onboarding

**Đây là luồng có tác động lớn nhất và khả năng automation cao nhất. Toàn bộ 5 bước sau SePay phải là tự động.**

**Hiện tại:** SePay webhook → [thủ công: tạo SBD → gửi Zalo OA → duyệt → add Zalo]
**Mục tiêu:** SePay webhook → tự động hoàn toàn trong < 5 phút

```
[SePay Webhook: payment_success]
Payload: {student_name, phone, email, exam_type, region, ref_code, order_id}
         │
         ▼
[Bước 1] Auto-generate SBD (< 1 giây)
  Format: [KỲ_THI]-[NĂM]-[SEQ_5_CHỮ_SỐ]
  Ví dụ:  HSA-26-08421 | BCA-26-00312 | HCM-26-01830
  Ghi vào: Google Sheet Master + EZSale record
         │
         ▼
[Bước 2] Zalo OA gửi ngay (< 2 phút)
  "✓ Đăng ký thành công!
   SBD của bạn: [SBD]
   Lớp học: [tên lớp] — [GV phụ trách]
   Lịch buổi đầu tiên: [ngày giờ]
   Nhóm lớp: [link invite]
   Đăng nhập ClassIn: [invoke link]"
         │
         ▼
[Bước 3] Email gửi ngay (< 2 phút)
  Nội dung đầy đủ:
  - SBD + mã đơn hàng
  - Hướng dẫn đăng nhập ClassIn (từng bước)
  - Lịch học chi tiết của lớp
  - Tên GV + CTV trợ giảng phụ trách
  - Link tài liệu chuẩn bị trước buổi 1
  - Liên hệ QLL phụ trách
         │
         ▼
[Bước 4] ClassIn API (sau khi ClassIn triển khai — Phase 1)
  a. register: SĐT + Email → ClassIn UID
  b. addSchoolStudent: UID vào trường HSA
  c. Lookup bảng mapping: exam_type + khoa_hoc → classin_course_id
  d. addCourseStudent: UID vào đúng course (identity=1)
  e. GV đã được gán từ lúc tạo course — không cần gán lại
         │
         ▼
[Bước 5] Cập nhật Google Sheet → Dashboard QLL
  Trạng thái: "Mới nhập học — chờ đăng nhập ClassIn"
  QLL nhận notification → 1 click xác nhận add Zalo lớp
         │
         ▼
[Bước 6] Nếu ref_code tồn tại trong payload:
  → Ghi nhận hoa hồng pending cho CTV/Đại sứ tương ứng
  → CTV nhận Zalo OA: "Bạn vừa có 1 học sinh đăng ký thành công"
```

**Điều kiện kỹ thuật bắt buộc — phải có TRƯỚC khi bật automation:**
- Bảng mapping: `khoa_hoc_code → classin_course_id → gv_uid → qll_uid`
- Học sinh phải follow Zalo OA trước (bước này đưa vào form đăng ký)
- Naming convention lớp ClassIn: `[KỲ_THI]_[MÔN]_[MÃ_LỚP]_[NĂM]`
  (Ví dụ: `HSA_Toan_L12A_2026`, `BCA_AnhVan_L01_2026`)

**Dashboard QLL — Trạng thái onboarding:**

```
Mới nhập học
    → Zalo OA đã gửi
    → Email đã gửi
    → ClassIn đã tạo tài khoản
    → Học sinh đã đăng nhập ClassIn lần đầu → Hoàn tất ✓
                                            ↘ Flag đỏ: sau 48h chưa login → QLL gọi
```

---

### 5.4 Luồng 4 — Học tập (ClassIn)

**ClassIn là nguồn dữ liệu học tập duy nhất. Không có dữ liệu nào được chấp nhận từ nguồn khác.**

```
GV vào lớp ClassIn (phải có mặt trước 5 phút — SLA)
       │
CTV Trợ giảng tham gia với vai trò Assistant
       │  → Xử lý Q&A live trong ClassIn
       │  → Theo dõi câu hỏi trong nhóm Zalo lớp sau buổi học
       │
ClassIn tự động ghi nhận:
       ├── Điểm danh: thời gian vào lớp, thời gian ở lại
       ├── Bài tập / quiz: điểm, thời gian nộp, tỷ lệ hoàn thành
       └── Hoạt động: đăng nhập, xem video, tương tác
       │
       ▼ Data Subscription (ClassIn PUSH về HSA endpoint)
       │
Google Sheet — Master Learning Data
       │
       ├──────────────────┬─────────────────┐
       ▼                  ▼                 ▼
[Dashboard QLL]  [Dashboard Ban ĐH]  [Dashboard GV]
Xem theo lớp:    Xem theo kỳ thi:    Xem lớp mình:
• Sĩ số hôm nay  • HN vs HCM        • Tỷ lệ tham dự
• Vắng mặt       • Tỷ lệ tham dự    • Điểm bài tập
• Điểm thấp      • Học sinh nguy cơ • Học sinh cần chú ý
• Không login    • NPS tổng hợp     • Lịch dạy tuần
  3+ ngày        • Sự cố mở         • Giờ dạy tháng
```

**Tần suất sync data:**
- Điểm danh: trong vòng 1 giờ sau buổi học kết thúc
- Bài tập / quiz: realtime khi nộp / chấm
- Dashboard: cập nhật khi Sheet thay đổi (Google Looker Studio)

**Yêu cầu vận hành lớp ClassIn:**

| Vai trò | Trách nhiệm | SLA |
|---|---|---|
| GV | Vào ClassIn trước 5 phút; upload tài liệu trước 24h | 100% |
| CTV Trợ giảng | Có mặt trong lớp ClassIn; phản hồi Zalo lớp trong 2h | Giờ hành chính |
| QLL | Kiểm tra dashboard mỗi sáng; xử lý flag đỏ trước 10h | Mỗi ngày |

---

### 5.5 Luồng 5 — Chăm sóc Học viên & Hoàn thành Khóa

**Chuỗi trigger tự động từ ClassIn data:**

| Trigger | Điều kiện | Hành động tự động | Fallback |
|---|---|---|---|
| Không đăng nhập | 3 ngày không login ClassIn | Zalo OA hỏi thăm + hướng dẫn lại | Ngày 5: QLL gọi điện |
| Vắng buổi học | Không có trong danh sách điểm danh | Zalo OA gửi tóm tắt + tài liệu buổi đó | 2+ buổi: QLL gọi điện |
| Điểm thấp | Điểm bài tập < ngưỡng (do kỳ thi quy định) | Zalo OA gợi ý tài liệu bổ trợ phù hợp môn | QLL nhận alert để tư vấn |
| Hoàn thành module | Nộp bài cuối module | Zalo OA chúc mừng + thông báo QLL | — |

**Chuỗi định kỳ theo mốc thời gian kỳ thi:**

| Mốc | Kênh | Nội dung |
|---|---|---|
| D-30 trước kỳ thi | Email + Zalo OA | Lịch ôn tập, kế hoạch 30 ngày cuối |
| D-7 | Email + Zalo OA | Thông tin phòng thi, thủ tục, những điều cần mang |
| D-3 | Zalo OA | Tips thi cụ thể theo kỳ thi (HSA/BCA/BQP/HCM khác nhau) |
| D-1 | Zalo OA | Chúc may mắn + nhắc nhở logistics |
| D+1 | Zalo OA | Hỏi thăm cảm nhận sau thi |
| D+7 (khi có kết quả) | Email | Kết quả + phân tích điểm yếu |
| Hàng tháng (trong khóa) | Email | Báo cáo tiến độ gửi phụ huynh |

**Chuỗi sau khi hoàn thành khóa:**

```
Kết quả kỳ thi có → phân loại:
├── Đỗ điểm cao → Mời tham gia Chương trình Đại sứ
├── Đỗ, điểm vừa → Cảm ơn + NPS survey
└── Chưa đỗ → Chuyển thông tin cho Sale (khóa ôn tiếp)
```

---

### 5.6 Luồng 6 — Quản lý Giảng viên (~70 GV)

**Hiện tại:** Lịch dạy qua Zalo/Sheet, thù lao tính tay cuối tháng.
**Mục tiêu:** ClassIn là nguồn dữ liệu duy nhất cho cả lịch dạy lẫn thù lao.

```
[Đầu mỗi đợt khai giảng]
GV nhận lịch dạy qua form/email (QLL tạo từ kế hoạch giảng dạy)
       │
QLL tạo khóa học ClassIn theo bảng mapping
  (Naming: [KỲ_THI]_[MÔN]_[MÃ_LỚP]_[NĂM])
       │
Gán GV vào khóa học qua ClassIn API (teacherUid)
  [Nếu nhiều GV cho 1 khóa: dùng LMS API createClass với teacherUid per buổi]
       │
GV xác nhận lịch → bắt đầu dạy

[Trong suốt khóa học]
ClassIn tự ghi nhận:
  → Số buổi đã dạy, giờ bắt đầu/kết thúc thực tế
  → Tỷ lệ học sinh tham dự lớp của GV
  → Không cần GV báo cáo thủ công

[Cuối tháng]
Auto-tổng hợp từ ClassIn:
  Giờ dạy × đơn giá = thù lao tháng
       │
Sheet tổng hợp → Kế toán chi review (30 phút thay vì cả ngày)
       │
Kế toán duyệt → Thanh toán
       │
GV nhận Zalo OA: "Tháng [X]: [Y] buổi, thù lao: [Z]đ — đã xử lý"
```

**KPI giảng viên — theo dõi từ ClassIn data:**

| Chỉ số | Mục tiêu | Cảnh báo khi |
|---|---|---|
| Vào ClassIn đúng giờ | > 98% | < 95% → QLL xử lý |
| Tỷ lệ học sinh tham dự lớp | > 80% | < 70% → check chất lượng |
| Upload tài liệu trước 24h | > 90% | < 80% → nhắc nhở |
| NPS từ học sinh | > 7.5/10 | < 7.0 → phỏng vấn |

---

### 5.7 Luồng 7 — CTV Sale & Đại sứ (~138 người)

**Vấn đề cốt lõi hiện tại:** 130 CTV sale đang được quản lý bằng Zalo group + Google Sheet thủ công. Ở quy mô này, tranh chấp hoa hồng và sai sót tính toán là không thể tránh.

**Nâng cấp: Hệ thống link tracking cá nhân hóa**

```
[Setup một lần cho mỗi CTV]
Cấp cho mỗi CTV 1 link riêng:
  hsavnu.edu.vn/dang-ky?ref=CTV001&exam=HSA
  (Có thể tạo nhiều link theo từng kỳ thi nếu CTV làm nhiều kỳ)
       │
[Khi học sinh dùng link]
Form tự động ghi CTV_code vào payload
       │
[Khi học sinh thanh toán]
SePay webhook payload chứa CTV_code
→ Google Sheet tự cập nhật: CTV001 | HS_name | order_id | amount | commission_pending
       │
[Cuối tháng]
Auto-tổng hợp: commission sheet per CTV
→ QL CTV review (check edge cases: hoàn tiền, trùng đơn)
→ Kế toán chi duyệt → Thanh toán
→ CTV nhận Zalo OA: "Tháng [X]: [N] học sinh, hoa hồng: [Y]đ — đã xử lý"
```

**Quản lý Đại sứ (8 người):**

| Yếu tố | Thiết kế |
|---|---|
| Tracking | Link riêng giống CTV, phân biệt bằng prefix `DS001` |
| Commission | Chính sách riêng (% khác CTV hoặc ưu đãi học phí) |
| Engagement | Check-in hàng tháng với quản lý Đại sứ |
| Recruitment path | Học sinh đỗ cao → được mời → onboard qua SOP-09 |

**Quy trình onboarding CTV mới:**
1. Ký hợp đồng CTV (có mã CTV duy nhất)
2. Nhận link tracking cá nhân
3. Training ngắn: cách dùng link, quy trình báo cáo, chính sách hoa hồng
4. Vào nhóm Zalo CTV theo kỳ thi phụ trách

---

## VI. ROADMAP VẬN HÀNH 2026–2028

### Tổng quan timeline

```
Q2–Q3/2026         Q3–Q4/2026         Q4/2026–Q1/2027    Q2–Q4/2027    2028
──────────────     ──────────────     ───────────────     ──────────    ─────────
PHASE 0            PHASE 1            PHASE 2             PHASE 3       HORIZON
Nền móng           Automation         Data Pipeline       Vận hành      Scale
hạ tầng            cốt lõi            & Chuẩn hóa         từ dữ liệu    2028
(4–6 tuần)         (8–10 tuần)        (10–12 tuần)        (6 tháng)
```

---

### PHASE 0 — Nền móng hạ tầng (4–6 tuần, KHÔNG THỂ BỎ QUA)

> Không triển khai bất kỳ automation nào trước khi Phase 0 hoàn thành. Mọi automation xây trên nền dữ liệu không được kiểm soát sẽ tạo ra lỗi hệ thống.

**Tuần 1–2:**
- [ ] Đăng ký Google Workspace — 1 domain cho toàn tổ chức
- [ ] Tạo email `@[domain].vn` cho 60 nhân sự offline (HN + HCM)
- [ ] Yêu cầu outsource dev: viết documentation toàn bộ hệ thống đang chạy (deadline 2 tuần)
- [ ] Thiết lập Weekly Sync HN–HCM (thứ Hai 9h, Google Meet, agenda cố định)

**Tuần 2–3:**
- [ ] Tạo Shared Drive với cấu trúc: `00_Toàn tổ chức / HÀ NỘI / HỒ CHÍ MINH / 4 kỳ thi`
- [ ] Migration: toàn bộ nhân sự chuyển file công việc vào Shared Drive
- [ ] Ban hành chính sách lưu trữ bằng văn bản (xác nhận từng người)
- [ ] Chuẩn hóa tên nhóm Zalo nội bộ: `[CƠ SỞ].[PHÒNG].[MỤC ĐÍCH].[NĂM]`

**Tuần 3–4:**
- [ ] Xác định phân quyền: GĐ VH Nam quyết định gì tự chủ, gì cần báo GĐ VH Bắc
- [ ] Đăng tuyển Tech Ops (xem Section IX)
- [ ] Đo KPI baseline: thời gian onboarding thủ công hiện tại, tỷ lệ lỗi SBD, lag time Zalo OA
- [ ] Inventory toàn bộ tool đang dùng: owner, dữ liệu nằm ở đâu, ai có quyền truy cập

**Output Phase 0:**
- Dữ liệu tổ chức được bảo vệ — không còn nằm trong Drive cá nhân
- Hệ thống đang chạy được documentation đầy đủ
- Cơ chế phối hợp HN–HCM tối thiểu đã hoạt động
- Baseline số liệu để đo hiệu quả các phase sau

---

### PHASE 1 — Automation cốt lõi (8–10 tuần)

> Xây một lần, phục vụ 4 kỳ thi và 2 cơ sở. Không build riêng cho từng kỳ thi.

**1.1 Auto-Onboarding sau SePay (3–4 tuần) — Ưu tiên #1**
- SePay webhook → Auto-generate SBD theo format `[KỲ_THI]-[NĂM]-[SEQ]`
- Auto-Zalo OA: gửi trong < 2 phút sau thanh toán (SBD + lớp + GV + link ClassIn)
- Auto-Email: guide nhập học đầy đủ theo từng kỳ thi
- Auto-log vào Google Sheet → Dashboard QLL basic

**1.2 ClassIn rollout — Pilot (3–4 tuần, song song với 1.1)**
- Build bảng mapping đầy đủ: `khoa_hoc_code → classin_course_id → gv_uid → qll_uid`
- Pilot 5–10 lớp: HSA 3 lớp + ĐGNL HCM 2 lớp (chọn lớp sắp khai giảng)
- Đào tạo GV và CTV trợ giảng sử dụng ClassIn
- Kiểm tra: học sinh đăng nhập được, GV vào được, chất lượng âm thanh/video đạt
- ClassIn API test: register + enroll + assign hoạt động đúng

**1.3 EZSale — Form → CRM tự động (2 tuần)**
- Webhook từ web form → EZSale API
- Auto-tag: kỳ thi + HN/HCM + nguồn (organic/CTV/đi trường)
- Auto-assign Sale theo kỳ thi + cơ sở
- CTV link tracking: cấp link `?ref=CTV_CODE` cho 130 CTV (ưu tiên HN trước)

**1.4 Dashboard QLL basic (1–2 tuần)**
- Google Sheet → Looker Studio
- View QLL: danh sách học sinh theo trạng thái onboarding (5 trạng thái)
- View COO: số liệu tổng hợp HN + HCM theo kỳ thi

**KPI đầu ra Phase 1:**

| Chỉ số | Mục tiêu |
|---|---|
| Thời gian thanh toán → nhận Zalo OA | < 5 phút |
| Tỷ lệ onboard thành công trong 24h | > 90% |
| Lead từ form → EZSale tự động (không nhập tay) | 100% |
| CTV tracking qua link (HN) | > 70% CTV đã dùng link |
| ClassIn pilot: học sinh đăng nhập thành công | > 85% |

---

### PHASE 2 — Data Pipeline & Chuẩn hóa vận hành (10–12 tuần)

**2.1 ClassIn full rollout & Data Pipeline (4–5 tuần) — Ưu tiên #1 của Phase 2**
- Rollout ClassIn toàn bộ lớp HN (tất cả 3 kỳ thi)
- Rollout ClassIn HCM (sau khi pilot Phase 1 thành công)
- Implement ClassIn Data Subscription (phải setup với ClassIn account manager):
  - Điểm danh → Google Sheet trong vòng 1 giờ sau buổi học
  - Bài tập / quiz → Sheet realtime
- Build 3 dashboard đầy đủ: QLL / Ban điều hành / GV

**2.2 Trigger chăm sóc học viên từ ClassIn data (3–4 tuần)**
- Trigger: 3 ngày không login → Zalo OA tự động
- Trigger: vắng buổi học → Zalo OA gửi tóm tắt
- Trigger: điểm thấp → Zalo OA gợi ý tài liệu
- Chuỗi D-30 / D-7 / D-3 / D-1 theo lịch từng kỳ thi

**2.3 GV Payroll tự động từ ClassIn (2–3 tuần)**
- ClassIn data → tổng hợp giờ dạy tháng → Sheet kế toán
- Kế toán chi: review 30 phút thay vì cả ngày

**2.4 CTV Commission tự động (2 tuần)**
- Hoàn thiện link tracking cho 130 CTV (cả HCM)
- Auto-tính hoa hồng từ SePay webhook + CTV_code
- Monthly summary Zalo OA cho từng CTV

**2.5 SOP hoàn chỉnh (song song với các mục trên)**

| Mã | Nội dung | Người viết | Deadline |
|---|---|---|---|
| SOP-01 | Onboarding học sinh — từ thanh toán đến buổi học đầu tiên | QLL Lead | Phase 2, tuần 2 |
| SOP-02 | Quản lý lớp hàng ngày — QLL làm gì, khi nào | QLL Lead | Phase 2, tuần 3 |
| SOP-03 | Quản lý giảng viên — lịch dạy, tài liệu, thù lao | QLL Lead | Phase 2, tuần 3 |
| SOP-04 | Xử lý sự cố học viên — không vào lớp, khiếu nại, hoàn tiền | QLL Lead | Phase 2, tuần 4 |
| SOP-05 | Xử lý sự cố kỹ thuật — ClassIn lỗi, Zalo OA lỗi, web lỗi | Tech Ops | Phase 2, tuần 4 |
| SOP-06 | Phối hợp HN–HCM — ai quyết định gì, leo thang thế nào | GĐ VH Bắc | Phase 2, tuần 2 |
| SOP-07 | Onboarding nhân sự mới | Hành chính NS | Phase 2, tuần 5 |
| SOP-08 | Quản lý CTV — đăng ký, tracking link, thanh toán hoa hồng | QL CTV | Phase 2, tuần 3 |
| SOP-09 | Chương trình Đại sứ — tuyển chọn, quản lý, incentive | QL Đại sứ | Phase 2, tuần 6 |

**KPI đầu ra Phase 2:**

| Chỉ số | Mục tiêu |
|---|---|
| ClassIn adoption (HN) | > 95% học sinh đăng nhập thành công |
| ClassIn adoption (HCM) | > 90% |
| Trigger chăm sóc fired đúng điều kiện | 100% |
| Hoa hồng CTV tự động | > 90% (không còn tính tay) |
| Thù lao GV tự động từ ClassIn data | > 90% |
| SOP coverage | 100% quy trình quan trọng có văn bản |

---

### PHASE 3 — Vận hành dựa trên dữ liệu (Q2–Q4/2027)

> Khi hệ thống đã ổn định, chuyển từ vận hành phản ứng sang vận hành chủ động từ dữ liệu.

**3.1 Dashboard nâng cao (3 tầng hoàn chỉnh)**

*QLL Dashboard — nâng cấp:*
- Thêm cột "Nguy cơ bỏ học": vắng nhiều + điểm thấp + không tương tác
- Hàng đợi việc cần làm hôm nay, sắp xếp theo mức độ ưu tiên
- Lịch sử liên lạc: QLL gọi ai, kết quả thế nào

*Ban điều hành Dashboard — mới:*
- Tổng quan theo kỳ thi: tỷ lệ tham dự, NPS, tỷ lệ hoàn thành
- HN vs HCM cạnh nhau theo từng kỳ thi
- Sự cố đang mở và thời gian xử lý trung bình
- Hiệu suất GV: NPS theo GV, tỷ lệ tham dự lớp theo GV

*GV Dashboard — mới:*
- Lớp mình đang dạy, sĩ số, tỷ lệ tham dự
- Học sinh cần chú ý trong lớp mình
- Số giờ dạy tháng này + thù lao tích lũy

**3.2 Chatbot FAQ 24/7 (Zalo OA)**
- Phạm vi: lịch học, SBD, cách đăng nhập ClassIn, lịch thi thử
- Ngoài phạm vi: câu hỏi học thuật, khiếu nại, hoàn tiền → chuyển QLL/Sale
- Triển khai: Zalo OA chatbot tích hợp vào OA hiện có

**3.3 Quản lý sự cố có cấu trúc**
- Mọi sự cố thành ticket: loại sự cố + cơ sở + người xử lý + thời gian giải quyết
- Báo cáo sự cố hàng tuần → ưu tiên cải thiện SOP
- SLA: lỗi nghiêm trọng phản hồi trong 15 phút, giải quyết trong 2 giờ

**3.4 Tài liệu tự học trên ClassIn LMS**
- GV upload video, đề luyện tập, trắc nghiệm vào ClassIn
- Phân quyền: học sinh chỉ xem nội dung của khóa đã mua
- Quy trình: GV → upload → QLL review → publish
- *Scope cần thiết kế cùng bộ phận học thuật trước khi vận hành triển khai*

---

### HORIZON 2028 — Thiết kế cho scale

> Hệ thống xây năm 2026 phải phục vụ được quy mô 2028 mà không cần xây lại.

| Chiều scale | 2026 | 2028 (mục tiêu) | Yêu cầu thiết kế ngay từ Phase 1 |
|---|---|---|---|
| Học sinh HCM/năm | ~8.000 | ~16.000+ | API batch processing, không single-call |
| Lớp ClassIn/đợt HCM | 40–50 | 80–100 | Tạo lớp hàng loạt từ template |
| Nhân sự fulltime | 60 | ~120 (x2) | Dashboard phân quyền theo team/phòng |
| CTV toàn tổ chức | ~130 | ~195 (x1.5) | Link tracking tự động, không quản tay |
| GV online | ~70 | ~140 (x2) | Payroll tự động hoàn toàn từ ClassIn |

**Cột mốc kiểm soát tăng trưởng:**
- **Q4/2026:** Đánh giá capacity hệ thống sau 2 đợt khai giảng HCM
- **Q1/2027:** Quyết định nâng cấp kỹ thuật trước khi tăng tốc tuyển sinh 2027
- **Q3/2027:** Review cơ cấu nhân sự theo lộ trình x2 fulltime
- **Q1/2028:** Hệ thống phải ổn định ở quy mô 2027 trước khi bước vào năm tăng trưởng lớn

---

## VII. MA TRẬN RỦI RO VẬN HÀNH

| # | Rủi ro | Xác suất | Tác động | Mức độ | Trạng thái |
|---|---|---|---|---|---|
| R1 | 1 outsource dev phục vụ cả HN + HCM + 4 kỳ thi | Rất cao | Rất cao | **Khủng hoảng** | Đang xảy ra |
| R2 | Dữ liệu trong Drive cá nhân — mất khi nhân sự nghỉ | Rất cao | Cao | **Nghiêm trọng** | Đang xảy ra |
| R3 | "Duyệt học sinh" 1 người — tắc nếu nghỉ | Cao | Cao | **Nghiêm trọng** | Đang xảy ra |
| R4 | CTV tracking thủ công — 130 CTV → tranh chấp | Cao | Cao | **Cao** | Đang xảy ra |
| R5 | HCM tự phát triển quy trình, lệch chuẩn | Cao | Cao | **Cao** | Đang xảy ra |
| R6 | Lãnh đạo không nắm tình trạng HCM (không dashboard) | Cao | Cao | **Cao** | Đang xảy ra |
| R7 | Onboarding spike HCM: 1.300 HS trong 1 tuần, làm thủ công | Rất cao | Cao | **Cao** | Sắp xảy ra |
| R8 | ClassIn triển khai không đồng bộ → dữ liệu học tập trống | Trung bình | Cao | **Cao** | Rủi ro triển khai |
| R9 | Automation gửi sai SBD / sai lớp / sai GV | Trung bình | Cao | **Trung bình** | Rủi ro khi go-live |
| R10 | 20 người truyền thông (4 team) không có shared asset → branding lệch | Trung bình | Trung bình | **Trung bình** | Đang xảy ra |
| R11 | GV không dùng ClassIn đúng cách → dữ liệu học tập không tin cậy | Trung bình | Cao | **Cao** | Rủi ro triển khai |
| R12 | Hệ thống thiết kế cho 2026 không scale lên 2028 | Thấp | Rất cao | **Nghiêm trọng** | Rủi ro thiết kế |

**Biện pháp xử lý theo thứ tự:**

| Rủi ro | Biện pháp | Thời hạn |
|---|---|---|
| R1 — Outsource dev | Yêu cầu documentation ngay; tuyển Tech Ops trong 60 ngày | 30 ngày |
| R2 — Mất data | Google Workspace + Shared Drive + chính sách bắt buộc | 30 ngày |
| R3 — Duyệt HS tắc | Automation onboarding loại bỏ bước này; 1 người chuyển thành QA exceptions | Phase 1 |
| R4 — CTV tracking | Cấp link tracking cho 130 CTV; training 1 buổi | Phase 1 |
| R5 — HCM lệch chuẩn | SOP-06 + training HCM team + weekly sync | 45 ngày |
| R6 — Mù HCM | Dashboard COO view Phase 1 | Phase 1 |
| R7 — Spike HCM | Auto-onboarding batch processing từ Phase 1 | Phase 1 |
| R8 — ClassIn không đồng bộ | Pilot kỹ Phase 1, rollout theo lộ trình có kiểm soát | Phase 1–2 |
| R9 — Automation sai | Validation layer + log đầy đủ + fallback thủ công | Trước go-live |
| R10 — Branding lệch | Shared Drive: thư mục asset chung cho 4 team; style guide | Phase 0 |
| R11 — GV không dùng ClassIn | Training bắt buộc; SLA GV vào ClassIn đúng giờ | Phase 1 |
| R12 — Không scale | Thiết kế batch API từ đầu; load test mỗi đợt khai giảng lớn | Xuyên suốt |

---

## VIII. VẬN HÀNH CHI NHÁNH HCM

### 8.1 Đặc điểm vận hành HCM 2026

| Yếu tố | HN (3 kỳ thi) | HCM (1 kỳ thi: ĐGNL HCM) |
|---|---|---|
| Nhân sự offline | 50 người | 10 người |
| CTV Sale | ~100 | ~30 |
| Giảng viên | ~60 GV (phụ trách chính) | ~10 GV remote từ HN |
| Khai giảng | Rải đều quanh năm | 6 đợt lớn, ~1.300 HS/đợt |
| Tải onboarding | ~55–60 HS/ngày đều | Spike ~260 HS/ngày trong tuần khai giảng |
| Quản lý QLL | 8 QLL (chung HN+HCM) | Không có QLL riêng — dùng chung |

### 8.2 Mô hình quản trị Hub–Spoke

```
HÀ NỘI (Hub — định chuẩn)         HỒ CHÍ MINH (Spoke — thực thi)
───────────────────────────        ──────────────────────────────
Engine vận hành & Công nghệ    →   Dùng chung, không build riêng
SOP & Quy trình chuẩn          →   HCM follow, không tự sáng tạo
Sản phẩm & Nội dung giảng dạy  →   Nhận và dùng đúng phiên bản
Báo cáo & Kiểm soát            →   HCM báo cáo về HN theo lịch cố định
```

**GĐ Vận hành Nam tự chủ quyết định:**
- Lịch làm việc nội bộ team HCM
- Phân công Sale và QLL cho từng lớp HCM
- Xử lý sự cố cấp độ lớp

**GĐ Vận hành Nam báo GĐ Vận hành Bắc trước khi quyết định:**
- Thay đổi quy trình onboarding hay SOP chuẩn
- Tuyển dụng hoặc chấm dứt hợp tác với GV/CTV
- Chi phí ngoài ngân sách đã duyệt

### 8.3 Cơ chế phối hợp HN–HCM

**Weekly Sync (thứ Hai 9h, Google Meet, 30 phút):**
- Agenda cố định: KPI tuần trước → Vấn đề cần giải quyết → Kế hoạch tuần này
- Ghi chú lưu Shared Drive (không lưu trong Zalo)

**Chuẩn Zalo nội bộ:**
```
[CƠ SỞ].[PHÒNG].[MỤC ĐÍCH].[Năm]
Ví dụ:
HN.QLL.LopHSA12A — 2026
HCM.Sale.ChotDon — 2026
ALL.MGMT.WeeklySync — 2026
HN-HCM.OPS.XuLySuCo — 2026
```

### 8.4 Quy trình leo thang sự cố HCM

```
Sự cố xảy ra tại HCM
       │
QLL / Sale HCM xử lý theo SOP-04/05
       │
  Giải quyết được? ──Có──▶ Ghi ticket, đóng
       │
      Không
       │
GĐ Vận hành Nam xử lý
       │
  Giải quyết được? ──Có──▶ Ghi ticket, đóng
       │
      Không
       │
Leo thang → nhóm [HN-HCM.OPS.XuLySuCo]
+ Ghi nhận để cải thiện SOP
```

### 8.5 Điều kiện để HCM vận hành hoàn toàn tự chủ

- [ ] GĐ Vận hành Nam đã được đào tạo đầy đủ SOP-01 → SOP-06
- [ ] Toàn bộ nhân sự HCM có email công ty và dùng Shared Drive
- [ ] ClassIn đã rollout và team HCM vận hành thành thạo
- [ ] Automation onboarding chạy ổn định qua ít nhất 1 đợt khai giảng HCM
- [ ] Dashboard COO view đang cập nhật số liệu HCM hàng ngày
- [ ] Ít nhất 1 chu kỳ khóa học HCM đã hoàn thành và có báo cáo tổng kết

---

## IX. NHÂN SỰ KỸ THUẬT

### 9.1 Tình trạng hiện tại — Rủi ro R1

1 outsource dev đang phục vụ toàn bộ hệ thống cho 4 kỳ thi, 2 cơ sở. Kịch bản tệ nhất: dev bận hoặc nghỉ hợp đồng → toàn bộ automation ngừng hoạt động, không ai sửa lỗi, không ai triển khai tính năng mới.

**Hành động ngay (trong 2 tuần):** Yêu cầu outsource dev dừng phát triển feature mới, ưu tiên viết documentation đầy đủ cho toàn bộ hệ thống hiện tại. Đây là điều kiện bắt buộc để onboard người kế tiếp.

### 9.2 Lộ trình tuyển dụng kỹ thuật

| Thời điểm | Vị trí | Mô tả | Chi phí ước tính |
|---|---|---|---|
| **Ngay — tháng 6/2026** | **Tech Ops** | Quản trị Workspace, monitor automation, xử lý lỗi cấu hình, cầu nối với outsource dev | 18–25 triệu/tháng |
| Q4/2026 | Junior Developer | Bảo trì automation, phát triển feature nhỏ, giảm phụ thuộc outsource | 20–30 triệu/tháng |
| Q2/2027 | Head of Technology | Định hướng kỹ thuật dài hạn khi quy mô 2 cơ sở ổn định | 50–80 triệu/tháng |

### 9.3 Mô tả Tech Ops (ưu tiên tuyển ngay)

**Cần có:**
- Quản trị Google Workspace (tạo/xóa account, phân quyền Drive, nhóm)
- Đọc hiểu webhook, API log (không cần viết code, nhưng phải debug được)
- Google Apps Script ở mức chỉnh sửa script sẵn có
- Viết tài liệu kỹ thuật rõ ràng, dễ đọc

**Chịu trách nhiệm:**
- Monitor hàng ngày: automation có chạy không, lỗi gì không
- Xử lý lỗi cấp độ cấu hình (không phải lỗi code)
- Quản lý Workspace cho 60+ tài khoản
- Cầu nối: vận hành mô tả vấn đề → outsource dev fix

---

## X. KPI VẬN HÀNH TỔNG HỢP

### 10.1 KPI cấp COO — Review hàng tuần

| Chỉ số | Mục tiêu | Cảnh báo khi |
|---|---|---|
| Thời gian payment → Zalo OA | < 5 phút | > 15 phút |
| Tỷ lệ onboard thành công trong 24h | > 90% | < 85% |
| Lead vào EZSale tự động (không nhập tay) | 100% | < 95% |
| Học sinh vắng 2+ buổi chưa được liên hệ | 0% | > 0 |
| Sự cố kỹ thuật chưa được ghi nhận | 0% | > 0 |
| ClassIn data sync trễ > 2h | 0 lần/tuần | ≥ 1 → điều tra |

### 10.2 KPI cấp Operations Manager — Review hàng ngày

| Chỉ số | Mục tiêu |
|---|---|
| Flag đỏ trên Dashboard QLL (chưa login 48h) | Xử lý hết trước 10h |
| Học sinh mới chưa onboard > 24h | 0 |
| Ticket sự cố mở > 4h | 0 |
| Câu hỏi trong nhóm hỏi đáp chưa trả lời > 2h | 0 (giờ hành chính) |

### 10.3 KPI giảng viên — Review hàng tháng

| Chỉ số | Mục tiêu |
|---|---|
| Vào ClassIn đúng giờ (trước 5 phút) | > 98% |
| Tỷ lệ học sinh tham dự lớp trung bình | > 80% |
| Upload tài liệu trước buổi học > 24h | > 90% |
| NPS học sinh cho GV | > 7.5/10 |

### 10.4 KPI trigger chăm sóc học viên (Phase 2+)

| Trigger | Đo lường | Mục tiêu |
|---|---|---|
| Không login 3 ngày | Fired / Zalo OA gửi | 100% trong 1h sau khi điều kiện đạt |
| Vắng buổi học | Thời gian vắng → Zalo OA gửi | < 2h sau buổi học kết thúc |
| Điểm thấp | Gợi ý tài liệu gửi | 100% học sinh điểm thấp nhận gợi ý |
| Leo thang QLL gọi điện | Đúng SLA (2+ vắng → QLL gọi) | 100% |

### 10.5 KPI hệ thống kỹ thuật — Monitor hàng ngày

| Chỉ số | Ngưỡng cảnh báo |
|---|---|
| Zalo OA gửi thất bại | > 2% → điều tra |
| Email gửi thất bại | > 1% → điều tra |
| ClassIn API lỗi | > 0 → xử lý ngay |
| SePay webhook không nhận | > 0 → xử lý khẩn |
| ClassIn → Sheet sync trễ > 2h | > 0 → kiểm tra pipeline |
| CTV commission tính sai | > 0 → điều tra |

### 10.6 SLA vận hành — Chuẩn xử lý bắt buộc

| Loại | SLA phản hồi đầu tiên | SLA giải quyết |
|---|---|---|
| Học sinh chưa nhận Zalo OA sau thanh toán | 15 phút | 1 giờ |
| Học sinh không vào được ClassIn | 1 giờ | 4 giờ |
| SePay webhook lỗi | 15 phút | 1 giờ |
| GV vắng không báo trước | 15 phút phát hiện | 30 phút tìm GV thay |
| Hỏi đáp học viên trong nhóm lớp | 2 giờ (giờ hành chính) | 24 giờ |
| Tranh chấp hoa hồng CTV | 24 giờ | 48 giờ |
| Yêu cầu hoàn tiền | 4 giờ (phản hồi) | Theo chính sách công ty |

---

## XI. NGUYÊN TẮC VẬN HÀNH

**Nguyên tắc 1 — Kỳ thi là trục tổ chức chính**
Mọi quy trình, template, dashboard, chuỗi chăm sóc đều được cấu hình theo kỳ thi trước, địa phương sau. Không thiết kế riêng cho HN rồi copy sang HCM — thiết kế theo kỳ thi, thêm tham số địa phương.

**Nguyên tắc 2 — Một engine, nhiều cơ sở**
Không build hệ thống riêng cho HCM. Mọi automation, SOP, công cụ đều phục vụ cả HN và HCM từ cùng một engine. Chi phí vận hành một hệ thống luôn thấp hơn hai hệ thống.

**Nguyên tắc 3 — HCM follow SOP, không tự sáng tạo**
HCM có thể điều chỉnh giọng văn giao tiếp phù hợp địa phương. Không thay đổi quy trình lõi mà không có phê duyệt. Mọi đề xuất cải tiến từ HCM được ghi nhận và xem xét cập nhật vào SOP chính thức.

**Nguyên tắc 4 — Dữ liệu thuộc tổ chức, không thuộc cá nhân**
Không lưu file công việc vào Drive cá nhân. Không trao đổi thông tin học sinh qua email cá nhân. Khi nhân sự nghỉ, dữ liệu ở lại trong tổ chức.

**Nguyên tắc 5 — Automation phải có log và fallback**
Mọi luồng tự động phải có: log thành công/thất bại, cảnh báo khi lỗi, người chịu trách nhiệm khi có sự cố, cơ chế xử lý tay khi automation không chạy được.

**Nguyên tắc 6 — ClassIn là nguồn dữ liệu học tập duy nhất**
Không chấp nhận báo cáo thủ công về điểm danh, bài tập, tiến độ. Mọi số liệu học tập đều từ ClassIn. Nếu ClassIn không ghi nhận, coi như không có dữ liệu — đây là động lực buộc GV và học sinh dùng ClassIn đúng cách.

**Nguyên tắc 7 — Đo lường trước, tối ưu sau**
Không thay đổi quy trình dựa trên cảm tính. Mọi thay đổi vận hành phải có số liệu baseline trước và được đánh giá sau tối thiểu 4 tuần triển khai.

**Nguyên tắc 8 — Thiết kế cho 2028, không chỉ cho 2026**
Mọi quyết định kiến trúc kỹ thuật trong 2026 phải chịu được tải gấp đôi. Không tối ưu cho hiện tại nếu sẽ phải xây lại trong 18 tháng.

---

## XII. CHECKLIST 30–60–90 NGÀY (Từ Q2/2026)

### 30 ngày — Nền móng & Kiểm soát

- [ ] Google Workspace setup; toàn bộ nhân sự HN + HCM có email công ty
- [ ] Shared Drive tạo cấu trúc đầy đủ; toàn bộ nhân sự đã migrate file
- [ ] Chính sách lưu trữ ban hành và xác nhận từng người
- [ ] Outsource dev đã giao nộp documentation hệ thống
- [ ] Weekly Sync HN–HCM đã chạy lần đầu (thứ Hai 9h)
- [ ] Tên nhóm Zalo nội bộ đã chuẩn hóa (cả HN và HCM)
- [ ] Phân quyền GĐ VH Bắc / GĐ VH Nam đã xác định bằng văn bản
- [ ] Đăng tuyển Tech Ops đã đăng
- [ ] KPI baseline đã đo: thời gian onboarding thủ công, tỷ lệ lỗi SBD

### 60 ngày — Automation & Tuyển dụng

- [ ] Auto-SBD generation sau SePay đang chạy
- [ ] Auto-Zalo OA onboarding (< 5 phút sau thanh toán)
- [ ] Auto-Email hướng dẫn nhập học
- [ ] EZSale: form → CRM tự động, tag kỳ thi + nguồn
- [ ] CTV link tracking: 100 CTV HN đã có link riêng
- [ ] ClassIn pilot: 5–10 lớp đang chạy ổn định
- [ ] Dashboard QLL basic đang hoạt động (HN + HCM)
- [ ] Tech Ops đã tuyển hoặc đang phỏng vấn vòng cuối
- [ ] SOP-01 (Onboarding) và SOP-06 (HN–HCM) đã viết xong

### 90 ngày — Chuẩn hóa & Đánh giá

- [ ] Tech Ops đã onboard và đang vận hành hệ thống
- [ ] ClassIn: tất cả lớp mới đều chạy trên ClassIn (không còn Zoom)
- [ ] ClassIn data pipeline đang chạy (điểm danh về Sheet trong 1h)
- [ ] SOP-01 → SOP-06 đã hoàn thành và training HCM team
- [ ] Đánh giá Phase 1: so sánh KPI với baseline
- [ ] Quyết định: đủ điều kiện bước vào Phase 2 chưa?

---

## XIII. KẾT LUẬN

Ưu tiên vận hành không thay đổi theo thứ tự:

**30 ngày đầu:** Nền móng hạ tầng — Google Workspace, documentation hệ thống, phân quyền, Weekly Sync. Không có nền móng vững, mọi automation xây lên đều dễ sụp.

**60 ngày:** Ba automation cốt lõi — auto-SBD, auto-Zalo OA, ClassIn pilot. Ba cái này giải quyết điểm nghẽn nghiêm trọng nhất của quy trình hiện tại.

**90 ngày:** Đánh giá thực tế — số liệu có cải thiện không? ClassIn có được GV và học sinh chấp nhận không? QLL có dùng dashboard không? Dựa trên thực tế, không dựa trên kế hoạch.

Hệ thống vận hành tốt không phải hệ thống có nhiều tính năng nhất — mà là hệ thống được đội ngũ thực sự dùng, dữ liệu thực sự chính xác, và sự cố được phát hiện trước khi ảnh hưởng đến học sinh.

---

*Phiên bản 3.0 — Q2/2026 — Rewrite toàn diện dựa trên cơ cấu tổ chức và luồng vận hành thực tế*
*Review tiếp theo: Cuối Q3/2026 — sau khi Phase 1 hoàn thành*
*Người chịu trách nhiệm cập nhật: Giám đốc vận hành*
