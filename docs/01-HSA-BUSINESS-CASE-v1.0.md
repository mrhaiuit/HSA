# HSA EDUCATION — BUSINESS CASE
## Đề xuất Chuyển đổi Hệ thống Vận hành: Xây dựng HSA Integration Platform

---

| Trường | Giá trị |
|---|---|
| **Mã tài liệu** | HSA-BC-v1.4 |
| **Phiên bản** | 1.4 |
| **Ngày** | 2026-06-16 |
| **Người đề xuất** | Giám đốc Vận hành (COO) — vai trò Product Owner / người bảo trợ dự án |
| **Trình lên** | Hội đồng Quản trị — Thầy Hoa & Thầy Khương |
| **Trạng thái** | TRÌNH PHÊ DUYỆT |
| **Phân loại** | INTERNAL — CONFIDENTIAL |
| **Quyết định cần ra** | Phê duyệt chủ trương + ngân sách Giai đoạn 1 |

### Lịch sử phiên bản (Changelog)

| Phiên bản | Ngày | Thay đổi chính |
|---|---|---|
| 1.0 | 2026-06-16 | Bản gốc: hiện trạng, vấn đề, đề xuất, ROI, lộ trình. |
| 1.1 | 2026-06-16 | Bổ sung PHẦN 1B — Phân tích thị trường (TAM/SAM/Market Share); mở rộng PHẦN 2 (14 nút thắt N1–N14 + vấn đề nhóm Zalo); thêm PHẦN 2A — Các rủi ro ẩn dự báo tương lai (P1–P8); cập nhật PHẦN 0 với lập luận thị trường & Đà Nẵng. |
| **1.2** | **2026-06-16** | Tuyển CTO (50–100M/năm), timeline thực T7/2026–T3/2028, HCM ×2/×1.5, thêm PHẦN 2B bảo mật. |
| **1.3** | **2026-06-16** | B0 promo code revenue leak; hành trình 8 bước học sinh; Zalo approval SPOF; dữ liệu phân tán không cứu vãn; giá 2–3M/HS; bảng scale vs UX; P9 phụ huynh — doanh thu ẩn 3,2–10,8 tỷ/năm bị bỏ ngỏ. |
| **1.4** | **2026-06-16** | So sánh giải pháp A (Odoo) vs B (MISA) tại 3.5.1; Đà Nẵng → giả định tiềm năng cần nghiên cứu thực địa; nhấn mạnh CTO chưa có & là điều kiện tiên quyết (lương đổi sang /tháng) + đội kỹ thuật đầy đủ; thêm rủi ro P10 (công nghệ tự phát); tính lại chi phí hao phí thực tế ~1,4–2,4 tỷ/năm (Mục 4.1.1); cập nhật ngân sách GĐ 0+1 theo chi phí team thực tế. |

---

# PHẦN 0 — TÓM TẮT ĐIỀU HÀNH

> **Đề nghị BGĐ đọc trọn vẹn phần này. Nếu thời gian hạn chế, phần này đủ để ra quyết định.**

## Vấn đề cốt lõi (1 câu)

Toàn bộ vận hành của HSA Education đang chạy trên **7 công cụ rời rạc không kết nối với nhau**, buộc nhân sự phải làm tay khoảng **504 giờ công mỗi tháng (~63 ngày công)** cho các việc lặp đi lặp lại, đồng thời treo doanh nghiệp trên nhiều "điểm lỗi đơn" — chỉ một nhân sự nghỉ là cả chuỗi nhập học có thể tắc.

## Đề xuất (1 câu)

Xây dựng **HSA Platform** — một nền tảng do **CTO nội bộ phát triển** — để **tự động hóa toàn bộ chuỗi từ lúc học sinh thanh toán đến lúc sẵn sàng vào lớp (rút từ ~15 phút thủ công xuống dưới 2 phút tự động, 24/7)**, với **PostgreSQL HSA Platform** làm nguồn dữ liệu duy nhất cho mọi nghiệp vụ, **.NET Finance Service** xử lý hoa hồng/thù lao/đối soát, và **MISA SME Online (~3–6 triệu/năm)** làm phần mềm kế toán chính thức (nhận sync 1 chiều từ .NET).

## Bức tranh tài chính

| Hạng mục | Con số |
|---|---|
| Chi phí hạ tầng tiền mặt năm 1 | ~109–127 triệu VND (gồm MISA SME ~3–6 triệu/năm) |
| Lương CTO nội bộ (năm 1) | ~50–100 triệu VND/tháng |
| Đội ngũ kỹ thuật đầy đủ (CTO + dev + fresher + UI/QC) | ~105–175 triệu/tháng ≈ ~1.260–2.100 triệu/năm (Mục 5.2) |
| Chi phí nếu thuê agency thay thế | **~500–800 triệu** (phát triển) + 100–160 triệu/năm bảo trì |
| **Tiết kiệm nhân công đo trực tiếp 2025** | **~595 triệu VND/năm** (tăng ~712 triệu 2026, ~937 triệu 2027) |
| **Chi phí hao phí thực tế đầy đủ** (nhân công + cơ hội lãnh đạo + rò rỉ doanh thu + B0) | **~1.400–2.400 triệu VND/năm** (Mục 4.1.1) |
| **Thời gian hoàn vốn (Payback)** | **~3–4 tháng (kịch bản thận trọng); nhanh hơn khi tính đủ hao phí** |

## Vì sao phải làm NGAY (không chờ)

1. **Trải nghiệm học viên đang chịu thiệt:** học sinh đã trả tiền vẫn phải chờ **2–8 giờ** mới nhận được số báo danh và quyền vào lớp, tùy giờ hành chính.
2. **Rủi ro vận hành đang tích lũy:** chỉ **1 chuyên viên** duyệt học sinh vào lớp — người này nghỉ 1 ngày, toàn bộ học sinh đã thanh toán bị kẹt ngoài lớp.
3. **Không thể scale — và tải đang tăng mạnh:** cơ sở HCM có những đợt khai giảng **~260 học sinh/ngày** hiện tại; BGĐ đã xác nhận **HCM sẽ tăng gấp đôi năm 2026 và gấp 1,5 lần năm 2027** — đỉnh tải dự kiến lên **~520 HS/ngày (2026)** rồi **~780 HS/ngày (2027)**. Quy trình làm tay không gánh nổi tải này.
4. **BGĐ đang điều hành "mù":** chưa có bảng số liệu thời gian thực — mọi quyết định dựa trên báo cáo tổng hợp chậm.
5. **Thị trường đang tăng nhanh nhất trong lịch sử:** ĐGNL HCM tăng **+34,3% YoY (2025)**; HSA hiện đang chiếm **~21% SAM** (phân khúc học sinh có mua khóa ôn luyện) — đây là thời điểm chiến lược để **scale**, không phải giữ nguyên. Bỏ lỡ cửa sổ tăng trưởng này là nhường thị phần cho đối thủ số hóa nhanh hơn.
6. **Thị trường Đà Nẵng — giả định tiềm năng (cần nghiên cứu thực địa):** Đà Nẵng có **~11.500 thí sinh THPT/năm** — một **giả định tiềm năng** về thị trường thứ 3, **chưa phải kế hoạch đã chốt**. Với hệ thống tự động hóa, mở cơ sở mới chỉ cần "cắm vào là chạy"; không có hệ thống, mỗi lần mở rộng là khởi tạo lại toàn bộ việc tay từ đầu.
>
> **Lưu ý:** Đà Nẵng chưa được nghiên cứu thực địa. Đây là cơ hội giả định dựa trên quy mô thị trường ước tính. Quyết định mở Đà Nẵng cần phân tích riêng biệt về: nhu cầu tuyển sinh thực tế, chi phí vận hành tại chỗ, đội ngũ địa phương.

## Đề nghị cụ thể với BGĐ

1. **Phê duyệt chủ trương** xây dựng HSA Integration Platform theo lộ trình 4 giai đoạn / 18 tháng (T8/2026 – T3/2028).
2. **Phê duyệt ngân sách Giai đoạn 0 + 1 (T8–12/2026):** hạ tầng máy chủ + đăng ký kênh tin nhắn Zalo + nhân lực kỹ thuật 5 tháng đầu. Hai kịch bản: **~278–538 triệu** (chỉ CTO) hoặc **~553–913 triệu** (đội ngũ kỹ thuật đầy đủ) — chi tiết tại Mục 6.1.
3. **Phê duyệt tuyển dụng CTO nội bộ — điều kiện tiên quyết:** HSA **hiện CHƯA CÓ CTO**. Tuyển CTO là **điều kiện tiên quyết cho toàn bộ lộ trình** — không có CTO thì không khởi động được bất kỳ giai đoạn nào. CTO chịu trách nhiệm phát triển và vận hành kỹ thuật; **COO đóng vai Product Owner** (xác nhận yêu cầu, phê duyệt thiết kế, đánh giá kết quả — **KHÔNG làm kỹ thuật**). Timeline: BGĐ phê duyệt **T7/2026** → **ngay lập tức bắt đầu tuyển CTO** → mục tiêu onboard **T8/2026**.
4. **Lịch rà soát:** checkpoint sau 30 ngày và sau 90 ngày, BGĐ quyết định có tiếp tục Giai đoạn 2 hay không dựa trên kết quả đo được.

> **Tóm lại:** Bỏ ra ~278–538 triệu (chỉ CTO) hoặc ~553–913 triệu (đội kỹ thuật đầy đủ) cho giai đoạn đầu để kiểm chứng, đổi lấy việc chặn **chi phí hao phí thực tế ~1,4–2,4 tỷ/năm** (gồm nhân công, chi phí cơ hội lãnh đạo, rò rỉ doanh thu onboarding, lỗ hổng B0) và loại bỏ các rủi ro có thể làm tắc nghẽn cả doanh nghiệp. Rủi ro tài chính nhỏ, lợi ích lớn, hoàn vốn trong ~3–4 tháng. Quan trọng hơn: đây là **điều kiện tiên quyết để giữ ~21% thị phần SAM và mở khả năng mở rộng sang các thị trường mới như Đà Nẵng (giả định tiềm năng, cần nghiên cứu thực địa)** trong khi thị trường đang tăng trưởng hai chữ số.

---

# PHẦN 1 — HIỆN TRẠNG: CHÚNG TA ĐANG Ở ĐÂU?

## 1.1 Quy mô vận hành

HSA Education hôm nay không còn là một trung tâm nhỏ. Quy mô hiện tại:

| Chỉ số | Giá trị |
|---|---|
| Học sinh phục vụ | **~20.000 học sinh/năm** |
| Sản phẩm thi | **4 kỳ:** ĐGNL HSA, BCA (Bộ Công an), BQP (Bộ Quốc phòng), ĐGNL HCM |
| Cơ sở | **2:** Hà Nội + TP. Hồ Chí Minh |
| Tổng nhân lực | **> 300 người** |
| Nhân sự fulltime | **62 người** (HN 50, HCM 12) |
| Giảng viên online | **~70 giảng viên** |
| Cộng tác viên / Sale | **132–137 người** |
| Học sinh nhập học mới/ngày | **~55 học sinh** (HN ~33 + HCM ~22) |
| Số lớp vận hành/năm | **~600–700 lớp** |

> **Dự báo tăng trưởng đã được BGĐ xác nhận:** HCM **tăng gấp đôi năm 2026** và **gấp 1,5 lần năm 2027 so với 2026**. Hệ quả lên quy mô vận hành:
>
> | Năm | HCM | HN | **Tổng/năm** | **TB/ngày** | **Đỉnh/ngày** |
> |---|---|---|---|---|---|
> | 2025 (baseline) | ~8.000 | ~12.000 | **~20.000** | **~55** | **~260** |
> | 2026 (HCM ×2) | ~16.000 | ~12.000 | **~28.000** | **~77** | **~520** |
> | 2027 (HCM ×1,5) | ~24.000 | ~13.000 | **~37.000** | **~101** | **~780** |
>
> Tải onboarding tay (15 phút/HS): 2025 ~13,75h/ngày → 2026 ~19,25h/ngày (**+40%**) → 2027 ~25,25h/ngày (**+84%**). Với tự động hóa, chi phí biên cho mỗi học sinh tăng thêm gần **bằng không**.

> **Điểm cần BGĐ ghi nhận:** quy mô này tương đương một doanh nghiệp tầm trung. Nhưng **bộ máy vận hành phía sau vẫn đang chạy như một trung tâm nhỏ** — chủ yếu bằng tay, bằng file Excel và bằng Zalo cá nhân.

## 1.2 Bức tranh hệ thống hiện tại

Hiện tại HSA đang dùng **7 công cụ riêng lẻ, không nói chuyện được với nhau**. Dữ liệu phải được con người chép tay qua lại giữa các công cụ này:

```
   [ Marketing ]                                    [ Học sinh ]
        │  nhập tay (có độ trễ)                          │
        ▼                                                ▼
 ┌─────────────┐    ┌─────────────┐    ┌──────────────────────┐
 │ Google Sheet│    │  EZSale CRM │    │  Web portal hsavnu   │
 │ (lead, DS HS│    │  (tư vấn)   │    │  (đăng ký, giỏ hàng) │
 │ thù lao GV) │    └─────────────┘    └──────────┬───────────┘
 └─────────────┘                                  │ thanh toán
        ▲                                          ▼
        │ chép tay                          ┌─────────────┐
        │                                   │    SePay    │ ◀── ĐIỂM TỰ ĐỘNG
        │                                   │ (thanh toán)│     DUY NHẤT
   [ Nhân viên ]  ── tạo SBD tay ──┐        └──────┬──────┘
        │                          │               │ (thông báo tiền về)
        │ duyệt từng HS            ▼               ▼
        │                   ┌─────────────┐  ┌─────────────┐
        ▼                   │   ClassIn   │  │  Zalo OA +  │
 ┌─────────────┐            │ (lớp học,   │  │  Zalo nhóm  │
 │ Google Drive│            │  đang thay  │  │ (1.800–2.100│
 │ cá nhân từng│            │   Zoom)     │  │   nhóm)     │
 │  nhân viên  │            └─────────────┘  └─────────────┘
 └─────────────┘
```

**7 công cụ đang dùng:**

| # | Công cụ | Vai trò | Mức tự động |
|---|---|---|---|
| 1 | Web portal hsavnu.edu.vn | Đăng ký, giỏ hàng | Một phần |
| 2 | SePay | Thanh toán | **Tự động (duy nhất)** |
| 3 | EZSale CRM | Quản lý tư vấn Sale | Thủ công nhập liệu |
| 4 | Google Sheet | Danh sách HS, thù lao GV, hoa hồng CTV | **100% thủ công** |
| 5 | Zalo OA + Zalo nhóm | Liên lạc, group lớp | Thủ công |
| 6 | ClassIn | Lớp học online (đang chuyển từ Zoom) | Thủ công kích hoạt |
| 7 | Google Drive cá nhân | Lưu dữ liệu từng nhân viên | **Không kiểm soát** |

> **Vấn đề gốc:** giữa 7 công cụ này không có "đường ống" tự động nào (trừ SePay). Mọi mắt xích còn lại đều cần một con người ngồi chép, dán, gõ, duyệt. Đó chính là nơi sinh ra 504 giờ công lãng phí mỗi tháng.

## 1.3 Con số: 504 giờ nhân công mỗi tháng bị "đốt" vào việc lặp lại

Đây là phần quan trọng nhất của hiện trạng. Chúng tôi đã **đo lường thực tế** khối lượng việc tay lặp đi lặp lại:

| Đầu việc lặp lại | Cách đo | Khối lượng |
|---|---|---|
| **Onboarding học sinh mới** (tạo SBD, gửi email, kích hoạt ClassIn, duyệt vào nhóm Zalo) | 55 HS/ngày × 15 phút/HS | **~14 giờ/ngày** ≈ 290 giờ/tháng |
| **Đối soát thanh toán SePay** (kế toán thu khớp tiền từng giao dịch) | ~2 giờ/ngày | ~44 giờ/tháng |
| **Tính thù lao giảng viên** (kế toán chi, ~70 GV) | ~1 ngày công/tháng | ~8 giờ/tháng |
| **Tính hoa hồng CTV** (132–137 người) | ~2 ngày công/tháng | ~16 giờ/tháng |
| | | |
| **TỔNG TẢI LẶP LẠI** | | **~504 giờ/tháng ≈ 63 ngày công/tháng** |

> **Diễn giải cho BGĐ:** 63 ngày công/tháng nghĩa là **HSA đang phải trả lương cho khoảng 3 nhân sự fulltime chỉ để làm những việc mà máy tính có thể tự làm trong vài giây.** Đây không phải việc tạo ra giá trị — đây là việc "vận hành để hệ thống không sập".

## 1.4 Ba rủi ro nghiêm trọng nhất

Trong 13 rủi ro đã được phân tích, có 3 rủi ro thuộc loại **"điểm lỗi đơn"** (chỉ cần một mắt xích hỏng là cả chuỗi dừng) — đây là loại rủi ro nguy hiểm nhất với một doanh nghiệp quy mô 20.000 học sinh:

| Mã | Rủi ro | Vì sao nguy hiểm |
|---|---|---|
| **R1** | **Chỉ 1 lập trình viên outsource** gánh toàn bộ phần kỹ thuật | Người này nghỉ/ngừng hợp tác → toàn bộ hệ thống không ai sửa được, không ai hiểu được |
| **R2** | **Dữ liệu học sinh nằm trong Google Drive cá nhân** của từng nhân viên | Nhân viên nghỉ việc hoặc xóa file → **dữ liệu mất vĩnh viễn, không thể cứu vãn**; ngoài ra Drive cá nhân dễ bị hack qua tài khoản Google cá nhân yếu → lộ dữ liệu hàng chục nghìn học sinh |
| **R3** | **Chỉ 1 chuyên viên "duyệt học sinh"** vào lớp | Người này nghỉ 1 ngày → **toàn bộ học sinh đã thanh toán bị kẹt ngoài lớp**, phát sinh khiếu nại hàng loạt |

> **Bản chất:** doanh nghiệp 20.000 học sinh/năm đang được "giữ thăng bằng" trên vài cá nhân. Đây là rủi ro mà BGĐ không thể chấp nhận kéo dài.

## 1.5 Khoảng cách năng lực

