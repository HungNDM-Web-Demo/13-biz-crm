# Tổng quan nghiệp vụ — CRM Quản lý Khách hàng

> Mô tả demo này cho ai, giải quyết vấn đề gì. Cập nhật khi phạm vi thay đổi.

---

## 1. Thông tin chung

| Mục | Nội dung |
| :--- | :--- |
| Tên demo | CRM Quản lý Khách hàng |
| Mã dự án | `biz-crm` |
| Ngành / loại khách hàng | Doanh nghiệp B2B, B2C có đội kinh doanh |
| Loại sản phẩm | Web quản lý |
| Ý tưởng gốc | 5.1 · CRM quản lý khách hàng (`ZDOCS/02-KE-HOACH/DANH-SACH-Y-TUONG.md`) |
| Người phụ trách | |
| Trạng thái | Có tài liệu nghiệp vụ (SRS, use case, kịch bản demo, kịch bản kiểm thử), chưa code |

## 2. Vấn đề của khách hàng

- Khách hàng nằm trong danh bạ từng sales, nghỉ việc là mất
- Không biết lead đang ở bước nào
- Quên chăm sóc lại khách
- Quản lý không đo được hiệu quả của từng sales

## 3. Giải pháp demo thể hiện

- Lead, khách hàng, người liên hệ
- Pipeline cơ hội dạng Kanban kéo thả
- Lịch hẹn, công việc chăm sóc, nhắc việc
- Lịch sử tương tác, báo giá
- Báo cáo doanh số, tỉ lệ chuyển đổi theo sales

## 4. Vai trò người dùng

| Vai trò | Mã | Dùng trên | Làm được gì |
| :--- | :--- | :--- | :--- |
| Quản trị viên | ADMIN | Desktop | Toàn quyền |
| Trưởng phòng kinh doanh | MANAGER | Desktop | Phân lead, xem toàn đội, báo cáo |
| Nhân viên kinh doanh | STAFF | Desktop, điện thoại | Lead và cơ hội của mình |

## 5. Phân hệ và chức năng

| Area | Chức năng | Màn hình (route FE dự kiến) | Ưu tiên |
| :--- | :--- | :--- | :---: |
| `System` | Đăng nhập, tài khoản, vai trò, cài đặt | Có sẵn | — |
| `Crm` | Tổng quan kinh doanh | `/dashboard` | P1 |
| `Crm` | Lead | `/leads` | P1 |
| `Crm` | Khách hàng, người liên hệ | `/customers` | P1 |
| `Crm` | Hồ sơ khách hàng | `/customers/[id]` | P1 |
| `Crm` | Pipeline cơ hội (Kanban) | `/pipeline` | P1 |
| `Crm` | Hoạt động chăm sóc, nhắc việc | `/activities` | P1 |
| `Crm` | Báo giá | `/quotes` | P2 |
| `Crm` | Soạn, in báo giá | `/quotes/[id]` | P2 |
| `Crm` | Nhập lead từ Excel | `/leads/import` | P2 |
| `Crm` | Báo cáo doanh số, chuyển đổi | `/reports` | P1 |

P1 = bắt buộc có để demo · P2 = nên có · P3 = làm thêm nếu còn thời gian.

## 6. Luồng nghiệp vụ chính

```mermaid
flowchart LR
    S0["Lead mới"] --> S1["Phân cho sales"] --> S2["Liên hệ, tư vấn"] --> S3["Tạo cơ hội"] --> S4["Gửi báo giá"] --> S5["Chốt hoặc thất bại"]
```

## 7. Phạm vi

**Có trong demo:**

- Lead, khách hàng, pipeline, chăm sóc, báo cáo; sales chỉ thấy khách của mình

**Không làm (nói rõ với khách nếu được hỏi):**

- Gửi email hàng loạt thật
- Tổng đài gọi điện
- Các kết nối bên ngoài khác chỉ giả lập

## 8. Dữ liệu mẫu cần chuẩn bị

| Dữ liệu | Số lượng | Ghi chú |
| :--- | :---: | :--- |
| Lead | 300 | Nhiều nguồn |
| Khách hàng | 150 |  |
| Cơ hội | 120 | Đủ các giai đoạn |
| Hoạt động | ~1.000 | 90 ngày |
| Nhân viên kinh doanh | 6 |  |
