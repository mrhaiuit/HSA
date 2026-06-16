# Đánh Giá ClassIn API — Khả Năng Tích Hợp vào Pipeline HSA Education
## Tài liệu kỹ thuật dành cho COO & outsource developer

---

**Mục đích:** Đánh giá toàn bộ khả năng tích hợp ClassIn API vào pipeline vận hành HSA Education
**Nguồn:** Crawl toàn bộ tài liệu API tại https://classin.vn/huong-dan/api-docs-vi/
**Phiên bản API:** V1 (partner API) + V2 (LMS API)
**Ngày đánh giá:** Q2/2026

---

## I. TỔNG QUAN KỸ THUẬT

### 1.1 Kiến trúc API ClassIn

```
2 Base URL — 2 phiên bản API song song:

V1 (Partner API):
  https://api.eeo.cn/partner/api/course.api.php?action=[action]
  Auth: MD5(SECRET + timeStamp) → safeKey trong request body
  Timestamp valid: 20 phút

V2 (LMS API):
  https://api.eeo.cn/lms/[endpoint]
  Auth: Headers X-EEO-SIGN, X-EEO-UID, X-EEO-TS
  Timestamp valid: 5 phút (ketat hơn)

Lưu ý quan trọng:
  - Server đặt tại Trung Quốc (api.eeo.cn)
  - Cần đánh giá latency từ Việt Nam, đặc biệt khi trigger realtime
  - Mọi dữ liệu phải lưu lại tại database HSA (ClassIn không cung cấp query API đầy đủ)
```

### 1.2 Mô hình dữ liệu quan trọng cần nắm

```
Hệ thống định danh ClassIn:
  SID   = School ID (cố định cho HSA, lấy từ ClassIn management console)
  UID   = User ID (tạo ra khi đăng ký tài khoản → phải lưu lại)
  courseId = ID khóa học (tạo ra khi tạo course → phải lưu lại)
  classId  = ID buổi học (tạo ra khi tạo lesson → phải lưu lại)

HSA bắt buộc phải lưu trữ trong database:
  học_sinh.uid          ← từ Register API
  khoa_hoc.course_id    ← từ Create Course API hoặc nhập tay
  buoi_hoc.class_id     ← từ Create Class API hoặc nhập tay
  giang_vien.uid        ← từ Register API hoặc nhập tay

Không lưu → automation không thể hoạt động.
```

---

## II. KIỂM KÊ API THEO NHÓM

### 2.1 Account & User APIs

| API | Action | Dùng cho HSA | Ghi chú quan trọng |
|---|---|---|---|
| Register User | `action=register` | ✅ Tạo tài khoản học sinh sau thanh toán | Error 135 = SĐT đã đăng ký → trả về UID, dùng UID đó |
| Register Multiple | `action=registerMultiple` | ⚠️ Bulk create | Dùng khi migrate dữ liệu cũ |
| Add Student to School | `action=addSchoolStudent` | ✅ Bước 2 sau register | Bắt buộc trước khi add vào course |
| Add Teacher | `action=addTeacher` | ✅ Thêm GV mới | |
| Edit Student Info | `action=editStudent` | ⚠️ Cập nhật thông tin | |
| Edit Teacher Info | `action=editTeacher` | ⚠️ Cập nhật thông tin GV | |
| Disable/Enable Teacher | `action=disableTeacher` | ⚠️ Khi GV nghỉ hợp tác | |
| Evaluate Students | `action=evaluateStudents` | ❌ Nằm ngoài automation scope | GV làm thủ công |

### 2.2 Course & Lesson APIs

