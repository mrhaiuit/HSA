# HSA Education — Phân Tích Hiện Trạng Vận Hành
## Báo cáo As-Is Operations Analysis — Q2/2026

---

**Loại tài liệu:** Báo cáo phân tích hiện trạng vận hành chuyên sâu (As-Is Operations Analysis)
**Phạm vi:** Toàn bộ chuỗi vận hành HSA Education tại thời điểm Q2/2026
**Phương pháp:** Quan sát luồng nghiệp vụ thực tế · Phỏng vấn vai trò vận hành · Kiểm kê công cụ và dữ liệu · Đo lường tải hậu cần
**Vai trò trong bộ tài liệu:** Tài liệu 1/2 — xác định vấn đề, mức độ nghiêm trọng, phụ thuộc, rủi ro và nợ vận hành trước khi chọn giải pháp
**Bao gồm:** Phân tích hiện trạng · Điểm nghẽn · Rủi ro · Nợ vận hành · Định hướng xử lý và chuyển đổi
**Không bao gồm:** Thiết kế module chi tiết · Lộ trình triển khai chi tiết · Ngân sách triển khai · Quyết định cuối cùng về nền tảng
**Tài liệu kế tiếp:** Đánh giá phù hợp Odoo & lộ trình chuyển đổi chi tiết (xem [danh-gia-phu-hop-odoo-va-lo-trinh-chuyen-doi-hsa-education-2026-2028.md](danh-gia-phu-hop-odoo-va-lo-trinh-chuyen-doi-hsa-education-2026-2028.md))
**Phiên bản:** 1.1 — Q2/2026

---

## Mục lục

- I. Tóm tắt điều hành (Snapshot Q2/2026)
- II. Cơ cấu tổ chức và Nhân sự
- III. Bản đồ hệ thống đang sử dụng
- IV. Hiện trạng vận hành theo 9 luồng nghiệp vụ
- V. Phân tích điểm nghẽn (Bottleneck Inventory)
- VI. Ma trận rủi ro vận hành
- VII. Nợ vận hành (Operational Debt)
- VIII. Phụ thuộc và Single Points of Failure
- IX. Tóm lược kết quả đánh giá
- X. Định hướng xử lý và chuyển đổi
- XI. Tài liệu liên quan

---

## I. TÓM TẮT ĐIỀU HÀNH (SNAPSHOT Q2/2026)

> Mục này mô tả hiện trạng tại một thời điểm — không đưa ra giải pháp chi tiết. Mọi nhận định đều dựa trên quan sát luồng nghiệp vụ thực tế của Q2/2026.

### 1.1 Quy mô vận hành đang gánh

| Chiều đo | Giá trị Q2/2026 | Ghi chú nguồn |
|---|---|---|
| Học sinh đăng ký/năm | ~20.000 | Tổng cộng 2 cơ sở |
| Học sinh nhập học mới/ngày | ~55 | Bình quân năm (HN ~33–34 + HCM ~21–22) |
| Sản phẩm | 4 kỳ thi | HSA · BCA · BQP · ĐGNL HCM |
| Cơ sở / thị trường vận hành | 2 | HN là thị trường lõi; HCM là thị trường mở rộng từ 2026 |
| Nhân sự fulltime/offline | 62 | HN 50 · HCM 12 |
| Giảng viên online / dạy chính | ~70 GV online; HCM có 15 GV dạy chính | GV online phục vụ 2 miền; nhóm GV chính HCM được ghi nhận riêng |
| Cộng tác viên / freelance | >170 | HN ~100 CTV Sale; HCM có Sale 20–25 người và Marketing 20 người gồm cả fulltime/CTV; cần tách dữ liệu để tránh đếm trùng |
| **Tổng nhân lực hoạt động** | **>300 người** | Tổng ước tính vì một số nhóm HCM đang pha trộn fulltime và CTV |
| Lớp học/năm (ước tính) | ~600–700 | Chủ yếu HN, cộng dồn 4 kỳ thi |
| Áp lực mở rộng thị trường HCM | 6 đợt khai giảng/năm | Trung bình ~1.300 HS/đợt, chịu áp lực cạnh tranh cao |

### 1.2 Ba điểm mấu chốt của hiện trạng

> Đây là tóm lược *quan sát*, không phải đề xuất hành động.

**Điểm mấu chốt 1 — Tự động hóa chỉ tồn tại ở duy nhất bước thanh toán.**
SePay webhook chạy ổn định cho khâu xác nhận thanh toán. *Toàn bộ* chuỗi nghiệp vụ trước và sau thanh toán đều phụ thuộc thao tác tay của con người: nhập lead vào CRM, tạo SBD, gửi Zalo OA, duyệt học sinh, add/enroll học sinh vào lớp, gửi hướng dẫn học, tổng hợp thù lao GV, tính hoa hồng CTV.

**Điểm mấu chốt 2 — Học tập đang chuyển từ Zoom sang ClassIn, nhưng chưa thành lớp dữ liệu vận hành.**
ClassIn đang được đưa vào để thay dần Zoom ở vai trò nền tảng lớp học live. Tuy nhiên, ClassIn hiện mới được dùng ở tầng vận hành lớp học; API/data subscription chưa được tích hợp sâu vào CRM, chăm sóc học viên, dashboard, thù lao GV hoặc báo cáo quản trị. Vì vậy, dữ liệu điểm danh, thời lượng tham dự, bài tập, login và hành vi học tập vẫn chưa trở thành nguồn dữ liệu tự động cho vận hành.

**Điểm mấu chốt 3 — Dữ liệu tổ chức phân mảnh trên 5–7 hệ thống không kết nối.**
Dữ liệu học sinh nằm rải rác giữa: form web → Google Sheet → EZSale → SePay → Zalo cá nhân/nhóm → ClassIn/Zoom chuyển tiếp → Google Drive cá nhân. Mỗi hệ thống có người chủ trì riêng, không có nguồn sự thật duy nhất (single source of truth). Phần lớn tài liệu vận hành đang lưu trên Drive cá nhân của nhân viên, không thuộc tổ chức.

### 1.3 Khoảng cách giữa năng lực hệ thống và quy mô đang vận hành

| Khoảng cách | Quy mô đang gánh | Năng lực hệ thống hiện tại |
|---|---|---|
| Tỷ lệ QLL/Học sinh | 20.000 HS / 8 QLL = **2.500 HS/QLL** | Quản lý qua nhóm Zalo và Google Sheet |
| Tải onboarding/ngày | ~55 HS, mỗi ca ~15 phút thủ công | ~14 giờ nhân công/ngày chỉ cho onboarding |
| Spike tải HCM (1 tuần khai giảng) | ~260 HS/ngày × 6 đợt/năm | Cùng quy trình thủ công, không có batch processing |
| Đội kỹ thuật | 4 kỳ thi × 2 cơ sở × 8 hệ thống | 1 outsource developer (phần lớn workload) |
| Hồ sơ Sale/CTV cần tính hoa hồng | ~132–137 người | Tracking qua Zalo + Sheet, đối soát cuối tháng bằng tay |
| Hồ sơ GV cần tính thù lao | ~70 GV online + 15 GV chính HCM | Tổng hợp giờ dạy thủ công cuối tháng; cần đối chiếu tránh trùng giữa 2 nhóm |
| Single point of failure được phát hiện | "Duyệt học sinh" — 1 người | Nghỉ phép → toàn bộ chuỗi onboarding tắc |

---

## II. CƠ CẤU TỔ CHỨC VÀ NHÂN SỰ

### 2.1 Sơ đồ tổ chức cấp cao

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

#### 2.2.1 Lãnh đạo

| Vị trí | SL | Phạm vi |
|---|---|---|
| Đồng sáng lập / HĐQT | 2 | Hoa, Thầy Khương — quản lý toàn tổ chức |
| GĐ Vận hành Bắc | 1 | 3 kỳ thi: HSA, BCA, BQP |
| GĐ Vận hành Nam | 1 | 1 kỳ thi: ĐGNL HCM |

#### 2.2.2 Tài chính — Kế toán (chung toàn tổ chức)

| Vị trí | SL |
|---|---|
| Kế toán thu | 1 |
| Kế toán chi | 1 |
| Kế toán tổng hợp | 1 |
| **Tổng** | **3** |

#### 2.2.3 Sale

```
MIỀN BẮC (HÀ NỘI)
├── Phòng Sale Offline: 1 Trưởng phòng + 11 sale = 12 người
└── Đội Sale Online:    1 quản lý + ~100 CTV

MIỀN NAM (HỒ CHÍ MINH)
└── Sale: 20–25 người

TỔNG MẠNG LƯỚI SALE: HN ~112 người + HCM 20–25 người = ~132–137 người
```

**Cơ cấu nhân sự HCM cập nhật:**

