# Báo cáo tự kiểm và đối chiếu với Lê Quang Khải

Tôi là Hoàng Kim Chi, MSSV 2A202603172, làm bộ ảnh G07. Tôi đối chiếu export của mình với file export do Khải cung cấp để kiểm cấu trúc, thuộc tính, vị trí box và các bất đồng cần xử lý.

## Dữ liệu dùng để kiểm tra

| Nguồn | Phạm vi | Cách sử dụng |
|---|---|---|
| Bản của tôi, job 27, thời điểm xuất ghi trong tên file là 08:23:34 UTC ngày 21/11/2026 | 22 ảnh | Lưu nguyên byte dưới tên export_final.zip; thời gian tương ứng 15:23:34 giờ Việt Nam. |
| Project export của Khải, project 6, thời điểm 08:20:47 UTC ngày 21/11/2026 | 44 ảnh thuộc task 44 và task 45 | Task 44 có 22 ảnh chưa có nhãn; task 45 có 22 ảnh có nhãn. Tôi dùng phần task 45 để đối chiếu. |

Project export thêm hậu tố _1 vào tên ảnh của task 45 vì trùng tên với task 44. Tôi chuẩn hóa D01_1.png thành D01.png và S01_ir_1.png thành S01_ir.png trong bản chỉ phục vụ so sánh. Cả 22 tên và kích thước ảnh đều khớp bản của tôi. Không thay đổi nhãn, tọa độ hoặc thuộc tính của Khải. Tên file và kích thước không phải bằng chứng tuyệt đối rằng nội dung ảnh giống nhau; cần Khải xác nhận đúng bộ G07 và task đã hoàn tất rà soát.

Tôi giữ nguyên file nguồn trên máy, không đưa bản export của Khải vào bài nộp dưới tên của tôi. Sau lượt đối chiếu này, tôi xác nhận Khải đã sửa theo bài của tôi và hai bên không còn bất đồng. Tôi chưa có export mới của Khải để chạy lại diff; vì vậy số 140 bên dưới là lịch sử của bản trước khi sửa, không phải số bất đồng hiện tại.

## Kết quả kiểm cấu trúc

| Phép kiểm | Kết quả |
|---|---|
| ZIP của tôi | Chỉ có annotations.xml, không kèm ảnh. |
| Checker trên 22 ảnh của tôi | 0 lỗi, 0 cảnh báo theo schema đã bổ sung occupied=yes. |
| Modality và light của tôi | Các ảnh _rgb dùng rgb; _ir và D## dùng ir; light của S## là unknown. |
| Checker trên project gốc của Khải | 23 lỗi và 12 cảnh báo: 1 lỗi số ảnh do có 44 ảnh, 22 lỗi thiếu frame_meta ở task trống. Kết quả này không được dùng để chấm riêng task có nhãn. |
| Checker trên phần 22 ảnh đã có nhãn của Khải | 0 lỗi, 15 cảnh báo sau khi chuẩn hóa tên ảnh. |
| Diff đúng 22 ảnh chung | 140 dòng khác biệt ở ngưỡng IoU 0,5. |

Trong disagreement_log.csv, tôi điền verdict, guideline_rule và action cho cả 140 dòng. Có 14 dòng về seat đủ căn cứ kết luận peer theo quy ước và tọa độ; 126 dòng còn lại mang verdict=ambiguous để kiểm bằng ảnh hoặc hỏi thêm. Các verdict này mô tả lần phân tích file cũ. Sau đó Khải sửa theo bài của tôi và tôi xác nhận không còn bất đồng; cột followup_status và followup_basis ghi nhận thông tin này. Tôi không đổi số liệu diff cũ thành 0 khi chưa có export mới để đối chiếu.

## Lỗi trong bài của tôi cần sửa

Tôi phát hiện 22 box trên 14 ảnh bị đảo rear_left và rear_right so với vị trí trái/phải ảnh. Danh sách đầy đủ có trong seat_review.csv, gồm tên ảnh, loại box và tọa độ để tìm đúng đối tượng trên CVAT. Các giá trị đề xuất là mục tiêu kiểm/sửa, không phải bản XML đã chỉnh.

