# HỆ THỐNG ĐẶC TẢ API - CAB SYSTEM (OPENAPI 3.0.3)

Tài liệu này đóng vai trò là chỉ mục và danh mục điều hướng toàn diện cho toàn bộ hệ thống API của **CAB System**, được thiết kế theo kiến trúc **Hướng Dịch Vụ (SOA) / Microservices** theo chuẩn quốc tế **OpenAPI 3.0.3 (Swagger)**.

Tất cả các API được phân bổ chính xác theo **10 Dịch vụ / Miền nghiệp vụ sẽ xây dựng trong hệ thống**, tích hợp đầy đủ các nghiệp vụ **CRUD (Create - Read - Update - Delete)** theo tài liệu [`SRS.md`](file:///d:/HK1_26_27/HuongDichVu/SRS.md) (Mục 2.2 Phân rã Miền nghiệp vụ, Mục 2.3 Khoanh vùng Phạm vi MoSCoW, Chương 5 SR_01 – SR_25, và Chương 6 Ca sử dụng UC-01 – UC-16):

- **Tác giả / Sinh viên thực hiện:** Nguyễn Đình Hảo (MSSV: 22713431)
- **Mã môn học / Đề tài:** Kiến trúc Hướng Dịch Vụ - Hệ Thống Đặt Xe Trực Tuyến CAB System
- **Chuẩn đặc tả:** OpenAPI Specification 3.0.3 (YAML format)
- **Cấu trúc lưu trữ:** Mỗi dịch vụ được tổ chức trong một thư mục tiếng Việt không dấu độc lập, chứa tệp đặc tả `.yaml` tương ứng.

---

## 🚀 DANH MỤC 10 DỊCH VỤ NGHIỆP VỤ HỆ THỐNG (TÍCH HỢP ĐẦY ĐỦ CRUD)

| STT | Thư mục & Tệp YAML | Microservice Đảm nhận | Các Nghiệp vụ CRUD Cốt lõi | Các SR & UC Liên quan |
| :---: | :--- | :--- | :--- | :---: |
| **01** | [`01_dinh_danh_xac_thuc/01_dinh_danh_xac_thuc.yaml`](file:///d:/HK1_26_27/HuongDichVu/01_dinh_danh_xac_thuc/01_dinh_danh_xac_thuc.yaml) | **User & Auth Service** | • **Create:** Đăng ký Khách hàng / Tài xế (`POST /register/*`)<br>• **Read:** Xem Profile cá nhân (`GET /profile/me`), Danh sách khách hàng (`GET /admin/customers`), Chi tiết (`GET /admin/customers/{id}`)<br>• **Update:** Cập nhật thông tin (`PUT /profile/me`), Đổi mật khẩu (`POST /profile/change-password`), Khóa/Mở khóa (`PATCH /admin/customers/{id}/status`)<br>• **Delete:** Khách hủy tài khoản (`DELETE /profile/me`), Admin xóa mềm (`DELETE /admin/customers/{id}`) | `SR_01`, `SR_02`, `SR_03`, `UC-14`, `UC-15` |
| **02** | [`02_dinh_vi_telemetry/02_dinh_vi_telemetry.yaml`](file:///d:/HK1_26_27/HuongDichVu/02_dinh_vi_telemetry/02_dinh_vi_telemetry.yaml) | **Location & Tracking Service** | • **Create/Update:** Thu thập tọa độ GPS 1–3s (`POST /telemetry/ping`)<br>• **Read:** Tìm xe bán kính 2km–10km (`GET /search/nearby-drivers`), Live Stream WebSocket vị trí xe realtime | `SR_04`, `SR_05`, `SR_06`, `SR_13`, `UC-02` |
| **03** | [`03_dieu_phoi_ghep_xe/03_dieu_phoi_ghep_xe.yaml`](file:///d:/HK1_26_27/HuongDichVu/03_dieu_phoi_ghep_xe/03_dieu_phoi_ghep_xe.yaml) | **Matching & Dispatch Service** | • **Create:** Kích hoạt điều phối tìm xe theo PriorityScore (`POST /dispatch/match-request`)<br>• **Update:** Tài xế chấp nhận hoặc từ chối cuốc trong 15s (`POST /dispatch/offers/{trip_id}/respond`) | `SR_09`, `SR_10`, `UC-01`, `UC-07` |
| **04** | [`04_quan_ly_chuyen_di/04_quan_ly_chuyen_di.yaml`](file:///d:/HK1_26_27/HuongDichVu/04_quan_ly_chuyen_di/04_quan_ly_chuyen_di.yaml) | **Trip Management Service** | • **Create:** Khởi tạo cuốc xe (`POST /trips`)<br>• **Read:** Xem lịch sử chuyến đi có phân trang (`GET /trips`), Xem chi tiết cuốc (`GET /trips/{trip_id}`)<br>• **Update:** Cập nhật trạng thái ARRIVED ➔ IN_TRIP ➔ COMPLETED (`PATCH /trips/{trip_id}/status`)<br>• **Delete/Cancel:** Hủy chuyến và tính phạt sau 2 phút (`POST /trips/{trip_id}/cancel`) | `SR_08`, `SR_11`, `SR_12`, `SR_14`, `UC-01`, `UC-03`, `UC-08` |
| **05** | [`05_dinh_gia_tinh_cuoc/05_dinh_gia_tinh_cuoc.yaml`](file:///d:/HK1_26_27/HuongDichVu/05_dinh_gia_tinh_cuoc/05_dinh_gia_tinh_cuoc.yaml) | **Pricing & Billing Service** | • **Calculation:** Ước tính cước ban đầu (`POST /pricing/estimate`), Quyết toán cuối (`POST /pricing/finalize`)<br>• **Fare Rules CRUD (Admin):** Tạo biểu phí mới (`POST /pricing/fare-rules`), Xem danh sách/chi tiết (`GET /pricing/fare-rules`), Cập nhật biểu phí (`PUT`), Xóa biểu phí (`DELETE`) | `SR_07`, `SR_15`, `BRULE_03`, `BRULE_04`, `UC-16` |
| **06** | [`06_thanh_toan_doi_soat/06_thanh_toan_doi_soat.yaml`](file:///d:/HK1_26_27/HuongDichVu/06_thanh_toan_doi_soat/06_thanh_toan_doi_soat.yaml) | **Payment Integration Service** | • **Create/Process:** Tạo giao dịch cổng điện tử Sandbox (`POST /payments/digital/initiate`), Xác nhận tiền mặt (`POST /payments/cash/confirm`)<br>• **Compensate (Update):** Bù trừ Hermes Saga chuyển sang Tiền mặt khi lỗi cổng | `SR_16`, `SR_17`, `SR_18`, `UC-04`, `UC-09` |
| **07** | [`07_thong_bao_da_kenh/07_thong_bao_da_kenh.yaml`](file:///d:/HK1_26_27/HuongDichVu/07_thong_bao_da_kenh/07_thong_bao_da_kenh.yaml) | **Notification Service** | • **Create/Send:** Bắn Push FCM Khách (`POST /push/customer`), Chuông báo Tài xế (`POST /push/driver`), SMS OTP (`POST /sms/send`)<br>• **Read/Update:** Hộp thư thông báo cá nhân (`GET /inbox`), Đánh dấu đã đọc (`PATCH /inbox/{id}/read`) | `SR_21`, `SR_22`, `BR_07` |
| **08** | [`08_danh_gia_phan_hoi/08_danh_gia_phan_hoi.yaml`](file:///d:/HK1_26_27/HuongDichVu/08_danh_gia_phan_hoi/08_danh_gia_phan_hoi.yaml) | **Rating & Review Service** | • **Create:** Chấm 1–5 sao trong 24h (`POST /trips/{tripId}/review`)<br>• **Read:** Xem đánh giá cuốc xe, Điểm trung bình tài xế (`GET /drivers/{id}/rating-summary`), Danh sách nhận xét công khai<br>• **Update:** Khách sửa nhận xét trong 2h (`PUT /reviews/{id}`)<br>• **Delete:** Admin ẩn/xóa đánh giá vi phạm (`DELETE /reviews/{id}`) | `SR_19`, `SR_20`, `BRULE_06`, `BRULE_09`, `UC-05` |
| **09** | [`09_giam_sat_van_hanh/09_giam_sat_van_hanh.yaml`](file:///d:/HK1_26_27/HuongDichVu/09_giam_sat_van_hanh/09_giam_sat_van_hanh.yaml) | **Admin & Operations Service** | • **Read:** Bản đồ số trực tiếp đội xe (`GET /fleet/live-map`), Danh sách chờ duyệt (`GET /approvals/drivers`), Nhật ký Audit Log (`GET /audit-logs`), Báo cáo doanh thu (`GET /analytics/revenue`)<br>• **Update:** Duyệt hồ sơ tài xế (`PATCH /approvals/drivers/{id}`), Can thiệp sự cố khẩn cấp (lý do $\ge 10$ ký tự) | `SR_03`, `SR_23`, `SR_24`, `SR_25`, `BRULE_10`, `UC-10`, `UC-11`, `UC-12`, `UC-13` |
| **10** | [`10_api_gateway/10_api_gateway.yaml`](file:///d:/HK1_26_27/HuongDichVu/10_api_gateway/10_api_gateway.yaml) | **API Gateway & Event Bus** | • **Read:** Kiểm tra tình trạng sức khỏe hạ tầng (`GET /health`), Danh mục định tuyến Reverse Proxy (`GET /gateway/routes`), Đăng ký sự kiện Hermes Event Bus (`GET /events/registry`) | Chương 8, `SR_02`, `BR_10` |

---

## 🛡️ NGUYÊN TẮC THIẾT KẾ & TUÂN THỦ PHẠM VI (SCOPE BOUNDARIES)

1. **Tuân thủ Phạm vi MoSCoW (SRS Mục 2.3):**
   - **In-Scope:** Hệ thống tập trung giải quyết trọn vẹn luồng đặt xe trực tuyến từ định danh & CRUD tài khoản, tìm xe, điều phối, chuyến đi, tính cước & CRUD biểu phí, thanh toán song song (Tiền mặt + Cổng điện tử Sandbox), đánh giá & CRUD phản hồi đến giám sát vận hành.
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