| Nhóm | Quy mô | Ghi chú |
|---|---:|---|
| Offline | 12 người | Nhân sự làm việc trực tiếp tại cơ sở HCM |
| Sale | 20–25 người | Cần tách rõ fulltime/CTV trong CRM và chính sách hoa hồng |
| Marketing | 20 người | Bao gồm cả fulltime và CTV |
| Vận hành lớp học | 10 người | Phụ trách vận hành lớp, hỗ trợ học viên, phối hợp GV |
| Giáo viên dạy chính | 15 người | Nhóm GV chính cho thị trường HCM |

#### 2.2.4 Học vụ & Quản lý lớp học

| Bộ phận | Vị trí | SL | Ghi chú |
|---|---|---|---|
| Duyệt học sinh | Chuyên viên | 1 | Xác nhận thanh toán + tạo SBD + add Zalo lớp |
| Trợ giảng | Lead | 1 | Điều phối đội trợ giảng toàn tổ chức |
| Trợ giảng | CTV / môn | 2–3 | Q&A online theo môn học |
| Quản lý lớp (QLL) | Lead | 1 | Quản lý vận hành toàn bộ lớp HN + HCM |
| Quản lý lớp (QLL) | CTV | 7 | Chạy lớp hàng ngày |
| Vận hành lớp học HCM | Team vận hành | 10 | Vận hành lớp HCM, hỗ trợ học viên, phối hợp GV |

> **Quan sát đo lường:** Nếu tính riêng 8 QLL hiện hữu (1 lead + 7 CTV), tải vẫn ở mức ~20.000 học sinh → **2.500 HS/người**. Sau khi ghi nhận thêm đội vận hành lớp học HCM 10 người, tỷ lệ vận hành toàn hệ thống giảm về khoảng **~1.110 HS/người**, nhưng vẫn phụ thuộc workaround thủ công và chưa có dữ liệu học tập tập trung để theo dõi.

#### 2.2.5 Giảng viên

| | SL | Ghi chú |
|---|---|---|
| GV online (2 miền) | ~70 | Dạy remote, phục vụ cả HN và HCM |
| GV dạy chính HCM | 15 | Nhóm GV chính cho thị trường HCM, cần đối chiếu có nằm trong ~70 GV online hay là nhóm bổ sung |
| GV làm đề thi | Chưa xác định | Vận hành riêng biệt, không nằm trong scope báo cáo này |

#### 2.2.6 Truyền thông — Tổ chức theo kỳ thi

> Cấu hình HN hiện tại: mỗi kỳ thi 1 Lead + 2 Content + 1 Edit + 1 Design = **5 người/kỳ thi**. Riêng HCM đang ghi nhận **20 người Marketing** gồm cả fulltime và CTV.

| Kỳ thi | Cơ sở | Team |
|---|---|---|
| ĐGNL HSA | HN | 5 người |
| ĐGNL Bộ Công An | HN | 5 người |
| ĐGNL Bộ Quốc Phòng | HN | 5 người |
| ĐGNL HCM | HCM | 20 người (fulltime + CTV) |
| **Tổng** | | **~35 người** |

**Tuyến đi trường:** 2 người — phụ trách hoạt động tại trường THPT, hội thảo, tư vấn trực tiếp.

#### 2.2.7 Đại sứ & Hành chính

| Vị trí | SL | Ghi chú |
|---|---|---|
| Đại sứ HSA (Ambassador) | 8 | Active cả 2 miền — cựu học sinh xuất sắc |
| Hành chính — Nhân sự | 1 | Chung toàn tổ chức |

### 2.3 Bảng tổng kết nhân sự

```
NHÂN SỰ FULLTIME / OFFLINE
─────────────────────────────────────────
HÀ NỘI                        : 50 người
HỒ CHÍ MINH                   : 12 người
Tổng offline                   : 62 người

SALE
─────────────────────────────────────────
Sale offline HN                :  12
CTV Sale HN                    : ~100
Sale HCM                       : 20–25
Tổng mạng lưới Sale            : ~132–137

MARKETING / TRUYỀN THÔNG
─────────────────────────────────────────
Marketing HN (3 kỳ thi)        :  15
Marketing HCM                  :  20 (fulltime + CTV)
Tổng Marketing                 : ~35

VẬN HÀNH LỚP HỌC
─────────────────────────────────────────
QLL Lead + CTV hiện hữu        :   8
Vận hành lớp học HCM           :  10
Tổng vận hành lớp học          : ~18

NHÂN SỰ CTV / FREELANCE KHÁC
─────────────────────────────────────────
CTV Trợ giảng (2–3/môn)        :  ~25
Đại sứ                         :    8

GIÁO VIÊN ONLINE
─────────────────────────────────────────
GV online (không tính GV đề)   :  ~70
GV dạy chính HCM               :   15 (ghi nhận riêng)

TỔNG TOÀN TỔ CHỨC              : >300 người (cần chuẩn hóa để tránh đếm trùng fulltime/CTV HCM)
```

### 2.4 Tỷ lệ phụ thuộc nhân sự — Đo lường

| Chỉ số | Giá trị thực tế Q2/2026 |
|---|---|
| Tỷ lệ fulltime/offline / tổng nhân lực | 62/>300 = **~20%** |
| Tỷ lệ CTV + GV / tổng nhân lực | Ước tính **~80%**, cần chuẩn hóa vì HCM có nhóm vừa fulltime vừa CTV |
| Tỷ lệ HS/QLL hiện hữu | 20.000/8 = **2.500 HS/người** |
| Tỷ lệ HS/vận hành lớp học sau khi tính HCM | 20.000/18 ≈ **1.110 HS/người** |
| Tỷ lệ HS/Sale offline (HN) | ~12.000/12 ≈ **1.000/Sale** |
| Tỷ lệ HS/Sale (HCM) | 8.000/20–25 ≈ **320–400 HS/người** |
| Tỷ lệ CTV/quản lý CTV (HN) | 100/1 = **100/1** |

---

## III. BẢN ĐỒ HỆ THỐNG ĐANG SỬ DỤNG

### 3.1 Inventory hệ thống — Vai trò, chủ trì, vị trí dữ liệu

| Hệ thống | Vai trò trong vận hành | Người chủ trì | Dữ liệu lưu ở đâu | Trạng thái |
|---|---|---|---|---|
| Web portal `hsavnu.edu.vn` | Form đăng ký + thanh toán | Outsource dev | Database web (không nắm chi tiết) | Đang chạy |
| SePay (cổng thanh toán) | Webhook xác nhận thanh toán | Kế toán + outsource dev | SePay cloud | **Tự động ổn định** |
| EZSale (CRM) | Quản lý lead Sale | Sale Lead HN | EZSale cloud | Đang dùng — nhập một phần thủ công |
| Google Sheet (vận hành) | Danh sách HS, SBD, hoa hồng CTV, thù lao GV | Nhiều người (rải rác) | Drive cá nhân nhân viên | **Phân mảnh** |
| Google Drive (lưu file) | Tài liệu nội bộ, hồ sơ | Mỗi nhân viên tự lưu | Drive cá nhân | **Nguy cơ mất dữ liệu** |
| Zalo OA | Gửi thông báo cho HS | Vận hành | Zalo cloud | Đang dùng — gửi thủ công |
| Zalo nhóm (nội bộ) | Giao tiếp nội bộ HN/HCM | Tự phát theo phòng ban | Zalo cloud | Không chuẩn hóa tên |
| Zalo nhóm (lớp học) | Tương tác GV–HS–QLL | QLL | Zalo cloud | Là kênh chính của học tập |
| Zoom | Lớp học live legacy / dự phòng | GV + QLL | Zoom cloud | Đang giảm vai trò khi chuyển sang ClassIn |
| ClassIn | Nền tảng lớp học live thay dần Zoom | Học vụ + QLL + GV | ClassIn cloud | Đang triển khai vận hành lớp học; **chưa tích hợp API/data sâu** |
| Email công ty | Gửi/nhận chính thức | Một số bộ phận | — | **Chưa có domain công ty thống nhất** |

### 3.2 Bản đồ luồng dữ liệu — Mức độ kết nối giữa các hệ thống

```
EZSale (CRM)        ←─?──   Web portal (form)          ──→ SePay (thanh toán)
   │                            │                              │
   │ thủ công                   │ webhook                      │ webhook
   │ nhập tay                   │ (chạy ổn định)               │
   ▼                            ▼                              ▼
Google Sheet  ←──copy-paste──→ Zalo (OA + nhóm)  ←──không kết nối sâu──→  ClassIn
   │                                                                    │
   │ (thủ công)                                                         │
   ▼                                                                    │
Kế toán đối soát                       ──── chưa có data pipeline ─────┘
   │
   ▼
Báo cáo cuối tháng (Sheet thủ công)
```

