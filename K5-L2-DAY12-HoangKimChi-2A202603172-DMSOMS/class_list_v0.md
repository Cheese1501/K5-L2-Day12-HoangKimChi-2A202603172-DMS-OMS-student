# Danh sách lớp v0 của Hoàng Kim Chi

MSSV: 2A202603172. Bộ ảnh: G07.

Tôi chia danh sách thành DMS để theo dõi người lái và OMS để nhận biết người, ghế trẻ em, đồ vật trong cabin. Các giá trị dưới đây bám theo Guideline Cabin v1.0, có cập nhật occupied=yes theo cấu hình mới ngày 21/11/2026. Khả năng quan sát trên IR còn phụ thuộc góc nhìn, độ phân giải và mức che khuất.

## DMS

| # | Lớp hoặc thuộc tính | Giá trị | Để làm gì | Quan sát trên IR |
|---|---|---|---|---|
| 1 | person.seat | driver | Xác định người lái để gắn kết quả theo dõi đúng người. | Dựa vào vị trí và hình dạng; không dựa vào màu áo. |
| 2 | face | Hộp mặt | Xác định vùng mặt để xử lý mắt và các điểm mặt. | Có thể thấy nếu mặt đủ rõ và không bị che gần hết. |
| 3 | face.eye_left và eye_right | open, closed, unknown | Mô tả từng mắt ở khung hiện tại; cần chuỗi thời gian nếu muốn đánh giá nhắm mắt kéo dài. | Không rõ mí do kính phản xạ hoặc che khuất thì unknown. |
| 4 | face.glasses | none, clear, sun, unknown | Nhận biết kính để xử lý các trường hợp khó quan sát mắt. | Không suy loại kính chỉ từ độ tối của tròng trên NIR. |
| 5 | face.gaze_zone | road, mirror_l, mirror_r, mirror_c, cluster, center, lap, other | Phân loại vùng nhìn để hỗ trợ đánh giá sự chú ý của người lái. | Cần thấy mắt và hướng đầu; không chắc thì other và ghi lý do. |
| 6 | face13 | 13 điểm mặt | Xác định cấu trúc mặt để hỗ trợ xử lý hướng đầu và vùng mắt, miệng. | Chỉ gán trên D## khi đạt ngưỡng kích thước; đánh cờ điểm bị che. |
| 7 | hand | whose, side, on_wheel | Phân biệt tay người lái với hành khách và ghi trạng thái chạm vô lăng. | Có thể nhận theo hình dạng, nhưng tay bị che có thể không xác định được trạng thái. |
| 8 | phone.state | held, mounted, lap | Nhận biết điện thoại và vị trí sử dụng để hỗ trợ phân tích hành vi. | Phải thấy cạnh, góc hoặc màn hình; không suy từ tư thế tay. |

## OMS

| # | Lớp hoặc thuộc tính | Giá trị | Để làm gì | Quan sát trên IR |
|---|---|---|---|---|
| 1 | person | Hộp người nhìn thấy | Nhận biết người đang có mặt trên hàng ghế sau. | Dựa vào cơ thể nhìn thấy; có thể khó khi bị vật khác che. |
| 2 | person.seat | rear_left, rear_middle, rear_right | Gắn người với đúng vị trí trong xe để tổng hợp trạng thái từng ghế. | Xác định bằng vị trí; trái ảnh OMS là rear_right. |
| 3 | person.seatbelt | not_worn, unknown trong SVIRO | Ghi trạng thái dây khi đủ bằng chứng; bộ ảnh này không cung cấp ví dụ thắt dây để học lớp worn_correct. | Dây không rõ thì unknown, không suy từ ảnh RGB cùng cảnh. |
| 4 | face | Hộp mặt người lớn | Bổ sung vị trí mặt của người ngồi sau theo phạm vi bài lab. | Chỉ vẽ khi nhìn thấy mặt; S## không gán face13. |
| 5 | child_seat | Hộp ghế; occupied=unknown, no, yes theo cấu hình cập nhật | Nhận biết ghế trẻ em và trạng thái có người ngồi theo hướng dẫn mới. | Nhận theo hình dạng; không rõ bên trong thì occupied=unknown. |
| 6 | infant_seat | Hộp nôi; facing=rear hoặc front | Nhận biết nôi xách và hướng đặt trên ghế. | Cần nhìn rõ cấu trúc và hướng; không dựa vào màu. |
| 7 | object | Hộp vật; type=bag, box, cup, other | Nhận biết đồ vật chiếm chỗ để tránh tính nhầm thành người hoặc ghế trẻ em. | Dựa vào hình dạng; không chắc loại thì other. |
| 8 | seat của child_seat, infant_seat, object | rear_left, rear_middle, rear_right | Tổng hợp loại vật thể theo từng ghế; vật trong ghế trẻ em vẫn có box riêng cùng seat. | Vị trí ghế vẫn xác định theo xe, không theo bên trái/phải của ảnh. |

Ghế trống không phải một box trong schema này. Tôi ghi số ghế trống ở ghi chú. Chỉ nhận ra ghế trẻ em chưa đủ để kết luận có trẻ bị bỏ quên trong xe.

## Hai câu hỏi leo thang

1. Cấu hình mới chấp nhận occupied=yes, trong khi guideline cũ chỉ mô tả ghế trẻ em trống. Với ghế có người ngồi, cần gán đồng thời child_seat và person hay chỉ child_seat với occupied=yes? Quy tắc này có áp dụng cho infant_seat không?
   - Khung ví dụ: S01_rgb/S01_ir và S08_rgb/S08_ir. Bản của tôi có child_seat với occupied=yes; bản của Khải có các box người ở vùng ghế tương ứng và khác lớp ghế.
   - Cách tôi xử lý: giữ nguyên export để bảo toàn bằng chứng, công nhận yes là giá trị hợp lệ của schema mới nhưng chưa kết luận lớp vật thể hoặc số box đúng. Tôi ghi ca này vào review để hỏi coach trước khi sửa theo diễn giải mới.
2. Với ảnh D## xoay 90°, ngưỡng mặt rộng 60 px được tính theo chiều ngang box trong ảnh hay chiều rộng giải phẫu của mặt? Cần quy định thế nào khi hai kích thước nằm khác phía ngưỡng?
   - Khung ví dụ: D01.png có box face rộng 337,71 px, cao 324,29 px trong bản export. Khung này đạt 60 px theo cả hai chiều, nhưng minh họa được việc phải xác định rõ trục đo khi ảnh xoay.
   - Cách tôi xử lý: giữ face13 trong D01 vì đạt ngưỡng theo cả hai cách. Với trường hợp sát ngưỡng, tôi sẽ hỏi coach thay vì tự đổi cách đo.

Tôi chưa có slide 54 trong bộ tài liệu đã nhận; cần đối chiếu slide trước khi chốt danh sách theo đầy đủ nội dung bài giảng.
