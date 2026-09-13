# HỆ THỐNG ĐẶC TẢ API - CAB SYSTEM (OPENAPI 3.0)

Tài liệu này đóng vai trò là danh mục điều hướng và chỉ mục tổng hợp cho toàn bộ hệ thống API của **CAB System**, được thiết kế theo kiến trúc Hướng Dịch Vụ (SOA) và phân tách độc lập theo từng nhóm tác nhân người dùng (**User-Oriented API Separation**) theo chuẩn **OpenAPI 3.0.3 (Swagger)**.

---

## 📂 Danh mục 4 File Đặc tả API theo từng Nhóm User

Mỗi nhóm người dùng được lưu trữ trong thư mục riêng biệt kèm file đặc tả YAML độc lập:

| Nhóm Người dùng (User Role) | Đường dẫn Thư mục & File YAML | Quy cách & Phạm vi Nghiệp vụ cốt lõi | Các Use Case liên quan |
| :--- | :--- | :--- | :---: |
| 🧑‍💼 **Khách hàng (Customer)** | [`customer/customer.yaml`](file:///d:/HK1_26_27/HuongDichVu/customer/customer.yaml) | Đăng ký/Đăng nhập, Ước tính cước & ETA, Khởi tạo đặt xe, Live Tracking GPS realtime, Hủy chuyến & Phạt hủy, Thanh toán đa kênh (Tiền mặt/VNPay/MoMo), Đánh giá 1-5 sao, Thông báo. | `UC-01` ➔ `UC-05` |
| 🚗 **Tài xế (Driver)** | [`driver/driver.yaml`](file:///d:/HK1_26_27/HuongDichVu/driver/driver.yaml) | Đăng ký hồ sơ & bằng lái (A1/B2/C), Quản lý xe, Bật/tắt Online/Offline, Phát sóng GPS (1-3s), Nhận/Từ chối cuốc (15s timeout), Cập nhật tiến trình (`ARRIVED` ➔ `IN_TRIP` ➔ `COMPLETED`), Xác nhận thu tiền mặt, Báo cáo thu nhập & Điểm uy tín. | `UC-06` ➔ `UC-09`, `UC-03` |
| 🎧 **Nhân viên Vận hành (Operator)** | [`operator/operator.yaml`](file:///d:/HK1_26_27/HuongDichVu/operator/operator.yaml) | Giám sát trực tiếp đội xe trên bản đồ số (Live Map), Cảnh báo sự cố bất thường (xe đứng yên > 5p, mất GPS, SOS), Can thiệp hủy chuyến cưỡng bức, Điều phối tài xế cứu hộ, Ghi nhật ký kiểm toán bất biến. | `UC-10`, `UC-11` |
| 👑 **Quản trị viên (Admin)** | [`admin/admin.yaml`](file:///d:/HK1_26_27/HuongDichVu/admin/admin.yaml) | Quản trị người dùng & phân quyền RBAC, Kiểm duyệt hồ sơ tài xế & phương tiện, Giám sát và đình chỉ tài xế rating thấp (< 3.5), Cấu hình bảng cước phí động, Báo cáo tài chính & đối soát giao dịch, Truy vấn Audit Logs. | `UC-12`, `UC-13` |

---

## 🛠️ Hướng dẫn Khởi chạy & Xem Tài liệu trên Swagger UI

Bạn có thể dễ dàng xem và kiểm thử trực quan các file API này thông qua các công cụ chuẩn công nghiệp:

### 1. Sử dụng Swagger Editor trực tuyến
1. Mở trình duyệt truy cập: [https://editor.swagger.io](https://editor.swagger.io)
2. Chọn menu **File ➔ Import File** và chọn file YAML tương ứng (`customer.yaml`, `driver.yaml`, `operator.yaml` hoặc `admin.yaml`).

### 2. Sử dụng VS Code / IDE Plugin
- Cài đặt extension: **OpenAPI (Swagger) Editor** hoặc **Swagger Viewer**.
- Mở bất kỳ file `.yaml` nào trong 4 thư mục trên và bấm tổ hợp phím `Shift + Alt + P` để xem giao diện Swagger UI trực tiếp.

### 3. Nhập vào Postman
- Mở ứng dụng Postman ➔ Bấm nút **Import** ➔ Chọn file `.yaml` bất kỳ để tự động sinh toàn bộ Collection và Request mẫu có sẵn Authentication Bearer Token.

---

## 🔗 Liên kết Tham chiếu Nghiệp vụ
- Tài liệu Đặc tả Yêu cầu Phần mềm: [`SRS.md`](file:///d:/HK1_26_27/HuongDichVu/SRS.md)
- Bảng Ma trận Truy vết Toàn diện (BG ➔ BR ➔ BPMN ➔ SR ➔ UC ➔ AC): [SRS.md - Mục 11.6](file:///d:/HK1_26_27/HuongDichVu/SRS.md#116-bảng-truy-vết-hợp-nhất-toàn-diện-end-to-end-master-traceability-matrix-bg--br--bpmn--sr--uc--ac)
- Sơ đồ Kiến trúc & Tuần tự: [`diagrams_preview.html`](file:///d:/HK1_26_27/HuongDichVu/diagrams_preview.html)
