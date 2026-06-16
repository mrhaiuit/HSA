# HSA EDUCATION — BUSINESS CASE
## Đề xuất Chuyển đổi Hệ thống Vận hành: Xây dựng HSA Integration Platform

---

| Trường | Giá trị |
|---|---|
| **Mã tài liệu** | HSA-BC-v1.3 |
| **Phiên bản** | 1.3 |
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
| **1.3** | **2026-06-16** | **Bổ sung B0 (lỗ hổng mã KM đang gây thất thoát doanh thu — xác nhận thực tế); chi tiết hành trình 8 bước học sinh; lỗ hổng duyệt Zalo không đối chiếu tên; nhấn mạnh dữ liệu cá nhân phân tán không kiểm soát; sửa đơn giá HS từ 15 triệu → 2–3 triệu (đúng thực tế); bổ sung bảng suy giảm trải nghiệm học viên theo scale.** |

---

# PHẦN 0 — TÓM TẮT ĐIỀU HÀNH

> **Đề nghị BGĐ đọc trọn vẹn phần này. Nếu thời gian hạn chế, phần này đủ để ra quyết định.**

## Vấn đề cốt lõi (1 câu)

Toàn bộ vận hành của HSA Education đang chạy trên **7 công cụ rời rạc không kết nối với nhau**, buộc nhân sự phải làm tay khoảng **504 giờ công mỗi tháng (~63 ngày công)** cho các việc lặp đi lặp lại, đồng thời treo doanh nghiệp trên nhiều "điểm lỗi đơn" — chỉ một nhân sự nghỉ là cả chuỗi nhập học có thể tắc.

## Đề xuất (1 câu)

Xây dựng **HSA Integration Platform** — một lớp kết nối do **CTO nội bộ phát triển** — để **tự động hóa toàn bộ chuỗi từ lúc học sinh thanh toán đến lúc sẵn sàng vào lớp (rút từ ~15 phút thủ công xuống dưới 2 phút tự động, 24/7)**, kết hợp nền tảng quản trị **Odoo Community (miễn phí)** làm một nguồn dữ liệu duy nhất cho báo cáo và tài chính.

## Bức tranh tài chính

| Hạng mục | Con số |
|---|---|
| Chi phí hạ tầng tiền mặt năm 1 | ~106–121 triệu VND |
| Lương CTO nội bộ (năm 1) | ~50–100 triệu VND |
| **Tổng chi phí năm 1** | **~156–221 triệu VND** |
| Chi phí năm 2+ (hạ tầng) | ~101–111 triệu VND/năm |
| Lương CTO (năm 2+) | ~50–100 triệu VND/năm |
| **Tổng chi phí năm 2+** | **~151–211 triệu VND/năm** |
| Chi phí nếu thuê agency thay thế | **~500–800 triệu** (phát triển) + 100–160 triệu/năm bảo trì |
| **Tiết kiệm nhân công 2025** | **~595 triệu VND/năm** |
| **Tiết kiệm nhân công 2026** (HS tăng lên ~28.000) | **~712 triệu VND/năm** |
| **Tiết kiệm nhân công 2027** (HS tăng lên ~37.000) | **~937 triệu VND/năm** |
| **ROI ròng năm 1** | **~374–439 triệu VND** |
| **ROI ròng năm 2** | **~501–561 triệu VND** (tiết kiệm tăng theo scale) |
| **Thời gian hoàn vốn (Payback)** | **~3–4 tháng** |

## Vì sao phải làm NGAY (không chờ)

1. **Trải nghiệm học viên đang chịu thiệt:** học sinh đã trả tiền vẫn phải chờ **2–8 giờ** mới nhận được số báo danh và quyền vào lớp, tùy giờ hành chính.
2. **Rủi ro vận hành đang tích lũy:** chỉ **1 chuyên viên** duyệt học sinh vào lớp — người này nghỉ 1 ngày, toàn bộ học sinh đã thanh toán bị kẹt ngoài lớp.
3. **Không thể scale — và tải đang tăng mạnh:** cơ sở HCM có những đợt khai giảng **~260 học sinh/ngày** hiện tại; BGĐ đã xác nhận **HCM sẽ tăng gấp đôi năm 2026 và gấp 1,5 lần năm 2027** — đỉnh tải dự kiến lên **~520 HS/ngày (2026)** rồi **~780 HS/ngày (2027)**. Quy trình làm tay không gánh nổi tải này.
4. **BGĐ đang điều hành "mù":** chưa có bảng số liệu thời gian thực — mọi quyết định dựa trên báo cáo tổng hợp chậm.
5. **Thị trường đang tăng nhanh nhất trong lịch sử:** ĐGNL HCM tăng **+34,3% YoY (2025)**; HSA hiện đang chiếm **~21% SAM** (phân khúc học sinh có mua khóa ôn luyện) — đây là thời điểm chiến lược để **scale**, không phải giữ nguyên. Bỏ lỡ cửa sổ tăng trưởng này là nhường thị phần cho đối thủ số hóa nhanh hơn.
6. **Thị trường Đà Nẵng cần được tiếp cận:** Đà Nẵng có **~11.500 thí sinh THPT/năm** — thị trường thứ 3 HSA cần phục vụ. Với hệ thống tự động hóa, mở cơ sở mới chỉ cần "cắm vào là chạy"; không có hệ thống, mỗi lần mở rộng là khởi tạo lại toàn bộ việc tay từ đầu.