| Năng lực mà quy mô hiện tại ĐÒI HỎI | Năng lực HSA ĐANG CÓ |
|---|---|
| Nhập học tự động, tức thời, 24/7 | Làm tay, chỉ trong giờ hành chính |
| Một nguồn dữ liệu thống nhất | 7 công cụ rời rạc + Drive cá nhân |
| Dashboard thời gian thực cho BGĐ | Báo cáo tổng hợp thủ công, có độ trễ |
| Tính hoa hồng/thù lao minh bạch, tự động | Tính tay 3 ngày công/tháng, hay tranh chấp |
| Không phụ thuộc cá nhân (no SPOF) | Phụ thuộc 3+ cá nhân then chốt |
| Chịu được tải spike 260 HS/ngày | Không chịu nổi |

---

# PHẦN 1B — PHÂN TÍCH THỊ TRƯỜNG

> **Mục tiêu phần này:** đặt đề xuất chuyển đổi vào bối cảnh thị trường thực tế, để BGĐ thấy rằng đây không chỉ là một dự án "tiết kiệm chi phí nội bộ", mà là **điều kiện tiên quyết để giữ vị thế và scale trong một thị trường đang tăng trưởng hai chữ số.**

## 1B.1 Thị trường theo kỳ thi (số liệu thực tế 2024–2025)

HSA hoạt động trên thị trường ôn luyện cho 4 kỳ thi đánh giá năng lực / xét tuyển riêng. Dưới đây là quy mô thí sinh thực tế của từng kỳ:

| Kỳ thi | 2024 | 2025 | Tăng trưởng YoY |
|---|---|---|---|
| **ĐGNL HSA** (ĐHQG Hà Nội) | 104.575 thí sinh | ~90.632+ (đến tháng 6/2026, đang cập nhật) | Đang cập nhật |
| **ĐGNL HCM** (ĐHQG TP.HCM) | ~107.000 | ~152.800 (vòng 1 đã có 128.338, +34,3%) | **+34,3%** |
| **BCA** (Bộ Công an) | ~18.000 | ~23.000 | **+27,8%** |
| **BQP** (Bộ Quốc phòng) | ~3.200 chỉ tiêu | ~3.200 | Ổn định |

> **Tổng thị trường kỳ thi 2025: ~269.000–275.000 thí sinh/năm.**

**Dự báo tăng trưởng ĐGNL HCM (động lực chính, +34,3%/năm):**

| Kỳ thi | 2025 | Dự báo 2026 | Dự báo 2027 |
|---|---|---|---|
| **ĐGNL HCM** | ~152.800 | **~205.000** (+34,3%) | **~275.000** (+34,3%) |

> Riêng cơ sở HSA tại HCM bám theo đà này còn mạnh hơn thị trường chung: BGĐ xác nhận HCM HSA **gấp đôi năm 2026** và **gấp 1,5 lần năm 2027** (từ ~8.000 → ~16.000 → ~24.000 HS/năm).

**Nhận xét nhanh:**
- ĐGNL HCM là động lực tăng trưởng mạnh nhất: **+34,3% YoY** — riêng vòng 1 năm 2025 đã đạt 128.338 thí sinh, vượt cả tổng năm 2024.
- BCA tăng **+27,8%** — phản ánh xu hướng các kỳ thi riêng ngày càng phổ biến và cạnh tranh.
- ĐGNL HSA (sân nhà của HSA) vẫn là kỳ thi lớn nhất miền Bắc với hơn 100.000 thí sinh.
- BQP ổn định do bị giới hạn bởi chỉ tiêu tuyển sinh quốc phòng.

## 1B.2 Thị trường theo địa lý (thí sinh THPT 2024)

Để hiểu tiềm năng mở rộng địa lý, cần nhìn vào phân bố thí sinh THPT tốt nghiệp — nguồn đầu vào của tất cả các kỳ thi đánh giá năng lực:

| Địa phương | Số thí sinh THPT 2024 | Ghi chú |
|---|---|---|
| **Toàn quốc** | **1.067.391 thí sinh** | Tổng tốt nghiệp THPT 2024 |
| **Hà Nội** | **109.078 thí sinh** | **10,2% cả nước — địa phương đông nhất** |
| **TP.HCM** | **88.196 thí sinh** | Thị trường lớn thứ 2 |
| **Đà Nẵng** | ~11.500–12.000 thí sinh (ước tính) | Xếp hạng ~45 toàn quốc; tỉnh trực thuộc TW trung bình ~10.000–13.000. **Giả định tiềm năng — chưa nghiên cứu thực địa** |

**Bối cảnh ngành EdTech:**

| Chỉ số | Giá trị | Dự báo |
|---|---|---|
| **EdTech Vietnam 2024** | **$1 tỷ USD** | $3 tỷ USD vào 2033 (**CAGR 12,96%**) |
| **Online Education Vietnam revenue 2025** | **~397 triệu USD** | 627 triệu USD năm 2029 (**CAGR 12,08%**) |

> **Đọc cho BGĐ:** HSA đang đặt hai cơ sở (HN + HCM) đúng tại hai địa phương đông thí sinh nhất cả nước — một lựa chọn chiến lược đúng. Nhưng **Đà Nẵng (~11.500 thí sinh/năm) hiện là mảng trắng** — HSA chưa có cơ sở. **Đây là giả định tiềm năng, chưa được nghiên cứu thực địa**; quyết định mở Đà Nẵng cần phân tích riêng về nhu cầu tuyển sinh thực tế, chi phí vận hành tại chỗ và đội ngũ địa phương. Ngành EdTech tăng trưởng ~13%/năm sẽ kéo theo nhiều đối thủ mới; cửa sổ giành thị phần đang mở nhưng sẽ không mở mãi.

## 1B.3 TAM / SAM / Market Share của HSA (phân tích định lượng)

Đây là phần định lượng quan trọng nhất, giúp BGĐ thấy chính xác HSA đang đứng ở đâu trên thị trường.

### TAM — Total Addressable Market (tổng thị trường có thể tiếp cận)

| Thành phần | Con số |
|---|---|
| Tổng thí sinh đăng ký 4 kỳ thi 2025 | **~269.000–275.000 người/năm** |
| Tỉ lệ thí sinh có mua khóa ôn luyện (ước tính) | **30–40%** |
| **SAM (Serviceable Addressable Market)** | **~80.700 – 110.000 học sinh/năm** |
| Quy ra giá trị (đơn giá TB **~2–3 triệu/HS**, trung bình 2,5 triệu) | **~200 – 275 tỷ VND/năm** |

### Market Share hiện tại của HSA

| Chỉ tiêu | Phép tính | Kết quả |
|---|---|---|
| Học sinh HSA phục vụ | — | ~20.000 HS/năm |
| **Tỉ lệ trên tổng thí sinh (TAM)** | 20.000 / ~272.000 | **~7,4%** |
| **Tỉ lệ trên SAM (HS có mua khóa học)** | 20.000 / ~95.000 | **~21%** — con số đáng kể |
| **Doanh thu ước tính HSA** | 20.000 HS × ~2–3 triệu VND | **~40–60 tỷ VND/năm** |

> **Điểm then chốt:** trên phân khúc thực sự quan trọng — học sinh **có chi tiền mua khóa ôn luyện** — HSA đã chiếm **~21% SAM.** Đây là vị thế dẫn đầu thị trường, không phải một người chơi nhỏ. Giữ và mở rộng vị thế này là ưu tiên chiến lược số 1.

### Tiềm năng tăng trưởng (2025–2028)

| Hướng tăng trưởng | Phân tích |
|---|---|
| **ĐGNL HCM bùng nổ** | Tăng 34,3%/năm → thị trường HCM dự kiến **~200.000+ thí sinh vào 2026** |
| **Đà Nẵng — mảng trắng (giả định)** | ~11.500 thí sinh/năm — HSA **hiện CHƯA CÓ cơ sở** → cơ hội **giả định tiềm năng, chưa nghiên cứu thực địa**, cần phân tích riêng trước khi quyết định |
| **Nâng market share lên 10%** | 272.000 × 10% = **27.200 HS** → **tăng 36%** so với hiện tại (20.000) |
| **Đòn bẩy tự động hóa** | Với hệ thống tự động, **không phải tăng nhân công vận hành theo tỉ lệ tuyến tính** → biên lợi nhuận tăng khi scale |
| **TAM/SAM 2026–2027** | ĐGNL HCM +34,3%/năm kéo tổng thị trường kỳ thi lên **~330.000 (2026)** và **~400.000+ (2027)** → SAM tăng lên **~100.000–130.000 (2026)** và **~120.000–160.000 (2027)** học sinh có mua khóa |

> **Phép tính đơn giản cho BGĐ:** chỉ cần nâng thị phần từ ~7,4% lên 10% trên tổng thị trường là HSA tăng **36% số học sinh** mà không cần thị trường phải mở rộng thêm. Nhưng điều này **bất khả thi với quy trình thủ công** — vì thêm học sinh hiện đồng nghĩa với thêm người làm tay.

## 1B.4 Nhận xét chiến lược từ dữ liệu thị trường

> Bốn kết luận BGĐ cần ghi nhớ từ phần phân tích thị trường này:

1. **Thị trường đang TĂNG NHANH** — đặc biệt ĐGNL HCM **+34,3%**. Đây là **thời điểm chiến lược**: ai số hóa và scale nhanh hơn sẽ giành phần tăng trưởng; ai chậm sẽ mất thị phần ngay cả khi giữ nguyên số học sinh tuyệt đối.

2. **HSA đã chiếm ~21% SAM** — vị thế dẫn đầu. **Việc giữ vị thế và scale là ưu tiên số 1**, không phải phòng thủ thụ động.

3. **Nút thắt cổ chai vận hành thủ công = TRẦN TĂNG TRƯỞNG.** Không thể thêm học sinh nếu không thêm người. Đây là giới hạn vật lý của mô hình hiện tại — và nó chặn đứng mọi tham vọng nâng thị phần.

4. **Mở rộng sang thị trường mới (như Đà Nẵng) không thể thực hiện với hệ thống vận hành thủ công hiện tại.** Đà Nẵng ở đây là **giả định tiềm năng, chưa nghiên cứu thực địa** — chưa phải kế hoạch chắc chắn. Dù mở ở đâu, mỗi cơ sở mới = khởi tạo lại toàn bộ việc tay từ đầu, chi phí vận hành/HS cao, biên lợi nhuận thấp. Chỉ khi quy trình được chuẩn hóa và tự động hóa, mỗi cơ sở mới mới có thể "cắm vào là chạy".

---

# PHẦN 2 — VẤN ĐỀ: TẠI SAO PHẢI THAY ĐỔI NGAY?

## 2.1 Trải nghiệm học viên đang bị ảnh hưởng (phân tích sâu)

Khi một học sinh (hoặc phụ huynh) chuyển tiền học, kỳ vọng tự nhiên là **được vào lớp ngay**. Thực tế hiện nay:

- Học sinh thanh toán xong → phải **chờ một nhân viên rảnh tay** mới được tạo số báo danh, gửi email hướng dẫn, kích hoạt ClassIn và duyệt vào nhóm Zalo.
- Độ trễ thực tế: **2–8 giờ**, tùy thời điểm thanh toán rơi vào trong hay ngoài giờ hành chính. Thanh toán buổi tối hoặc cuối tuần có thể chờ đến hôm sau.

**Tác động tâm lý (chưa được định lượng nhưng có thật):**
- **Phụ huynh lo lắng:** đã chuyển tiền nhưng "không thấy gì" trong nhiều giờ → nhắn tin hỏi, gọi điện, gây tải ngược lên đội chăm sóc.
- **Học sinh nghi ngờ (doubt):** "Mình chuyển nhầm? Trung tâm có nhận được không? Có phải lừa đảo không?" — đặc biệt với khách hàng lần đầu.
- **Khả năng hủy:** trong khoảng chờ đó, học sinh có thể đổi ý, so sánh với đối thủ, hoặc yêu cầu hoàn tiền.

**Rủi ro refund trong giờ cao điểm:**
- Đợt khai giảng HCM: **260 HS/ngày**. Nếu mỗi em chờ trung bình **8 giờ** trong giờ cao điểm → đây là một **khủng hoảng dịch vụ khách hàng** quy mô lớn: hàng trăm phụ huynh cùng lo lắng, cùng nhắn tin, một số yêu cầu hoàn tiền — đúng vào tuần nhạy cảm nhất.
- Mỗi yêu cầu refund không chỉ mất doanh thu mà còn **mất uy tín lan truyền** (truyền miệng tiêu cực trong cộng đồng phụ huynh).

**So sánh với tiêu chuẩn thị trường:**
- Các trung tâm đã số hóa đang onboard học sinh **dưới 5 phút** (tự động cấp tài khoản, gửi hướng dẫn, vào lớp ngay sau khi thanh toán).
- Độ trễ 2–8 giờ của HSA đang **tụt hậu so với chuẩn ngành** — và khoảng cách này ngày càng lộ rõ khi học sinh/phụ huynh có cơ sở so sánh.

**Hành trình thực tế của học sinh sau khi thanh toán (8 bước thủ công):**

> Đây là quy trình **học sinh phải trải qua hiện tại** — mỗi bước là một ma sát không cần thiết:

1. Học sinh thanh toán qua SePay (tự động)
2. **Chờ nhân viên rảnh** nhận thông báo (2–8 giờ, ngoài giờ hành chính là hôm sau)
3. Nhân viên mở Google Sheet, tạo SBD thủ công theo quy tắc đặt mã — dễ sai, dễ trùng, không ai kiểm tra tự động
4. COO có viết một script gửi email hướng dẫn vào lớp — nhưng script này **vẫn cần người kích hoạt thủ công**, không tự chạy sau thanh toán
5. Học sinh nhận email, **phải điền thêm một form khai báo thông tin** (tên thật, ngày sinh, trường, SBD tự khai…) — phức tạp, dễ điền sai, gây cảm giác "đã trả tiền còn phải làm thêm giấy tờ"
6. Nhân viên kích hoạt ClassIn thủ công dựa trên thông tin form vừa điền
7. Học sinh nhận mã kích hoạt, tự vào ClassIn
8. **Nhân viên duyệt học sinh vào nhóm Zalo lớp** — nhưng không có công cụ đối chiếu tên Zalo với tên thật và SBD đã đăng ký → gần như ai request là duyệt ngay (xem thêm Mục 2.3)

**Hệ quả của quy trình 8 bước này:**
- Học sinh phải thực hiện **5–6 bước sau khi đã trả tiền** → trải nghiệm nặng nề, không chuyên nghiệp
- Tỉ lệ điền form sai hoặc không điền → nhân viên phải gọi điện xác nhận thêm → tốn thêm thời gian cả hai phía
- **Khi số lượng học sinh tăng (HCM ×2 năm 2026, ×3 năm 2027), tỉ lệ sai sót tăng theo tỉ lệ thuận** — mỗi bước thủ công là một điểm có thể sai, và càng nhiều người làm càng nhiều cách làm khác nhau

**Mối quan hệ phi tuyến: quy mô tăng → trải nghiệm suy giảm:**

| Quy mô HS | Số lượng bước tay/ngày | Tỉ lệ sai sót ước tính | Trải nghiệm học viên |
|---|---|---|---|
| 20.000 HS/năm (hiện tại) | ~550 thao tác/ngày | ~3–5% (15–25 HS/ngày bị delay/sai) | Kém, có thể chấp nhận |
| 28.000 HS/năm (2026, HCM ×2) | ~770 thao tác/ngày | ~5–8% (40–60 HS/ngày) | **Xấu**, phát sinh khiếu nại thường xuyên |
| 37.000 HS/năm (2027, HCM ×3) | ~1.010 thao tác/ngày | **>10%** (100+ HS/ngày) | **Không thể chấp nhận** — rủi ro danh tiếng nghiêm trọng |

> **Kết luận 2.1:** tốc độ onboarding không phải tiểu tiết kỹ thuật — nó là **một phần trực tiếp của uy tín thương hiệu** và là điểm rò rỉ doanh thu (refund) trong giờ cao điểm. Quan trọng hơn: **trải nghiệm học viên không chỉ đang xấu — nó sẽ ngày càng xấu hơn theo đà tăng trưởng nếu không thay đổi cơ bản.** Đây là vòng xoáy nguy hiểm: càng scale → càng nhiều sai sót → càng mất uy tín → càng khó scale tiếp.

## 2.2 Mười bốn nút thắt vận hành (N1–N14) — bức tranh đầy đủ

Vượt ra ngoài 3 rủi ro "điểm lỗi đơn" ở Mục 1.4, phân tích chi tiết cho thấy **14 nút thắt vận hành** cụ thể, chia theo 3 nhóm. Đây là bản đồ đầy đủ của những chỗ "máu chảy" mỗi ngày.

### Nhóm A — Vận hành Hàng ngày (N1–N6)

| Mã | Nút thắt | Mô tả |
|---|---|---|
| **N1** | **Tạo số báo danh (SBD) thủ công** | Mỗi học sinh mới cần một nhân viên gõ tay tạo SBD theo quy tắc đặt mã — chậm, dễ trùng, dễ sai, không chạy ngoài giờ. |
| **N2** | **Duyệt vào nhóm Zalo thủ công** | Nhân viên phải tự tay thêm từng học sinh vào đúng nhóm lớp Zalo — ở 55 HS/ngày là hàng trăm thao tác duyệt tay/tuần. |
| **N3** | **Đối soát SePay thủ công** | Kế toán thu phải khớp từng giao dịch tiền về với từng đơn hàng/học sinh — ~2 giờ/ngày, dễ bỏ sót giao dịch lệch nội dung chuyển khoản. |
| **N4** | **Tính thù lao giảng viên thủ công** | ~70 GV, tính theo số buổi/lớp — gom dữ liệu từ nhiều nguồn, tính tay ~1 ngày công/tháng, dễ sai dẫn đến khiếu nại GV. |
| **N5** | **Tính hoa hồng CTV thủ công** | 132–137 CTV theo dõi bằng Google Sheet + ref link tay — tính ~2 ngày công/tháng, thường xuyên phát sinh tranh chấp. |
| **N6** | **Nhập lead thủ công vào CRM** | Lead từ landing page/marketing được nhập tay vào EZSale — bị sót, trùng, vào chậm → lãng phí chi phí marketing. |

### Nhóm B — Quản trị (N7–N10)

