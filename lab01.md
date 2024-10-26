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
  * Cơ chế ghi nhận thời gian và thanh toán
    * Nhân viên theo giờ:
      * Ghi nhận số giờ làm việc hàng ngày.
      * Tính lương theo công thức: Lương = Giờ làm việc bình thường * Mức lương theo giờ + Giờ làm thêm * Mức lương theo giờ * 1.5.
      * Lương được trả hàng tuần vào mỗi thứ Sáu.
    * Nhân viên lương cố định:
      * Ghi nhận số giờ làm việc cho các mục đích theo dõi.
      * Lương cố định được trả vào ngày làm việc cuối cùng của tháng.
    * Nhân viên có hoa hồng:
      * Ghi nhận đơn đặt hàng mua để tính hoa hồng.
      * Tính toán hoa hồng dựa trên tỷ lệ phần trăm đã được xác định (10%, 15%, 25%, 35%).
      * Tổng hợp thanh toán cho cả lương và hoa hồng.
  * Báo cáo và truy vấn thông tin
    Nhân viên có thể truy vấn các thông tin:
      * Số giờ làm việc.
      * Tổng giờ làm việc đã lập hóa đơn cho từng dự án.
      * Tổng lương nhận được từ đầu năm.
      * Thời gian nghỉ còn lại.
  * Tự động hóa quy trình thanh toán
    * Hệ thống sẽ tự động chạy vào mỗi thứ Sáu và ngày làm việc cuối cùng của tháng.
    * Hệ thống sẽ tổng hợp dữ liệu từ lần thanh toán trước đó đến ngày thanh toán hiện tại để tính toán chính xác.
      
3.Phân tích ca sử dụng Payment
-  Xác Định Các Lớp Phân Tích:
  * Employee:
    * Đại diện cho một nhân viên trong tổ chức.
    * Thuộc tính: EmployeeID, Tên, Địa chỉ, v.v.
  * PaymentMethod:
    * Đại diện cho một tùy chọn phương thức thanh toán.
    * Thuộc tính: MethodID, MethodName (ví dụ: "Lấy tại chỗ", "Gửi qua bưu điện", "Chuyển khoản trực tiếp").
  * PaymentInformation:
    * Lưu trữ thông tin cụ thể về phương thức thanh toán đã chọn.
    * Thuộc tính: PaymentMethodID, Địa chỉ (đối với "Gửi qua bưu điện"), Tên ngân hàng, Số tài khoản (đối với "Chuyển khoản trực tiếp").