## Đề nghị cụ thể với BGĐ

1. **Phê duyệt chủ trương** xây dựng HSA Integration Platform theo lộ trình 4 giai đoạn / 18 tháng (T8/2026 – T3/2028).
2. **Phê duyệt ngân sách Giai đoạn 0 + 1 (T8–12/2026): ~49–80 triệu VND** (hạ tầng máy chủ + đăng ký kênh tin nhắn Zalo + lương CTO 5 tháng đầu).
3. **Phê duyệt tuyển dụng CTO nội bộ** chịu trách nhiệm phát triển và vận hành kỹ thuật; **COO đóng vai Product Owner** (xác nhận yêu cầu, phê duyệt thiết kế, đánh giá kết quả).
4. **Lịch rà soát:** checkpoint sau 30 ngày và sau 90 ngày, BGĐ quyết định có tiếp tục Giai đoạn 2 hay không dựa trên kết quả đo được.

> **Tóm lại:** Bỏ ra ~49–80 triệu cho giai đoạn đầu để kiểm chứng (đã gồm lương CTO), đổi lấy tiềm năng tiết kiệm gần 600 triệu/năm (và tăng lên ~712 triệu năm 2026, ~937 triệu năm 2027 theo scale) và loại bỏ các rủi ro có thể làm tắc nghẽn cả doanh nghiệp. Rủi ro tài chính nhỏ, lợi ích lớn, hoàn vốn trong ~3–4 tháng. Quan trọng hơn: đây là **điều kiện tiên quyết để giữ ~21% thị phần SAM và mở rộng sang Đà Nẵng** trong khi thị trường đang tăng trưởng hai chữ số.

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
| **Đà Nẵng** | ~11.500–12.000 thí sinh (ước tính) | Xếp hạng ~45 toàn quốc; tỉnh trực thuộc TW trung bình ~10.000–13.000 |

**Bối cảnh ngành EdTech:**

| Chỉ số | Giá trị | Dự báo |
|---|---|---|
| **EdTech Vietnam 2024** | **$1 tỷ USD** | $3 tỷ USD vào 2033 (**CAGR 12,96%**) |
| **Online Education Vietnam revenue 2025** | **~397 triệu USD** | 627 triệu USD năm 2029 (**CAGR 12,08%**) |

> **Đọc cho BGĐ:** HSA đang đặt hai cơ sở (HN + HCM) đúng tại hai địa phương đông thí sinh nhất cả nước — một lựa chọn chiến lược đúng. Nhưng **Đà Nẵng (~11.500 thí sinh/năm) hiện là mảng trắng** — HSA chưa có cơ sở. Ngành EdTech tăng trưởng ~13%/năm sẽ kéo theo nhiều đối thủ mới; cửa sổ giành thị phần đang mở nhưng sẽ không mở mãi.

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
| **Đà Nẵng — mảng trắng** | ~11.500 thí sinh/năm — HSA **hiện CHƯA CÓ cơ sở** → cơ hội chưa khai thác |
| **Nâng market share lên 10%** | 272.000 × 10% = **27.200 HS** → **tăng 36%** so với hiện tại (20.000) |
| **Đòn bẩy tự động hóa** | Với hệ thống tự động, **không phải tăng nhân công vận hành theo tỉ lệ tuyến tính** → biên lợi nhuận tăng khi scale |
| **TAM/SAM 2026–2027** | ĐGNL HCM +34,3%/năm kéo tổng thị trường kỳ thi lên **~330.000 (2026)** và **~400.000+ (2027)** → SAM tăng lên **~100.000–130.000 (2026)** và **~120.000–160.000 (2027)** học sinh có mua khóa |

