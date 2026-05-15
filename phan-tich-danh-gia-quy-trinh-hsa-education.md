# Phân tích và đánh giá quy trình vận hành HSA Education

## 1. Mục tiêu tài liệu

Tài liệu này tổng hợp, phân tích và đánh giá quy trình vận hành hiện tại của HSA Education dựa trên luồng nghiệp vụ từ Marketing, CRM/Sale, thanh toán, onboarding, học tập, chăm sóc học viên đến upsell/cross-sell/referral.

Mục tiêu chính:

- Nhận diện các điểm mạnh của quy trình hiện tại.
- Chỉ ra các điểm nghẽn, điểm thủ công và rủi ro vận hành.
- Đánh giá mức độ sẵn sàng để tự động hóa theo từng giai đoạn.
- Đề xuất thứ tự ưu tiên nâng cấp để giảm tải cho Sale, QLL và đội vận hành.
- Xác lập các chỉ số cần theo dõi để đánh giá hiệu quả cải tiến.

## 2. Tóm tắt điều hành

Quy trình hiện tại của HSA Education đã có nền tảng vận hành khá đầy đủ, bao gồm phát sinh lead từ quảng cáo, thu lead qua landing page, quản lý cơ hội bán hàng bằng EZSale, thanh toán qua web, xác nhận thanh toán bằng SePay webhook, tổ chức lớp qua Zalo và Zoom, chăm sóc học viên trong nhóm lớp, cũng như khai thác học viên cũ thông qua chương trình Đại sứ và upsell/cross-sell.

Điểm mạnh lớn nhất là HSA Education đã hình thành được một hành trình khách hàng từ đầu đến cuối. Đặc biệt, khâu thanh toán đã có tự động hóa bằng SePay webhook, giúp giảm đáng kể việc đối soát thủ công.

Tuy nhiên, các khâu trước và sau thanh toán vẫn còn phụ thuộc nhiều vào con người. Nhập lead vào CRM, nhắn tin nhắc lead, tạo nhóm Zalo, thêm học sinh vào lớp, gửi link Zoom, điểm danh, nhắc học sinh vắng, chăm sóc học viên và khai thác lại lead cũ đều có nhiều thao tác thủ công. Khi quy mô lớp và số lượng lead tăng, các điểm nghẽn này có thể gây quá tải cho Sale và QLL, làm giảm trải nghiệm học viên và tăng rủi ro thất thoát doanh thu.

Hướng nâng cấp được nêu trong quy trình là phù hợp: tự động hóa onboarding, kết nối landing page với CRM, sử dụng Zalo OA để nurture và chăm sóc, chuyển dần sang ClassIn để chuẩn hóa lớp học, xây dashboard realtime, và khai thác dữ liệu học tập để tạo trigger chăm sóc/upsell. Tuy nhiên, cần triển khai theo thứ tự ưu tiên rõ ràng, tránh làm đồng thời quá nhiều hạng mục khi chưa có nền tảng dữ liệu và owner vận hành.

## 3. Mô tả quy trình hiện tại

### 3.1 Marketing

Luồng hiện tại:

- Chạy quảng cáo trên Facebook, TikTok, Google.
- Đưa người dùng về landing page.
- Thu thông tin qua form đăng ký hoặc form tư vấn.
- Sale nhập lead vào EZSale CRM theo cách thủ công.

Nhận xét:

Marketing đã có kênh tạo nhu cầu và landing page để thu lead, nhưng điểm yếu nằm ở việc dữ liệu lead chưa được đẩy tự động vào CRM. Điều này tạo ra nguy cơ chậm phản hồi, nhập sai thông tin, trùng lead hoặc bỏ sót lead.

### 3.2 CRM và Sale

Luồng hiện tại:

- EZSale được dùng để theo dõi lead.
- Sale quản lý trạng thái Hot, Warm, Cold.
- Sale gọi điện, nhắn tin và chốt sale trực tiếp.
- Một phần nhắc tin, gọi lại và chăm sóc Warm/Cold lead vẫn thực hiện thủ công.
- Khi chốt thành công, Sale gửi link giỏ hàng qua Zalo hoặc tin nhắn trực tiếp.

Nhận xét:

