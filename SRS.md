# BẢN ĐẶC TẢ YÊU CẦU PHẦN MỀM (SRS)
## HỆ THỐNG ĐẶT XE TRỰC TUYẾN - CAB SYSTEM

---

## 1. TỔNG QUAN DỰ ÁN (PROJECT OVERVIEW)

- **Tên dự án:** CAB System – Nền tảng đặt xe trực tuyến hướng dịch vụ (Service-Oriented Architecture).
- **Mục tiêu:** Xây dựng hệ thống đặt xe quy mô lớn, linh hoạt, giải quyết các hạn chế của hệ thống cũ (phân công thủ công, khó theo dõi chuyến, thanh toán phân tán, khó mở rộng).
- **Thời gian triển khai:** 7 tuần.
- **Đối tượng sử dụng chính:** Khách hàng (Customer), Tài xế (Driver), Nhân viên vận hành & Quản trị viên (Operator / Admin).

---

## 2. CÁC TÁC NHÂN TRONG HỆ THỐNG (ACTORS)

| Tác nhân (Actor) | Mô tả vai trò |
| :--- | :--- |
| **Khách hàng (Customer)** | Đăng ký, đăng nhập, tìm chuyến, xem cước phí, theo dõi tài xế thời gian thực, thanh toán và đánh giá chuyến đi. |
| **Tài xế (Driver)** | Bật/tắt trạng thái làm việc, gửi vị trí GPS, nhận/từ chối chuyến xe, cập nhật trạng thái đón và trả khách. |
| **Nhân viên vận hành (Operator)** | Giám sát các chuyến đi đang hoạt động, can thiệp xử lý sự cố, hỗ trợ điều phối và hỗ trợ khách hàng/tài xế. |
| **Quản trị viên (Admin)** | Quản lý người dùng, duyệt hồ sơ tài xế/phương tiện, phân quyền hệ thống, xem báo cáo doanh thu & hiệu suất. |
| **Hệ thống bên thứ ba (External Services)** | Cổng thanh toán (Payment Gateway), Dịch vụ bản đồ (Map API), Dịch vụ thông báo (Push/SMS Notification Provider). |

---

## 3. YÊU CẦU CHỨC NĂNG CHI TIẾT (FUNCTIONAL REQUIREMENTS)

### 3.1. Phân hệ Quản lý Tài khoản & Xác thực (Identity & User Management)
* **FR-AUTH-01 (Đăng ký tài khoản):** 
  * Khách hàng tự đăng ký tài khoản qua ứng dụng (Số điện thoại / Email / Mật khẩu).
  * Tài xế đăng ký hồ sơ hoặc được nhân viên vận hành tạo tài khoản trên hệ thống.
* **FR-AUTH-02 (Đăng nhập & Xác thực):** Xác thực an toàn đa vai trò (Customer, Driver, Operator, Admin) sử dụng cơ chế Token-based Authentication (JWT).
* **FR-AUTH-03 (Quản lý hồ sơ cá nhân):** Cho phép người dùng xem và cập nhật thông tin cá nhân (Họ tên, ảnh đại diện, số liên lạc).
* **FR-AUTH-04 (Quản lý hồ sơ tài xế & phương tiện):** Cho phép cập nhật và lưu trữ bằng lái xe, thông tin xe (biển số, hãng xe, màu xe) và phân loại xe (4 chỗ, 7 chỗ, xe máy,...).

---

### 3.2. Phân hệ Quản lý Trạng thái & Vị trí Tài xế (Driver & Location Management)
* **FR-DRV-01 (Cập nhật trạng thái làm việc):** Tài xế chuyển đổi trạng thái: *Sẵn sàng nhận chuyến (Online)*, *Đang bận (Busy)*, *Nghỉ làm (Offline)*.
* **FR-DRV-02 (Cập nhật vị trí GPS thời gian thực):** Định kỳ gửi và lưu trữ tọa độ của tài xế khi ở trạng thái Online.
* **FR-DRV-03 (Tìm kiếm tài xế lân cận):** Cung cấp khả năng tìm kiếm danh sách tài xế rảnh trong bán kính gần điểm đón của khách hàng.

---

### 3.3. Phân hệ Đặt xe & Điều phối Chuyến đi (Booking & Dispatching)
* **FR-BOOK-01 (Tạo yêu cầu đặt xe):** Khách hàng nhập điểm đón, điểm đến, lựa chọn loại dịch vụ/xe và gửi yêu cầu.
* **FR-BOOK-02 (Ước tính cước & thời gian di chuyển):** Tính toán và hiển thị giá cước dự kiến (Fare Estimate) và thời gian tài xế đến (ETA) trước khi xác nhận đặt xe.
* **FR-BOOK-03 (Tự động tìm kiếm & Đề xuất tài xế):** Thuật toán tự động tìm tài xế tối ưu nhất dựa trên vị trí gần nhất, trạng thái sẵn sàng và tiêu chí vận hành.
* **FR-BOOK-04 (Xử lý phản hồi từ tài xế):** Gửi thông báo cuốc xe tới tài xế; tài xế có thời gian quy định (Timeout) để *Chấp nhận (Accept)* hoặc *Từ chối (Reject)*.
* **FR-BOOK-05 (Tự động chuyển tiếp tìm tài xế khác):** Nếu tài xế từ chối hoặc hết giờ phản hồi, hệ thống tự động tìm và chuyển chuyến sang tài xế tiếp theo mà khách hàng không cần tạo lại yêu cầu.
* **FR-BOOK-06 (Xử lý không tìm thấy tài xế):** Thông báo rõ ràng cho khách hàng khi không có tài xế phù hợp sau thời gian tìm kiếm.
* **FR-BOOK-07 (Hủy chuyến đi):** Cho phép khách hàng hoặc tài xế hủy chuyến theo quy định và lý do hủy chuyến.

