# PHẦN I — CÔNG NGHỆ BẮT BUỘC

## 1. Tech Stack

- Programming Language: Java.
- IDE: Android Studio.
- Android SDK: Native Android.
- Minimum SDK: API 26 (Android 8.0).
- Target SDK: Phiên bản ổn định, phù hợp yêu cầu hiện hành của Android.
- UI: XML Layout + Material Components.
- Architecture: MVVM + Repository Pattern.
- Database: Room Persistence Library (SQLite).
- State Management: LiveData + ViewModel.
- Background Processing: ExecutorService.
- Navigation: Activity + Fragment hoặc Navigation Component.
- Build System: Gradle.
- Testing: JUnit, AndroidX Test, Espresso và Room Database Tests.
- Version Control: Git.

Không sử dụng Kotlin, Jetpack Compose, Flutter hoặc React Native.

Ưu tiên Java thuần và các thư viện Android chính thức.

Chọn phiên bản thư viện tương thích, tránh dùng dependency đã lỗi thời hoặc không được duy trì.

Ứng dụng phải hoạt động offline, không yêu cầu backend hoặc tài khoản đăng nhập trong phiên bản đầu tiên.

## 2. Nguyên tắc thiết kế

- Clean Code.
- SOLID.
- Separation of Concerns.
- Single Source of Truth.
- Không viết toàn bộ logic trong Activity hoặc Fragment.
- Không thực hiện truy vấn database trên main thread.
- Không hardcode dữ liệu kinh doanh.
- Không lưu tiền bằng float hoặc double.
- Không xóa dữ liệu doanh thu khi người dùng xóa món.
- Không tự động thực hiện giao dịch ngân hàng.
- Không yêu cầu quyền Android không cần thiết.

---

# PHẦN II — CÁC CHỨC NĂNG

## MODULE 1 — QUẢN LÝ THỰC ĐƠN

Xây dựng chức năng quản lý món ăn và đồ uống.

### Chức năng

1. Thêm món mới.
2. Chỉnh sửa món.
3. Ngừng bán món.
4. Quản lý danh mục.
5. Tìm kiếm món.
6. Lọc theo danh mục.
7. Hiển thị giá bán.
8. Sắp xếp danh sách món.

### Thông tin món

- Product ID.
- Product Name.
- Category ID.
- Selling Price.
- Active Status.
- Created At.
- Updated At.

### Validation

- Tên món không được để trống.
- Giá bán phải lớn hơn hoặc bằng 0.
- Không cho phép nhập số tiền vượt phạm vi lưu trữ.
- Không cho phép thêm món đã ngừng bán vào đơn mới.
- Ngừng bán món không làm mất lịch sử bán hàng.

---

## MODULE 2 — TẠO ĐƠN HÀNG

Xây dựng màn hình thu ngân.

### Chức năng

1. Hiển thị danh sách món.
2. Chọn món để thêm vào giỏ hàng.
3. Thêm nhiều món vào cùng một đơn.
4. Tăng số lượng.
5. Giảm số lượng.
6. Xóa món khỏi giỏ hàng.
7. Hiển thị đơn giá.
8. Hiển thị thành tiền.
9. Tính tổng tiền tự động.
10. Hủy đơn đang tạo.
11. Xác nhận trước khi hủy.

### Công thức

Line Total = Unit Price × Quantity

Order Total = SUM(Line Total)

### Yêu cầu

- Số lượng phải là số nguyên dương.
- Số tiền lưu bằng long, đơn vị VNĐ.
- Kiểm tra tràn số khi nhân và cộng.
- Đơn giá phải được lưu lại tại thời điểm bán.
- Thay đổi giá món không làm thay đổi đơn hàng đã thanh toán.
- Không tạo đơn trùng khi nhấn nút nhiều lần.

---

## MODULE 3 — THANH TOÁN

### Phương thức thanh toán

1. Tiền mặt.
2. Chuyển khoản thủ công.

### Thanh toán tiền mặt

Cho phép nhân viên nhập số tiền khách đưa.

Tự động tính:

Change = Customer Paid - Order Total

Nếu khách đưa thiếu tiền, không cho hoàn tất thanh toán.

### Thanh toán chuyển khoản