CRM đã được đưa vào quy trình, đây là nền tảng tốt. Tuy nhiên, việc chăm sóc lead vẫn dựa nhiều vào kỷ luật cá nhân của Sale. Nếu không có automation và SLA rõ ràng, nhóm Warm/Cold rất dễ bị rơi khỏi phễu bán hàng.

### 3.3 Thanh toán

Luồng hiện tại:

- Học sinh/phụ huynh vào web đặt hàng.
- Thanh toán qua chuyển khoản hoặc cổng thanh toán.
- SePay webhook xác nhận thanh toán tự động.
- Nếu thanh toán không thành công, Sale nhắc thủ công.

Nhận xét:

Đây là phần trưởng thành nhất của quy trình. SePay webhook giúp giảm rủi ro đối soát tay và tạo điều kiện để kích hoạt các bước sau thanh toán. Tuy nhiên, các hành động sau khi thanh toán thành công vẫn chưa được tự động hóa đầy đủ.

### 3.4 Onboarding học sinh

Luồng hiện tại:

- Sale tạo nhóm Zalo với QLL để báo học sinh đã thanh toán.
- QLL kiểm tra thông tin và duyệt học sinh.
- QLL thêm học sinh vào nhóm Zalo lớp.
- QLL gửi hướng dẫn onboard, link, số báo danh và tài liệu.
- Học sinh tự làm theo hướng dẫn và hỏi lại nếu chưa hiểu.
- QLL hỗ trợ từng trường hợp trong nhóm Zalo.

Nhận xét:

Onboarding là một trong những điểm nghẽn lớn nhất. Dù thanh toán đã tự động, quá trình đưa học sinh vào lớp vẫn phụ thuộc vào Sale và QLL. Nếu số lượng học sinh tăng đột biến, khả năng chậm onboard, thiếu thông tin, gửi sai lớp hoặc bỏ sót học sinh sẽ tăng cao.

### 3.5 Học tập

Luồng hiện tại:

- QLL gửi link Zoom hằng ngày trong nhóm Zalo lớp.
- Giáo viên dạy trên Zoom.
- Tài liệu gửi qua Zalo hoặc web.
- Điểm danh thủ công.
- Hỏi đáp diễn ra trong nhóm Zalo.
- QLL gửi lại link khi học sinh bị miss hoặc hỏi lại.

Nhận xét:

Zoom và Zalo dễ triển khai nhanh, quen thuộc với học sinh và phụ huynh. Tuy nhiên, đây không phải mô hình tối ưu khi quy mô lớn. Các tác vụ lặp lại như gửi link, điểm danh, tổng hợp học sinh vắng, quản lý tài liệu và theo dõi hành vi học tập cần được chuyển sang nền tảng có dữ liệu cấu trúc hơn.

### 3.6 Chăm sóc học viên

Luồng hiện tại:

- Học viên hỏi đáp trong nhóm Zalo lớp.
- Giáo viên và QLL phản hồi trực tiếp.
- QLL nhắc từng học sinh nếu vắng.
- Chưa có khảo sát định kỳ có hệ thống.

Nhận xét:

Chăm sóc hiện tại có tính gần gũi, nhưng khó đo lường và khó mở rộng. Phản hồi nằm rải rác trong Zalo, không được chuẩn hóa thành ticket, không có mức độ ưu tiên, không có SLA và không tạo được dữ liệu phân tích dài hạn.

### 3.7 Upsell, cross-sell và referral

Luồng hiện tại:

- Sale gợi ý khóa mới khi gặp dịp.
- Có chương trình Đại sứ để học sinh cũ giới thiệu học sinh mới.
- Hoa hồng Đại sứ được xử lý thủ công.
- Chưa có retarget tự động cho lead cũ.

Nhận xét:

HSA đã có ý thức khai thác vòng đời khách hàng sau khóa học. Tuy nhiên, cơ hội doanh thu từ học sinh cũ, lead cũ và nhóm học sinh sắp thi/xong khóa chưa được hệ thống hóa. Việc upsell còn phụ thuộc nhiều vào Sale và thời điểm thủ công.

## 4. Ưu điểm của quy trình

### 4.1 Đã bao phủ toàn bộ hành trình khách hàng

Quy trình không chỉ tập trung vào bán hàng mà bao gồm cả các giai đoạn sau bán: onboarding, học tập, chăm sóc, upsell và referral. Đây là điểm mạnh quan trọng vì giúp doanh nghiệp nhìn học viên như một vòng đời dài hạn, không chỉ là một giao dịch.

