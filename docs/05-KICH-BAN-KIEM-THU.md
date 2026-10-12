# Kịch bản kiểm thử nghiệm thu — CRM Quản lý Khách hàng

> Test case theo use case trong `03-USE-CASE.md`. Phần nền (đăng nhập, phân quyền, đặt lại dữ liệu) dùng TC-SYS-01 đến TC-SYS-10 trong `ZDOCS/03-KIEN-TRUC/USE-CASE-NEN.md` §8.

---

## 1. Cách chạy

| Mục | Nội dung |
| :--- | :--- |
| Môi trường | Chạy cả hai chế độ: dữ liệu mẫu (`mock`) và BE thật (`api`) |
| Dữ liệu | Đặt lại dữ liệu demo trước mỗi lượt test |
| Thứ tự | Chạy theo thứ tự mã TC trong một lượt; số liệu mong đợi giả định các TC trước đã chạy |
| Tài khoản | Theo cột "Vai trò": Trưởng phòng là `quanly`, Sales là `nhanvien` (Nguyễn Minh Tuấn), Sales 2 là `sales2` (Lê Hoàng Yến) |
| Thiết bị | Desktop 1366×768; TC-05, TC-27 chạy thêm một lần ở cửa sổ 375 px |
| Đạt | Kết quả thực tế khớp toàn bộ cột "Kết quả mong đợi" |

Dữ liệu mẫu dùng trong test:

- **Sales:** Nguyễn Minh Tuấn (`nhanvien`) đang giữ 3 lead Mới; Lê Hoàng Yến (`sales2`) giữ đúng 8 lead Mới; Đỗ Quang Huy giữ 4 lead Mới; Vũ Thị Mai, Bùi Đức Long, Hoàng Ngọc Anh mỗi người dưới 20 lead Mới.
- **Quy tắc phân lead:** đang bật; thứ tự Tuấn, Yến, Huy, Mai, Long, Ngọc Anh; ngưỡng 30; người nhận gần nhất là Hoàng Ngọc Anh.
- **Lead của Tuấn:** "Trần Văn Bình" (Mới, SĐT `0912345678`, email `binh.tran@thepvietnhat.vn`, công ty "Công ty CP Thép Việt Nhật", phân 26 giờ trước, chưa có hoạt động); "Lý Thị Thu" (Mới, phân 30 giờ trước, chưa có hoạt động); "Hoàng Văn Nam" (Đang liên hệ); "Võ Thanh Sơn" (Đang liên hệ, công ty ghi "công ty cp vận tải  nam long").
- **Lead của Yến:** "Phạm Thị Lan" (Đang liên hệ, SĐT `0987654321`); "Trịnh Quốc Việt" (Đang liên hệ, công ty "Công ty CP Vận tải Nam Long").
- **Khách hàng:** "Công ty CP Vận tải Nam Long" `KH00012`, MST `0312345678`, của Tuấn. "Công ty TNHH Dược phẩm Minh Châu", SĐT `02873001234`, của Yến.
- **Cơ hội của Tuấn:** "Phần mềm điều phối xe — Vận tải Nam Long" 260.000.000 ₫, Khảo sát nhu cầu; "Máy chấm công nhà xưởng — Bao bì Phú Thịnh" Thương lượng, ngày chốt dự kiến là hôm qua; "Phần mềm kho — Thực phẩm Sao Mai" Thua (lý do Giá cao), đóng từ giai đoạn Thương lượng.
- **Cơ hội của Yến:** "Phần mềm quản lý nhà thuốc — Minh Châu" Thắng trong tháng này; "Máy chấm công — Minh Châu" 36.000.000 ₫, Tiếp cận.
- **Hoạt động:** "Gọi lại Bao bì Phú Thịnh" của Tuấn, Kế hoạch, hôm qua 15:00 (quá hạn).
- **Chỉ tiêu:** tháng này của Tuấn 500.000.000 ₫; doanh số đã thắng trong tháng 180.000.000 ₫.
- **Sản phẩm:** `SP001` Phần mềm quản lý vận tải — bản quyền 12 tháng, Gói, 180.000.000 ₫, Không chịu thuế; `DV001` Triển khai, cấu hình hệ thống, Gói, 60.000.000 ₫, 10%; `TB001` Máy chấm công khuôn mặt, Cái, 4.500.000 ₫, 10%.

---

## 2. Test case