---

### 3.4. Phân hệ Quản lý Tiến trình Chuyến đi (Trip Execution & Tracking)
* **FR-TRIP-01 (Cập nhật trạng thái chuyến đi):** Tài xế cập nhật tuần tự các mốc trạng thái:
  * `Đã nhận chuyến (Accepted)`
  * `Đã đến điểm đón (Arrived at Pickup)`
  * `Đã đón khách / Đang di chuyển (In Trip / Started)`
  * `Hoàn thành chuyến (Completed)`
* **FR-TRIP-02 (Theo dõi chuyến đi trực tuyến - Live Tracking):** Khách hàng theo dõi vị trí di chuyển của tài xế trên bản đồ và thời gian dự kiến đến điểm đón/điểm đến.
* **FR-TRIP-03 (Lịch sử chuyến đi):** Cho phép khách hàng và tài xế tra cứu danh sách lịch sử các chuyến đã thực hiện (lộ trình, thời gian, chi phí, tài xế/khách hàng).

---

### 3.5. Phân hệ Tính cước & Thanh toán (Pricing & Payment)
* **FR-PAY-01 (Tính toán cước phí chính thức):** Tự động tính cước sau khi hoàn thành chuyến đi dựa trên loại dịch vụ, quãng đường thực tế, thời gian di chuyển và phụ phí phát sinh.
* **FR-PAY-02 (Thanh toán tiền mặt - Cash):** Cho phép khách hàng trả tiền mặt trực tiếp cho tài xế; tài xế bấm xác nhận đã thu tiền.
* **FR-PAY-03 (Thanh toán điện tử - Digital Payment):** Tích hợp cổng thanh toán bên thứ ba (Ví điện tử, Thẻ ngân hàng), tuân thủ nguyên tắc không lưu trữ thông tin nhạy cảm của thẻ trên CAB System.
* **FR-PAY-04 (Xử lý lỗi thanh toán):** Thông báo ngay khi giao dịch điện tử thất bại và hỗ trợ thanh toán lại (Retry) hoặc chuyển sang trả tiền mặt.

---

### 3.6. Phân hệ Đánh giá & Phản hồi (Rating & Review)
* **FR-REV-01 (Đánh giá sau chuyến đi):** Khách hàng đánh giá mức độ hài lòng (1 - 5 sao) và để lại phản hồi/nhận xét về tài xế sau khi hoàn thành chuyến.
* **FR-REV-02 (Tổng hợp điểm chất lượng):** Hệ thống tính toán điểm trung bình sao của tài xế để đánh giá mức độ uy tín.

---

### 3.7. Phân hệ Thông báo (Notification Service)
* **FR-NOTI-01 (Thông báo cho Khách hàng):** Gửi thông báo đẩy (Push notification / SMS) khi: Yêu cầu được tiếp nhận, Có tài xế nhận, Tài xế đến điểm đón, Bắt đầu chuyến, Hoàn thành và Kết quả thanh toán.
* **FR-NOTI-02 (Thông báo cho Tài xế):** Gửi thông báo khi: Có chuyến mới được phân phối, Khách hủy chuyến, Thay đổi lộ trình.
* **FR-NOTI-03 (Mở rộng đa kênh thông báo):** Thiết kế độc lập cho phép cắm thêm các nhà cung cấp thông báo khác (FCM, Twilio SMS, Email) mà không ảnh hưởng luồng nghiệp vụ.

---

### 3.8. Phân hệ Quản trị & Vận hành (Admin & Operations Portal)
* **FR-ADM-01 (Quản lý người dùng & phương tiện):** Xem danh sách, kích hoạt/khóa tài khoản khách hàng, tài xế và phê duyệt phương tiện.
* **FR-ADM-02 (Giám sát trực tiếp chuyến đi):** Theo dõi bản đồ trực quan các chuyến đi đang hoạt động và vị trí tài xế theo thời gian thực.
* **FR-ADM-03 (Xử lý sự cố chuyến đi):** Can thiệp xử lý các chuyến bị lỗi, hủy chuyến khẩn cấp, gán lại tài xế thủ công.
* **FR-ADM-04 (Đối soát giao dịch thanh toán):** Tra cứu, thống kê và đối soát lịch sử dòng tiền thanh toán.
* **FR-ADM-05 (Báo cáo & Thống kê Dashboard):** Báo cáo số lượng chuyến, doanh thu, tỷ lệ hoàn thành/hủy chuyến, năng suất tài xế theo chu kỳ thời gian.
* **FR-ADM-06 (Phân quyền & Nhật ký kiểm toán):** Phân quyền truy cập theo vai trò (RBAC) và ghi log kiểm toán (Audit Logs) cho các hành động nhạy cảm.

---

## 4. PHÂN RÃ CÁC NGHIỆP VỤ CON (SUB-BUSINESSES) & MA TRẬN PHÂN TÍCH ẢNH HƯỞNG (IMPACT ANALYSIS)

Theo mô hình thiết kế hướng miền (Domain-Driven Design - DDD) và Kiến trúc Hướng dịch vụ (SOA), hệ thống CAB System được phân rã thành **9 nghiệp vụ con (Sub-businesses / Sub-domains)**. Dưới đây là chi tiết chức năng, vai trò và phân tích tác động của từng nghiệp vụ con:

