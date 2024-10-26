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
- các lớp phân tích :
  * Nhân viên (Employee)
  * Phương thức thanh toán (PaymentMethod)
  * Hệ thống thanh toán (PaymentSystem)
  * Cơ sở dữ liệu (Database)
- Các thuộc tính và mối quan hệ:
  * Nhân viên (Employee)
    * Thuộc tính: employeeID, tên, địa chỉ, paymentMethod
    * Quan hệ: Liên kết với PaymentMethod
  * Phương thức thanh toán (PaymentMethod)
    * Thuộc tính: methodType (liệt kê các giá trị "nhận tại chỗ", "gửi qua bưu điện", "chuyển khoản trực tiếp")
    * Quan hệ: Tổng quát hóa cho các loại thanh toán cụ thể như PickUp, Mail, DirectDeposit
  * Cơ sở dữ liệu (Database)
    * Phương thức: findEmployee(employeeID), updatePaymentMethod(employeeID, paymentMethod)
- Biểu đồ sequence

  ![Diagram](https://www.planttext.com/api/plantuml/png/lLHDIyD04BtlhnXoxnyeLAGVAFX1GGzwJACqY-asjXkXPmyY8bWyUBHHH1IB2eAGFVImfVzZ_ecpMsrJjC4UD8SDpBxtPkObCyjjwRPar0YOwybc2vnDNzy8EEBwKJZWjg72GW9mZuUkaE2ieZe1wQFgYZgzNU63REfuZArkpOAh_kXb55iWgWVlEtvZ5byQnT16TGrxg9uXu9Ghii1oaR520TOZL9Savn6kXicjW4A1LI2tH9dpiAiV8_0mkQUJyfqB2EVdPzKjmwiiIfjaAqMc44pkGWXGlRg_8Rk1D0PZqXSWCOcv9Lqv3EWP5rJToCFm0fDtcQK4WUx1Tw6KJhb89T9z2CB0CjKihLo4t6ThEphsnZHXEfneiFsWlG_E2ge-XyF8XThe9O9XrtmdaMvYrjLX5zNPs9zJg5kof68i_YSxi_0Bc_HpiVzZLCcFEgYE1HpuJVktoXRzAQJ8CHc4ymsJD7iWwcOEVTsZIewe4PgBBjtq-9VQNFyMe2ziPtPBVswhs9lExJnKPgRq5g5uns2IGHxMVFrcGlgfOKDt7LARB4ApPgPjRHjG-fPMqIkH6_es7MQfZhBbfpsjSckkAVoCJakgwRXfLimLPsoP732xcdq0)
  * giải thích:
    * Luồng cơ bản:
      1.Bước 1: Nhân viên yêu cầu chọn phương thức thanh toán.
        * Nhân viên -> Hệ thống thanh toán: Nhân viên khởi tạo yêu cầu chọn phương thức thanh toán thông qua giao diện hệ thống.
      2.Bước 2: Hệ thống yêu cầu nhân viên cung cấp lựa chọn.
        * Hệ thống thanh toán -> Nhân viên: Hệ thống phản hồi yêu cầu và yêu cầu nhân viên chọn một trong các phương thức thanh toán: "Nhận tại chỗ", "Gửi qua bưu điện", hoặc "Chuyển khoản trực tiếp".
      3.Bước 3: Nhân viên cung cấp lựa chọn phương thức thanh toán.
        * Nhân viên -> Hệ thống thanh toán: Nhân viên chọn phương thức thanh toán mong muốn.
      4.Bước 4: Xử lý tùy thuộc vào phương thức thanh toán được chọn:
        * Nếu chọn "Nhận tại chỗ":
          * Hệ thống chỉ cần ghi nhận phương thức thanh toán mà không yêu cầu thêm thông tin.
          * Hệ thống thanh toán -> Phương thức thanh toán: Đặt phương thức thanh toán là "Nhận tại chỗ".
          * Hệ thống thanh toán -> Cơ sở dữ liệu: Cập nhật phương thức thanh toán trong cơ sở dữ liệu.
          * Cơ sở dữ liệu -> Hệ thống thanh toán: Xác nhận phương thức thanh toán đã được cập nhật thành công.
        * Nếu chọn "Gửi qua bưu điện":
          * Hệ thống yêu cầu nhân viên cung cấp địa chỉ gửi thư.
          * Hệ thống thanh toán -> Nhân viên: Yêu cầu địa chỉ gửi thư.
          * Nhân viên -> Hệ thống thanh toán: Nhân viên cung cấp địa chỉ gửi thư.
          * Hệ thống thanh toán -> Phương thức thanh toán: Đặt phương thức thanh toán là "Gửi qua bưu điện".
          * Hệ thống thanh toán -> Cơ sở dữ liệu: Cập nhật phương thức thanh toán trong cơ sở dữ liệu.
          * Cơ sở dữ liệu -> Hệ thống thanh toán: Xác nhận cập nhật thành công.
        * Nếu chọn "Chuyển khoản trực tiếp":
          * Hệ thống yêu cầu nhân viên cung cấp tên ngân hàng và số tài khoản.
          * Hệ thống thanh toán -> Nhân viên: Yêu cầu thông tin ngân hàng.
          * Nhân viên -> Hệ thống thanh toán: Nhân viên cung cấp tên ngân hàng và số tài khoản.
          * Hệ thống thanh toán -> Phương thức thanh toán: Đặt phương thức thanh toán là "Chuyển khoản trực tiếp".
          * Hệ thống thanh toán -> Cơ sở dữ liệu: Cập nhật phương thức thanh toán trong cơ sở dữ liệu.
          * Cơ sở dữ liệu -> Hệ thống thanh toán: Xác nhận cập nhật thành công.
        5.Bước 5: Xác nhận hoàn tất cập nhật.
          * Hệ thống thanh toán -> Nhân viên: Thông báo cho nhân viên rằng phương thức thanh toán đã được cập nhật thành công.
    * Luồng thay thế: Không tìm thấy nhân viên:
      * Trong trường hợp hệ thống không tìm thấy thông tin của nhân viên, biểu đồ mô tả một nhánh thay thế:
        * Hệ thống thanh toán -> Cơ sở dữ liệu: Hệ thống tìm kiếm thông tin của nhân viên dựa trên mã nhân viên.
        * Cơ sở dữ liệu -> Hệ thống thanh toán: Cơ sở dữ liệu phản hồi rằng không tìm thấy thông tin nhân viên.
        * Hệ thống thanh toán -> Nhân viên: Hệ thống hiển thị thông báo lỗi và dừng luồng xử lý.
      
  - Biểu đồ lớp:

  ![Diagram](https://www.planttext.com/api/plantuml/png/lLD1JiCm4Bpx5JvIGJvG8LH4a42Y5L83TzTP6gk94s9R8WBE277g2r0vSk41EGRnZ_m4XwHfd8HmmpVlZdT7C-E9a2IMAl0HzOaGUEPbR_oQPUyStoEippu4aHyc0EVs6CzbpFYoh4kDCIkVwpnz8PXwUVfiTYAI1C3b5AGNkcDysRoM204-K6aqzaRe4LMqZCQMMV1pSv88wcsx1uokhY8CTnAustrVuwQ4-R-YoYqQeKSVksuCGdGtsIpMp6s8Gi7ayAW5uQiP2S0KXr0QAYvdAbX0t9r_bgTFZfPqpPUEHxZdXDccDYU6MmMYfLlNiL69Lf5B9Fm5Fi13TZLCEVdQpFFqrSxJZmkcMQejgnl6tTDgxZ-mQMJMievQwJADE7omh2eRVPqY3NrrmsFKxqYnkgF807R76dM5R07GH8Ug-AJV)
  * giải thích:
    1.Các lớp trong biểu đồ:
      * NhânViên (Employee):
        * Thuộc tính:
          * employeeID: String: Mã nhân viên duy nhất.
          * tên: String: Tên của nhân viên.
          * địa chỉ: String: Địa chỉ của nhân viên.
          * paymentMethod: PaymentMethod: Phương thức thanh toán của nhân viên.
        * Phương thức:
          * selectPaymentMethod(): Phương thức cho phép nhân viên chọn phương thức thanh toán.
        * Giải thích: Lớp này đại diện cho nhân viên trong hệ thống, chứa các thông tin cá nhân và phương thức thanh toán của họ. Nhân viên có thể chọn hoặc thay đổi phương thức thanh toán thông qua phương thức selectPaymentMethod().
      * PaymentMethod (Phương thức thanh toán):
        * Thuộc tính:
          * methodType: String: Loại phương thức thanh toán (ví dụ: "Nhận tại chỗ", "Gửi qua bưu điện", "Chuyển khoản trực tiếp").
        * Giải thích: Đây là lớp trừu tượng cho các phương thức thanh toán khác nhau. Các lớp con sẽ kế thừa từ lớp này để cụ thể hóa các phương thức thanh toán khác nhau mà nhân viên có thể chọn.
      * PickUp (Nhận tại chỗ):
        * Lớp con của PaymentMethod, đại diện cho phương thức thanh toán mà nhân viên nhận lương trực tiếp tại công ty.
        * Giải thích: Lớp này không có thêm thuộc tính nào vì phương thức "Nhận tại chỗ" không yêu cầu thông tin bổ sung.
      * Mail (Gửi qua bưu điện):
        * Thuộc tính:
          * mailingAddress: String: Địa chỉ gửi thư mà nhân viên cung cấp để nhận lương.
        * Giải thích: Lớp này đại diện cho phương thức thanh toán mà nhân viên nhận lương qua đường bưu điện. Nó yêu cầu cung cấp địa chỉ gửi thư, thông tin này sẽ được lưu trong thuộc tính mailingAddress.
      * DirectDeposit (Chuyển khoản trực tiếp):
        * Thuộc tính:
          * bankName: String: Tên ngân hàng của nhân viên.
          * accountNumber: String: Số tài khoản ngân hàng của nhân viên.
        * Giải thích: Lớp này đại diện cho phương thức thanh toán mà lương của nhân viên được chuyển trực tiếp vào tài khoản ngân hàng. Nó yêu cầu thông tin về ngân hàng và số tài khoản, được lưu trong các thuộc tính bankName và accountNumber.
      * HệThốngThanhToán (Payment System):
        * Phương thức:
          * requestPaymentMethod(employeeID: String): Phương thức để yêu cầu nhân viên chọn phương thức thanh toán.
          * updatePaymentMethod(employeeID: String, paymentMethod: PaymentMethod): Phương thức để cập nhật phương thức thanh toán của nhân viên trong hệ thống.
        * Giải thích: Đây là lớp đại diện cho hệ thống xử lý các yêu cầu chọn và cập nhật phương thức thanh toán của nhân viên. Hệ thống này sẽ giao tiếp với cơ sở dữ liệu và nhân viên để thực hiện các tác vụ liên quan.
      * CơSởDữLiệu (Database):
        * Phương thức:
          * findEmployee(employeeID: String): Tìm nhân viên trong cơ sở dữ liệu dựa trên mã nhân viên.
          * updatePaymentMethod(employeeID: String, paymentMethod: PaymentMethod): Cập nhật phương thức thanh toán cho nhân viên trong cơ sở dữ liệu.
        * Giải thích: Đây là lớp đại diện cho cơ sở dữ liệu của hệ thống. Nó lưu trữ thông tin về nhân viên và phương thức thanh toán. Cơ sở dữ liệu sẽ được hệ thống thanh toán sử dụng để tìm kiếm và cập nhật thông tin.
  2.Quan hệ giữa 2 lớp:
  * NhânViên --> PaymentMethod: Quan hệ này cho thấy nhân viên có thể chọn một phương thức thanh toán.
  * PaymentMethod <|-- PickUp, Mail, DirectDeposit: Đây là quan hệ kế thừa. Các lớp PickUp, Mail, và DirectDeposit là các lớp con của lớp trừu tượng PaymentMethod, thể hiện các loại phương thức thanh toán khác nhau.
  * HệThốngThanhToán --> NhânViên: Hệ thống thanh toán tương tác với nhân viên để yêu cầu và nhận thông tin liên quan đến phương thức thanh toán.
  * HệThốngThanhToán --> CơSởDữLiệu: Hệ thống thanh toán tương tác với cơ sở dữ liệu để tìm kiếm và cập nhật thông tin về phương thức thanh toán của nhân viên.
  * CơSởDữLiệu --> NhânViên: Cơ sở dữ liệu lưu trữ thông tin của nhân viên và cung cấp lại khi cần thiết.
  
  - Nhiệm vụ của các lớp:
    * Nhân viên (Employee): Chịu trách nhiệm khởi tạo việc chọn phương thức thanh toán.
    * Hệ thống thanh toán (PaymentSystem): Chịu trách nhiệm điều phối các tương tác và cập nhật phương thức thanh toán.
    * Phương thức thanh toán (PaymentMethod): Lưu trữ các thông tin cụ thể về phương thức thanh toán đã chọn
    * Cơ sở dữ liệu (Database): Tương tác với hệ thống để tìm nhân viên và cập nhật thông tin phương thức thanh toán.


4.Phân tích ca sử dụng Maintain Timecard
- Các lớp phân tích
  * Nhân viên (Employee): Đại diện cho nhân viên sử dụng hệ thống để cập nhật và gửi bảng chấm công.
    * Thuộc tính: maNhanVien, ten, vaiTro
    * Nhiệm vụ: Khởi tạo quá trình chấm công, nhập và chỉnh sửa chi tiết bảng chấm công, gửi bảng chấm công.
    * Quan hệ: Liên kết với Bảng chấm công (một-nhiều) vì một nhân viên có thể có nhiều bảng chấm công.
  * Bảng chấm công (Timecard): Đại diện cho bảng chấm công cho kỳ trả lương hiện tại.
    * Thuộc tính: maBangChamCong, ngayBatDau, ngayKetThuc, trangThai, ngayGui
    * Nhiệm vụ: Lưu thông tin giờ làm việc cho từng ngày trong kỳ, cùng với số công việc và trạng thái gửi.
    * Quan hệ: Liên kết với Số công việc (nhiều-một), tham chiếu đến Nhân viên.
  * Số công việc (ChargeNumber): Đại diện cho các số công việc mà nhân viên có thể khai báo giờ làm việc.
    * Thuộc tính: maSoCongViec, maDuAn, soGioLam
    * Nhiệm vụ: Lưu mã dự án và số giờ đã khai báo cho số công việc đó.
    * Quan hệ: Liên kết với Dự án (nhiều-một) và Bảng chấm công (nhiều-một).
  * Hệ thống quản lý dự án (ProjectManagementSystem): Đại diện cho hệ thống chứa các số công việc dự án.
    * Thuộc tính: danhSachDuAn, trangThaiHoatDong
    * Nhiệm vụ: Cung cấp danh sách các số công việc có sẵn, xử lý lỗi nếu không khả dụng.
  * Hệ thống bảng chấm công (TimecardSystem): Đại diện cho hệ thống xử lý việc tạo, lưu và gửi bảng chấm công.
    * Thuộc tính: Không có (lớp hệ thống)
    * Nhiệm vụ: Xác thực và xử lý bảng chấm công, quản lý lưu trữ và chuyển đổi trạng thái (ví dụ, từ nháp sang đã gửi).
- Nhiệm vụ của các lớp phân tích
  * Nhân viên (Employee): Yêu cầu các hành động, khởi tạo chỉnh sửa và gửi bảng chấm công.
  * Hệ thống bảng chấm công (TimecardSystem): Quản lý vòng đời của bảng chấm công, xác thực dữ liệu, tương tác với cơ sở dữ liệu.
  * Hệ thống quản lý dự án (ProjectManagementSystem): Cung cấp dữ liệu dự án, xử lý lỗi khi dữ liệu không khả dụng.
  * Bảng chấm công (Timecard): Lưu thông tin về giờ làm việc và số công việc liên quan.
  * Số công việc (ChargeNumber): Liên kết giờ làm việc với các dự án.
- Biểu đồ Sequence
![Diagram](https://www.planttext.com/api/plantuml/png/ZLJDQi904BxlKmov-mA2HQgYq5PQzD0UuofkQBAAReIUU_3Ga_JGGobeHGg2eBIz99GUjlWU-oQTtQXKaqWl2SdCzpEpNvBl2xidVCybDXMT7bXL9byv31mvvnROVIYHBZOurEdQN81LKU4G15FXYBVXL0LJl56cWfa7L7xmji7KQqT0Lxv6WIk_eAZ25SX997HvSO1APTzlgo5jL4NNA4HDKw5AI2u7LGGlkj36mWjAZhrD3-Hof0IP4xIQXHivzu6guCq75xNke15N1gPn78GA9WwnNP4qFFoCv1HRmiTj8w20aSd2-V-eMsw_bSXDsNfffv7NyKKdaTnJuJEoyYQ9cd7DMozDcpFeomd5w-4IyU4TtmQVd202CfjL5Oz0o6QZsVIj3-Gu0LVNi48r2mK3unej4j2KTP9KyPZBAbmuYUEdd7Cx3K1sAxt6AKreLpJknIIW77E8tTCkXYWS_ZsSlR543uyAHGMJ4qU3mJAfCOwilS2ibtwYHHEHkFWjYz8Kzr32W4J5XiIOOEqfYeJUtPaAleMs4vYlOeUe8EizDnjuqEKTd7Nas4Hm9aWUMVJFunS0)
* giải thích:
  * Các bước trong biểu đồ Sequence
    Nhập giờ làm việc
    1.Yêu cầu lấy bảng chấm công hiện tại:
      * Nhân viên gửi yêu cầu đến hệ thống bảng chấm công để lấy thông tin về bảng chấm công hiện tại.
    2.Kiểm tra tồn tại bảng chấm công:
      * Bảng chấm công đã tồn tại: Hệ thống sẽ lấy bảng chấm công hiện tại.
      * Bảng chấm công chưa tồn tại: Hệ thống sẽ tạo một bảng chấm công mới cho nhân viên.
    3.Lấy danh sách số công việc:
      * Hệ thống bảng chấm công gửi yêu cầu đến hệ thống quản lý dự án để lấy danh sách các số công việc có sẵn.
      * Hệ thống quản lý dự án phản hồi lại danh sách các số công việc cho hệ thống bảng chấm công.
    4.Nhập giờ làm việc và số công việc:
      * Nhân viên nhập giờ làm việc cho từng số công việc mà họ đã thực hiện. Hệ thống cập nhật giờ làm việc tương ứng với các số công việc đã được gán.
    Gửi bảng chấm công
    5.Yêu cầu gửi bảng chấm công:
      * Nhân viên yêu cầu hệ thống gửi bảng chấm công đã nhập.
    6.Xác thực giờ làm việc:
      * Hệ thống bảng chấm công xác thực giờ làm việc nhập vào
      * Giờ làm việc hợp lệ: Nếu giờ làm việc được xác thực là hợp lệ, hệ thống sẽ cập nhật trạng thái của bảng chấm công thành "đã gửi" và lưu bảng chấm công.
      * Giờ làm việc không hợp lệ: Nếu giờ làm việc không hợp lệ, hệ thống sẽ gửi thông báo lỗi cho nhân viên.
    Kết thúc
    7.Hiển thị thông báo hoàn tất:
      * Sau khi gửi thành công hoặc nếu có lỗi xảy ra, hệ thống sẽ hiển thị thông báo cho nhân viên biết về tình trạng của bảng chấm công (đã gửi hoặc có lỗi).

- Biểu đồ lớp:
![Diagram](https://www.planttext.com/api/plantuml/png/ZLFBIiD05DtdAovTAT8V259g6wXWAKYA--iqP0QIcPhCb1PnxTeFS575XO8BmLM3gtp9_8apZJQFFkWix-ESSy-zqqqb9bB5aKCYJ5DeZMZ-mc76yW_UWdi7p3kBSKIHtx20k-3BXF6mYCjcO8Pid8XwZ5ES3fZEdeSO6mXoSkh2JB19aGqLwo4F-nJZlk3X9ldW_0lgwTKh3u5GlNgBWUJVFAm8gpOtU4DSz51wg5pmK0Pbz2gGOwf8DImJWpEcoBOn1efLAmbEZHbom4HpdbAohxEdGganKcUFf2BO5rQgRQnJnUYfYuQTgIXeN61SLl0l3IP8QbWCmxMo1a6K_u2dio_8b-NY9iYfZuGMq4EMolC2lrFa8rnuVoMbBZjGhNGFyQ75H4hjNEiwEKTY_QzRU2j4522tPFKRRBSxku_01QbNI-7ilf1rs_OYN5kMlQDpX8dx8PIbCsnJvmXB3Wnmjw4z6gZgZGjpdNsksvrkm2Vs6_y2)

  * giải thích
      * Các lớp trong biểu đồ
        Lớp 1: Nhân viên
          * Mô tả: Đại diện cho người lao động trong hệ thống, những người nhập giờ làm việc và gửi bảng chấm công.
          * Thuộc tính:
              * maNhanVien: Mã định danh của nhân viên, giúp phân biệt giữa các nhân viên.
              * ten: Tên của nhân viên, được sử dụng để hiển thị trong giao diện người dùng.
              * vaiTro: Chỉ định vai trò của nhân viên, có thể là "hourly" (theo giờ) hoặc "salaried" (lương cố định).
          * Phương thức:
              * nhapGiờLamViec(): Cho phép nhân viên nhập giờ làm việc cho bảng chấm công.
              * guiBangChamCong(): Cho phép nhân viên gửi bảng chấm công sau khi đã nhập thông tin cần thiết.
        Lớp 2: Bảng chấm công
          * Mô tả: Lớp này đại diện cho bảng chấm công của một nhân viên trong một khoảng thời gian nhất định.
          * Thuộc tính:
            * maBangChamCong: Mã định danh cho bảng chấm công.
            * ngayBatDau: Ngày bắt đầu của kỳ bảng chấm công.
            * ngayKetThuc: Ngày kết thúc của kỳ bảng chấm công.
            * trangThai: Trạng thái của bảng chấm công (ví dụ: "đã gửi", "chưa gửi").
            * ngayGui: Ngày mà bảng chấm công được gửi.
          * Phương thức:
            * capNhatGioLamViec(): Cho phép cập nhật giờ làm việc cho bảng chấm công, bao gồm các số công việc mà nhân viên đã làm.
        Lớp 3: Số công việc
          * Mô tả: Đại diện cho từng công việc cụ thể mà nhân viên đã thực hiện trong kỳ bảng chấm công.
          * Thuộc tính:
            * maSoCongViec: Mã định danh cho số công việc.
            * maDuAn: Mã của dự án mà số công việc thuộc về.
            * soGioLam: Số giờ mà nhân viên đã làm cho công việc đó.
        Lớp 4: Hệ thống Quản lý Dự án
          * Mô tả: Lớp này quản lý các dự án và cung cấp thông tin cho nhân viên về số công việc có sẵn.
          * Thuộc tính:
            * danhSachDuAn: Danh sách các dự án đang có trong hệ thống.
          * Phương thức:
            * layDanhSachSoCongViec(): Phương thức để lấy danh sách số công việc có sẵn từ cơ sở dữ liệu cho nhân viên lựa chọn khi nhập giờ làm việc.
        Lớp 5: Hệ thống Bảng chấm công
          * Mô tả: Lớp này xử lý tất cả các hoạt động liên quan đến bảng chấm công.
          * Phương thức:
            * luuBangChamCong(): Phương thức để lưu bảng chấm công vào cơ sở dữ liệu sau khi nhân viên gửi.
            * xacThucGioLamViec(): Phương thức để kiểm tra tính hợp lệ của giờ làm việc mà nhân viên nhập vào.
        3. Mối quan hệ giữa các lớp
          * Nhân viên tạo Bảng chấm công: Mỗi nhân viên có thể tạo bảng chấm công riêng cho mình.
          * Bảng chấm công chứa nhiều Số công việc: Mỗi bảng chấm công có thể có nhiều số công việc liên quan.
          * Hệ thống Quản lý Dự án cung cấp danh sách Số công việc: Hệ thống quản lý dự án cung cấp thông tin cho nhân viên về các số công việc mà họ có thể ghi lại trong bảng chấm công.
          * Hệ thống Bảng chấm công quản lý Bảng chấm công: Hệ thống này có trách nhiệm lưu trữ và xử lý tất cả các hoạt động liên quan đến bảng chấm công.
          * Hệ thống Bảng chấm công xử lý yêu cầu từ Nhân viên: Hệ thống sẽ tiếp nhận và xử lý các thao tác mà nhân viên thực hiện liên quan đến bảng chấm công.