| API | Action / Endpoint | Dùng cho HSA | Ghi chú quan trọng |
|---|---|---|---|
| Create Course | `action=addCourse` | ✅ Khi mở khóa học mới | Thường làm thủ công, lưu courseId |
| Add Student to Course | `action=addCourseStudent` | ✅ Enroll học sinh tự động | identity=1 (student), cần courseId + UID |
| Remove Student | `action=removeCourseStudent` | ⚠️ Xử lý hoàn tiền | |
| Modify Course Teacher | `action=modifyCourseTeacher` | ⚠️ Giới hạn | **Đổi GV cho TẤT CẢ buổi chưa bắt đầu — không per-lesson** |
| Create Class (Lesson) | POST `/lms/activity/createClass` | ✅ Tạo buổi học + gán GV per-lesson | V2 API, cho phép gán GV riêng cho từng buổi |
| Edit Class | `action=editClass` | ⚠️ Khi thay đổi lịch | |
| Create School Tag | `action=createSchoolTag` | ⚠️ Phân loại lớp HN/HCM | Hữu ích cho filter |
| Add Group Learning | `action=addGroupCourse` | ❌ Chưa cần Phase 1-2 | |

### 2.3 Data Subscription APIs — QUAN TRỌNG NHẤT

**Mô hình hoạt động: PUSH (ClassIn đẩy data về HSA — không phải HSA kéo)**

```
ClassIn Server ──push──▶ HSA Webhook Endpoint (phải có server nhận)
                              │
                              ▼
                         HSA Database
                              │
                              ▼
                     Trigger Automation (Zalo OA, alert QLL)
```

**Cách setup:**
- HSA phải cung cấp 1 URL endpoint (server nhận data) cho ClassIn account manager
- ClassIn gửi test message để verify endpoint
- HSA endpoint phải trả về `{"error_info": {"errno": 1, "error": "..."}}`
- ClassIn tự động retry nếu HSA không phản hồi đúng format

**Dữ liệu có thể nhận qua webhook:**

| Loại data | Thời điểm nhận | Dữ liệu chính | Ứng dụng HSA |
|---|---|---|---|
| **Điểm danh** (After Class) | Trong vòng 20 phút sau buổi học | Entry/exit timestamps, số HS thực tế vs. enrolled, thiết bị | Trigger: vắng buổi học, vắng 3 ngày liên tiếp |
| **Đánh giá sau lớp** (After Class) | Trong vòng 20 phút | GV đánh giá HS, HS đánh giá GV | Dashboard GV |
| **Điểm bài tập LMS** (LMS) | Realtime khi submit/chấm | studentUid, courseId, điểm, tỷ lệ hoàn thành | Trigger: điểm thấp → gợi ý tài liệu |
| **Điểm bài kiểm tra LMS** | Realtime | Tương tự bài tập | Trigger: điểm thấp |
| **Sự kiện trong lớp** (During Class) | Realtime | Entry/exit, hand raise, rewards | Có thể dùng để detect login activity |
| **Thay đổi tài khoản** (School) | Realtime | UID, SĐT mới | Sync nếu HS đổi SĐT |
| **Hủy tài khoản** (School) | Realtime | UID, AccountStatus=255 | Xử lý học sinh hủy tài khoản |

### 2.4 Cloud Drive APIs

| API | Dùng cho HSA | Ghi chú |
|---|---|---|
| Upload File | ✅ Upload tài liệu giảng dạy | Tự động hóa upload tài liệu cho GV |
| Get Folder List | ✅ Kiểm tra cấu trúc thư mục | |
| Create/Delete Folder | ✅ Tạo folder theo lớp/khóa | |

### 2.5 Invoke Link — Tính năng đặc biệt hữu ích

```
URL format:
classin://www.eeo.cn/enterclass?telephone=[SĐT]&classId=[ID]&courseId=[ID]&schoolId=[SID]

Hoặc qua intermediate page (để tự động download nếu chưa cài):
https://www.eeo.cn/client/invoke/index.html?telephone=...&classId=...

Ứng dụng: Nhúng link này vào email hướng dẫn → học sinh click 1 lần → vào thẳng lớp
```

---

