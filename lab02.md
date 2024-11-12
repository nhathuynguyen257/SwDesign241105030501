
# LAB 2 PHÂN TÍCH CÁC CA SỬ DỤNG TRONG PAYROLL SYSTEM

## Phân tích ca sử dụng Create Administrative Report
### 1. Mô tả
   
Ca sử dụng Create Administrative Report cho phép Payroll Administrator tạo báo cáo quản trị dựa trên các tiêu chí về giờ làm việc hoặc tổng thu nhập năm cho nhân viên.

### 2. Xác định các lớp

Boundary Classes:
  - PaymentSelectionUI: giao diện cho người dùng chọn hình thức thanh toán.
Control Classes:
  - PaymentController: lớp điều khiển xử lý lựa chọn hình thức thanh toán.
Entity Classes:
  - PaymentMethod: lớp chứa các phương thức thanh toán.
  - Employee: thực thể chứa thông tin về nhân viên (để liên kết thanh toán với nhân viên cụ thể).
  - Payment: thực thể chứa thông tin thanh toán cụ thể (tiền lương, hình thức thanh toán).

### 3. Biểu đồ tuần tự
![CreateAdministrativeReport](https://www.planttext.com/api/plantuml/png/f9InJkD048RxVOefEGbU8BeWWf5yI5T43XIKZhEALonhv8mZkJnTGK6LYkAQS152GegWeB8BYaMynpu1ht2pkyab4LPq9roil3kVv_zdTkJt-kLWX76EnOLaB4umow4RbtacPMTm8PGOOHxRmtW4tGvZ_QnGWpWl6w7JOukT7hCaKqXHYFXbbcFWTvAxB570k4A1vI8QSiN_IaJXPj2TRHxr28s7t4LwZBNRS6AgsmmEDIs9NTfjrkt0tZuvQS6PVYWWCTLz0UYu_f9ZP9UWADW6mTZKlmIWS4Igvx0ZCqB42ja5DTJJ4lgcUaHudTWqoxDpKxqWOAghP1TGFoXGgVwjO4pvr1SM1Sv1s5hKi99Tg7mYDuibmf6f7q4AKryLa9fwTWcItXdG4uLEUobDkUi95VhsH9WQhhN9mG5ytVD6SrFDR5T-hBbzdUYPxvoZgR6MfiP-8-cVYaoQ-dej9PSZlk7jFDNF9DfiaVAS-BZBGE4RiKq7Fy1S3ToaV7zxAlvXKAJ5ckOaDFLSGBb9Bc-nr_BvFyoElPgndjejcSlrtD-DWydhLAM4asDVSw-nvaPsLVzsrhLx8MUgExJZT2ksoTck-UB-fy_-2zVi0rhjB-KF0000__y30000)

### 4. Các hành vi cảu từng lớp

PayrollAdministrator (PA)
  - Request to create report: PA yêu cầu hệ thống tạo một báo cáo.
  - Provide report criteria: Cung cấp các thông tin cần thiết để tạo báo cáo, bao gồm loại báo cáo, ngày bắt đầu, ngày kết thúc, và tên nhân viên.
  - Request to save report (nếu có): Nếu chọn lưu báo cáo, PA yêu cầu hệ thống lưu báo cáo, đồng thời cung cấp tên và vị trí để lưu.
  - No save request (nếu không lưu): PA không thực hiện yêu cầu lưu báo cáo nếu quyết định không lưu.

ReportRequestUI (UI)
  - Request report criteria: Hiển thị biểu mẫu và yêu cầu PA cung cấp các tiêu chí cần thiết để tạo báo cáo.
  - Send report criteria: Gửi các tiêu chí báo cáo đã nhận được từ PA tới ReportController để xử lý việc tạo báo cáo.
  - Display report: Hiển thị báo cáo đã được tạo cho PA xem.
  - Request name and location (khi lưu báo cáo): Yêu cầu PA cung cấp tên và vị trí lưu báo cáo nếu PA chọn lưu.
  - Send save report request: Gửi yêu cầu lưu báo cáo tới ReportController với tên và vị trí mà PA đã cung cấp.
  - Discard report: Nếu PA không chọn lưu báo cáo, UI gửi lệnh hủy báo cáo tới ReportController.

ReportController (RC)
  - Send report criteria to ReportService: Nhận tiêu chí từ UI và gửi tới ReportService để bắt đầu quá trình tạo báo cáo.
  - Return report to UI: Sau khi báo cáo được tạo, nhận báo cáo từ ReportService và gửi lại cho UI để hiển thị cho PA.
  - Save report to ReportService: Khi nhận được yêu cầu lưu báo cáo từ UI, ReportController gửi yêu cầu này tới ReportService để thực hiện lưu báo cáo.
  - Discard report: Nếu không có yêu cầu lưu báo cáo, ReportController thực hiện việc hủy bỏ báo cáo theo yêu cầu từ UI.

ReportService (RS)
  - Generate report: Nhận tiêu chí từ ReportController và thực hiện việc tạo báo cáo, gửi yêu cầu tạo báo cáo tới lớp Report (entity).
  -Save report: Nhận yêu cầu lưu báo cáo từ ReportController và lưu báo cáo vào vị trí và tên được chỉ định bởi PA.
Report (R)
  - Create report: Nhận thông tin từ ReportService và tạo báo cáo theo các tiêu chí đã cung cấp (loại báo cáo, ngày tháng, danh sách nhân viên).
  - Return generated report: Trả báo cáo đã tạo lại cho ReportService.

### 5. Biểu đồ lớp

a) Các lớp phân tích và nhiệm vụ của từng lớp

AdminReportUI:
  - Nhiệm vụ: Đây là lớp giao diện, tương tác trực tiếp với người dùng để yêu cầu nhập tiêu chí báo cáo và khởi tạo yêu cầu tạo báo cáo từ AdminReportController. Lớp này cũng cho phép người dùng lưu báo cáo.
  - Phương thức:
    + generateReport(): Yêu cầu tạo báo cáo từ AdminReportController.
    + saveReport(location: String): Lưu báo cáo vào vị trí do người dùng chỉ định.

AdminReportController:
  - Nhiệm vụ: Đây là lớp điều khiển, chịu trách nhiệm quản lý quá trình tạo báo cáo, yêu cầu và lấy dữ liệu từ các lớp liên quan (như Employee và Project). Nó cũng xử lý logic lưu báo cáo.
  - Phương thức:
      + requestReport(criteria: ReportCriteria): Nhận yêu cầu từ AdminReportUI và tạo báo cáo dựa trên tiêu chí từ ReportCriteria.
      + saveReport(report: Report, location: String): Lưu báo cáo được tạo vào vị trí chỉ định.

Report:
  - Nhiệm vụ: Lớp này đại diện cho báo cáo thực tế, bao gồm các loại báo cáo khác nhau (như giờ làm, nghỉ phép, lương, v.v.) cùng các phương thức để tạo và lấy dữ liệu báo cáo.
  - Phương thức:
    + generate(): Tạo báo cáo dựa trên dữ liệu có sẵn và tiêu chí nhận được từ ReportCriteria.
    + getData(): Trả về dữ liệu báo cáo để hiển thị hoặc lưu.

ReportCriteria:
  - Nhiệm vụ: Lớp này lưu trữ tiêu chí báo cáo, chẳng hạn như loại báo cáo, ngày bắt đầu và kết thúc. Nó giúp AdminReportController có thể xác định rõ ràng thông tin cần để tạo báo cáo.
  - Thuộc tính: reportType, startDate, endDate.

Project:
  - Nhiệm vụ: Lớp này đại diện cho một dự án, bao gồm các thuộc tính liên quan đến mã số dự án và cung cấp danh sách các số mã (charge numbers) liên quan đến dự án để báo cáo về dự án.
  - Phương thức:
    + getChargeNumbers(): Cung cấp danh sách mã chi phí liên quan đến dự án nếu báo cáo yêu cầu dữ liệu về dự án.

Employee:
  - Nhiệm vụ: Đại diện cho nhân viên, bao gồm thông tin về thời gian làm việc, ngày nghỉ phép, tiền lương, và phương thức để tạo báo cáo dựa trên yêu cầu của AdminReportController.
  - Phương thức:
    + generateReport(reportType: String, startDate: Date, endDate: Date): Tạo báo cáo cho nhân viên dựa trên loại báo cáo và khoảng thời gian.
   
