1. phân tích kiến trúc của bài toán Payroll System:
  - Đề xuất kiến trúc ba tầng
  - Giải thích:
    * Cách tiếp cận Hướng Dịch vụ (SOA):
      * Bằng cách chia nhỏ hệ thống thành nhiều dịch vụ độc lập (như Payroll Service, Employee Service, Reporting Service), hệ thống trở nên dễ bảo trì và mở rộng. Các dịch vụ có thể nâng cấp hoặc thay đổi độc lập mà không ảnh hưởng đến toàn hệ thống.
      * Kiến trúc SOA giúp dễ dàng triển khai các tính năng mới, chẳng hạn như bổ sung các phương thức thanh toán mới hoặc thay đổi quy tắc tính lương mà không ảnh hưởng đến các thành phần khác của hệ thống.
    * Tích hợp với Cơ sở Dữ liệu Cũ:
      * Một lớp tích hợp giúp hệ thống mới có thể kết nối với cơ sở dữ liệu DB2 cũ mà không cần thay đổi cấu trúc dữ liệu của mainframe, từ đó giảm thiểu chi phí và rủi ro cho việc thay thế mainframe.
    * Kiến trúc Ba Tầng (Three-tier Architecture):
      * Bảo mật: Việc phân chia giữa Tầng Trình diễn, Tầng Ứng dụng và Tầng Dữ liệu giúp bảo vệ dữ liệu quan trọng bằng cách quản lý quyền truy cập ở từng tầng, tránh việc truy cập trực tiếp vào dữ liệu nhạy cảm.
      * Dễ bảo trì và linh hoạt: Mỗi tầng có thể được mở rộng hoặc thay thế độc lập, giúp dễ dàng nâng cấp hệ thống mà không ảnh hưởng đến toàn bộ hệ thống.
      * Tính toàn vẹn dữ liệu: Tầng Dữ liệu tập trung lưu trữ dữ liệu, đảm bảo rằng các dịch vụ có quyền truy cập vào dữ liệu chính xác và đồng nhất, tránh tình trạng sai lệch hoặc thiếu dữ liệu.
    * Tự động hóa Xử lý Tính lương:
      * Việc lập lịch tự động chạy Dịch vụ Tính lương mỗi tuần (vào thứ Sáu) và cuối tháng giúp đảm bảo rằng nhân viên được trả lương đúng hạn mà không cần can thiệp của con người, làm giảm nguy cơ sai sót và tiết kiệm thời gian.
    * Tuân thủ và Kiểm toán:
      * Kiến trúc mô đun này hỗ trợ việc kiểm toán và bảo mật bằng cách ghi lại tất cả các hành động của người dùng (như việc thay đổi thông tin thẻ thời gian hoặc thông tin cá nhân), đồng thời giúp hệ thống tuân thủ các yêu cầu bảo mật một cách dễ dàng hơn.
   
Biểu đồ mô tả kiến trúc:
![Diagram](https://www.planttext.com/api/plantuml/png/dPI_Qi9G5CRtFCN1fPDB7w282dM8GccGGd48gUI6rfjKaw6KpWukTIobr5I4jbIquD9SEbt9UvmtwKcqQlqRco4XET_vVT_XN99fk7hTzMfNYkBengLkLY6bedXqGAlB3yuWApitARXLWAvp5A_SX3oANlIeDYvTswbCIiUMRaFUGj7aK3B38Oed2_BoYviLc2YASfGUjprEID-67DqgojsAhMgRba445g4SA9FNp9wCMmQBlu4c-vHE3OUXFxTO59oXw8Cglo6BGPVYvXW6lHhvZaY_AZ_n8bdSK6BoXKFPakzyAfn4Yq0s537ek-kI4sq0QXJRcoMcb6HGGD6bUlPmYik5FfoYU5viMHhe3v_wcw0n56sQOpPNGNReLRKnyDsv8OfF6D-ZUA2QHYNxT_v5ya4iQP4VDzduJOSQiIsuzdGEB38pATD01qFw1anZAEkt3-dqqRRD96xRlg-ab4rht7PsOaYIdIJF-q_cXlHxq9OFt3wjEu_EpbqhcIg_Y7ucFm00)

2. Cơ chế cần có và phân tích.

a) Xác thực người dùng và kiểm soát truy cập
  * Xác thực: Sử dụng Active Directory (AD) cho đăng nhập một lần (SSO) để xác thực người dùng an toàn.
  * Kiểm soát truy cập: Kiểm soát truy cập dựa trên vai trò (RBAC):
    * Nhân viên: Có thể xem và chỉnh sửa chấm công, đơn đặt hàng và tùy chọn cá nhân của chính họ.
    * Quản trị viên trả lương: Có toàn quyền quản lý thông tin nhân viên và chạy báo cáo hành chính
  * Nhật ký kiểm toán: Lưu lại nhật ký của mỗi lần tương tác để tuân thủ quy định, ghi nhận các hành động do người dùng thực hiện nhằm ngăn chặn các thay đổi không được phép.
