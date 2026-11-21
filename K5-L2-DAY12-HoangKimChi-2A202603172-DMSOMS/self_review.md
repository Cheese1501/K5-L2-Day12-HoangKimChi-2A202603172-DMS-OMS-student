# Tự đánh giá của Hoàng Kim Chi

MSSV: 2A202603172. Bộ ảnh: G07.

## Rà soát bốn ảnh từ dữ liệu gán nhãn

Tôi rà thuộc tính, tọa độ box và cờ điểm trong export. Việc này chưa thay thế lượt mở lại ảnh trên CVAT để xem độ sát box và điểm mặt.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Tình trạng |
|---|---|---|---|---|
| S01_ir.png | child_seat ở trái ảnh | seat_flip | Box x=116,69–373,30 đang mang rear_left; theo quy ước camera nhìn về sau cần kiểm/sửa thành rear_right. | Chưa sửa trong CVAT |
| S02_rgb.png | person ở phải ảnh và infant_seat ở trái ảnh | seat_flip | Tôi đã gọi ghế theo bên ảnh. Người cần kiểm/sửa sang rear_left; nôi sang rear_right. | Chưa sửa trong CVAT |
| D04.png | face và 8 điểm mắt | ok về sự nhất quán của cờ | Hai mắt đều unknown; 8 điểm mắt có occluded=1. Bản của Khải cũng để mắt unknown nhưng không bật cờ điểm. | Cần xác nhận bằng ảnh, không tự đổi unknown thành open |
| D06.png | face và mắt phải | flag | Bản xuất đầu có cảnh báo mắt/cờ không nhất quán. Bản hiện tại để mắt trái open và không che; mắt phải unknown với 4 điểm outside=1. | Đã hết cảnh báo; cần kiểm vị trí điểm trên ảnh |

## Lỗi tôi lặp lại nhiều nhất

Lỗi lặp lại là đảo rear_left và rear_right. Tọa độ export cho thấy lỗi này xuất hiện ở 22 box thuộc 14 ảnh OMS; các box rear_middle không nằm trong danh sách cần đảo.

Tôi đã dùng hướng trái/phải của ảnh khi chọn seat, thay vì quy ước theo người ngồi trong xe nhìn về phía trước. Lần sau, trước khi chọn thuộc tính, tôi sẽ kiểm ba vị trí: trái ảnh là rear_right, giữa ảnh là rear_middle, phải ảnh là rear_left. Sau đó tôi soát lại từng cặp RGB/IR.

## Điều tôi học từ đối chiếu với Khải

Ở dòng S02_rgb.png, person.seat, box của hai bản khớp IoU khoảng 0,97. Tôi ghi rear_right, Khải ghi rear_left. Box nằm rõ bên phải ảnh, nên theo GUIDE §4.4-3, giá trị seat của Khải phù hợp quy ước. Kết luận này không đồng nghĩa mọi thuộc tính hoặc lớp vật thể của Khải đều đúng.

Tôi cũng nhận ra checker báo 0 lỗi chỉ có nghĩa dữ liệu vượt các điều kiện mà script đang kiểm. Script hiện chưa tự phát hiện đầy đủ đảo ghế theo vị trí, gán nhầm lớp hoặc thiếu vật thể.

## Kế hoạch sửa và kiểm lại

Tôi cần sửa các seat trong seat_review.csv trên CVAT, kiểm riêng các ca khác lớp ghế và điện thoại, rồi xuất export_final_v2.zip và chạy lại checker. Khải đã sửa theo bài của tôi và tôi xác nhận hai bên không còn bất đồng. Các nhận xét ambiguous trong CSV là kết quả phân tích bản cũ, không phải tình trạng trao đổi hiện tại. Tôi chưa có export mới của Khải để kiểm lại mức khớp bằng diff. Việc đồng thuận không thay thế kiểm đúng theo guideline, đặc biệt các lỗi seat nêu trên.
