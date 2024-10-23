1. phân tích kiến trúc của bài toán Payroll System:
- Người dùng hệ thống:
  + Nhân viên :
    * Ghi nhận bảng chấm công (dành cho nhân viên hưởng lương theo giờ và nhân viên hưởng lương cố định).
    * Nhập đơn đặt hàng (cho nhân viên hưởng hoa hồng).
    * Thay đổi phương thức thanh toán (chuyển khoản, gửi qua bưu điện, hoặc nhận trực tiếp).
    * Xem báo cáo: tổng số giờ làm việc, tiền lương đã nhận, thời gian nghỉ còn lại, giờ làm việc theo dự án.
- Yêu cầu chức năng:
  + Quản lý thời gian làm việc:
    * Đối với nhân viên làm thêm giờ (>8 giờ), cần tính lương với mức tăng 1.5 lần so với lương cơ bản.
    * Nhân viên cần ghi nhận thời gian làm việc (bảng chấm công) cho các dự án mà họ tham gia. Bảng chấm công này phải ghi rõ mã số dự án (charge number), số giờ làm việc mỗi ngày.
  + Quản lý đơn đặt hàng:
    * Nhân viên hưởng hoa hồng cần ghi nhận đơn đặt hàng với các thông tin: ngày tháng, giá trị bán hàng.
  + Tính lương:
    * Nhân viên theo giờ: Tính lương theo số giờ làm việc và số giờ làm thêm.
    * Nhân viên hưởng lương cố định: Lương cố định được trả vào ngày làm việc cuối cùng của tháng.
    * Nhân viên hưởng hoa hồng: Ngoài lương cố định, họ còn nhận thêm hoa hồng dựa trên doanh số bán hàng.
  + Tích hợp với cơ sở dữ liệu dự án (DB2):
    * Hệ thống cần truy cập (chỉ đọc) vào cơ sở dữ liệu DB2 để lấy thông tin mã số dự án (charge number) phục vụ cho việc ghi nhận bảng chấm công.
  - Yêu cầu phi chức năng:
    + Hệ thống phải đảm bảo tính bảo mật thông tin, chỉ cho phép nhân viên truy cập vào dữ liệu cá nhân của họ (bảng chấm công, đơn đặt hàng). Quản trị viên có quyền truy cập vào dữ liệu của tất cả nhân viên.
    + Hệ thống phải có khả năng mở rộng để hỗ trợ thêm nhiều nhân viên hơn (hiện tại Acme có khoảng 5,000 nhân viên trên toàn cầu).
    + Hệ thống cần tự động hóa quá trình tính lương, tránh sự can thiệp thủ công trong các hoạt động hàng ngày.

  - Đề xuất kiến trúc nhiều lớp (multi-tier),Kiến trúc này sẽ chia hệ thống thành ba lớp chính:
  a.Lớp trình bày (Client)
    * Ứng dụng Desktop dựa trên Windows: Đây sẽ là giao diện người dùng cho nhân viên và quản trị viên,Nó sẽ cho phép người dùng:
      * Nhập bảng chấm công hoặc đơn đặt hàng.
      * Thay đổi phương thức thanh toán.
      * Xem báo cáo (giờ làm việc, tiền lương đã nhận, v.v.).
      Lý do lựa chọn: Ứng dụng desktop phù hợp để đảm bảo giao diện phản hồi nhanh và tích hợp dễ dàng với hạ tầng Windows sẵn có. Nó đảm bảo tính bảo mật vì người dùng có thể truy cập dữ liệu trên máy tính của họ với các cơ chế xác thực phù hợp.
  b. Lớp logic nghiệp vụ (Service Layer)
    * Dịch vụ tính lương: Dịch vụ này sẽ xử lý các quy tắc nghiệp vụ và tính toán lương.
    * Xử lý bảng chấm công và đơn đặt hàng: Dịch vụ xử lý các bảng chấm công và đơn đặt hàng, đảm bảo ghi nhận đúng mã số dự án và dữ liệu bán hàng.
    * Quản lý nhân viên: Dịch vụ quản lý việc thêm, xóa và cập nhật thông tin nhân viên.
    * Lớp bảo mật: Xác thực và phân quyền để đảm bảo rằng nhân viên chỉ có thể truy cập vào dữ liệu của họ.
    Lý do lựa chọn: Việc tách logic nghiệp vụ ra khỏi giao diện người dùng giúp hệ thống dễ bảo trì, mở rộng và đảm bảo tính bảo mật. Các quy tắc nghiệp vụ như tính lương có thể thay đổi mà không ảnh hưởng đến giao diện.
  c. Lớp dữ liệu (Database và Integration Layer)
    * Tích hợp DB2: Vì Acme sử dụng cơ sở dữ liệu DB2 cho quản lý dự án, hệ thống sẽ bao gồm một kết nối DB2 để đọc mã số dự án. Điều này sẽ chỉ cho phép đọc để đảm bảo tính toàn vẹn của hệ thống cũ.
    * Cơ sở dữ liệu Payroll: Một cơ sở dữ liệu riêng (như SQL Server hoặc MySQL) để lưu trữ dữ liệu nhân viên, bảng chấm công, đơn đặt hàng, phương thức thanh toán và báo cáo.
    Lý do lựa chọn: Kết nối DB2 đảm bảo tương thích với hệ thống quản lý dự án cũ. Việc sử dụng cơ sở dữ liệu riêng cho trả lương đảm bảo hệ thống độc lập, dễ mở rộng và phù hợp với các yêu cầu xử lý trả lương.
  d. Hệ thống xử lý hàng loạt
    * Tự động tạo bảng lương: Một công việc hoặc dịch vụ định kỳ
    Lý do lựa chọn: Điều này đảm bảo việc trả lương diễn ra đều đặn và đáng tin cậy mà không cần can thiệp thủ công, tăng hiệu quả hoạt động.