### 4.2 Đã có nền tảng CRM và thanh toán

Việc sử dụng EZSale và SePay webhook cho thấy HSA đã bắt đầu xây nền tảng dữ liệu và tự động hóa. Đặc biệt, xác nhận thanh toán tự động là một nút chặn quan trọng vì đây là sự kiện có thể kích hoạt các bước tiếp theo như gửi email, gửi Zalo OA, tạo task QLL, cấp quyền lớp học và cập nhật trạng thái học viên.

### 4.3 Kênh giao tiếp quen thuộc với người học

Zalo là kênh phù hợp với thị trường Việt Nam, đặc biệt với phụ huynh và học sinh. Việc dùng Zalo nhóm giúp giao tiếp nhanh, tạo cảm giác gần gũi và giảm rào cản sử dụng công cụ mới trong giai đoạn đầu.

### 4.4 Lộ trình nâng cấp có định hướng đúng

Tài liệu đã xác định đúng các nhóm cần nâng cấp:

- Tự động hóa lead từ landing page vào CRM.
- Zalo OA nurture Warm/Cold lead.
- Tự động hóa onboarding sau thanh toán.
- Dashboard QLL duyệt 1 click.
- Chuyển Zoom sang ClassIn.
- Tự động hóa điểm danh, tài liệu, nhắc lịch.
- NPS định kỳ.
- Retarget ads từ CRM.
- Upsell/cross-sell theo nhóm học viên.

Đây là các hạng mục có tác động trực tiếp đến năng suất vận hành và doanh thu.

## 5. Nhược điểm và điểm nghẽn

### 5.1 Phụ thuộc cao vào thao tác thủ công

Nhiều bước quan trọng vẫn cần con người thực hiện:

- Nhập lead vào EZSale.
- Nhắn tin nhắc lead.
- Gọi lại Warm/Cold lead.
- Tạo nhóm Zalo giữa Sale và QLL.
- QLL kiểm tra, duyệt và thêm học sinh vào lớp.
- Gửi hướng dẫn onboard.
- Gửi link Zoom hằng ngày.
- Điểm danh.
- Nhắc học sinh vắng.
- Tổng hợp phản hồi học viên.
- Xử lý hoa hồng Đại sứ.

Khi quy mô tăng, các thao tác này không chỉ tốn thời gian mà còn làm tăng rủi ro sai sót.

### 5.2 Dữ liệu phân mảnh trên nhiều công cụ

Dữ liệu đang nằm ở nhiều nơi:

- Landing page.
- EZSale CRM.
- Website thanh toán.
- SePay.
- Zalo cá nhân/nhóm.
- Zoom.
- Web tài liệu.
- Google Sheet.
- Sau này có thể thêm Zalo OA và ClassIn.

Nếu không có chuẩn dữ liệu và hệ thống đồng bộ, doanh nghiệp sẽ gặp khó khi muốn xây dashboard, trigger tự động, phân tích tỷ lệ chuyển đổi hoặc cá nhân hóa chăm sóc.

### 5.3 Chưa rõ owner và SLA

Tài liệu mô tả luồng công việc nhưng chưa nói rõ:

- Ai là người chịu trách nhiệm cuối cùng cho từng bước?
- Lead mới phải được gọi trong bao lâu?
- Học sinh đã thanh toán phải được onboard trong bao lâu?
- Câu hỏi trong nhóm Zalo phải được phản hồi trong bao lâu?
- Ai theo dõi học sinh vắng?
- Ai xử lý lỗi thanh toán, lỗi vào lớp, lỗi tài liệu?

Thiếu owner và SLA sẽ làm quy trình khó kiểm soát khi nhân sự tăng.

### 5.4 Chưa có hệ thống đo lường chất lượng vận hành

Tài liệu có nêu mục tiêu giảm 80% việc thủ công QLL, nhưng chưa có chỉ số nền để đo. Ví dụ:

- Trung bình QLL mất bao nhiêu phút để onboard một học sinh?
- Tỷ lệ học sinh thanh toán xong nhưng chưa vào lớp trong 24 giờ là bao nhiêu?
- Tỷ lệ lead bị bỏ sót là bao nhiêu?
- Tỷ lệ học sinh vắng 2-3 buổi liên tiếp là bao nhiêu?
- Tỷ lệ học sinh hài lòng/NPS hiện tại là bao nhiêu?