```mermaid
graph TD
    subgraph CoreDomain [Nghiệp vụ Cốt lõi - Core Domains]
        B_Match[3. Điều phối & Ghép chuyến]
        B_Trip[4. Quản lý Chuyến đi]
        B_Pricing[5. Định giá & Tính cước]
    end

    subgraph SupportingDomain [Nghiệp vụ Hỗ trợ - Supporting Domains]
        B_Loc[2. Định vị & Quản lý Tài xế]
        B_Rating[8. Đánh giá & Phản hồi]
        B_Admin[9. Giám sát & Vận hành]
    end

    subgraph GenericDomain [Nghiệp vụ Hạ tầng/Chung - Generic Domains]
        B_Auth[1. Định danh & Xác thực]
        B_Pay[6. Thanh toán & Đối soát]
        B_Noti[7. Thông báo Đa kênh]
    end

    B_Auth --> B_Trip
    B_Loc --> B_Match
    B_Match --> B_Trip
    B_Trip --> B_Pricing
    B_Pricing --> B_Pay
    B_Pay --> B_Rating
    B_Trip -.-> B_Noti
    B_Match -.-> B_Noti
    B_Pay -.-> B_Noti
    B_Trip --> B_Admin
```

---

### Bảng Chi tiết Phân rã Nghiệp vụ con & Đánh giá Tác động

| STT | Nghiệp vụ con (Sub-business) | Phân loại Miền (Domain Type) | Trách nhiệm cốt lõi (Core Responsibility) | Tác động khi Hoạt động bình thường (Positive Impact) | Tác động khi Xảy ra Sự cố (Failure & Ripple Effect) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **1** | **Quản lý Định danh & Xác thực (Identity & Access)** | Generic Sub-domain | Đăng ký, đăng nhập (JWT), phân quyền (RBAC), quản lý hồ sơ cá nhân và kiểm duyệt phương tiện. | Cung cấp định danh tin cậy và cơ chế bảo mật cho mọi yêu cầu gọi dịch vụ trong toàn hệ thống. | **Nghiêm trọng (High):** Người dùng không thể đăng nhập phiên mới. Tuy nhiên, nếu áp dụng JWT Stateless, các phiên đang hoạt động với token hợp lệ vẫn tiếp tục chuyến bình thường. |
| **2** | **Định vị & Theo dõi Tài xế (Location & Telemetry)** | Supporting Sub-domain | Thu thập tọa độ GPS theo thời gian thực (1-3s), quản lý Geo-Spatial Index trên Redis Cache, tìm tài xế gần điểm đón. | Cung cấp dữ liệu vị trí tức thời cho thuật toán điều phối và hỗ trợ tính năng Live Tracking hành trình cho khách hàng. | **Trung bình - Cao (Medium-High):** Khách hàng không xem được xe di chuyển trên bản đồ; Điều phối phải dùng tọa độ gần nhất (Fallback Cache) hoặc mở rộng bán kính tìm kiếm. |
| **3** | **Điều phối & Ghép chuyến (Matching & Dispatching)** | **Core Domain (Cốt lõi)** | Tìm kiếm tài xế tối ưu theo vị trí/tiêu chí, quản lý hàng đợi mời cuốc (15s timeout), tự động chuyển tiếp tài xế khi bị từ chối. | Rút ngắn thời gian chờ xe của khách hàng, tối ưu hóa tỷ lệ nhận chuyến và quãng đường di chuyển rỗng của tài xế. | **Nghiêm trọng (Critical):** Chuyến xe bị treo, khách hàng chờ lâu và hủy app. Cần cơ chế tự động báo "Không tìm thấy xe" hoặc chuyển sang nhân viên điều phối thủ công. |
| **4** | **Quản lý Vòng đời Chuyến đi (Trip Lifecycle Management)** | **Core Domain (Cốt lõi)** | Quản lý máy trạng thái chuyến (`CREATED` ➔ `ACCEPTED` ➔ `ARRIVED` ➔ `IN_TRIP` ➔ `COMPLETED` ➔ `PAID`), lưu vết lộ trình. | Giữ vai trò nhạc trưởng đồng bộ trạng thái giữa Khách hàng, Tài xế và phát sự kiện sang các dịch vụ khác. | **Nghiêm trọng (Critical):** Trạng thái chuyến đi bị lệch giữa khách và tài xế, không thể chuyển tiếp hành trình. Cần cơ chế lưu trạng thái bền vững (State Machine Persistence). |
| **5** | **Định giá & Tính cước (Pricing & Billing)** | Supporting / Core | Ước tính giá trước chuyến (Fare Estimate) và tính toán tổng cước phí chính xác sau chuyến đi dựa trên quãng đường/thời gian/phụ phí. | Tạo sự minh bạch chi phí cho khách hàng, bảo đảm tính đúng doanh thu và hoa hồng tài xế. | **Cao (High):** Không ước tính được giá -> Khách không bấm đặt xe được. Nếu lỗi lúc kết thúc chuyến -> Áp dụng công thức cước cơ bản Fallback (Default Base Fare) dựa trên GPS đã ghi nhận. |
| **6** | **Thanh toán & Đối soát (Payment & Settlement)** | Generic Sub-domain | Tích hợp cổng thanh toán trực tuyến (Momo, VNPay, Thẻ) và thanh toán Tiền mặt (Cash), quản lý giao dịch và đối soát ví. | Xử lý thanh toán nhanh chóng, an toàn không lưu thẻ nhạy cảm, tự động hạch toán doanh thu. | **Trung bình (Medium):** Khi cổng thanh toán bên thứ ba bị sập/chậm, hệ thống **không bị tê liệt** nhờ kiến trúc phân tán; tự động kích hoạt chuyển sang thanh toán **Tiền mặt (Cash)** cho tài xế. |
| **7** | **Thông báo Đa kênh (Notification Service)** | Supporting Sub-domain | Tiếp nhận sự kiện bất đồng bộ từ Event Bus và đẩy thông báo Push (FCM), SMS (Twilio) hoặc In-app notification. | Cung cấp thông tin kịp thời (tài xế đến, trạng thái thanh toán), nâng cao trải nghiệm người dùng. | **Thấp (Low):** Dịch vụ thông báo lỗi không làm gián đoạn luồng đặt xe hay thanh toán chính (Loose Coupling). Khách vẫn xem được trạng thái trên giao diện chính nhờ WebSocket/Polling. |
| **8** | **Đánh giá & Phản hồi (Rating & Quality Control)** | Supporting Sub-domain | Tiếp nhận điểm sao (1-5 sao) và nhận xét của khách, tính điểm trung bình uy tín của tài xế. | Sàng lọc và nâng cao chất lượng dịch vụ; cung cấp chỉ số đánh giá làm đầu vào ưu tiên cho thuật toán điều phối xe. | **Rất thấp (Very Low):** Hoàn toàn không chặn luồng nghiệp vụ di chuyển hay thanh toán của hệ thống. |
| **9** | **Giám sát & Vận hành (Operations & Incident Portal)** | Supporting Sub-domain | Cung cấp Dashboard theo dõi chuyến đi trực tiếp, can thiệp xử lý sự cố (hủy cưỡng bức, gán lại xe), báo cáo doanh thu & kiểm toán. | Cho phép nhân viên vận hành kiểm soát toàn cục, can thiệp các ca sự cố ngoại lệ và hỗ trợ khách hàng kịp thời. | **Trung bình (Medium):** Không ảnh hưởng đến các chuyến xe tự động đang chạy giữa khách và tài xế, nhưng làm chậm khả năng xử lý khiếu nại và giám sát sự cố phát sinh. |

