# Báo cáo tự kiểm và đối chiếu với Lê Quang Khải

Tôi là Hoàng Kim Chi, MSSV 2A202603172, thực hiện bộ ảnh G07. Tôi đối chiếu bài của mình với Lê Quang Khải, MSSV 2A202603237, sau đó sửa và xuất lại dữ liệu. Bản sau sửa được lưu trong export_final_v2.zip, xuất lúc 17:24:44 ngày 21/11/2026 theo giờ Việt Nam.

## Kết quả sau sửa

| Nội dung kiểm tra | Kết quả |
|---|---|
| Số ảnh | Đủ 22 ảnh: 16 ảnh OMS và 6 ảnh DMS. |
| Tag frame_meta | Cả 22 ảnh có đúng một tag. |
| Modality | Ảnh _rgb dùng rgb; ảnh _ir và D## dùng ir. |
| Light trên SVIRO | Tất cả dùng unknown. |
| Kiểm bằng cabin_check | 0 lỗi, 0 cảnh báo theo cấu hình mới có occupied=yes. |
| Kiểm bổ sung ghế theo xe | Đã sửa và kiểm lại đủ 22 box từng bị đảo ghế; không còn box bị phép rà soát này đánh dấu. |
| Cấu trúc ZIP | Chỉ chứa annotations.xml, không kèm ảnh. |

Tôi dùng check_final_v2.txt làm kết quả kiểm tra sau cùng. export_final.zip và check_final.txt được giữ làm bằng chứng trước rework, không phải bản cuối sau sửa.

## Những lỗi tôi đã sửa

Lỗi lặp lại nhiều nhất là gọi trái/phải ghế theo ảnh thay vì theo xe. Tôi đã sửa 22 box trên 14 ảnh OMS. Trong lượt sửa đầu tôi đã xử lý 18 box; lượt cuối xử lý thêm 4 box person tại S02_rgb, S02_ir, S08_rgb và S08_ir từ rear_right thành rear_left.

| Cảnh, áp dụng cho cả RGB và IR | Nội dung đã sửa |
|---|---|
| S01 | child_seat bên trái ảnh dùng rear_right. |
| S02 | person bên phải ảnh dùng rear_left; infant_seat bên trái ảnh dùng rear_right. |
| S04 | infant_seat bên trái ảnh dùng rear_right; child_seat bên phải ảnh dùng rear_left. |
| S05 | Box infant_seat bên trái ảnh dùng rear_right. |
| S06 | child_seat bên phải ảnh dùng rear_left; infant_seat bên trái ảnh dùng rear_right. |
| S07 | object bên trái ảnh dùng rear_right. |
| S08 | person bên phải ảnh dùng rear_left; child_seat bên trái ảnh dùng rear_right. |

Các box ở giữa ảnh giữ rear_middle. Tôi đối chiếu từng box theo tên ảnh, loại nhãn và tọa độ, rồi kiểm thuộc tính seat trong bản xuất mới. seat_review.csv lưu 22 mục với trạng thái đã sửa và kiểm lại. Tôi sửa trên CVAT và xuất lại, không sửa nhãn trực tiếp trong XML.

Tôi cũng đã xử lý modality của S02_rgb/S03_rgb, light trên các ảnh SVIRO và sự nhất quán giữa trạng thái mắt với cờ điểm ở D06. Các vấn đề này không còn xuất hiện trong kết quả kiểm sau cùng.

## QC chéo với Khải

File project export ban đầu của Khải có 44 ảnh thuộc hai task: task 44 gồm 22 ảnh chưa có nhãn và task 45 gồm 22 ảnh có nhãn. Tôi dùng task 45 để đối chiếu, bỏ hậu tố _1 do trùng tên trong project export nhằm ghép đúng 22 tên ảnh. Tôi không đổi nhãn, tọa độ hoặc thuộc tính của Khải trong bản dùng để so sánh.

Lần diff trước sửa ở ngưỡng IoU 0,5 ghi nhận 140 khác biệt. Tôi đã ghi quy tắc và hướng xử lý trong disagreement_log.csv. Trong đó có 14 dòng về seat đủ căn cứ kết luận theo quy ước và tọa độ; 126 dòng còn lại được ghi ambiguous ở thời điểm phân tích XML. Tôi giữ các verdict này làm lịch sử, cùng cột followup ghi nhận kết quả trao đổi sau đó.

Các vấn đề tôi đã tổng hợp cho Khải ở bản cũ gồm 12 cảnh báo mắt/cờ điểm trên D01–D06, 3 cảnh báo object.seat=driver trên ảnh OMS, khác lớp ghế giữa cặp RGB/IR, hai box phone tại D05, khác kích thước box face và khác quy ước tay trái/phải. Báo cáo check_peer_khai.txt áp dụng cho bản cũ đó.

Sau trao đổi, Khải đã sửa theo bài của tôi và hai bên không còn bất đồng theo xác nhận của tôi. Tôi chưa nhận export mới của Khải để chạy lại diff, nên không ghi 0 khác biệt như một kết quả đo mới. Đồng thuận giữa hai người không thay thế việc áp dụng guideline; vì vậy tôi vẫn sửa và kiểm riêng lỗi ghế trong bài mình.

## Cấu hình nhãn và nhật ký IR

Cấu hình cập nhật cho phép child_seat.occupied nhận unknown, no và yes. Tôi đã đồng bộ labels_cabin_v1.json và cabin_schema.py theo cấu hình này. Bốn giá trị yes ở S01/S08 hợp lệ về schema; tôi không đổi chúng chỉ để phù hợp checker cũ.

ir_log.csv có 4 dòng tại S02_ir, S07_ir, D04 và D06, tách hai nguồn IR_sim và real_IR. Tôi ghi nhận từ dữ liệu nhãn: trạng thái mắt chưa xác định, loại object nhất quán giữa RGB/IR và cờ điểm mắt trên NIR. Các dòng này dựa vào export, không khẳng định đã quan sát trực tiếp nguyên nhân như chói kính hoặc phản xạ. Tôi không suy hiện tượng NIR thật từ ảnh IR mô phỏng.

## Hồ sơ và phạm vi xác nhận

Tôi đã chuẩn bị đủ 11 file bắt buộc theo SUBMISSION.md, kèm export_final_v2.zip và check_final_v2.txt sau sửa. class_list_v0.md có 8 mục DMS, 8 mục OMS và hai câu hỏi leo thang gắn với khung ví dụ. Các file QC bổ sung gồm báo cáo này, seat_review.csv và check_peer_khai.txt.

check_cpA.txt và check_cpB.txt là các lần kiểm bù đúng phạm vi từ export hoàn chỉnh, không phải log gốc đã lưu tại thời điểm checkpoint trong lớp. Tôi giữ ghi chú này để phân biệt kết quả kiểm hiện tại với bằng chứng tiến trình.

Tôi xác nhận các lỗi cấu trúc, metadata và đảo ghế đã được phát hiện trong quá trình QC đều đã xử lý. Phép kiểm XML không thay thế việc đánh giá trực quan độ sát box, đúng lớp vật thể, vị trí điểm mặt hoặc nguyên nhân mất thông tin trên ảnh IR.