> **Phép tính đơn giản cho BGĐ:** chỉ cần nâng thị phần từ ~7,4% lên 10% trên tổng thị trường là HSA tăng **36% số học sinh** mà không cần thị trường phải mở rộng thêm. Nhưng điều này **bất khả thi với quy trình thủ công** — vì thêm học sinh hiện đồng nghĩa với thêm người làm tay.

## 1B.4 Nhận xét chiến lược từ dữ liệu thị trường

> Bốn kết luận BGĐ cần ghi nhớ từ phần phân tích thị trường này:

1. **Thị trường đang TĂNG NHANH** — đặc biệt ĐGNL HCM **+34,3%**. Đây là **thời điểm chiến lược**: ai số hóa và scale nhanh hơn sẽ giành phần tăng trưởng; ai chậm sẽ mất thị phần ngay cả khi giữ nguyên số học sinh tuyệt đối.

2. **HSA đã chiếm ~21% SAM** — vị thế dẫn đầu. **Việc giữ vị thế và scale là ưu tiên số 1**, không phải phòng thủ thụ động.

3. **Nút thắt cổ chai vận hành thủ công = TRẦN TĂNG TRƯỞNG.** Không thể thêm học sinh nếu không thêm người. Đây là giới hạn vật lý của mô hình hiện tại — và nó chặn đứng mọi tham vọng nâng thị phần.

4. **Mở Đà Nẵng không thể thực hiện với hệ thống vận hành thủ công hiện tại.** Mỗi cơ sở mới = khởi tạo lại toàn bộ việc tay từ đầu, chi phí vận hành/HS cao, biên lợi nhuận thấp. Chỉ khi quy trình được chuẩn hóa và tự động hóa, mỗi cơ sở mới mới có thể "cắm vào là chạy".

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

## 2.6 Vấn đề nhóm Zalo — quả bom hẹn giờ

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

Zalo giới hạn số nhóm mà một tài khoản admin có thể quản lý, đồng thời mỗi nhóm tối đa ~1.000 thành viên. Ở quy mô hiện tại (1.800–2.100 nhóm) hệ thống đã căng; khi HSA tiến tới **30.000 học sinh** — mục tiêu thực tế nếu mở Đà Nẵng và nâng thị phần — số nhóm sẽ vọt lên **3.600+ nhóm**. Vấn đề nghiêm trọng hơn cả số lượng là **không có hệ thống theo dõi học sinh thuộc nhóm nào**: khi cần liên hệ một em cụ thể, không ai biết em ở nhóm nào, dẫn đến hiện tượng **"học sinh mất trong Zalo"** — đã thanh toán, đã vào học, nhưng không thể chăm sóc cá nhân hóa. Hệ quả: chất lượng dịch vụ tụt đúng lúc quy mô lớn nhất. **Giải pháp:** tích hợp **Zalo OA + CRM** để track membership tự động, mỗi học sinh được gắn đúng nhóm/lớp trong cơ sở dữ liệu trung tâm, tra cứu và liên hệ trong vài giây thay vì dò tay.

## P2 — ClassIn sẽ tăng giá tại kỳ gia hạn (dự báo: ≤24 tháng)

HSA đang phụ thuộc **ClassIn 100%** cho toàn bộ lớp học online (đang chuyển từ Zoom sang). Đây là dạng phụ thuộc nhà cung cấp (vendor lock-in) điển hình: HSA **không có dữ liệu học tập riêng** (điểm danh, điểm số, lịch sử học) được lưu độc lập, nên **không thể chuyển sang nền tảng khác** nếu ClassIn tăng giá — toàn bộ dữ liệu và quy trình đang nằm trong hệ sinh thái của họ. Khi HSA đạt **20.000+ học sinh/năm**, HSA trở thành khách hàng lớn — nhưng cũng là khách hàng **bị khóa chặt nhất**, mất hoàn toàn vị thế đàm phán giá. Một đợt tăng giá 20–30% ở kỳ gia hạn có thể ăn vào biên lợi nhuận đáng kể mà HSA gần như không có lựa chọn từ chối. **Giải pháp:** tích hợp **Data Subscription** của ClassIn để HSA **sở hữu bản sao dữ liệu học tập riêng** — vừa phục vụ chăm sóc chủ động, vừa tạo đòn bẩy đàm phán và lối thoát kỹ thuật nếu cần đổi nền tảng.

## P3 — Mạng lưới CTV vượt tầm kiểm soát thủ công (dự báo: 6–12 tháng)