## III. ĐÁNH GIÁ KHẢ NĂNG TÍCH HỢP THEO TỪNG TÍNH NĂNG TRONG PIPELINE

### 3.1 Auto-onboarding sau thanh toán (Phase 1)

**Luồng kỹ thuật đầy đủ:**

```
[SePay Webhook: payment_success]
         │
         ▼
[HSA Backend]
  Lấy: họ tên, SĐT, email, khóa_học từ đơn hàng
         │
         ▼ (Bước 1)
[ClassIn API: action=register]
  POST https://api.eeo.cn/partner/api/course.api.php?action=register
  Body: SID, safeKey, timeStamp, telephone=0084-[SĐT], password=[random], nickname=[tên]
  Response: {"data": [UID], "error_info": {"errno": 1}}
  Xử lý: if errno=135 (đã có) → dùng UID trả về
  Lưu: học_sinh.classin_uid = UID
         │
         ▼ (Bước 2)
[ClassIn API: action=addSchoolStudent]
  Body: SID, safeKey, timeStamp, studentAccount=[SĐT], studentName=[tên]
  Lưu: trạng thái = "added_to_school"
         │
         ▼ (Bước 3)
[Lookup: khoa_hoc → course_id]
  Tra bảng mapping: mã_khoa_hoc → classin_course_id
  ⚠️ Bảng này phải tồn tại TRƯỚC — xem mục 3.6
         │
         ▼ (Bước 4)
[ClassIn API: action=addCourseStudent]
  Body: SID, safeKey, timeStamp, courseId=[ID], studentUid=[UID], studentAccount=[SĐT], identity=1
  Lưu: trạng thái = "enrolled"
         │
         ▼ (Bước 5 — song song với các bước trên)
[Gửi email hướng dẫn]
  Bao gồm: thông tin đăng nhập ClassIn, invoke link vào lớp
  Invoke link: classin://www.eeo.cn/enterclass?telephone=[SĐT]&courseId=[ID]&schoolId=[SID]
```

**Đánh giá:** ✅ HOÀN TOÀN KHẢ THI — Phức tạp thấp đến trung bình. Outsource dev estimate: 1–2 tuần.

### 3.2 Gán giảng viên tự động (Phase 1)

**Có 2 cách, khác nhau về mức độ linh hoạt:**

**Cách A — `modifyCourseTeacher` (V1 API):**
```
Endpoint: action=modifyCourseTeacher
Tham số: courseId + teacherUid
Kết quả: ĐỔI GV CHO TẤT CẢ BUỔI CHƯA BẮT ĐẦU trong course
⚠️ Hạn chế nghiêm trọng: Nếu 1 khóa học có nhiều GV dạy các buổi khác nhau → không dùng được cách này
```

**Cách B — Create Class với `teacherUid` per lesson (V2 LMS API):**
```
Endpoint: POST https://api.eeo.cn/lms/activity/createClass
Tham số: courseId, name, teacherUid (per lesson!), startTime, endTime, assistantUids[]
Kết quả: Tạo từng buổi học với GV riêng → linh hoạt hoàn toàn
✅ Đây là cách đúng cho HSA nếu mỗi khóa có nhiều GV
⚠️ Phức tạp hơn: phải tạo lịch học trước, biết trước lịch từng buổi
```

**Khuyến nghị cho HSA:**
- Nếu 1 khóa học chỉ có 1 GV xuyên suốt → Cách A đơn giản hơn
- Nếu nhiều GV luân phiên → Cách B, tức là cần tạo schedule từng buổi qua API

### 3.3 ClassIn Data Pipeline — Điểm danh & Điểm số (Phase 2)

**Luồng kỹ thuật:**