- Hiển thị tổng tiền cần chuyển.
- Nhân viên tự kiểm tra tiền đã nhận.
- Yêu cầu xác nhận thủ công.
- Không tự động truy cập tài khoản ngân hàng.
- Không tự động xác nhận giao dịch ngân hàng.

### Trạng thái đơn hàng

- DRAFT: Đang tạo.
- PAID: Đã thanh toán.
- CANCELLED: Đã hủy.

### Quy tắc

- Chỉ đơn PAID được tính vào doanh thu.
- Mỗi đơn chỉ được thanh toán thành công một lần.
- Sử dụng Room Transaction để lưu dữ liệu nhất quán.
- Không tự ý xóa hoặc sửa số tiền của đơn đã thanh toán.
- Lưu thời gian thanh toán.
- Lưu phương thức thanh toán.
- Tạo mã đơn hàng duy nhất.
- Có cơ chế ngăn thanh toán trùng ngay cả khi người dùng nhấn nút liên tục.

---

## MODULE 4 — LỊCH SỬ ĐƠN HÀNG

Xây dựng màn hình lịch sử giao dịch.

### Chức năng

1. Xem danh sách đơn hàng.
2. Xem chi tiết đơn.
3. Tìm kiếm theo mã đơn.
4. Lọc theo ngày.
5. Lọc theo trạng thái.
6. Xem phương thức thanh toán.
7. Xem tổng tiền.
8. Xem danh sách món đã bán.

Đơn đã thanh toán phải giữ nguyên dữ liệu lịch sử.

Không cho phép chỉnh sửa trực tiếp tổng tiền của đơn PAID.

---

## MODULE 5 — THỐNG KÊ DOANH THU

Xây dựng màn hình thống kê.

### Doanh thu theo ngày

- Tổng doanh thu hôm nay.
- Tổng số đơn đã thanh toán.
- Tổng số món đã bán.
- Danh sách đơn hàng trong ngày.
- Cho phép chọn ngày khác.

### Doanh thu theo tháng

- Tổng doanh thu tháng hiện tại.
- Tổng số đơn.
- Tổng số món đã bán.
- Doanh thu theo từng ngày.
- Cho phép chọn tháng khác.

### Doanh thu theo năm

- Tổng doanh thu năm hiện tại.
- Tổng số đơn.
- Tổng số món đã bán.
- Doanh thu theo từng tháng.
- Cho phép chọn năm khác.

### Quy tắc thống kê

Revenue = SUM(total_amount WHERE status = PAID)

- Chỉ tính đơn đã thanh toán.
- Không tính đơn DRAFT hoặc CANCELLED.
- Sử dụng thời gian thanh toán để xác định kỳ doanh thu.
- Múi giờ mặc định: Asia/Ho_Chi_Minh.
- Lưu timestamp nhất quán và chuyển đổi đúng theo múi giờ khi thống kê.
- Sử dụng khoảng thời gian bắt đầu bao gồm, kết thúc không bao gồm.
- Không tính trùng đơn.
- Doanh thu không đồng nghĩa với lợi nhuận.
- Hiển thị tiền tệ theo định dạng Việt Nam.

---

# PHẦN III — DATABASE DESIGN

Sử dụng Room Database.

Thiết kế tối thiểu các bảng sau:

## categories

- id: Long, Primary Key
- name: String

## products

- id: Long, Primary Key
- category_id: Long, Foreign Key
- name: String
- price: Long
- is_active: Boolean
- created_at: Long
- updated_at: Long

## orders

- id: Long, Primary Key
- order_code: String, Unique
- status: String
- payment_method: String, Nullable
- total_amount: Long
- customer_paid: Long, Nullable
- change_amount: Long, Nullable
- created_at: Long
- paid_at: Long, Nullable

## order_items

- id: Long, Primary Key
- order_id: Long, Foreign Key
- product_id: Long, Nullable
- product_name_snapshot: String
- unit_price_snapshot: Long
- quantity: Integer
- line_total: Long

### Database Requirements