Hiện 132–137 CTV được theo dõi bằng **Google Sheet + ref link thủ công**. Mô hình này đã căng và sẽ vỡ khi mạng lưới mở rộng. Khi lên **200+ CTV**, vấn đề **multi-touch attribution** sẽ bùng nổ: một học sinh thường tiếp xúc **2+ CTV** (qua quảng cáo, giới thiệu, tư vấn) trước khi quyết định mua — ai là người được ghi nhận hoa hồng? Với theo dõi tay, điều này dẫn đến **tranh chấp hoa hồng hàng loạt**, tốn thời gian xử lý và bào mòn lòng tin. Mất một CTV giỏi không chỉ mất một người — mà mất cả **kênh tuyển sinh** mà người đó đang nắm (mạng lưới phụ huynh, trường học, cộng đồng). Ở một doanh nghiệp mà CTV là nguồn tăng trưởng chính, đây là rủi ro trực tiếp lên doanh thu. **Giải pháp:** hệ thống cho **CTV tự xem hoa hồng realtime** + **quy tắc attribution rõ ràng, công khai** (first-touch/last-touch/chia tỉ lệ) được hệ thống áp dụng tự động, không tranh cãi.

## P4 — Quy định PDPA/dữ liệu cá nhân bắt đầu có hiệu lực (rủi ro pháp lý)

**Nghị định 13/2023/NĐ-CP** về bảo vệ dữ liệu cá nhân đã có hiệu lực và đang được siết dần trong khâu thực thi. HSA hiện lưu dữ liệu của **~20.000 học sinh/năm** (họ tên, ngày sinh, số điện thoại, thông tin phụ huynh, kết quả học tập) phân tán trong **Google Drive cá nhân** và nhiều công cụ **không có audit trail**. Đây là vi phạm tiềm tàng nhiều nguyên tắc cốt lõi của nghị định: không có cơ sở pháp lý xử lý dữ liệu rõ ràng, không kiểm soát truy cập, không nhật ký, không cơ chế thu hồi. Nguy cơ cụ thể: **bị thanh tra, bị phạt hành chính, hoặc bị phụ huynh khiếu kiện** khi xảy ra lộ/lọt thông tin (mà với dữ liệu nằm trên Drive cá nhân, rủi ro lộ lọt là rất thực). Khác với các rủi ro vận hành, rủi ro pháp lý có thể gây tổn hại uy tín nghiêm trọng và khó phục hồi. **Giải pháp:** **tập trung dữ liệu** về một kho có kiểm soát + **nhật ký truy cập** (ai xem/sửa gì, khi nào) + **chính sách xử lý dữ liệu** thành văn.

## P5 — Đà Nẵng expansion không thể thực hiện với hệ thống hiện tại (rủi ro chiến lược)

Đà Nẵng có **~11.500 thí sinh THPT/năm**, tương đương **~3.500–4.600 học sinh tiềm năng mua khóa học** (áp tỉ lệ 30–40%) — một thị trường đủ lớn để mở cơ sở thứ ba. Nhưng với hệ thống hiện tại, **mở văn phòng Đà Nẵng = khởi tạo lại toàn bộ quy trình thủ công từ đầu**: tuyển và đào tạo đội onboarding tay, dựng lại các Google Sheet, các nhóm Zalo, các quy trình duyệt — vì không có SOP chuẩn hóa (N13) để "nhân bản". Tệ hơn, **cost-to-operate mỗi học sinh ở Đà Nẵng = HN/HCM** (vì vẫn làm tay), nhưng **doanh thu nhỏ hơn** (thị trường nhỏ hơn) → **biên lợi nhuận thấp hơn**, có thể không đủ bù chi phí mở cơ sở. Kết quả: một cơ hội thị trường thực sự bị bỏ lỡ chỉ vì hạ tầng vận hành không sẵn sàng. **Giải pháp:** **chuẩn hóa và tự động hóa quy trình trước khi mở rộng** → mỗi cơ sở mới chỉ cần "cắm vào hệ thống là chạy", chi phí biên gần như bằng không.

## P6 — Đối thủ số hóa nhanh hơn sẽ vượt qua (rủi ro cạnh tranh)