```
[Sau mỗi buổi học kết thúc — max 20 phút]
ClassIn Server ──push──▶ HSA Webhook Endpoint
                              │
                     Nhận JSON điểm danh:
                     {
                       "CourseID": ...,
                       "ClassID": ...,
                       "StartTime": ...,
                       "CloseTime": ...,
                       "attendance": [
                         {"UID": ..., "entryTime": ..., "exitTime": ...},
                         ...
                       ],
                       "actualCount": 28,
                       "enrolledCount": 30
                     }
                              │
                              ▼
                     Ghi vào HSA Database: bảng attendance
                              │
                    ┌─────────┴─────────┐
                    ▼                   ▼
         Kiểm tra học sinh         Tính tỷ lệ
         vắng buổi nào?            tham dự tháng
                    │
                    ▼
         Học sinh vắng ≥ 2 buổi liên tiếp?
                    │
                   Có → Alert QLL (push notification / Slack / dashboard flag)
                    │
         Học sinh không login 3 ngày liên tiếp?
                    │
                   Có → Trigger Zalo OA hỏi thăm
```

**Đánh giá:** ✅ KHẢ THI — nhưng cần setup Data Subscription với ClassIn account manager trước.
Mức độ phức tạp: Trung bình. Outsource dev estimate: 2–3 tuần.

**Điểm danh "3 ngày không login" — cách tính:**
```
Logic cần implement tại HSA:
  Mỗi ngày có lớp học → sau lớp nhận webhook attendance
  Nếu HS không trong danh sách attendance → vắng ngày đó
  Count(ngày vắng liên tiếp) >= 3 → trigger Zalo OA
  Reset counter khi HS có mặt 1 buổi bất kỳ
```

### 3.4 Trigger Điểm Thấp — LMS Score Webhook (Phase 2)

**Dữ liệu nhận được (realtime):**
```json
{
  "SID": ...,
  "CourseID": ...,
  "studentUid": ...,
  "activityName": "Bài tập chương 3",
  "score": 4.5,
  "totalScore": 10,
  "completionRate": 0.45,
  "submissionTime": ...,
  "correctionTime": ...
}
```

**Logic trigger:**
```
if completionRate < 0.5 (dưới 50%)
  → Zalo OA: gợi ý tài liệu bổ trợ theo môn học của courseId đó
  → Log vào HSA database: học sinh X cần hỗ trợ môn Y
  → Alert QLL nếu điểm thấp lần 2 liên tiếp
```

**Đánh giá:** ✅ KHẢ THI — Dữ liệu webhook đầy đủ. Cần mapping: courseId → môn học → tài liệu bổ trợ tương ứng. Outsource dev estimate: 1–2 tuần sau khi có webhook.

### 3.5 Dashboard 3 tầng từ ClassIn Data (Phase 2)

```
ClassIn webhook → HSA Database (bảng: attendance, scores, class_log)
                          │
                          ▼
               Google Looker Studio / Google Sheet
                          │
              ┌───────────┼───────────┐
              ▼           ▼           ▼
          QLL View    COO View    GV View
          (per lớp)  (HN + HCM)  (per GV)
```

**Dữ liệu có thể hiển thị trong dashboard từ ClassIn:**

| Chỉ số | Nguồn ClassIn | Cập nhật |
|---|---|---|
| Tỷ lệ tham dự buổi học | After-class attendance webhook | Trong 20 phút sau buổi học |
| Điểm bài tập trung bình | LMS score webhook | Realtime |
| Học sinh vắng hôm nay | After-class webhook | Trong 20 phút |
| Học sinh không login 3 ngày | Tính từ attendance history | Hàng ngày |
| Số buổi học đã diễn ra | ClassID logs | Theo buổi |
| Tỷ lệ hoàn thành khóa | LMS completion rate | Realtime |

---

## IV. NHỮNG GÌ CLASSIN API KHÔNG LÀM ĐƯỢC (HẠN CHẾ)

