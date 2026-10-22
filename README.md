# phamlehuyhoang-frontend — Ứng dụng Di động Điểm danh Sinh viên (Flutter)

> **Đồ án cá nhân môn Lập trình thiết bị di động**  
> **Sinh viên thực hiện:** Phạm Lê Huy Hoàng  
> **Repository Frontend:** https://github.com/plhhoang2005/phamlehuyhoang-frontend  
> **Repository Backend:** https://github.com/plhhoang2005/phamlehuyhoang-backend  

---

## 1. Giới thiệu ứng dụng

Ứng dụng di động Flutter phục vụ quy trình điểm danh sinh viên với giao diện Material 3 trực quan, hỗ trợ 3 phân quyền người dùng:

- **Quản trị viên (Admin):** Quản lý tài khoản, lớp học, phân công giảng dạy và giám sát hệ thống.
- **Giảng viên (Teacher):** Mở phiên điểm danh lớp học, hiển thị mã QR tự động làm mới sau 60s, thiết lập tọa độ GPS phòng học và bán kính hợp lệ, chốt kết quả điểm danh.
- **Sinh viên (Student):** Quét mã QR điểm danh, gửi kèm tọa độ GPS kiểm tra khoảng cách Haversine, đăng nhập bằng khuôn mặt, theo dõi số buổi vắng và dự báo chuyên cần.

---

## 2. Các chức năng mở rộng & Điểm nhấn công nghệ

1. **Đăng nhập bằng nhận diện khuôn mặt:**
   - Chụp ảnh chân dung trực tiếp qua Camera thiết bị.
   - Trích xuất đặc trưng và đối chiếu cosine similarity với vector mẫu đã đăng ký trên máy chủ.
   - Kết hợp xác thực mã OTP 2 lớp gửi về email.

2. **Xác thực vị trí khi điểm danh (GPS):**
   - Lấy tọa độ GPS thiết bị với sai số cho phép $\le 50$m.
   - Máy chủ tính toán khoảng cách cầu theo công thức Haversine tới phòng học của giảng viên (bán kính mặc định 100m).

3. **Mã QR phiên học bảo mật:**
   - Giảng viên sinh mã QR phiên học có thời hạn 60 giây.
   - Kiểm tra chống điểm danh trùng lặp ở tầng cơ sở dữ liệu.

4. **Tính năng mới đề xuất: Theo dõi số buổi vắng & Dự báo chuyên cần:**
   - Tính toán trực tiếp số buổi vắng không phép $A$, số buổi được phép vắng tối đa $L$.
   - Tính tỷ lệ chuyên cần hiện tại: $(M - A) / M \times 100\%$.
   - Cảnh báo kịp thời khi sinh viên có nguy cơ bị cấm thi.

---

## 3. Cấu trúc thư mục

```text
phamlehuyhoang-frontend/
├── android/                        # Cấu hình Android gốc (Kotlin DSL, Permissions)
├── ios/                            # Cấu hình iOS gốc
├── lib/
│   ├── models/                     # Dữ liệu mô hình (User, Session, Attendance)
│   ├── screens/                    # Giao diện màn hình (Login, FaceAuth, Admin, Teacher, Student)
│   ├── services/                   # Xử lý API, Auth, Vị trí GPS, Điểm danh
│   ├── utils/                      # Công thức dự báo chuyên cần, format ngày giờ
│   ├── widgets/                    # Thành phần giao diện tái sử dụng
│   └── main.dart                   # Entry point của ứng dụng Flutter
├── test/                           # Bộ kiểm thử widget và logic
├── pubspec.yaml                    # Quản lý thư viện phụ thuộc (name: phamlehuyhoang)
├── .gitignore                      # Git ignore chuẩn Flutter
└── README.md                       # Tài liệu hướng dẫn
```

---

## 4. Hướng dẫn cài đặt & Chạy ứng dụng

### 4.1. Yêu cầu môi trường
- Flutter SDK 3.x trở lên.
- Android Studio / VS Code có cài đặt Flutter & Dart extension.
- Thiết bị di động thật (bật USB Debugging) hoặc Android Emulator.

### 4.2. Cài đặt thư viện
Từ thư mục gốc của repository:
```bash
flutter pub get
```

### 4.3. Cấu hình địa chỉ Backend API
Khi khởi chạy ứng dụng, bạn có thể truyền địa chỉ Backend API (Local hoặc Tunnel Cloudflare) bằng cờ `--dart-define`:
```bash
# Ví dụ kết nối qua Cloudflare Tunnel:
flutter run --dart-define=API_BASE_URL="https://your-tunnel-url.trycloudflare.com"

# Hoặc khi dùng Android Emulator trỏ về máy tính:
flutter run --dart-define=API_BASE_URL="http://10.0.2.2:5000"
```
*Lưu ý:* Ứng dụng cũng tích hợp sẵn nút cấu hình URL ngay trên màn hình Đăng nhập để bạn có thể đổi địa chỉ IP/Domain bất kỳ lúc nào mà không cần build lại.

### 4.4. Build file cài đặt APK
```bash
flutter build apk --release
```
File APK xuất ra tại: `build/app/outputs/flutter-apk/app-release.apk`.

---

## 5. Tài khoản kiểm thử mẫu

| Vai trò | Email đăng nhập | Mật khẩu mặc định |
| :--- | :--- | :--- |
| **Quản trị viên (Admin)** | `admin@hutech.edu.vn` | `Admin@123456` |
| **Giảng viên (Teacher)** | `teacher@hutech.edu.vn` | `Teacher@123456` |
| **Sinh viên (Student)** | `student@hutech.edu.vn` | `Student@123456` |