b) Quan hệ giữa các lớp
  - AdminReportUI — AdminReportController: AdminReportUI là giao diện người dùng sẽ gửi yêu cầu tạo báo cáo đến AdminReportController.
  - AdminReportController — Report: AdminReportController quản lý quá trình tạo báo cáo bằng cách lấy và cung cấp dữ liệu từ các lớp khác (như Employee và Project) dựa trên tiêu chí của ReportCriteria.
  - ReportCriteria — Report: ReportCriteria chứa thông tin về loại báo cáo và khoảng thời gian, được sử dụng trong lớp Report để tạo báo cáo chính xác.
  - AdminReportController — Project và Employee: AdminReportController truy vấn dữ liệu từ Project và Employee dựa trên loại báo cáo yêu cầu để tổng hợp thông tin cho báo cáo.

c) Biểu đồ lớp phân tích
![](https://www.planttext.com/api/plantuml/png/h5HBJiCm4Dtd55usgBr0XAgY5aIbgggW2B4SaY6O9dOOEvKYnCbOS2IkW5t7WJYqAx98DCypRtxF-Vhud2aDfEkoYDIE2qPIOPGMe1Ixo4ekRh2IfE-Mx2rYzibH8856XuzYXohOUwIGAMWkHS9kDN6Hnz5xD2ISIw595WMI9oPyhL7fbYKbhf4u9AprR-rXFZfylD-OdSZlN7uIMclRLEXTMsuxZuLfCM7sxK0KMGXbe4rvAwxqkGkVzYVaPvEZPOFHe13VqpyKr35lIBvWslLOENEPzHbRU0rbaChKEdy6od5Tbuz8QXG77NQ9Bikga0sYpuIj7QRIKaDnBMjIzv9sQ4wl2WdQ7Ux1xMe1ZKhOKhImukbkXMR50NxWsa3pW41RwTh_nHP8SpZESJZASN-CiLUHRREl_ibaUaI-YLUkYlvsiA6jyX9MWe0SJxdw3LfUKpNkKHsaTYAasyKW9X1QhnH_nTYJfD3nR38vscwjJFtHp4pE_ZI-0G00__y30000)
## Phân tích ca sử dụng Create Employee Report. 
### 1. Mô tả ngắn gọn

Nhân viên có thể tạo các loại báo cáo như "Tổng giờ làm việc," "Tổng giờ làm việc cho một dự án," "Nghỉ phép/Bệnh," hoặc "Tổng lương đến hiện tại trong năm." Báo cáo này có thể được lưu lại nếu nhân viên yêu cầu.
### 2. Các Lớp Phân Tích
  - Boundary Class: ReportUI
  - Control Class: ReportController
  - Entity Class: Report
  - Entity Class: Project
### 3. Biểu đồ tuần tự
![CreateEmployeeReport](https://www.planttext.com/api/plantuml/png/Z5JDYXD14BxtKnHxqiE-G0wocIIx2gk8Hj1ZfzDaHcUwGwSdx1p5moABXnn4H0HZa8M5u8gAE7tmq67Vev_0Lx2w9_zcqJaqpLTVVLLVTJ6_pQ-3WQPAvrbAADDIGIlhfxBWd7HaBhfK5KlaqHsW0wWJ9eLMCbtY3tXVAjseq9Ghpue85phH1LJ18owuebuUOutDc8UQcz13PD8Uzv4MwL9DEtJ0uRwIJpdJTwd0M8O9pSWp3WbPT0Bxjw1UWyYLdpNCHguypw6m5pcmSDMk74leM3mO7gJk-L4DZfoP9g2Jm8pjT8r2Kmt74lEI5GYf_G1xRMTUYnuCd1b1Bt7clOSp6EBrbA6CXCoPjngwpdm1EnPx1F2BVCd36hHLNi19xifFoA0YXe4TinWoEwaKcVs6ufLOI3o4_QhPjdBb1DBGqdzbHlEft4ReXG0TEtFspynWu5viFme4x8K8IbjZRg3IAt5DPIww95HkOCkRSmyZeQ0LwgvDdJGylVaNdJHtML-fpKROG7XQijFgYhdjQV7-JrOhabvTvciPDxJzMM2UDtgpac_Lu7YJD7Jc7QwFTpF4pHZwecXkMdNcar_wPJHd8YQjXPV7E7iGiIkdOjlBR7HrwSo4XMQMdjfn66zdXn5VyZcSh2bksY1XZUS2EX7mhBeo-nLVhlmk8COL_y4ME7OywUESpUdr2wJNsa7ccsJNXhIHEfq_sBm48kS5PbE94njNUq8EyCG_q1y0003__mC0)
### 4. Nhiệm vụ của từng lớp

Employee (Nhân viên)
  - Tương tác với giao diện người dùng để khởi tạo yêu cầu tạo báo cáo, cung cấp các tiêu chí cho báo cáo, và chọn lưu báo cáo nếu cần.

ReportUI (Lớp giao diện người dùng)
  - Thu thập thông tin tiêu chí báo cáo từ nhân viên, bao gồm loại báo cáo, ngày bắt đầu, ngày kết thúc và mã dự án (nếu cần).
  - Hiển thị báo cáo cho nhân viên và cung cấp tùy chọn lưu báo cáo.
  - Xác nhận tên và vị trí lưu báo cáo khi nhân viên chọn lưu.

ReportController (Lớp xử lý)
  - Xử lý và điều phối quy trình tạo báo cáo.
  - Gửi yêu cầu đến lớp Project để lấy danh sách mã dự án nếu loại báo cáo cần mã dự án.
  - Gửi yêu cầu đến lớp Report để tạo báo cáo dựa trên tiêu chí được cung cấp và lưu trữ báo cáo nếu nhân viên chọn lưu.

Project (Lớp thực thể dự án)
  - Cung cấp danh sách mã dự án từ Cơ sở Dữ liệu Quản lý Dự án, để hỗ trợ việc tạo báo cáo "Total Hours Worked for a Project".

Report (Lớp thực thể báo cáo)
  - Tạo dữ liệu báo cáo dựa trên tiêu chí được cung cấp từ ReportController.
  - Lưu báo cáo đến tên và vị trí được chỉ định khi ReportController yêu cầu lưu.
### 5. Biểu đồ lớp

a) Các lớp phân tích và nhiệm vụ của từng lớp
Employee
  - Nhiệm vụ: Lớp này đại diện cho nhân viên, cho phép truy xuất và quản lý thông tin về nhân viên.
  - Thuộc tính:
    + employeeID
    + name
    + position
Phương thức:
  - getEmployeeInfo(): Lấy thông tin nhân viên cho báo cáo.

ReportGenerator
  - Nhiệm vụ: Xử lý yêu cầu từ người dùng để tạo các loại báo cáo khác nhau.
  - Thuộc tính:
    + reportType
    + startDate
    + endDate
  - Phương thức:
    + generateReport(): Tạo báo cáo dựa trên tiêu chí được cung cấp.
    + selectChargeNumber(): Lựa chọn mã phí cho báo cáo dự án cụ thể.

EmployeeReportUI
  - Nhiệm vụ: Giao diện cho phép người dùng chọn loại báo cáo, nhập tiêu chí, và yêu cầu lưu báo cáo.
  - Thuộc tính:
    + selectedReportType
    + inputCriteria
  - Phương thức:
    + displayReportOptions(): Hiển thị các lựa chọn loại báo cáo.
    + enterCriteria(): Yêu cầu người dùng nhập tiêu chí báo cáo.
    + displayReport(): Hiển thị báo cáo đã tạo cho người dùng.

FileManager
  - Nhiệm vụ: Xử lý lưu trữ và xóa các báo cáo đã tạo.
  - Thuộc tính:
    + fileName
    + filePath
  - Phương thức:
    + saveReport(): Lưu báo cáo vào hệ thống.
    + discardReport(): Xóa báo cáo không lưu.

b) Quan hệ giữa các lớp
  - EmployeeReportUI tương tác với ReportGenerator để yêu cầu tạo báo cáo và lấy thông tin liên quan từ Employee.
  - ReportGenerator có thể yêu cầu Employee cung cấp thông tin nhân viên nếu cần thiết cho báo cáo.
  - ReportGenerator sau khi tạo báo cáo sẽ gửi kết quả tới EmployeeReportUI để hiển thị.
  - FileManager được EmployeeReportUI sử dụng để lưu hoặc xóa báo cáo sau khi người dùng xác nhận.