---

## 5. KHOANH VÙNG PHẠM VI DỰ ÁN & GIỚI HẠN MODULE PHÁT TRIỂN (PROJECT SCOPE & BOUNDARIES)

Do thời gian thực hiện đồ án giới hạn trong **7 tuần** theo chuẩn môn học Kiến trúc Hướng Dịch Vụ (SOA), hệ thống được phân định rõ ràng các giới hạn phát triển theo mô hình **MoSCoW**:

```mermaid
quadrantChart
    title Ma trận Ưu tiên Phát triển Module (Khung 7 Tuần)
    x-axis Độ phức tạp Thấp --> Độ phức tạp Cao
    y-axis Giá trị Cốt lõi Thấp --> Giá trị Cốt lõi Cao
    quadrant-1 Bắt buộc làm & Tập trung kiến trúc (Must-Have Core)
    quadrant-2 Làm nhanh & Đơn giản hóa (Quick Wins)
    quadrant-3 Cắt giảm / Bỏ qua (Out of Scope)
    quadrant-4 Giả lập / Mocking (Simulated)
    
    "Trip Management Service": [0.65, 0.95]
    "Matching & Dispatching": [0.75, 0.90]
    "API Gateway & Event Bus": [0.70, 0.85]
    "User & Auth Service": [0.35, 0.80]
    "Pricing & Billing": [0.40, 0.75]
    "Location Tracking": [0.55, 0.70]
    "Payment Service (Sandbox)": [0.70, 0.40]
    "Notification (WebSocket/FCM)": [0.45, 0.50]
    "Rating Service": [0.20, 0.35]
    "Admin Dashboard": [0.40, 0.30]
    "Hệ thống Bản đồ riêng": [0.95, 0.15]
    "AI Định giá thời tiết": [0.90, 0.20]
    "Tổng đài gọi điện VoIP": [0.85, 0.10]
```

### 5.1. Các Module BẮT BUỘC Phát triển (In-Scope: Must-Have)
*Đây là các module tạo nên "xương sống" và quy trình cốt lõi mà đề bài yêu cầu:*
1. **API Gateway & Event Bus (Hermes Message Bus - Hạ tầng SOA):**
   - Routing API tập trung, kiểm tra JWT Token.
   - Cấu hình Message Broker (Kafka/RabbitMQ) để các microservice giao tiếp phi đồng bộ và tách rời phụ thuộc.
2. **Module Quản lý Định danh & Xác thực (User & Auth Service):**
   - Đăng ký, đăng nhập JWT cho 3 roles: Khách hàng, Tài xế, Quản trị viên (RBAC).
3. **Module Quản lý Vị trí & Trạng thái Tài xế (Location & Driver Service):**
   - Bật/tắt trạng thái Online/Offline, lưu tọa độ GPS của tài xế vào Redis Cache, API tìm tài xế gần điểm đón.
   - *Hỗ trợ demo:* Có công cụ/script **giả lập di chuyển GPS** của tài xế trên bản đồ.
4. **Module Điều phối & Ghép xe (Matching & Dispatching Service):**
   - Thuật toán tìm tài xế gần nhất, gửi lời mời nhận chuyến, quản lý đếm ngược (15s Timeout) và **tự động chuyển tiếp sang tài xế tiếp theo** khi bị từ chối.
5. **Module Quản lý Vòng đời Chuyến đi (Trip Management Service):**
   - Quản lý máy trạng thái chuyến (`CREATED` ➔ `ACCEPTED` ➔ `ARRIVED` ➔ `IN_TRIP` ➔ `COMPLETED` ➔ `PAID`), đồng bộ hành trình Khách - Tài xế.