**Vietnam EdTech 2024: $1 tỷ USD, tăng trưởng 12,96%/năm** — một thị trường đủ hấp dẫn để thu hút làn sóng đối thủ mới. Các đối thủ này thường **sinh ra từ công nghệ** (app học tập, AI tutor, mô hình online-first) và **không mang gánh nặng vận hành thủ công** như HSA — họ onboard học sinh trong vài phút, có dashboard, có dữ liệu học tập, scale gần như không giới hạn. HSA hiện có **lợi thế thương hiệu và chất lượng giảng dạy** — nhưng nếu trải nghiệm onboarding vẫn chậm (2–8 giờ) và dịch vụ kém cá nhân hóa, học sinh/phụ huynh sẽ **so sánh** và lợi thế đó bị bào mòn. **Dự báo:** trong **2–3 năm**, khoảng cách số hóa giữa HSA và các đối thủ nhanh sẽ **thu hẹp hoặc đảo chiều** nếu HSA không hành động ngay. Lợi thế thương hiệu mua được thời gian, nhưng không mua được vĩnh viễn. **Giải pháp:** dùng cửa sổ thời gian hiện tại — khi HSA vẫn dẫn đầu (~21% SAM) — để số hóa, biến lợi thế thương hiệu thành lợi thế công nghệ + thương hiệu kép.

## P7 — Tình trạng "học sinh ghost" ngày càng phổ biến (rủi ro giữ chân)

Hiện HSA **không có hệ thống theo dõi chuyên cần tự động** → không biết học sinh nào vắng nhiều buổi cho đến khi đã quá muộn (em đó đã bỏ học hẳn). Ở quy mô **20.000 học sinh**, chỉ cần tỉ lệ drop-out **5%** → **1.000 học sinh không hoàn thành khóa** → ảnh hưởng trực tiếp đến **kết quả thi** (điểm yếu tố quyết định uy tín của một trung tâm luyện thi) và **danh tiếng truyền miệng**. Quan trọng không kém: thiếu chăm sóc chủ động đồng nghĩa với **mất cơ hội up-sell** — chính những em đang gặp khó khăn (cần gia hạn, cần khóa nâng cao, cần phụ đạo) lại là nhóm khách hàng có nhu cầu chi thêm cao nhất, nhưng HSA không nhận diện được họ kịp thời. **Giải pháp:** tích hợp **ClassIn Data Subscription** → hệ thống **tự động alert khi học sinh vắng 3+ buổi** → Quản lý lớp (QLL) liên hệ kịp thời để giữ chân và mở cơ hội bán thêm.

## P8 — Google Sheet sẽ sập dưới tải dữ liệu lớn và nhiều người dùng đồng thời (rủi ro hạ tầng)

Google Sheet đang bị HSA dùng như một **database thực sự** (N8) — vai trò nó không được thiết kế để gánh. Google Sheet có **giới hạn cứng: ~10 triệu ô/file và ~200 người dùng đồng thời** — và HSA **đang tiếp cận các ngưỡng này**. Khi **~70 giảng viên + 132 CTV + đội HN + đội HCM** cùng truy cập các file lớn (danh sách học sinh, thù lao, hoa hồng), hệ quả điển hình là **chậm, treo, lỗi đồng bộ, thậm chí corruption (mất/hỏng dữ liệu)**. Đã có nhiều tiền lệ ở các doanh nghiệp Việt Nam tương tự: **Sheet bị "treo" đúng đợt cao điểm** — chính là lúc không được phép hỏng. Một sự cố mất dữ liệu thù lao/hoa hồng giữa đợt khai giảng có thể gây khủng hoảng niềm tin với cả giảng viên lẫn CTV. **Giải pháp:** chuyển sang **database thực sự** (Odoo + Integration DB) — được thiết kế cho hàng triệu bản ghi và hàng trăm người dùng đồng thời, có sao lưu và toàn vẹn dữ liệu.

> **Tổng kết PHẦN 2A:** 8 rủi ro ẩn này có chung một đặc điểm — **chúng đều có thể dự báo, và đều có cùng một lời giải gốc: chuyển từ vận hành thủ công phân tán sang một hệ thống tích hợp, có dữ liệu tập trung.** Hành động hôm nay (khi chúng còn là "dự báo") rẻ hơn rất nhiều so với xử lý khủng hoảng ngày mai.

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

## 3.1 Tầm nhìn hệ thống mới

> **HSA Integration Platform là "bộ não kết nối" đặt giữa 7 công cụ rời rạc hiện tại, để mọi việc từ lúc học sinh thanh toán đến lúc sẵn sàng vào lớp diễn ra tự động trong dưới 2 phút, 24/7 — và để toàn bộ dữ liệu vận hành dồn về MỘT nơi duy nhất phục vụ báo cáo.**

```
                    [ Học sinh thanh toán qua SePay ]
                                  │
                                  ▼  (tự động, không cần người)
        ╔══════════════════════════════════════════════════╗
        ║          HSA INTEGRATION PLATFORM                 ║
        ║      "Tự động tối đa — Người xử lý ngoại lệ"       ║
        ╚══════════════════════════════════════════════════╝
            │           │            │             │
            ▼           ▼            ▼             ▼
      Tạo SBD     Kích hoạt    Gửi tin nhắn   Ghi vào kho
      tự động     ClassIn      Zalo + email   dữ liệu Odoo
                                                    │
                                                    ▼
                                       [ Dashboard cho BGĐ ]
```