| Cảnh, áp dụng cho cả RGB và IR | Thuộc tính cần kiểm/sửa theo quy ước ghế |
|---|---|
| S01 | child_seat ở trái ảnh: rear_left → rear_right. |
| S02 | person ở phải ảnh: rear_right → rear_left; infant_seat ở trái ảnh: rear_left → rear_right. |
| S04 | infant_seat ở trái ảnh: rear_left → rear_right; child_seat ở phải ảnh: rear_right → rear_left. |
| S05 | Box đang mang infant_seat ở trái ảnh: rear_left → rear_right; lớp infant_seat/child_seat còn bất đồng. |
| S06 | child_seat ở phải ảnh: rear_right → rear_left; infant_seat ở trái ảnh: rear_left → rear_right. |
| S07 | object ở trái ảnh: rear_left → rear_right. |
| S08 | person ở phải ảnh: rear_right → rear_left; box đang mang child_seat ở trái ảnh: rear_left → rear_right. |

Quy tắc áp dụng là GUIDE §4.4-3: camera OMS nhìn về phía sau xe, nên trái ảnh là rear_right. Box giữa ảnh ở S01/S03/S07 không nằm trong danh sách cần đảo. Tôi không tự đổi lớp vật thể chỉ dựa trên vị trí box.

## Nhận xét cho bài của Khải

1. **Mắt và cờ điểm trên D01–D06:** cả hai mắt đều unknown, nhưng các điểm mắt không có cờ occluded/outside, tạo 12 cảnh báo. Tôi đề nghị kiểm từng mắt trên ảnh. Nếu bị che thì chọn cờ phù hợp; nếu nhìn rõ thì kiểm lại open/closed. Không đổi toàn bộ unknown thành open chỉ để hết cảnh báo.
2. **Seat=driver trên ảnh OMS:** S02_rgb có một object và S03_ir có hai object mang driver. Ảnh S## thuộc hàng ghế sau, nên cần kiểm lại seat và xem vật đó có thuộc phạm vi cần gán không. Các cảnh báo này áp dụng cho task có nhãn, không phải task trống.
3. **Khác lớp giữa cặp RGB/IR:** tại S04/S06, một số box tương ứng là child_seat trên RGB nhưng infant_seat trên IR. Tại S05, box ghế cũng khác lớp giữa hai modality. Đây là dấu hiệu cần rà soát tính nhất quán, chưa đủ để chỉ định lớp đúng nếu chưa nhìn ảnh.
4. **Hai box phone ở D05:** bản của tôi không có phone, bản của Khải có hai box. Tôi đề nghị kiểm cạnh/góc/màn hình điện thoại theo §4.7; không suy vật đang cầm là phone từ tư thế tay.
5. **Box face khác kích thước:** một số box không ghép được ở ngưỡng IoU 0,5. Cần rà trán–cằm, tai–tai và phần che khuất; không coi IoU thấp là bằng chứng bên nào chắc chắn sai.
6. **Tay trái/phải:** hai bản có nhiều giá trị side ngược nhau dù box tay khớp khá cao. Cần xác định theo cơ thể người lái trên ảnh xoay, không theo vị trí trái/phải của ảnh.

## Diễn giải thay đổi cấu hình

Cấu hình mới bổ sung child_seat.occupied=yes, giữ unknown và no. Tôi đã đồng bộ JSON và cabin_schema.py theo cấu hình này. Vì vậy bốn dòng yes ở S01/S08 không còn là lỗi giá trị. Điều này chưa xác nhận yes là cách diễn giải đúng cho mọi vật nằm trong ghế; tôi cần coach làm rõ trường hợp ghế chứa người và ghế chứa đồ vật.

## Giới hạn và bước tiếp theo

Tôi mới thực hiện QC từ XML, chưa xác nhận trực quan độ sát box, nhầm lớp ghế, che khuất điểm mặt, loại kính hoặc vùng nhìn. Tôi đã ghi 4 dòng ir_log từ nhãn trong export tại S02_ir, S07_ir, D04 và D06, tách IR_sim với real_IR. Các dòng này ghi nhận dữ liệu nhãn, không chứng minh nguyên nhân quang học hay mức mất thông tin trên ảnh; tôi cần mở ảnh để xác nhận trước khi coi là nhật ký quan sát trực tiếp. Tôi xác nhận đã thống nhất với Khải theo hướng Khải sửa theo bài của tôi; chưa có export mới để xác minh bằng diff.

Tôi sẽ sửa các seat trên CVAT, kiểm các ca ambiguous ưu tiên, xuất export_final_v2.zip và chạy lại checker. Tôi không sửa XML bằng tay, không tạo bản v2 khi chưa sửa nhãn, và không dùng kết quả 0 lỗi cấu trúc để khẳng định bài hoàn toàn đúng.
