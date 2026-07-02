# TÀI LIỆU ĐẶC TẢ USER STORIES & USE CASES (eKYC ABC BANK)
**Dự án:** Số hóa quy trình mở tài khoản và phát hành thẻ trực tuyến  
**Vai trò:** Senior Business Analyst (FinTech & Digital Banking)

Tài liệu này được phát triển dựa trên kết quả phân tích hệ thống eKYC tại [ekyc_brd_analysis.md](file:///C:/Users/LOQ%202023/.gemini/antigravity/brain/d28ff58d-6276-4c32-825d-0aa1c120063e/ekyc_brd_analysis.md), phân tách các yêu cầu nghiệp vụ thành các User Stories phục vụ phát triển Agile và các Use Cases chi tiết phục vụ thiết kế hệ thống.

---

## PHẦN 1: DANH SÁCH USER STORIES (AGILE/SCRUM)

### EPIC 1: ĐĂNG KÝ & XÁC THỰC BAN ĐẦU (ONBOARDING)

#### **US-01: Nhập Số điện thoại & Nhận SMS OTP**
*   **User Story:**  
    *As a* Khách hàng tiềm năng,  
    *I want to* nhập Số điện thoại cá nhân và nhận mã SMS OTP tức thì,  
    *So that* tôi có thể xác thực số điện thoại và bắt đầu quá trình mở tài khoản.
*   **Acceptance Criteria (AC):**
    1.  **Given** Khách hàng ở màn hình Onboarding, **When** Khách hàng nhập số điện thoại hợp lệ (10 chữ số, đúng đầu số Việt Nam) và bấm "Gửi OTP", **Then** Hệ thống phải kiểm tra xem số điện thoại đã đăng ký tài khoản chưa.
    2.  Nếu số điện thoại chưa từng đăng ký, hệ thống kích hoạt SMS Gateway gửi mã OTP gồm 6 chữ số qua tin nhắn định danh (SMS Brandname) của ABC Bank trong vòng dưới 10 giây.
    3.  Hệ thống hiển thị màn hình nhập OTP kèm đồng hồ đếm ngược 120 giây.
    4.  Nếu số điện thoại đã tồn tại trong hệ thống, hiển thị cảnh báo: *"Số điện thoại đã được đăng ký tài khoản. Vui lòng Đăng nhập hoặc Khôi phục mật khẩu"* kèm nút chuyển hướng.

#### **US-02: Xác thực OTP thành công**
*   **User Story:**  
    *As a* Khách hàng tiềm năng,  
    *I want to* nhập mã OTP được gửi đến điện thoại của tôi,  
    *So that* tôi có thể chứng minh quyền sở hữu số điện thoại và đi tiếp sang bước chụp giấy tờ.
*   **Acceptance Criteria (AC):**
    1.  **Given** Khách hàng đang ở màn hình nhập OTP, **When** Khách hàng nhập đúng 6 chữ số OTP trước khi hết thời gian đếm ngược, **Then** Hệ thống xác thực thành công và tự động chuyển sang bước "Chụp CCCD".
    2.  Nếu khách hàng nhập sai OTP, hệ thống phải hiển thị thông báo lỗi đỏ: *"Mã OTP không chính xác. Vui lòng kiểm tra lại."*
    3.  Nếu nhập sai quá 3 lần liên tiếp, hệ thống phải vô hiệu hóa tính năng gửi/nhập OTP của số điện thoại đó trong vòng 24 giờ để ngăn ngừa brute-force.
    4.  Nếu hết thời gian đếm ngược mà chưa nhập, hiển thị nút "Gửi lại mã OTP" (giới hạn tối đa yêu cầu gửi lại 3 lần/phiên giao dịch).

---

### EPIC 2: ĐỊNH DANH SINH TRẮC HỌC & GIẤY TỜ TÙY THÂN (OCR & BIOMETRICS)

#### **US-03: Chụp ảnh CCCD & Trích xuất OCR tự động**
*   **User Story:**  
    *As a* Khách hàng tiềm năng,  
    *I want to* chụp ảnh mặt trước và mặt sau CCCD thông qua camera điện thoại,  
    *So that* hệ thống có thể tự động điền các thông tin cá nhân của tôi mà tôi không cần gõ thủ công.
*   **Acceptance Criteria (AC):**
    1.  **Given** Khách hàng đã xác thực OTP thành công, **When** Khách hàng cấp quyền truy cập camera và chụp ảnh CCCD, **Then** Hệ thống phải kiểm tra chất lượng ảnh thời gian thực ở client (độ sáng, độ sắc nét, không bị mất góc/lóa sáng).
    2.  Nếu chất lượng ảnh không đạt, hiển thị hướng dẫn chụp lại ngay lập tức (giới hạn tối đa chụp lại 3 lần).
    3.  Nếu ảnh đạt chuẩn, hệ thống eKYC Engine chạy OCR để trích xuất: Họ tên, Số CCCD, Ngày sinh, Quê quán, Địa chỉ thường trú, Ngày hết hạn.
    4.  Độ chính xác trích xuất ký tự (chữ và số) phải đạt tối thiểu 98% đối với giấy tờ không bị hư hại vật lý.
    5.  Hệ thống phải tự động phát hiện nếu ảnh chụp là photocopy, in ấn giả mạo hoặc chụp từ màn hình thiết bị khác và hiển thị lỗi từ chối mở tài khoản: *"Vui lòng sử dụng giấy tờ gốc để xác thực"*.

#### **US-04: Quét khuôn mặt Liveness Check**
*   **User Story:**  
    *As a* Khách hàng tiềm năng,  
    *I want to* quét khuôn mặt theo hướng dẫn động của camera di động,  
    *So that* hệ thống có thể xác minh tôi là một người thật đang thao tác trực tiếp.
*   **Acceptance Criteria (AC):**
    1.  **Given** Khách hàng đã hoàn tất bước trích xuất giấy tờ, **When** Khách hàng quét khuôn mặt bằng camera trước, **Then** Hệ thống phải thực hiện kiểm tra Liveness (kiểm tra thực thể sống) động (chớp mắt, quay đầu hoặc thay đổi khoảng cách theo chỉ thị trên màn hình).
    2.  Hệ thống phải chặn được các hành vi giả mạo: ảnh chụp chân dung, video phát lại (replay attack), mặt nạ 3D hoặc deepfake dạng cơ bản.
    3.  Thời gian xử lý Liveness Check trên máy chủ không được quá 2 giây. Nếu phát hiện giả mạo, báo lỗi từ chối mở tài khoản ngay lập tức.

#### **US-05: So khớp khuôn mặt (Face Matching)**
*   **User Story:**  
    *As a* Khách hàng tiềm năng,  
    *I want* hệ thống tự động so khớp khuôn mặt vừa quét với khuôn mặt trên CCCD của tôi,  
    *So that* tôi được chứng minh là chính chủ của giấy tờ đó mà không cần gặp giao dịch viên.
*   **Acceptance Criteria (AC):**
    1.  **Given** Kết quả quét khuôn mặt Liveness đạt yêu cầu, **When** Hệ thống tiến hành so sánh đối khớp 1:1 khuôn mặt trực tiếp với ảnh chân dung trên CCCD, **Then** Hệ thống tính toán điểm tương đồng (Confidence Score).
    2.  Nếu Điểm tương đồng >= 80% (ngưỡng mặc định cấu hình), hệ thống tự động phê duyệt bước này và chuyển sang bước đối soát AML/PEP.
    3.  Nếu Điểm tương đồng < 80%, hệ thống từ chối mở tài khoản tự động và gửi cảnh báo lý do: *"Nhận diện khuôn mặt không trùng khớp với giấy tờ tùy thân"*.

---

### EPIC 3: PHÒNG CHỐNG GIAN LẬN & KIỂM TRA TUÂN THỦ (FRAUD & COMPLIANCE)

#### **US-06: Đối soát thông tin thuê bao di động**
*   **User Story:**  
    *As a* Hệ thống phòng chống gian lận của ABC Bank,  
    *I want to* gọi API kết nối với Nhà mạng Viễn thông để đối sánh thông tin số điện thoại đăng ký của khách hàng,  
    *So that* ngăn chặn hành vi sử dụng số điện thoại của người khác để mở tài khoản bất hợp pháp.
*   **Acceptance Criteria (AC):**
    1.  **Given** Khách hàng hoàn tất xác thực sinh trắc học, **When** Hệ thống thực hiện kiểm tra điều kiện mở tài khoản, **Then** Hệ thống gửi thông tin (Số điện thoại, Họ tên, Số CCCD) sang nhà mạng viễn thông sở hữu đầu số đó.
    2.  Nếu nhà mạng trả về kết quả *"Khớp thông tin"*, hệ thống phê duyệt đi tiếp.
    3.  Nếu nhà mạng trả về kết quả *"Không khớp thông tin"* (Số điện thoại không chính chủ), hệ thống hiển thị cảnh báo rủi ro cao và đưa vào diện hậu kiểm hoặc từ chối tùy cấu hình rủi ro hiện tại.

#### **US-07: Tra cứu Blacklist & AML/PEP**
*   **User Story:**  
    *As a* Chuyên viên Tuân thủ (Compliance Officer) của ngân hàng,  
    *I want* hệ thống tự động tra cứu họ tên và số CCCD khách hàng trong cơ sở dữ liệu AML/PEP/Blacklist,  
    *So that* ngăn chặn rủi ro rửa tiền, tài trợ khủng bố hoặc cung cấp dịch vụ cho các đối tượng bị cấm vận pháp lý.
*   **Acceptance Criteria (AC):**
    1.  **Given** Dữ liệu OCR CCCD đã sẵn có, **When** Hệ thống tiến hành kiểm tra điều kiện phát hành tài khoản, **Then** Hệ thống gọi API tra cứu danh sách đen quốc tế (OFAC, Interpol), danh sách PEP và danh sách đen nội bộ của ABC Bank.
    2.  Nếu phát hiện trùng khớp thông tin danh sách đen (Blacklist): Hệ thống lập tức từ chối phê duyệt mở tài khoản tự động, lưu trạng thái "Từ chối - Nghi vấn AML" và ghi nhận log giao dịch.
    3.  Nếu phát hiện trùng khớp danh sách PEP (Chính trị gia): Hệ thống đánh dấu mức rủi ro cao và chuyển hồ sơ về trạng thái chờ phê duyệt của cấp Quản lý Tuân thủ (không mở tự động trực tuyến).

---

### EPIC 4: TẠO TÀI KHOẢN & PHÁT HÀNH DỊCH VỤ (PROVISIONING)

#### **US-08: Xem và Ký Hợp đồng điện tử (E-contract)**
*   **User Story:**  
    *As a* Khách hàng tiềm năng,  
    *I want to* xem bản hợp đồng mở tài khoản và ký kết bằng mã OTP ngay trên ứng dụng,  
    *So that* tôi hoàn tất thủ tục pháp lý thiết lập quan hệ giao dịch với ngân hàng mà không cần ký giấy tờ vật lý.
*   **Acceptance Criteria (AC):**
    1.  **Given** Khách hàng đã kiểm tra thông tin cá nhân chính xác, **When** Khách hàng chọn đồng ý điều khoản dịch vụ, **Then** Hệ thống tạo file PDF hợp đồng mở tài khoản có đầy đủ thông tin cá nhân của khách hàng.
    2.  Khách hàng có thể đọc toàn văn hợp đồng trực tiếp trên ứng dụng.
    3.  Khi bấm "Ký hợp đồng", hệ thống gửi mã xác thực ký số (OTP) qua SMS. Khách hàng nhập đúng mã OTP này để hoàn tất ký kết. Hợp đồng điện tử được lưu trữ kèm mã Hash bảo mật chống sửa đổi.

#### **US-09: Khởi tạo CIF & Mở tài khoản tự động trên Core Banking**
*   **User Story:**  
    *As a* Khách hàng đã ký hợp đồng thành công,  
    *I want* hệ thống tự động kích hoạt tài khoản thanh toán và gửi thông tin đăng nhập ngân hàng số cho tôi trong thời gian thực,  
    *So that* tôi có thể sử dụng các dịch vụ chuyển tiền, thanh toán của ABC Bank ngay lập tức.
*   **Acceptance Criteria (AC):**
    1.  **Given** Hợp đồng điện tử được ký kết thành công, **When** Hệ thống eKYC gọi API Core Banking, **Then** Core Banking khởi tạo CIF mới và số tài khoản thanh toán tự động trong tối đa 3 giây.
    2.  Hệ thống tự động kích hoạt trạng thái hoạt động (Active) cho tài khoản thanh toán với hạn mức giao dịch ban đầu tối đa 100 triệu VND/tháng theo Thông tư 16.
    3.  Gửi tin nhắn SMS Brandname/Email cho khách hàng thông báo: *"Mở tài khoản thành công! Số tài khoản của Quý khách là XXXXXXXXXX. Vui lòng đăng nhập ứng dụng bằng SĐT để kích hoạt dịch vụ."*

---

### EPIC 5: HẬU KIỂM & QUẢN TRỊ THAM SỐ (BACK-OFFICE OPERATIONS)

#### **US-10: Tra cứu và xử lý hồ sơ eKYC cảnh báo nghi ngờ**
*   **As a** Chuyên viên Kiểm soát Rủi ro (Fraud Analyst),  
    **I want to** xem danh sách các hồ sơ đăng ký eKYC bị hệ thống đánh dấu cảnh báo rủi ro (Face Match tiệm cận ngưỡng hoặc số điện thoại không trùng khớp),  
    **So that** tôi có thể hậu kiểm thủ công nhanh chóng để phê duyệt hoặc từ chối, đảm bảo không bỏ sót khách hàng thực tế và ngăn chặn kẻ gian lận.
*   **Acceptance Criteria (AC):**
    1.  Hệ thống Back-office cung cấp trang danh sách hồ sơ cần hậu kiểm với bộ lọc: Thời gian, Điểm tương đồng khuôn mặt, Loại cảnh báo (Liveness warning, Telco mismatch, PEP warning).
    2.  Khi nhấn vào chi tiết một hồ sơ, chuyên viên phải xem được đầy đủ: Ảnh CCCD mặt trước/sau, thông tin OCR, video/ảnh quét khuôn mặt, điểm số khớp và lý do hệ thống đưa vào danh sách cảnh báo.
    3.  Chuyên viên có thể bấm nút "Phê duyệt" (Approve) hoặc "Từ chối" (Reject) kèm lý do ghi chú bắt buộc. Trạng thái sau phê duyệt phải cập nhật đồng bộ sang Core Banking lập tức.

#### **US-11: Cấu hình tham số ngưỡng phê duyệt eKYC**
*   **As a** Quản trị viên Hệ thống (System Admin),  
    **I want to** cấu hình các tham số ngưỡng đối khớp sinh trắc học và giới hạn số lần thử lại trực tiếp trên cổng admin,  
    **So that** ngân hàng có thể linh hoạt điều chỉnh chính sách kiểm soát rủi ro theo từng thời kỳ mà không cần can thiệp vào mã nguồn phần mềm.
*   **Acceptance Criteria (AC):**
    1.  Hệ thống cung cấp màn hình quản lý tham số chỉ cho phép người dùng có quyền Admin truy cập.
    2.  Admin có thể chỉnh sửa các tham số sau:
        *   Ngưỡng tối thiểu Face Matching (mặc định 80%, dải cho phép chỉnh sửa: 50% - 99%).
        *   Số lần thử OTP tối đa (mặc định 3 lần).
        *   Số lần chụp lại ảnh CCCD tối đa (mặc định 3 lần).
        *   Thời gian tạm khóa thiết bị khi eKYC thất bại liên tục (mặc định 2 giờ).
    3.  Mọi thao tác chỉnh sửa tham số phải được ghi vết lịch sử (Audit Log) bao gồm: Người thực hiện, Giá trị cũ, Giá trị mới, Thời gian thay đổi và cần cơ chế duyệt kép (Maker - Checker) trước khi áp dụng.

---

## PHẦN 2: DANH SÁCH USE CASES CHI TIẾT (UML/IEEE)

### MODULE 1: ĐĂNG KÝ & XÁC THỰC BAN ĐẦU

#### **UC-01: Đăng ký Số điện thoại và Xác thực OTP**
*   **Use Case ID:** UC-01
*   **Use Case Name:** Đăng ký Số điện thoại và Xác thực OTP
*   **Primary Actor:** Khách hàng (Customer)
*   **Brief Description:** Khách hàng nhập số điện thoại trên ứng dụng di động để nhận mã OTP và thực hiện xác thực để mở phiên đăng ký eKYC mới.
*   **Precondition:** 
    *   Khách hàng đã tải ứng dụng ABC Bank Mobile về điện thoại.
    *   Thiết bị di động có kết nối Internet ổn định.
*   **Main Flow (Luồng chính):**
    1.  Khách hàng mở ứng dụng, chọn chức năng "Mở tài khoản trực tuyến".
    2.  Hệ thống hiển thị màn hình nhập Số điện thoại và Email.
    3.  Khách hàng nhập Số điện thoại và Email, sau đó bấm nút "Tiếp tục".
    4.  Hệ thống kiểm tra định dạng Số điện thoại và Email hợp lệ, gọi dịch vụ SMS Gateway tạo và gửi mã OTP 6 chữ số đến số điện thoại vừa nhập.
    5.  Hệ thống chuyển sang giao diện nhập OTP kèm đồng hồ đếm ngược 120 giây.
    6.  Khách hàng nhận được tin nhắn SMS chứa OTP, nhập mã OTP vào ứng dụng.
    7.  Hệ thống xác thực mã OTP chính xác và chuyển khách hàng sang bước tiếp theo (UC-02).
*   **Alternative Flow (Luồng thay thế - Hết hạn OTP):**
    *   Tại bước 5, nếu hết 120 giây mà khách hàng chưa nhập OTP, ứng dụng hiển thị nút "Gửi lại OTP". Khách hàng bấm nút này, hệ thống sẽ thực hiện lại bước 4.
*   **Exception Flow (Luồng ngoại lệ - Lỗi xác thực):**
    *   *Số điện thoại đã tồn tại:* Tại bước 4, nếu số điện thoại đã tồn tại trong Core Banking, hệ thống hiển thị cảnh báo: *"Số điện thoại đã đăng ký. Vui lòng đăng nhập hoặc chọn Quên mật khẩu"* và kết thúc Use Case.
    *   *Nhập sai OTP:* Tại bước 6, nếu khách hàng nhập sai OTP, hệ thống báo lỗi *"Mã OTP không chính xác"*. Nếu nhập sai liên tiếp quá 3 lần, hệ thống hiển thị thông báo khóa chức năng đăng ký đối với số điện thoại này trong 24 giờ và kết thúc Use Case.
*   **Postcondition:** Phiên giao dịch (Session ID) được khởi tạo và số điện thoại được xác thực thành công.

---

### MODULE 2: NHẬN DIỆN GIẤY TỜ TÙY THÂN

#### **UC-02: Xác thực Giấy tờ tùy thân qua OCR**
*   **Use Case ID:** UC-02
*   **Use Case Name:** Xác thực Giấy tờ tùy thân qua OCR
*   **Primary Actor:** Khách hàng (Customer)
*   **Brief Description:** Khách hàng sử dụng camera để chụp ảnh hai mặt CCCD gốc; hệ thống tự động kiểm tra chất lượng ảnh, trích xuất dữ liệu thông tin cá nhân và thực hiện chống giả mạo giấy tờ.
*   **Precondition:** Khách hàng đã hoàn tất xác thực OTP thành công (UC-01) và cấp quyền truy cập camera cho ứng dụng.
*   **Main Flow (Luồng chính):**
    1.  Hệ thống hiển thị hướng dẫn chụp ảnh mặt trước CCCD.
    2.  Khách hàng đưa mặt trước CCCD vào khung hình hiển thị và bấm chụp.
    3.  Hệ thống Client SDK tự động kiểm tra chất lượng ảnh chụp (đủ sáng, rõ nét, không mất góc). Ảnh đạt chuẩn được gửi lên máy chủ eKYC Engine.
    4.  Hệ thống tiếp tục hướng dẫn chụp mặt sau CCCD. Khách hàng thực hiện chụp mặt sau và gửi lên máy chủ tương tự.
    5.  Hệ thống eKYC Engine thực hiện quét OCR để trích xuất thông tin cá nhân từ ảnh giấy tờ.
    6.  Hệ thống chạy thuật toán phát hiện gian lận giấy tờ (chống photocopy, cắt ghép, chụp màn hình).
    7.  Hệ thống hiển thị thông tin trích xuất lên màn hình (Họ tên, Ngày sinh, Số CCCD, Quê quán, Địa chỉ thường trú, Ngày cấp, Ngày hết hạn) để khách hàng xác nhận.
    8.  Khách hàng bấm xác nhận thông tin chính xác và chuyển sang bước tiếp theo (UC-03).
*   **Alternative Flow (Luồng thay thế - Thay đổi thông tin liên hệ):**
    *   Tại bước 7, khách hàng phát hiện địa chỉ liên hệ hiện tại khác địa chỉ thường trú trên CCCD. Khách hàng chọn chức năng chỉnh sửa "Địa chỉ liên hệ hiện tại" và điền địa chỉ mới trước khi bấm xác nhận. (Hệ thống khóa không cho phép sửa đổi Họ tên, Ngày sinh và Số CCCD trích xuất từ OCR).
*   **Exception Flow (Luồng ngoại lệ - Lỗi ảnh và từ chối giấy tờ):**
    *   *Ảnh không đạt chất lượng:* Tại bước 3 hoặc bước 4, nếu ảnh bị mờ hoặc mất góc, hệ thống hiển thị thông báo lỗi yêu cầu chụp lại. Nếu thử lại quá 3 lần vẫn lỗi, hệ thống hủy phiên đăng ký và yêu cầu khách hàng ra quầy giao dịch.
    *   *Giấy tờ không hợp lệ/Hết hạn:* Tại bước 5/6, nếu hệ thống OCR phát hiện CCCD đã hết hạn sử dụng, hoặc thuật toán phát hiện giấy tờ là bản photocopy/bản in lại/chụp từ màn hình khác, hệ thống hiển thị màn hình từ chối dịch vụ: *"Giấy tờ không hợp lệ. Vui lòng sử dụng giấy tờ gốc còn hạn sử dụng."* và kết thúc Use Case.
*   **Postcondition:** Dữ liệu thông tin cá nhân trích xuất từ CCCD được lưu trữ tạm thời trong phiên đăng ký.

---

### MODULE 3: ĐỐI KHỚP SINH TRẮC HỌC

#### **UC-03: Xác thực sinh trắc học khuôn mặt**
*   **Use Case ID:** UC-03
*   **Use Case Name:** Xác thực sinh trắc học khuôn mặt (Liveness & Face Match)
*   **Primary Actor:** Khách hàng (Customer)
*   **Brief Description:** Khách hàng thực hiện quét khuôn mặt di động để chứng thực thực thể sống và so khớp với ảnh chụp chân dung trên giấy tờ tùy thân.
*   **Precondition:** Khách hàng đã xác thực thông tin giấy tờ tùy thân thành công (UC-02).
*   **Main Flow (Luồng chính):**
    1.  Hệ thống hiển thị hướng dẫn xác thực khuôn mặt (yêu cầu không đeo kính râm, khẩu trang, đứng nơi đủ sáng).
    2.  Khách hàng đưa khuôn mặt vào khung tròn camera trước của ứng dụng.
    3.  Hệ thống kích hoạt camera và chạy thuật toán **Liveness Detection** (yêu cầu khách hàng quay trái, quay phải, mỉm cười hoặc nhấp nháy mắt theo chỉ dẫn động trên màn hình).
    4.  Client SDK gửi dữ liệu video/hình ảnh khuôn mặt về máy chủ eKYC Engine.
    5.  Hệ thống eKYC Engine xác nhận Liveness thành công (chứng minh người thật) và thực hiện thuật toán **Face Matching 1:1** giữa ảnh quét trực tiếp và ảnh chân dung trích xuất từ CCCD ở UC-02.
    6.  Hệ thống tính toán điểm tương đồng đạt **85%** (vượt ngưỡng quy định 80%).
    7.  Hệ thống hiển thị màn hình chúc mừng xác thực khuôn mặt thành công và chuyển sang bước ký hợp đồng (UC-04).
*   **Exception Flow (Luồng ngoại lệ - Liveness/Face Match thất bại):**
    *   *Liveness thất bại:* Tại bước 5, nếu hệ thống phát hiện đây là hình ảnh giả mạo (ảnh in, video phát lại), hệ thống lập tức khóa phiên giao dịch, hiển thị màn hình cảnh báo từ chối dịch vụ và kết thúc Use Case.
    *   *Face Match không khớp:* Tại bước 6, nếu điểm tương đồng khuôn mặt dưới 80%, hệ thống ghi nhận lỗi không khớp sinh trắc học. Khách hàng được yêu cầu thử lại tối đa 2 lần. Nếu sau 2 lần vẫn thất bại, hệ thống báo lỗi không khớp sinh trắc học, kết thúc phiên giao dịch tự động.
*   **Postcondition:** Thông tin sinh trắc học khuôn mặt được đối khớp thành công và liên kết vào hồ sơ đăng ký.

---

### MODULE 5/6: HẬU KIỂM & QUẢN TRỊ HỆ THỐNG

#### **UC-04: Hậu kiểm hồ sơ eKYC nghi vấn gian lận**
*   **Use Case ID:** UC-04
*   **Use Case Name:** Hậu kiểm hồ sơ eKYC nghi vấn gian lận
*   **Primary Actor:** Giao dịch viên Kiểm soát (Back-office Analyst)
*   **Brief Description:** Giao dịch viên thực hiện kiểm tra thủ công các hồ sơ eKYC bị hệ thống gắn cờ nghi ngờ (ngưỡng Face Match cận biên, số thuê bao không trùng khớp) để quyết định phê duyệt mở tài khoản hoặc từ chối giao dịch.
*   **Precondition:** Hệ thống eKYC Engine đã đưa hồ sơ nghi ngờ vào danh sách chờ duyệt (Pending Review Queue) trên cổng quản trị Admin Portal.
*   **Main Flow (Luồng chính):**
    1.  Giao dịch viên đăng nhập vào hệ thống Admin Portal bằng tài khoản nghiệp vụ được phân quyền.
    2.  Giao dịch viên truy cập trang "Quản lý Hồ sơ eKYC Hàng chờ Duyệt".
    3.  Hệ thống hiển thị danh sách hồ sơ bị cảnh báo rủi ro kèm theo nhãn lý do cụ thể.
    4.  Giao dịch viên chọn một hồ sơ cụ thể để xem chi tiết.
    5.  Hệ thống hiển thị:
        *   Ảnh chụp CCCD mặt trước/sau của khách hàng cùng thông tin OCR trích xuất.
        *   Ảnh chân dung cắt từ camera quét trực tiếp và điểm số khớp (ví dụ: 78% - cảnh báo tiệm cận).
        *   Thông báo lỗi đối soát thông tin thuê bao của nhà mạng viễn thông.
    6.  Giao dịch viên xem xét kỹ lưỡng hình ảnh trực quan, đối chiếu khuôn mặt người thật và ảnh trên giấy tờ, xác định khách hàng là chính chủ (điểm thấp do ánh sáng lúc chụp).
    7.  Giao dịch viên bấm nút "Phê duyệt" (Approve), nhập ghi chú nghiệp vụ hợp lệ và bấm xác nhận.
    8.  Hệ thống eKYC gửi chỉ thị sang Core Banking để kích hoạt CIF, khởi tạo số tài khoản tự động và gửi SMS chúc mừng khách hàng mở tài khoản thành công.
*   **Alternative Flow (Luồng thay thế - Từ chối hồ sơ):**
    *   Tại bước 6, giao dịch viên phát hiện ảnh chân dung quét trực tiếp khác biệt hoàn toàn với ảnh trên CCCD (nghi ngờ giả mạo danh tính cố ý). Giao dịch viên bấm nút "Từ chối" (Reject), chọn lý do từ chối phù hợp từ danh sách có sẵn (ví dụ: *"Thông tin sinh trắc học không khớp"*), và bấm xác nhận. Hệ thống ghi nhận trạng thái từ chối vĩnh viễn cho hồ sơ đăng ký này và gửi email/SMS thông báo từ chối mở tài khoản cho khách hàng.
*   **Postcondition:** Hồ sơ eKYC chuyển từ trạng thái "Pending" sang "Approved" hoặc "Rejected". Mọi thao tác phê duyệt được ghi vết đầy đủ vào hệ thống Audit Log.