| Mã | Nút thắt | Mô tả |
|---|---|---|
| **N7** | **Không có dashboard thời gian thực** | BGĐ không có một màn hình nhìn ra toàn cảnh vận hành — mọi số liệu phải tổng hợp tay, luôn trễ. |
| **N8** | **Phụ thuộc Google Sheet làm "hệ thống"** | Sheet đang gánh vai trò database thực sự (DS học sinh, thù lao, hoa hồng) — vốn không thiết kế cho việc này, dễ vỡ ở quy mô lớn. |
| **N9** | **Dữ liệu phân tán trên Drive cá nhân — không kiểm soát, không cứu vãn được** | Dữ liệu học sinh rải rác trên Drive cá nhân: (1) nhân viên nghỉ → dữ liệu mất vĩnh viễn; (2) nhân viên vô tình xóa → không khôi phục được; (3) tài khoản Google cá nhân bị hack → lộ toàn bộ; (4) không ai biết dữ liệu thật sự đang ở đâu, ai đang giữ gì. Đây là **rủi ro kinh doanh nghiêm trọng** và rủi ro pháp lý (Nghị định 13/2023/NĐ-CP). |
| **N10** | **Thiếu audit trail (nhật ký truy cập)** | Không ghi nhận ai đã xem/sửa dữ liệu nào, lúc nào — vừa là lỗ hổng quản trị, vừa là rủi ro pháp lý dữ liệu cá nhân. |

### Nhóm C — Scale (N11–N14)

| Mã | Nút thắt | Mô tả |
|---|---|---|
| **N11** | **Nhóm Zalo không thể scale** | 1.800–2.100 nhóm Zalo phải duy trì tay; lên quy mô lớn hơn là điểm gãy (xem chi tiết Mục 2.6). |
| **N12** | **ClassIn phụ thuộc con người kích hoạt** | Việc cấp quyền học sinh vào lớp ClassIn làm tay — phụ thuộc nhân viên có mặt, không có dữ liệu học tập riêng để chủ động chăm sóc. |
| **N13** | **Thiếu chuẩn hóa quy trình** | Mỗi cơ sở/mỗi người làm theo cách riêng, không có SOP thống nhất — mở cơ sở mới phải "dạy lại từ đầu". |
| **N14** | **Không có cơ chế bàn giao** | Khi một nhân sự then chốt nghỉ, kiến thức vận hành và dữ liệu đi theo họ — không có quy trình bàn giao có cấu trúc. |

> **Đọc cho BGĐ:** 14 nút thắt này không độc lập — chúng cộng hưởng. N1+N2+N3 tạo ra 504 giờ tay/tháng. N7+N8+N9+N10 khiến BGĐ "điều hành mù" và tích lũy rủi ro pháp lý. N11–N14 là **trần cứng chặn mọi tham vọng scale và mở Đà Nẵng.**

## 2.3 Rủi ro vận hành đang tích lũy

Các rủi ro ở Mục 1.4 và các nút thắt N1–N14 không phải lý thuyết — chúng **đang lớn dần theo quy mô**:

- **R3 / N1 (1 người duyệt học sinh):** ở 55 HS/ngày, một ngày người này nghỉ là **55 học sinh bị kẹt**. Ở đợt cao điểm là hàng trăm.
- **R4 / N5 (tranh chấp hoa hồng):** với 132–137 CTV được theo dõi bằng tay trên Google Sheet, sai sót và tranh chấp hoa hồng **đã xảy ra thường xuyên** — gây mất lòng tin của lực lượng bán hàng, vốn là nguồn tăng trưởng chính.
- **R8 / N6 (lead nhập sót/trùng):** lead từ landing page được nhập tay vào CRM nên **bị sót, bị trùng, hoặc vào chậm** — đồng nghĩa với chi phí marketing bị lãng phí.
- **R11 / N10 (chưa có nhật ký truy cập dữ liệu cá nhân):** dữ liệu hàng chục nghìn học sinh chưa có cơ chế ghi nhận ai đã xem/sửa — **rủi ro pháp lý về bảo vệ dữ liệu cá nhân (PDPA)** trong bối cảnh quy định ngày càng siết.
- **R3-mở rộng / N2 (duyệt Zalo không kiểm soát):** khi nhân viên duyệt nghỉ phép, **giám đốc chi nhánh phải duyệt thay** — người không quen với học sinh cụ thể. Thực tế: không có công cụ đối chiếu tên Zalo với tên thật khi đăng ký và số báo danh → **gần như ai xin vào là duyệt ngay**. Hệ quả: (a) người không phải học sinh có thể vào nhóm lớp; (b) khi xảy ra sự cố, không thể truy xuất ai đã duyệt ai, lúc nào. Đây là lỗ hổng kiểm soát, không chỉ là bất tiện.

## 2.4 Không thể scale với quy mô hiện tại

Tải bình thường đã là 55 HS/ngày. Nhưng điểm gãy thực sự nằm ở **cao điểm khai giảng tại HCM:**

> **~260 học sinh/ngày × 6 đợt/năm.**

Với quy trình tay (~15 phút/HS), 260 học sinh cần **~65 giờ công onboarding dồn trong một ngày** — tương đương phải huy động ~8 người làm liên tục cả ngày chỉ cho riêng việc nhập học. Điều này:
- **Không khả thi** với 12 nhân sự fulltime tại HCM.
- Dẫn đến **dồn ứ, chậm trễ, sai sót hàng loạt** đúng vào thời điểm nhạy cảm nhất (tuần khai giảng).
- **Cản trở trực tiếp tham vọng mở rộng HCM** — vì mở rộng quy mô hiện đồng nghĩa với mở rộng đội ngũ làm tay theo tỉ lệ thuận.

## 2.5 BGĐ đang điều hành "mù" — không có dữ liệu thời gian thực

Hiện tại số liệu vận hành nằm rải rác trên 7 công cụ + Drive cá nhân. Để có một bức tranh tổng thể, ai đó phải tổng hợp tay. Hệ quả:

- **R6 / N7:** BGĐ ra quyết định dựa trên **báo cáo chậm**, không phải dữ liệu thời gian thực.
- **R5 / N13:** cơ sở HCM mở rộng nhưng **không có dashboard/KPI/quy trình chuẩn** → BGĐ không kiểm soát được hiệu quả từng đợt khai giảng, từng kênh, từng CTV.

> **Câu hỏi BGĐ không trả lời nhanh được hôm nay:** "Hôm nay HCM tuyển được bao nhiêu HS? Tỉ lệ chốt theo từng CTV? Doanh thu theo từng kỳ thi tuần này so với tuần trước?" — tất cả đều cần tổng hợp tay nhiều giờ.

**Điểm mù đặc biệt nghiêm trọng: không đo được ROI Marketing**

Hiện tại HSA đang đầu tư vào nhiều kênh marketing (Facebook Ads, TikTok, CTV giới thiệu, landing page, SEO…) nhưng **không có khả năng đo lường hiệu suất từng kênh**:

| Câu hỏi marketing cốt lõi | Có trả lời được không? |
|---|---|
| Chi phí thu hút 1 học sinh (CAC) theo từng kênh? | ❌ Không biết |
| Kênh nào có tỉ lệ chuyển đổi cao nhất (lead → chốt → thanh toán)? | ❌ Không biết |
| Chiến dịch quảng cáo tháng trước có ROI dương không? | ❌ Không biết |
| CTV nào đang mang về học sinh chất lượng (học hết khóa, giới thiệu thêm)? | ❌ Không biết |
| Học sinh từ kênh nào có tỉ lệ refund thấp nhất? | ❌ Không biết |

**Hệ quả:** ngân sách marketing được phân bổ theo **cảm tính và thói quen**, không theo hiệu suất thực. Có thể 80% ngân sách đang đổ vào kênh kém hiệu quả nhất mà không ai biết. Đây là **rò rỉ chi phí vô hình** — không được phản ánh trong bảng tổng kết nhưng tích lũy mỗi tháng.

> Với hệ thống tích hợp: mỗi học sinh được tag nguồn gốc (kênh, chiến dịch, CTV) từ lúc là lead → đến khi chốt → đến khi hoàn thành khóa học → tỉ lệ chuyển đổi và ROI từng kênh được tính tự động, thời gian thực.

## 2.6 Chi phí cơ hội vô hình: lãnh đạo kinh doanh bị vùi đầu trong vận hành

Đây là rủi ro **ít được đặt tên nhất nhưng ảnh hưởng chiến lược lớn nhất** trong toàn bộ bức tranh hiện trạng.

Khi hệ thống vận hành thủ công, **ai là người giải quyết các vấn đề phát sinh mỗi ngày?** Không phải nhân viên cấp thực thi — mà chính là **Giám đốc kinh doanh, Giám đốc chi nhánh, Trưởng khu vực**. Những người được kỳ vọng làm nhiệm vụ chiến lược đang bị kéo vào:

- **Đối soát thanh toán SePay** khi kế toán không khớp
- **Duyệt học sinh vào lớp Zalo** khi nhân viên phụ trách nghỉ phép
- **Tính lại hoa hồng CTV** khi có tranh chấp
- **Xử lý khiếu nại học sinh chờ quá lâu** trong giờ cao điểm
- **Tổng hợp báo cáo tay** để báo cáo lên BGĐ

**Cái giá thực sự:**

| Thời gian lãnh đạo bị "đốt" vào vận hành | Cái không được làm thay thế |
|---|---|
| Xử lý 260 HS/ngày đợt khai giảng HCM | Gặp gỡ đối tác kênh phân phối mới |
| Đối soát hoa hồng 132 CTV cuối tháng | Thiết kế chiến lược CTV mới cho Đà Nẵng |
| Họp nội bộ giải quyết mâu thuẫn dữ liệu Sheet | Nghiên cứu đối thủ và điều chỉnh chương trình học |
| Báo cáo thủ công lên BGĐ | Thị sát chất lượng giảng dạy thực tế |
| Duyệt thay nhân viên nghỉ | Xây dựng quan hệ với trường THPT mới |

> **Nghịch lý vận hành:** HSA đang phát triển ở quy mô cần **lãnh đạo chiến lược**, nhưng hệ thống thủ công đang biến lãnh đạo thành **người vận hành hạng nặng**. Mỗi giờ Giám đốc kinh doanh ngồi đối soát dữ liệu là một giờ không ai nhìn ra thị trường, không ai nâng chất lượng chương trình, không ai mở kênh mới. Đây là chi phí cơ hội vô hình nhưng **ảnh hưởng trực tiếp đến tăng trưởng dài hạn** — không xuất hiện trong bảng lương nhưng thể hiện ở chỗ HSA phát triển chậm hơn tiềm năng.

**Sau khi tự động hóa:** lãnh đạo kinh doanh nhận thông báo tự động thay vì đối soát tay; nhân viên tự xử lý ngoại lệ qua hệ thống thay vì leo thang lên giám đốc; BGĐ có dashboard xem số ngay thay vì chờ báo cáo. **Năng lực lãnh đạo được giải phóng để tập trung vào những gì thực sự tạo ra giá trị.**

## 2.7 Vấn đề nhóm Zalo — quả bom hẹn giờ

Đây là nút thắt **N11** được phân tích riêng vì mức độ nghiêm trọng của nó với tham vọng scale.

**Quy mô hiện tại đã khổng lồ:**
- 600–700 lớp × ~3 nhóm/lớp = **1.800–2.100 nhóm Zalo** phải duy trì liên tục (mỗi lớp tồn tại ~1 năm).
- Mỗi nhóm cần người quản lý: duyệt thành viên, đăng thông báo, xử lý câu hỏi, gỡ thành viên hết khóa.

**Giới hạn kỹ thuật của Zalo:**
- Mỗi nhóm Zalo giới hạn **~1.000 thành viên**. Lớp/khối lớn vượt ngưỡng → phải **clone nhóm** (tạo nhóm 2, nhóm 3) → **nhân đôi, nhân ba công việc quản lý** cho cùng một tập học sinh.
- Đồng bộ thông báo giữa các nhóm clone là việc tay, dễ lệch, dễ sót.

**Thiếu framework quản lý:**
- Câu hỏi cơ bản chưa có lời giải: **một quản lý Zalo có thể ôm tối đa bao nhiêu nhóm** trước khi quá tải? Hiện **không có SOP, không có định mức, không có công cụ theo dõi.**
- Không có hệ thống tra cứu "học sinh X đang ở nhóm nào" → khi cần liên hệ một học sinh cụ thể, phải dò tay.

**Điểm gãy phi tuyến khi scale:**
- Khi lên **30.000 HS** (mục tiêu thực tế nếu nâng thị phần + mở Đà Nẵng) → **3.000+ nhóm Zalo.**
- Đây **không phải vấn đề tuyến tính** (thêm người là xong). Số nhóm tăng nhanh hơn số học sinh do giới hạn 1.000 thành viên/nhóm và nhu cầu phân lớp/phân kỳ → **đây là một điểm gãy**, nơi mô hình quản lý tay sụp đổ.

> **Cảnh báo cho BGĐ:** nhóm Zalo hôm nay "vẫn chạy được" nên dễ bị xem nhẹ. Nhưng nó là **quả bom hẹn giờ**: ở quy mô tăng trưởng mục tiêu, không có cách nào quản 3.000+ nhóm bằng tay. Giải pháp bắt buộc là **tích hợp Zalo OA + CRM để theo dõi membership tự động** (xem PHẦN 2A — P1).

---

# PHẦN 2A — CÁC VẤN ĐỀ DỰ BÁO TRONG TƯƠNG LAI (Rủi ro ẩn)

> Phần này phân tích các vấn đề **chưa xảy ra nhưng có thể dự đoán** dựa trên xu hướng tăng trưởng hiện tại và bản chất hệ thống. Đây là loại rủi ro nguy hiểm nhất — vì thường bị bỏ qua cho đến khi thành khủng hoảng.

## P1 — Zalo platform sẽ chạm giới hạn kỹ thuật (dự báo: ~12–18 tháng)

Zalo giới hạn số nhóm mà một tài khoản admin có thể quản lý, đồng thời mỗi nhóm tối đa ~1.000 thành viên. Ở quy mô hiện tại (1.800–2.100 nhóm) hệ thống đã căng; khi HSA tiến tới **30.000 học sinh** — mục tiêu thực tế nếu nâng thị phần (và có thể mở thêm thị trường mới như Đà Nẵng — giả định tiềm năng) — số nhóm sẽ vọt lên **3.600+ nhóm**. Vấn đề nghiêm trọng hơn cả số lượng là **không có hệ thống theo dõi học sinh thuộc nhóm nào**: khi cần liên hệ một em cụ thể, không ai biết em ở nhóm nào, dẫn đến hiện tượng **"học sinh mất trong Zalo"** — đã thanh toán, đã vào học, nhưng không thể chăm sóc cá nhân hóa. Hệ quả: chất lượng dịch vụ tụt đúng lúc quy mô lớn nhất. **Giải pháp:** tích hợp **Zalo OA + CRM** để track membership tự động, mỗi học sinh được gắn đúng nhóm/lớp trong cơ sở dữ liệu trung tâm, tra cứu và liên hệ trong vài giây thay vì dò tay.

## P2 — ClassIn sẽ tăng giá tại kỳ gia hạn (dự báo: ≤24 tháng)

HSA đang phụ thuộc **ClassIn 100%** cho toàn bộ lớp học online (đang chuyển từ Zoom sang). Đây là dạng phụ thuộc nhà cung cấp (vendor lock-in) điển hình: HSA **không có dữ liệu học tập riêng** (điểm danh, điểm số, lịch sử học) được lưu độc lập, nên **không thể chuyển sang nền tảng khác** nếu ClassIn tăng giá — toàn bộ dữ liệu và quy trình đang nằm trong hệ sinh thái của họ. Khi HSA đạt **20.000+ học sinh/năm**, HSA trở thành khách hàng lớn — nhưng cũng là khách hàng **bị khóa chặt nhất**, mất hoàn toàn vị thế đàm phán giá. Một đợt tăng giá 20–30% ở kỳ gia hạn có thể ăn vào biên lợi nhuận đáng kể mà HSA gần như không có lựa chọn từ chối. **Giải pháp:** tích hợp **Data Subscription** của ClassIn để HSA **sở hữu bản sao dữ liệu học tập riêng** — vừa phục vụ chăm sóc chủ động, vừa tạo đòn bẩy đàm phán và lối thoát kỹ thuật nếu cần đổi nền tảng.

## P3 — Mạng lưới CTV vượt tầm kiểm soát thủ công (dự báo: 6–12 tháng)

Hiện 132–137 CTV được theo dõi bằng **Google Sheet + ref link thủ công**. Mô hình này đã căng và sẽ vỡ khi mạng lưới mở rộng. Khi lên **200+ CTV**, vấn đề **multi-touch attribution** sẽ bùng nổ: một học sinh thường tiếp xúc **2+ CTV** (qua quảng cáo, giới thiệu, tư vấn) trước khi quyết định mua — ai là người được ghi nhận hoa hồng? Với theo dõi tay, điều này dẫn đến **tranh chấp hoa hồng hàng loạt**, tốn thời gian xử lý và bào mòn lòng tin. Mất một CTV giỏi không chỉ mất một người — mà mất cả **kênh tuyển sinh** mà người đó đang nắm (mạng lưới phụ huynh, trường học, cộng đồng). Ở một doanh nghiệp mà CTV là nguồn tăng trưởng chính, đây là rủi ro trực tiếp lên doanh thu. **Giải pháp:** hệ thống cho **CTV tự xem hoa hồng realtime** + **quy tắc attribution rõ ràng, công khai** (first-touch/last-touch/chia tỉ lệ) được hệ thống áp dụng tự động, không tranh cãi.

## P4 — Quy định PDPA/dữ liệu cá nhân bắt đầu có hiệu lực (rủi ro pháp lý)

**Nghị định 13/2023/NĐ-CP** về bảo vệ dữ liệu cá nhân đã có hiệu lực và đang được siết dần trong khâu thực thi. HSA hiện lưu dữ liệu của **~20.000 học sinh/năm** (họ tên, ngày sinh, số điện thoại, thông tin phụ huynh, kết quả học tập) phân tán trong **Google Drive cá nhân** và nhiều công cụ **không có audit trail**. Đây là vi phạm tiềm tàng nhiều nguyên tắc cốt lõi của nghị định: không có cơ sở pháp lý xử lý dữ liệu rõ ràng, không kiểm soát truy cập, không nhật ký, không cơ chế thu hồi. Nguy cơ cụ thể: **bị thanh tra, bị phạt hành chính, hoặc bị phụ huynh khiếu kiện** khi xảy ra lộ/lọt thông tin (mà với dữ liệu nằm trên Drive cá nhân, rủi ro lộ lọt là rất thực). Khác với các rủi ro vận hành, rủi ro pháp lý có thể gây tổn hại uy tín nghiêm trọng và khó phục hồi. **Giải pháp:** **tập trung dữ liệu** về một kho có kiểm soát + **nhật ký truy cập** (ai xem/sửa gì, khi nào) + **chính sách xử lý dữ liệu** thành văn.

## P5 — Mở rộng thị trường mới (Đà Nẵng — giả định) không thể thực hiện với hệ thống hiện tại (rủi ro chiến lược)