c) Biểu đồ lớp
![](https://www.planttext.com/api/plantuml/png/T5BBJiCm4BpxAto4GyGz1rIf1PG31HNuW2NP9XQ9RRoReWZnPHpu97u1Eo-eKtA8el7EpcGytvzVAs9mt3Qre1UbfJE48g-1I5urjZOTedmNqZ-9n178DgbKcaTKGuEfV62dT3b2rf1YPVGHB6M9FEtCzDwSdQVoO5GXFiIek4Dh7D-WHWTit2piUlonix5Gxtq3xF7mddpg8iA2ThyK1ubPUZWah37dTGMkn6tRFADRUfkS3mkUijdSGCPYzvz9fMtBQwSOdO8eaaAHhQ4Rk7SsX4QHETIUED6ZioFwqlErgl4MD9Juc-NUOzlbbGNu7hYA_14SJaVcbNDmnL9vaLEIN2ukjk-F_ywPv9lYIiG3WJJtB_K5U6sH_70132U7__vgsjkcYz4ZZVqHOkMR4Ph-0m00__y30000)

## Phân tích ca sử dụng Maintain Employee Information 
### a. Mô tả ngắn gọn
  Ca sử dụng "Maintain Employee Information" cho phép Payroll Administrator (Quản trị viên Lương) thực hiện các thao tác duy trì thông tin nhân viên như: thêm mới, cập nhật, và xóa. Payroll Administrator sẽ chọn hành động muốn thực hiện (thêm, cập nhật, hoặc xóa). Hệ thống sẽ hướng dẫn để nhập thông tin cần thiết, xác nhận và lưu lại thông tin.
### b. Các lớp phân tích
- Lớp Boundary: PayrollAdministratorInterface, EmployeeInformationForm
- Lớp Controller: MaintainEmployeeController
- Lớp Entities: Employee
### c. Biểu đồ Sequence

![MaintainEmployeeInformation](https://www.planttext.com/api/plantuml/png/v5OzRzD06DxlLxpAGZ8Wzaf1ZKAX4IBA4A4odXstFjK-1-UCwfa98IGG0mCBeQeg1JgLoHuwNF_8_OB-1TwpJU9pxDQ2RaHAx7llUTxFvnpVf5Ux2q534VaUeRO8GkXCQ1m6dWU3cSyuMuYGeha3T06J0I5M4F4P3UCrpe2Dk732Gsex6NwzAh7s_BaNn8upueT1w5F10luKRpAylY5sm0NwXSuBohZ0xn_6CD_md3oPpP8uN31HyftjuuAG1p1rvSe7xihl7DumkUAato-CuypuKXkXtoUJ0JnylCaPTc3eglG3XywMZmxPm92pIGL9h-Gg0bibvr4jiOIjH2iHXIj_yICGZ1kP6q5riv2rprJwbYD3fU_1qei8V9NyQ7IIyP2FvMA54I8mvjdyBhXHupELNh0cXbaXZW49KvKi0x1KSihXo6LbF6QRVcMtz6KQ8WqyzC1WzCIWTh71txWBjawafySzLCd5735u4TMfvtlZVA_ry0sFzIMtbKChLwq4OlR135yTR0LREvumYk4aGdXJNcXs0dH5DA5QOtb2hKHnFzf5sZiS_WB5IEzDld3zIPxgpgqdLTinOvIrkcwfw6ELN0bu7MdBjfmFv2MjoZYpjPPlrKDRhMxp_WTXDbHr8fTsFcoEzvUqfgxRqDlJEQX2weg__YYNg8OPLXzL98f79USLLTz9G4rR-fJL1FiRPL9FmFEYeVAdhuzmSXQRp-Ro4NcavTGZW9_cuCRZe1YN9V5_mrFf5mRTSSdxSH5SfP-HeVFjuMl0BCziNwNdSLOgTFyfSEOYwvkhNUOHTl4NNvT-0m00__y30000)

### d. Nhiệm vụ của từng lớp phân tích:
- PayrollAdministratorInterface:
  + Cung cấp giao diện để Quản trị viên tiền lương lựa chọn thao tác muốn thực hiện (thêm, cập nhật, hoặc xóa nhân viên).
  + Hiển thị các tùy chọn và yêu cầu Quản trị viên cung cấp thông tin liên quan (ví dụ: ID nhân viên khi cập nhật hoặc xóa).
  + Quản lý việc hiển thị thông báo lỗi hoặc xác nhận khi cần thiết (ví dụ: khi không tìm thấy nhân viên hoặc khi Quản trị viên xác nhận xóa).
- EmployeeInformationForm: 
  + Hiển thị biểu mẫu để Quản trị viên nhập thông tin nhân viên (bao gồm tên, địa chỉ, lương, số an sinh xã hội, và các thông tin khác).
  + Thu thập thông tin từ Quản trị viên và chuyển tiếp cho lớp Controller để xử lý (khi thêm hoặc cập nhật nhân viên).
  + Cung cấp giao diện để chỉnh sửa thông tin nhân viên khi thực hiện thao tác cập nhật.
- MaintainEmployeeController:
  + Xử lý các yêu cầu từ PayrollAdministratorInterface (chọn thao tác thêm, cập nhật, hoặc xóa nhân viên).
  + Khi thao tác là thêm hoặc cập nhật, yêu cầu EmployeeInformationForm thu thập thông tin nhân viên từ Quản trị viên.
  + Sau khi nhận thông tin, MaintainEmployeeController sẽ xử lý và thực hiện các thay đổi cần thiết trong lớp Employee (thêm, cập nhật, hoặc xóa).
  + Quản lý các thông báo và trạng thái kết quả (thành công hoặc lỗi) sau khi hoàn thành thao tác.
  + Cung cấp các thông tin phản hồi (ví dụ: trả lại ID nhân viên khi thêm mới hoặc kết quả xóa khi thao tác hoàn tất).
- Employee:
  + Lưu trữ thông tin về nhân viên như tên, địa chỉ, loại nhân viên, lương, và các dữ liệu liên quan khác.
  + Cung cấp các phương thức để tạo mới, cập nhật và xóa thông tin nhân viên từ cơ sở dữ liệu.
  + Khi thao tác là xóa, sẽ đánh dấu nhân viên là đã bị xóa trong hệ thống (thay vì xóa hoàn toàn).
### e. Một số thuộc tính và phương thức của các lớp phân tích:
- PayrollAdministratorInterface:
  + selectedAction: Lưu trữ thao tác mà Quản trị viên chọn (thêm, cập nhật, xóa nhân viên).
  + employeeId: ID của nhân viên khi Quản trị viên thực hiện cập nhật hoặc xóa.
  + chooseAction(): Cho phép Quản trị viên chọn thao tác (thêm, cập nhật, xóa).
  + displayEmployeeForm(): Hiển thị biểu mẫu thông tin nhân viên.
  + showMessage(String message): Hiển thị thông báo cho Quản trị viên (ví dụ: thông báo lỗi hoặc thành công).
  + getEmployeeDetails(): Thu thập thông tin nhân viên từ Quản trị viên.
- EmployeeInformationForm:
  + employeeName: Tên của nhân viên.
  + employeeType: Loại nhân viên (giờ, lương cố định, hoa hồng).
  + salaryOrHourlyRate: Lương hoặc mức lương theo giờ.
  + displayForm(): Hiển thị biểu mẫu nhập thông tin nhân viên.
  + getInput(): Thu thập thông tin từ Quản trị viên và trả về dưới dạng đối tượng.
- MaintainEmployeeController:
  + action: Hành động mà Quản trị viên muốn thực hiện (thêm, cập nhật, xóa).
  + employee: Đối tượng nhân viên để thao tác (thêm, cập nhật, xóa).
  + handleAddEmployee(): Xử lý thêm mới nhân viên vào hệ thống.
  + handleUpdateEmployee(): Xử lý cập nhật thông tin nhân viên.
  + handleDeleteEmployee(): Xử lý xóa nhân viên khỏi hệ thống.
  + validateEmployeeData(): Kiểm tra tính hợp lệ của thông tin nhân viên (trước khi thêm hoặc cập nhật).
- Employee:
  + employeeId: ID của nhân viên.
  + name: Tên nhân viên.
  + employeeType: Loại nhân viên (giờ, lương cố định, hoa hồng).
  + salary: Lương của nhân viên (dành cho nhân viên lương cố định).
  + hourlyRate: Mức lương theo giờ (dành cho nhân viên làm theo giờ).
  + commissionRate: Mức hoa hồng (dành cho nhân viên hoa hồng).
  + status: Trạng thái nhân viên (đã xóa hay không).
  + createEmployee(): Tạo bản ghi nhân viên mới.
  + updateEmployee(): Cập nhật thông tin nhân viên.
  + deleteEmployee(): Đánh dấu nhân viên là đã xóa.
  + getEmployeeById(String employeeId): Truy xuất thông tin nhân viên theo ID.
  + validateData(): Kiểm tra tính hợp lệ của thông tin nhân viên.
### f. Mối quan hệ giữa các lớp
  - PayrollAdministratorInterface → MaintainEmployeeController: có quan hệ "uses". Gửi yêu cầu thao tác (thêm, cập nhật, xóa).
  - MaintainEmployeeController → EmployeeInformationForm: có quan hệ "uses". Thu thập thông tin nhân viên từ biểu mẫu.
  - MaintainEmployeeController → Employee: có quan hệ "depends on". Thực hiện thao tác thêm, cập nhật, xóa trên Employee.
  - EmployeeInformationForm → MaintainEmployeeController: có quan hệ "depends on". Gửi thông tin đã thu thập cho MaintainEmployeeController.
  - MaintainEmployeeController → PayrollAdministratorInterface:có quan hệ "uses". Trả kết quả thao tác (thành công, lỗi). 
### g. Biểu đồ lớp mô tả lớp phân tích

![maintainEmployee](https://www.planttext.com/api/plantuml/png/L90nRW9134Lxdy8Nu0AfM1Q8A2Abo0NCxi1QclMWMI_IdY27e4hAI8Y6YYaezYHp0gwG6KK1KLYodj__XM_XEksKlFQj1LYxNcho0xxJu9srHTsoSAUUrFcLgF4RgWnIXyN3NRGxwmPZLh9nlYLb9ykqP6i6bHDDJVX6B9hcNox_k3K-UoKOKTP7LuPpW08d4opn1L-P72h7otK7ipkCuSYepNYMRJeAIZD-2-vv_14eipLFraUJe-DNXViO3enr32Uq7CDd_nI0gP4wV-4N003__mC0)

##  Ca sử dụng Maintain Purchase Order
### a. Mô tả ngắn gọn 
  Ca sử dụng "Maintain Purchase Order" cho phép nhân viên thực hiện các thao tác quản lý đơn hàng, bao gồm thêm mới, cập nhật và xóa đơn hàng. Hệ thống hướng dẫn nhân viên nhập thông tin đơn hàng cần thiết, xác nhận thao tác và lưu lại kết quả
### b. Các lớp phân tích
- Lớp Boundary:  CommissionedEmployeeInterface, PurchaseOrderForm
- Lớp Controller: MaintainPurchaseOrderController
- Lớp entities: PurchaseOrder
### c. Biểu đồ Sequence

![MaintainPurchaseOrder](https://www.planttext.com/api/plantuml/png/l5IzRjim4Dxv53UcGFe26GBRSf86QDeE6TAHaTcGY4Iw56NKOz6fA39axX8dA0guyDG21QGX0uEy1vyWhv2Z5FzGbJ9MW21GzttVVNT7yg6yxMM6QfEd2Q6nKHeYbQOYouIIRBINZXCrPOoGKvNB4TNJrl2XD4n_e343ca5_ZNsNwvZJZBtL8wRtbKvzV41Y9OrM2HnH8Gs-0IogWmdJ7XmH9eqm3IaV6HBIPWLUxa8VTY3YhhoGO3XLOEmiXgrZRkVfDaIkM8n1SloORJYnl-aBqlUi25a7hbm8cDfv3h4hVkO1tnKprSwFbbdVRpBj7ta6HaYukxoVIU3s2jTRXyDWpPKh_iQRw9WB_BhYrZmP6w3mA-7ABxuSLtw3Kx_88NN5hwuyPB2qz8PNXZjWZSexK8Gc1ghwWz-0JrNw40N-2QE_yhkeGCDbbcFjYXkOkF8pX7rOQrN3ov6ERVmnRi2mEGfxRwybJ8ITIyAIZ0KZwJRu6lMcWPZXJ662JeiTtV3mzi5Cx1KwTELNoI73Xj8DYOesQAamo69lQc9eFIZm6LUh8axyZgtmqcTPaV4qZUff-etxFtlLTFNfsVojxbghypfrLNMQjYkXCQLpVxRWO-wzhyut8JrKyRVW8m000F__0m00)

### d. Nhiệm vụ của từng lớp phân tích:
- CommissionedEmployeeInterface:
  + Cho phép nhân viên có hoa hồng chọn thao tác họ muốn thực hiện (thêm, cập nhật, hoặc xóa đơn hàng).
  + Hiển thị thông báo và yêu cầu nhập thông tin liên quan đến đơn hàng (ví dụ: ID đơn hàng, thông tin khách hàng, sản phẩm, v.v.).
  + Hiển thị các thông báo lỗi hoặc xác nhận (ví dụ: đơn hàng không tồn tại, không phải của nhân viên này, hoặc đơn hàng đã đóng).
- PurchaseOrderForm:
  + Hiển thị biểu mẫu để nhân viên nhập thông tin chi tiết về đơn hàng (ví dụ: khách hàng, sản phẩm, địa chỉ thanh toán, ngày mua).
  + Thu thập dữ liệu từ nhân viên và chuyển tiếp cho Controller để xử lý.
-  MaintainPurchaseOrderController:
  + Xử lý các yêu cầu từ CommissionedEmployeeInterface (thêm, cập nhật, xóa đơn hàng).
  + Khi thao tác là Create, yêu cầu PurchaseOrderForm thu thập thông tin đơn hàng từ nhân viên.
  + Kiểm tra tính hợp lệ của thông tin đơn hàng (ví dụ: đảm bảo đơn hàng không bị đóng, là của nhân viên này).
  + Tương tác với PurchaseOrder (lớp thực thể) để tạo, cập nhật, hoặc xóa đơn hàng.
  + Trả về kết quả thành công hoặc lỗi cho CommissionedEmployeeInterface.
- PurchaseOrder:
  + Quản lý và xử lý dữ liệu liên quan đến đơn hàng, bao gồm việc tạo, cập nhật, xóa và truy vấn thông tin.
### e. Một số thuộc tính và phương thức của các lớp phân tích:
- CommissionedEmployeeInterface: 
  + selectedAction: Lưu trữ hành động mà nhân viên chọn (Tạo, Cập nhật, Xóa đơn hàng).
  + employeeId: ID của nhân viên đang thực hiện thao tác.
  + selectAction(): Cho phép nhân viên chọn thao tác (Tạo, Cập nhật, Xóa).
  + displayOrderForm(): Hiển thị biểu mẫu để nhân viên nhập thông tin đơn hàng.
  + displayMessage(String message): Hiển thị thông báo thành công hoặc lỗi cho nhân viên.
  + getEnteredOrderInfo(): Lấy thông tin đơn hàng mà nhân viên đã nhập vào biểu mẫu.
- PurchaseOrderForm: 
  + orderId: ID của đơn hàng cần cập nhật hoặc xóa.
  + customerName: Tên khách hàng.
  + billingAddress: Địa chỉ thanh toán của khách hàng.
  + products: Danh sách các sản phẩm trong đơn hàng.
  + orderDate: Ngày tạo đơn hàng.
  + collectOrderDetails(): Thu thập thông tin chi tiết về đơn hàng từ nhân viên.
  + displayOrderDetails(PurchaseOrder order): Hiển thị chi tiết đơn hàng để nhân viên xem và chỉnh sửa.
  + getOrderDetails(): Trả về thông tin đơn hàng mà nhân viên đã nhập.
- MaintainPurchaseOrderController:  
  + currentAction: Hành động hiện tại mà nhân viên đang thực hiện (Tạo, Cập nhật, Xóa).
  + currentEmployeeId: ID của nhân viên thực hiện thao tác.
  + orderService: Dịch vụ hoặc đối tượng giúp xử lý các thao tác liên quan đến đơn hàng (tạo, cập nhật, xóa).
  + handleCreateOrder(): Xử lý thao tác tạo đơn hàng mới.
  + handleUpdateOrder(): Xử lý thao tác cập nhật thông tin đơn hàng.
  + handleDeleteOrder(): Xử lý thao tác xóa đơn hàng.
  + validateOrderInfo(): Kiểm tra tính hợp lệ của thông tin đơn hàng (đảm bảo thông tin hợp lệ trước khi tạo hoặc cập nhật).
  + getOrderById(): Lấy thông tin đơn hàng từ ID.
  + confirmDeletion(): Xác nhận việc xóa đơn hàng từ nhân viên.
- PurchaseOrder: 
  + orderId: ID của đơn hàng.
  + customerName: Tên khách hàng.
  + customerBillingAddress: Địa chỉ thanh toán của khách hàng.
  + products: Danh sách các sản phẩm trong đơn hàng.
  + orderDate: Ngày tạo đơn hàng.
  + status: Trạng thái đơn hàng (Mở, Đã đóng).
  + commissionedEmployeeId: ID của nhân viên có hoa hồng đã tạo đơn hàng.
  + createOrder(): Tạo đơn hàng mới và lưu vào hệ thống.
  + updateOrder(): Cập nhật thông tin đơn hàng.
  + deleteOrder(): Xóa đơn hàng khỏi hệ thống.
  + getOrderById(String orderId): Lấy thông tin đơn hàng bằng ID.
  + validateOrder(): Kiểm tra tính hợp lệ của đơn hàng (còn mở, là của nhân viên hiện tại, v.v.).
  + closeOrder(): Đóng đơn hàng sau khi hoàn thành giao dịch.
### f. Mối quan hệ giữa các lớp
  - CommissionedEmployeeInterface → MaintainPurchaseOrderController: Quan hệ "uses" (sử dụng), vì CommissionedEmployeeInterface gửi yêu cầu thao tác đến MaintainPurchaseOrderController.
  - MaintainPurchaseOrderController → PurchaseOrderForm: Quan hệ "uses" (sử dụng), vì MaintainPurchaseOrderController yêu cầu PurchaseOrderForm thu thập thông tin đơn hàng từ nhân viên.
  - MaintainPurchaseOrderController → PurchaseOrder: Quan hệ "depends on" (phụ thuộc), vì MaintainPurchaseOrderController cần PurchaseOrder để thực hiện các thao tác như tạo, cập nhật, hoặc xóa đơn hàng.
  - PurchaseOrderForm → PurchaseOrder: Quan hệ "associates" (liên kết), vì PurchaseOrderForm thu thập và chuyển tiếp thông tin đơn hàng đến PurchaseOrder.
### g. Biểu đồ lớp mô tả lớp phân tích

![MaintainPurchaseOrder](https://www.planttext.com/api/plantuml/png/N8wn3G8n34LxJ-45ReUxou54WM25a1WHAR6HunGt6mKZiG842WJ5ro_UqzT_tEvZDQ_MIWOuIUFeTKKdfQHQap35JRbcMObsRAHd7mXznUdh7fk6Ywzqq4Yw5IsTpn24JINZtYUsLtuqzu6PjCiEY2tPtrGd2y24mu0EONutk5uB0ep4iPz-0W00__y30000)

## Phân tích ca sử dụng Run Payroll
### a. Mô tả ngắn gọn 
  Ca sử dụng "Run Payroll" mô tả quá trình hệ thống tự động xử lý bảng lương cho nhân viên vào mỗi thứ Sáu và ngày làm việc cuối cùng của tháng. Hệ thống sẽ tính toán tiền lương dựa trên các thông tin liên quan như thời gian làm việc, đơn hàng, thông tin nhân viên và các khoản khấu trừ hợp pháp. Hệ thống sẽ in phiếu lương nếu phương thức thanh toán là thư hoặc nhận trực tiếp, hoặc tạo giao dịch ngân hàng nếu là chuyển khoản trực tiếp.
### b. Các lớp phân tích
- Lớp Boundary:  PayrollInterface, BankSystemInterface
- Lớp Controller: RunPayrollController
- Lớp Entities: Employee, Payroll
### c. Biểu đồ Sequence

![RunPayroll](https://www.planttext.com/api/plantuml/png/X9F1Jjmm48RlVefvWQht7YeskwlI6oezqAF9fciBxnWvOuJFFN3Wn1kGLbLLf1Kz9weukE8z_0HzXOx3Xi22IYwHnj_y_--Pv6ztirEJTEHNHWXPadMm9uEpnamMAusw9YUvACIXzRYGBWp7xv4gzrcM5SWQ9kDn8V5eFzHKhHuHXIWj4ZV21uyRYUbTnLGk4rDH8MaAC5yT6nkglcqs53SjkJONuhc8yEejJE0D5Acz9lXpaTeV7agLsYR0OMg_uHBCxQ_R1fTYajafiv_Y5JCzUPgwDPZuUvkTPdR6x4Vd0vpwr7udMAJk6enEtPa7LF4hmecELtW7ppFCjdPRQdul5TUeW6niS3ZafFQfLBxFBjjyGI2LkdCWny9CaugDtjONqX3igOrWRlXPyakENl6IVNpe1O-KpUq2-EdD2ZPxnrFGiDJIvZkUbmfmcJEfUCa66Is6sHt4fkJ4gLtZmmPHcRfwCSMnqgczyKFqrniTac7CCxkVunRDtyH2Z8lPjHmEg5_CSylB-pZuttQVJFan16eq46A7pVFFyWy00F__0m00)

### d. Nhiệm vụ của từng lớp phân tích:
- PayrollInterface: Cung cấp giao diện để hệ thống hiển thị trạng thái và kết quả khi chạy bảng lương. Đây là lớp mà người dùng (chẳng hạn như Payroll Administrator) tương tác để xem báo cáo, xác nhận thanh toán, và thực hiện các thao tác liên quan đến bảng lương.
- BankSystemInterface: Cung cấp giao diện để kết nối và truyền dữ liệu đến hệ thống ngân hàng để xử lý thanh toán qua chuyển khoản trực tiếp.
-  RunPayrollController: lớp điều khiển chính trong ca sử dụng Run Payroll, chịu trách nhiệm xử lý các bước như tính toán lương, in phiếu lương, gửi giao dịch ngân hàng, v.v.
- Employee: Đại diện cho một nhân viên trong hệ thống, bao gồm các thông tin cá nhân và thông tin thanh toán.
- Payroll: Lớp đại diện cho bảng lương, bao gồm thông tin liên quan đến việc tính toán lương của tất cả nhân viên trong mỗi chu kỳ bảng lương.
### e. Một số thuộc tính và phương thức của các lớp phân tích:
- PayrollInterface:
  + payrollStatus: Trạng thái của việc chạy bảng lương (hoàn thành, lỗi, đang chờ).
  + employeeList: Danh sách nhân viên đã nhận thanh toán.
  + displayPayrollStatus(): Hiển thị trạng thái bảng lương.
  + displayEmployeePaymentDetails(): Hiển thị chi tiết thanh toán của từng nhân viên.
-BankSystemInterface:
  + transactionStatus: Trạng thái giao dịch với hệ thống ngân hàng.
  + sendBankTransaction(): Gửi giao dịch ngân hàng.
  + retryBankTransaction(): Thử lại giao dịch nếu hệ thống ngân hàng không sẵn sàng.
- RunPayrollController:
  + payrollDate: Ngày bảng lương được chạy.
  + employeeList: Danh sách nhân viên cần được thanh toán.
  + calculatePay(): Tính toán lương cho từng nhân viên dựa trên thông tin từ thẻ chấm công, đơn hàng và thông tin nhân viên.
  + processPayment(): Xử lý thanh toán cho nhân viên, bao gồm in phiếu lương hoặc gửi giao dịch ngân hàng.
  + checkBankSystemStatus(): Kiểm tra trạng thái hệ thống ngân hàng và xử lý nếu hệ thống không khả dụng.
  + deleteEmployee(): Xóa nhân viên đã bị đánh dấu xóa sau khi bảng lương đã được xử lý.
- Employee:
  + employeeId: Mã số nhân viên.
  + name: Tên nhân viên.
  + salary: Lương cơ bản của nhân viên (nếu là nhân viên lương cố định).
  + hourlyRate: Mức lương theo giờ (nếu là nhân viên lương theo giờ).
  + benefits: Các phúc lợi (Bảo hiểm, nghỉ phép, v.v.).
  + paymentMethod: Phương thức thanh toán (chuyển khoản, lấy phiếu lương, v.v.).
  + status: Trạng thái của nhân viên (đang làm việc, đã nghỉ việc, v.v.).\
  + calculatePay(): Tính toán lương cho nhân viên.
  + generatePaycheck(): Tạo phiếu lương cho nhân viên.
  + sendPayment(): Gửi thanh toán cho nhân viên qua chuyển khoản ngân hàng.
- Payroll:
  + payrollDate: Ngày chạy bảng lương.
  + employeePayments: Danh sách các khoản thanh toán cho nhân viên.
  + runPayroll(): Chạy bảng lương cho tất cả nhân viên.
  + generateReports(): Tạo báo cáo về bảng lương.
  + processPayments(): Xử lý thanh toán cho nhân viên.
### f. Mối quan hệ giữa các lớp
  - PayrollInterface → RunPayrollController: Quan hệ "uses" (sử dụng), vì PayrollInterface gửi yêu cầu xử lý bảng lương và nhận kết quả từ RunPayrollController.
  - RunPayrollController → Employee: Quan hệ "associates" (liên kết), vì RunPayrollController cần truy xuất thông tin nhân viên để thực hiện tính toán lương.
  - RunPayrollController → Payroll: Quan hệ "depends on" (phụ thuộc), vì RunPayrollController tạo và xử lý thông tin bảng lương thông qua Payroll.
  - RunPayrollController → BankSystemInterface: Quan hệ "uses" (sử dụng), vì RunPayrollController cần kết nối với BankSystemInterface để gửi các giao dịch thanh toán ngân hàng.
  - PayrollInterface → Payroll: Quan hệ "associates" (liên kết), vì PayrollInterface hiển thị thông tin từ Payroll cho người dùng.
### g. Biểu đồ lớp mô tả lớp phân tích

![RunPayroll](https://www.planttext.com/api/plantuml/png/L8x13S8m34Nldi8BT8SsQG_S44nWMYCX4WSbpY6pzS18h413Wn2d9xt_l-NN-koJKjJi7S0bP5ae5ZnIYS6vWoZ7AysCb73unORaVYv9sVyr3Cn1T1lYAKixONVZEDQ61HQzQS79Frme_9cDNzacrKq00tOTMWJJQ2l77LlSioprwJS0003__mC0)

### 3. Biểu đồ tuần tự
![](https://www.planttext.com/api/plantuml/png/T5D1ReCm4Bpx5IjEYTHyW4CLGLALGwD89UhPDKknC3QobqFUraEVr2zqJKAIbfG3icTdTeR5_lxyMWUIdeREYD1g2zu555AHyx2NH--CEHGW0nmAePmb1YOyFsqD-bY_xWHQqdI4RTSRTqICLLvFSAaxLD9N4Ixp2JttZ20l9pIJjYszj843QMTZDIk5u4Ihnnl75FnWpnqMIt4JZ6bidS87qjRe3_rkS8eLcCbhMFrfPNGWS3NWcGyu2OHnheUQ9uDIDHTS03-_FSjyj7nsWm-BYLTKov5QvZFF9XBVd6-nkjEDlom59OqQZ2JS8J4GknQsTW-tsbD_hiuCx2WQoz9Gf7GyOkYG6bU13f1ij4T5iC7U1Kt9I9r7oSeKUkyKXd3pNwvXJZxBYpehPr7egdibIKCOoMW2telr8hL9W4UUxBLgLqF_Nx934PDf6_rLWwgz7mhjlGcFPwIJldroVeC6OxYYtY5MMF4nYQ8rl-8b-G400F__0m00)
### 4. Nhiệm vụ của từng lớp

  - PayrollUI: Khởi tạo quá trình xử lý bảng lương và hiển thị trạng thái hoặc kết quả cho người dùng.
  - PayrollController: Điều phối toàn bộ quy trình chạy bảng lương, lấy danh sách nhân viên, tính toán và thực hiện thanh toán dựa trên phương thức đã chọn.
  - Employee:Đại diện cho từng nhân viên và lưu trữ các thông tin chi tiết như mức lương, phương thức thanh toán và trạng thái xóa.
  - Timecard: Quản lý giờ làm việc của từng nhân viên để cung cấp dữ liệu tính lương.
  - Payroll: Thực hiện các phép tính cho lương ròng và áp dụng các khoản khấu trừ hợp pháp.
  - BankTransaction: Xử lý giao dịch ngân hàng cho các khoản thanh toán qua chuyển khoản.

### 5.Biểu đồ lớp

a) Các lớp phân tích và nhiệm vụ của từng lớp

PayrollUI
  - Nhiệm vụ: Giao diện người dùng để khởi tạo quá trình chạy bảng lương và hiển thị trạng thái hoặc kết quả.
  - Phương thức:
    + runPayroll(): Gửi yêu cầu chạy bảng lương tới PayrollController.
    + printPaycheck(empInfo: Employee, netPay: Float): In phiếu lương cho nhân viên.
    + displayPayrollStatus(status: String): Hiển thị trạng thái quá trình chạy bảng lương.

PayrollController
  - Nhiệm vụ: Điều phối toàn bộ quá trình chạy bảng lương, bao gồm lấy danh sách nhân viên, tính toán lương, và xử lý thanh toán.
  - Phương thức:
    + runPayroll(): Quản lý toàn bộ quá trình chạy bảng lương.
    + getEligibleEmployees(): List<Employee>: Lấy danh sách nhân viên đủ điều kiện nhận lương.
    + processEmployeePay(employee: Employee): Tính toán và xử lý thanh toán cho từng nhân viên.
    + markForDeletionIfNeeded(employee: Employee): Đánh dấu xóa nhân viên nếu cần.

Employee
  - Nhiệm vụ: Lưu trữ thông tin chi tiết của từng nhân viên và cung cấp các phương thức liên quan đến nhân viên.
  - Thuộc tính:
    + employeeId: String
    + salary: Float
    + paymentMethod: String
    + markedForDeletion: Boolean
  - Phương thức:
    + isEligibleForPayroll(currentDate: Date): Boolean: Kiểm tra điều kiện nhận lương.
    + getPaymentDetails(): Payment: Lấy thông tin thanh toán của nhân viên.

Timecard
  - Nhiệm vụ: Quản lý và cung cấp dữ liệu về giờ làm việc của nhân viên.
  - Thuộc tính:
    + employeeId: String
    + hoursWorked: Float
    + date: Date
  - Phương thức:
    + getHours(employeeId: String): Float: Lấy số giờ làm việc của nhân viên.

Payroll
  - Nhiệm vụ: Thực hiện các phép tính chi tiết cho quá trình chạy bảng lương.
  - Phương thức:
    + calculateNetPay(empInfo: Employee, hoursWorked: Float): Float: Tính lương ròng.
    + applyDeductions(empInfo: Employee): Float: Áp dụng các khoản khấu trừ hợp pháp.

BankTransaction 
  - Nhiệm vụ: Xử lý giao dịch ngân hàng cho các khoản thanh toán qua chuyển khoản.
  - Thuộc tính:
    + transactionId: String
    + amount: Float
  - Phương thức:
    + processTransaction(empInfo: Employee, netPay: Float): Boolean: Thực hiện giao dịch ngân hàng.

b) Quan hệ giữa các lớp
  - PayrollUI giao tiếp với PayrollController để khởi tạo quá trình chạy bảng lương và nhận trạng thái từ quá trình.
  - PayrollController tương tác với Employee để lấy danh sách nhân viên đủ điều kiện nhận lương.
  - PayrollController sử dụng Timecard để lấy dữ liệu giờ làm việc của từng nhân viên.
  - PayrollController gọi Payroll để tính toán lương ròng cho từng nhân viên.
  - PayrollController tương tác với BankTransaction nếu phương thức thanh toán của nhân viên là chuyển khoản trực tiếp.
  - PayrollController tương tác lại với Employee để đánh dấu xóa nhân viên nếu cần thiết sau khi xử lý lương.

c) Biểu đồ lớp phân tích
![](https://www.planttext.com/api/plantuml/png/X9JFYjim4CRlUWeTRMXUG9HbsMQN1jgbi5jwdbgpYOWi6OsqO4gVh8S-Kb-XactPZXsJ76BpUURJR_xO__xylISFpeTQCpehmvqbP9K68luDMcUr_dxWlnXFFnVCe1LbhpHE6H-rweJLkS2wEPWtA_XZtMZR8dxW1jDZmP-q1JyaIKMDXdQmUl7W0nNKNGH_yT7oMBBVx9BYapK-NT5jqnpHFsfrL3yrPW8gIi6_AF8VitANoMs5H5cDJWc_kv_u1zyQtFd9kZrgzCgQmzipeaHvDM7apjA0kyl11vcBx7K23Ivtg9SQQ6iq_Ylwarr49nIKCnZ17wpL2AP7LPGx46DoUwhWNFJRWu-ewRzSP1sxAQKpz-X1wQvhWp9LzAfghC39MnMTR73qmoRGYxBaUFvuwkSKMgoDofouN8Cy_0fq5NIqUkxGhwtU6gESut1e6jtkKOOgzP7M5ck81p3dLmU6eCl9ZV2JjEm5r3OOVt7ki7apdzilpZIlo3AzbxlTtPNtGt1bb5UnESJMJrFEk9k2Euoq-BuPEWvTy42RKNaw8bUt6RbiuHoMZLnRXKNtToMHUDmbO2FRpV3PBcIkpQJOaU0C3HDWI-1RQRDbw3zjx1wDJD_N_m000F__0m00)

## Phân tích biểu đồ Select Payment Method
### 1. Mô tả ngắn gọn

Ca sử dụng này cho phép Nhân viên chọn một phương thức thanh toán. Phương thức thanh toán sẽ quyết định cách Nhân viên nhận lương. Nhân viên có thể chọn một trong ba phương thức: nhận séc trực tiếp, nhận séc qua đường bưu điện, hoặc nhận lương qua chuyển khoản trực tiếp vào tài khoản ngân hàng đã đăng ký.

### 2. Các lớp phân tích
  - Boundary Class: PaymentUI
  - Controller Class: PaymentController
  - Entity Class: Employee, EmployeePayment
### 3. Biểu đồ tuần tự
![](https://www.planttext.com/api/plantuml/png/b5JBQjmm5DthAowppmy4N492eQLfo2A5TkdnY94OMpAs7EfbwQAKKEZGHRVZXb8QMff2L_PY5XhcF_G5_OLUoKxzcCaaySB6L-UUSy-nvB_LyY1LVgAoA2JfZ0j8P4g97oYPmECe3cLEAVWfK4B6CXCJFydXrCyZAjIBftOXuoIGGYKolAaVrQyXJUnwe9AGO9Mhl4yOnSDoq-zMOpydXCBU8nI0VJWqvIy5gxaflsKGC5Dz412pzVMw45DG-Fuzm8Sl62Yf2q6m2LkjDZQ_qbVOTDzMueSAJWU0aM2c_2b09QRwZNXkocKy8e2N4q4nEpAA7I4k1WTN4DUEdbF5v0IyssXecD9DoV7wEaFBt5JlH5_AHT9nvLXZ6qzruJkFxns-Je4Y-6GJ0Lr-1s_ZNdtSXUk5UvPakQdUY3ku7vHIppcSwmM4TQNZRtjc0RFJl1KmOzAKb_VBJJ7zntBWxXTJ_KK0qmintPwWqbSzb9ikDTKSLnRyHdOGvzbT0iCiTqpqe20tucZ3l4M2YWaX0urZYzzrP1okjV5I5m4qEtqrrvSsxVz3ajx7Q98PwqBQYHP86TWAQOD_mfq3Ati1hrjgGZmFZZDJVN93uGG8APVxyAnnfdz4hlGrxvbbrkmvjz_HyznXF-cKPWtIqkAzBFZc3kfD59hgCuVH5j6EqgZEzG3lzaVx3m00__y30000)
### 4. Nhiệm vụ của từng lớp phân tích
PaymentUI
  - Hiển thị các tùy chọn phương thức thanh toán cho nhân viên (nhận trực tiếp, bưu điện, chuyển khoản).
  - Nhận thông tin đầu vào từ nhân viên, như địa chỉ bưu điện (khi chọn phương thức bưu điện) hoặc thông tin ngân hàng (khi chọn phương thức chuyển khoản).
  - Hiển thị thông báo xác nhận hoặc lỗi cho nhân viên sau khi xử lý.
PaymentController
  - Nhận yêu cầu chọn phương thức thanh toán từ PaymentUI.
  - Kiểm tra và yêu cầu thông tin bổ sung nếu cần, dựa trên phương thức thanh toán được chọn (ví dụ, yêu cầu địa chỉ bưu điện hoặc thông tin ngân hàng).
  - Tương tác với lớp EmployeePayment để lưu thông tin phương thức thanh toán đã chọn.Truyền kết quả cập nhật thành công hoặc thông báo lỗi về PaymentUI.
Employee
  - Lưu trữ thông tin cơ bản của nhân viên, cần thiết để xác định danh tính khi chọn phương thức thanh toán.
  - Cung cấp các phương thức cần thiết để lấy thông tin nhân viên và kiểm tra tính hợp lệ của nhân viên.
EmployeePayment
  - Lưu trữ thông tin về phương thức thanh toán mà nhân viên đã chọn.
  - Lưu các thông tin bổ sung, như địa chỉ (với phương thức bưu điện) hoặc tên ngân hàng và số tài khoản (với phương thức chuyển khoản).
  - Cung cấp các phương thức để cập nhật và xác nhận thông tin thanh toán đã chọn.
### 5. Phân tích các lớp
a) Thuộc tính của từng lớp
PaymentUI (Boundary Class):
  - selectedMethod: Chuỗi biểu thị phương thức thanh toán đã chọn ("pick up", "mail", hoặc "direct deposit").
  - address: Chuỗi chứa địa chỉ, nếu phương thức thanh toán là "mail".
  - bankName: Chuỗi tên ngân hàng, nếu phương thức thanh toán là "direct deposit".
  - accountNumber: Chuỗi chứa số tài khoản, nếu phương thức thanh toán là "direct deposit".
PaymentController (Control Class):
  - employee: Đối tượng Employee đại diện cho nhân viên thực hiện lựa chọn.
  - payment: Đối tượng EmployeePayment đại diện cho thông tin thanh toán của nhân viên.
Employee (Entity Class):
  - employeeId: ID duy nhất của nhân viên.
  - name: Tên nhân viên.
  - position: Chức vụ của nhân viên.
EmployeePayment (Entity Class):
  - paymentMethod: Chuỗi phương thức thanh toán ("pick up", "mail", hoặc "direct deposit").
  - mailingAddress: Chuỗi địa chỉ nhận qua thư, nếu phương thức là "mail".
  - bankName: Tên ngân hàng, nếu phương thức là "direct deposit".
  - accountNumber: Số tài khoản ngân hàng, nếu phương thức là "direct deposit".
  - status: Trạng thái thanh toán (có thể là "active", "pending", v.v.).
b) Quan hệ giữa các lớp
  - PaymentUI ↔ PaymentController: PaymentUI tương tác với PaymentController để gửi phương thức thanh toán được chọn và nhận lại kết quả xác nhận hoặc lỗi.
  - PaymentController ↔ Employee: PaymentController có quan hệ với Employee để truy cập và xác minh thông tin nhân viên thực hiện lựa chọn phương thức thanh toán.
  - PaymentController ↔ EmployeePayment: PaymentController cũng liên kết với EmployeePayment để cập nhật thông tin thanh toán mới và xác nhận phương thức thanh toán.

c) Biểu đồ lớp
![](https://www.planttext.com/api/plantuml/png/h9DDJiCm48NtEONLLI8r5-W22G6BB2X84GTmusbhrN_oE48HucGiE19Nm4cSD7P0RCWgyOpdVVDcylNnYHUkYDK8MICe8dccdGJbNYhobX7_b0H1GEE0FO8xQxOZjRDSdKAGCcO1CJazK7NPKmbfSjFeLhbzAmzWenWXZACHj0loJyPnhJ0lGlG4hWuO8MEaoOka39xrwvrMHsubxKlawAXhPxuYUy_YHdsoire8i7F388tG7NZwX_0M0cQySZqFDIRjWJ3cav5fKplDI1XIymNLL7a5KwNEgxM_HYFlcquyyUPDPU_1KIvmOTjAEK3D06RPJo8eVJ7_TVi_I-1NBHfMm6yDesx2gjHH9wPkqoNS3jAByAZuon3gGJDsvFeJbEND3vko8viM0JVPEjHVc8VeyxouH_ixTxAzBpqqx6zy0m00__y30000)
d) Giải thích biểu đồ lớp
  - PaymentUI: Đây là lớp giao diện, chứa các thuộc tính về phương thức thanh toán đã chọn và thông tin bổ sung như địa chỉ hoặc thông tin ngân hàng. PaymentUI chịu trách nhiệm thu thập đầu vào từ nhân viên và hiển thị kết quả sau khi PaymentController xử lý.
  - PaymentController: Lớp điều khiển này quản lý quá trình xử lý chọn phương thức thanh toán. Nó chứa tham chiếu tới Employee (để truy xuất thông tin nhân viên) và EmployeePayment (để lưu thông tin thanh toán của nhân viên).
  - Employee: Lớp này đại diện cho nhân viên đang thực hiện lựa chọn phương thức thanh toán, chứa các thuộc tính cơ bản của nhân viên như ID, tên, và chức vụ.
  - EmployeePayment: Lớp lưu trữ thông tin thanh toán của nhân viên, bao gồm phương thức thanh toán và các thuộc tính liên quan. Lớp này kết nối với PaymentController để lưu trữ và quản lý thông tin thanh toán.

## Code java mô phỏng ca sử dụng Maintain Timecard.
```package maintain;

import java.time.LocalDate;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Scanner;

 Class đại diện cho timecard
class Timecard {
    private int employeeId;
    private LocalDate startDate;
    private LocalDate endDate;
    private LocalDate submittedDate;
    private boolean isSubmitted;
    private Map<Integer, Map<LocalDate, Double>> hoursPerChargeNumber; // Charge number -> Date -> hours
    private static final int MAX_HOURS_PER_DAY = 24;
    private static final int MAX_HOURS_PER_WEEK = 40;

    public Timecard(int employeeId) {
        this.employeeId = employeeId;
        this.startDate = LocalDate.now().minusDays(LocalDate.now().getDayOfWeek().getValue() - 1);
        this.endDate = startDate.plusDays(6);
        this.isSubmitted = false;
        this.hoursPerChargeNumber = new HashMap<>();
    }

    public boolean addHours(int chargeNumber, LocalDate date, double hours) {
        if (isSubmitted) {
            System.out.println("Timecard đã được submit, không thể thay đổi.");
            return false;
        }
        
        if (hours > MAX_HOURS_PER_DAY) {
            System.out.println("Số giờ làm việc trong ngày không thể vượt quá 24 giờ.");
            return false;
        }

        if (date.isBefore(startDate) || date.isAfter(endDate)) {
            System.out.println("Ngày không nằm trong khoảng thời gian của timecard.");
            return false;
        }

        // Tính tổng số giờ trong ngày
        double totalHoursForDay = hoursPerChargeNumber.values().stream()
            .mapToDouble(dateMap -> dateMap.getOrDefault(date, 0.0))
            .sum() + hours;

        if (totalHoursForDay > MAX_HOURS_PER_DAY) {
            System.out.println("Tổng số giờ làm việc trong ngày vượt quá 24 giờ.");
            return false;
        }

        hoursPerChargeNumber.computeIfAbsent(chargeNumber, k -> new HashMap<>())
                           .put(date, hours);
        return true;
    }

    public void displayTimecard() {
        System.out.println("\n=== Thông tin Timecard ===");
        System.out.println("Employee ID: " + employeeId);
        System.out.println("Thời gian: " + startDate + " đến " + endDate);
        System.out.println("Trạng thái: " + (isSubmitted ? "Đã gửi" : "Nháp"));
        if (isSubmitted) {
            System.out.println("Ngày gửi: " + submittedDate);
        }
        System.out.println("\nSố giờ làm việc:");
        hoursPerChargeNumber.forEach((chargeNumber, dateMap) -> {
            System.out.println("\nCharge Number: " + chargeNumber);
            dateMap.forEach((date, hours) -> 
                System.out.println(date + ": " + hours + " giờ"));
        });
    }

    public boolean submit() {
        if (isSubmitted) {
            System.out.println("Timecard đã được submit trước đó.");
            return false;
        }

        // Validate total hours per week
        double totalHours = hoursPerChargeNumber.values().stream()
            .flatMap(dateMap -> dateMap.values().stream())
            .mapToDouble(Double::doubleValue)
            .sum();

        if (totalHours > MAX_HOURS_PER_WEEK) {
            System.out.println("Tổng số giờ làm việc trong tuần vượt quá " + MAX_HOURS_PER_WEEK + " giờ.");
            return false;
        }

        this.submittedDate = LocalDate.now();
        this.isSubmitted = true;
        System.out.println("Timecard đã được gửi thành công.");
        return true;
    }

    public boolean isSubmitted() {
        return isSubmitted;
    }
}

 Class quản lý Project Management Database
class ProjectManagementDB {
    private boolean isAvailable;
    private List<Integer> chargeNumbers;

    public ProjectManagementDB() {
        this.isAvailable = true;
        this.chargeNumbers = new ArrayList<>(Arrays.asList(1001, 1002, 1003, 1004));
    }

    public List<Integer> getChargeNumbers() throws DatabaseUnavailableException {
        if (!isAvailable) {
            throw new DatabaseUnavailableException("Project Management Database không khả dụng.");
        }
        return new ArrayList<>(chargeNumbers);
    }

    public void setAvailable(boolean available) {
        this.isAvailable = available;
    }
}

class DatabaseUnavailableException extends Exception {
    public DatabaseUnavailableException(String message) {
        super(message);
    }
}

Main class để demo với user input
public class Maintain {
    private static Scanner scanner = new Scanner(System.in);
    private static Map<Integer, Timecard> timecards = new HashMap<>();
    private static ProjectManagementDB projectDB = new ProjectManagementDB();

    public static void main(String[] args) {
        System.out.println("Chào mừng đến với Hệ thống Timecard");
        
        while (true) {
            try {
                System.out.println("\n=== Menu ===");
                System.out.println("1. Xem Timecard");
                System.out.println("2. Thêm Giờ");
                System.out.println("3. Gửi Timecard");
                System.out.println("4. Thoát");
                System.out.print("Chọn một tùy chọn: ");

                int choice = Integer.parseInt(scanner.nextLine());
                
                switch (choice) {
                    case 1:
                        viewTimecard();
                        break;
                    case 2:
                        addHours();
                        break;
                    case 3:
                        submitTimecard();
                        break;
                    case 4:
                        System.out.println("Tạm biệt!");
                        return;
                    default:
                        System.out.println("Tùy chọn không hợp lệ. Vui lòng thử lại.");
                }
            } catch (NumberFormatException e) {
                System.out.println("Vui lòng nhập một số hợp lệ.");
            }
        }
    }

    private static void viewTimecard() {
        System.out.print("Nhập Employee ID: ");
        int employeeId = Integer.parseInt(scanner.nextLine());
        
        Timecard timecard = timecards.get(employeeId);
        if (timecard == null) {
            System.out.println("Không tìm thấy timecard. Đang tạo timecard mới...");
            timecard = new Timecard(employeeId);
            timecards.put(employeeId, timecard);
        }
        
        timecard.displayTimecard();
    }

    private static void addHours() {
        System.out.print("Nhập Employee ID: ");
        int employeeId = Integer.parseInt(scanner.nextLine());
        
        Timecard timecard = timecards.computeIfAbsent(employeeId, Timecard::new);
        
        try {
            List<Integer> chargeNumbers = projectDB.getChargeNumbers();
            System.out.println("Các charge number có sẵn: " + chargeNumbers);
            
            System.out.print("Nhập charge number: ");
            int chargeNumber = Integer.parseInt(scanner.nextLine());
            
            System.out.print("Nhập ngày (YYYY-MM-DD): ");
            LocalDate date = LocalDate.parse(scanner.nextLine());
            
            System.out.print("Nhập số giờ làm việc: ");
            double hours = Double.parseDouble(scanner.nextLine());
            
            if (timecard.addHours(chargeNumber, date, hours)) {
                System.out.println("Giờ đã được thêm thành công.");
            }
        } catch (DatabaseUnavailableException e) {
            System.out.println(e.getMessage());
            System.out.print("Bạn có muốn tiếp tục mà không có charge numbers không? (y/n): ");
            if (scanner.nextLine().toLowerCase().equals("n")) {
                return;
            }
        }
    }

    private static void submitTimecard() {
        System.out.print("Nhập Employee ID: ");
        int employeeId = Integer.parseInt(scanner.nextLine());
        
        Timecard timecard = timecards.get(employeeId);
        if (timecard == null) {
            System.out.println("Không tìm thấy timecard cho nhân viên này.");
            return;
        }
        
        timecard.submit();
    }
}
```
