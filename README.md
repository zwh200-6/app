# Thu ngân POS

Ứng dụng thu ngân Android bằng Java và Room cho quán ăn, quán cà phê và cửa hàng đồ uống. Dữ liệu đơn hàng và ảnh sản phẩm nằm trên thiết bị. Có thể sử dụng sau khi cài mà không cần tài khoản, máy chủ hoặc Internet.

## Mở và chạy dự án

1. Mở thư mục repository trong Android Studio. Cài Android SDK Platform 36 và SDK Build-Tools 35.0.0 nếu IDE yêu cầu.
2. Chọn JDK 17 cho Gradle, rồi đồng bộ dự án. Gradle Wrapper sử dụng Gradle 8.11.1.
3. Chọn thiết bị hoặc emulator chạy Android 8.0 (API 26) trở lên và chạy cấu hình `app`.

Có thể build và kiểm tra qua dòng lệnh nếu JDK và Android SDK đã được cấu hình.

```bash
./gradlew assembleDebug testDebugUnitTest lintDebug
./gradlew connectedDebugAndroidTest
```

Lệnh thứ hai cần thiết bị hoặc emulator đang kết nối. Trên Windows, dùng `gradlew.bat`. APK debug được tạo tại `app/build/outputs/apk/debug/app-debug.apk`. Có thể cài APK qua Android Studio hoặc bằng `adb install -r app/build/outputs/apk/debug/app-debug.apk`.

## Sử dụng

1. Vào **Thực đơn**, tạo danh mục rồi thêm món và giá bằng VNĐ. Có thể chọn ảnh từ thư viện, thay ảnh hoặc dùng ảnh mặc định.
2. Vào **Bán hàng**, nhấn thẻ món để thêm vào giỏ. Chỉnh số lượng, kiểm tra tổng và chọn **Thanh toán**.
3. Với **tiền mặt**, nhập số tiền khách đưa và kiểm tra tiền thừa. Với **chuyển khoản**, kiểm tra tiền đã vào tài khoản rồi xác nhận trong ứng dụng.
4. Dùng **Lịch sử** để lọc đơn theo mã, ngày tạo hoặc trạng thái. Dùng **Thống kê** để xem doanh thu theo ngày, tháng hoặc năm theo giờ Việt Nam. Doanh thu ghi nhận theo thời điểm thanh toán thành công.

Ảnh đại diện được sao chép vào bộ nhớ riêng để có thể xem offline. Giới hạn mỗi ảnh là 25 MB. Xóa dữ liệu ứng dụng bằng cài đặt Android sẽ xóa dữ liệu được lưu trên thiết bị.

## Dữ liệu và kiểm thử

Schema Room ban đầu nằm trong `app/schemas/`. Khi nâng cấp schema đã phát hành, bổ sung Room Migration và kiểm thử trên dữ liệu phiên bản trước. Các bài kiểm thử JVM dùng JUnit và Robolectric cho phép kiểm tra tính tiền, Room và một số luồng giao diện mà không cần thiết bị.

## VietQR và thực đơn mẫu

- Vào **Tổng quan → Cấu hình VietQR** và nhập mã BIN 6 chữ số của ngân hàng cùng số tài khoản nhận. Tại màn hình **Thanh toán**, chọn **Hiện mã VietQR** để khách quét mã chứa số tiền và nội dung của đơn. Kiểm tra tên người nhận trong ứng dụng ngân hàng và chỉ xác nhận trong POS sau khi đã nhận đủ tiền.
- Vào **Tổng quan → Tạo dữ liệu mẫu** để thêm 2 danh mục và 6 món có nhãn `[Mẫu]`. Có thể bấm lại mà không tạo bản sao của những món mẫu đang có. Thực đơn mẫu không tạo sẵn giao dịch hay doanh thu. Nếu cần thử ghi nhận thanh toán và thống kê, hãy dùng bản cài riêng vì đơn thanh toán thử được lưu như các đơn khác.