- Biểu đồ sequence

  ![Diagram](https://www.planttext.com/api/plantuml/png/fP7DIiD04CVlUOev9X1Ve0SljI08KcZmlWqX6qmdjTq80Gy53r94i7U5L16n85Jmb1myRCbxR9_4IMqfwaPQcrCsa__7_BRHwOPHudcmjmzDbdGy52slTnwu7jJ0vIg_mIOlfiKOVOTEBwx36N8dacCqBUE7WZmQAxyQ978IWwkovxYkyI5rOzjiSpwu4psLlxaW0fLzT06vobvnFhY72w3XMSoWNKnZc8q2bL-j1owF4vLV8fpoI6MFvS0o31OA2CcEBTCWb2bKPnX0p--L3vXWRTPVbAO_bmAnSNKgueKarpNHB5JHGWy-Hh-kigVkx5RbrwoyXo61Bnt-Xg_JpKRb-wBWqDgLXMOn6xudL5FHtEQ_g1pxyW4FPOh-YKnRAXyvCWsElZotmkHm-KxR1RXsD8MS861v1bv-iA5F2I0eOjLxPueQ43Pi5Fm9)
  
 Biểu đồ chuỗi thể hiện các bước sau:

  * Nhân viên gửi yêu cầu cho hệ thống để chọn phương thức thanh toán
  * Hệ thống trình bày các tùy chọn phương thức thanh toán có sẵn cho nhân viên
  * Nhân viên chọn một phương thức thanh toán và cung cấp thông tin cần thiết (nếu có)
  * Hệ thống xác thực thông tin đầu vào và cập nhật thông tin thanh toán của nhân viên
  
- Biểu đồ lớp:

  ![Diagram](https://www.planttext.com/api/plantuml/png/lLD1JiCm4Bpx5JvIGJvG8LH4a42Y5L83TzTP6gk94s9R8WBE277g2r0vSk41EGRnZ_m4XwHfd8HmmpVlZdT7C-E9a2IMAl0HzOaGUEPbR_oQPUyStoEippu4aHyc0EVs6CzbpFYoh4kDCIkVwpnz8PXwUVfiTYAI1C3b5AGNkcDysRoM204-K6aqzaRe4LMqZCQMMV1pSv88wcsx1uokhY8CTnAustrVuwQ4-R-YoYqQeKSVksuCGdGtsIpMp6s8Gi7ayAW5uQiP2S0KXr0QAYvdAbX0t9r_bgTFZfPqpPUEHxZdXDccDYU6MmMYfLlNiL69Lf5B9Fm5Fi13TZLCEVdQpFFqrSxJZmkcMQejgnl6tTDgxZ-mQMJMievQwJADE7omh2eRVPqY3NrrmsFKxqYnkgF807R76dM5R07GH8Ug-AJV)
  
 Biểu đồ lớp thể hiện các mối quan hệ sau:
  * Employee có mối quan hệ một-một với PaymentInformation. Điều này có nghĩa là mỗi nhân viên chỉ có một phương thức thanh toán đang hoạt động.
  * PaymentInformation có mối quan hệ một-một với PaymentMethod. Điều này có nghĩa là mỗi bản ghi thông tin thanh toán được liên kết với một phương thức thanh toán cụ thể.
- Trách Nhiệm của Các Lớp
  * Employee:
    * Cung cấp sự lựa chọn phương thức thanh toán.
    * Cung cấp thông tin bổ sung khi cần thiết (ví dụ: địa chỉ, chi tiết ngân hàng).
  * PaymentMethod:
    * Lưu trữ các tùy chọn phương thức thanh toán (ví dụ: "Lấy tại chỗ", "Gửi qua bưu điện", "Chuyển khoản trực tiếp").
  * PaymentInformation:
    * Lưu trữ chi tiết thanh toán cụ thể cho phương thức đã chọn.
    * Xác thực thông tin được cung cấp.

4.Phân tích ca sử dụng Maintain Timecard
- Xác Định Các Lớp Phân Tích:
  * Employee: Đại diện cho một nhân viên có thể gửi thời gian làm việc.
  * Timecard: Đại diện cho một thời gian làm việc với các chi tiết như ngày bắt đầu, ngày kết thúc, ngày gửi và danh sách các mục thời gian làm việc.
  * TimecardEntry: Đại diện cho một mục đơn lẻ trên thời gian làm việc, bao gồm ngày, số giờ làm việc và số dự án liên quan.
  * Project: Đại diện cho một dự án với một số dự án duy nhất.

- Biểu đồ Sequence
![Diagram](https://www.planttext.com/api/plantuml/png/ZLJDQi904BxlKmov-mA2HQgYq5PQzD0UuofkQBAAReIUU_3Ga_JGGobeHGg2eBIz99GUjlWU-oQTtQXKaqWl2SdCzpEpNvBl2xidVCybDXMT7bXL9byv31mvvnROVIYHBZOurEdQN81LKU4G15FXYBVXL0LJl56cWfa7L7xmji7KQqT0Lxv6WIk_eAZ25SX997HvSO1APTzlgo5jL4NNA4HDKw5AI2u7LGGlkj36mWjAZhrD3-Hof0IP4xIQXHivzu6guCq75xNke15N1gPn78GA9WwnNP4qFFoCv1HRmiTj8w20aSd2-V-eMsw_bSXDsNfffv7NyKKdaTnJuJEoyYQ9cd7DMozDcpFeomd5w-4IyU4TtmQVd202CfjL5Oz0o6QZsVIj3-Gu0LVNi48r2mK3unej4j2KTP9KyPZBAbmuYUEdd7Cx3K1sAxt6AKreLpJknIIW77E8tTCkXYWS_ZsSlR543uyAHGMJ4qU3mJAfCOwilS2ibtwYHHEHkFWjYz8Kzr32W4J5XiIOOEqfYeJUtPaAleMs4vYlOeUe8EizDnjuqEKTd7Nas4Hm9aWUMVJFunS0)

- Biểu đồ lớp:

![Diagram](https://www.planttext.com/api/plantuml/png/ZLFBIiD05DtdAovTAT8V259g6wXWAKYA--iqP0QIcPhCb1PnxTeFS575XO8BmLM3gtp9_8apZJQFFkWix-ESSy-zqqqb9bB5aKCYJ5DeZMZ-mc76yW_UWdi7p3kBSKIHtx20k-3BXF6mYCjcO8Pid8XwZ5ES3fZEdeSO6mXoSkh2JB19aGqLwo4F-nJZlk3X9ldW_0lgwTKh3u5GlNgBWUJVFAm8gpOtU4DSz51wg5pmK0Pbz2gGOwf8DImJWpEcoBOn1efLAmbEZHbom4HpdbAohxEdGganKcUFf2BO5rQgRQnJnUYfYuQTgIXeN61SLl0l3IP8QbWCmxMo1a6K_u2dio_8b-NY9iYfZuGMq4EMolC2lrFa8rnuVoMbBZjGhNGFyQ75H4hjNEiwEKTY_QzRU2j4522tPFKRRBSxku_01QbNI-7ilf1rs_OYN5kMlQDpX8dx8PIbCsnJvmXB3Wnmjw4z6gZgZGjpdNsksvrkm2Vs6_y2)
- Mô Tả và Trách Nhiệm Các Lớp
  * Employee:
    * Xem và chỉnh sửa thời gian làm việc của riêng mình.
    * Thêm, sửa đổi hoặc xóa các mục thời gian làm việc.
    * Gửi thời gian làm việc để phê duyệt.
    * Thuộc tính: Mã nhân viên, Tên, Vai trò.
  * Timecard:
    * Lưu trữ thông tin về một kỳ thanh toán cụ thể.
    * Duy trì danh sách các mục thời gian làm việc.
    * Tính tổng số giờ làm việc.
    * Thuộc tính: Ngày bắt đầu, Ngày kết thúc, Ngày gửi, Trạng thái.
  * TimecardEntry:
    * Đại diện cho một ngày làm việc đơn lẻ.
    * Lưu trữ ngày, số giờ làm việc và số dự án.
    * Thuộc tính: Ngày, Số giờ làm việc, Số dự án.
  * Project:
    * Lưu trữ thông tin về một dự án.
    * Thuộc tính: Mã dự án, Tên, Số dự án.
- Mối Quan Hệ
    * Employee đến Timecard: Mối quan hệ một-nhiều. Một nhân viên có thể có nhiều thời gian làm việc.
    * Timecard đến TimecardEntry: Mối quan hệ một-nhiều. Một thời gian làm việc có thể có nhiều mục thời gian làm việc.
    * TimecardEntry đến Project: Mối quan hệ nhiều-một. Nhiều mục thời gian làm việc có thể được liên kết với một dự án.
  
5.Hợp nhất 2 ca sử dụng Payment và Maintain Timecard
- tài liệu:
  * Giới thiệu
   * Tài liệu này mô tả ca sử dụng hợp nhất giữa việc "Duy Trì Thời Gian Làm Việc" và "Chọn Phương Thức Thanh Toán". Quy trình này cho phép nhân viên nhập thông tin thời gian làm việc, chọn phương thức thanh toán cho lương, và gửi thông tin đó cho hệ thống để xử lý.
  * Mục tiêu
   * Mục tiêu của ca sử dụng này là cung cấp một cách thức hiệu quả để nhân viên quản lý thời gian làm việc của họ và lựa chọn phương thức nhận lương, đồng thời đảm bảo thông tin được lưu trữ một cách chính xác và an toàn.
  * Các bên liên quan
      * Nhân viên: Người sử dụng hệ thống để nhập giờ làm việc và chọn phương thức thanh toán.
      * Hệ thống: Phần mềm quản lý thời gian làm việc và thanh toán.
      * Cơ sở dữ liệu: Lưu trữ thông tin về thời gian làm việc và phương thức thanh toán.
  * Các lớp phân tích
    * Employee:
      * Mô tả: Đại diện cho nhân viên trong tổ chức.
      * Thuộc tính: EmployeeID, Tên, Địa chỉ, v.v.
    * Timecard:
      * Mô tả: Đại diện cho thời gian làm việc của nhân viên.
      * Thuộc tính: StartDate, EndDate, SubmittedDate, Status.
    * TimecardEntry:
      * Mô tả: Đại diện cho một mục thời gian làm việc cụ thể.
      * Thuộc tính: Date, HoursWorked, ChargeNumber.
    * PaymentMethod:
      * Mô tả: Đại diện cho các tùy chọn phương thức thanh toán.
      * Thuộc tính: MethodID, MethodName.
    * PaymentInformation:
      * Mô tả: Lưu trữ thông tin chi tiết về phương thức thanh toán đã chọn.
      * Thuộc tính: PaymentMethodID, Address (đối với "Gửi qua bưu điện"), BankName, AccountNumber (đối với "Chuyển khoản trực tiếp").
  * Quy Trình Thực Hiện
      * Nhập Thời Gian Làm Việc
          * Nhân viên gửi yêu cầu để xem thời gian làm việc.
          * Hệ thống lấy thời gian làm việc hiện tại và hiển thị cho nhân viên.
          * Nhân viên nhập giờ làm việc cho các ngày cụ thể và gán cho các dự án.
          * Nhân viên gửi thông tin thời gian làm việc cho hệ thống.
          * Hệ thống lưu thông tin giờ làm việc và gửi yêu cầu xác nhận gửi.
          * Hệ thống nhận được kết quả và thông báo cho nhân viên.
      * Chọn Phương Thức Thanh Toán
          * Nhân viên gửi yêu cầu chọn phương thức thanh toán.
          * Hệ thống lấy danh sách phương thức thanh toán có sẵn và hiển thị cho nhân viên.
          * Nhân viên chọn phương thức thanh toán và nhập thông tin cần thiết (ví dụ: địa chỉ hoặc chi tiết tài khoản ngân hàng).
          * Hệ thống lưu thông tin thanh toán và xác thực thông tin.
          * Hệ thống thông báo kết quả xác thực cho nhân viên.
          - sơ đồ hợp nhất
          Biểu đồ chuỗi mô tả quy trình tương tác giữa các đối tượng liên quan (Nhân viên, Hệ thống, Thời gian làm việc, Phương thức thanh toán, và Thông tin thanh toán) trong quy trình này đã được cung cấp trước đó.
  ![Diagram](https://www.planttext.com/api/plantuml/png/hPL1IiD058RtESMxW1SeH8GksaKH5qNSXPYMp60xZUcaj4jnuK9SU812enInMAZWAbcu6EWzvYREoxIQD4rCnTMGp9ll_VFFFyd9aDi-3_ezmtRum8TJ8fwOTKx5WXcUcg4EzMp6eIdZEwjDPFp8tCm4lfjIgq5jPhN4hA_Mgb8dKtD6rgjroDm4yEgemMR4QWz4o7POuWEhACkK6AQfTCbewMsw0NO3x0Qw6vYcaL8EHySPmJLeo7X9OI8M0JWoUWbWtEq3rp0NOJs4ZRJqz5945Mon1YqPpIT5620uCE0o2gbHmCWhRVioUeQHZ6ygwqljl0PDLNFBahNtXOHTimftkB7TLO_J3A62spmvJU2K_OFJwrMG1kO5LKRbeo121nXkL75nya-7TIMZJmxN0RxG58njNLK-iEyB0Fe2G99kID9zU7GTpVIsJAwPF67ovCRicb5o0RSfxk3geRA2b99BIANs5ECFY7EPi78Jwxzw1YCnqodmKa7R4wjzdwXyGCjEeuXnWK54PUzEMiB-yifX7FzGZNAlTIEu5E40o43Cac_N73aY1EwBDqQCaoxhuD_Y2m00)
  