Không có baseline thì khó chứng minh hiệu quả của tự động hóa.

## 6. Phân tích rủi ro

### 6.1 Rủi ro bỏ sót lead

Do lead từ landing page chưa tự động đẩy vào CRM, Sale có thể nhập thiếu, nhập chậm, nhập sai hoặc trùng lặp. Rủi ro này ảnh hưởng trực tiếp đến doanh thu vì thời gian phản hồi lead là yếu tố quan trọng trong tư vấn giáo dục.

Mức độ ảnh hưởng: Cao  
Khả năng xảy ra: Cao  
Ưu tiên xử lý: Rất cao

### 6.2 Rủi ro chậm onboarding sau thanh toán

Thanh toán đã được xác nhận tự động, nhưng onboarding vẫn cần nhiều thao tác của Sale và QLL. Nếu học sinh đã trả tiền nhưng chưa nhận được hướng dẫn kịp thời, trải nghiệm sẽ giảm và có thể phát sinh khiếu nại.

Mức độ ảnh hưởng: Cao  
Khả năng xảy ra: Trung bình đến cao  
Ưu tiên xử lý: Rất cao

### 6.3 Rủi ro phụ thuộc vào Zalo nhóm

Zalo nhóm tiện lợi nhưng không phải hệ thống vận hành có cấu trúc. Các vấn đề có thể gặp:

- Khó tìm lại lịch sử xử lý.
- Khó gán ticket/owner.
- Khó thống kê câu hỏi lặp lại.
- Khó đo SLA phản hồi.
- Khó bàn giao khi QLL nghỉ hoặc đổi lớp.

Mức độ ảnh hưởng: Trung bình đến cao  
Khả năng xảy ra: Cao  
Ưu tiên xử lý: Cao

### 6.4 Rủi ro khi chuyển sang ClassIn

Chuyển từ Zoom/Zalo sang ClassIn có thể mang lại lợi ích lớn, nhưng đây là thay đổi hành vi của cả học sinh, giáo viên và QLL. Nếu triển khai nhanh mà thiếu pilot, có thể phát sinh lỗi đăng nhập, sai lịch, học sinh không biết vào lớp, giáo viên không quen công cụ, hoặc dữ liệu điểm danh không được đối soát đúng.

Mức độ ảnh hưởng: Cao  
Khả năng xảy ra: Trung bình  
Ưu tiên xử lý: Cao, nhưng nên pilot trước

### 6.5 Rủi ro bảo mật và dữ liệu cá nhân

Quy trình có nhiều dữ liệu nhạy cảm: thông tin học sinh/phụ huynh, số điện thoại, lịch sử thanh toán, hành vi học tập, điểm danh, phản hồi, nhóm retarget ads. Nếu phân quyền và lưu trữ không chặt chẽ, có thể phát sinh rủi ro rò rỉ dữ liệu hoặc sử dụng dữ liệu marketing chưa có đồng ý rõ ràng.

Mức độ ảnh hưởng: Cao  
Khả năng xảy ra: Trung bình  
Ưu tiên xử lý: Cao

### 6.6 Rủi ro automation sai kịch bản

Nếu tự động hóa nhưng dữ liệu đầu vào sai, học sinh có thể nhận sai lớp, sai link, sai lịch học, sai nội dung ưu đãi hoặc bị nhận quá nhiều tin. Automation cần có rule, log và cơ chế fallback cho con người can thiệp.

Mức độ ảnh hưởng: Trung bình đến cao  
Khả năng xảy ra: Trung bình  
Ưu tiên xử lý: Trung bình đến cao

## 7. Đánh giá theo từng bộ phận

| Bộ phận | Hiện trạng | Điểm mạnh | Vấn đề chính | Mức ưu tiên |
|---|---|---|---|---|
| Marketing | Ads + landing page | Đã có kênh tạo lead | Lead chưa tự động vào CRM | Rất cao |
| CRM/Sale | Quản lý Hot/Warm/Cold trên EZSale | Có CRM làm nền tảng | Nhắc lead và nurture còn thủ công | Cao |
| Thanh toán | Web + SePay webhook | Tự động xác nhận thanh toán | Nhắc thanh toán fail còn thủ công | Trung bình |
| Onboarding | Sale/QLL xử lý qua Zalo | Gần gũi, linh hoạt | Chậm, dễ sót, tốn nhân lực | Rất cao |
| Học tập | Zoom + Zalo | Dễ triển khai, quen thuộc | Điểm danh và link học thủ công | Cao |
| Chăm sóc | GV/QLL phản hồi trong Zalo | Trực tiếp, gần học viên | Khó đo lường, khó mở rộng | Cao |
| Upsell/Referral | Sale gợi ý, Đại sứ thủ công | Có cơ hội doanh thu sau khóa | Chưa có trigger và retarget | Trung bình đến cao |