## 3.2 Nguyên tắc nền tảng: "Tự động tối đa — Con người xử lý ngoại lệ"

Đây là triết lý cốt lõi của đề xuất:

- **Việc đúng quy trình (95% trường hợp)** → máy tự làm hết, không cần người chạm tay.
- **Việc ngoại lệ (5%)** → hệ thống tự phát hiện, đẩy lên cho nhân viên xử lý đúng người, đúng lúc.

> Kết quả: nhân sự không còn "ngồi gõ" cho từng học sinh, mà chuyển sang **giám sát và xử lý các trường hợp đặc biệt** — công việc có giá trị cao hơn, ít sai sót hơn, không thể bị tắc vì một người nghỉ.

## 3.3 Các thành phần giải pháp

| Thành phần | Vai trò (giải thích cho BGĐ) | Chi phí |
|---|---|---|
| **HSA Integration Platform** | "Bộ não kết nối" do CTO nội bộ phát triển, nối SePay → tạo SBD → ClassIn → Zalo thành một chuỗi tự động | Lương CTO: 50–100 triệu/năm |
| **Odoo Community** | Phần mềm quản trị doanh nghiệp mã nguồn mở, làm "kho dữ liệu chung" và nơi xuất báo cáo/dashboard | **Miễn phí** |
| **Tích hợp SePay** | Tận dụng điểm tự động sẵn có để kích hoạt cả chuỗi onboarding | 0 |
| **Tích hợp ClassIn** | Tự động cấp quyền học sinh vào lớp + sau này lấy dữ liệu điểm danh/điểm số để chăm sóc chủ động | 0 (trong hợp đồng ClassIn hiện có) |
| **Zalo OA (tin nhắn tự động)** | Tự động gửi SBD, hướng dẫn, cảnh báo cho học sinh/phụ huynh | Phí tin nhắn ~16 triệu/năm (GĐ 1, chỉ gửi tin onboarding) — **scale theo số học sinh** (đầy đủ ~32–48 triệu/năm khi gửi nhiều loại tin cho ~20.000+ HS) |
| **Máy chủ (server)** | Nơi chạy hệ thống, có cả bản chính và bản dự phòng kiểm thử | ~50–60 triệu/năm |

## 3.4 Điểm khác biệt quan trọng: GIỮ EZSale — không gây gián đoạn đội Sale

> **Đây là điểm BGĐ cần yên tâm nhất.**

Một rủi ro lớn của các dự án chuyển đổi là **phá vỡ thói quen của đội bán hàng**, làm tụt doanh số trong giai đoạn chuyển tiếp. Đề xuất này **chủ động tránh điều đó:**

- **EZSale CRM được giữ nguyên** trong giai đoạn đầu. Đội Sale/CTV **không phải đổi cách làm việc.**
- Hệ thống mới chỉ **"đọc" dữ liệu** từ EZSale, **không can thiệp** vào quy trình tư vấn.
- Việc chuyển EZSale sang Odoo (nếu có) chỉ làm **về sau**, khi đội ngũ đã quen và sẵn sàng — không ép buộc.

> Triết lý: **tự động hóa phần "khô khan, lặp lại" trước (onboarding, đối soát, hoa hồng); để yên phần "con người" (tư vấn, chốt sale) cho đến khi an toàn.**

## 3.5 Bảng So sánh Trước / Sau

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
| **GĐ 0 — Dựng nền** | **Tháng 8–9/2026** | **Tuyển dụng CTO**, dựng hạ tầng máy chủ, đăng ký kênh tin nhắn Zalo, cài Odoo | CTO onboard + hệ thống nền sẵn sàng |
| **GĐ 1 — Onboarding tự động** | **Tháng 10–12/2026** | Tự động hóa chuỗi: thanh toán → SBD → ClassIn → Zalo | **Time-to-SBD < 2 phút**; bỏ ~13h/ngày việc tay |
| **Checkpoint GĐ 1 / Quyết định GĐ 2** | **Tháng 1/2027** | Nghiệm thu 2 chỉ số giá trị; BGĐ quyết định tiếp GĐ 2 | Quyết định go/no-go có dữ liệu |
| **GĐ 2 — Dữ liệu lớp + CTV** | **Tháng 2–4/2027** | Lấy dữ liệu điểm danh/điểm số từ ClassIn; tính hoa hồng CTV tự động | Chăm sóc chủ động + hoa hồng minh bạch |
| **GĐ 3 — Quản trị & Dashboard** | **Tháng 5–10/2027** | Đưa CRM, kế toán, báo cáo lên Odoo; dashboard cho BGĐ | **Đối soát SePay < 10 phút/ngày**; dashboard realtime |
| **GĐ 4 — Tối ưu & Scale** | **Tháng 11/2027–3/2028** | Tối ưu hiệu năng, chuẩn hóa cho mở rộng (sẵn sàng Đà Nẵng) | Sẵn sàng scale, vận hành ổn định |