### 2.1. Lead

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-01 | UC-01 | Sales thêm lead hợp lệ | Sales | 1. Lead → Thêm lead 2. Họ tên "Đặng Thị Hồng", SĐT `0903 111 222`, email `hong.dang@gmail.com`, công ty "Công ty TNHH May mặc Phương Nam", nguồn Facebook, mức quan tâm Ấm 3. Lưu | Lead có mã `LD{yyMMdd}{nnn}`, trạng thái Mới, sales phụ trách Nguyễn Minh Tuấn; SĐT lưu `0903111222` |
| TC-02 | UC-01 | Trùng SĐT khác định dạng, trùng email khác hoa thường | Sales 2 | 1. Thêm lead "Bùi Văn Tâm", SĐT `+84 912.345.678` 2. Xóa SĐT, nhập email ` BINH.TRAN@ThepVietNhat.vn ` 3. Gõ thẳng URL chi tiết lead Trần Văn Bình | Bước 1: báo "Trùng SĐT với lead LD… do Nguyễn Minh Tuấn phụ trách", nút Lưu khóa. Bước 2: báo trùng email tương tự. Bước 3: "Không tìm thấy bản ghi" |
| TC-03 | UC-01 | Thiếu liên lạc, SĐT sai | Sales | 1. Thêm lead chỉ có họ tên "Kiều Văn Hải", Lưu 2. Nhập SĐT `0123 456 78` | Bước 1: báo phải có SĐT hoặc email. Bước 2: báo "Số điện thoại không hợp lệ" (9 chữ số sau chuẩn hóa) |
| TC-04 | UC-01 | Không đạt và mở lại | Sales, Trưởng phòng | 1. Mở lead "Hoàng Văn Nam", bấm Không đạt, không chọn lý do 2. Chọn "Ngân sách không phù hợp", xác nhận 3. Tìm nút Mở lại 4. Đổi sang Trưởng phòng, mở lead, bấm Mở lại | Bước 1: không xác nhận được. Bước 2: lead "Không đạt". Bước 3: sales không có nút. Bước 4: lead về "Đang liên hệ" |
| TC-05 | UC-01 | Nhãn Chậm liên hệ mất khi ghi hoạt động | Sales | 1. Mở Lead, lọc "Chậm liên hệ" 2. Mở "Trần Văn Bình", Ghi hoạt động: Gọi điện, kết quả "Không nghe máy" 3. Lưu, xem lại danh sách | Bước 1: có Trần Văn Bình và Lý Thị Thu, nhãn đỏ. Bước 3: Trần Văn Bình chuyển "Đang liên hệ", không còn nhãn, rời bộ lọc |
| TC-06 | UC-02 | Phân vòng tròn | Trưởng phòng | 1. Quy tắc phân lead: chỉ giữ Tuấn, Yến, Huy theo thứ tự đó, lưu 2. Thêm 3 lead không chọn sales: "Nguyễn Hữu Phước" `0905100001`, "Trần Mỹ Linh" `0905100002`, "Lê Quốc Khánh" `0905100003` | Lần lượt giao cho Tuấn, Yến, Huy (người nhận gần nhất Ngọc Anh không còn trong danh sách nên bắt đầu từ đầu); lịch sử phân ghi "Hệ thống phân"; mỗi sales có thông báo |
| TC-07 | UC-02 | Bỏ qua sales đạt ngưỡng | Trưởng phòng | 1. Đặt ngưỡng 8, lưu 2. Thêm 2 lead "Đào Minh Khôi" `0905100004`, "Phùng Thị Hạnh" `0905100005` | Yến đang giữ 9 lead Mới (≥ 8) nên bị bỏ qua; 2 lead giao cho Tuấn và Huy |
| TC-08 | UC-02 | Phân lại thủ công | Trưởng phòng | 1. Lọc lead của Đỗ Quang Huy 2. Tick "Lê Quốc Khánh", "Phùng Thị Hạnh", bấm Phân cho → Lê Hoàng Yến, ghi chú "Khu vực miền Nam" 3. Mở chi tiết một lead | Hai lead đổi sang Yến; lịch sử phân ghi người phân `quanly`, thời điểm, ghi chú; thời điểm phân mới (mốc 24 giờ tính lại); Yến có thông báo |
| TC-09 | UC-02 | Sales không phân được lead | Sales | 1. Mở Lead 2. Gõ thẳng URL `/leads/import` | Bước 1: không có nút "Phân cho", "Quy tắc phân lead", "Nhập Excel"; không có lead "Chưa phân". Bước 2: chuyển tới `/forbidden` |
| TC-10 | UC-03 | Nhập Excel có dòng lỗi và trùng | Trưởng phòng | 1. Quy tắc phân lead: đủ 6 sales theo thứ tự ban đầu, ngưỡng 30 2. Nhập Excel file 20 dòng, cột sales bỏ trống: dòng 1–17 hợp lệ nguồn "Hội chợ"; dòng 18 SĐT `0912 345 678`; dòng 19 trùng email dòng 2; dòng 20 SĐT `09123` 3. Kiểm tra 4. Nhập 5. Tải file dòng không nhập | Bước 3: Hợp lệ 17, Trùng 2, Lỗi 1, mỗi dòng có lý do. Bước 4: tạo 17 lead nguồn Hội chợ; Mai, Long, Ngọc Anh, Tuấn, Yến mỗi người 3 lead, Huy 2 lead. Bước 5: file có 3 dòng kèm cột lý do |
| TC-11 | UC-03 | File không hợp lệ | Trưởng phòng | 1. Chọn file `.csv` 2. Chọn file `.xlsx` 1.001 dòng 3. Chọn file `.xlsx` bị xóa cột "Họ tên" | Lần lượt báo "Chỉ hỗ trợ file .xlsx", "Tối đa 1.000 dòng", "File không đúng mẫu"; không lead nào được tạo |