| Hạn chế | Mô tả | Ảnh hưởng | Giải pháp |
|---|---|---|---|
| Không có "Pull attendance API" | Chỉ push webhook — không thể query lịch sử | Nếu HSA miss webhook → mất data | Phải có retry mechanism, log mọi webhook nhận được |
| Server tại Trung Quốc | api.eeo.cn — latency từ VN có thể 200-500ms | Ảnh hưởng tốc độ auto-enroll | Không critical nếu không realtime cứng (5 phút là chấp nhận được) |
| Không có SSO thật sự | Invoke link cần app cài sẵn | Học sinh phải cài app trước | Dùng intermediate page link, email hướng dẫn cài app |
| Data Subscription phải setup thủ công | Không thể bật qua API — phải liên hệ account manager ClassIn | Delay triển khai | Làm sớm, xong trước Phase 1 |
| Không có "query học sinh" API đầy đủ | Không thể lấy danh sách học sinh đang học từ ClassIn | Dashboard phải dùng data HSA, không phải ClassIn | Mọi data phải lưu tại HSA khi tạo |
| `modifyCourseTeacher` thay đổi toàn bộ | Không per-lesson | Nếu nhiều GV trong 1 khóa → phải dùng V2 Create Class | Thay đổi cách tạo lịch học |
| Không có API kiểm tra trạng thái học sinh | Không query "HS này đã login chưa?" | Phải tính từ attendance webhook | Logic phải cài tại HSA side |
| Rate limit không được document | Không rõ bao nhiêu request/phút | Có thể bị throttle khi bulk enroll | Test kỹ trước khi production |

---

## V. ĐIỀU KIỆN TIÊN QUYẾT TRƯỚC KHI BẮT ĐẦU TÍCH HỢP

### 5.1 Từ phía ClassIn (làm ngay, không tốn code)

- [ ] **Lấy SID và SECRET** từ ClassIn management console (admin account)
- [ ] **Đăng ký Data Subscription** với ClassIn account manager VN (028 7105 9900)
  - Cung cấp: URL webhook endpoint của HSA, SID, email nhận thông báo lỗi
  - Xác nhận: ClassIn gửi test message → HSA phản hồi đúng format
- [ ] **Lấy courseId cho tất cả khóa học hiện có** — export từ ClassIn console hoặc yêu cầu ClassIn cung cấp
- [ ] **Lấy UID của tất cả GV hiện có** — tương tự
- [ ] Xác nhận với ClassIn VN: API có rate limit không? Bao nhiêu request/phút?

### 5.2 Từ phía HSA (cần outsource dev implement)

- [ ] **Bảng mapping trong database HSA:**

```sql
-- Bảng bắt buộc phải có trước khi bật automation

TABLE khoa_hoc_mapping (
  hsa_course_code    VARCHAR(50),   -- mã khóa học bên HSA
  classin_course_id  BIGINT,        -- courseId trong ClassIn
  classin_gv_uid     BIGINT,        -- UID giảng viên phụ trách
  market             VARCHAR(10),   -- 'HN' hoặc 'HCM'
  updated_at         TIMESTAMP
)

TABLE hoc_sinh (
  hsa_student_id     VARCHAR(50),
  classin_uid        BIGINT,        -- UID từ ClassIn register API
  phone              VARCHAR(20),
  email              VARCHAR(100),
  classin_enrolled   BOOLEAN,
  enrolled_at        TIMESTAMP
)

TABLE attendance_log (
  classin_class_id   BIGINT,
  classin_uid        BIGINT,
  class_date         DATE,
  entry_time         TIMESTAMP,
  exit_time          TIMESTAMP,
  is_present         BOOLEAN
)

TABLE lms_score_log (
  classin_uid        BIGINT,
  course_id          BIGINT,
  activity_name      VARCHAR(200),
  score              DECIMAL(5,2),
  total_score        DECIMAL(5,2),
  completion_rate    DECIMAL(4,2),
  submitted_at       TIMESTAMP
)
```

- [ ] **Webhook endpoint server** tại HSA để nhận ClassIn push data
- [ ] **Retry/fallback logic** khi ClassIn API timeout