**Quan sát:** Trong 7+ hệ thống đang dùng, **chỉ duy nhất 1 luồng kết nối tự động ổn định** (SePay → Web). ClassIn đang được đưa vào lớp học live, nhưng chưa được nối sâu qua API/data subscription về hệ thống quản trị. Mọi luồng còn lại vẫn dựa vào con người chuyển dữ liệu thủ công bằng copy-paste, nhập tay, hoặc tổng hợp Sheet.

### 3.3 Trạng thái phân quyền và quản trị dữ liệu

| Yếu tố | Trạng thái Q2/2026 |
|---|---|
| Email tổ chức (domain công ty) | Chưa có cho toàn bộ 60 nhân sự offline |
| Google Workspace tập trung | Chưa có |
| Shared Drive theo cấu trúc tổ chức | Chưa có |
| Chính sách lưu trữ bằng văn bản | Chưa có |
| Phân quyền theo vai trò | Không có (mỗi tool có chính sách riêng) |
| Backup tập trung | Không có |
| Quy ước đặt tên nhóm Zalo | Không có (tự phát) |
| Audit log truy cập dữ liệu | Không có |

### 3.4 Trạng thái documentation hệ thống

| Hạng mục | Trạng thái |
|---|---|
| Sơ đồ kiến trúc kỹ thuật toàn bộ | Không có (ngoại trừ kiến thức trong đầu outsource dev) |
| Tài liệu webhook (SePay, Zalo OA) | Không có |
| Tài liệu cấu hình EZSale | Không có |
| Tài liệu cấu hình vận hành ClassIn | Chưa có đầy đủ |
| Tài liệu tích hợp ClassIn API vào hệ thống quản trị | Chưa có triển khai thực tế |
| SOP (Standard Operating Procedure) chính thức | Không có cho phần lớn quy trình |
| Bản vẽ luồng nghiệp vụ (chính thức) | Có 1 sơ đồ tham khảo (HTML file: *Quy trình vận hành thực tế*) |
| Tài liệu phân tích quy trình | Có (file [phan-tich-danh-gia-quy-trinh-hsa-education.md](phan-tich-danh-gia-quy-trinh-hsa-education.md)) |
| Tài liệu phân tích ClassIn API | Có (file [danh-gia-classin-api-tich-hop.md](danh-gia-classin-api-tich-hop.md)) |

---

## IV. HIỆN TRẠNG VẬN HÀNH THEO 9 LUỒNG NGHIỆP VỤ

> Phần này mô tả *từng bước* của quy trình đang thực sự diễn ra trong vận hành — không phải quy trình lý tưởng. Mỗi luồng được phân tích theo cấu trúc: (a) Mô tả luồng, (b) Người chịu trách nhiệm và công cụ, (c) Quan sát đo lường, (d) Nguyên nhân gốc của các vấn đề phát hiện được.

### 4.1 Luồng 1 — Marketing & Tạo lead

**(a) Mô tả luồng đang diễn ra:**
```
Quảng cáo Facebook/TikTok/Google
       │
       ▼
Landing page (web portal hsavnu.edu.vn)
       │
       ▼
Form đăng ký / form tư vấn → web database
       │
       ▼ (thủ công, có độ trễ)
Sale nhập lead vào EZSale CRM
```

**(b) Người chịu trách nhiệm và công cụ:**
- Marketing chạy quảng cáo trên 3 nền tảng.
- Lead thu về landing page.
- Sale (HN: 12 sale offline + ~100 CTV; HCM: 20–25 người) chịu trách nhiệm chuyển lead từ web/Sheet sang EZSale.

**(c) Quan sát đo lường:**
- Không có integration tự động giữa form và EZSale → lead vào EZSale qua thao tác nhập tay.
- Thời gian từ lúc HS điền form đến lúc lead xuất hiện trong EZSale: không được đo lường có hệ thống; phụ thuộc giờ làm việc của Sale.
- Không có rule chống trùng lead theo SĐT trong EZSale (theo quan sát hiện tại).
- Không có auto-tag nguồn (organic vs CTV vs ads) ở mức tự động — Sale phải nhập trường nguồn bằng tay.

**(d) Nguyên nhân gốc các vấn đề quan sát được:**
- Web portal và EZSale là 2 hệ thống độc lập, chưa được tích hợp.
- Không có quy trình bắt buộc về SLA "lead → CRM trong X phút".
- Đội Sale không được giải phóng khỏi task nhập tay → mất thời gian trên việc administrative thay vì tư vấn.

### 4.2 Luồng 2 — CRM, Sale, và Nurture

**(a) Mô tả luồng:**
```
Lead trong EZSale (Hot / Warm / Cold)
       │
       ▼
HOT       → Sale gọi điện trực tiếp, chốt đơn
WARM/COLD → Sale nhắc lại thủ công (Zalo cá nhân, gọi điện)
           Một số trường hợp được gửi vào nhóm Zalo CTV để nurture
       │
       ▼
Khi gần chốt → Sale gửi link giỏ hàng web qua Zalo cá nhân
       │
       ▼
HS tự vào web, chọn khóa, thanh toán
```

**Luồng giám sát và hỗ trợ tư vấn hiện tại:**
```
Sale/CTV tư vấn học viên
       │
       ▼
Phát sinh case khó / tư vấn sai / cần hỗ trợ chốt
       │
       ▼
Quản lý hoặc nhân sự có kinh nghiệm mở CRM kiểm tra thủ công
       │
       ▼
Review ghi chú, trạng thái lead, lịch sử xử lý còn lại trong CRM
       │
       ▼
Trao đổi lại riêng với Sale/CTV để hướng dẫn cách xử lý
       │
       ▼
Nếu case tương tự xuất hiện ở nhân sự khác hoặc học viên khác
→ người quản lý phải review lại gần như từ đầu
```

**(b) Người chịu trách nhiệm và công cụ:**
- Sale phân loại lead trong EZSale (Hot/Warm/Cold).
- Việc nhắc và nurture Warm/Cold lead hiện phụ thuộc kỷ luật cá nhân Sale.
- Zalo cá nhân của Sale là kênh chính để giao tiếp với lead.
- Sale Lead / quản lý CTV hỗ trợ tư vấn và kiểm tra chất lượng case theo kiểu ad-hoc.
- Công cụ review chủ yếu là CRM + ghi chú thủ công + trao đổi riêng; chưa có checklist QA, playbook tư vấn, hoặc thư viện case dùng lại.

**(c) Quan sát đo lường:**
- Không có chuỗi nurture tự động qua Zalo OA cho Warm/Cold lead.
- Không có SLA bằng văn bản cho phản hồi lead Hot.
- Lịch sử trao đổi với lead nằm trong Zalo cá nhân của Sale → khi Sale nghỉ việc, lịch sử mất.
- Tỷ lệ lead Warm/Cold quay lại tư vấn không được đo lường.
- Việc kiểm tra chất lượng tư vấn đang hoàn toàn thủ công: người quản lý phải vào CRM xem từng lead/case, đọc ghi chú, hỏi lại Sale/CTV, rồi hướng dẫn riêng.
- Không có quy trình xác định case nào bắt buộc phải review, ai review, review trong bao lâu, và kết quả review được ghi lại ở đâu.
- Một vấn đề tư vấn giống nhau có thể lặp lại ở nhiều nhân sự hoặc nhiều học viên khác nhau, nhưng chưa có cơ chế biến lần review đầu tiên thành hướng dẫn chuẩn cho các lần sau.
- Không đo được tỷ lệ case tư vấn bị sai, thiếu thông tin, cần escalation, hoặc cần coaching lại.

**(d) Nguyên nhân gốc:**
- Zalo OA chưa được kết nối với CRM để tự kích hoạt chuỗi nurture theo stage.
- Sale dùng Zalo cá nhân thay vì kênh chính thức → không có audit trail.
- CRM hiện chủ yếu được dùng để lưu trạng thái lead, chưa được thiết kế như hệ thống coaching/QA cho tư vấn.
- Không có taxonomy case tư vấn (học phí, chọn khóa, chuyển lớp, hoàn tiền, phản đối phụ huynh, so sánh đối thủ, học lực yếu, lịch học không phù hợp...) để gom nhóm và tái sử dụng cách xử lý.
- Không có playbook tư vấn chuẩn cho Sale/CTV theo từng loại tình huống; kiến thức nằm trong đầu quản lý hoặc nhân sự giỏi.
- Không có workflow escalation từ Sale/CTV lên quản lý và quay lại CRM dưới dạng quyết định có thể audit.

### 4.3 Luồng 3 — Thanh toán

**(a) Mô tả luồng:**
```
HS chọn khóa trên web portal
       │
       ▼
Thanh toán (chuyển khoản hoặc cổng thanh toán)
       │
       ▼
SePay webhook → web backend: payment_success
       │
       ▼ (tự động — chỉ đến đây là tự động)
Web database ghi nhận đơn hàng thanh toán
       │
       ▼ (sau đây toàn bộ chuyển sang luồng 4 — thủ công)
```