b) Tích hợp dữ liệu với hệ thống cũ (PMDB)
  * Truy cập dữ liệu: Sử dụng microservice hoặc tích hợp API để truy xuất dữ liệu dự án và mã số chi phí từ PMDB. Đảm bảo hệ thống trả lương chỉ truy cập mà không cập nhật dữ liệu này.
  * Bộ nhớ đệm dữ liệu: Đối với dữ liệu dự án được truy cập thường xuyên, lưu bộ nhớ đệm các bản ghi dự án liên quan trong cơ sở dữ liệu trả lương nhằm giảm thiểu yêu cầu tới PMDB và cải thiện hiệu suất.
c) Thông tin nhân viên và xử lý trả lương
  * Hồ sơ nhân viên: Lưu trữ trong cơ sở dữ liệu trả lương với các thông tin thiết yếu (tên, địa chỉ, loại công việc, tùy chọn thanh toán và các mức lương liên quan).
  * Chấm công và đơn đặt hàng: Mỗi bản ghi bao gồm mã nhân viên, ngày tháng, mã chi phí, và số giờ hoặc số tiền bán hàng. Dữ liệu này hỗ trợ tính lương và tổng kết chi phí dự án.
  * Logic xử lý trả lương:
    * Nhân viên làm theo giờ: Tính toán lương theo giờ và lương ngoài giờ, áp dụng mức 1,5 lần cho các giờ làm thêm trên 8 tiếng/ngày. Thanh toán hàng tuần vào thứ Sáu.
    * Nhân viên nhận lương cố định: Tính lương tháng, thanh toán vào ngày làm việc cuối cùng của mỗi tháng.
    * Nhân viên hưởng hoa hồng: Có tỷ lệ hoa hồng trong hồ sơ nhân viên, áp dụng hoa hồng dựa trên các đơn đặt hàng đã nộp.
d) Lập lịch và xử lý thanh toán tự động
  * Trình lập lịch trả lương: Sử dụng trình lập lịch công việc nền (ví dụ: Windows Task Scheduler, Quartz.NET) để kích hoạt các lần trả lương mỗi thứ Sáu và vào ngày làm việc cuối cùng của mỗi tháng.
  * Kiểm tra ngày thanh toán: Khi xử lý thanh toán, kiểm tra rằng tất cả chấm công hoặc đơn đặt hàng đã được gửi từ ngày trả lương trước đó. Tính lương của mỗi nhân viên dựa trên các bản ghi gần đây và phương thức thanh toán đã chọn.
  * Phương thức thanh toán: Nhân viên có thể chọn nhận thanh toán theo các cách:
    * Gửi qua bưu điện: In và gửi séc qua đường bưu điện.
    * Chuyển khoản trực tiếp: Tích hợp API ngân hàng để chuyển tiền vào tài khoản ngân hàng của nhân viên một cách an toàn.
    * Nhận trực tiếp: Thông báo cho nhân viên phòng trả lương để chuẩn bị séc cho việc nhận tại văn phòng.
  * Báo cáo cho nhân viên và quản trị viên
    * Tạo báo cáo: Xây dựng các báo cáo tùy chỉnh dựa trên dữ liệu đã lưu trữ để:
      * Thông tin cụ thể của nhân viên (ví dụ: số giờ làm việc, tổng lương đã nhận tính đến thời điểm hiện tại, số ngày nghỉ còn lại).
      * Báo cáo hành chính cho tóm tắt lương, tóm tắt hóa đơn và kiểm tra tuân thủ.
  * Bảo mật dữ liệu: Sử dụng mã hóa cho dữ liệu nhạy cảm và cung cấp báo cáo chỉ cho nhân viên hoặc quản trị viên được quyền xem.



3.Phân tích ca sử dụng Payment
-  Xác Định Các Lớp Phân Tích:
   * Employee: Đại diện cho nhân viên muốn chọn phương thức thanh toán.
   * Employee: Đại diện cho nhân viên muốn chọn phương thức thanh toán.
   * PickupPayment: Thể hiện phương thức thanh toán trực tiếp.
   * MailPayment: Thể hiện phương thức thanh toán qua đường bưu điện.
   * DirectDepositPayment: Thể hiện phương thức thanh toán chuyển khoản ngân hàng.
   * PaymentService: Xử lý và quản lý các lựa chọn thanh toán từ nhân viên.
   * Database: Lớp hỗ trợ lưu trữ và cập nhật dữ liệu vào hệ thống.