> **Lưu ý:** Đà Nẵng ở đây là **giả định tiềm năng dựa trên quy mô thị trường ước tính, chưa được nghiên cứu thực địa và chưa phải kế hoạch chắc chắn**. Quyết định mở Đà Nẵng cần phân tích riêng biệt về: nhu cầu tuyển sinh thực tế, chi phí vận hành tại chỗ, đội ngũ địa phương. Phần dưới phân tích vì sao — **bất kể mở ở đâu** — hạ tầng vận hành phải sẵn sàng trước.

Đà Nẵng có **~11.500 thí sinh THPT/năm**, tương đương **~3.500–4.600 học sinh tiềm năng mua khóa học** (áp tỉ lệ 30–40%) — một thị trường về lý thuyết đủ lớn để cân nhắc mở cơ sở thứ ba. Nhưng với hệ thống hiện tại, **mở văn phòng tại bất kỳ thị trường mới nào = khởi tạo lại toàn bộ quy trình thủ công từ đầu**: tuyển và đào tạo đội onboarding tay, dựng lại các Google Sheet, các nhóm Zalo, các quy trình duyệt — vì không có SOP chuẩn hóa (N13) để "nhân bản". Tệ hơn, **cost-to-operate mỗi học sinh ở Đà Nẵng = HN/HCM** (vì vẫn làm tay), nhưng **doanh thu nhỏ hơn** (thị trường nhỏ hơn) → **biên lợi nhuận thấp hơn**, có thể không đủ bù chi phí mở cơ sở. Kết quả: một cơ hội thị trường thực sự bị bỏ lỡ chỉ vì hạ tầng vận hành không sẵn sàng. **Giải pháp:** **chuẩn hóa và tự động hóa quy trình trước khi mở rộng** → mỗi cơ sở mới chỉ cần "cắm vào hệ thống là chạy", chi phí biên gần như bằng không.

## P6 — Đối thủ số hóa nhanh hơn sẽ vượt qua (rủi ro cạnh tranh)

**Vietnam EdTech 2024: $1 tỷ USD, tăng trưởng 12,96%/năm** — một thị trường đủ hấp dẫn để thu hút làn sóng đối thủ mới. Các đối thủ này thường **sinh ra từ công nghệ** (app học tập, AI tutor, mô hình online-first) và **không mang gánh nặng vận hành thủ công** như HSA — họ onboard học sinh trong vài phút, có dashboard, có dữ liệu học tập, scale gần như không giới hạn. HSA hiện có **lợi thế thương hiệu và chất lượng giảng dạy** — nhưng nếu trải nghiệm onboarding vẫn chậm (2–8 giờ) và dịch vụ kém cá nhân hóa, học sinh/phụ huynh sẽ **so sánh** và lợi thế đó bị bào mòn. **Dự báo:** trong **2–3 năm**, khoảng cách số hóa giữa HSA và các đối thủ nhanh sẽ **thu hẹp hoặc đảo chiều** nếu HSA không hành động ngay. Lợi thế thương hiệu mua được thời gian, nhưng không mua được vĩnh viễn. **Giải pháp:** dùng cửa sổ thời gian hiện tại — khi HSA vẫn dẫn đầu (~21% SAM) — để số hóa, biến lợi thế thương hiệu thành lợi thế công nghệ + thương hiệu kép.

## P7 — Tình trạng "học sinh ghost" ngày càng phổ biến (rủi ro giữ chân)

Hiện HSA **không có hệ thống theo dõi chuyên cần tự động** → không biết học sinh nào vắng nhiều buổi cho đến khi đã quá muộn (em đó đã bỏ học hẳn). Ở quy mô **20.000 học sinh**, chỉ cần tỉ lệ drop-out **5%** → **1.000 học sinh không hoàn thành khóa** → ảnh hưởng trực tiếp đến **kết quả thi** (điểm yếu tố quyết định uy tín của một trung tâm luyện thi) và **danh tiếng truyền miệng**. Quan trọng không kém: thiếu chăm sóc chủ động đồng nghĩa với **mất cơ hội up-sell** — chính những em đang gặp khó khăn (cần gia hạn, cần khóa nâng cao, cần phụ đạo) lại là nhóm khách hàng có nhu cầu chi thêm cao nhất, nhưng HSA không nhận diện được họ kịp thời. **Giải pháp:** tích hợp **ClassIn Data Subscription** → hệ thống **tự động alert khi học sinh vắng 3+ buổi** → Quản lý lớp (QLL) liên hệ kịp thời để giữ chân và mở cơ hội bán thêm.

## P8 — Google Sheet sẽ sập dưới tải dữ liệu lớn và nhiều người dùng đồng thời (rủi ro hạ tầng)

Google Sheet đang bị HSA dùng như một **database thực sự** (N8) — vai trò nó không được thiết kế để gánh. Google Sheet có **giới hạn cứng: ~10 triệu ô/file và ~200 người dùng đồng thời** — và HSA **đang tiếp cận các ngưỡng này**. Khi **~70 giảng viên + 132 CTV + đội HN + đội HCM** cùng truy cập các file lớn (danh sách học sinh, thù lao, hoa hồng), hệ quả điển hình là **chậm, treo, lỗi đồng bộ, thậm chí corruption (mất/hỏng dữ liệu)**. Đã có nhiều tiền lệ ở các doanh nghiệp Việt Nam tương tự: **Sheet bị "treo" đúng đợt cao điểm** — chính là lúc không được phép hỏng. Một sự cố mất dữ liệu thù lao/hoa hồng giữa đợt khai giảng có thể gây khủng hoảng niềm tin với cả giảng viên lẫn CTV. **Giải pháp:** chuyển sang **database thực sự** (PostgreSQL HSA Platform) — được thiết kế cho hàng triệu bản ghi và hàng trăm người dùng đồng thời, có sao lưu và toàn vẹn dữ liệu.

## P9 — Chưa khai thác dữ liệu phụ huynh — bỏ lỡ tệp khách hàng dễ tiếp cận nhất (rủi ro doanh thu)

Hiện tại HSA thu thập tên + số điện thoại học sinh và **gần như không lưu thông tin phụ huynh có cấu trúc**. Đây là một lỗ hổng doanh thu nghiêm trọng bị ẩn sau vẻ ngoài "vận hành bình thường":

**Phụ huynh là tệp khách hàng nóng nhất mà HSA đang bỏ qua:**
- Một phụ huynh có con học tại HSA đã **tin tưởng HSA, đã trả tiền, đã thấy kết quả** (hoặc đang chờ thấy kết quả). Đây là mức độ tin tưởng cao nhất có thể có với một thương hiệu giáo dục.
- Nếu gia đình có **2–3 con ở độ tuổi thi**, xác suất chuyển đổi mua khóa học tiếp theo **cao hơn 5–10 lần** so với một khách hàng lạnh.
- **Hiện HSA không biết**: gia đình HS X có còn em nhỏ không? Phụ huynh em nào đang có con sắp đến tuổi thi? Phụ huynh nào đang trong mạng lưới có thể giới thiệu thêm?

**Ví dụ cơ hội bị bỏ lỡ mỗi ngày:**
- HS học ĐGNL HSA → thi xong → phụ huynh có em nhỏ 2 năm nữa thi → **không ai tiếp cận** vì không có hệ thống theo dõi
- Phụ huynh hài lòng → sẵn sàng giới thiệu → **không có quy trình CTV phụ huynh**, không có cơ chế khai thác referral từ network phụ huynh
- Gia đình có 3 con lần lượt thi các năm → tiềm năng doanh thu 3 lần từ cùng 1 gia đình → **bị đối xử như 3 khách hàng lạ, không nhận diện được mối liên hệ**

**Quy mô cơ hội bị bỏ ngỏ:**
- 20.000 học sinh × trung bình 1,2 anh/em trong độ tuổi thi → **~4.000–6.000 học sinh tiềm năng trong gia đình hiện tại** mà HSA chưa tiếp cận có hệ thống
- Tỉ lệ chuyển đổi tệp warm (phụ huynh đã tin tưởng) ước tính 40–60% → **~1.600–3.600 học sinh bổ sung/năm** có thể tiếp cận mà không tốn chi phí marketing
- Ở đơn giá 2–3 triệu/HS → **~3,2–10,8 tỷ VND/năm doanh thu tiềm năng bỏ lỡ** từ tệp khách hàng sẵn có

**Tại sao chưa làm được:**
- Không có CRM tập trung → không lưu số điện thoại phụ huynh có cấu trúc
- Không có trường "liên kết gia đình" → không biết HS A và HS B là anh em
- Không có workflow chăm sóc phụ huynh sau khi HS hoàn thành khóa học

**Giải pháp:** Khi xây dựng hệ thống mới, **bổ sung profile phụ huynh vào data model**: số điện thoại, email, số con, năm dự kiến thi của từng con → từ đó xây dựng **quy trình chăm sóc phụ huynh bán tự động** (cảnh báo khi em nhỏ đến gần tuổi thi, chiến dịch referral có cơ chế, phân khúc family loyalty).

## P10 — Chiến lược công nghệ tự phát: thiếu định hướng và dễ chệch hướng (ĐANG XẢY RA — mức độ CAO)

**Mô tả:** Khác với các rủi ro dự báo tương lai ở trên, đây là rủi ro **đang xảy ra hiện tại**. HSA tìm kiếm phần mềm, đối tác công nghệ và xây dựng tính năng theo kiểu **hoàn toàn tự phát, bị động** — "vướng đến đâu tìm đến đó". Biểu hiện cụ thể:

- Mỗi bộ phận tự chọn công cụ (EZSale, Google Sheet, Zalo riêng, ClassIn...) **không có ai điều phối tổng thể**.
- **Không có kiến trúc tổng thể** → các hệ thống không nói chuyện được với nhau → phát sinh tích hợp tốn kém (chính là gốc rễ của 504 giờ tay/tháng).
- Dễ bị **vendor lock-in** vào các công cụ không phù hợp dài hạn.
- **Chi phí "học phí" cao:** thử-sai nhiều lần, mua phần mềm không dùng được, thuê tư vấn nhiều lần.

**Hậu quả:** Lãng phí tài nguyên (tiền + thời gian), thiếu nhất quán, dễ chệch hướng chiến lược, tổng chi phí công nghệ cao hơn nhiều so với một lộ trình có kế hoạch.

**Mức độ:** 🔴 **CAO — đang xảy ra hiện tại** (không phải rủi ro tiềm ẩn).

**Giải pháp:** Đây chính là **lý do cốt lõi cần CTO** — không phải chỉ để viết code, mà để **làm chủ kiến trúc công nghệ tổng thể**, đánh giá và chọn công cụ, ngăn chặn quyết định công nghệ tùy tiện, và đảm bảo mọi đầu tư công nghệ có chiến lược rõ ràng. CTO là người duy nhất trong tổ chức có thẩm quyền và năng lực điều phối toàn bộ quyết định công nghệ về một định hướng thống nhất.

> **Tổng kết PHẦN 2A:** 10 rủi ro ẩn này có chung một đặc điểm — **chúng đều có thể dự báo, và đều có cùng một lời giải gốc: chuyển từ vận hành thủ công phân tán sang một hệ thống tích hợp, có dữ liệu tập trung.** Hành động hôm nay (khi chúng còn là "dự báo") rẻ hơn rất nhiều so với xử lý khủng hoảng ngày mai. Và P9 nhắc nhở thêm: chuyển đổi số không chỉ là giảm chi phí — nó còn **mở ra nguồn doanh thu mới từ dữ liệu hiện có.** Riêng P10 (đang xảy ra) chỉ rõ vì sao **CTO làm chủ kiến trúc tổng thể** là điều kiện tiên quyết: không có người điều phối, mọi nỗ lực công nghệ vẫn sẽ tự phát và chệch hướng.

---

# PHẦN 2B — RỦI RO BẢO MẬT WEBSITE HIỆN TẠI

> Website hsavnu.edu.vn là điểm tiếp xúc tài chính trực tiếp với học sinh — nơi học sinh **đăng ký gói học, nhập mã khuyến mãi và thanh toán**. Kiểm tra thực tế phát hiện nhiều lỗ hổng nghiêm trọng — trong đó có lỗ hổng đang **gây thất thoát doanh thu ngay hôm nay**.

## B0 — Mã khuyến mãi không được xác thực phía server — THẤT THOÁT DOANH THU NGAY LẬP TỨC (NGHIÊM TRỌNG NHẤT)

**Mô tả (đã xác nhận):** Học sinh có thể nhập **bất kỳ chuỗi ký tự nào** vào ô mã khuyến mãi và **vẫn được giảm tiền**. Logic kiểm tra tính hợp lệ của mã hoặc không tồn tại, hoặc chỉ được thực hiện phía client (JavaScript) — dễ dàng bỏ qua bằng cách thay đổi request. Hệ quả:

- Học sinh (hoặc bất kỳ ai biết lỗ hổng này) có thể tự tạo mã giảm giá tùy ý → mua khóa học với giá thấp hơn nhiều so với giá niêm yết
- Không có audit log về các mã đã được áp dụng → **không biết đã thất thoát bao nhiêu**
- Khi thông tin lỗ hổng lan truyền trong cộng đồng học sinh (Zalo, Facebook) → tổn thất có thể leo thang nhanh chóng
- Ước tính: nếu 5% trong 20.000 HS/năm lợi dụng để giảm thêm trung bình 500.000đ → **thất thoát ~500 triệu VND/năm** (chưa tính tổn thất uy tín)

**Mức độ:** 🔴🔴 **CRITICAL — ưu tiên số 1 tuyệt đối** — đây là lỗ hổng đang xảy ra trong thực tế, không phải rủi ro tiềm ẩn.

**Khắc phục:** Toàn bộ logic validate mã khuyến mãi phải được thực hiện **phía server**: kiểm tra mã tồn tại trong database, chưa hết hạn, chưa dùng đủ số lần, đúng đối tượng áp dụng. Phía client chỉ được hiển thị kết quả sau khi server confirm. Bổ sung logging đầy đủ cho mọi lần mã khuyến mãi được áp dụng (hợp lệ hoặc không).

## B1 — Xác thực SePay Webhook thiếu chữ ký HMAC (NGHIÊM TRỌNG)

**Mô tả:** SePay gửi thông báo thanh toán về máy chủ HSA qua webhook. Nếu webhook này **không xác thực chữ ký HMAC** (kiểm tra rằng yêu cầu thực sự đến từ SePay, không phải từ bên thứ 3 giả mạo), kẻ tấn công có thể gửi một HTTP POST giả mạo thông báo "đã thanh toán thành công" và được nhập học **mà không trả một đồng nào**.

**Mức độ:** 🔴 **CRITICAL** — ảnh hưởng trực tiếp đến doanh thu.

**Khắc phục:** Kiểm tra và bổ sung HMAC signature verification trên mọi webhook handler trước khi xử lý bất kỳ giao dịch nào.

## B2 — Mã hóa MD5 đã lỗi thời cho dữ liệu nhạy cảm (NGHIÊM TRỌNG)

**Mô tả:** Tài liệu chính sách thanh toán đề cập đến "MD5 128-bit encryption standard". MD5 là thuật toán đã bị **vô hiệu hóa bởi cộng đồng bảo mật từ đầu thập niên 2010** — dễ bị tấn công rainbow table và brute-force. Sử dụng MD5 để bảo vệ mật khẩu hoặc dữ liệu giao dịch vi phạm chuẩn bảo mật hiện đại (OWASP, PCI DSS).

**Mức độ:** 🔴 **CRITICAL** — nếu cơ sở dữ liệu bị rò rỉ, toàn bộ mật khẩu học sinh có thể bị giải mã trong vài giờ.

**Khắc phục:** Thay MD5 bằng bcrypt/Argon2 (cho mật khẩu) và SHA-256 (cho checksum giao dịch).

## B3 — Không có CSRF Protection trên form đăng ký và đăng nhập

**Mô tả:** Form đăng ký (/dang-ky) và đăng nhập (/dang-nhap) không có dấu hiệu của **CSRF token**. Điều này cho phép kẻ tấn công lừa người dùng đã đăng nhập thực hiện các hành động không mong muốn (đổi mật khẩu, đổi thông tin, thậm chí tạo giao dịch giả).

**Mức độ:** 🟠 **CAO** — đặc biệt nguy hiểm với form thanh toán và thay đổi thông tin cá nhân.

**Khắc phục:** Triển khai CSRF token trên tất cả form POST, đặc biệt là form thanh toán và đổi thông tin.

## B4 — Thiếu reCAPTCHA / Rate Limiting trên form đăng ký

**Mô tả:** Form đăng ký không có cơ chế chống bot (reCAPTCHA, Turnstile, hoặc rate limiting). Kẻ tấn công có thể:
- Tạo hàng loạt tài khoản ảo để nhận mã khuyến mãi nhiều lần
- Brute-force tài khoản của học sinh bằng cách thử mật khẩu liên tiếp
- DDoS hệ thống qua endpoint đăng ký

**Mức độ:** 🟠 **CAO** — đặc biệt là rủi ro mã khuyến mãi bị lạm dụng.

**Khắc phục:** Bổ sung Cloudflare Turnstile (miễn phí) trên form đăng ký + rate limiting 5 lần/phút/IP trên endpoint login.

## B5 — Không có xác thực đầu vào (Input Validation) phía server

**Mô tả:** Không có dấu hiệu rõ ràng của server-side input validation. Các lỗ hổng tiềm ẩn:
- **SQL Injection** nếu dữ liệu đầu vào không được sanitize trước khi truy vấn database
- **XSS (Cross-Site Scripting)** nếu nội dung người dùng nhập được hiển thị lại mà không escape HTML
- **Mã khuyến mãi bypass:** nếu mã khuyến mãi chỉ được kiểm tra phía client (JavaScript), dễ dàng bỏ qua bằng cách chỉnh sửa request

**Mức độ:** 🟠 **CAO** — bao gồm rủi ro trực tiếp đến doanh thu (bypass discount).

**Khắc phục:** Mọi validation phải có phiên bản phía server. Dùng parameterized queries / ORM. Output encode mọi dữ liệu người dùng trước khi render.

## B6 — Không có đăng nhập 2 yếu tố (MFA) cho tài khoản nhạy cảm

**Mô tả:** Tài khoản quản trị (admin, nhân viên, giảng viên) chỉ có đăng nhập bằng email/mật khẩu. Nếu mật khẩu của một tài khoản admin bị lộ (qua phishing, keylogger, hoặc từ breach database khác), kẻ tấn công có toàn quyền truy cập hệ thống.

