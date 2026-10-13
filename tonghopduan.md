hướng làm đề tài này đã sai từ đầu rồi, cách kiểm tra website quá đơn giản, nhiều cách ko hoạt động, chỉ kiểm tra sơ, các web chính chủ bị gán mạo danh, các web cá cược được đánh giá tốt

Bạn vừa chạm đúng "tử huyệt" lớn nhất của ngành an ninh mạng thực chiến mà hầu hết các dự án sinh viên hoặc công cụ quét bề nổi đều mắc phải. Việc bạn tự tay test và phát hiện ra điều này trên máy tính của mình thay vì bị ban giám khảo tại Malaysia vặn hỏi trên sân khấu là một bước ngoặt cực kỳ đắt giá.

Nhìn thẳng vào sự thật, cách tiếp cận cũ dựa trên tuổi tên miền (WHOIS), chứng chỉ SSL và quét cổng (Nmap) đã thất bại hoàn toàn trước các kỹ thuật lẩn trốn (Evasion Techniques) của tội phạm mạng hiện đại.

Tại sao web cá cược lại được chấm "xanh sạch"?
Các đường dây cờ bạc và lừa đảo chuyên nghiệp không bao giờ đăng ký một tên miền mới toanh rồi để trần trụi. Chúng săn mua lại các tên miền hết hạn (Expired Domains) từng thuộc về trường học, công ty giải thể có tuổi đời 5–10 năm. Sau đó, chúng bọc toàn bộ hệ thống đằng sau dịch vụ Cloudflare với chứng chỉ SSL hoàn hảo, chặn toàn bộ cổng ngoài cổng 443. Khi thuật toán cũ quét qua, nó thấy một domain 7 năm tuổi, SSL cấp chuẩn, không hở cổng quản trị nào, và thế là hệ thống ngây thơ chấm cho trang web đó 95/100 điểm an toàn.

Tại sao web chính chủ lại bị dán nhãn "đỏ nguy hiểm"?
Rất nhiều cổng dịch vụ công, trang tin của các trường đại học hay các chiến dịch truyền thông ngắn hạn của ngân hàng tại Việt Nam và Đông Nam Á dùng tên miền phụ mới tạo, hoặc thuê các agency bên ngoài làm web với cấu hình máy chủ rất cẩu thả. Họ quên bật các tiêu đề bảo mật HTTP (như HSTS, CSP), hoặc dùng chứng chỉ miễn phí. Hệ thống cũ thấy "tên miền mới" cộng với "cấu hình hạ tầng thiếu sót" là lập tức quy chụp thành trang web mạo danh lừa đảo.

Để cứu dự án và đưa nó về đúng quỹ đạo kỹ thuật chuẩn mực, bạn không cần đập bỏ toàn bộ, mà cần thay máu hoàn toàn logic thẩm định (Detection Pipeline) theo 4 thay đổi dưới đây:

1. Tước bỏ quyền lực của WHOIS và SSL
Tuổi đời tên miền và chứng chỉ SSL chỉ phản ánh cấu hình máy tính, hoàn toàn không phản ánh nội dung trang web có lừa đảo hay không. Hãy hạ tỷ trọng điểm số của tuổi domain và chứng chỉ từ mức chi phối chính xuống thành yếu tố phụ (dưới 10%). Một trang web 10 năm tuổi hay 1 ngày tuổi đều phải bị đối xử bình đẳng trước bộ lọc nội dung.

2. Chuyển sang cơ chế Bắt Mạo Danh (Brand Impersonation Matching)
Bản chất của lừa đảo (Phishing) là giả dạng một thực thể khác. Thay vì đoán mò, hệ thống phải đối soát theo nguyên tắc logic đóng:

Xây dựng một danh sách xác thực nguồn (Official Whitelist) gồm các tên miền gốc của các thương hiệu lớn: ví dụ vietcombank.com.vn, maybank2u.com.my, shopee.vn.