6. **Module Định giá & Tính cước (Pricing & Billing Service):**
   - Ước tính cước ban đầu và tính cước chính thức sau chuyến đi theo công thức cố định: `Giá mở cửa + (Số km × Đơn giá) + Phụ phí giờ cao điểm`.

### 5.2. Các Module Đơn giản hóa (In-Scope: Should-Have / Simplified)
*Tối ưu hóa thời gian thực hiện nhưng vẫn đáp ứng đầy đủ kịch bản demo kiến trúc:*
1. **Module Thanh toán (Payment Integration Service):**
   - Hỗ trợ thanh toán **Tiền mặt (Cash)** có xác nhận của tài xế.
   - Hỗ trợ thanh toán **Điện tử**: Tích hợp Cổng **Sandbox / Mock Payment Gateway** (VNPay Sandbox hoặc Mock Service giả lập thành công/thất bại) để demo kịch bản **Saga bù trừ giao dịch** khi cổng thanh toán lỗi.
2. **Module Thông báo (Notification Service):**
   - Đẩy thông báo thời gian thực qua **WebSocket (In-app)** hoặc **Firebase Cloud Messaging (FCM)**.
3. **Module Đánh giá & Phản hồi (Rating Service):**
   - Form chấm 1–5 sao và nhận xét cơ bản sau chuyến đi, tính điểm trung bình cho tài xế.
4. **Module Quản trị Vận hành (Admin & Ops Portal):**
   - Giao diện Web đơn giản để xem danh sách chuyến đang hoạt động, tài xế online và doanh thu cơ bản.

### 5.3. Các Tính năng LOẠI BỎ khỏi phạm vi (Out-of-Scope: Won't-Have)
*Không triển khai trong khung 7 tuần để tránh quá tải và không đúng trọng tâm kiến trúc dịch vụ:*

| Tính năng ngoài phạm vi | Lý do loại bỏ / Giải pháp thay thế cho đồ án |
| :--- | :--- |
| ❌ **Tự xây dựng bản đồ số riêng (Map Engine)** | Tốn kém tài nguyên. **Giải pháp:** Sử dụng API có sẵn (Google Maps, OpenStreetMap/Leaflet, Mapbox). |
| ❌ **Thuật toán AI dự đoán giá động thời tiết phức tạp** | Không thuộc trọng tâm môn học. **Giải pháp:** Áp dụng bảng phụ phí Surge Pricing cố định theo khung giờ. |
| ❌ **Tổng đài thoại VoIP / Gọi trực tiếp qua SIM** | Khó khăn hạ tầng viễn thông. **Giải pháp:** Sử dụng Chat trong ứng dụng hoặc hiển thị SĐT. |
| ❌ **Xử lý mất kết nối mạng kéo dài nhiều ngày (Offline Mesh)** | Đặt xe cần xử lý thời gian thực. **Giải pháp:** Timeout 30s tự hủy tìm kiếm hoặc hỗ trợ gửi lại lệnh (Retry). |
| ❌ **Hệ thống Ví điện tử / Nạp - Rút ngân hàng đa tầng** | Rủi ro bảo mật tài chính. **Giải pháp:** Thanh toán chuyến nào quyết toán chuyến đó qua Cổng trung gian. |

---

## 6. SƠ ĐỒ THIẾT KẾ MERMAID (MERMAID DIAGRAMS)

### 6.1. Sơ đồ Use Case Tổng thể (Use Case Diagram)

```mermaid
graph TD
    %% Actors
    Customer((Khách hàng))
    Driver((Tài xế))
    Operator((Nhân viên vận hành))
    Admin((Quản trị viên))
    PaymentGW[Cổng thanh toán]
    NotiService[Dịch vụ thông báo]

    %% Use cases Customer
    subgraph Customer_UC [Phân hệ Khách hàng]
        UC_RegCust[Đăng ký / Đăng nhập]
        UC_BookTrip[Đặt chuyến & Xem giá dự kiến]
        UC_TrackTrip[Theo dõi hành trình & Tài xế]
        UC_CancelTrip[Hủy chuyến]
        UC_Pay[Thanh toán Tiền mặt / Online]
        UC_Rate[Đánh giá tài xế]
    end

    %% Use cases Driver
    subgraph Driver_UC [Phân hệ Tài xế]
        UC_UpdateStatus[Bật / Tắt trạng thái Online]
        UC_UpdateLoc[Cập nhật vị trí GPS]
        UC_AcceptTrip[Nhận / Từ chối chuyến]
        UC_UpdateTripStatus[Cập nhật trạng thái chuyến]
        UC_ConfirmCash[Xác nhận thu tiền mặt]
    end

    %% Use cases Operations & Admin
    subgraph Admin_UC [Phân hệ Vận hành & Quản trị]
        UC_LiveMonitor[Giám sát chuyến đi trực tiếp]
        UC_ResolveIncident[Xử lý sự cố chuyến đi]
        UC_ManageUsers[Quản lý Người dùng & Phương tiện]
        UC_ViewReport[Xem Báo cáo Doanh thu & Vận hành]
    end

    %% Customer Connections
    Customer --> UC_RegCust
    Customer --> UC_BookTrip
    Customer --> UC_TrackTrip
    Customer --> UC_CancelTrip
    Customer --> UC_Pay
    Customer --> UC_Rate

    %% Driver Connections
    Driver --> UC_UpdateStatus
    Driver --> UC_UpdateLoc
    Driver --> UC_AcceptTrip
    Driver --> UC_UpdateTripStatus
    Driver --> UC_ConfirmCash

    %% Admin & Operator Connections
    Operator --> UC_LiveMonitor
    Operator --> UC_ResolveIncident
    Admin --> UC_ManageUsers
    Admin --> UC_ViewReport
    Admin --> UC_ResolveIncident

    %% External Connections
    UC_Pay -.-> PaymentGW
    UC_BookTrip -.-> NotiService
    UC_UpdateTripStatus -.-> NotiService
```

