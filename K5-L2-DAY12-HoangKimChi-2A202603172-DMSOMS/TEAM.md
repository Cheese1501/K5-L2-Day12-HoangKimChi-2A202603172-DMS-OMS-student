# Thông tin làm bài và review chéo

Họ tên: Hoàng Kim Chi. MSSV: 2A202603172. Bộ ảnh: G07. Hồ sơ nộp cá nhân.

| Họ tên | MSSV | Đã gán đủ 22 ảnh | Review với | Tình trạng xử lý bất đồng |
|---|---|---|---|---|
| Hoàng Kim Chi | 2A202603172 | Có, export chứa đủ 22 ảnh | Lê Quang Khải | 140 khác biệt ở bản trước khi sửa; Khải đã sửa theo bài của tôi, hai bên không còn bất đồng theo xác nhận của tôi |

## Phần tôi thực hiện

Tôi có bản gán nhãn 22 ảnh của mình và đối chiếu với file project export của Khải. Project của Khải chứa một task 22 ảnh trống và một task 22 ảnh có nhãn. Tôi dùng task 45 có nhãn để so sánh; việc tách chỉ thay tên ảnh về tên gốc và không thay nhãn, thuộc tính hay tọa độ.

Tôi xác nhận Khải đã sửa theo bài của tôi và hai bên không còn bất đồng. File export của Khải đang lưu là bản trước khi sửa; tôi chưa có bản mới để chạy diff xác minh 0 khác biệt. Các verdict trong disagreement_log.csv được giữ làm lịch sử phân tích, kèm cột ghi nhận kết quả thống nhất sau đó.

## Kết quả và việc cần sửa

Lượt QC trước ghi nhận 22 box trên 14 ảnh OMS cần sửa ghế. Tôi đã có export_final_v2.zip sau sửa; kiểm lại cho thấy 18 box đã được sửa, còn 4 box person tại S02_rgb, S02_ir, S08_rgb và S08_ir cần đổi rear_right thành rear_left. seat_review.csv hiện ghi 4 box này. Tôi không sửa XML bằng tay.

Trong phần có nhãn của Khải, checker báo 0 lỗi cấu trúc và 15 cảnh báo. Có 12 cảnh báo về trạng thái mắt/cờ điểm mặt và 3 cảnh báo object mang seat=driver trong ảnh OMS. Tôi ghi các ca này trong BAO_CAO_QC.md để Khải kiểm lại trên CVAT.

Các ca cần hỏi coach gồm cách áp dụng occupied=yes trong schema mới cho S01/S08 và cách đo ngưỡng face13 trên ảnh xoay. Tôi mới lập câu hỏi, chưa có phản hồi của coach.