## 8. Thứ tự ưu tiên cải tiến

### Ưu tiên 1: Kết nối landing page với EZSale CRM

Mục tiêu:

- Tự động đẩy lead từ form về CRM.
- Giảm nhập tay.
- Giảm bỏ sót lead.
- Tăng tốc độ phản hồi.

Đề xuất:

- Chuẩn hóa trường dữ liệu lead: họ tên, số điện thoại, lớp/khóa quan tâm, nguồn ads, campaign, thời gian đăng ký.
- Tạo rule tránh trùng lead theo số điện thoại.
- Gán owner hoặc queue Sale tự động.
- Tạo thông báo lead mới cho Sale.

KPI cần đo:

- Tỷ lệ lead vào CRM thành công.
- Thời gian từ lúc đăng ký đến lúc Sale liên hệ đầu tiên.
- Tỷ lệ lead trùng.
- Tỷ lệ lead bị bỏ sót.

### Ưu tiên 2: Tự động hóa onboarding sau thanh toán

Mục tiêu:

- Khi SePay xác nhận thanh toán, hệ thống tự động kích hoạt onboarding.
- Học sinh nhận được hướng dẫn nhanh hơn.
- QLL giảm việc nhắn tay và gửi lặp lại.

Đề xuất:

- Tạo event `payment_success`.
- Tự động gửi email/Zalo OA xác nhận thanh toán.
- Tự động gửi hướng dẫn vào lớp, link nhóm, tài liệu, SBD nếu đã có dữ liệu.
- Tạo dashboard danh sách học sinh cho QLL duyệt.
- Có trạng thái onboarding: Chờ duyệt, Đã duyệt, Đã vào nhóm, Đã nhận hướng dẫn, Cần hỗ trợ.

KPI cần đo:

- Thời gian từ thanh toán thành công đến khi học sinh nhận hướng dẫn.
- Tỷ lệ học sinh onboard thành công trong 24 giờ.
- Số phút QLL xử lý trung bình mỗi học sinh.
- Số ticket phát sinh trong quá trình onboarding.

### Ưu tiên 3: Zalo OA nurture Warm/Cold lead

Mục tiêu:

- Sale tập trung vào Hot lead.
- Warm/Cold lead vẫn được chăm sóc có hệ thống.
- Giảm việc nhắc thủ công.

Đề xuất:

- Xây chuỗi tin nhắn theo trạng thái lead.
- Nội dung tin nhắn theo khóa học quan tâm.
- Đặt giờ gửi, giới hạn tần suất gửi.
- Theo dõi hành vi: đã đọc, bấm link, đăng ký lại, thanh toán.

KPI cần đo:

- Tỷ lệ Warm/Cold quay lại tư vấn.
- Tỷ lệ click link giỏ hàng.
- Tỷ lệ chuyển đổi sau nurture.
- Tỷ lệ hủy nhận tin hoặc phản hồi tiêu cực.

### Ưu tiên 4: Dashboard vận hành cho QLL và Sale

Mục tiêu:

- Tập trung các việc cần xử lý mỗi ngày.
- Giảm phụ thuộc vào Zalo và ghi nhớ cá nhân.
- Tạo khả năng đo lường SLA.

Dashboard nên có:

- Lead mới chưa xử lý.
- Lead quá hạn gọi lại.
- Học sinh đã thanh toán chờ onboard.
- Học sinh chưa vào nhóm lớp.
- Học sinh vắng học.
- Câu hỏi/chăm sóc chưa xử lý.
- Học sinh có nguy cơ bỏ học.

KPI cần đo:

- Số task quá hạn.
- SLA xử lý lead.
- SLA onboarding.
- SLA phản hồi học viên.
- Tỷ lệ task hoàn thành đúng hạn.

### Ưu tiên 5: Pilot ClassIn trước khi rollout

