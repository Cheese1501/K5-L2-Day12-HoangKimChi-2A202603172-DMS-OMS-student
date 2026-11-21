# Tự đánh giá của Hoàng Kim Chi

MSSV: 2A202603172. Bộ ảnh: G07.

## Rà soát bốn ảnh

Tôi kiểm lại thuộc tính, tọa độ box và cờ điểm trong export sau sửa.

| Ảnh | Đối tượng | Loại lỗi trước sửa | Kết quả sau sửa |
|---|---|---|---|
| S01_ir.png | child_seat bên trái ảnh | seat_flip | Đã đổi rear_left thành rear_right và kiểm lại trong bản xuất mới. |
| S02_rgb.png | person bên phải và infant_seat bên trái ảnh | seat_flip | person dùng rear_left; infant_seat dùng rear_right. Cả hai đã được kiểm lại. |
| D04.png | face và 8 điểm mắt | Kiểm sự nhất quán của cờ | Hai mắt unknown; 8 điểm mắt có occluded=1. Không còn cảnh báo không nhất quán. |
| D06.png | face và điểm mắt phải | flag | Mắt trái open với điểm không che; mắt phải unknown, 4 điểm right_eye có outside=1. Không còn cảnh báo không nhất quán. |

## Lỗi tôi lặp lại nhiều nhất

Tôi đã gọi rear_left và rear_right theo phía của ảnh. Lỗi này xuất hiện ở 22 box trên 14 ảnh OMS và đã được sửa hết trong export_final_v2.zip. Các box rear_middle được giữ nguyên.

Tôi khắc phục bằng cách đọc lại quy ước trước khi chọn seat: camera nhìn về sau xe nên trái ảnh là rear_right, giữa ảnh là rear_middle, phải ảnh là rear_left. Tôi kiểm cả hai ảnh RGB/IR của mỗi cảnh và đối chiếu từng box theo tọa độ, không chỉ nhìn kết quả 0 lỗi của checker.

## Điều tôi học từ review chéo

Ở S02_rgb.png, box person của hai bản khớp IoU khoảng 0,97. Bản ban đầu của tôi ghi rear_right, bản của Khải ghi rear_left. Theo tọa độ ở bên phải ảnh và GUIDE §4.4-3, rear_left phù hợp quy ước. Tôi đã sửa thuộc tính này trong bản cuối.

Khải đã sửa theo bài của tôi và tôi xác nhận hai bên không còn bất đồng. Các dòng diff cũ vẫn được giữ để thể hiện quá trình review, không được coi là số khác biệt hiện tại. Tôi chưa có export mới của Khải để chạy diff xác minh sau sửa.

## Kết quả cuối cùng

export_final_v2.zip có đủ 22 ảnh và 22 tag frame_meta. check_final_v2.txt báo 0 lỗi, 0 cảnh báo; kiểm bổ sung không còn box bị đánh dấu đảo ghế. Nhật ký IR có 4 dòng dựa trên nhãn trong export. Kết quả này xác nhận các điều kiện đã kiểm, chưa thay thế đánh giá trực quan box, lớp vật thể và điểm mặt trên ảnh.