---

### 4.2. Sơ đồ Tuần tự Luồng Đặt xe & Điều phối Tài xế (Sequence Diagram)

```mermaid
sequenceDiagram
    autonumber
    actor C as Khách hàng
    participant App as CAB Mobile / Web App
    participant Match as Matching & Dispatch Service
    participant Loc as Location Service
    participant Trip as Trip Service
    actor D1 as Tài xế 1 (Gần nhất)
    actor D2 as Tài xế 2 (Dự phòng)
    participant Noti as Notification Service

    C->>App: Chọn điểm đón, điểm đến, loại xe
    App->>Trip: Yêu cầu ước tính giá & tạo chuyến
    Trip-->>App: Trả về giá cước dự kiến & ID chuyến
    C->>App: Xác nhận đặt xe
    App->>Match: Gửi yêu cầu tìm tài xế
    Match->>Loc: Lấy danh sách tài xế Online gần điểm đón
    Loc-->>Match: Trả về danh sách [D1, D2, ...]

    Match->>Noti: Gửi lời mời nhận chuyến tới D1
    Noti->>D1: Thông báo chuyến đi mới (Hạn chờ 15s)
    
    alt D1 từ chối hoặc hết thời gian chờ
        D1-->>Match: Từ chối / Timeout
        Match->>Noti: Gửi lời mời nhận chuyến tới D2
        Noti->>D2: Thông báo chuyến đi mới
        D2->>Match: Chấp nhận chuyến (Accept)
        Match->>Trip: Gán D2 cho chuyến đi (Trạng thái: ACCEPTED)
        Trip->>Noti: Báo cho Khách hàng thông tin D2
        Noti->>C: Tài xế D2 đã nhận chuyến!
    else D1 chấp nhận chuyến
        D1->>Match: Chấp nhận chuyến (Accept)
        Match->>Trip: Gán D1 cho chuyến đi
        Trip->>Noti: Báo cho Khách hàng thông tin D1
        Noti->>C: Tài xế D1 đã nhận chuyến!
    end
```

---

### 4.3. Sơ đồ Trạng thái Vòng đời Chuyến đi (Trip State Machine Diagram)

```mermaid
stateDiagram-v2
    [*] --> CREATED: Khách hàng tạo yêu cầu đặt xe
    CREATED --> SEARCHING_DRIVER: Hệ thống tìm tài xế phù hợp
    
    SEARCHING_DRIVER --> NO_DRIVER_FOUND: Không có tài xế nhận / Hết tài xế
    NO_DRIVER_FOUND --> [*]

    SEARCHING_DRIVER --> CANCELLED: Khách hàng hủy chuyến
    CANCELLED --> [*]

    SEARCHING_DRIVER --> ACCEPTED: Tài xế chấp nhận chuyến
    ACCEPTED --> ARRIVED_AT_PICKUP: Tài xế đến điểm đón
    ACCEPTED --> CANCELLED: Khách hoặc Tài xế hủy chuyến

    ARRIVED_AT_PICKUP --> IN_TRIP: Đã đón khách & bắt đầu di chuyển
    ARRIVED_AT_PICKUP --> CANCELLED: Hủy chuyến tại điểm đón

    IN_TRIP --> COMPLETED: Đến điểm đến & hoàn thành chuyến
    
    COMPLETED --> PAYMENT_PENDING: Tính toán cước phí
    PAYMENT_PENDING --> PAID: Thanh toán thành công (Tiền mặt / Online)
    PAYMENT_PENDING --> PAYMENT_FAILED: Thanh toán Online lỗi (Chuyển Tiền mặt / Retry)
    PAYMENT_FAILED --> PAID: Thanh toán lại thành công
    
    PAID --> RATED: Khách hàng đánh giá & nhận xét
    PAID --> [*]
    RATED --> [*]
```

---

### 4.4. Sơ đồ Kiến trúc Tổng quan Hệ thống Hướng Dịch Vụ (Service-Oriented Architecture - SOA)

```mermaid
graph TB
    subgraph Clients [Tầng Ứng dụng Khách]
        CustApp[Customer App / Web]
        DriverApp[Driver App]
        AdminPortal[Admin & Ops Web Dashboard]
    end

    subgraph APIGateway [API Gateway Layer]
        Gateway[API Gateway / Reverse Proxy & Auth Check]
    end

    subgraph CoreServices [Tầng Dịch vụ Lõi - Microservices]
        AuthSvc[User & Auth Service]
        LocationSvc[Location & Tracking Service]
        MatchingSvc[Matching & Dispatch Service]
        TripSvc[Trip Management Service]
        PricingSvc[Pricing & Billing Service]
        PaymentSvc[Payment Integration Service]
        NotiSvc[Notification Service]
        AdminSvc[Admin & Reporting Service]
    end

    subgraph MessageBroker [Tầng Điều phối Sự kiện - Event Broker]
        Kafka[Message Broker: Kafka / RabbitMQ]
    end

    subgraph Databases [Tầng Dữ liệu]
        UserDB[(User DB)]
        TripDB[(Trip DB)]
        RedisCache[(Redis Cache - Tọa độ & Session)]
        PaymentDB[(Payment DB)]
    end

    subgraph ExternalServices [Dịch vụ bên thứ ba]
        MapsAPI[Google Maps / Mapbox API]
        PayGateway[VNPay / MoMo / Stripe]
        PushSMS[FCM / SMS Gateway]
    end

    %% Client to Gateway
    CustApp --> Gateway
    DriverApp --> Gateway
    AdminPortal --> Gateway

    %% Gateway to Services
    Gateway --> AuthSvc
    Gateway --> LocationSvc
    Gateway --> MatchingSvc
    Gateway --> TripSvc
    Gateway --> PricingSvc
    Gateway --> PaymentSvc
    Gateway --> NotiSvc
    Gateway --> AdminSvc

    %% Service to Event Broker
    TripSvc <--> Kafka
    MatchingSvc <--> Kafka
    PaymentSvc <--> Kafka
    NotiSvc <--> Kafka
    LocationSvc <--> Kafka

    %% Databases
    AuthSvc --> UserDB
    TripSvc --> TripDB
    LocationSvc --> RedisCache
    PaymentSvc --> PaymentDB

    %% External integrations
    LocationSvc --> MapsAPI
    PaymentSvc --> PayGateway
    NotiSvc --> PushSMS
```