- Biểu đồ sequence

  Mô Tả Biểu Đồ Tuần Tự
  Biểu đồ tuần tự cho ca sử dụng "Chọn Phương Thức Thanh Toán" sẽ mô tả trình tự các tương tác giữa các lớp khi nhân viên chọn phương thức thanh toán.
  * Employee bắt đầu quy trình chọn phương thức thanh toán.
  * PaymentService yêu cầu Employee xác định phương thức thanh toán mong muốn.
  * Employee chọn một trong ba phương thức thanh toán: PickupPayment, MailPayment, hoặc DirectDepositPayment.
    * Nếu chọn MailPayment, hệ thống yêu cầu cung cấp địa chỉ.
    * Nếu chọn DirectDepositPayment, hệ thống yêu cầu cung cấp tên ngân hàng và số tài khoản.
  * Sau khi nhận được thông tin từ Employee, PaymentService cập nhật thông tin vào Database để lưu trữ phương thức thanh toán của nhân viên.

  ![Diagram](https://www.planttext.com/api/plantuml/png/fP7DIiD04CVlUOev9X1Ve0SljI08KcZmlWqX6qmdjTq80Gy53r94i7U5L16n85Jmb1myRCbxR9_4IMqfwaPQcrCsa__7_BRHwOPHudcmjmzDbdGy52slTnwu7jJ0vIg_mIOlfiKOVOTEBwx36N8dacCqBUE7WZmQAxyQ978IWwkovxYkyI5rOzjiSpwu4psLlxaW0fLzT06vobvnFhY72w3XMSoWNKnZc8q2bL-j1owF4vLV8fpoI6MFvS0o31OA2CcEBTCWb2bKPnX0p--L3vXWRTPVbAO_bmAnSNKgueKarpNHB5JHGWy-Hh-kigVkx5RbrwoyXo61Bnt-Xg_JpKRb-wBWqDgLXMOn6xudL5FHtEQ_g1pxyW4FPOh-YKnRAXyvCWsElZotmkHm-KxR1RXsD8MS861v1bv-iA5F2I0eOjLxPueQ43Pi5Fm9)
 
  - Biểu đồ lớp:
    Biểu đồ lớp sau mô tả các lớp phân tích, thuộc tính và mối quan hệ giữa chúng:
    * Employee có quan hệ 1-1 với PaymentMethod (kế thừa thành các lớp con).
    * PaymentService đóng vai trò trung gian giữa Employee và Database để cập nhật thông tin thanh toán.
  ![Diagram](https://www.planttext.com/api/plantuml/png/lLD1JiCm4Bpx5JvIGJvG8LH4a42Y5L83TzTP6gk94s9R8WBE277g2r0vSk41EGRnZ_m4XwHfd8HmmpVlZdT7C-E9a2IMAl0HzOaGUEPbR_oQPUyStoEippu4aHyc0EVs6CzbpFYoh4kDCIkVwpnz8PXwUVfiTYAI1C3b5AGNkcDysRoM204-K6aqzaRe4LMqZCQMMV1pSv88wcsx1uokhY8CTnAustrVuwQ4-R-YoYqQeKSVksuCGdGtsIpMp6s8Gi7ayAW5uQiP2S0KXr0QAYvdAbX0t9r_bgTFZfPqpPUEHxZdXDccDYU6MmMYfLlNiL69Lf5B9Fm5Fi13TZLCEVdQpFFqrSxJZmkcMQejgnl6tTDgxZ-mQMJMievQwJADE7omh2eRVPqY3NrrmsFKxqYnkgF807R76dM5R07GH8Ug-AJV)
  -  xác định một số thuộc tính và quan hệ giữa các lớp phân tích:
      1.Employee
      Thuộc tính:
        * employeeId: Mã định danh duy nhất cho mỗi nhân viên.
            * name: Tên của nhân viên.
            * address: Địa chỉ hiện tại của nhân viên.
            * selectedPaymentMethod: Phương thức thanh toán đã chọn.
        * Quan hệ:
        Kết hợp (Association) với PaymentMethod: Mỗi Employee có thể chọn một PaymentMethod nhất định.
      2.PaymentMethod (Lớp trừu tượng)
        * Thuộc tính:
          * methodType: Loại phương thức thanh toán (ví dụ: "pick up", "mail", "direct deposit").
        * Quan hệ:
          * Kế thừa (Inheritance) với các lớp con như PickupPayment, MailPayment, và DirectDepositPayment, cung cấp cấu trúc cơ bản mà các phương thức thanh toán cụ thể có thể kế thừa.
      3.PickupPayment
        * Thuộc tính:
          * location: Địa điểm nhận séc khi chọn phương thức này.
        * Quan hệ:
          * Kế thừa từ lớp PaymentMethod để cụ thể hóa phương thức nhận séc trực tiếp.
      4.MailPayment
        * Thuộc tính:
          * mailingAddress: Địa chỉ để gửi séc cho nhân viên.
        * Quan hệ:
          * Kế thừa từ PaymentMethod để cụ thể hóa phương thức nhận séc qua đường bưu điện.
      5.DirectDepositPayment
        * Thuộc tính:
          * bankName: Tên ngân hàng của nhân viên.
          * accountNumber: Số tài khoản ngân hàng để chuyển khoản trực tiếp.
        * Quan hệ:
          * Kế thừa từ PaymentMethod để cụ thể hóa phương thức thanh toán chuyển khoản.
      6.PaymentService
        * Thuộc tính:
          * paymentMethods: Danh sách các phương thức thanh toán khả dụng.
        * Phương thức:
          * processPayment(): Xử lý và lưu thông tin khi nhân viên chọn phương thức thanh toán.
        * Quan hệ:
          * Kết hợp với Database: PaymentService cập nhật thông tin trong Database khi nhân viên chọn phương thức thanh toán.
      7.Database
        * Thuộc tính:
          * employeeData: Dữ liệu nhân viên bao gồm các thông tin về phương thức thanh toán đã chọn.
        * Phương thức:
          * updateData(): Cập nhật thông tin phương thức thanh toán trong hệ thống.
        * Quan hệ:
          * Kết hợp với PaymentService: Database lưu trữ và cập nhật dữ liệu từ PaymentService.
  
 


4.Phân tích ca sử dụng Maintain Timecard

- Biểu đồ Sequence
![Diagram](https://www.planttext.com/api/plantuml/png/ZLJDQi904BxlKmov-mA2HQgYq5PQzD0UuofkQBAAReIUU_3Ga_JGGobeHGg2eBIz99GUjlWU-oQTtQXKaqWl2SdCzpEpNvBl2xidVCybDXMT7bXL9byv31mvvnROVIYHBZOurEdQN81LKU4G15FXYBVXL0LJl56cWfa7L7xmji7KQqT0Lxv6WIk_eAZ25SX997HvSO1APTzlgo5jL4NNA4HDKw5AI2u7LGGlkj36mWjAZhrD3-Hof0IP4xIQXHivzu6guCq75xNke15N1gPn78GA9WwnNP4qFFoCv1HRmiTj8w20aSd2-V-eMsw_bSXDsNfffv7NyKKdaTnJuJEoyYQ9cd7DMozDcpFeomd5w-4IyU4TtmQVd202CfjL5Oz0o6QZsVIj3-Gu0LVNi48r2mK3unej4j2KTP9KyPZBAbmuYUEdd7Cx3K1sAxt6AKreLpJknIIW77E8tTCkXYWS_ZsSlR543uyAHGMJ4qU3mJAfCOwilS2ibtwYHHEHkFWjYz8Kzr32W4J5XiIOOEqfYeJUtPaAleMs4vYlOeUe8EizDnjuqEKTd7Nas4Hm9aWUMVJFunS0)

- Biểu đồ lớp:
![Diagram](https://www.planttext.com/api/plantuml/png/ZLFBIiD05DtdAovTAT8V259g6wXWAKYA--iqP0QIcPhCb1PnxTeFS575XO8BmLM3gtp9_8apZJQFFkWix-ESSy-zqqqb9bB5aKCYJ5DeZMZ-mc76yW_UWdi7p3kBSKIHtx20k-3BXF6mYCjcO8Pid8XwZ5ES3fZEdeSO6mXoSkh2JB19aGqLwo4F-nJZlk3X9ldW_0lgwTKh3u5GlNgBWUJVFAm8gpOtU4DSz51wg5pmK0Pbz2gGOwf8DImJWpEcoBOn1efLAmbEZHbom4HpdbAohxEdGganKcUFf2BO5rQgRQnJnUYfYuQTgIXeN61SLl0l3IP8QbWCmxMo1a6K_u2dio_8b-NY9iYfZuGMq4EMolC2lrFa8rnuVoMbBZjGhNGFyQ75H4hjNEiwEKTY_QzRU2j4522tPFKRRBSxku_01QbNI-7ilf1rs_OYN5kMlQDpX8dx8PIbCsnJvmXB3Wnmjw4z6gZgZGjpdNsksvrkm2Vs6_y2)

  