### 2.2. Chuyển đổi lead, khách hàng

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-12 | UC-04 | Chuyển đổi tạo khách hàng mới | Sales | 1. Mở lead "Trần Văn Bình", bấm Chuyển đổi 2. Giữ "Tạo khách hàng mới"; bật Tạo cơ hội: tên "Hệ thống chấm công — Thép Việt Nhật", giá trị 85.000.000 ₫, giữ ngày chốt mặc định 3. Chuyển đổi | Có khách hàng doanh nghiệp "Công ty CP Thép Việt Nhật" mã `KH{5 số}`, sales Nguyễn Minh Tuấn; người liên hệ chính Trần Văn Bình; cơ hội ở Tiếp cận 10%, gia quyền 8.500.000 ₫, ngày chốt hôm nay + 30; lead "Đã chuyển đổi", chỉ đọc; tab Hoạt động của khách có cuộc gọi "Không nghe máy" ở TC-05 |
| TC-13 | UC-04 | Gợi ý gắn vào khách hàng có sẵn | Sales | 1. Mở lead "Võ Thanh Sơn", bấm Chuyển đổi 2. Chọn "Gắn vào Công ty CP Vận tải Nam Long", tắt Tạo cơ hội 3. Chuyển đổi | Bước 1: hệ thống gợi ý `KH00012` dù tên công ty viết thường và thừa khoảng trắng. Bước 3: không tạo khách hàng mới; Nam Long có thêm người liên hệ Võ Thanh Sơn |
| TC-14 | UC-04 | Khách hàng trùng thuộc sales khác | Sales 2 | 1. Mở lead "Trịnh Quốc Việt", bấm Chuyển đổi | Báo "Khách hàng KH00012 do Nguyễn Minh Tuấn phụ trách, liên hệ trưởng phòng"; nút Chuyển đổi khóa; lead giữ "Đang liên hệ" |
| TC-15 | UC-05 | Thêm khách doanh nghiệp, đổi liên hệ chính | Sales | 1. Thêm khách hàng Doanh nghiệp "Công ty TNHH Gỗ Mỹ Nghệ Tân Phú", MST `3701234567`, Bình Dương, quy mô 50–199 2. Thêm người liên hệ "Phan Văn Lợi", Giám đốc, tick chính 3. Thêm "Trần Thị Mỹ", Kế toán trưởng, tick chính | Khách có mã `KH{5 số}`, sales Nguyễn Minh Tuấn; sau bước 3 Trần Thị Mỹ là liên hệ chính, Phan Văn Lợi tự bỏ dấu chính |
| TC-16 | UC-05 | MST trùng, sai định dạng | Sales | 1. Thêm khách doanh nghiệp, MST `0312345678` 2. Đổi MST `031234567` 3. Đổi MST `0312345679-01` | Bước 1: "MST đã tồn tại". Bước 2 và 3: "MST không đúng định dạng"; không lưu |
| TC-17 | UC-05 | Phạm vi dữ liệu | Sales, Trưởng phòng | 1. Sales mở Khách hàng, tìm "Minh Châu" và `028 7300 1234` 2. Gõ URL hồ sơ Dược phẩm Minh Châu 3. Mở Pipeline 4. Đổi sang Trưởng phòng, tìm "Minh Châu" | Bước 1: không có kết quả. Bước 2: "Không tìm thấy bản ghi". Bước 3: không có thẻ "Máy chấm công — Minh Châu". Bước 4: thấy khách, sales phụ trách Lê Hoàng Yến |
| TC-18 | UC-05 | Xóa khách hàng đã có cơ hội | Sales, Trưởng phòng | 1. Sales mở hồ sơ Vận tải Nam Long, tìm nút Xóa 2. Trưởng phòng mở cùng hồ sơ, bấm Xóa | Bước 1: không có nút Xóa. Bước 2: từ chối, báo "Khách hàng đã có cơ hội hoặc báo giá" |
| TC-19 | UC-06 | Chuyển giao toàn bộ | Trưởng phòng | 1. Ghi lại doanh số tháng này của Lê Hoàng Yến ở Báo cáo 2. Khách hàng → Chuyển giao: từ Lê Hoàng Yến sang Đỗ Quang Huy, xác nhận 3. Xem lại khách Minh Châu, hai cơ hội của Minh Châu, lead Phạm Thị Lan, báo cáo doanh số | Minh Châu, cơ hội "Máy chấm công — Minh Châu", lead Phạm Thị Lan chuyển sang Huy; cơ hội Thắng "Phần mềm quản lý nhà thuốc — Minh Châu" vẫn của Yến; doanh số tháng của Yến không đổi so với bước 1 |