## 7. MÔ HÌNH KIẾN TRÚC & QUY TRÌNH HERMES CHO ĐỒ ÁN (HERMES MODEL)

### 7.1. Mô hình Kiến trúc Hướng sự kiện Hermes (Hermes Event-Driven SOA Architecture)
Trong kiến trúc hướng dịch vụ hiện đại của CAB System, **Hermes Event-Driven Architecture** đóng vai trò là xương sống trung gian (Event Middleware / Service Bus) đảm bảo các dịch vụ hoạt động phi đồng bộ (asynchronous), chịu tải cao và tách biệt phụ thuộc (loose coupling):

```mermaid
graph TB
    subgraph Producers [Event Producers]
        P_Trip[Trip Service]
        P_Match[Matching Service]
        P_Pay[Payment Service]
        P_Loc[Location Service]
    end

    subgraph HermesBus [Hermes Event-Driven Message Bus / Middleware]
        E_TripCreated["Topic: trip.created"]
        E_DriverFound["Topic: driver.matched"]
        E_TripStatus["Topic: trip.status.updated"]
        E_Payment["Topic: payment.completed"]
        E_NotiQueue["Topic: notification.dispatch"]
    end

    subgraph Consumers [Event Consumers / Handlers]
        C_Match[Matching Service Handler]
        C_Trip[Trip Service Handler]
        C_Billing[Billing & Pricing Handler]
        C_Noti[Notification Push Worker]
        C_Audit[Audit & Analytics Worker]
        C_Admin[Realtime Dashboard Stream]
    end

    %% Flow from producers to Hermes Bus
    P_Trip -->|Publish TripCreated| E_TripCreated
    P_Match -->|Publish DriverMatched| E_DriverFound
    P_Trip -->|Publish StatusUpdate| E_TripStatus
    P_Pay -->|Publish PaymentSuccess| E_Payment
    P_Loc -->|Publish DriverMoved| E_TripStatus

    %% Hermes Bus to Consumers
    E_TripCreated -->|Subscribe| C_Match
    E_DriverFound -->|Subscribe| C_Trip
    E_DriverFound -->|Subscribe| C_NotiQueue
    E_TripStatus -->|Subscribe| C_NotiQueue
    E_TripStatus -->|Subscribe| C_Admin
    E_Payment -->|Subscribe| C_Trip
    E_Payment -->|Subscribe| C_Audit
    E_Payment -->|Subscribe| C_NotiQueue
    E_NotiQueue -->|Process & Send| C_Noti
```

---

### 7.2. Sơ đồ Điều phối Giao dịch Phân tán Hermes Saga (Hermes Saga Orchestration)
Xử lý giao dịch phân tán giữa Trip Service, Matching Service, Payment Service và Notification Service để tránh lỗi dữ liệu khi có dịch vụ bên thứ ba bị gián đoạn:

```mermaid
sequenceDiagram
    autonumber
    actor Customer
    participant HermesSaga as Hermes Saga Orchestrator
    participant TripSvc as Trip Service
    participant MatchSvc as Matching Service
    participant PaySvc as Payment Service
    participant NotiSvc as Notification Service

    Customer->>HermesSaga: Yêu cầu hoàn thành chuyến & thanh toán
    Note over HermesSaga: Bước 1: Tính toán cước phí & cập nhật Trip
    HermesSaga->>TripSvc: Chuyển trạng thái TRIP_COMPLETED
    TripSvc-->>HermesSaga: Xác nhận thành công

    Note over HermesSaga: Bước 2: Khởi tạo giao dịch thanh toán
    HermesSaga->>PaySvc: Gửi lệnh trừ tiền / thu tiền
    alt Thanh toán thành công
        PaySvc-->>HermesSaga: Payment Succeeded
        HermesSaga->>TripSvc: Cập nhật trạng thái PAID
        HermesSaga->>NotiSvc: Gửi hóa đơn điện tử cho khách hàng
        NotiSvc-->>Customer: Thông báo thanh toán thành công
    else Thanh toán thất bại
        PaySvc-->>HermesSaga: Payment Failed
        Note over HermesSaga: Thực hiện bù trừ (Compensating Transaction)
        HermesSaga->>TripSvc: Chuyển sang PAYMENT_PENDING_CASH (Trả tiền mặt)
        HermesSaga->>NotiSvc: Báo khách & tài xế thanh toán lỗi, chuyển thu tiền mặt
        NotiSvc-->>Customer: Báo lỗi thanh toán, vui lòng trả tiền mặt cho tài xế
    end
```

---