### 5.3 Quy trình vận hành khi mở khóa học mới (SOP bắt buộc)

```
Mỗi khi mở khóa học mới → làm đúng thứ tự:

1. Admin tạo Course trong ClassIn console
   → Lấy courseId từ URL hoặc response

2. Admin thêm GV vào ClassIn (nếu GV mới)
   → Lấy GV UID

3. Admin cập nhật bảng khoa_hoc_mapping:
   INSERT (hsa_course_code, classin_course_id, classin_gv_uid, market)

4. Kiểm tra: thử enroll 1 học sinh test → verify đúng lớp
5. Bật automation cho khóa học này
```

**Nếu bỏ qua bước này → học sinh thanh toán nhưng không được enroll → sự cố nghiêm trọng.**

---

## VI. ƯỚC TÍNH THỜI GIAN TRIỂN KHAI

| Hạng mục | Độ phức tạp | Outsource dev estimate | Phụ thuộc |
|---|---|---|---|
| Register + Enroll tự động sau thanh toán | Thấp-Trung bình | 1–2 tuần | Bảng mapping phải có sẵn |
| Gửi invoke link trong email hướng dẫn | Thấp | 1–2 ngày | courseId có trong mapping |
| Setup Data Subscription webhook endpoint | Trung bình | 1 tuần | ClassIn account manager đã setup |
| Attendance → Database → "Vắng 3 ngày" trigger | Trung bình | 2–3 tuần | Webhook endpoint hoạt động |
| LMS Score → Trigger Zalo OA | Trung bình | 1–2 tuần | Webhook + mapping môn → tài liệu |
| Dashboard 3 tầng (Looker Studio) | Trung bình | 2–3 tuần | Database đã có đủ data |
| Per-lesson teacher assignment (V2 LMS API) | Cao | 2–4 tuần | Cần lịch học cụ thể từ đầu |

**Tổng Phase 1 (auto-enroll):** 2–3 tuần
**Tổng Phase 2 (data pipeline + triggers):** 4–6 tuần sau Phase 1
**Tổng Phase 3 (dashboard đầy đủ):** 2–3 tuần sau Phase 2

---

## VII. MA TRẬN RỦI RO TÍCH HỢP API

| Rủi ro | Xác suất | Tác động | Biện pháp |
|---|---|---|---|
| Bảng mapping không được cập nhật khi mở khóa mới | **Cao** | Cao | SOP bắt buộc, thêm alert nếu courseId không tồn tại |
| Data Subscription chưa được setup → bắt đầu Phase 2 sớm | **Cao** | Cao | Làm ngay với ClassIn account manager, không chờ |
| Webhook miss do server HSA downtime | Trung bình | Cao | Implement retry handler, alert khi webhook không nhận 30+ phút |
| Rate limit API khi bulk enroll nhiều học sinh cùng lúc | Trung bình | Trung bình | Queue-based enrollment, không gọi song song |
| Latency api.eeo.cn từ VN | Thấp-Trung bình | Thấp | Timeout 10s, retry 3 lần, async flow |
| Học sinh đổi SĐT → UID không khớp | Thấp | Cao | Webhook account change → update HSA database |
| GV UID không đúng → gán GV sai | Thấp | Cao | Validate GV UID trước khi lưu mapping |

---

## VIII. KHUYẾN NGHỊ TRIỂN KHAI

### 8.1 Thứ tự làm đúng