### 2.3. Cơ hội và pipeline

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-20 | UC-07 | Tạo cơ hội | Sales | 1. Ghi lại đầu cột Tiếp cận 2. Thêm cơ hội "Camera giám sát kho bãi — Nam Long", khách Vận tải Nam Long, giá trị 120.000.000 ₫, ngày chốt hôm nay + 45 | Thẻ mới ở cột Tiếp cận, mã `CH{yyMMdd}{nnn}`, xác suất 10%; đầu cột tăng 1 cơ hội, tổng giá trị tăng 120.000.000 ₫, gia quyền tăng 12.000.000 ₫ |
| TC-21 | UC-07 | Giá trị, ngày chốt không hợp lệ | Sales | 1. Thêm cơ hội giá trị 0 ₫ 2. Giá trị 1.500.500 ₫ 3. Giá trị 50.000.000 ₫, ngày chốt hôm qua | Bước 1, 2: báo lỗi tại ô giá trị (phải lớn hơn 0, bội số 1.000 ₫). Bước 3: báo lỗi tại ô ngày chốt; không lưu |
| TC-22 | UC-08 | Kéo đổi giai đoạn | Sales | 1. Kéo "Camera giám sát kho bãi — Nam Long" từ Tiếp cận sang Thương lượng 2. Mở chi tiết, xem lịch sử giai đoạn | Xác suất 70%, gia quyền 84.000.000 ₫; cột Tiếp cận giảm 120.000.000 ₫ tổng và 12.000.000 ₫ gia quyền; cột Thương lượng tăng 120.000.000 ₫ và 84.000.000 ₫; lịch sử ghi Tiếp cận → Thương lượng, người đổi, thời điểm |
| TC-23 | UC-08 | Thắng | Sales | 1. Kéo "Camera giám sát kho bãi — Nam Long" vào vùng Thắng 2. Giữ giá trị gợi ý, ngày chốt hôm nay, xác nhận 3. Mở Tổng quan | Bước 1: giá trị gợi ý 120.000.000 ₫ (chưa có báo giá). Bước 2: thẻ rời bảng. Bước 3: doanh số tháng 300.000.000 ₫, đạt 60,0% chỉ tiêu (trước đó 180.000.000 ₫, 36,0%) |
| TC-24 | UC-08 | Thua thiếu lý do, hủy | Sales | 1. Kéo "Máy chấm công nhà xưởng — Bao bì Phú Thịnh" vào vùng Thua 2. Không chọn lý do 3. Chọn "Khác", để trống ghi chú 4. Bấm Hủy | Bước 2, 3: nút Xác nhận khóa. Bước 4: thẻ về cột Thương lượng, không đổi gì |
| TC-25 | UC-07 | Nhãn Quá hạn chốt | Sales | 1. Xem thẻ "Máy chấm công nhà xưởng — Bao bì Phú Thịnh" 2. Sửa ngày chốt dự kiến thành hôm nay + 5 | Bước 1: thẻ có nhãn "Quá hạn chốt". Bước 2: nhãn mất |
| TC-26 | UC-08 | Mở lại cơ hội Thua | Sales, Trưởng phòng | 1. Sales mở "Phần mềm kho — Thực phẩm Sao Mai" 2. Đổi sang Trưởng phòng, mở cơ hội, bấm Mở lại | Bước 1: mọi ô chỉ đọc, không có nút Mở lại. Bước 2: cơ hội về cột Thương lượng, lý do thua bị xóa |