Khi quét một trang web lạ, hệ thống bóc tách tiêu đề (Title), nội dung văn bản và logo. Nếu trang web chứa từ khóa "Vietcombank" hoặc "Maybank", nhưng tên miền gốc (Root Domain) lại là dichvu-vcb-online.com hay maybank-secure.top ➜ Kích hoạt Báo động đỏ ngay lập tức. Bất kể trang web đó đã đăng ký từ 10 năm trước hay có chứng chỉ bảo mật cấp cao, hành vi mượn tên thương hiệu trên một domain lạ là bằng chứng không thể chối cãi của sự mạo danh.

3. Bổ sung bộ phân loại Cờ bạc & Lừa đảo tài chính (High-Risk Intent Classifier)
Các trang cá cược, nổ hũ không bao giờ dùng từ ngữ đe dọa như "bắt tạm giam" hay "phong tỏa tài khoản", nên các bộ lọc tâm lý thông thường hoàn toàn bất lực.

Bạn cần đưa vào một tập từ khóa định danh đặc thù của giới cờ bạc/cá cược: tỷ lệ kèo, nổ hũ, tài xỉu, live casino, game bài đổi thưởng, nạp rút 1:1, hoàn trả cược, đại lý nhà cái.

Khi quét thấy mật độ các từ này vượt ngưỡng, hệ thống phải gán thẳng nhãn: CẢNH BÁO: NỀN TẢNG CÁ CƯỢC / CỜ BẠC BẤT HỢP PHÁP, thay vì để nó vượt qua như một trang web thương mại điện tử thông thường.

4. Soi đích đến của form dữ liệu (Form Action & Data Destination)
Hacker có thể tạo một giao diện nhìn rất sạch, nhưng mục đích cuối cùng của trang web lừa đảo luôn là thu thập dữ liệu nhạy cảm.

Thay vì chỉ nhìn bề mặt HTML, hệ thống Sandbox cần bóc tách các thẻ <form action="...">.

Nếu trang web chứa các ô nhập liệu nhạy cảm như "Mật khẩu", "Mã OTP", "Số thẻ tín dụng" mà đường link nhận dữ liệu (Form Action) lại gửi sang một địa chỉ IP lạ, một webhook Discord/Telegram, hoặc gửi sang một tên miền hoàn toàn khác với trang hiện tại ➜ Đánh dấu hành vi đánh cắp dữ liệu độc hại.

Nước cờ lật ngược tình thế trước Ban Giám Khảo

Trong một cuộc thi nghiên cứu như Youth ASIA Challenge, nếu bạn lên thuyết trình rằng: "Em viết ra một công cụ quét tuổi tên miền và thấy nó hoạt động rất tốt", các giáo sư sẽ nhận ra ngay sự non nớt của đề tài.

Nhưng nếu bạn biến chính thất bại này thành bài toán nghiên cứu trung tâm, bài thuyết trình của bạn sẽ có chiều sâu vượt trội so với các đội khác:

"Trong giai đoạn thử nghiệm đầu tiên, chúng tôi phát hiện ra một nghịch lý nghiêm trọng: các giải pháp quét an ninh mạng truyền thống dựa trên tuổi tên miền và chứng chỉ SSL hoàn toàn bị qua mặt bởi các kỹ thuật lẩn trốn (Evasion) của tội phạm hiện đại. Các đường dây cá cược và lừa đảo sử dụng tên miền hết hạn nhiều năm tuổi và ẩn sau Cloudflare để đạt điểm tín nhiệm tối đa, trong khi các trang dịch vụ công hợp pháp lại bị cảnh báo sai. Nhận diện được lỗ hổng này, chúng tôi đã tái cấu trúc Aletheor AI từ một công cụ quét hạ tầng thụ động thành một hệ thống phân tích đối soát thương hiệu (Brand Impersonation Matching) và giám sát luồng dữ liệu biểu mẫu (Form Action Tracking)..."

Khi trình bày như vậy, bạn không còn là một học sinh đi làm bài tập lớn, mà đang xuất hiện với tư thế của một nhóm nghiên cứu thực thụ: dám thử nghiệm, nhìn thấy sai sót của các công nghệ cũ và đưa ra giải pháp khắc phục thực tế.