1. Thiết lập Primary Key và Foreign Key.
2. Tạo Index cho các trường tìm kiếm thường xuyên.
3. Tạo Unique Index cho order_code.
4. Sử dụng Room Transaction cho các thao tác thanh toán.
5. Không dùng destructive migration.
6. Không xóa dữ liệu lịch sử khi thay đổi thực đơn.
7. Viết migration khi thay đổi schema.
8. Tạo DAO và Repository riêng.
9. Xử lý lỗi database rõ ràng.
10. Đảm bảo dữ liệu vẫn tồn tại sau khi đóng và mở lại ứng dụng.

---

# PHẦN IV — THIẾT KẾ GIAO DIỆN

Sử dụng XML Layout và Material Components.

Giao diện bằng tiếng Việt.

## Các màn hình bắt buộc

### Screen 1 — Dashboard

Hiển thị:
- Doanh thu hôm nay.
- Số đơn đã thanh toán.
- Số món đã bán.
- Nút tạo đơn mới.
- Lối tắt đến thống kê.

### Screen 2 — POS / Order

Hiển thị:
- Danh sách món.
- Danh mục.
- Thanh tìm kiếm.
- Giỏ hàng.
- Số lượng.
- Thành tiền.
- Tổng tiền.
- Nút thanh toán.

### Screen 3 — Payment

Hiển thị:
- Chi tiết đơn.
- Tổng tiền.
- Chọn tiền mặt hoặc chuyển khoản.
- Tiền khách đưa.
- Tiền thừa.
- Nút xác nhận thanh toán.

### Screen 4 — Product Management

Hiển thị:
- Danh sách món.
- Thêm món.
- Sửa món.
- Ngừng bán món.
- Quản lý danh mục.

### Screen 5 — Order History

Hiển thị:
- Danh sách đơn.
- Tìm kiếm.
- Bộ lọc.
- Chi tiết đơn.

### Screen 6 — Revenue Statistics

Hiển thị:
- Doanh thu ngày.
- Doanh thu tháng.
- Doanh thu năm.
- Bộ chọn thời gian.
- Biểu đồ hoặc danh sách thống kê.

### UI Requirements

- Responsive cho điện thoại và máy tính bảng.
- Giao diện rõ ràng, dễ thao tác.
- Nút thanh toán nổi bật.
- Không bị tràn nội dung.
- Hỗ trợ cuộn danh sách.
- Hiển thị trạng thái loading, empty và error.
- Xác nhận trước các thao tác quan trọng.
- Không dùng dữ liệu giả để thay thế dữ liệu thật sau khi hoàn thành tích hợp Room.

---

# PHẦN V — CẤU TRÚC DỰ ÁN

Đề xuất cấu trúc:

app/
  src/
    main/
      java/com/example/androidpos/
        data/
          local/
            dao/
            entity/
            database/
          repository/
        domain/
          model/
          util/
        ui/
          dashboard/
          products/
          orders/
          payment/
          history/
          statistics/
        viewmodel/
        utils/
      res/
        layout/
        drawable/
        values/
        menu/
        navigation/
    test/
    androidTest/

Có thể điều chỉnh cấu trúc nếu cần nhưng phải giải thích lý do.

---

# PHẦN VI — KIỂM THỬ TỰ ĐỘNG

## Unit Tests

Kiểm thử:

1. Tính thành tiền.
2. Tính tổng tiền.
3. Tính tiền thừa.
4. Kiểm tra số lượng.
5. Kiểm tra giá bán.
6. Xử lý tràn số.
7. Không cho thanh toán thiếu.
8. Không cho thanh toán trùng.
9. Tính doanh thu theo ngày.
10. Tính doanh thu theo tháng.
11. Tính doanh thu theo năm.
12. Không tính đơn chưa thanh toán.
13. Không tính đơn đã hủy.

## Database Tests

Kiểm thử:

- Insert.
- Update.
- Query.
- Foreign Key.
- Unique Constraint.
- Transaction.
- Migration.
- Lưu lịch sử giá bán.
- Dữ liệu không bị mất sau khi mở lại database.

## UI Tests

Kiểm thử:

- Chọn món.
- Thay đổi số lượng.
- Thanh toán.
- Xem lịch sử.
- Xem thống kê.
- Hiển thị trên màn hình điện thoại.

## Build Verification

Thực hiện các lệnh Gradle phù hợp, ví dụ:

- assembleDebug
- testDebugUnitTest
- connectedDebugAndroidTest khi có thiết bị hoặc emulator.