**(b) Người chịu trách nhiệm và công cụ:**
- HS thanh toán trên web portal.
- SePay xử lý cổng thanh toán và gửi webhook về web backend.
- Kế toán thu đối soát lại thủ công (đối chiếu SePay log với báo cáo ngân hàng).

**(c) Quan sát đo lường:**
- **Đây là khâu trưởng thành nhất của toàn bộ quy trình.** SePay webhook chạy ổn định, không phải là điểm đau quan sát được.
- Kế toán thu báo cáo mất khoảng ~2h/ngày để đối soát SePay với đơn hàng (có thể giảm khi có hệ thống tự match).
- Trường hợp thanh toán fail: Sale nhắc lại thủ công qua Zalo cá nhân.

**(d) Nguyên nhân gốc của phần thủ công còn lại:**
- Hệ thống chỉ ghi `payment_success`, không tự động kích hoạt chain các bước tiếp theo.
- Auto-reconciliation giữa SePay log và Sales Order chưa được thực hiện ở mức báo cáo kế toán.

### 4.4 Luồng 4 — Onboarding sau thanh toán

> Đây là luồng nghiệp vụ tập trung khối lượng thủ công lớn nhất của toàn bộ tổ chức.

**(a) Mô tả luồng đang diễn ra:**
```
Web đã ghi nhận thanh toán
       │
       ▼ [Toàn bộ bước sau là thủ công]
Nhân sự tạo SBD (số báo danh)
   → ghi vào Google Sheet
       │
       ▼
Gửi SBD + link nhóm Zalo lớp qua Zalo OA (gửi tay)
       │
       ▼
"Duyệt học sinh": 1 chuyên viên kiểm tra sheet có SBD + mã đơn hàng
   → Add học sinh vào nhóm Zalo lớp
       │
       ▼
QLL gửi hướng dẫn học, thông tin lớp ClassIn/Zoom chuyển tiếp, tài liệu trong nhóm Zalo lớp
       │
       ▼
HS tự đọc hướng dẫn, vào nhóm Zalo, tự cài/đăng nhập Zoom hoặc ClassIn tùy lớp
   → Nếu vướng → hỏi trong nhóm Zalo, QLL hỗ trợ từng người
```

**(b) Người chịu trách nhiệm và công cụ:**
- "Duyệt học sinh" — 1 chuyên viên duy nhất.
- QLL Lead + 7 QLL CTV — quản lý nhóm Zalo lớp.
- Công cụ: Google Sheet (master danh sách HS), Zalo OA (gửi thông báo), Zalo nhóm (lớp học).

**(c) Quan sát đo lường:**

| Chỉ số | Giá trị quan sát |
|---|---|
| Số HS nhập học mới/ngày | ~55 (HN ~33–34 + HCM ~21–22) |
| Thời gian ước tính/ca onboarding thủ công | ~15 phút |
| Tổng nhân công/ngày cho onboarding | HN ~8.5h + HCM ~5.5h = **~14h/ngày** |
| Spike HCM trong tuần khai giảng | Lên tới ~260 HS/ngày × 6 đợt/năm |
| Lag thời gian HS nhận SBD sau thanh toán | Phụ thuộc giờ hành chính, có thể nhiều giờ |
| Số người là single-point-of-failure | 1 (chuyên viên "Duyệt học sinh") |

**(d) Nguyên nhân gốc:**
- Không có integration giữa SePay webhook và logic tạo SBD/gửi Zalo OA/enroll lớp.
- Enroll ClassIn nếu có vẫn chủ yếu là thao tác vận hành / import bán thủ công; chưa có chuỗi tự động `thanh toán → tạo SBD → tạo/enroll ClassIn → gửi hướng dẫn`.
- Bảng mapping `khóa học → lớp → GV → QLL` chưa được chuẩn hóa thành dữ liệu có thể được tra cứu tự động.
- Học sinh phải tự follow Zalo OA trước khi nhận được tin nhắn → không có trong checkout flow.
- "Duyệt học sinh" là một bước con người được thiết kế ra để bù lại việc thiếu validation tự động.

### 4.5 Luồng 5 — Học tập (ClassIn transition + Zalo)

**(a) Mô tả luồng đang diễn ra:**
```
QLL/GV tạo hoặc vận hành lớp trên ClassIn
       │
       ▼
QLL gửi hướng dẫn/link lớp trong nhóm Zalo lớp
       │
       ▼
GV dạy trên ClassIn; một số lớp/ca vẫn có thể dùng Zoom chuyển tiếp hoặc dự phòng.
       │
       ▼
Điểm danh/dữ liệu học tập: có thể tồn tại trong ClassIn nhưng chưa sync tự động về hệ thống quản trị
       │
       ▼
Học sinh hỏi đáp trong nhóm Zalo lớp sau giờ học
       │
       ▼
QLL kiểm tra tình trạng học thủ công, đối chiếu ClassIn/Zalo/Sheet nếu cần
```

**(b) Người chịu trách nhiệm và công cụ:**
- GV: dạy trên ClassIn; Zoom giữ vai trò chuyển tiếp/dự phòng ở một số trường hợp.
- QLL: vận hành lớp, gửi hướng dẫn ClassIn/link lớp, theo dõi nhóm Zalo lớp.
- CTV trợ giảng: hỗ trợ Q&A.
- Công cụ: ClassIn (lớp học live), Zalo nhóm (giao tiếp), Drive/Web (tài liệu), Zoom (legacy/dự phòng).

**(c) Quan sát đo lường:**
- ClassIn có tiềm năng sinh dữ liệu cấu trúc về điểm danh, thời gian tham dự, bài tập, login, nhưng **dữ liệu này chưa được tích hợp sâu qua API/data subscription** vào CRM, dashboard hoặc workflow chăm sóc.
- Điểm danh nếu cần báo cáo vận hành vẫn phải export, xem trong ClassIn, hoặc tổng hợp lại thủ công vào Sheet.
- Tỷ lệ tham dự trung bình mỗi lớp: chưa đo lường có hệ thống ở cấp tổ chức.
- Học sinh vắng: phát hiện được khi QLL có thời gian xem nhóm Zalo và đối chiếu tay.
- Tài liệu giảng dạy: phân tán giữa ClassIn, Zalo, Drive, và web — chưa có version control tập trung.

**(d) Nguyên nhân gốc:**
- ClassIn đang được triển khai ở tầng nền tảng lớp học, nhưng chưa được thiết kế thành nguồn dữ liệu vận hành tập trung.
- Chưa có mapping chuẩn `student_id/SBD ↔ classin_uid ↔ lớp ↔ GV ↔ QLL` để nối dữ liệu học tập với hồ sơ học sinh.
- Chưa có middleware/API job kéo attendance, login, bài tập, giờ dạy từ ClassIn về hệ thống quản trị.
- Zalo nhóm không phải hệ thống ghi nhận hành vi học tập.

### 4.6 Luồng 6 — Chăm sóc học viên

**(a) Mô tả luồng:**
```
HS hỏi → nhóm Zalo lớp
       │
       ▼
GV / CTV trợ giảng / QLL phản hồi trực tiếp trong nhóm
       │
       ▼ (nếu QLL phát hiện)
HS vắng nhiều buổi → QLL nhắc thủ công (Zalo cá nhân hoặc gọi)
       │
       ▼
Trước kỳ thi: gửi tip thủ công trong nhóm Zalo
Sau kỳ thi:    không có quy trình thu thập NPS có hệ thống
```

**(b) Người chịu trách nhiệm và công cụ:**
- GV, CTV trợ giảng, QLL — tất cả cùng làm việc trong nhóm Zalo lớp.
- Không có cơ chế gán ticket, ưu tiên, hoặc SLA.

**(c) Quan sát đo lường:**
- Phản hồi trung bình: không đo lường.
- Số câu hỏi/lớp/tuần: không đo lường.
- NPS định kỳ: không có.
- Học sinh có nguy cơ bỏ học: phát hiện thụ động (khi QLL có thời gian check).
- Lịch sử chăm sóc nằm trong Zalo, không tổng hợp được thành báo cáo.

**(d) Nguyên nhân gốc:**
- Không có hệ thống ticket / helpdesk.
- Không có dữ liệu học tập tập trung để generate trigger tự động (luồng 5 thiếu data đầu nguồn).
- Việc chăm sóc dựa trên trí nhớ và sự sát sao cá nhân của QLL.

### 4.7 Luồng 7 — Quản lý Giảng viên (lịch dạy + thù lao)

**(a) Mô tả luồng:**
```
[Đầu mỗi đợt khai giảng]
QLL Lead lập lịch dạy → gửi GV qua Zalo / Google Sheet
       │
       ▼
GV nhận lịch, xác nhận
       │
       ▼ [Trong khóa]
GV vào ClassIn dạy (Zoom còn là phương án chuyển tiếp/dự phòng)
       │
       ▼ [Cuối tháng]
GV (hoặc QLL) tổng hợp giờ dạy thủ công vào Sheet
       │
       ▼
Kế toán chi đối chiếu Sheet và xử lý thù lao
```