Mục tiêu:

- Kiểm chứng khả năng thay thế Zoom/Zalo trong một phạm vi nhỏ.
- Đo mức độ chấp nhận của học sinh, giáo viên và QLL.
- Đảm bảo dữ liệu điểm danh và tài liệu được ghi nhận đúng.

Đề xuất:

- Chọn 1-3 lớp pilot.
- Chạy song song quy trình backup trong 1-2 tuần đầu.
- Tạo hướng dẫn vào lớp cho học sinh/phụ huynh.
- Đào tạo giáo viên và QLL.
- Sau pilot mới rollout theo đợt.

KPI cần đo:

- Tỷ lệ học sinh vào lớp thành công.
- Tỷ lệ lỗi đăng nhập/lỗi link.
- Tỷ lệ điểm danh đúng.
- Mức hài lòng của GV, QLL, học sinh.
- Số ticket hỗ trợ trên mỗi lớp.

## 9. Lộ trình triển khai đề xuất

### Phase 1: Ổn định dữ liệu và giảm việc lặp lại

Thời gian gợi ý: 2-4 tuần

Hạng mục:

- Kết nối form landing page vào EZSale.
- Chuẩn hóa trường dữ liệu lead.
- Tạo rule chống trùng lead.
- Tạo email/Zalo OA xác nhận thanh toán và hướng dẫn cơ bản.
- Tạo danh sách học sinh đã thanh toán cho QLL xử lý.
- Thiết lập KPI baseline cho Sale và QLL.

Kết quả mong đợi:

- Giảm nhập tay lead.
- Giảm bỏ sót lead.
- Học sinh thanh toán xong nhận thông tin nhanh hơn.
- Có dữ liệu để đo hiệu quả các phase sau.

### Phase 2: Tự động hóa chăm sóc và lớp học

Thời gian gợi ý: 1-2 tháng

Hạng mục:

- Zalo OA nurture Warm/Cold.
- Zalo OA nhắc thanh toán chưa thành công.
- Dashboard QLL duyệt onboarding 1 click.
- Pilot ClassIn.
- Tự động hóa điểm danh và tài liệu với ClassIn nếu pilot đạt yêu cầu.
- Tạo NPS định kỳ.
- Broadcast ưu đãi theo danh sách.

Kết quả mong đợi:

- Sale tập trung vào Hot lead.
- QLL giảm việc gửi link, nhắc từng học sinh, điểm danh tay.
- Có dữ liệu học tập có cấu trúc hơn.
- Có cơ chế chăm sóc định kỳ thay vì phản hồi rời rạc.

### Phase 3: Vận hành dựa trên dữ liệu

Thời gian gợi ý: Sau 3 tháng

Hạng mục:

- Dashboard realtime từ ClassIn, CRM và thanh toán.
- Trigger học sinh vắng nhiều buổi, điểm thấp, không tương tác.
- Chatbot Zalo OA/web cho câu hỏi thường gặp.
- Retarget Ads từ CRM custom audience.
- Cross-sell/upsell theo hành vi học và mốc thời gian.
- Tự động tính hoa hồng Đại sứ.

Kết quả mong đợi:

- Vận hành chủ động dựa trên tín hiệu dữ liệu.
- Tăng tỷ lệ giữ chân và tái mua.
- Giảm tải cho Sale, QLL, GV.
- Tạo nền tảng mở rộng quy mô lớp và khóa học mới.

## 10. Hệ thống KPI nên theo dõi

### 10.1 KPI Marketing và CRM

- Số lead mới theo ngày/tuần/tháng.
- Chi phí trên mỗi lead.
- Tỷ lệ lead vào CRM thành công.
- Tỷ lệ lead trùng.
- Thời gian phản hồi lead đầu tiên.
- Tỷ lệ chuyển đổi lead thành học viên.
- Tỷ lệ Hot/Warm/Cold theo nguồn.

### 10.2 KPI Sale

- Số lead được liên hệ mỗi ngày.
- Tỷ lệ lead liên hệ thành công.
- Tỷ lệ chốt sale theo Sale.
- Tỷ lệ follow-up đúng hạn.
- Doanh thu theo Sale.
- Tỷ lệ Warm/Cold được nurture lại.

### 10.3 KPI Thanh toán

