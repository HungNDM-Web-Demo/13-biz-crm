# Kịch bản demo — CRM Quản lý Khách hàng

> Dùng khi trình bày cho giám đốc, trưởng phòng kinh doanh của doanh nghiệp có đội sales. Thời lượng 12–15 phút. Đi theo một ngày làm việc của phòng kinh doanh: nhận lead từ hội chợ, chia cho sales, gọi điện, chuyển đổi, báo giá, chốt đơn, xem báo cáo.

---

## 1. Chuẩn bị trước buổi demo

- [ ] Mở sẵn link demo, thử "Đăng nhập nhanh" cả 3 vai trò; đăng nhập thử tài khoản `sales2`
- [ ] Đặt lại dữ liệu demo (thanh công cụ demo → Đặt lại dữ liệu) để có số liệu 90 ngày tính tới hôm nay
- [ ] Đổi tên, logo, màu theo thương hiệu khách (Cài đặt → Thương hiệu) nếu đã biết
- [ ] Chuẩn bị file `lead-hoi-cho.xlsx` từ file mẫu (Lead → Nhập Excel → Tải file mẫu): 17 dòng hợp lệ nguồn "Hội chợ", 1 dòng trùng SĐT `0912 345 678` (lead Trần Văn Bình có sẵn), 1 dòng trùng email với một dòng khác trong file, 1 dòng SĐT sai `09123`
- [ ] Mở link demo trên điện thoại để cho thấy nút Gọi mở trình gọi điện (không bấm gọi thật)
- [ ] Ghi nhớ dữ liệu mẫu: lead "Trần Văn Bình" đang chậm liên hệ; cơ hội "Phần mềm điều phối xe — Vận tải Nam Long" 260.000.000 ₫ ở giai đoạn Khảo sát nhu cầu; chỉ tiêu tháng này của Nguyễn Minh Tuấn 500.000.000 ₫, đã đạt 180.000.000 ₫

| Link | Ghi chú |
| :--- | :--- |
| FE | `http://localhost:3113` hoặc link online |
| Swagger | `https://localhost:7113/swagger` (khi khách hỏi về tích hợp) |

| Tài khoản | Vai trò | Dùng để cho xem |
| :--- | :--- | :--- |
| `admin` | Quản trị viên | Phân quyền, thương hiệu |
| `quanly` | Trưởng phòng kinh doanh (Phạm Thu Trang) | Tổng quan toàn đội, nhập và phân lead, duyệt báo giá, báo cáo, chuyển giao khách |
| `nhanvien` | Nhân viên kinh doanh (Nguyễn Minh Tuấn) | Lead của mình, gọi điện, chuyển đổi, pipeline, báo giá |
| `sales2` | Nhân viên kinh doanh (Lê Hoàng Yến) | Chứng minh hai sales không thấy khách của nhau |

Mật khẩu: giá trị `DEMO_PASSWORD` trong `.env`, hoặc bấm "Đăng nhập nhanh" (riêng `sales2` đăng nhập bằng mật khẩu).

---

## 2. Câu chuyện demo

> Mở đầu: "Anh/chị hình dung hôm qua công ty mình vừa tham gia hội chợ, thu được một xấp danh thiếp. Sáng nay em đi đúng một ngày của phòng kinh doanh để anh/chị thấy phần mềm giúp gì từ lúc có lead tới lúc chốt đơn."