**(b) Người chịu trách nhiệm và công cụ:**
- QLL Lead: lập lịch dạy.
- GV: nhận lịch qua Zalo, xác nhận, dạy.
- Kế toán chi: xử lý thù lao ~70 GV online và 15 GV chính HCM mỗi tháng (cần đối chiếu tránh trùng hồ sơ).

**(c) Quan sát đo lường:**

| Chỉ số | Giá trị quan sát |
|---|---|
| Số GV cần đối soát thù lao/tháng | ~70 |
| Thời gian kế toán chi xử lý thù lao GV/tháng | Khoảng 1 ngày làm việc |
| Tỷ lệ GV vào lớp đúng giờ | Không đo lường |
| Tỷ lệ học sinh tham dự lớp của GV | Không đo lường |
| NPS từ học sinh cho GV | Không có |

**(d) Nguyên nhân gốc:**
- ClassIn có thể ghi nhận giờ dạy/tham dự, nhưng dữ liệu này chưa được tích hợp API sâu vào hệ thống tính thù lao.
- Giờ dạy thực tế vẫn phải đối chiếu thủ công giữa lịch, ClassIn/Zoom chuyển tiếp và Sheet.
- Không có HR system để quản lý hồ sơ GV thỉnh giảng.

### 4.8 Luồng 8 — Quản lý CTV Sale & Đại sứ (tracking + hoa hồng)

**(a) Mô tả luồng:**
```
CTV giới thiệu HS → nhắn vào nhóm Zalo CTV
       │
       ▼
Quản lý CTV ghi nhận vào Google Sheet
       │
       ▼
HS thanh toán → CTV / quản lý CTV đối chiếu lại tay
       │
       ▼ [Cuối tháng]
Quản lý CTV tổng hợp số HS theo từng CTV
       │
       ▼
Tính hoa hồng thủ công → chuyển kế toán chi
       │
       ▼
Kế toán chi xử lý mạng lưới Sale/CTV ~132–137 người
```

**(b) Người chịu trách nhiệm và công cụ:**
- 1 quản lý CTV HN + đội Sale HCM 20–25 người + vận hành/kế toán chi phối hợp đối soát.
- Công cụ: Zalo nhóm CTV + Google Sheet (chính).

**(c) Quan sát đo lường:**

| Chỉ số | Giá trị quan sát |
|---|---|
| Số Sale/CTV cần đối soát hoa hồng/tháng | ~132–137 |
| Thời gian xử lý hoa hồng Sale/CTV/tháng (tổng) | Khoảng 2 ngày làm việc |
| Tỷ lệ tranh chấp/khiếu nại về attribution | Có xảy ra nhưng không có số liệu thống kê |
| Tracking attribution tự động | Không có (không có link riêng `?ref=CTV_CODE`) |
| Phân loại Đại sứ vs Sale/CTV | 8 Đại sứ, mạng lưới Sale/CTV ~132–137 người — có chính sách riêng nhưng cùng quy trình thủ công |

**(d) Nguyên nhân gốc:**
- Không có hệ thống link tracking cá nhân hóa (UTM/ref code) cho CTV/Đại sứ.
- Form web không nhận tham số ref_code để gắn vào lead/đơn hàng.
- Không có module commission tự động kết nối với confirmed orders.

### 4.9 Luồng 9 — Đối soát Kế toán

**(a) Mô tả luồng:**
```
Hàng ngày:
  Kế toán thu đối soát SePay log vs đơn hàng (~2h/ngày)

Cuối tháng:
  Kế toán chi xử lý thù lao GV (~1 ngày)
  Kế toán chi xử lý hoa hồng CTV (~2 ngày)
  Kế toán tổng hợp báo cáo (~vài ngày)
```

**(b) Người chịu trách nhiệm:**
- Kế toán thu (1), Kế toán chi (1), Kế toán tổng hợp (1) — chung toàn tổ chức.

**(c) Quan sát đo lường:**

| Hoạt động | Tải nhân công |
|---|---|
| Đối soát SePay daily | ~2h/ngày × 1 người |
| Tổng hợp thù lao GV monthly | ~1 ngày × 1 người |
| Tính hoa hồng CTV monthly | ~2 ngày × 1 người |
| Báo cáo P&L theo kỳ thi / cơ sở | **Không tồn tại** dưới dạng realtime |

**(d) Nguyên nhân gốc:**
- Hệ thống kế toán chuyên dụng chưa được triển khai.
- Dữ liệu đầu vào cho kế toán nằm rải rác (Sheet thù lao, Sheet hoa hồng, SePay log) → mỗi báo cáo đều cần tổng hợp tay.
- Không có analytic dimensions để báo cáo P&L theo kỳ thi × cơ sở.

---

## V. PHÂN TÍCH ĐIỂM NGHẼN (BOTTLENECK INVENTORY)

> Mỗi điểm nghẽn được đánh giá trên 3 chiều: (a) Mức độ tác động vận hành, (b) Tần suất xuất hiện, (c) Khả năng có thể tự động hóa về nguyên tắc.

| # | Điểm nghẽn | Vị trí trong luồng | Tác động quan sát | Tần suất | Có thể tự động hóa (về nguyên tắc) |
|---|---|---|---|---|---|
| N1 | Tạo SBD thủ công sau thanh toán | Luồng 4 | HS chờ nhiều giờ mới có SBD | Mỗi đơn hàng | 100% |
| N2 | Gửi Zalo OA thủ công (SBD + link nhóm) | Luồng 4 | Delay, lệ thuộc giờ hành chính | Mỗi đơn hàng | 100% |
| N3 | "Duyệt học sinh" — 1 người làm tay | Luồng 4 | Nghỉ phép → toàn bộ chuỗi onboarding tắc | Daily | ~80% (còn exception handling) |
| N4 | Add HS vào nhóm Zalo lớp thủ công | Luồng 4 | QLL mất thời gian, dễ nhầm lớp | Mỗi HS mới | Có (1-click từ dashboard) |
| N5 | Lead nhập tay vào EZSale | Luồng 1 | Chậm phản hồi, dễ sót/trùng | Mỗi lead | 100% |
| N6 | Nurture Warm/Cold lead thủ công | Luồng 2 | Tỷ lệ chuyển đổi không đo được | Liên tục | Phần lớn |
| N7 | ClassIn đã/đang dùng cho lớp học nhưng chưa có data pipeline | Luồng 5 | Không trigger được chăm sóc tự động từ attendance/login/bài tập | Mỗi buổi học | Cần tích hợp ClassIn API |
| N8 | Tracking CTV thủ công qua Zalo + Sheet | Luồng 8 | Tranh chấp hoa hồng, sai sót tính tay | Mỗi giới thiệu | Có (link tracking) |
| N9 | Tổng hợp thù lao GV thủ công cuối tháng | Luồng 7 | 1 ngày kế toán chi/tháng | Hàng tháng | Cần sync giờ dạy từ ClassIn |
| N10 | Đối soát SePay thủ công | Luồng 9 | ~2h kế toán thu/ngày | Hàng ngày | Có (auto-match) |
| N11 | Báo cáo P&L theo kỳ thi × cơ sở | Luồng 9 | Không có realtime view | Cuối tháng | Có (analytic dimensions) |
| N12 | Sự cố/khiếu nại trong nhóm Zalo | Luồng 6 | Không có lịch sử, không có SLA | Liên tục | Có (helpdesk system) |
| N13 | Báo cáo trạng thái theo thị trường/cơ sở | Quản trị đa thị trường | Lãnh đạo không có visibility realtime theo thị trường | Hàng ngày/tuần | Có (dashboard) |
| N14 | Giám sát và hỗ trợ tư vấn Sale/CTV thủ công trên CRM | Luồng 2 | Case tư vấn lặp lại, quản lý phải review từng lead thủ công, chất lượng tư vấn không đồng nhất | Hàng ngày | Một phần (QA checklist, case library, CRM review queue) |

### 5.1 Tải nhân công ước tính do các điểm nghẽn

| Điểm nghẽn | Ước tính tải nhân công/tháng |
|---|---|
| Onboarding sau thanh toán (N1–N4) | ~420 giờ/tháng (14h/ngày × 30 ngày) |
| Đối soát SePay (N10) | ~60 giờ/tháng (2h/ngày × 30 ngày) |
| Tính thù lao GV (N9) | ~8 giờ/tháng |
| Tính hoa hồng CTV (N8) | ~16 giờ/tháng |
| Giám sát tư vấn Sale/CTV (N14) | Chưa đo — hiện là thời gian ẩn của quản lý Sale/CTV |
| **Tổng tải dồn cho 4 điểm nghẽn đã định lượng** | **~504 giờ/tháng** (~63 ngày công/tháng), chưa gồm N14 |