> **Lưu ý quan trọng:** GĐ 0 nay bao gồm **tuyển dụng CTO** như một kết quả bàn giao bắt buộc — **không có CTO thì không thể xây dựng được gì.** Đây là lý do cần mở JD tuyển CTO ngay sau khi BGĐ phê duyệt.

## 5.2 Nguồn lực: Tuyển CTO nội bộ — lợi thế chiến lược

> **Đây là điểm khiến đề xuất này có ROI vượt trội so với mọi phương án thuê ngoài.**

Đề xuất tuyển dụng 01 CTO (Chief Technology Officer) nội bộ ở mức senior, chịu trách nhiệm toàn bộ thiết kế, phát triển và vận hành kỹ thuật HSA Integration Platform.

- **Mức lương:** 50–100 triệu VND/năm (theo kinh nghiệm và thỏa thuận).
- **So sánh:** thuê agency làm tương đương tốn 500–800 triệu, chưa kể bảo trì, không có cam kết bàn giao và knowledge transfer.
- **Lợi thế CTO nội bộ:** làm chủ kỹ thuật lâu dài, tài liệu hóa trong nhà, không phụ thuộc bên ngoài — giải quyết trực tiếp R1 (phụ thuộc 1 dev outsource).
- **COO đóng vai Product Owner:** xác nhận yêu cầu, phê duyệt thiết kế, đánh giá kết quả — đây là vai trò phù hợp với vị trí COO, không cần code.
- **Ưu tiên tuyển** người có kinh nghiệm .NET + tích hợp API, hiểu quy trình EdTech là lợi thế.

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
| Phần mềm Odoo Community | **Miễn phí** | Miễn phí |
| Máy chủ (chính + kiểm thử) | 50–60 triệu | 50–60 triệu |
| Đăng ký Zalo ZNS (một lần) | 5–10 triệu | — |
| Phí tin nhắn Zalo | ~16 triệu (GĐ 1) — scale lên ~32–48 triệu theo volume | ~16–48 triệu |
| Google Workspace (20 người) | ~35 triệu | ~35 triệu |
| Tích hợp ClassIn | 0 (trong hợp đồng) | 0 |
| **TỔNG HẠ TẦNG (tiền mặt)** | **~106–121 triệu** | **~101–111 triệu** |
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

| Chỉ tiêu | Giá trị |
|---|---|
| Tiết kiệm nhân công 2025 (baseline) | **+595 triệu VND/năm** |
| Tiết kiệm nhân công 2026 (HS ~28.000) | **+712 triệu VND/năm** |
| Tiết kiệm nhân công 2027 (HS ~37.000) | **+937 triệu VND/năm** |
| Tổng chi phí năm 1 (hạ tầng + CTO) | −156 đến −221 triệu VND |
| **ROI ròng năm 1** | **+374 đến +439 triệu VND** |
| Tổng chi phí năm 2+ (hạ tầng + CTO) | −151 đến −211 triệu VND/năm |
| **ROI ròng năm 2** (dùng tiết kiệm 2026 ~712 triệu) | **+501 đến +561 triệu VND** |
| **Thời gian hoàn vốn (Payback)** | **~3–4 tháng** |

> **Cách đọc đơn giản cho BGĐ:** ngay cả khi đã tính lương CTO, mỗi 1 đồng bỏ ra năm 1 vẫn thu về khoảng **2,7–3,8 đồng tiết kiệm.** Vốn được hoàn lại trong khoảng 3–4 tháng. Quan trọng hơn: tiết kiệm **tăng theo scale** (712 triệu năm 2026, 937 triệu năm 2027) trong khi chi phí gần như cố định → ROI năm 2+ còn mạnh hơn năm 1.