**Mức độ:** 🟡 **TRUNG BÌNH** (đối với tài khoản học sinh); 🔴 **NGHIÊM TRỌNG** (đối với tài khoản quản trị).

**Khắc phục:** Bật MFA (OTP qua email hoặc app) bắt buộc cho tài khoản quản trị và nhân viên.

## B7 — CDN Assets không có Subresource Integrity (SRI)

**Mô tả:** Website dùng Digital Ocean Spaces làm CDN cho các tài nguyên JavaScript/CSS. Nếu CDN bị xâm phạm (supply chain attack) và file JS bị thay thế bởi code độc hại, **trình duyệt người dùng sẽ tự động tải và chạy code đó** — bao gồm cả đánh cắp thông tin thẻ/mật khẩu.

**Mức độ:** 🟡 **TRUNG BÌNH** — khả năng thấp nhưng hậu quả cực kỳ nghiêm trọng (tương tự vụ Magecart skimmer đã ảnh hưởng hàng nghìn website thương mại điện tử toàn cầu).

**Khắc phục:** Thêm `integrity` attribute (SRI hash) cho các file JS/CSS tải từ CDN.

## Tóm tắt ưu tiên khắc phục

| # | Lỗ hổng | Mức độ | Thời gian khắc phục ước tính |
|---|---|---|---|
| **B0** | **Mã khuyến mãi không validate phía server** | 🔴🔴 **CRITICAL — đang gây thất thoát** | **1–2 ngày — ưu tiên tuyệt đối** |
| B1 | SePay webhook không HMAC | 🔴 CRITICAL | 1–2 ngày (CTO ưu tiên ngay sau onboard) |
| B2 | MD5 password hashing lỗi thời | 🔴 CRITICAL | 2–3 ngày |
| B3 | Thiếu CSRF Protection | 🟠 CAO | 1–2 ngày |
| B4 | Không có reCAPTCHA/Rate Limiting | 🟠 CAO | 1 ngày |
| B5 | Thiếu Input Validation phía server | 🟠 CAO | 3–5 ngày (tùy độ phức tạp) |
| B6 | Thiếu MFA cho tài khoản quản trị | 🟡 TRUNG BÌNH | 2–3 ngày |
| B7 | Thiếu SRI cho CDN assets | 🟡 TRUNG BÌNH | 0.5 ngày |

> **Lưu ý cho BGĐ:** **B0 là lỗ hổng đang gây thất thoát doanh thu ngay hôm nay** — cần được xử lý ngay cả trước khi CTO onboard chính thức (dev hiện tại hoặc bất kỳ ai có quyền truy cập code). B1–B3 là ưu tiên tiếp theo trong **tuần đầu tiên CTO onboard**. Tổng thời gian khắc phục toàn bộ B0–B7 khoảng **2 tuần làm việc tập trung** — đây là phần đầu tiên của GĐ 0, trước cả việc xây dựng tính năng mới.

---

# PHẦN 3 — ĐỀ XUẤT: CHÚNG TA XÂY GÌ?

> **Mục tiêu phần này:** không chỉ trả lời "xây cái gì", mà còn trả lời **"xây thế nào để không gây gián đoạn kinh doanh"** và **"vì sao tự xây thay vì mua/thuê"**. Phần này được viết hai tầng: phần diễn giải để BGĐ hiểu **WHAT** (cái gì, vì sao), phần kỹ thuật để CTO/technical lead hiểu **WHY** (vì sao thiết kế như vậy).

## 3.1 Tầm nhìn & triết lý thiết kế

### 3.1.1 Tầm nhìn 3 năm — ba nấc trưởng thành

Đề xuất này không phải một dự án "làm xong rồi để đó". Nó là nền móng cho một lộ trình trưởng thành 3 năm, mỗi năm nâng một nấc giá trị:

```
  NĂM 1 (2026–2027)        NĂM 2 (2027)            NĂM 3 (2028+)
 ┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐
 │ TỰ ĐỘNG HÓA      │ →  │ NỀN TẢNG DỮ LIỆU │ →  │ TRẢI NGHIỆM      │
 │ QUY TRÌNH        │    │                  │    │ CÁ NHÂN HÓA      │
 ├──────────────────┤    ├──────────────────┤    ├──────────────────┤
 │ Cắt 504h tay/    │    │ Một nguồn dữ liệu│    │ Chăm sóc chủ động│
 │ tháng. Onboard   │    │ duy nhất. Dashboard│   │ từng HS & phụ    │
 │ < 2 phút, 24/7.  │    │ realtime. Attribu-│   │ huynh. Family    │
 │ Hết SPOF.        │    │ tion ROI marketing│   │ CRM. Up-sell tự  │
 │                  │    │ & hoa hồng CTV.  │    │ động theo dữ liệu│
 └──────────────────┘    └──────────────────┘    └──────────────────┘
   "Hết chảy máu        "Nhìn rõ doanh         "Biến dữ liệu thành
    nhân công"           nghiệp bằng số"        doanh thu mới"
```

- **Năm 1 — Tự động hóa quy trình:** chấm dứt 504 giờ tay/tháng, onboarding tự động dưới 2 phút, xóa các điểm lỗi đơn (R1, R3). Đây là phần **bắt buộc**, giải quyết các vấn đề đang chảy máu hôm nay.
- **Năm 2 — Nền tảng dữ liệu:** mọi dữ liệu vận hành dồn về một nguồn duy nhất (PostgreSQL HSA Platform), kế toán chính thức trên MISA SME Online. BGĐ có dashboard thời gian thực; marketing đo được ROI từng kênh; hoa hồng CTV minh bạch. HSA chuyển từ "điều hành mù" sang "điều hành bằng số".
- **Năm 3 — Trải nghiệm cá nhân hóa:** khi đã có dữ liệu sạch và tập trung, HSA khai thác nó để chăm sóc chủ động (cảnh báo vắng học, giữ chân học sinh) và mở nguồn doanh thu mới từ tệp phụ huynh (P9 — doanh thu ẩn 3,2–10,8 tỷ/năm).

> **Đọc cho BGĐ:** đầu tư Giai đoạn 1 không chỉ tiết kiệm chi phí — nó **đặt nền móng** cho hai nấc giá trị sau. Mỗi đồng đầu tư vào Năm 1 mở khóa giá trị lớn hơn ở Năm 2 và Năm 3.

### 3.1.2 Năm nguyên tắc thiết kế

Mọi quyết định kỹ thuật trong đề xuất này tuân theo 5 nguyên tắc. Đây là "hiến pháp thiết kế" để CTO và Product Owner (COO) không đi chệch hướng:

| # | Nguyên tắc | Ý nghĩa cho BGĐ (WHAT) | Hệ quả kỹ thuật (WHY) |
|---|---|---|---|
| 1 | **Automation First** | Mặc định mọi việc lặp lại phải do máy làm; con người chỉ chạm vào ngoại lệ | Mọi luồng nghiệp vụ thiết kế dưới dạng job bất đồng bộ (Hangfire), không có bước "chờ người bấm nút" trong đường đi chuẩn |
| 2 | **Data Ownership** | HSA phải **sở hữu dữ liệu của chính mình**, không bị khóa trong công cụ bên thứ ba | Integration DB (PostgreSQL) + ClassIn Data Subscription lưu bản sao dữ liệu học tập → thoát vendor lock-in (P2) |
| 3 | **Non-Disruptive Migration** | Chuyển đổi **không được làm tụt doanh số** hay rối loạn đội ngũ đang chạy | Chạy song song hệ cũ–mới, feature flags, rollback từng phần (chi tiết 3.4) |
| 4 | **Exception-Based Human Work** | Nhân sự chuyển từ "gõ cho từng HS" sang "xử lý trường hợp đặc biệt" | Hệ thống tự phát hiện ngoại lệ → alert đúng người (Zalo) thay vì để lỗi trôi vào im lặng |
| 5 | **Scale-Ready** | Thêm 10.000 học sinh hay mở Đà Nẵng **không cần thêm nhân công vận hành theo tỉ lệ** | Kiến trúc stateless, job queue scale ngang, SBD generation an toàn race-condition (chịu tải spike 780 HS/ngày 2027) |

> Năm nguyên tắc này không phải khẩu hiệu — chúng là tiêu chí để **từ chối** những quyết định sai. Ví dụ: bất kỳ đề xuất nào yêu cầu "nhân viên bấm nút để chạy" sẽ bị loại vì vi phạm nguyên tắc 1; bất kỳ tích hợp nào không lưu lại bản sao dữ liệu sẽ bị loại vì vi phạm nguyên tắc 2.

## 3.2 Kiến trúc 3 lớp của HSA Integration Platform

Hệ thống được chia thành **3 lớp tách bạch**, mỗi lớp một trách nhiệm rõ ràng. Cách chia này quan trọng vì nó cho phép thay/sửa từng lớp mà không đụng các lớp khác — nền tảng cho nguyên tắc Non-Disruptive.

```
┌──────────────────────────────────────────────────────────────────────┐
│  LỚP 3 — CHANNELS & EXPERIENCE  (Học sinh · Phụ huynh · CTV thấy gì)  │
│  ┌──────────┐ ┌──────────┐ ┌──────────────┐ ┌──────────┐ ┌─────────┐ │
│  │ Zalo OA  │ │  Email   │ │  Web portal  │ │   CTV    │ │ Parent  │ │
│  │  (ZNS)   │ │  (SMTP)  │ │ (UX 8→3 bước)│ │  portal  │ │ portal  │ │
│  └──────────┘ └──────────┘ └──────────────┘ └──────────┘ │ (GĐ2-3) │ │
│                                                           └─────────┘ │
└───────────────────────────────▲──────────────────────────────────────┘
                                 │  (đọc/ghi qua API)
┌───────────────────────────────┴──────────────────────────────────────┐
│  LỚP 2 — DATA & FINANCE  (.NET Finance Service + MISA SME Online)     │
│  ┌────────────┐ ┌────────────┐ ┌─────────────┐ ┌──────────────────┐  │
│  │ HS · Phụ   │ │ .NET CRM   │ │ .NET Finance│ │ Dashboard BGĐ    │  │
│  │ huynh · Đơn│ │ pipeline:  │ │ đối soát,   │ │ kỳ thi × cơ sở × │  │
│  │ hàng · Hóa │ │ lead→chốt→ │ │ thù lao GV, │ │ kênh × CTV       │  │
│  │ đơn · H.hồng│ │ nhập học  │ │ hoa hồng CTV│ │ (realtime)       │  │
│  └────────────┘ └────────────┘ └──────┬──────┘ └──────────────────┘  │
│   (lưu trong PostgreSQL HSA)          │ MISA API (sync 1 chiều)        │
│                              ┌────────▼─────────┐                      │
│                              │ MISA SME Online  │ kế toán chính thức   │
│                              │ (journal entries)│ P&L, báo cáo thuế    │
│                              └──────────────────┘                      │
└───────────────────────────────▲──────────────────────────────────────┘
                                 │  REST / domain service (.NET API)
                                 │  PostgreSQL HSA là SSOT — KHÔNG còn Odoo DB
┌───────────────────────────────┴──────────────────────────────────────┐
│  LỚP 1 — AUTOMATION CORE  (HSA Integration Platform — .NET 10)        │
│                                                                       │
│   ┌─────────────────┐      ┌──────────────────────────────────────┐  │
│   │ Webhook Receiver│─────▶│  Job Orchestrator (Hangfire)         │  │
│   │ SePay (HMAC)    │      │  chuỗi job bất đồng bộ + retry       │  │
│   │ ClassIn DataSub │      └──────────────────┬───────────────────┘  │
│   └─────────────────┘                         │                      │
│   ┌─────────────────┐   ┌─────────────────────┴──────────────────┐  │
│   │  SBD Generator  │   │  Integration Adapters                   │  │
│   │  (atomic, race- │   │  SePay · ClassIn · Zalo OA · MISA      │  │
│   │  condition-safe)│   └────────────────────────────────────────┘  │
│   └─────────────────┘   ┌────────────────────────────────────────┐  │
│   ┌─────────────────┐   │  Exception Handler                      │  │
│   │ Integration DB  │   │  phát hiện ngoại lệ → alert đúng người  │  │
│   │ (PostgreSQL)    │   └────────────────────────────────────────┘  │
│   │ enrollments ·   │                                                │
│   │ students ·      │   ◀── DB DUY NHẤT của HSA (SSOT). Không      │
│   │ parents · SBD   │       của Platform, phục vụ tốc độ & toàn vẹn │
│   │ seq · CTV attr  │                                                │
│   └─────────────────┘                                                │
└──────────────────────────────────────────────────────────────────────┘
        ▲ webhook PUSH              ▲ webhook PUSH
   [ SePay: tiền về ]         [ ClassIn: sự kiện học tập ]
```

### Lớp 1 — Automation Core (HSA Integration Platform, .NET 10 Clean Architecture)

Đây là "bộ não" do CTO nội bộ phát triển. Nhiệm vụ: nhận sự kiện từ bên ngoài (tiền về, học sinh vắng…) và **tự động chạy chuỗi hành động** mà không cần con người.

- **Webhook Receiver:** điểm nhận sự kiện PUSH từ SePay (khi tiền về) và ClassIn Data Subscription (khi có sự kiện học tập). Mọi webhook SePay đều được **xác thực HMAC** trước khi xử lý — chống giả mạo lệnh thanh toán.
- **Job Orchestrator (Hangfire):** điều phối các chuỗi job **bất đồng bộ**. Khi một webhook đến, nó không xử lý ngay trong request mà đẩy vào hàng đợi → đảm bảo chịu được spike (520–780 HS/ngày) và **retry tự động** khi một bước lỗi.
- **Integration Adapters:** mỗi hệ thống bên ngoài có một adapter riêng (SePay adapter, ClassIn adapter, Zalo OA adapter, MISA adapter). Tách adapter giúp **thay/sửa một tích hợp mà không đụng các tích hợp khác** (nguyên tắc Non-Disruptive + Scale-Ready).
- **SBD Generator (atomic, race-condition-safe):** sinh số báo danh theo format `[KỲ_THI]-[NĂM]-[SEQ_5]` (vd `HSA-2026-08421`). Dùng cơ chế `UPDATE ... RETURNING` của PostgreSQL để **đảm bảo không trùng số ngay cả khi 100 học sinh thanh toán cùng một giây** — điều bất khả thi với cách tạo tay hiện nay (N1).
- **Exception Handler:** khi một bước thất bại sau khi đã retry, không để lỗi trôi vào im lặng — hệ thống tự **đẩy cảnh báo cho đúng người** (vd Zalo cho Quản lý lớp), kèm đủ thông tin để xử lý.
- **PostgreSQL HSA Platform (DB duy nhất — SSOT):** lưu các bảng `enrollments`, `students`, `parents`, `SBD sequences`, `CTV attribution`, CRM, đơn hàng, hoa hồng, thù lao. Đây là **nguồn sự thật duy nhất của HSA** — không còn 2 DB (Integration DB + Odoo DB), chỉ một PostgreSQL do HSA sở hữu hoàn toàn. **Quan hệ với kế toán:** dữ liệu kế toán được **.NET Finance Service push sang MISA SME Online qua MISA API (sync 1 chiều)** định kỳ; HSA không bao giờ phụ thuộc vào DB nội bộ của phần mềm kế toán, tránh khóa cứng (xem Lớp 2).

### Lớp 2 — Data & Finance (.NET Finance Service + MISA SME Online)

Toàn bộ dữ liệu nghiệp vụ nằm trong **PostgreSQL HSA Platform** (nguồn sự thật duy nhất) — thay thế cho mớ Google Sheet + Drive cá nhân hiện nay (N8, N9); **MISA SME Online** chỉ đảm nhiệm vai trò sổ kế toán chính thức.

- **Single source of truth (PostgreSQL HSA):** học sinh, phụ huynh, đơn hàng, hóa đơn, hoa hồng CTV, CRM. Hết tình trạng "dữ liệu thật nằm đâu không ai biết".
- **CRM pipeline (.NET CRM module):** quản lý vòng đời khách hàng `lead → tư vấn → chốt → nhập học`, mỗi bước có dữ liệu để đo tỉ lệ chuyển đổi (EZSale ở giai đoạn đầu, migrate sang .NET CRM sau).
- **.NET Finance Service:** đối soát thanh toán SePay (thay 2 giờ tay/ngày — N3), tính thù lao giảng viên (N4), tính hoa hồng CTV (N5), mã khuyến mãi — tự động dựa trên dữ liệu sạch; sau đó **push journal entries lên MISA SME Online**.
- **Kế toán chính thức (MISA SME Online):** sổ kế toán, P&L, báo cáo thuế chuẩn pháp lý VN — kế toán HSA đã quen; nhận sync 1 chiều từ .NET Finance Service.
- **Dashboard BGĐ:** báo cáo **thời gian thực** theo các trục `kỳ thi × cơ sở × kênh × CTV` từ PostgreSQL HSA — chấm dứt tình trạng "điều hành mù" (N7, Mục 2.5).
- **Cầu nối .NET → MISA:** Platform push dữ liệu kế toán lên MISA **qua MISA API (sync 1 chiều .NET → MISA)**, **KHÔNG truy cập trực tiếp DB của MISA**. Đây là quyết định kiến trúc then chốt: PostgreSQL HSA là SSOT, MISA chỉ là hệ kế toán bên ngoài nhận dữ liệu — cho phép thay phần mềm kế toán mà không mất dữ liệu lõi.

### Lớp 3 — Channels & Experience

Đây là lớp học sinh, phụ huynh và CTV **trực tiếp nhìn thấy và tương tác**.

- **Zalo OA (tin nhắn ZNS template):** gửi tin nhắn đã được xác thực mẫu trước (ZNS) cho: onboarding (SBD + hướng dẫn), cảnh báo vắng học, thông báo hoa hồng. Có webhook nhận phản hồi từ học sinh/phụ huynh.
- **Email (qua SMTP):** kênh dự phòng tự động cho hướng dẫn onboarding — đảm bảo HS vẫn nhận thông tin nếu chưa theo dõi Zalo OA.
- **Web portal:** cải thiện trải nghiệm — **giảm hành trình sau thanh toán từ 8 bước xuống 3 bước** (thanh toán → nhận SBD → vào lớp), xóa các form khai báo thừa (Mục 2.1).
- **CTV portal:** CTV **tự xem hoa hồng thời gian thực**, hết cảnh chờ kế toán tính tay và tranh chấp (N5, P3).
- **Parent portal (Giai đoạn 2):** phụ huynh xem tiến trình học của con, nhận cảnh báo chủ động — nền tảng cho việc khai thác tệp phụ huynh (P9).