---

## VI. MA TRẬN RỦI RO VẬN HÀNH

> Đánh giá rủi ro tại thời điểm Q2/2026, dựa trên hiện trạng đã mô tả ở các phần trên.

### 6.1 Ma trận rủi ro

| # | Rủi ro | Xác suất | Tác động | Mức độ tổng hợp | Trạng thái |
|---|---|---|---|---|---|
| R1 | 1 outsource dev phục vụ cả HN + HCM + 4 kỳ thi + chuyển đổi ClassIn | Rất cao | Rất cao | **Khủng hoảng tiềm ẩn** | Đang xảy ra |
| R2 | Dữ liệu trong Drive cá nhân — mất khi nhân sự nghỉ | Rất cao | Cao | **Nghiêm trọng** | Đang xảy ra |
| R3 | "Duyệt học sinh" 1 người — tắc nếu nghỉ | Cao | Cao | **Nghiêm trọng** | Đang xảy ra |
| R4 | Sale/CTV tracking thủ công — mạng lưới ~132–137 người → tranh chấp hoa hồng | Cao | Cao | **Cao** | Đang xảy ra |
| R5 | Mở rộng thị trường mới nhưng chưa có SOP/KPI/reporting thống nhất | Cao | Cao | **Cao** | Đang xảy ra |
| R6 | Lãnh đạo không có dashboard theo thị trường/cơ sở | Cao | Cao | **Cao** | Đang xảy ra |
| R7 | Spike khai giảng ở thị trường mới: ~1.300 HS/đợt, làm thủ công | Rất cao | Cao | **Cao** | Sắp xảy ra mỗi đợt khai giảng |
| R8 | Lead từ landing page bị nhập sót/trùng/chậm vào EZSale | Cao | Cao | **Cao** | Đang xảy ra |
| R9 | Lịch sử trao đổi với HS/lead nằm trong Zalo cá nhân Sale | Cao | Trung bình | **Trung bình** | Đang xảy ra |
| R10 | 20 người truyền thông (4 team) không có shared asset → branding lệch | Trung bình | Trung bình | **Trung bình** | Đang xảy ra |
| R11 | Không có audit log truy cập dữ liệu cá nhân học sinh | Trung bình | Cao (pháp lý) | **Cao** | Đang xảy ra |
| R12 | Sự cố vận hành xảy ra nhưng không được ghi nhận (mất khi đóng nhóm Zalo) | Cao | Trung bình | **Cao** | Đang xảy ra |
| R13 | Tư vấn Sale/CTV không nhất quán vì thiếu QA workflow và playbook case | Cao | Cao | **Cao** | Đang xảy ra |

### 6.2 Phân loại rủi ro theo nguồn gốc

**Rủi ro do thiếu nền tảng kỹ thuật:**
- R1 (1 dev), R3 (single person), R7 (không batch), R8 (không integration), R11 (không audit log)

**Rủi ro do thiếu quản trị dữ liệu:**
- R2 (Drive cá nhân), R9 (Zalo cá nhân), R12 (Zalo không lưu trữ)

**Rủi ro do thiếu hạ tầng phối hợp đa thị trường:**
- R5 (thiếu chuẩn vận hành khi mở rộng), R6 (thiếu visibility theo thị trường), R10 (lệch branding)

**Rủi ro do thiếu công cụ tracking attribution:**
- R4 (CTV manual), R8 (lead manual)

**Rủi ro do thiếu quản trị chất lượng tư vấn:**
- R9 (lịch sử tư vấn nằm ngoài CRM), R13 (không có QA workflow/playbook)

---

## VII. NỢ VẬN HÀNH (OPERATIONAL DEBT)

> Nợ vận hành = các hạng mục đáng lẽ phải có để vận hành quy mô hiện tại, nhưng chưa được xây dựng. Khác với rủi ro: nợ vận hành là *thiếu sót đã quan sát được*, không phải khả năng.

### 7.1 Documentation debt

| Hạng mục | Trạng thái | Tác động |
|---|---|---|
| Sơ đồ kiến trúc kỹ thuật toàn hệ thống | Không có | Khi outsource dev nghỉ → không ai onboard kế tiếp được |
| Tài liệu webhook (SePay, Zalo OA) | Không có | Phụ thuộc kiến thức trong đầu dev |
| Tài liệu cấu hình EZSale | Không có | Sale lead nghỉ → mất config |
| Playbook tư vấn Sale/CTV theo case | Chưa có | Case giống nhau phải được quản lý review và hướng dẫn lại nhiều lần |
| Checklist QA/review tư vấn trên CRM | Chưa có | Không biết lead nào đã được kiểm tra, lỗi tư vấn nào lặp lại, Sale/CTV nào cần coaching |
| SOP onboarding học sinh | Chưa được hệ thống hóa | Người mới onboard mất thời gian |
| SOP quản lý lớp hàng ngày | Chưa được hệ thống hóa | QLL CTV làm theo kinh nghiệm cá nhân |
| SOP xử lý sự cố | Chưa có | Mỗi sự cố xử lý ad-hoc |
| SOP phối hợp đa thị trường / đa cơ sở | Chưa có | Mỗi thị trường phải tự suy luận phạm vi tự chủ, báo cáo và escalation |
| SOP onboarding nhân sự mới | Chưa có | Nhân sự mới mất thời gian thích nghi |

### 7.2 Data debt

| Hạng mục | Trạng thái |
|---|---|
| Single source of truth cho học sinh | Không có (dữ liệu rải rác giữa Sheet, EZSale, Zalo) |
| Lịch sử tư vấn có thể audit | Không đầy đủ (một phần ở CRM, phần lớn ở Zalo cá nhân / gọi điện) |
| Taxonomy case tư vấn | Không có (chưa phân loại case theo học phí, chọn khóa, phản đối, hoàn tiền, so sánh đối thủ...) |
| Naming convention thống nhất (lớp, GV, CTV) | Không có |
| Bảng mapping `khóa học → lớp → GV → QLL` chuẩn hóa | Chưa có (nằm rải rác trong Sheet cá nhân) |
| Master data về danh mục khóa học, kỳ thi | Phân tán |
| Dữ liệu lịch sử học tập | Có thể phát sinh trong ClassIn nhưng chưa được đồng bộ về kho dữ liệu vận hành |
| Dữ liệu hành vi học (login, hoàn thành bài) | Có tiềm năng lấy từ ClassIn, nhưng chưa tích hợp API/data subscription |
| Lịch sử giao tiếp với HS/lead | Nằm trong Zalo cá nhân, không truy cập được tập trung |

### 7.3 Process debt

| Hạng mục | Trạng thái |
|---|---|
| SLA cho phản hồi lead Hot | Không có bằng văn bản |
| Quy trình QA/review tư vấn Sale/CTV | Không có |
| Quy trình escalation case tư vấn khó | Không có |
| SLA cho onboarding HS sau thanh toán | Không có |
| SLA cho phản hồi câu hỏi học viên | Không có |
| Quy trình leo thang sự cố | Không có |
| Quy trình audit hoa hồng Sale/CTV | Không có (chỉ tính tay cuối tháng) |
| Quy trình quản lý đổi GV trong khóa | Không có chính thức |
| Quy trình hoàn tiền | Có nhưng không thành SOP |

### 7.4 Tool debt

| Hạng mục cần | Trạng thái | Hệ quả |
|---|---|---|
| Google Workspace với domain công ty | Chưa có cho toàn 60 nhân sự | Email cá nhân vẫn là kênh nhận thông tin chính thức |
| Shared Drive theo cấu trúc tổ chức | Chưa có | File nằm trong Drive cá nhân |
| Tích hợp ClassIn API/data subscription | Chưa triển khai sâu | ClassIn có dữ liệu nhưng chưa tạo được trigger, dashboard, payroll hoặc chăm sóc tự động |
| Hệ thống ticket/helpdesk | Chưa có | Sự cố không được track có hệ thống |
| Dashboard vận hành QLL | Chưa có | QLL check từng nhóm Zalo để biết tình trạng |
| Dashboard COO/lãnh đạo | Chưa có | Không có realtime view |
| CRM review queue cho Sale/CTV | Chưa có | Quản lý phải tự lọc lead/case và review thủ công |
| Case library/playbook tư vấn | Chưa có | Kiến thức xử lý case không được tái sử dụng |
| Link tracking cho CTV | Chưa có | Attribution thủ công |
| Hệ thống ERP/kế toán tập trung | Chưa có | Kế toán dùng Sheet + Excel |
| Middleware Zalo OA ↔ hệ thống | Chưa có | Zalo OA gửi tay |

### 7.5 People debt