> **Lưu ý thận trọng (đúng tinh thần Principal PO):** con số tiết kiệm đầy đủ ~595 triệu chỉ đạt được khi **hoàn tất đến Giai đoạn 3.** Giai đoạn 1 (3 tháng đầu) đã mang lại phần lớn (~487 triệu từ onboarding). Vì vậy ngay cả kịch bản thận trọng nhất — chỉ làm xong Giai đoạn 1 — đề xuất vẫn **lãi đậm.**

> **Lưu ý về giá trị chiến lược (không nằm trong ROI tiền mặt):** ngoài ~595 triệu tiết kiệm, đề xuất còn **mở khóa khả năng nâng thị phần** (từ ~7,4% lên 10% TAM = +36% học sinh) và **mở khóa thị trường Đà Nẵng** (~3.500–4.600 HS tiềm năng). Những giá trị này lớn hơn nhiều con số tiết kiệm, nhưng được giữ ngoài tính toán ROI để bảo toàn tính thận trọng.

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

   | Hạng mục | Ngân sách đề nghị |
   |---|---|
   | Máy chủ (T8–12/2026, 5 tháng) | ~20–25 triệu |
   | Đăng ký kênh Zalo ZNS + mẫu tin | ~5–10 triệu |
   | Phí tin nhắn Zalo (bắt đầu T10) | ~3 triệu |
   | Lương CTO (5 tháng T8–12/2026) | ~21–42 triệu (5/12 × 50–100 triệu) |
   | **TỔNG ĐỀ NGHỊ DUYỆT (GĐ 0+1)** | **~49–80 triệu VND** |

   > Đây là toàn bộ tiền mặt cần để **kiểm chứng giá trị** của dự án qua giai đoạn mang ROI cao nhất (đã gồm lương CTO 5 tháng đầu).

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
- Hạ tầng máy chủ + Odoo + kênh Zalo đã sẵn sàng.
- Chuỗi onboarding tự động chạy thử thành công trên môi trường kiểm thử.

**Sau 90 ngày** — nghiệm thu giá trị (2 chỉ số quyết định):
- ✅ **Thời gian từ thanh toán → có SBD < 2 phút** (đo trên 95% học sinh).
- ✅ **Giảm ≥ 80% thời gian làm tay khâu onboarding** (từ ~14h/ngày xuống ~2–3h/ngày).

> Nếu đạt 2 chỉ số này, dự án đã **tự chứng minh** ROI và BGĐ phê duyệt tiếp Giai đoạn 2. Nếu không đạt, BGĐ dừng — tổng rủi ro tài chính tối đa chỉ ~49–80 triệu (đã gồm lương CTO 5 tháng đầu).

## 6.5 Bước tiếp theo ngay sau phê duyệt

1. **Mở JD tuyển dụng CTO** (senior, .NET + tích hợp API), COO tham gia phỏng vấn với vai trò Product Owner.
2. COO lập kế hoạch chi tiết Giai đoạn 0 (danh mục cấu hình máy chủ, hồ sơ đăng ký Zalo).
3. CTO onboard → thiết lập máy chủ + cài đặt nền tảng; **ưu tiên xử lý các lỗ hổng bảo mật B1–B3 trong tuần đầu (xem PHẦN 2B).**
4. Báo cáo tiến độ định kỳ 2 tuần/lần lên BGĐ trong suốt giai đoạn đầu.

---

> **Kết luận:** HSA Education đã đạt quy mô của một doanh nghiệp tầm trung — dẫn đầu phân khúc với **~21% SAM** trong một thị trường đang **tăng trưởng hai chữ số** (ĐGNL HCM +34,3% YoY, HCM HSA ×2 năm 2026 và ×1,5 năm 2027) — nhưng vẫn vận hành bằng bộ máy thủ công của một trung tâm nhỏ. Khoảng cách đó đang **tiêu tốn ~595 triệu/năm (và tăng lên ~712 triệu năm 2026, ~937 triệu năm 2027 theo scale)**, **tích lũy rủi ro gián đoạn** (gồm cả các lỗ hổng bảo mật website tại PHẦN 2B), và quan trọng nhất là **chặn đứng tham vọng nâng thị phần và mở Đà Nẵng**. Đề xuất này giải quyết tận gốc bằng cách **tuyển một CTO nội bộ (lương 50–100 triệu/năm)** làm chủ kỹ thuật lâu dài, hoàn vốn trong ~3–4 tháng, và có cơ chế kiểm chứng từng bước để BGĐ luôn nắm quyền quyết định.
>
> **Đề nghị BGĐ phê duyệt chủ trương, tuyển dụng CTO, và ngân sách ~49–80 triệu VND cho Giai đoạn 0+1.**

---

*— Hết tài liệu HSA-BC-v1.2 —*