### 2.4. Hoạt động chăm sóc

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-27 | UC-09 | Ghi cuộc gọi kèm việc tiếp theo | Sales | 1. Mở hồ sơ Vận tải Nam Long, bấm Gọi rồi Ghi hoạt động 2. Kết quả "Đã trao đổi", ghi chú "Khách muốn demo tại văn phòng" 3. Bật Tạo việc tiếp theo: Gặp mặt, ngày mai 09:00, 60 phút, nhắc trước 1 giờ 4. Lưu | Có hai hoạt động: cuộc gọi Hoàn thành và buổi gặp Kế hoạch; tab Sắp tới có buổi gặp; ngày tương tác gần nhất của Nam Long là hôm nay. Ở cửa sổ 375 px, nút Gọi mở liên kết `tel:` |
| TC-28 | UC-09 | Hoàn thành không nhập kết quả | Sales | 1. Hoạt động → tab Quá hạn 2. Bấm Hoàn thành ở "Gọi lại Bao bì Phú Thịnh", để trống kết quả, lưu | Báo lỗi bắt buộc nhập kết quả; hoạt động vẫn ở tab Quá hạn |
| TC-29 | UC-09 | Ghi lùi quá 7 ngày | Sales, Trưởng phòng | 1. Sales ghi hoạt động Gặp mặt cho khách Nam Long, thời gian hôm nay − 10 ngày 2. Thời gian ngày mai 3. Trưởng phòng ghi lại như bước 1 | Bước 1: "Chỉ ghi nhận hoạt động trong 7 ngày gần nhất". Bước 2: "Thời gian không được ở tương lai". Bước 3: lưu được |
| TC-30 | UC-10 | Nhắc trước giờ hẹn | Sales | 1. Thêm hoạt động Gọi điện cho khách Nam Long, bắt đầu sau 16 phút, nhắc trước 15 phút 2. Chờ 1 phút | Chuông tăng 1 thông báo chưa đọc; bấm thông báo mở đúng hoạt động |

### 2.5. Báo giá

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-31 | UC-11 | Tính tiền báo giá | Sales | 1. Mở cơ hội "Phần mềm điều phối xe — Vận tải Nam Long", Lập báo giá 2. Thêm `SP001` SL 1, CK 5%; `DV001` SL 1, CK 0%; `TB001` SL 4, CK 0% 3. Lưu | Thành tiền dòng 171.000.000 ₫, 60.000.000 ₫, 18.000.000 ₫; thuế dòng 0 ₫, 6.000.000 ₫, 1.800.000 ₫. Tổng tiền hàng 258.000.000 ₫, chiết khấu 9.000.000 ₫, trước thuế 249.000.000 ₫, thuế 7.800.000 ₫, tổng thanh toán 256.800.000 ₫. Trạng thái Nháp, mã `BG{yyMMdd}{nnn}`, hạn hiệu lực hôm nay + 15; có nút "Đánh dấu đã gửi" |
| TC-32 | UC-11 | Làm tròn tiền | Sales | 1. Trong báo giá TC-31, thêm `TB001` SL 3, sửa đơn giá 1.234.567 ₫, CK 7% 2. Xem dòng 3. Xóa dòng vừa thêm | Thành tiền dòng 3.444.442 ₫ (3.444.441,93 làm tròn); thuế dòng 344.444 ₫. Sau bước 3, các tổng trở về như TC-31 |
| TC-33 | UC-11 | Chiết khấu vượt hạn mức, duyệt, gửi | Sales, Trưởng phòng | 1. Sửa CK dòng `SP001` thành 15% 2. Gửi duyệt 3. Đổi sang Trưởng phòng, mở thông báo, Duyệt 4. Đổi về Sales, bấm Đánh dấu đã gửi 5. Xem cơ hội | Bước 1: thành tiền dòng 153.000.000 ₫, chiết khấu 27.000.000 ₫, trước thuế 231.000.000 ₫, thuế 7.800.000 ₫, tổng thanh toán 238.800.000 ₫; nút "Gửi duyệt" thay "Đánh dấu đã gửi". Bước 2: Chờ duyệt, trưởng phòng có thông báo. Bước 3: Đã duyệt, sales có thông báo. Bước 5: cơ hội chuyển Khảo sát nhu cầu → Gửi báo giá, xác suất 50%, gia quyền 130.000.000 ₫ |
| TC-34 | UC-11 | Chiết khấu vượt giới hạn cứng | Sales | 1. Lập báo giá mới cho cơ hội "Hệ thống chấm công — Thép Việt Nhật", thêm `TB001` SL 10, CK 35% | Báo "Chiết khấu tối đa 30%", không lưu |
| TC-35 | UC-11 | Khách đồng ý | Sales | 1. Mở báo giá TC-33, bấm Khách đồng ý 2. Xem cơ hội 3. Thử sửa báo giá | Cơ hội có giá trị dự kiến 231.000.000 ₫, chuyển Chờ ký hợp đồng 90%, gia quyền 207.900.000 ₫; báo giá chỉ đọc |