| Hạng mục | Trạng thái |
|---|---|
| Tech Ops (1 vị trí) | Chưa tuyển — outsource dev gánh toàn bộ |
| Backup cho "Duyệt học sinh" | Không có |
| Backup cho QLL Lead | Không có |
| Backup cho 1 outsource dev | Không có |
| Đào tạo nhân sự mới về SOP | Không có vì SOP chưa có |
| Đánh giá hiệu suất Sale/CTV (~132–137 người) | Không có cơ chế chính thức |
| Coaching chất lượng tư vấn Sale/CTV | Phụ thuộc quản lý trực tiếp, chưa có nhịp review/chỉ số/chủ đề đào tạo rõ ràng |

---

## VIII. PHỤ THUỘC VÀ SINGLE POINTS OF FAILURE

> Các điểm trong hệ thống vận hành mà nếu mất, toàn bộ chuỗi sẽ dừng hoặc suy giảm nghiêm trọng.

### 8.1 Single points of failure (SPOF) — Con người

| SPOF | Vai trò | Phạm vi ảnh hưởng nếu vắng |
|---|---|---|
| Outsource dev | Maintain toàn bộ hệ thống kỹ thuật | Toàn bộ automation (SePay, Web, EZSale integration) ngừng được sửa lỗi và phát triển |
| Chuyên viên "Duyệt học sinh" | Bước cốt lõi của onboarding | Toàn bộ chuỗi onboarding tắc — HS thanh toán xong không vào được lớp |
| QLL Lead | Điều phối 7 QLL CTV + lập lịch dạy | Lập lịch GV và phân bổ QLL theo lớp đình trệ |
| Quản lý CTV HN | 100 CTV Sale HN | Tracking và tính hoa hồng 100 CTV bị gián đoạn |
| Sale Lead HN | Quản lý 11 Sale offline + EZSale | Lead phân bổ chậm, EZSale không ai maintain |
| Kế toán tổng hợp | Báo cáo cuối tháng | Báo cáo bị trễ |

### 8.2 Single points of failure — Hệ thống & dữ liệu

| SPOF | Phạm vi ảnh hưởng nếu mất |
|---|---|
| SePay webhook | Không xác nhận được thanh toán → đơn hàng không được ghi nhận |
| Web portal `hsavnu.edu.vn` | Không nhận được lead mới + không thanh toán được |
| EZSale CRM | Sale không có view về pipeline lead |
| Google Drive cá nhân của từng nhân viên | Mất file/tài liệu thuộc về tổ chức |
| Google Sheet "Master danh sách HS" | Mất danh sách học sinh đã đăng ký |
| Zalo OA | Không gửi được thông báo tới HS |
| Zalo nhóm lớp | Mất kênh giao tiếp chính giữa GV-HS-QLL |

### 8.3 Phụ thuộc chéo giữa các luồng

```
Luồng 4 (Onboarding) phụ thuộc → Luồng 3 (SePay webhook)
Luồng 5 (Học tập)     phụ thuộc → Luồng 4 (HS đã onboard xong)
Luồng 6 (Chăm sóc)    phụ thuộc → Luồng 5 (data học tập ClassIn — *chưa sync tự động*)
Luồng 7 (Thù lao GV)  phụ thuộc → Luồng 5 (giờ dạy ClassIn/Zoom chuyển tiếp — *tính thủ công*)
Luồng 8 (Hoa hồng Sale/CTV) phụ thuộc → Luồng 1 (attribution — *thiếu link tracking*)
Luồng 9 (Kế toán)     phụ thuộc → Luồng 3, 7, 8 (mọi nguồn doanh thu/chi phí)
```

**Quan sát:** Phần lớn các phụ thuộc *hướng xuống* hiện đang được "bù" bằng nhân công thủ công thay vì kết nối hệ thống. Khi quy mô tăng, chi phí "bù bằng người" sẽ tăng tuyến tính theo số HS.

---

## IX. TÓM LƯỢC KẾT QUẢ ĐÁNH GIÁ

> Phần này tổng kết hiện trạng và mức độ nghiêm trọng của vấn đề. Quyết định giải pháp, thiết kế module, ngân sách và lộ trình triển khai chi tiết được tách sang tài liệu Odoo.

### 9.1 Năng lực vận hành hiện tại — Đánh giá theo 5 chiều

| Chiều | Trạng thái Q2/2026 |
|---|---|
| **Tự động hóa (Automation)** | Tự động hóa chỉ tồn tại ở khâu thanh toán. Toàn bộ chuỗi trước và sau thanh toán đều thủ công. |
| **Dữ liệu (Data)** | Phân mảnh trên 5–7 hệ thống không kết nối. ClassIn bắt đầu có dữ liệu học tập nhưng chưa sync sâu. Không có single source of truth. Phần lớn lưu trong Drive cá nhân. |
| **Quy trình (Process)** | Quy trình tồn tại trong đầu nhân sự. SOP bằng văn bản gần như không có cho phần lớn nghiệp vụ. |
| **Nhân sự kỹ thuật (Technical staffing)** | 1 outsource dev phục vụ toàn hệ thống. Không có Tech Ops hoặc đội kỹ thuật in-house. |
| **Quản trị đa cơ sở (Multi-branch governance)** | HN và HCM dùng chung quy trình thủ công nhưng không có cơ chế phối hợp, đồng bộ, hoặc visibility chính thức. |

### 9.2 Khả năng chịu tải hiện tại vs quy mô đang gánh

| Chiều | Năng lực hệ thống đáp ứng được | Quy mô đang gánh |
|---|---|---|
| Onboarding sau thanh toán | Thiết kế cho quy mô nhỏ — duy trì được bằng nhân công ~14h/ngày | ~55 HS/ngày |
| Quản lý lớp học | Đang chuyển sang ClassIn nhưng chưa có dashboard vận hành/API sync — QLL vẫn phải check ClassIn/Zalo/Sheet thủ công | ~600–700 lớp/năm |
| Quản lý GV online/GV chính HCM | Thù lao tính tay từ Sheet | ~70 GV online + 15 GV chính HCM |
| Quản lý mạng lưới Sale/CTV | Tracking + commission tay | ~132–137 người |
| Khai giảng HCM (spike) | Cùng quy trình thủ công, không có batch | 1.300 HS/đợt × 6 đợt/năm |

### 9.3 Đánh giá tóm lược

HSA Education hiện đang vận hành ở quy mô ~20.000 học sinh/năm trên một nền tảng kỹ thuật chỉ có **một điểm tự động hoạt động ổn định (thanh toán)**. ClassIn đang được đưa vào để thay dần Zoom ở lớp học live, nhưng chưa được tích hợp API/data sâu vào hệ thống quản trị. Toàn bộ chuỗi giá trị còn lại — từ thu lead, nurture, onboarding, quản lý lớp học, chăm sóc, đến tính thù lao và hoa hồng — vẫn được "bù" bằng nhân công thủ công.

Cơ cấu nhân sự hiện đã vượt mốc 300 người khi tính 62 fulltime/offline, ~70 GV online, 15 GV chính HCM và mạng lưới Sale/CTV/Marketing pha trộn fulltime-CTV. Phần lớn workload vẫn là việc lặp lại có thể tự động hóa được về nguyên tắc. Tỷ lệ 2.500 HS/QLL hiện hữu, ~1.110 HS/người nếu tính thêm đội vận hành lớp HCM, và ~132–137 người trong mạng lưới Sale/CTV chỉ duy trì được vì hệ thống chưa thực sự thực thi mức quản lý tương xứng.

Hai thị trường/cơ sở hiện chia sẻ cùng hệ thống công cụ thủ công nhưng chưa có cơ chế đồng bộ chính thức bằng văn bản, chưa có dashboard chung, và chưa có SOP chuẩn cho phối hợp đa thị trường. Điều này đặc biệt đáng chú ý khi HSA mở rộng sang thị trường HCM, nơi áp lực cạnh tranh cao hơn và đã có đối thủ dẫn đầu.

Mức nợ vận hành (operational debt) phát hiện được trải đều trên 5 chiều: documentation, data, process, tool, và people. Không có chiều nào đang ở trạng thái lành mạnh tương xứng với quy mô đang vận hành.

---

## X. ĐỊNH HƯỚNG XỬ LÝ VÀ CHUYỂN ĐỔI

> Phần này không phải lộ trình triển khai chi tiết. Mục tiêu là chốt cách tiếp cận: trước mắt xử lý tạm thời các vấn đề nghiêm trọng để giảm rủi ro vận hành; sau đó xây dựng định hướng giải pháp toàn diện với tầm nhìn tổng quan, nhưng triển khai theo từng bước nhỏ đủ kiểm soát.

### 10.1 Xử lý tạm thời các vấn đề nghiêm trọng

Các can thiệp ngắn hạn không nên được thiết kế như "hệ thống tương lai". Chúng chỉ nhằm giảm rủi ro đang xảy ra, tạo dữ liệu tối thiểu để quản lý, và tránh để các điểm nghẽn quan trọng phụ thuộc hoàn toàn vào một cá nhân.