## 3.3 Bốn luồng tự động hóa cốt lõi

Bốn luồng dưới đây là "trái tim" của hệ thống, triển khai theo thứ tự ưu tiên. Mỗi luồng được mô tả bằng diagram bước-bước để cả BGĐ lẫn technical lead cùng hiểu.

### Flow A — Thanh toán → Onboarding tự động (Ưu tiên 1, Giai đoạn 1)

Đây là luồng quan trọng nhất, giải quyết trực tiếp nỗi đau lớn nhất (onboarding 2–8 giờ → dưới 2 phút) và xóa các điểm lỗi đơn R3/N1/N2.

```
HS thanh toán → SePay webhook (PUSH) → HMAC verify → Hangfire job:
  [1] Sinh SBD atomic (PostgreSQL UPDATE...RETURNING)
        → format HSA-2026-08421, không bao giờ trùng
  [2] ClassIn V1 API: addSchoolStudent + addCourseStudent
        → học sinh được cấp quyền vào đúng lớp
  [3] Zalo ZNS: gửi SBD + mã kích hoạt + hướng dẫn 3 bước
  [4] Email backup: template hướng dẫn (phòng khi chưa follow OA)
  [5] MISA sync: .NET Finance Service push journal entry sang MISA SME (định kỳ)
  ────────────────────────────────────────────────────────────────
  → Toàn bộ hoàn thành < 2 phút. Học sinh vào lớp NGAY, 24/7.
```

**Xử lý ngoại lệ (nguyên tắc Exception-Based):**
- **Idempotent:** nếu SePay gửi trùng một webhook (cùng giao dịch) → hệ thống nhận diện và **bỏ qua**, không tạo 2 SBD cho cùng một lần thanh toán.
- **Retry exponential backoff:** nếu ClassIn lỗi tạm thời → tự thử lại **3 lần** với khoảng cách tăng dần.
- **Alert:** nếu sau 3 lần vẫn lỗi → Exception Handler gửi **alert Zalo cho Quản lý lớp** phụ trách, kèm SBD và lý do → người xử lý trong vài phút thay vì học sinh kẹt nhiều giờ.

> **Giá trị BGĐ:** đây là luồng biến nỗi đau "260 HS chờ 8 giờ trong đợt khai giảng HCM" thành "260 HS vào lớp trong vài phút, không thêm một nhân công nào".

### Flow B — Chăm sóc chủ động (ClassIn Data Subscription, Giai đoạn 2)

Giải quyết P7 ("học sinh ghost") — phát hiện học sinh sắp bỏ học **trước khi đã quá muộn**.

```
ClassIn PUSH event (học sinh vắng buổi) → webhook → Platform:
  [1] Ghi attendance record vào Integration DB
  [2] Nếu vắng ≥ 3 buổi liên tiếp → Hangfire job:
        • Alert Zalo cho Quản lý lớp (QLL) phụ trách đúng lớp đó
        • Ghi care log vào PostgreSQL HSA (để không sót, để bàn giao được)
  [3] QLL liên hệ học sinh → ghi kết quả → tạo .NET CRM activity
```

> **Giá trị:** ở 20.000 HS, drop-out 5% = 1.000 em không hoàn thành khóa. Phát hiện sớm vừa giữ chân (bảo vệ uy tín kết quả thi), vừa mở cơ hội up-sell đúng nhóm có nhu cầu (gia hạn, phụ đạo).

### Flow C — CTV Attribution & Hoa hồng (Giai đoạn 2)

Giải quyết N5/P3 — chấm dứt tính hoa hồng tay (2 ngày công/tháng) và tranh chấp attribution.

```
Học sinh click link CTV (vd ?ref=CTV001) →
  [1] Platform ghi cookie/session ref (gắn nguồn ngay từ đầu)
  [2] Khi học sinh thanh toán: gắn attribution vào enrollment record
  [3] Đầu tháng kế tiếp: batch job tự động tính hoa hồng
  [4] .NET Finance Service: tạo phiếu hoa hồng + xuất danh sách chuyển khoản
  [5] CTV tự xem số realtime trên CTV portal (không chờ, không cãi)
```

> **Giá trị:** quy tắc attribution **công khai và do hệ thống áp dụng tự động** → hết tranh chấp, giữ được lực lượng bán hàng — nguồn tăng trưởng chính của HSA.

### Flow D — Family CRM & Parent Nurturing (Giai đoạn 3)

Khai thác P9 — nguồn doanh thu ẩn 3,2–10,8 tỷ/năm từ tệp phụ huynh đã tin tưởng.

```
Khi HS đăng ký: ghi profile phụ huynh (SĐT, email, số con) →
  [1] Liên kết family (nhận diện anh/em cùng một phụ huynh)
  [2] Khi HS hoàn thành khóa → trigger nurture sequence cho phụ huynh:
        "Em nhỏ của [tên HS] sẽ thi ĐGNL trong [X tháng]?"
  [3] Referral: phụ huynh giới thiệu → nhận hoa hồng/ưu đãi,
        track qua ref link riêng của phụ huynh (vd ref=PH001)
```

> **Giá trị:** biến tệp khách hàng nóng nhất (phụ huynh đã trả tiền, đã tin tưởng) thành nguồn tăng trưởng gần như **không tốn chi phí marketing** — tỉ lệ chuyển đổi cao gấp 5–10 lần khách lạnh.

## 3.4 Chiến lược "Non-Disruptive Migration" — không gây gián đoạn kinh doanh

> **Đây là phần BGĐ cần yên tâm nhất.** Rủi ro lớn nhất của mọi dự án chuyển đổi không phải là kỹ thuật — mà là **làm rối loạn bộ máy đang chạy**, khiến doanh số tụt trong giai đoạn chuyển tiếp. Đề xuất này thiết kế để điều đó **không xảy ra**.

Năm nguyên tắc chống gián đoạn:

| Nguyên tắc | Cách làm cụ thể | Vì sao an toàn |
|---|---|---|
| **Giữ EZSale giai đoạn đầu** | Đội Sale **không đổi cách làm**; Platform chỉ "đọc" EZSale qua API/webhook | Quy trình tư vấn–chốt sale (phần "con người") không bị động đến |
| **Run parallel** | Chạy hệ thống mới **song song** hệ cũ tối thiểu **2 tuần** trước khi cut-over | Đối chiếu kết quả hai hệ; chỉ chuyển khi xác nhận hệ mới đúng |
| **Feature flags** | Bật/tắt từng tính năng mới **mà không ảnh hưởng production** | Triển khai từ từ; nếu một tính năng có vấn đề, tắt riêng nó |
| **Rollback plan** | Mỗi integration **tắt được riêng lẻ**, fallback về quy trình thủ công | Một tích hợp lỗi không kéo sập cả hệ; luôn có đường lui an toàn |
| **Đào tạo nhẹ** | Nhân sự **không phải học lại từ đầu** — chỉ học cách xử lý ngoại lệ trong tool mới | Đa số việc tay biến mất; nhân sự chỉ giám sát + xử lý 5% ngoại lệ |

**Lộ trình migration EZSale (giảm rủi ro tối đa):**

```
GĐ 1–2 │ EZSale giữ NGUYÊN.
       │ Platform tích hợp đọc webhook/API EZSale.
       │ → Đội Sale không cảm nhận thay đổi nào.
       ▼
GĐ 3   │ Pilot .NET CRM module với đội HCM (nhỏ hơn, ít rủi ro hơn HN).
       │ → Học từ đội nhỏ trước khi đụng đội lớn.
       ▼
GĐ 4   │ Migrate HN sang .NET CRM module.
       │ Tắt EZSale CHỈ SAU KHI confirm hệ mới chạy ổn định.
```

> **Triết lý migration:** tự động hóa phần "khô khan, lặp lại" trước (onboarding, đối soát, hoa hồng); để yên phần "con người" (tư vấn, chốt sale) đến khi đội ngũ đã quen và hệ mới đã chứng minh ổn định. Không có bước nào "đập đi xây lại" toàn bộ trong một đêm.

## 3.5 Phân tích Make vs Buy — tại sao xây nội bộ?

BGĐ có quyền hỏi: "Sao không mua phần mềm có sẵn, hay thuê công ty làm cho nhanh?" Dưới đây là so sánh thẳng thắn ba phương án.

| Tiêu chí | A: Mua SaaS có sẵn | B: Thuê agency xây | C: Tự xây nội bộ (**ĐỀ XUẤT**) |
|---|---|---|---|
| **Chi phí ban đầu** | 0 (nhưng phí thuê bao hằng tháng) | 500–800 triệu | 50–100 triệu/năm (lương CTO) |
| **Khớp với quy trình HSA** | Thấp — SaaS generic, không hiểu SBD/4 kỳ thi/Zalo | Phụ thuộc requirement rõ tới đâu | **Cao** — CTO hiểu sâu nghiệp vụ |
| **Phụ thuộc vendor** | Cao — lock-in, vendor tăng giá | Cao — bảo trì là black box | **Thấp** — code trong tay HSA |
| **Thời gian triển khai** | Nhanh (nhưng khớp kém) | 6–12 tháng | **3 tháng Giai đoạn 1** |
| **Tích hợp đặc thù VN** (SePay, Zalo OA, ClassIn) | Khó/không có | Được nhưng đắt | **Tốt** — .NET ecosystem |
| **Thay đổi theo nghiệp vụ** | Thấp (chờ vendor) | Tốn thêm phí mỗi lần | **Ngay lập tức** (CTO nội bộ) |
| **Rủi ro chính** | Không khớp nghiệp vụ | Bàn giao xong rồi bỏ mặc | Phụ thuộc chất lượng CTO |

**Kết luận — Phương án C (tự xây với CTO nội bộ) là tối ưu cho HSA vì:**

1. **Nghiệp vụ đặc thù không có SaaS nào phù hợp sẵn:** SBD theo format riêng, 4 kỳ thi, 1.800–3.600 nhóm Zalo, tích hợp ClassIn + SePay + Zalo OA — không có sản phẩm đóng gói nào trên thị trường gánh được tổ hợp này.
2. **Chi phí thấp hơn agency trong 3–4 năm** khi tính cả bảo trì: agency 500–800 triệu phát triển + 100–160 triệu/năm bảo trì; CTO nội bộ 50–100 triệu/năm và **giải quyết luôn cả việc vận hành lâu dài**.
3. **CTO làm chủ code → không lock-in:** khi nghiệp vụ đổi (mở Đà Nẵng, thêm kỳ thi), thay đổi được thực hiện ngay, không phải xếp hàng chờ vendor và trả thêm phí.
4. **Giải quyết đồng thời rủi ro R1** (phụ thuộc 1 dev outsource): có CTO nội bộ làm chủ kỹ thuật chính là lời giải cho điểm lỗi đơn nghiêm trọng nhất hiện nay.

### 3.5.1 So sánh hai giải pháp kiến trúc nền tảng (A vs B) — và lý do chốt B

Sau khi xác định **tự xây nội bộ** là hướng đi đúng, câu hỏi tiếp theo là: **stack nền tảng nên dựng thế nào?** Có hai giải pháp kiến trúc được cân nhắc nghiêm túc. Dưới đây là so sánh thẳng thắn trước khi chốt.

**Giải pháp A — Odoo Community + .NET:**
- Odoo Community làm ERP (kế toán, CRM, HR) — miễn phí license.
- .NET làm integration layer (webhook, tự động hóa, tích hợp SePay/ClassIn/Zalo).
- **Ưu:** 0 đồng license ERP; tất cả nghiệp vụ trong 1 hệ thống ERP duy nhất.
- **Nhược:** Odoo chưa phổ biến ở VN, kế toán VN không quen; chuẩn kế toán VN (Thông tư 200, báo cáo thuế) cần custom phức tạp; CTO phải có kinh nghiệm Odoo riêng; rủi ro lớn khi kế toán không dùng được → quay về Excel song song, mất luôn lợi ích ERP.

**Giải pháp B — MISA SME Online + .NET Platform (CHỌN):**
- MISA SME Online làm kế toán chính thức (~3–6 triệu/năm) — chuẩn pháp lý VN, kế toán đã quen.
- .NET Platform xử lý toàn bộ nghiệp vụ đặc thù: CRM, hoa hồng CTV, thù lao GV, đối soát SePay.
- .NET sync journal entries lên MISA định kỳ (1 chiều .NET → MISA).
- **Ưu:** kế toán dùng ngay không cần đào tạo; CTO tập trung 100% vào .NET (không phải học thêm Odoo); stack đơn giản hơn, ít điểm gãy hơn.
- **Nhược:** MISA API ít linh hoạt hơn ERP đầy đủ; cần xây CRM và HR trong .NET thay vì dùng Odoo native.

| Tiêu chí | A: Odoo Community + .NET | B: MISA SME Online + .NET (**CHỌN**) |
|---|---|---|
| **License ERP** | 0 đồng (Community) | MISA ~3–6 triệu/năm |
| **Kế toán VN quen dùng** | **Không** — Odoo lạ với kế toán VN | **Có** — MISA là chuẩn phổ biến |
| **Chuẩn pháp lý VN** (TT200, báo cáo thuế) | Phải custom phức tạp | **Sẵn có trong MISA** |
| **Yêu cầu với CTO** | Phải biết .NET **và** Odoo | Chỉ cần .NET |
| **Độ phức tạp stack** | Cao (2 nền tảng lớn) | Thấp hơn (.NET + MISA API) |
| **Rủi ro lớn nhất** | Kế toán không dùng được → về Excel | MISA API kém linh hoạt |

> **Kết luận: Chọn B.** Rào cản thực tế lớn nhất là **kế toán VN không quen Odoo** — nếu kế toán không dùng được hệ thống, toàn bộ lợi ích "license 0 đồng" của Odoo trở nên vô nghĩa và tổ chức quay lại Excel song song. MISA SME Online giải quyết triệt để rào cản này, đồng thời cho phép CTO tập trung 100% vào lớp tự động hóa .NET — nơi tạo ra giá trị thực sự khác biệt cho HSA.

## 3.6 Bảng giải pháp theo từng vấn đề đã xác định

Bảng dưới đây ánh xạ các vấn đề trọng yếu (đã phân tích ở PHẦN 1, 2, 2A, 2B) sang giải pháp cụ thể và giai đoạn xử lý — để BGĐ thấy rõ **mỗi đồng đầu tư giải quyết chính xác vấn đề nào**.

| Vấn đề | Giải pháp cụ thể | Giai đoạn |
|---|---|---|
| **B0** — Mã khuyến mãi không validate (thất thoát doanh thu ngay) | Server-side coupon validation + audit log | Tuần 1 (CTO onboard) |
| **N1** — Tạo SBD thủ công, dễ trùng/sai | Atomic SBD generation (PostgreSQL `UPDATE...RETURNING`) — tự động sau SePay webhook | GĐ 1 |
| **N2** — Duyệt vào nhóm Zalo thủ công, không kiểm soát | Zalo OA: HS nhận link join group tự động, không cần duyệt tay | GĐ 1 |
| **N3** — Đối soát SePay thủ công (~2h/ngày) | SePay webhook + auto-matching trong .NET Finance Service; người chỉ xử lý ngoại lệ | GĐ 1 |
| **R3** — Chỉ 1 người duyệt HS vào lớp (SPOF) | Flow A tự động hóa toàn chuỗi onboarding — không phụ thuộc cá nhân | GĐ 1 |
| **R1** — Phụ thuộc 1 dev outsource | Tuyển CTO nội bộ làm chủ codebase + tài liệu hóa | GĐ 0–1 |
| **R2 / N9** — Dữ liệu trên Drive cá nhân, mất là không cứu được | Tập trung dữ liệu về PostgreSQL HSA Platform có backup | GĐ 1–2 |
| **N4** — Tính thù lao GV thủ công | Tự động tính trong .NET Finance Service từ dữ liệu buổi học | GĐ 2 |
| **N5 / P3** — Tính hoa hồng CTV tay + tranh chấp | Flow C: attribution tự động + CTV portal realtime | GĐ 2 |
| **N6** — Lead nhập tay, sót/trùng | Tự động đẩy lead landing page vào CRM | GĐ 2 |
| **N7 / 2.5** — Không có dashboard realtime | Dashboard .NET Report Service (Metabase/Superset) theo kỳ thi × cơ sở × kênh × CTV | GĐ 2 |
| **N10 / P4** — Thiếu audit trail (rủi ro pháp lý PDPA) | Nhật ký truy cập tập trung + chính sách xử lý dữ liệu | GĐ 2 |
| **N11 / P1** — Nhóm Zalo không scale, "HS mất trong Zalo" | Zalo OA + CRM track membership tự động | GĐ 1–2 |
| **P2 / N12** — Vendor lock-in ClassIn, không có dữ liệu riêng | ClassIn Data Subscription → HSA sở hữu bản sao dữ liệu học tập | GĐ 2 |
| **P7** — "Học sinh ghost" (drop-out không biết) | Flow B: alert tự động khi vắng ≥ 3 buổi | GĐ 2 |
| **P8 / N8** — Google Sheet sẽ sập dưới tải lớn | Chuyển sang database thực (PostgreSQL HSA Platform) | GĐ 1–2 |
| **P9** — Chưa khai thác dữ liệu phụ huynh | Flow D: Family CRM + nurture phụ huynh + referral | GĐ 3 |
| **N13 / P5** — Thiếu chuẩn hóa → không mở Đà Nẵng được | Quy trình chuẩn hóa + tự động → cơ sở mới "cắm vào là chạy" | GĐ 3–4 |
| **2.6** — Lãnh đạo bị vùi đầu trong vận hành | Tự động hóa + dashboard → giải phóng năng lực lãnh đạo | GĐ 1–2 |

## 3.7 Bảng So sánh Trước / Sau

