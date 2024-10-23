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