- Tỷ lệ thanh toán thành công.
- Tỷ lệ thanh toán lỗi/chưa hoàn tất.
- Thời gian xác nhận thanh toán.
- Số giao dịch cần xử lý thủ công.
- Tỷ lệ học sinh thanh toán xong nhưng chưa onboard.

### 10.4 KPI Onboarding

- Thời gian từ thanh toán đến onboard thành công.
- Tỷ lệ onboard trong 24 giờ.
- Số học sinh cần QLL hỗ trợ riêng.
- Số lỗi sai lớp, sai link, thiếu thông tin.
- Mức hài lòng sau onboarding.

### 10.5 KPI Học tập và chăm sóc

- Tỷ lệ tham gia lớp.
- Tỷ lệ vắng học.
- Tỷ lệ học sinh vắng 2-3 buổi liên tiếp.
- Thời gian phản hồi câu hỏi.
- Số câu hỏi/ticket mỗi lớp.
- Điểm NPS định kỳ.
- Tỷ lệ học sinh có nguy cơ bỏ học.

### 10.6 KPI Upsell, cross-sell và referral

- Tỷ lệ học sinh cũ mua khóa mới.
- Doanh thu upsell/cross-sell.
- Số referral từ Đại sứ.
- Tỷ lệ referral thành học viên.
- Chi phí hoa hồng trên doanh thu referral.
- Tỷ lệ lead cũ được kích hoạt lại.

## 11. Nguyên tắc triển khai automation

### 11.1 Tự động hóa nhưng vẫn cần có điểm kiểm soát

Không nên để automation tự quyết định toàn bộ các bước nhạy cảm. Các bước như gán lớp, cấp quyền học, gửi thông tin quan trọng và tính hoa hồng cần có log, trạng thái và cơ chế con người duyệt khi dữ liệu không chắc chắn.

### 11.2 Dữ liệu đầu vào phải sạch

Automation chỉ hiệu quả khi dữ liệu đúng. Cần chuẩn hóa:

- Mã học sinh.
- Số điện thoại.
- Khóa học.
- Lớp học.
- Nguồn lead.
- Trạng thái thanh toán.
- Trạng thái onboarding.
- Trạng thái học tập.

### 11.3 Tránh gửi quá nhiều tin nhắn

Zalo OA và email cần có tần suất hợp lý. Nếu học sinh/phụ huynh nhận quá nhiều tin, automation có thể gây phản cảm. Cần có rule giới hạn số lần gửi, khung giờ gửi và nội dung theo đúng ngữ cảnh.

### 11.4 Mỗi automation cần có log và fallback

Mỗi luồng tự động nên có:

- Trạng thái thành công/thất bại.
- Lý do lỗi.
- Người phụ trách xử lý nếu lỗi.
- Lịch sử gửi tin.
- Cơ chế gửi lại hoặc xử lý tay.

## 12. Kết luận và khuyến nghị

Quy trình vận hành của HSA Education đã có nền tảng tốt và tư duy nâng cấp đúng hướng. Hệ thống hiện tại đã đáp ứng được giai đoạn vận hành ban đầu, đặc biệt ở các điểm như quảng cáo, landing page, CRM, thanh toán web, SePay webhook, Zalo nhóm và Zoom.

Tuy nhiên, nếu mục tiêu là mở rộng quy mô, tăng năng suất và cải thiện trải nghiệm học viên, HSA cần xử lý sớm các điểm nghẽn thủ công. Trong đó, ba việc nên ưu tiên nhất là:

1. Tự động đẩy lead từ landing page vào EZSale CRM.
2. Tự động hóa onboarding sau khi thanh toán thành công.
3. Xây dashboard vận hành để Sale và QLL xử lý việc theo trạng thái, SLA và mức ưu tiên.

Sau khi nền tảng dữ liệu và dashboard ổn định, HSA có thể triển khai Zalo OA nurture, ClassIn, NPS, chatbot, retarget ads và upsell/cross-sell tự động. Cách tiếp cận tốt nhất là triển khai theo từng phase, có pilot, có KPI baseline và có người chịu trách nhiệm rõ ràng cho từng luồng nghiệp vụ.

Nếu làm đúng thứ tự, automation không chỉ giúp giảm việc thủ công mà còn giúp HSA tạo ra một hệ thống vận hành có khả năng mở rộng, đo lường được và liên tục cải tiến theo dữ liệu.