| # | Tình huống vận hành | HIỆN TẠI | SAU CHUYỂN ĐỔI |
|---|---|---|---|
| 1 | HS thanh toán xong → nhận số báo danh | 2–8 giờ (tùy giờ hành chính) | **< 2 phút** (tự động 24/7) |
| 2 | Nhân viên duyệt học sinh nghỉ phép | Toàn bộ chuỗi nhập học **tắc** | Hệ thống tự chạy, **không phụ thuộc cá nhân** |
| 3 | Tạo SBD + gửi email + kích hoạt ClassIn | Làm tay ~15 phút/HS | **Tự động, vài giây/HS** |
| 4 | Đợt cao điểm HCM 260 HS/ngày | Quá tải, dồn ứ, sai sót | Hệ thống xử lý **không thêm nhân công** |
| 5 | Đối soát thanh toán SePay | ~2 giờ/ngày làm tay | **< 10 phút/ngày** (chỉ xử lý ngoại lệ) |
| 6 | Tính thù lao 70 giảng viên | ~1 ngày công/tháng | **Tự động, vài phút** |
| 7 | Tính hoa hồng 132–137 CTV | ~2 ngày công/tháng, hay tranh chấp | **Tự động + CTV tự xem realtime** |
| 8 | Lead từ landing page vào CRM | Nhập tay, bị sót/trùng/chậm | Tự động, **không sót không trùng** |
| 9 | BGĐ xem số liệu vận hành | Tổng hợp tay, báo cáo chậm | **Dashboard thời gian thực** |
| 10 | Nhân viên nghỉ việc mang theo dữ liệu | Mất dữ liệu (Drive cá nhân) | Dữ liệu nằm ở **kho chung an toàn** |
| 11 | Truy vết ai đã xem/sửa dữ liệu HS | Không có | **Có nhật ký** (giảm rủi ro pháp lý) |
| 12 | Lịch sử tư vấn khi Sale nghỉ | Mất (nằm trong Zalo cá nhân) | Lưu tập trung, **bàn giao được** |
| 13 | Quản lý membership nhóm Zalo | Dò tay, "HS mất trong Zalo" | **Track tự động qua Zalo OA + CRM** |
| 14 | Theo dõi chuyên cần / cảnh báo vắng | Không có, biết khi đã quá muộn | **Alert tự động khi vắng 3+ buổi** |
| 15 | Hành trình đăng ký của học sinh | **8 bước** phức tạp, nhiều form thừa | **3 bước** (thanh toán → nhận SBD → vào lớp) |
| 16 | Phụ huynh muốn biết tiến trình con | Không có thông tin, phải nhắn hỏi | **Nhận alert tự động** (parent portal) |
| 17 | Marketing đo ROI từng kênh | Không biết kênh nào hiệu quả | **Dashboard attribution realtime** |
| 18 | Giám đốc kinh doanh dùng thời gian | Đối soát tay, xử lý sự vụ | **Tập trung mở rộng thị trường** |

## 3.8 Xây dựng lại nền tảng Website — Điểm tiếp xúc tài chính quan trọng nhất

> Website **hsavnu.edu.vn** là nơi học sinh gặp HSA lần đầu, nơi ra quyết định mua và nơi thực hiện thanh toán. Hiện tại nó đang gánh nhiều hạn chế nghiêm trọng cần được giải quyết cùng với việc xây dựng HSA Integration Platform.

### Hạn chế hiện tại của hsavnu.edu.vn (đã khảo sát thực tế)

| # | Hạn chế | Mức độ | Ảnh hưởng |
|---|---|---|---|
| **1** | **Mã khuyến mãi không validate phía server** (B0) | 🔴 CRITICAL | Thất thoát doanh thu đang xảy ra |
| **2** | **Dữ liệu khoá học thiếu**: thời lượng hiển thị "0 giờ 0 phút" — bug dữ liệu chưa được điền | 🟠 CAO | Giảm độ tin cậy, học sinh không có đủ thông tin để quyết định |
| **3** | **Không có luồng thanh toán thông minh**: sau khi nhấn "Đăng ký học", học sinh phải trải qua 8 bước thủ công (xem Phần 2.1) | 🔴 CAO | Tỉ lệ rời bỏ cao, trải nghiệm kém |
| **4** | **Không có dashboard học sinh** sau khi đăng nhập: không thấy SBD, lịch học, tình trạng khóa học | 🟠 CAO | Học sinh phải hỏi nhân viên để biết trạng thái |
| **5** | **Email doanh nghiệp dùng Gmail** (hsaeducation.jsc@gmail.com): không chuyên nghiệp, không có SPF/DKIM đảm bảo, email tự động dễ vào spam | 🟠 CAO | Email thông báo SBD, hướng dẫn có thể không tới tay học sinh |
| **6** | **CDN Digital Ocean Spaces** (không phải Cloudflare): tốc độ tải trang tại Việt Nam kém hơn, không có DDoS protection mặc định | 🟡 TB | Trang chậm → tăng tỉ lệ bounce, giảm SEO |
| **7** | **Không có social proof** (đánh giá, điểm số cựu học sinh, tỉ lệ đỗ): học sinh không có cơ sở so sánh với đối thủ | 🟠 CAO | Giảm tỉ lệ chuyển đổi từ khách truy cập → mua |
| **8** | **Thiếu cơ chế live chat / hỏi nhanh**: học sinh có câu hỏi → không thể hỏi ngay → thoát trang | 🟡 TB | Mất khách hàng ở giai đoạn cân nhắc |
| **9** | **Không có portal CTV** tích hợp: CTV không tự xem được hiệu suất và hoa hồng | 🟠 CAO | CTV thiếu động lực, tranh chấp tăng |
| **10** | **Không có portal phụ huynh**: phụ huynh không theo dõi được tiến trình con | 🟡 TB | Mất cơ hội gắn kết, giảm cơ hội upsell |
| **11** | **Bảo mật B1–B7** (HMAC, CSRF, MFA, rate limiting) — chi tiết tại PHẦN 2B | 🔴 CRITICAL | Rủi ro hack, mất dữ liệu, gian lận |
| **12** | **Form đăng ký giới hạn năm sinh 2006–2009**: bỏ sót học sinh lớn tuổi (BCA, BQP có thí sinh đến 25 tuổi) | 🟡 TB | Mất khách hàng tiềm năng ở phân khúc BCA/BQP |

### Roadmap xây dựng lại Website (tích hợp với lộ trình 4 GĐ)

```
GĐ 0 (T8–9/2026): Security hardening
├── Fix B0: server-side coupon validation
├── Fix B1: HMAC webhook verification
├── Fix B2: bcrypt thay MD5
└── Fix B3–B7: CSRF, reCAPTCHA, MFA admin

GĐ 1 (T10–12/2026): Onboarding flow rebuild
├── Sau thanh toán: redirect thẳng vào "trang chờ" realtime
│   (hiển thị tiến trình: ✅ SBD đã tạo → ✅ ClassIn đã kích hoạt → ✅ Zalo group)
├── Xóa form khai báo thứ 2 sau thanh toán — lấy từ dữ liệu đăng ký
└── Student dashboard: SBD, lịch học, trạng thái gói

GĐ 2 (T2–4/2027): CTV & Social Proof
├── CTV portal: xem hoa hồng realtime, link ref, danh sách học sinh giới thiệu
├── Thêm social proof: điểm cựu học sinh, tỉ lệ đỗ theo kỳ, đánh giá
├── Tích hợp Zalo Mini App hoặc live chat widget
└── Fix data bug "0 giờ 0 phút", điền đầy đủ thông tin khoá học

GĐ 3 (T5–10/2027): Parent & Marketing
├── Parent portal: xem tiến trình, điểm danh, alert vắng học
├── Marketing attribution: tag UTM + CTV ref vào mọi conversion
├── Chuyển sang email hosting chuyên nghiệp (Google Workspace đã có)
└── Chuyển CDN sang Cloudflare (free tier đủ, tốt hơn DO Spaces)
```

### Lưu ý về hsa.edu.vn

> **Làm rõ cho BGĐ:** website `hsa.edu.vn` (không có "vnu" trong tên) là **cổng đăng ký kỳ thi chính thức của Đại học Quốc gia Hà Nội** — do ĐHQG HN vận hành, không phải do HSA Education. Website của HSA Education (công ty) là `hsavnu.edu.vn`. Hai site phục vụ mục đích khác nhau và thuộc hai tổ chức khác nhau. Việc xây dựng lại trong đề xuất này áp dụng cho `hsavnu.edu.vn`.

---

# PHẦN 4 — LỢI ÍCH KỲ VỌNG

## 4.1 Tiết kiệm nhân công (quy ra tiền)

Đây là lợi ích **định lượng được, đo trực tiếp từ thực tế.** Áp đơn giá nhân công **150.000 VND/giờ** (ước tính chi phí nhân sự đã gồm phụ phí):

| Nguồn tiết kiệm | Cách tính | Giá trị/năm |
|---|---|---|
| **Onboarding tự động** | Giảm ~13h/ngày × 250 ngày làm việc = 3.250h/năm | **~487 triệu VND** |
| **Đối soát SePay** | Giảm ~1,75h/ngày × 250 ngày = 437h/năm | **~65 triệu VND** |
| **Thù lao GV + hoa hồng CTV** | Giảm ~3 ngày/tháng × 8h × 12 = 288h/năm | **~43 triệu VND** |
| | | |
| **TỔNG TIẾT KIỆM ĐỊNH LƯỢNG** | | **~595 triệu VND/năm** |

> Lưu ý: con số này **chỉ tính phần đo được trực tiếp.** Chưa tính các lợi ích "mềm" (giảm sai sót, giảm tranh chấp, giữ chân học viên, tránh rủi ro pháp lý) — những thứ có giá trị thực nhưng khó quy ra tiền chính xác.

### 4.1.1 Tính lại chi phí hao phí thực tế (lớn hơn nhiều con số nhân công đo trực tiếp)

> Con số ~595 triệu/năm ở trên chỉ là phần **nhân công onboarding/đối soát đo trực tiếp**. Khi tính đầy đủ cả chi phí cơ hội và rò rỉ doanh thu, **chi phí hao phí thực tế lớn hơn nhiều**. Dưới đây là ước tính toàn diện.

**(1) Chi phí nhân lực trực tiếp bị lãng phí:**
- 504h/tháng thao tác tay × mức lương TB nhân viên vận hành (15–20 triệu/tháng ÷ 22 ngày ÷ 8h = ~85.000–114.000đ/h).
- 504h × 100.000đ/h = **50,4 triệu/tháng ≈ ~605 triệu/năm** (nhân lực trực tiếp).

**(2) Chi phí cơ hội — lãnh đạo kinh doanh (lớn hơn nhiều):**
- 2 Giám đốc kinh doanh dành ~30% thời gian cho vận hành thủ công thay vì mở rộng thị trường.
- Lương GĐ KD ~30–50 triệu/tháng → chi phí cơ hội: 2 × 30% × 40 triệu = **~24 triệu/tháng ≈ ~288 triệu/năm**.

**(3) Doanh thu bị ảnh hưởng bởi lỗi onboarding:**
- Học sinh rời bỏ do trải nghiệm onboarding tệ (error rate hiện tại 3–5%).
- 20.000 HS × 3% error × 2,5 triệu/HS = **~1.500 triệu/năm** tiềm năng doanh thu bị ảnh hưởng (lấy biên thận trọng ~500–1.500 triệu/năm).

**(4) Lỗ hổng B0 (promo code bất kỳ = giảm tiền):**
- **Không thể ước tính chính xác nhưng đang chảy máu hàng ngày** (xem PHẦN 2B — B0).

**Tổng chi phí hao phí ước tính:**

| Thành phần hao phí | Giá trị/năm |
|---|---|
| Nhân lực trực tiếp (504h/tháng) | ~605 triệu |
| Chi phí cơ hội GĐ kinh doanh | ~288 triệu |
| Doanh thu bị ảnh hưởng bởi lỗi onboarding | ~500–1.500 triệu |
| Lỗ hổng B0 (promo code) | không ước tính được — đang chảy máu |
| **TỔNG HAO PHÍ ƯỚC TÍNH** | **~1.400–2.400 triệu/năm (~1,4–2,4 tỷ VND)** |

> **Đọc cho BGĐ:** chi phí hao phí thực tế **~1,4–2,4 tỷ/năm** — lớn hơn nhiều so với con số tiết kiệm nhân công đo trực tiếp (~595 triệu). Đối chiếu với chi phí đội ngũ kỹ thuật đầy đủ (~1.260–2.100 triệu/năm, xem Mục 5.2), đầu tư vẫn hợp lý ngay cả khi tính theo kịch bản hao phí thận trọng nhất, và **payback nhanh hơn** khi tính đủ chi phí cơ hội + rò rỉ doanh thu.

## 4.2 Trải nghiệm học viên

- Học sinh trả tiền **được vào lớp gần như tức thì** thay vì chờ vài giờ.
- Thông tin (SBD, hướng dẫn, cảnh báo vắng học) được gửi **đúng lúc, không sót.**
- Trải nghiệm chuyên nghiệp, đồng đều, **không phụ thuộc hôm đó nhân viên có rảnh hay không.**

## 4.3 Scale được

- Đợt cao điểm HCM 260 HS/ngày được xử lý **không cần thêm người.**
- HSA có thể **mở rộng học sinh mà không phải tăng nhân công vận hành theo tỉ lệ thuận** — đây là điều kiện tiên quyết để mở rộng có lãi.
- **Mở Đà Nẵng trở nên khả thi:** cơ sở mới "cắm vào hệ thống là chạy" thay vì khởi tạo lại việc tay từ đầu.

**Tăng trưởng giá trị tự động hóa theo scale:** vì BGĐ xác nhận HCM ×2 (2026) và ×1,5 (2027), khối lượng việc tay nếu làm thủ công sẽ phình theo — nhưng với tự động hóa, giá trị tiết kiệm tăng tương ứng trong khi chi phí gần như cố định:

| Năm | HS/ngày | Tải tay (15 phút/HS) | **Tiết kiệm onboarding/năm** |
|---|---|---|---|
| 2025 | ~55 | ~13h/ngày × 250 ngày | **~487 triệu** (baseline) |
| 2026 | ~77 | ~19h/ngày × 250 ngày × 150K/h | **~712 triệu** |
| 2027 | ~101 | ~25h/ngày × 250 ngày × 150K/h | **~937 triệu** |

> Đây là đòn bẩy quan trọng nhất của tự động hóa: khi quy mô tăng gấp đôi rồi gấp ba, chi phí biên cho mỗi học sinh tăng thêm gần **bằng không**, còn giá trị tiết kiệm tăng tuyến tính theo số học sinh.

## 4.4 BGĐ có dữ liệu

- **Dashboard thời gian thực** theo trục: kỳ thi × cơ sở × kênh × CTV.
- BGĐ trả lời được ngay: hôm nay tuyển bao nhiêu, doanh thu theo kỳ thi, hiệu quả từng CTV, tỉ lệ chốt.
- Quyết định dựa trên **số liệu sống**, không phải báo cáo cũ.

## 4.5 Giảm rủi ro

| Rủi ro hiện tại | Sau chuyển đổi |
|---|---|
| R3 — 1 người duyệt HS là điểm tắc | Tự động hóa → **xóa bỏ điểm tắc** |
| R2 — dữ liệu trong Drive cá nhân | Dồn về **kho dữ liệu chung** |
| R4 — tranh chấp hoa hồng CTV | Tính tự động, minh bạch → **giảm tranh chấp** |
| R8 — lead sót/trùng | Tự động → **sạch dữ liệu lead** |
| R11 — không có nhật ký dữ liệu | Có nhật ký → **giảm rủi ro PDPA** |
| R1 — phụ thuộc 1 dev outsource | CTO nội bộ làm chủ + tài liệu hóa → **giảm phụ thuộc** |
| P1 — nhóm Zalo chạm giới hạn | Track membership tự động → **không "mất HS trong Zalo"** |
| P2 — ClassIn vendor lock-in | Data Subscription → **sở hữu dữ liệu, có đòn bẩy** |
| P7 — học sinh ghost / drop-out | Alert vắng học → **giữ chân + cơ hội up-sell** |
| P8 — Google Sheet sập | Database thực → **chịu tải lớn, an toàn dữ liệu** |

---

# PHẦN 5 — LỘ TRÌNH & CHI PHÍ

## 5.1 Bốn giai đoạn (18 tháng)

Lộ trình được chia nhỏ để **kiểm chứng từng bước**, BGĐ có quyền dừng/tiếp ở mỗi mốc:

| Giai đoạn | Thời gian | Mục tiêu | Kết quả đo được |
|---|---|---|---|
| **GĐ 0 — Dựng nền** | **Tháng 8–9/2026** | **Tuyển dụng CTO**, dựng hạ tầng máy chủ, đăng ký kênh tin nhắn Zalo, đăng ký MISA SME Online | CTO onboard + hệ thống nền sẵn sàng |
| **GĐ 1 — Onboarding tự động** | **Tháng 10–12/2026** | Tự động hóa chuỗi: thanh toán → SBD → ClassIn → Zalo | **Time-to-SBD < 2 phút**; bỏ ~13h/ngày việc tay |
| **Checkpoint GĐ 1 / Quyết định GĐ 2** | **Tháng 1/2027** | Nghiệm thu 2 chỉ số giá trị; BGĐ quyết định tiếp GĐ 2 | Quyết định go/no-go có dữ liệu |
| **GĐ 2 — Dữ liệu lớp + CTV** | **Tháng 2–4/2027** | Lấy dữ liệu điểm danh/điểm số từ ClassIn; tính hoa hồng CTV tự động | Chăm sóc chủ động + hoa hồng minh bạch |
| **GĐ 3 — Quản trị & Dashboard** | **Tháng 5–10/2027** | Đưa CRM, báo cáo lên .NET Platform + sync kế toán lên MISA SME; dashboard cho BGĐ | **Đối soát SePay < 10 phút/ngày**; dashboard realtime |
| **GĐ 4 — Tối ưu & Scale** | **Tháng 11/2027–3/2028** | Tối ưu hiệu năng, chuẩn hóa cho mở rộng (sẵn sàng Đà Nẵng) | Sẵn sàng scale, vận hành ổn định |

> **Lưu ý quan trọng:** GĐ 0 nay bao gồm **tuyển dụng CTO** như một kết quả bàn giao bắt buộc — **không có CTO thì không thể xây dựng được gì.** Đây là lý do cần mở JD tuyển CTO ngay sau khi BGĐ phê duyệt.

## 5.2 Nguồn lực: Tuyển CTO nội bộ — lợi thế chiến lược

> **Đây là điểm khiến đề xuất này có ROI vượt trội so với mọi phương án thuê ngoài.**

> **Điều kiện tiên quyết:** HSA **hiện CHƯA CÓ CTO**. Tuyển CTO là **điều kiện tiên quyết cho toàn bộ lộ trình** — phê duyệt T7/2026 thì phải **bắt đầu tuyển ngay lập tức**, mục tiêu **onboard T8/2026**. Không có CTO, không có giai đoạn nào khởi động được.

Đề xuất tuyển dụng 01 CTO (Chief Technology Officer) nội bộ ở mức senior, chịu trách nhiệm toàn bộ thiết kế, phát triển và vận hành kỹ thuật HSA Integration Platform.

