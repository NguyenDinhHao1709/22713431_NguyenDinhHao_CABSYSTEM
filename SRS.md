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

## 4. SƠ ĐỒ THIẾT KẾ MERMAID (MERMAID DIAGRAMS)

### 4.1. Sơ đồ Use Case Tổng thể (Use Case Diagram)

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

## 5. MÔ HÌNH KIẾN TRÚC & QUY TRÌNH HERMES CHO ĐỒ ÁN (HERMES MODEL)

### 5.1. Mô hình Kiến trúc Hướng sự kiện Hermes (Hermes Event-Driven SOA Architecture)
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

### 5.2. Sơ đồ Điều phối Giao dịch Phân tán Hermes Saga (Hermes Saga Orchestration)
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

### 5.3. Mô hình Quản lý Vòng đời Đồ án theo Phương pháp luận HERMES (7 Tuần)
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

## 6. YÊU CẦU PHI CHỨC NĂNG (NON-FUNCTIONAL REQUIREMENTS)

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

## 7. CÁC VẤN ĐỀ NGHIỆP VỤ CẦN LÀM RÕ VỚI KHÁCH HÀNG (OPEN QUESTIONS)

1. **Công thức tính cước chi tiết:** Giá mở cửa, cước phí mỗi km tiếp theo, phụ phí thời gian chờ, hệ số nhân theo thời tiết và giờ cao điểm.
2. **Thuật toán điều phối:** Tiêu chí ưu tiên tài xế ngoài khoảng cách (Điểm đánh giá sao, tỷ lệ nhận chuyến, thời gian tài xế chờ cuốc).
3. **Thời gian chờ tài xế phản hồi:** Thời gian Timeout tối đa để tài xế bấm nhận chuyến trước khi chuyển cho tài xế khác (ví dụ: 15 giây).
4. **Chính sách hủy chuyến & Phí phạt:** Điều kiện hủy miễn phí và mức phí phạt nếu hủy sau khi tài xế đã di chuyển tới điểm đón.
5. **Cơ chế xử lý mất kết nối (Offline Handling):** Phương án xử lý lưu tạm và đồng bộ lại tọa độ khi tài xế/khách hàng bị rớt mạng giữa đường.
6. **Thời gian lưu trữ dữ liệu (Data Retention):** Quy định thời gian lưu trữ lịch sử GPS và nhật ký kiểm toán trước khi lưu trữ định kỳ (Archiving).