### 7.3. Mô hình Quản lý Vòng đời Đồ án theo Phương pháp luận HERMES (7 Tuần)
Áp dụng tiêu chuẩn quản lý dự án **HERMES Project Lifecycle** (4 giai đoạn - Milestones) để phát triển và triển khai hệ thống trong khung thời gian 7 tuần:

```mermaid
gantt
    title KẾ HOẠCH TRIỂN KHAI ĐỒ ÁN CAB SYSTEM THEO PHƯƠNG PHÁP HERMES (7 TUẦN)
    dateFormat  YYYY-MM-DD
    section Giai đoạn 1: Khởi tạo (Initiation)
    Thu thập & Phân tích yêu cầu khách hàng      :done, init1, 2026-09-01, 4d
    Xác định phạm vi & Lập kế hoạch dự án        :done, init2, after init1, 3d
    Cột mốc B1 - Phê duyệt Khởi tạo (Initiation Decision) :milestone, m1, 2026-09-07, 0d

    section Giai đoạn 2: Khái niệm & Thiết kế (Concept)
    Thiết kế Kiến trúc Dịch vụ (SOA/Microservices):active, con1, 2026-09-08, 5d
    Đặc tả API Gateway, Message Bus & Database   :con2, after con1, 5d
    Thiết kế UI/UX App Khách & App Tài xế        :con3, 2026-09-10, 5d
    Cột mốc B2 - Phê duyệt Thiết kế Kiến trúc     :milestone, m2, 2026-09-18, 0d

    section Giai đoạn 3: Thực thi (Implementation)
    Phát triển Auth, User & Driver Service       :imp1, 2026-09-19, 6d
    Phát triển Location & Matching Service       :imp2, 2026-09-22, 7d
    Phát triển Trip & Pricing Service            :imp3, 2026-09-25, 7d
    Tích hợp Payment Gateway & Notification Bus  :imp4, 2026-09-29, 6d
    Xây dựng Admin Portal & Live Tracking        :imp5, 2026-10-02, 6d
    Kiểm thử Tích hợp & Chịu tải (Load Testing) :imp6, 2026-10-06, 5d
    Cột mốc B3 - Sẵn sàng Triển khai             :milestone, m3, 2026-10-12, 0d

    section Giai đoạn 4: Triển khai & Nghiệm thu (Deployment)
    Triển khai hệ thống lên môi trường Staging/Cloud:dep1, 2026-10-13, 3d
    Kiểm thử UAT & Đóng gói sản phẩm             :dep2, after dep1, 3d
    Báo cáo tổng kết đồ án & Bàn giao            :dep3, after dep2, 2d
    Cột mốc B4 - Nghiệm thu hoàn tất Đồ án       :milestone, m4, 2026-10-20, 0d
```

---

## 8. YÊU CẦU PHI CHỨC NĂNG (NON-FUNCTIONAL REQUIREMENTS)

1. **Khả năng mở rộng & Tính sẵn sàng (Scalability & Availability):**
   - Kiến trúc hướng dịch vụ (SOA/Microservices) cho phép các dịch vụ (Payment, Notification, Matching) mở rộng độc lập khi lưu lượng tăng đột biến vào giờ cao điểm.
   - Tránh điểm lỗi đơn (Single Point of Failure): Lỗi ở phân hệ thanh toán hay thông báo không làm gián đoạn luồng đặt xe chính.
2. **Bảo mật & An toàn thông tin (Security & Privacy):**
   - Xác thực người dùng qua JWT/OAuth2. Phân quyền chặt chẽ theo vai trò (RBAC).
   - Mã hóa dữ liệu nhạy cảm trên đường truyền (HTTPS/TLS) và lưu trữ.
   - Tuân thủ tiêu chuẩn bảo mật thanh toán: Tuyệt đối không lưu trữ thông tin thẻ thanh toán nhạy cảm (CVV, Số thẻ đầy đủ) trên hệ thống nội bộ.
3. **Hiệu năng & Độ trễ (Performance & Latency):**
   - Định vị và phản hồi điều phối tài xế xử lý trong thời gian thực (độ trễ dưới 2 giây).
4. **Khả năng mở rộng trong tương lai (Extensibility):**
   - Dễ dàng tích hợp thêm loại hình dịch vụ mới (giao hàng, xe điện), thêm cổng thanh toán hoặc nhà cung cấp thông báo mới.

---

## 9. CÁC VẤN ĐỀ NGHIỆP VỤ CẦN LÀM RÕ VỚI KHÁCH HÀNG (OPEN QUESTIONS)

1. **Công thức tính cước chi tiết:** Giá mở cửa, cước phí mỗi km tiếp theo, phụ phí thời gian chờ, hệ số nhân theo thời tiết và giờ cao điểm.
2. **Thuật toán điều phối:** Tiêu chí ưu tiên tài xế ngoài khoảng cách (Điểm đánh giá sao, tỷ lệ nhận chuyến, thời gian tài xế chờ cuốc).
3. **Thời gian chờ tài xế phản hồi:** Thời gian Timeout tối đa để tài xế bấm nhận chuyến trước khi chuyển cho tài xế khác (ví dụ: 15 giây).
4. **Chính sách hủy chuyến & Phí phạt:** Điều kiện hủy miễn phí và mức phí phạt nếu hủy sau khi tài xế đã di chuyển tới điểm đón.
5. **Cơ chế xử lý mất kết nối (Offline Handling):** Phương án xử lý lưu tạm và đồng bộ lại tọa độ khi tài xế/khách hàng bị rớt mạng giữa đường.
6. **Thời gian lưu trữ dữ liệu (Data Retention):** Quy định thời gian lưu trữ lịch sử GPS và nhật ký kiểm toán trước khi lưu trữ định kỳ (Archiving).