| Vấn đề nghiêm trọng | Xử lý tạm thời cần làm | Mục tiêu ngắn hạn |
|---|---|---|
| Onboarding sau thanh toán phụ thuộc 1 người duyệt học sinh | Tạo backup duyệt học sinh, checklist xử lý trong ngày, tracker đơn thanh toán chưa onboard, nhịp kiểm tra cuối ngày | Không để HS thanh toán xong nhưng chậm vào lớp vì người phụ trách vắng mặt |
| ClassIn đang thay Zoom nhưng chưa có API/data sync | Chuẩn hóa mapping lớp - khóa - SBD - GV - QLL, quy định cách export/đối chiếu ClassIn tạm thời, chỉ định owner dữ liệu ClassIn | Có dữ liệu học tập tối thiểu để đối chiếu vận hành trước khi tích hợp sâu |
| Giám sát tư vấn Sale/CTV đang review thủ công từng case | Dùng tag/cột tạm trên CRM hoặc Sheet để đánh dấu case cần quản lý hỗ trợ, tạo playbook tư vấn bản đầu, ghi lại quyết định xử lý case lặp lại | Không phải review lại cùng một lỗi/case từ đầu cho nhiều Sale/CTV khác nhau |
| CTV tracking và hoa hồng đối soát tay | Chuẩn hóa mã CTV/ref code tạm, khóa format file tracking, đối soát định kỳ theo tuần thay vì dồn cuối tháng | Giảm tranh chấp attribution và giảm sai sót cuối tháng |
| Dữ liệu/tài liệu nằm trong Drive cá nhân | Gom tài liệu vận hành quan trọng vào thư mục tổ chức tạm, phân quyền tối thiểu, lập danh sách owner tài liệu | Giảm rủi ro mất dữ liệu khi nhân sự nghỉ hoặc đổi vai trò |
| 1 outsource dev là điểm phụ thuộc kỹ thuật | Lập inventory tài khoản, webhook, source/config quan trọng, change log, sơ đồ luồng kỹ thuật tối thiểu | Có khả năng bàn giao hoặc xử lý sự cố cơ bản khi dev chính không sẵn sàng |

### 10.2 Định hướng giải pháp toàn diện

Sau khi kiểm soát các rủi ro cấp bách, HSA cần thiết kế một định hướng giải pháp tổng thể thay vì vá từng điểm rời rạc. Định hướng này cần trả lời 5 câu hỏi ở cấp kiến trúc:

| Câu hỏi định hướng | Ý nghĩa |
|---|---|
| Đâu là nguồn dữ liệu chính cho học sinh, đơn hàng, thanh toán, lớp học và chăm sóc? | Tránh mỗi bộ phận giữ một bản dữ liệu khác nhau. |
| CRM sẽ chỉ là nơi lưu lead hay là hệ thống quản trị chất lượng tư vấn Sale/CTV? | Biến hoạt động tư vấn thành quy trình có review, playbook và dữ liệu coaching. |
| ClassIn chỉ là công cụ dạy học hay là nguồn dữ liệu học tập chính? | Kết nối attendance, login, bài tập, giờ dạy với chăm sóc học viên và thù lao GV. |
| Kế toán, hoa hồng CTV và thù lao GV sẽ lấy dữ liệu từ đâu? | Giảm đối soát tay và tránh báo cáo cuối tháng phụ thuộc Sheet rời rạc. |
| Dashboard quản trị cần nhìn theo chiều nào? | Tối thiểu cần nhìn theo kỳ thi, cơ sở/thị trường, lớp, Sale/CTV, GV và trạng thái onboarding/chăm sóc. |

Ở tầng nguyên tắc, giải pháp toàn diện cần có 4 lớp:

| Lớp giải pháp | Vai trò |
|---|---|
| **Process layer** | SOP, RACI, SLA, quy trình escalation, playbook tư vấn, chuẩn vận hành lớp học. |
| **Data layer** | Master data học sinh, khóa học, lớp, GV, QLL, CTV/ref code, SBD, mapping ClassIn. |
| **System layer** | CRM/ERP lõi, ClassIn, SePay, Zalo OA, dashboard, helpdesk/ticket, tài liệu tổ chức. |
| **Governance layer** | Owner dữ liệu, quyền truy cập, audit log, review định kỳ, backlog cải tiến và năng lực Tech Ops. |

### 10.3 Lộ trình theo nguyên tắc: tổng quan nhưng từng bước

Tầm nhìn tổng quan là xây một hệ thống vận hành có dữ liệu tập trung, quy trình rõ, và tự động hóa các điểm lặp lại. Cách thực hiện không nên là thay đổi toàn bộ cùng lúc, mà đi theo các bước có điều kiện hoàn thành rõ ràng.

| Giai đoạn | Trọng tâm | Kết quả cần đạt trước khi đi tiếp |
|---|---|---|
| **Bước 1 — Kiểm soát rủi ro ngay** | Backup người phụ trách, checklist tạm, tracker các case nghiêm trọng, gom tài liệu quan trọng | Không còn điểm nghẽn nào chỉ một người biết và một người làm |
| **Bước 2 — Chuẩn hóa dữ liệu và quy trình lõi** | SOP onboarding, QA tư vấn Sale/CTV, mapping ClassIn, ref code CTV, naming convention lớp/khóa | Các bộ phận dùng cùng một định nghĩa và cùng một cấu trúc dữ liệu tối thiểu |
| **Bước 3 — Tự động hóa hẹp nhưng có tác động cao** | Web form → CRM, SePay → trạng thái thanh toán, tạo SBD, gửi thông báo, review queue tư vấn, dashboard ngoại lệ | Giảm việc lặp lại hằng ngày mà chưa cần triển khai ERP toàn diện |
| **Bước 4 — Thiết kế hệ thống quản trị tập trung** | Đánh giá Odoo/ERP, target architecture, data migration, tích hợp ClassIn-SePay-Zalo OA | Có thiết kế tổng thể, ngân sách, owner và điều kiện go-live rõ |
| **Bước 5 — Rollout từng module** | CRM trước, kế toán/thanh toán sau, ClassIn data pipeline, helpdesk, hoa hồng/thù lao, dashboard | Mỗi module go-live xong phải ổn định trước khi mở module kế tiếp |

Tài liệu Odoo là tài liệu kế tiếp để phân tích tính phù hợp, thiết kế kiến trúc mục tiêu, ngân sách và lộ trình triển khai chi tiết cho định hướng này.

---

## XI. TÀI LIỆU LIÊN QUAN

| Tài liệu | Mục đích | Quan hệ với báo cáo này |
|---|---|---|
| [phan-tich-danh-gia-quy-trinh-hsa-education.md](phan-tich-danh-gia-quy-trinh-hsa-education.md) | Phân tích chi tiết quy trình hiện tại theo từng bộ phận | Nguồn tham khảo cho Phần IV |
| [danh-gia-classin-api-tich-hop.md](danh-gia-classin-api-tich-hop.md) | Đánh giá kỹ thuật ClassIn API và khả năng tích hợp | Nguồn tham khảo cho Phần IV.5, VII.4 |
| [danh-gia-phu-hop-odoo-va-lo-trinh-chuyen-doi-hsa-education-2026-2028.md](danh-gia-phu-hop-odoo-va-lo-trinh-chuyen-doi-hsa-education-2026-2028.md) | **Đánh giá phù hợp Odoo & Lộ trình chuyển đổi chi tiết** | **Tài liệu kế tiếp:** fit-gap, target architecture, migration plan, rollout plan và điều kiện triển khai cho các điểm nghẽn / rủi ro / nợ vận hành phát hiện trong báo cáo này |
| [HSA Education – Quy trình vận hành thực tế & lộ trình nâng cấp.html](HSA%20Education%20%E2%80%93%20Quy%20tr%C3%ACnh%20v%E1%BA%ADn%20h%C3%A0nh%20th%E1%BB%B1c%20t%E1%BA%BF%20%26%20l%E1%BB%99%20tr%C3%ACnh%20n%C3%A2ng%20c%E1%BA%A5p.html) | Sơ đồ trực quan luồng vận hành thực tế | Cross-reference cho Phần IV |

---

*Phiên bản 1.1 — Q2/2026 — As-Is Operations Analysis*
*Báo cáo này mô tả hiện trạng và chỉ đưa ra định hướng xử lý/chuyển đổi ở mức nguyên tắc. Đánh giá Odoo, thiết kế target architecture và lộ trình chuyển đổi chi tiết được trình bày trong tài liệu kế tiếp (xem mục XI).*
*Người chịu trách nhiệm cập nhật: Giám đốc vận hành (COO)*
*Lần đánh giá tiếp theo: Sau 6 tháng — để đo delta hiện trạng*