Biểu đồ mô tả kiến trúc:
![Diagram](https://www.planttext.com/api/plantuml/png/dPI_Qi9G5CRtFCN1fPDB7w282dM8GccGGd48gUI6rfjKaw6KpWukTIobr5I4jbIquD9SEbt9UvmtwKcqQlqRco4XET_vVT_XN99fk7hTzMfNYkBengLkLY6bedXqGAlB3yuWApitARXLWAvp5A_SX3oANlIeDYvTswbCIiUMRaFUGj7aK3B38Oed2_BoYviLc2YASfGUjprEID-67DqgojsAhMgRba445g4SA9FNp9wCMmQBlu4c-vHE3OUXFxTO59oXw8Cglo6BGPVYvXW6lHhvZaY_AZ_n8bdSK6BoXKFPakzyAfn4Yq0s537ek-kI4sq0QXJRcoMcb6HGGD6bUlPmYik5FfoYU5viMHhe3v_wcw0n56sQOpPNGNReLRKnyDsv8OfF6D-ZUA2QHYNxT_v5ya4iQP4VDzduJOSQiIsuzdGEB38pATD01qFw1anZAEkt3-dqqRRD96xRlg-ab4rht7PsOaYIdIJF-q_cXlHxq9OFt3wjEu_EpbqhcIg_Y7ucFm00)



2. Cơ chế cần có và phân tích.
Các cơ chế sau đây cần được giải quyết trong hệ thống trả lương mới:
a. Xác thực và phân quyền
  * Tại sao: Nhân viên chỉ có thể truy cập vào bảng chấm công và đơn đặt hàng của riêng họ, và quản trị viên phải kiểm soát được dữ liệu nhân viên.
  * Cách thực hiện: Triển khai kiểm soát truy cập dựa trên vai trò (RBAC) với xác thực bảo mật.
b. Tính toán lương.
  * Tại sao: Hệ thống phải xử lý các phương thức thanh toán khác nhau: tính theo giờ với làm thêm giờ, nhân viên hưởng lương và nhân viên hưởng hoa hồng.
  * Cách thực hiện: Thiết kế các thuật toán tính:
    * Lương theo giờ bao gồm làm thêm giờ (nếu làm hơn 8 giờ).
    * Lương cố định hàng tháng.
    * Hoa hồng dựa trên doanh số với tỷ lệ hoa hồng khác nhau (10%, 15%, v.v.).
c. Tích hợp cơ sở dữ liệu.
  * Tại sao: Hệ thống cần truy cập vào cơ sở dữ liệu DB2 để lấy mã số dự án nhưng không được phép thay đổi nó.
  * Cách thực hiện: Sử dụng lớp tích hợp chỉ đọc để truy cập DB2 và xác minh mã số dự án khi nhân viên ghi chép giờ làm việc.
d. Xác minh bảng chấm công và đơn đặt hàng.
  * Tại sao: Bảng chấm công và đơn đặt hàng phải được xác minh để đảm bảo chúng liên quan đến các dự án và dữ liệu bán hàng chính xác.
  * Cách thực hiện: Triển khai logic xác minh ở lớp dịch vụ để kiểm tra xem bảng chấm công có khớp với mã số dự án hợp lệ hay không và dữ liệu bán hàng được nhập chính xác.
e. Lựa chọn phương thức thanh toán.
  * Tại sao: Nhân viên có thể chọn giữa chuyển khoản trực tiếp, gửi qua bưu điện hoặc nhận tại văn phòng.
  * Cách thực hiện: Lưu trữ các tùy chọn thanh toán của nhân viên và triển khai logic để thực hiện trả lương theo phương thức đã chọn.
f. Hệ thống báo cáo.
  * Tại sao: Nhân viên cần truy cập vào giờ làm việc, thanh toán dự án, lịch sử lương và thời gian nghỉ phép còn lại.
  * Cách thực hiện: Tạo dịch vụ báo cáo, truy vấn cơ sở dữ liệu trả lương và cung cấp các báo cáo được lọc theo yêu cầu của nhân viên.
g. Xử lý hàng loạt cho việc thanh toán.
  * Tại sao: Hệ thống trả lương phải tự động chạy vào thứ Sáu và cuối tháng.
  * Cách thực hiện: Sử dụng các tác vụ hoặc dịch vụ định kỳ kích hoạt tính toán bảng lương và tạo thanh toán dựa trên dữ liệu bảng chấm công và đơn đặt hàng kể từ lần thanh toán trước.

Các cơ chế mong đợi:
  1.Xác thực và phân quyền (RBAC).
  2.Tính toán lương (theo giờ, lương cố định, hoa hồng).
  3.Tích hợp cơ sở dữ liệu (truy cập chỉ đọc DB2).
  4.Xác minh bảng chấm công và đơn đặt hàng.
  5.Lựa chọn phương thức thanh toán (chuyển khoản, bưu điện, nhận tại văn phòng).
  6.Hệ thống báo cáo.
  7.Xử lý hàng loạt (tự động tạo bảng lương).



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
