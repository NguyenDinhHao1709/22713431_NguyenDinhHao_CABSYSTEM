# HỆ THỐNG ĐẶC TẢ API - CAB SYSTEM (OPENAPI 3.0.3)

Tài liệu này đóng vai trò là chỉ mục và danh mục điều hướng toàn diện cho toàn bộ hệ thống API của **CAB System**, được thiết kế theo kiến trúc **Hướng Dịch Vụ (SOA) / Microservices** theo chuẩn quốc tế **OpenAPI 3.0.3 (Swagger)**.

Tất cả các API được phân bổ chính xác theo **10 Dịch vụ / Miền nghiệp vụ sẽ xây dựng trong hệ thống**, bám sát 100% phạm vi dự án trong tài liệu [`SRS.md`](file:///d:/HK1_26_27/HuongDichVu/SRS.md) (Mục 2.2 Phân rã Miền nghiệp vụ, Mục 2.3 Khoanh vùng Phạm vi MoSCoW, và Chương 5 Đặc tả Chức năng SR_01 – SR_25):

- **Tác giả / Sinh viên thực hiện:** Nguyễn Đình Hảo (MSSV: 22713431)
- **Mã môn học / Đề tài:** Kiến trúc Hướng Dịch Vụ - Hệ Thống Đặt Xe Trực Tuyến CAB System
- **Chuẩn đặc tả:** OpenAPI Specification 3.0.3 (YAML format)
- **Cấu trúc lưu trữ:** Mỗi dịch vụ được tổ chức trong một thư mục tiếng Việt không dấu độc lập, chứa tệp đặc tả `.yaml` tương ứng.

---

## 🚀 DANH MỤC 10 DỊCH VỤ NGHIỆP VỤ HỆ THỐNG (OPENAPI SPECIFICATIONS)

| STT | Thư mục & Tệp YAML | Microservice Đảm nhận | Phân loại Miền (SRS 2.2) | Các SR & BRULE Liên quan | Phạm vi Nghiệp vụ Cốt lõi (In-Scope) |
| :---: | :--- | :--- | :---: | :---: | :--- |
| **01** | [`01_dinh_danh_xac_thuc/01_dinh_danh_xac_thuc.yaml`](file:///d:/HK1_26_27/HuongDichVu/01_dinh_danh_xac_thuc/01_dinh_danh_xac_thuc.yaml) | **User & Auth Service** | Generic Domain | `SR_01`, `SR_02`, `BR_05`, `NFR_07` | Đăng ký tài khoản (Khách/Tài xế), nộp giấy phép lái xe, xác thực đăng nhập cấp cặp JWT Token Stateless, mã hóa BCrypt, phân quyền RBAC. |
| **02** | [`02_dinh_vi_telemetry/02_dinh_vi_telemetry.yaml`](file:///d:/HK1_26_27/HuongDichVu/02_dinh_vi_telemetry/02_dinh_vi_telemetry.yaml) | **Location & Tracking Service** | Supporting Domain | `SR_04`, `SR_05`, `SR_06`, `SR_13`, `BRULE_01` | Thu thập phát sóng GPS định kỳ 1–3s, chỉ mục Geo-Redis, tìm tài xế bán kính 2km–10km, Live Tracking lộ trình xe thời gian thực. |
| **03** | [`03_dieu_phoi_ghep_xe/03_dieu_phoi_ghep_xe.yaml`](file:///d:/HK1_26_27/HuongDichVu/03_dieu_phoi_ghep_xe/03_dieu_phoi_ghep_xe.yaml) | **Matching & Dispatch Service** | **Core Domain** | `SR_09`, `SR_10`, `BRULE_01`, `BRULE_02` | Thuật toán tính PriorityScore, phát lời mời nhận chuyến kèm đồng hồ đếm ngược 15s, tự động chuyển tiếp tài xế kế tiếp khi từ chối/timeout. |
| **04** | [`04_quan_ly_chuyen_di/04_quan_ly_chuyen_di.yaml`](file:///d:/HK1_26_27/HuongDichVu/04_quan_ly_chuyen_di/04_quan_ly_chuyen_di.yaml) | **Trip Management Service** | **Core Domain** | `SR_08`, `SR_11`, `SR_12`, `SR_14`, `BRULE_05` | Máy trạng thái vòng đời chuyến đi (`CREATED` ➔ `ARRIVED` ➔ `IN_TRIP` ➔ `COMPLETED`), chính sách hủy chuyến và tính phí phạt sau 2 phút di chuyển. |
| **05** | [`05_dinh_gia_tinh_cuoc/05_dinh_gia_tinh_cuoc.yaml`](file:///d:/HK1_26_27/HuongDichVu/05_dinh_gia_tinh_cuoc/05_dinh_gia_tinh_cuoc.yaml) | **Pricing & Billing Service** | **Core Domain** | `SR_07`, `SR_15`, `BRULE_03`, `BRULE_04` | Ước tính giá cước ban đầu (Base Fare + km + Surge Multiplier) và quyết toán cước phí thực tế dựa trên quãng đường GPS hoàn thành. |
| **06** | [`06_thanh_toan_doi_soat/06_thanh_toan_doi_soat.yaml`](file:///d:/HK1_26_27/HuongDichVu/06_thanh_toan_doi_soat/06_thanh_toan_doi_soat.yaml) | **Payment Integration Service** | Generic Domain | `SR_16`, `SR_17`, `SR_18`, `BRULE_07`, `BRULE_08` | Thanh toán Tiền mặt và Cổng điện tử Sandbox (VNPay/MoMo Webhook HMAC-SHA256), điều phối bù trừ Hermes Saga (tự động chuyển Tiền mặt khi lỗi cổng). |
| **07** | [`07_thong_bao_da_kenh/07_thong_bao_da_kenh.yaml`](file:///d:/HK1_26_27/HuongDichVu/07_thong_bao_da_kenh/07_thong_bao_da_kenh.yaml) | **Notification Service** | Supporting Domain | `SR_21`, `SR_22`, `BR_07` | Đẩy thông báo Push FCM tới khách hàng trong $\le 2$s (xe đến, hoàn tất chuyến), phát chuông báo cuốc xe nổi toàn màn hình cho tài xế, SMS OTP dự phòng. |
| **08** | [`08_danh_gia_phan_hoi/08_danh_gia_phan_hoi.yaml`](file:///d:/HK1_26_27/HuongDichVu/08_danh_gia_phan_hoi/08_danh_gia_phan_hoi.yaml) | **Rating & Review Service** | Supporting Domain | `SR_19`, `SR_20`, `BR_06`, `BRULE_06`, `BRULE_09` | Tiếp nhận đánh giá 1–5 sao trong 24h kể từ khi kết thúc chuyến đi, tự động cập nhật điểm uy tín tín nhiệm tài xế, cảnh báo vi phạm khi điểm $< 4.0$. |
| **09** | [`09_giam_sat_van_hanh/09_giam_sat_van_hanh.yaml`](file:///d:/HK1_26_27/HuongDichVu/09_giam_sat_van_hanh/09_giam_sat_van_hanh.yaml) | **Admin & Operations Service** | Supporting Domain | `SR_03`, `SR_23`, `SR_24`, `SR_25`, `BRULE_10` | Bản đồ giám sát đội xe trực tiếp (Live Map), can thiệp sự cố khẩn cấp (bắt buộc lý do $\ge 10$ ký tự), duyệt hồ sơ xe/tài xế, lưu vết 100% Audit Log và báo cáo doanh thu. |
| **10** | [`10_api_gateway/10_api_gateway.yaml`](file:///d:/HK1_26_27/HuongDichVu/10_api_gateway/10_api_gateway.yaml) | **API Gateway & Event Bus** | Hạ tầng / Routing | Chương 8, `SR_02`, `BR_10` | Cổng phân tuyến Reverse Proxy trung tâm, Pre-routing Auth Filter kiểm tra JWT stateless, Rate Limiting, Health check hệ thống và Catalog sự kiện Hermes Event Bus. |

---

## 🛡️ NGUYÊN TẮC THIẾT KẾ & TUÂN THỦ PHẠM VI (SCOPE BOUNDARIES)

1. **Tuân thủ Phạm vi MoSCoW (SRS Mục 2.3):**
   - **In-Scope:** Hệ thống tập trung giải quyết trọn vẹn luồng đặt xe trực tuyến từ định danh, tìm xe, điều phối, chuyến đi, tính cước, thanh toán song song (Tiền mặt + Cổng điện tử Sandbox), đánh giá đến giám sát vận hành.
   - **Out-of-Scope:** Không tự xây dựng Map Engine (sử dụng Google Maps/Mapbox API); Không áp dụng AI dự báo thời tiết phức tạp; Không tích hợp thoại VoIP nội bộ; Không triển khai ví điện tử trung gian nhiều tầng (sử dụng thanh toán trực tiếp qua Cổng Sandbox).
2. **Tuân thủ Định dạng YAML:**
   - Trường tác giả `name: "Nguyễn Đình Hảo (MSSV: 22713431)"` được bao bọc hoàn toàn bằng dấu ngoặc kép để tương thích 100% với trình phân tích cú pháp YAML parser (tránh nhầm lẫn dấu `:` với key/value delimiter).
   - Đã kiểm thử xác thực 10/10 file tệp bằng thư viện `PyYAML` chuẩn quốc tế.

---

## 🛠️ HƯỚNG DẪN KHỞI CHẠY & XEM TÀI LIỆU TRÊN SWAGGER UI

Bạn có thể dễ dàng xem và kiểm thử trực quan tất cả các file API này thông qua các công cụ:

### 1. Sử dụng Swagger Editor trực tuyến
1. Mở trình duyệt truy cập: [https://editor.swagger.io](https://editor.swagger.io)
2. Chọn menu **File ➔ Import File** và chọn file YAML tương ứng từ một trong 10 thư mục dịch vụ (`01_dinh_danh_xac_thuc` đến `10_api_gateway`).

### 2. Sử dụng VS Code / IDE Plugin
- Cài đặt extension: **OpenAPI (Swagger) Editor** hoặc **Swagger Viewer**.
- Mở bất kỳ file `.yaml` nào và bấm tổ hợp phím `Shift + Alt + P` để xem giao diện Swagger UI trực quan.

### 3. Nhập vào Postman
- Mở ứng dụng Postman ➔ Bấm nút **Import** ➔ Chọn bất kỳ tệp `.yaml` nào để tự động sinh toàn bộ Collection và Request mẫu có sẵn cấu hình xác thực Bearer Token (JWT).

---

## 🔗 LIÊN KẾT THAM CHIẾU NGHIỆP VỤ LIÊN QUAN
- **Tài liệu Đặc tả Yêu cầu Phần mềm:** [`SRS.md`](file:///d:/HK1_26_27/HuongDichVu/SRS.md)
- **Bảng Ma trận Truy vết Hợp nhất Toàn diện (BG ➔ BR ➔ BPMN ➔ SR ➔ UC ➔ AC):** [SRS.md - Mục 11.6](file:///d:/HK1_26_27/HuongDichVu/SRS.md#116-bảng-truy-vết-hợp-nhất-toàn-diện-end-to-end-master-traceability-matrix-bg--br--bpmn--sr--uc--ac)
- **Sơ đồ Kiến trúc & Tuần tự Dịch vụ:** [`diagrams_preview.html`](file:///d:/HK1_26_27/HuongDichVu/diagrams_preview.html)