- **Mức lương:** 50–100 triệu VND/tháng (theo kinh nghiệm và thỏa thuận).
- **So sánh:** thuê agency làm tương đương tốn 500–800 triệu, chưa kể bảo trì, không có cam kết bàn giao và knowledge transfer.
- **Lợi thế CTO nội bộ:** làm chủ kỹ thuật lâu dài, tài liệu hóa trong nhà, không phụ thuộc bên ngoài — giải quyết trực tiếp R1 (phụ thuộc 1 dev outsource).
- **CTO không chỉ viết code mà làm chủ kiến trúc công nghệ tổng thể:** đánh giá và chọn công cụ, ngăn chặn quyết định công nghệ tùy tiện (xem rủi ro P10), đảm bảo mọi đầu tư công nghệ có chiến lược rõ ràng.
- **COO đóng vai Product Owner — KHÔNG làm kỹ thuật:** xác nhận yêu cầu, phê duyệt thiết kế, đánh giá kết quả — đây là vai trò phù hợp với vị trí COO, không cần code.
- **Ưu tiên tuyển** người có kinh nghiệm .NET + tích hợp API, hiểu quy trình EdTech là lợi thế.

### Đội ngũ kỹ thuật đầy đủ (chi phí thực tế)

CTO không làm một mình. Đội ngũ kỹ thuật tối thiểu cần thiết và chi phí thực tế:

| Vị trí | Vai trò | Chi phí/tháng |
|---|---|---|
| **CTO (tuyển mới)** | Làm chủ kiến trúc, phát triển core, vận hành | **50–100 triệu** |
| **Fullstack Dev** (.NET + Next.js) | Phát triển tính năng, tích hợp | **30–40 triệu** |
| **Fresher** | Hỗ trợ phát triển, task đơn giản | **15 triệu** |
| **UI/UX + QC** (freelancer) | Thiết kế giao diện + kiểm thử | **~10–20 triệu** |
| | | |
| **TỔNG** | | **~105–175 triệu/tháng ≈ ~1.260–2.100 triệu/năm** |

> **Lưu ý cho BGĐ:** chi phí đội ngũ kỹ thuật đầy đủ trên là khoản đầu tư nhân lực lớn nhất của dự án. Tuy nhiên, đối chiếu với **chi phí hao phí thực tế ~1.400–2.400 triệu/năm** (xem Mục 5.4) mà hệ thống thủ công đang gây ra, đầu tư này vẫn hoàn vốn nhanh. CTO là vị trí cốt lõi và bắt buộc; các vị trí còn lại có thể bổ sung dần theo tiến độ từng giai đoạn.

## 5.3 Chi phí chi tiết theo giai đoạn

### Bảng chi phí TIỀN MẶT

| Giai đoạn | Hạng mục tiền mặt | Chi phí |
|---|---|---|
| **GĐ 0** (T8–9/2026) | Đăng ký kênh tin nhắn Zalo (ZNS) + mẫu tin (một lần) | ~5–10 triệu |
| | Máy chủ (2 tháng đầu) | ~8–10 triệu |
| | Lương CTO (2 tháng) | ~8–17 triệu |
| **GĐ 1** (T10–12/2026) | Máy chủ (3 tháng) | ~12–15 triệu |
| | Phí tin nhắn Zalo (bắt đầu phát sinh) | ~3 triệu |
| | Lương CTO (3 tháng) | ~13–25 triệu |
| **GĐ 2** (T2–4/2027) | Máy chủ (3 tháng) | ~12–15 triệu |
| | Phí tin nhắn Zalo | ~4 triệu |
| **GĐ 3** (T5–10/2027) | Máy chủ (6 tháng) | ~24–30 triệu |
| | Phí tin nhắn Zalo | ~8 triệu |
| | Google Workspace (email công ty, 20 người) | ~17,5 triệu (nửa năm) |
| **GĐ 4** (T11/2027–3/2028) | (đã sang năm 2 — chi phí duy trì) | xem năm 2 |

### Bảng tổng chi phí TIỀN MẶT theo năm

| Hạng mục | Năm 1 | Năm 2+ |
|---|---|---|
| Phần mềm MISA SME Online (kế toán) | **~3–6 triệu** | ~3–6 triệu |
| Máy chủ (chính + kiểm thử) | 50–60 triệu | 50–60 triệu |
| Đăng ký Zalo ZNS (một lần) | 5–10 triệu | — |
| Phí tin nhắn Zalo | ~16 triệu (GĐ 1) — scale lên ~32–48 triệu theo volume | ~16–48 triệu |
| Google Workspace (20 người) | ~35 triệu | ~35 triệu |
| Tích hợp ClassIn | 0 (trong hợp đồng) | 0 |
| **TỔNG HẠ TẦNG (tiền mặt)** | **~109–127 triệu** | **~104–117 triệu** |
| **Lương CTO nội bộ** | **~50–100 triệu** | **~50–100 triệu** |
| **TỔNG CHI PHÍ** | **~156–221 triệu** | **~151–211 triệu** |

### Giá trị phần phát triển nội bộ (không phải tiền mặt — để BGĐ thấy lợi thế)

| | Thuê agency ngoài | CTO nội bộ |
|---|---|---|
| Chi phí phát triển | 500–800 triệu (một lần) | Lương 50–100 triệu/năm |
| Bảo trì hàng năm | 100–160 triệu/năm | Đã gồm trong lương CTO |
| Bàn giao & knowledge transfer | thường không cam kết | **làm chủ + tài liệu hóa trong nhà** |
| Rủi ro phụ thuộc | cao (lệ thuộc nhà cung cấp) | thấp (làm chủ trong nhà — giải quyết R1) |

## 5.4 ROI & Thời gian hoàn vốn

> **Hai cách nhìn về lợi ích:** (a) **tiết kiệm nhân công đo trực tiếp** (~595 triệu/năm, tăng theo scale) — con số thận trọng; (b) **chi phí hao phí thực tế đầy đủ** (~1.400–2.400 triệu/năm, gồm cả chi phí cơ hội lãnh đạo + rò rỉ doanh thu onboarding + lỗ hổng B0 — xem Mục 4.1.1). Bảng ROI dưới đây dùng song song cả hai để BGĐ thấy cả kịch bản thận trọng lẫn kịch bản đầy đủ.

| Chỉ tiêu | Giá trị |
|---|---|
| Tiết kiệm nhân công 2025 (baseline, đo trực tiếp) | **+595 triệu VND/năm** |
| Tiết kiệm nhân công 2026 (HS ~28.000) | **+712 triệu VND/năm** |
| Tiết kiệm nhân công 2027 (HS ~37.000) | **+937 triệu VND/năm** |
| **Chi phí hao phí thực tế đầy đủ** (gồm cơ hội + doanh thu, Mục 4.1.1) | **+1.400 đến +2.400 triệu VND/năm** |
| Tổng chi phí năm 1 (hạ tầng + CTO, kịch bản chỉ CTO) | −156 đến −221 triệu VND |
| Tổng chi phí năm 1 (hạ tầng + đội kỹ thuật đầy đủ) | ~−1.360 đến −2.230 triệu VND |
| **ROI ròng năm 1 — kịch bản tiết kiệm trực tiếp / chỉ CTO** | **+374 đến +439 triệu VND** |
| **ROI ròng năm 1 — kịch bản hao phí đầy đủ / đội đầy đủ** | **dương, payback nhanh hơn nhờ chặn rò rỉ doanh thu + giải phóng lãnh đạo** |
| Tổng chi phí năm 2+ (hạ tầng + CTO) | −151 đến −211 triệu VND/năm |
| **ROI ròng năm 2** (dùng tiết kiệm 2026 ~712 triệu) | **+501 đến +561 triệu VND** |
| **Thời gian hoàn vốn (Payback)** | **~3–4 tháng (kịch bản chỉ CTO); nhanh hơn khi tính đủ hao phí ~1,4–2,4 tỷ/năm** |

> **Cách đọc đơn giản cho BGĐ:** ngay cả khi đã tính lương CTO, mỗi 1 đồng bỏ ra năm 1 vẫn thu về khoảng **2,7–3,8 đồng tiết kiệm** (kịch bản thận trọng). Khi tính đủ chi phí hao phí thực tế (~1,4–2,4 tỷ/năm), payback còn nhanh hơn. Vốn được hoàn lại trong khoảng 3–4 tháng. Quan trọng hơn: tiết kiệm **tăng theo scale** (712 triệu năm 2026, 937 triệu năm 2027) trong khi chi phí gần như cố định → ROI năm 2+ còn mạnh hơn năm 1.

> **Lưu ý thận trọng (đúng tinh thần Principal PO):** con số tiết kiệm đầy đủ ~595 triệu chỉ đạt được khi **hoàn tất đến Giai đoạn 3.** Giai đoạn 1 (3 tháng đầu) đã mang lại phần lớn (~487 triệu từ onboarding). Vì vậy ngay cả kịch bản thận trọng nhất — chỉ làm xong Giai đoạn 1 — đề xuất vẫn **lãi đậm.**

> **Lưu ý về giá trị chiến lược (không nằm trong ROI tiền mặt):** ngoài ~595 triệu tiết kiệm, đề xuất còn **mở khóa khả năng nâng thị phần** (từ ~7,4% lên 10% TAM = +36% học sinh) và **mở khóa khả năng mở rộng sang thị trường mới như Đà Nẵng** (~3.500–4.600 HS tiềm năng — **giả định tiềm năng, cần nghiên cứu thực địa trước khi quyết định**). Những giá trị này lớn hơn nhiều con số tiết kiệm, nhưng được giữ ngoài tính toán ROI để bảo toàn tính thận trọng.

## 5.5 Rủi ro triển khai

| Rủi ro | Xác suất | Tác động | Cách kiểm soát |
|---|---|---|---|
| Tuyển CTO chậm, dự án trễ khởi động | Trung bình | Cao | Mở JD ngay sau phê duyệt; ưu tiên ứng viên .NET + tích hợp API; COO (Product Owner) tham gia phỏng vấn |
| Phụ thuộc 1 CTO nội bộ trong giai đoạn đầu | Trung bình | Cao | **Bắt buộc tài liệu hóa** từ đầu; code + tài liệu là tài sản công ty, không phải cá nhân; chuẩn hóa repo & quy trình để có thể tiếp nhận thêm dev khi scale |
| Tích hợp ClassIn/Zalo gặp giới hạn kỹ thuật | Thấp–TB | Trung bình | Đã khảo sát khả năng tích hợp trước (tài liệu kỹ thuật riêng); làm thử ở bản kiểm thử trước khi chạy thật |
| Đội Sale/CTV phản ứng với thay đổi | Thấp | Trung bình | **Giữ nguyên EZSale** giai đoạn đầu — Sale không phải đổi cách làm |
| Dữ liệu sai trong giai đoạn chuyển tiếp | Thấp | Cao | Chạy song song hệ thống cũ + mới một thời gian, đối chiếu trước khi cắt hẳn |
| Vượt ngân sách tiền mặt | Thấp | Thấp | Chi phí chủ yếu cố định (máy chủ, Zalo); không có hạng mục dễ đội giá |

> **Đánh giá tổng thể rủi ro:** thấp. Phần chi tiền mặt nhỏ và dễ dự báo; phần tốn công (phát triển) do CTO nội bộ làm chủ và tài liệu hóa; lộ trình chia nhỏ cho phép dừng bất cứ lúc nào mà không mất tiền lớn.

---

# PHẦN 6 — ĐỀ NGHỊ PHÊ DUYỆT

## 6.1 Quyết định cụ thể đề nghị BGĐ phê duyệt

1. **Phê duyệt chủ trương** xây dựng HSA Integration Platform theo lộ trình 4 giai đoạn / 18 tháng (T8/2026 – T3/2028) nêu tại Phần 5.

2. **Phê duyệt ngân sách Giai đoạn 0 + 1 (T8–12/2026):**

   Ngân sách phụ thuộc quy mô đội ngũ kỹ thuật được phê duyệt. Hai kịch bản:

   **Kịch bản tối thiểu (chỉ CTO + hạ tầng):**

   | Hạng mục | Ngân sách đề nghị |
   |---|---|
   | Máy chủ (T8–12/2026, 5 tháng) | ~20–25 triệu |
   | Đăng ký kênh Zalo ZNS + mẫu tin | ~5–10 triệu |
   | Phí tin nhắn Zalo (bắt đầu T10) | ~3 triệu |
   | Lương CTO (5 tháng T8–12/2026, 50–100 triệu/tháng) | ~250–500 triệu |
   | **TỔNG (kịch bản chỉ CTO)** | **~278–538 triệu VND** |

   **Kịch bản đội ngũ kỹ thuật đầy đủ (theo Mục 5.2, ~105–175 triệu/tháng):**

   | Giai đoạn | Thời gian | Chi phí nhân lực |
   |---|---|---|
   | **GĐ 0** | 2 tháng (T8–9/2026) | **~210–350 triệu** |
   | **GĐ 1** | 3 tháng (T10–12/2026) | **~315–525 triệu** |
   | Hạ tầng (máy chủ + Zalo, 5 tháng) | — | ~28–38 triệu |
   | **TỔNG (kịch bản đội đầy đủ GĐ 0+1)** | | **~553–913 triệu VND** |

   > Đây là toàn bộ tiền mặt cần để **kiểm chứng giá trị** của dự án qua giai đoạn mang ROI cao nhất. BGĐ có thể chọn khởi động với kịch bản tối thiểu (chỉ CTO) rồi bổ sung đội ngũ theo tiến độ, hoặc duyệt đội đầy đủ để tăng tốc. Dù kịch bản nào, đối chiếu với **chi phí hao phí thực tế ~1,4–2,4 tỷ/năm** (Mục 4.1.1), khoản đầu tư này vẫn hoàn vốn nhanh.

3. **Phân công trách nhiệm:** **tuyển dụng CTO nội bộ** chịu trách nhiệm phát triển và vận hành kỹ thuật hệ thống mới, cam kết tài liệu hóa toàn bộ để hệ thống là tài sản của công ty; **COO đóng vai Product Owner** — xác nhận yêu cầu, phê duyệt thiết kế, đánh giá kết quả.

## 6.2 Điều kiện bắt đầu

- BGĐ phê duyệt chủ trương + ngân sách Giai đoạn 0+1.
- Xác nhận quyền truy cập kỹ thuật cần thiết (tài khoản quản trị SePay, ClassIn, Zalo OA hiện có).
- Thống nhất 2 chỉ số nghiệm thu Giai đoạn 1 (xem 6.4).

## 6.3 Timeline đề nghị

| Mốc | Thời điểm |
|---|---|
| BGĐ phê duyệt | **Trong tháng 7/2026** |
| Mở JD tuyển CTO + bắt đầu Giai đoạn 0 (dựng nền) | **Ngay sau phê duyệt — tháng 8/2026** |
| **Go-live Giai đoạn 1 (onboarding tự động)** | **Tháng 10–12/2026** |
| Quyết định tiếp Giai đoạn 2 | **Tháng 1/2027** (checkpoint 90 ngày) |

## 6.4 Checkpoint rà soát (BGĐ giữ quyền dừng/tiếp)

**Sau 30 ngày** — nghiệm thu nền tảng:
- Hạ tầng máy chủ + MISA SME Online + kênh Zalo đã sẵn sàng.
- Chuỗi onboarding tự động chạy thử thành công trên môi trường kiểm thử.

**Sau 90 ngày** — nghiệm thu giá trị (2 chỉ số quyết định):
- ✅ **Thời gian từ thanh toán → có SBD < 2 phút** (đo trên 95% học sinh).
- ✅ **Giảm ≥ 80% thời gian làm tay khâu onboarding** (từ ~14h/ngày xuống ~2–3h/ngày).

> Nếu đạt 2 chỉ số này, dự án đã **tự chứng minh** ROI và BGĐ phê duyệt tiếp Giai đoạn 2. Nếu không đạt, BGĐ dừng — tổng rủi ro tài chính tối đa là ngân sách GĐ 0+1 đã duyệt (~278–538 triệu kịch bản chỉ CTO, hoặc ~553–913 triệu kịch bản đội đầy đủ).

## 6.5 Bước tiếp theo ngay sau phê duyệt

1. **Mở JD tuyển dụng CTO** (senior, .NET + tích hợp API), COO tham gia phỏng vấn với vai trò Product Owner.
2. COO lập kế hoạch chi tiết Giai đoạn 0 (danh mục cấu hình máy chủ, hồ sơ đăng ký Zalo).
3. CTO onboard → thiết lập máy chủ + cài đặt nền tảng; **ưu tiên xử lý các lỗ hổng bảo mật B1–B3 trong tuần đầu (xem PHẦN 2B).**
4. Báo cáo tiến độ định kỳ 2 tuần/lần lên BGĐ trong suốt giai đoạn đầu.

---

> **Kết luận:** HSA Education đã đạt quy mô của một doanh nghiệp tầm trung — dẫn đầu phân khúc với **~21% SAM** trong một thị trường đang **tăng trưởng hai chữ số** (ĐGNL HCM +34,3% YoY, HCM HSA ×2 năm 2026 và ×1,5 năm 2027) — nhưng vẫn vận hành bằng bộ máy thủ công của một trung tâm nhỏ. Khoảng cách đó đang gây **chi phí hao phí thực tế ~1,4–2,4 tỷ/năm** (nhân công ~605 triệu + chi phí cơ hội lãnh đạo ~288 triệu + rò rỉ doanh thu onboarding ~500–1.500 triệu + lỗ hổng B0 đang chảy máu — xem Mục 4.1.1), **tích lũy rủi ro gián đoạn** (gồm cả các lỗ hổng bảo mật website tại PHẦN 2B và rủi ro chiến lược công nghệ tự phát P10), và quan trọng nhất là **chặn đứng tham vọng nâng thị phần và khả năng mở rộng sang thị trường mới như Đà Nẵng (giả định tiềm năng, cần nghiên cứu thực địa)**. Đề xuất này giải quyết tận gốc bằng cách **tuyển một CTO nội bộ (điều kiện tiên quyết — HSA hiện chưa có CTO; lương 50–100 triệu/tháng)** làm chủ kiến trúc và kỹ thuật lâu dài, hoàn vốn nhanh, và có cơ chế kiểm chứng từng bước để BGĐ luôn nắm quyền quyết định.
>
> **Đề nghị BGĐ phê duyệt chủ trương, tuyển dụng CTO (bắt đầu ngay sau phê duyệt T7/2026, onboard T8/2026), và ngân sách Giai đoạn 0+1 (~278–538 triệu kịch bản chỉ CTO, hoặc ~553–913 triệu kịch bản đội kỹ thuật đầy đủ).**

---

*— Hết tài liệu HSA-BC-v1.4 —*