### 2.6. Tổng quan, báo cáo, chỉ tiêu

| Mã | UC | Tình huống | Vai trò | Các bước | Kết quả mong đợi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-36 | UC-12 | Tổng quan của sales | Sales | 1. Đăng nhập nhanh vai trò Nhân viên | Doanh số tháng 300.000.000 ₫, 60,0% chỉ tiêu; Việc cần làm có lead chậm liên hệ "Lý Thị Thu" (không có Trần Văn Bình), hoạt động quá hạn "Gọi lại Bao bì Phú Thịnh", cơ hội sắp chốt "Máy chấm công nhà xưởng — Bao bì Phú Thịnh"; không có bảng xếp hạng; không có dữ liệu của sales khác |
| TC-37 | UC-12 | Tổng quan của trưởng phòng | Trưởng phòng | 1. Mở Tổng quan | Thẻ doanh số bằng tổng cột doanh số của 6 sales trong bảng xếp hạng; Tuấn có 300.000.000 ₫, 60,0%; lead chậm liên hệ gồm lead của nhiều sales |
| TC-38 | UC-13 | Báo cáo doanh số cập nhật sau khi thắng | Trưởng phòng | 1. Báo cáo → Doanh số, khoảng tháng này | Dòng Nguyễn Minh Tuấn: doanh số 300.000.000 ₫, chỉ tiêu 500.000.000 ₫, 60,0%, có cơ hội "Camera giám sát kho bãi — Nam Long"; tổng các dòng bằng cột tháng này trên biểu đồ |
| TC-39 | UC-13 | Phễu chuyển đổi theo nguồn | Trưởng phòng | 1. Báo cáo → Phễu chuyển đổi, nguồn Website, 90 ngày 2. Ở Lead, lọc nguồn Website, ngày tạo 90 ngày, mọi trạng thái; rồi lọc thêm "Đã chuyển đổi" | Số lead và số lead chuyển đổi ở bước 1 khớp số dòng đếm ở bước 2; tỉ lệ = chuyển đổi ÷ lead, làm tròn 1 chữ số thập phân |
| TC-40 | UC-13 | Sales không xem được báo cáo | Sales | 1. Gõ thẳng URL `/reports` | Chuyển tới `/forbidden`; menu không có mục Báo cáo |
| TC-41 | UC-14 | Đặt chỉ tiêu tháng sau | Trưởng phòng | 1. Báo cáo → Chỉ tiêu, nhập tháng sau của Nguyễn Minh Tuấn 550.000.000 ₫, lưu 2. Sửa thành 550.500.000 ₫ | Bước 1: lưu, tổng đội tháng sau tăng 550.000.000 ₫. Bước 2: báo lỗi bội số 1.000.000 ₫ |
| TC-42 | UC-12 | Tháng chưa có chỉ tiêu | Trưởng phòng, Sales | 1. Xóa trống chỉ tiêu tháng này của Nguyễn Minh Tuấn, lưu 2. Đổi sang Nhân viên, mở Tổng quan | Thẻ doanh số vẫn hiện 300.000.000 ₫, phần trăm chỉ tiêu hiển thị "—" |

---

## 3. Biên bản chạy thử

| Ngày | Người test | Chế độ | Số TC đạt / tổng | Lỗi ghi nhận |
| :--- | :--- | :--- | :---: | :--- |
| | | | | |