Nếu môi trường không có Android SDK, JDK hoặc emulator, báo cáo chính xác giới hạn và không tuyên bố kiểm thử đã thành công.

---

# PHẦN VII — QUY TRÌNH PHÁT TRIỂN THEO GIAI ĐOẠN

## PHASE 0 — ANALYSIS

Nhiệm vụ:

- Kiểm tra thư mục hoặc repository.
- Xác định có dự án Android hiện tại hay chưa.
- Phân tích yêu cầu.
- Chọn phiên bản công nghệ.
- Thiết kế kiến trúc.
- Đề xuất cấu trúc thư mục.
- Xác định rủi ro.
- Lập kế hoạch triển khai.
- Xác định phiên bản Android hỗ trợ để chọn Android Photo Picker và cơ chế dự phòng tương thích.
- Xác định phiên bản schema hiện có để lập kế hoạch Room Migration nếu ứng dụng đã có database.

Acceptance Criteria:

- Báo cáo tiếng Việt nêu phương án chọn ảnh, lưu ảnh offline, tạo ảnh thu nhỏ và bảo toàn dữ liệu khi nâng cấp.

## PHASE 1 — PROJECT INITIALIZATION

Nhiệm vụ:

- Khởi tạo Android Project bằng Java.
- Cấu hình Gradle.
- Cấu hình Material Components.
- Cấu hình ViewModel và LiveData.
- Cấu hình Room.
- Tạo cấu trúc package.
- Tạo màn hình khởi động cơ bản.

Acceptance Criteria:

- Gradle sync thành công.
- Build debug thành công khi môi trường hỗ trợ.
- Không có lỗi compile.
- Ứng dụng có thể khởi chạy khi có thiết bị hoặc emulator.

 

## PHASE 2 — DATABASE

Nhiệm vụ:

- Tạo Entity.
- Tạo DAO.
- Tạo Room Database.
- Tạo Repository.
- Thiết lập quan hệ bảng.
- Viết Database Tests.
- Bổ sung trường ảnh và kiểm thử nâng cấp schema nếu có database hiện hữu.

Acceptance Criteria:

- Database hoạt động.
- Test CRUD đạt.
- Quan hệ dữ liệu hợp lệ.
- Không có destructive migration.
- Bảng `products` có trường `image_path` kiểu String, cho phép null. Room chỉ lưu đường dẫn tương đối hoặc tên file trong bộ nhớ riêng của ứng dụng.
- Nếu đã có database, Room Migration nâng cấp schema và giữ nguyên dữ liệu sản phẩm. Kiểm thử trên schema cũ xác nhận giá trị `image_path` của các sản phẩm hiện có là null sau nâng cấp.

 

## PHASE 3 — PRODUCT MANAGEMENT

Nhiệm vụ:

- Xây dựng màn hình quản lý món.
- Thêm món.
- Sửa món.
- Ngừng bán món.
- Tìm kiếm món.
- Tích hợp chọn, thay và xóa ảnh. Chuẩn bị ảnh thu nhỏ và quản lý vòng đời file ảnh trong bộ nhớ riêng.
- Kiểm thử chức năng.

Acceptance Criteria:

- CRUD phù hợp yêu cầu.
- Validation hoạt động.
- Dữ liệu được lưu trong Room.
- Không làm mất lịch sử đơn hàng.
- Mỗi sản phẩm có tối đa một ảnh đại diện. Có thể thêm sản phẩm có hoặc không có ảnh, đổi ảnh và xóa ảnh khi chỉnh sửa.
- Chọn ảnh bằng Android Photo Picker hoặc cơ chế tương thích với phiên bản Android. Luồng chọn ảnh hoạt động với quyền truy cập vào ảnh đã chọn.
- Sao chép ảnh vào bộ nhớ riêng của ứng dụng và tạo ảnh thu nhỏ. Danh sách quản lý hiển thị ảnh thu nhỏ và ảnh vẫn hiển thị offline sau khi khởi động lại.
- Hiển thị ảnh mặc định khi sản phẩm chưa có ảnh hoặc file bị thiếu, lỗi hay không đọc được.
- Dùng lại file khi chọn lại cùng nội dung ảnh. Khi thay hoặc xóa ảnh, chỉ dọn file sau khi cập nhật Room thành công và xác nhận file không còn được tham chiếu. Nếu cập nhật thất bại, ảnh cũ vẫn dùng được.

 