| # | Vai trò | Thao tác | Điểm cần nhấn mạnh |
| :---: | :--- | :--- | :--- |
| 1 | Trưởng phòng | Đăng nhập `quanly`, mở Tổng quan: doanh số tháng so chỉ tiêu đội, pipeline gia quyền, lead chậm liên hệ, bảng xếp hạng sales | Mở máy là biết đội đang ở đâu so với chỉ tiêu, ai đang để lead nguội |
| 2 | Trưởng phòng | Lead → Nhập Excel, chọn `lead-hoi-cho.xlsx`; xem kết quả kiểm tra 17 hợp lệ, 2 trùng, 1 lỗi; bấm nhập | Không nhập trùng khách đã có, không phải ngồi lọc tay; 17 lead tự chia đều cho 6 sales |
| 3 | Nhân viên | Đổi vai trò sang Nhân viên trên thanh công cụ demo; mở Tổng quan cá nhân và chuông thông báo | Sales thấy ngay chỉ tiêu của mình (đã đạt 36,0%), lead mới được giao, lead đang chậm liên hệ |
| 4 | Nhân viên | Mở lead "Trần Văn Bình" (nhãn đỏ Chậm liên hệ), bấm Gọi trên điện thoại; ghi kết quả "Đã trao đổi", tạo việc tiếp theo Gặp mặt thứ Năm 09:00, nhắc trước 1 giờ | Gọi xong ghi lại trong 3 chạm; lead tự chuyển Đang liên hệ, nhãn đỏ mất; lịch hẹn có nhắc |
| 5 | Nhân viên | Thêm lead mới với SĐT `0987 654 321`; tìm khách hàng "Dược phẩm Minh Châu" | SĐT đã thuộc lead của Lê Hoàng Yến nên hệ thống chặn và nói rõ ai phụ trách; khách của đồng nghiệp không hiện trong tìm kiếm |
| 6 | Nhân viên | Chuyển đổi lead Trần Văn Bình: tạo khách hàng "Công ty CP Thép Việt Nhật", người liên hệ, cơ hội "Hệ thống chấm công — Thép Việt Nhật" 85.000.000 ₫ | Một bước có đủ khách hàng, người liên hệ, cơ hội; lịch sử cuộc gọi đi theo sang hồ sơ khách |
| 7 | Nhân viên | Mở Pipeline, mở cơ hội "Phần mềm điều phối xe — Vận tải Nam Long", lập báo giá 3 dòng: `SP001` phần mềm 180.000.000 ₫ chiết khấu 15%, `DV001` triển khai 60.000.000 ₫, `TB001` 4 máy chấm công 4.500.000 ₫; nút chuyển thành "Gửi duyệt" | Báo giá tự tính chiết khấu, thuế GTGT từng dòng; sales không tự giảm quá 10% |
| 8 | Trưởng phòng | Đổi sang Trưởng phòng, bấm chuông "Báo giá chờ duyệt", xem và bấm Duyệt | Duyệt ngay trên hệ thống, có lưu người duyệt; không cần chạy qua bàn ký giấy |
| 9 | Nhân viên | Đổi về Nhân viên, in PDF báo giá, đánh dấu Đã gửi; bấm "Khách đồng ý"; kéo thẻ vào vùng Thắng | Cơ hội tự nhảy giai đoạn theo báo giá, giá trị cập nhật 231.000.000 ₫; chốt xong doanh số tháng lên 82,2% chỉ tiêu |
| 10 | Trưởng phòng | Mở Báo cáo: doanh số theo sales so chỉ tiêu, phễu chuyển đổi so sánh nguồn Hội chợ với Website, tab Lý do thất bại | Biết nguồn lead nào ra tiền, sales nào chuyển đổi kém, mất khách vì giá hay vì đối thủ |
| 11 | Trưởng phòng | Khách hàng → Chuyển giao: chuyển toàn bộ khách của Lê Hoàng Yến (giả định nghỉ việc) sang Đỗ Quang Huy | Sales nghỉ việc không mang theo khách; cơ hội đang mở chuyển ngay, doanh số cũ của người nghỉ vẫn giữ để đối chiếu |
| 12 | Quản trị viên | Đổi sang Quản trị viên, mở Vai trò: cho thấy Nhân viên không có quyền xuất Excel; đổi màu thương hiệu | Phân quyền tới từng nút; sales không tải được danh sách khách mang ra ngoài |

> Kết: "Toàn bộ dữ liệu anh/chị vừa thấy là dữ liệu mẫu. Bản triển khai thật sẽ dùng các giai đoạn bán hàng, danh mục sản phẩm và mẫu báo giá của công ty mình."

---

## 3. Câu hỏi khách hay hỏi

| Câu hỏi | Trả lời gợi ý |
| :--- | :--- |
| Lead từ website, Facebook Ads có tự về CRM không? | Bản demo nhập tay và nhập Excel. Bản triển khai nối form website, Facebook Lead Ads qua webhook để lead về và tự chia cho sales. |
| Có gọi điện, ghi âm cuộc gọi ngay trên phần mềm không? | Demo dùng nút Gọi mở trình gọi của điện thoại, sales tự ghi kết quả. Bản triển khai có thể tích hợp tổng đài VoIP công ty đang dùng để bấm gọi và lưu ghi âm, cần khảo sát nhà cung cấp. |
| Có gửi email, Zalo, SMS hàng loạt chăm sóc khách không? | Demo chưa gửi thật, nút Gửi email chỉ giả lập. Bản triển khai có thể nối email marketing hoặc Zalo OA (tin ZNS cần đăng ký mẫu với Zalo). |
| Sales nghỉ việc có mang khách đi được không? | Sales chỉ thấy khách của mình, không có quyền xuất Excel. Khi nghỉ, trưởng phòng chuyển giao toàn bộ khách sang người khác bằng một thao tác. |
| Công ty có nhiều nhóm, chi nhánh thì phân quyền thế nào? | Demo có hai mức: thấy tất cả và chỉ thấy của mình. Bản triển khai thêm mức theo nhóm hoặc chi nhánh để trưởng nhóm thấy dữ liệu của nhóm. |
| Có quản lý hợp đồng, công nợ, xuất hóa đơn không? | Demo dừng ở cơ hội Thắng. Bản triển khai có thể thêm hợp đồng, đợt thanh toán, nối phần mềm kế toán hoặc hóa đơn điện tử công ty đang dùng. |
| Giai đoạn bán hàng của công ty tôi khác thì sao? | Demo dùng 5 giai đoạn phổ biến cho B2B. Bản triển khai cấu hình theo quy trình của công ty, kể cả xác suất thắng từng giai đoạn. |
| Dữ liệu khách cũ trong Excel có đưa vào được không? | Có. Lead nhập trực tiếp như vừa xem; danh sách khách hàng, người liên hệ cũ sẽ nhập một lần khi triển khai, có kiểm tra trùng. |
| Có app điện thoại cho sales đi thị trường không? | Giao diện web dùng tốt trên điện thoại: xem khách, gọi, ghi hoạt động. App riêng (check-in vị trí, chụp ảnh) cần khảo sát thêm. |

---

## 4. Sau buổi demo

- [ ] Ghi lại yêu cầu khách nêu thêm vào `01-TONG-QUAN-NGHIEP-VU.md`
- [ ] Ghi lại số sales, số nhóm, các nguồn lead chính, các giai đoạn bán hàng và mẫu báo giá khách đang dùng
- [ ] Gửi link demo và tài khoản `nhanvien`, `quanly` cho khách tự trải nghiệm (nếu đồng ý)