```
TUẦN 1–2: Chuẩn bị (không code)
├── Lấy SID + SECRET từ ClassIn
├── Export courseId + GV UID hiện có
├── Liên hệ ClassIn account manager đăng ký Data Subscription
├── Tạo bảng mapping trong database
└── Outsource dev đọc tài liệu API, dựng môi trường test

TUẦN 3–4: Phase 1 — Auto-enroll
├── Implement Register API
├── Implement addSchoolStudent API
├── Implement addCourseStudent API
├── Test toàn bộ flow với tài khoản test
├── Tích hợp invoke link vào email hướng dẫn
└── Go-live với 1 khóa học → monitor 1 tuần trước khi mở rộng

TUẦN 5–6: Setup Data Pipeline
├── Dựng webhook endpoint server
├── Xác nhận ClassIn đã setup Data Subscription
├── Test nhận webhook (attendance, LMS scores)
├── Ghi vào database
└── Verify data accuracy

TUẦN 7–10: Phase 2 — Triggers và Dashboard
├── Logic "vắng 3 ngày" + trigger Zalo OA
├── Logic "điểm thấp" + trigger Zalo OA
├── Alert QLL khi có sự kiện
├── Looker Studio dashboard QLL view
├── Looker Studio dashboard COO view
└── Looker Studio dashboard GV view
```

### 8.2 Yêu cầu đối với outsource developer

1. **Biết:** REST API, JSON, MD5 hashing, webhook receiver
2. **Cần làm:** Đọc kỹ tài liệu API ClassIn trước khi estimate
3. **Bắt buộc:** Viết documentation cho mọi integration đã build
4. **Bắt buộc:** Log toàn bộ API call (request + response + timestamp)
5. **Bắt buộc:** Implement retry logic cho mọi API call quan trọng
6. **Test environment:** Yêu cầu ClassIn cung cấp sandbox/test SID

### 8.3 Câu hỏi cần hỏi ClassIn VN trước khi bắt đầu

1. Rate limit của Partner API là bao nhiêu request/phút?
2. Có sandbox/test environment không?
3. Data Subscription mất bao lâu để setup sau khi đăng ký?
4. Khi ClassIn server bảo trì → webhook có retry không? Retry trong bao lâu?
5. courseId và classId có thể thay đổi không? (quan trọng cho data integrity)
6. API có phiên bản nâng cấp nào sắp ra không?

---

## IX. KẾT LUẬN

**Verdict tổng thể: ClassIn API đủ khả năng phục vụ pipeline HSA Education trong cả 3 Phase.**

| Tính năng | Khả thi? | Độ phức tạp | Phase |
|---|---|---|---|
| Auto-create tài khoản ClassIn sau thanh toán | ✅ Có | Thấp | 1 |
| Auto-enroll đúng lớp theo khóa mua | ✅ Có | Thấp-Trung bình | 1 |
| Gán GV cố định theo course | ✅ Có | Thấp | 1 |
| Gán GV riêng per buổi học | ✅ Có (V2 API) | Cao | 2 |
| Invoke link vào lớp 1 click trong email | ✅ Có | Thấp | 1 |
| Nhận điểm danh sau buổi học (webhook) | ✅ Có | Trung bình | 2 |
| Trigger "vắng 3 ngày" → Zalo OA | ✅ Có | Trung bình | 2 |
| Nhận điểm bài tập realtime (webhook) | ✅ Có | Trung bình | 2 |
| Trigger "điểm thấp" → gợi ý tài liệu | ✅ Có | Trung bình | 2 |
| Dashboard học tập 3 tầng | ✅ Có | Trung bình | 2–3 |
| Query lịch sử học sinh (pull) | ❌ Không | — | Phải dùng data HSA tự lưu |
| SSO đăng nhập tự động (không cần app) | ❌ Không | — | Học sinh phải cài app |

**Điều kiện thành công duy nhất:**
Bảng mapping `khoa_hoc → courseId → GV` phải được maintain đúng mỗi khi mở khóa học mới. Đây là điểm thất bại duy nhất không liên quan đến kỹ thuật — liên quan đến kỷ luật vận hành.

---

*Tài liệu này dựa trên crawl toàn bộ https://classin.vn/huong-dan/api-docs-vi/ — Q2/2026*
*Cần verify lại với ClassIn account manager VN trước khi triển khai production*