## PHASE 4 — ORDER MANAGEMENT

Nhiệm vụ:

- Xây dựng màn hình POS.
- Hiển thị ảnh trên thẻ chọn món và dùng luồng thêm vào giỏ hiện có khi nhấn thẻ.
- Chọn món.
- Thêm món vào giỏ.
- Tăng giảm số lượng.
- Tính tổng tiền.
- Hủy đơn.
- Viết Unit Tests.

Acceptance Criteria:

- Tổng tiền chính xác.
- Giỏ hàng hoạt động.
- Không có số lượng không hợp lệ.
- Không phát sinh lỗi tràn số.
- Màn hình chọn món hiển thị thẻ có ảnh sản phẩm, tên và giá bán. Dùng ảnh mặc định khi không tải được ảnh sản phẩm.
- Nhấn vào thẻ của món đang bán sẽ thêm món vào giỏ hàng.

 

## PHASE 5 — PAYMENT

Nhiệm vụ:

- Thanh toán tiền mặt.
- Ghi nhận chuyển khoản thủ công.
- Tính tiền thừa.
- Lưu đơn PAID.
- Ngăn thanh toán trùng.
- Viết kiểm thử.

Acceptance Criteria:

- Thanh toán hợp lệ được ghi nhận.
- Không thanh toán thiếu.
- Không có doanh thu trùng.
- Dữ liệu được lưu nhất quán.

 

## PHASE 6 — ORDER HISTORY

Nhiệm vụ:

- Danh sách đơn hàng.
- Chi tiết đơn.
- Tìm kiếm.
- Lọc dữ liệu.
- Kiểm thử.

Acceptance Criteria:

- Lịch sử chính xác.
- Không mất dữ liệu khi thay đổi giá món.
- Không sửa trực tiếp đơn đã thanh toán.

 

## PHASE 7 — REVENUE STATISTICS

Nhiệm vụ:

- Doanh thu ngày.
- Doanh thu tháng.
- Doanh thu năm.
- Bộ lọc thời gian.
- Biểu đồ.
- Kiểm thử truy vấn thống kê.

Acceptance Criteria:

- Doanh thu đúng.
- Không tính đơn chưa thanh toán.
- Không tính trùng đơn.
- Xử lý đúng ranh giới thời gian.

 

## PHASE 8 — UI/UX POLISH

Nhiệm vụ:

- Hoàn thiện giao diện.
- Cải thiện trải nghiệm sử dụng.
- Tối ưu danh sách món.
- Tối ưu thao tác thanh toán.
- Kiểm tra màn hình nhỏ và tablet.
- Kiểm thử UI.

Acceptance Criteria:

- Thẻ sản phẩm và danh sách quản lý hiển thị rõ ràng trên điện thoại và máy tính bảng.
- Kiểm tra cuộn danh sách khi tải ảnh thu nhỏ. Việc đọc và giải mã ảnh chạy ngoài luồng giao diện để tránh giật hoặc treo.

 

## PHASE 9 — FINAL QA & APK

Nhiệm vụ:

- Rà soát toàn bộ code.
- Kiểm tra lỗi.
- Chạy Unit Tests.
- Chạy Database Tests.
- Chạy UI Tests nếu có thiết bị.
- Build APK.
- Kiểm tra ứng dụng trên emulator hoặc thiết bị thật.
- Viết README hướng dẫn sử dụng.
- Tổng hợp báo cáo hoàn thành.

Acceptance Criteria:

- Build thành công.
- Không còn lỗi nghiêm trọng đã biết.
- Các chức năng chính được kiểm chứng.
- Có hướng dẫn cài đặt.
- Có APK nếu môi trường build hỗ trợ.
- Kiểm thử thêm sản phẩm có và không có ảnh, đổi ảnh, xóa ảnh đại diện, hiển thị offline sau khi khởi động lại và hiển thị ảnh mặc định khi file ảnh bị thiếu hoặc không đọc được.
- Nếu có schema cũ, kiểm thử nâng cấp Room xác nhận dữ liệu sản phẩm được giữ nguyên.
---
