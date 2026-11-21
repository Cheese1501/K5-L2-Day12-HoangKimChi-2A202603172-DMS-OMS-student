# Báo cáo thực hành gán nhãn cabin DMS và OMS

**Họ và tên:** Hoàng Kim Chi

**Mã số sinh viên:** 2A202603172

**Bộ dữ liệu:** G07

**Người review chéo:** Lê Quang Khải — MSSV 2A202603237

**Ngày thực hiện:** 21/11/2026

## 1 Mục tiêu và phạm vi thực hiện

Tôi thực hiện gán nhãn 22 khung hình cabin theo Guideline Cabin v1.0 và cấu hình nhãn cập nhật của lớp. Phần OMS gồm 8 cặp RGB và IR mô phỏng từ S01 đến S08, tổng cộng 16 ảnh. Phần DMS gồm 6 ảnh NIR thật từ D01 đến D06.

Mục tiêu của tôi là gán đúng đối tượng và thuộc tính, phân biệt vị trí ghế theo xe với trái/phải theo người, đồng thời sử dụng unknown khi không đủ thông tin. Tôi thực hiện tự kiểm, đối chiếu với bạn cùng bộ ảnh và sửa các lỗi đã phát hiện trước khi xuất bản cuối.

## 2 Nội dung thực hiện

Trên mỗi ảnh, tôi gán một tag frame_meta để ghi modality, light và camera_blocked. Ảnh RGB dùng modality=rgb; ảnh IR mô phỏng và NIR thật dùng modality=ir. Với ảnh dựng SVIRO, tôi đặt light=unknown.

Ở phần OMS, tôi sử dụng person, face, child_seat, infant_seat và object theo đối tượng cần gán. Tôi kiểm lại seat theo quy ước của xe: bên trái ảnh là rear_right, giữa ảnh là rear_middle, bên phải ảnh là rear_left. Các thuộc tính của ảnh IR được xét riêng, không suy giá trị không nhìn rõ từ ảnh RGB cùng cảnh.

Ở phần DMS, tôi gán person, face, face13 và hand theo nội dung từng khung. Thuộc tính mắt được xét riêng cho từng bên; tay và điểm mặt trái/phải tính theo người lái. Với các điểm không xác định được hoặc bị che, tôi sử dụng cờ outside hoặc occluded theo guideline. Tôi giữ nguyên hướng ảnh D## đã xoay và kiểm lại sự nhất quán giữa trạng thái mắt với cờ điểm.

Cấu hình nhãn mới cho phép child_seat.occupied nhận unknown, no và yes. Tôi sử dụng cấu hình này để kiểm bài, giữ bốn giá trị yes tại S01/S08 thay vì đổi theo giới hạn của checker cũ.

## 3 Kết quả tự kiểm và sửa lỗi

Bản sau sửa được lưu dưới tên **export_final_v2.zip**, xuất lúc **17:24:44 ngày 21/11/2026**, giờ Việt Nam. Tôi chạy lại cabin_check và kiểm bổ sung các thuộc tính ghế trong bản xuất này.

| Nội dung kiểm tra | Kết quả cuối cùng |
|---|---|
| Số lượng ảnh | Đủ 22 ảnh: 16 OMS và 6 DMS. |
| frame_meta | Cả 22 ảnh có đúng một tag. |
| modality | Khớp loại ảnh trên cả 22 khung. |
| light trên SVIRO | Tất cả là unknown. |
| cabin_check | 0 lỗi, 0 cảnh báo. |
| Ghế từng bị đảo trái/phải | Đã sửa và đối chiếu đủ 22 box trên 14 ảnh. |
| File export | Chỉ chứa annotations.xml, không kèm ảnh. |

Lỗi lặp lại nhiều nhất của tôi là chọn rear_left và rear_right theo phía của ảnh. Sau khi rà soát, tôi sửa toàn bộ 22 box bị ảnh hưởng, giữ nguyên các box rear_middle. Bốn box person tại S02_rgb, S02_ir, S08_rgb và S08_ir đã được sửa từ rear_right thành rear_left trong lượt cuối. Tôi kiểm từng box theo tên ảnh, loại nhãn và tọa độ; kết quả được lưu trong seat_review.csv.

Tôi cũng đã sửa modality của S02_rgb/S03_rgb, light trên các ảnh SVIRO liên quan và sự không nhất quán giữa thuộc tính mắt với cờ điểm ở D06. Tôi thực hiện sửa trên CVAT rồi export lại, không sửa nhãn trực tiếp trong XML. Kết quả kiểm sau sửa nằm trong check_final_v2.txt; bản trước sửa được giữ để đối chiếu quá trình rework.

## 4 Kết quả review chéo

Tôi đối chiếu bài với Lê Quang Khải, cùng bộ G07. File project ban đầu của Khải gồm hai task; tôi chọn phần 22 ảnh có nhãn, chuẩn hóa tên ảnh trùng để ghép đúng khung, không thay đổi nhãn, tọa độ hoặc thuộc tính của bạn.

Lần diff trên hai bản trước sửa ghi nhận **140 khác biệt** ở ngưỡng IoU 0,5. Tôi ghi nhận xét, mục guideline và hướng xử lý trong disagreement_log.csv. Các nhóm cần kiểm gồm vị trí ghế, lớp ghế trẻ em/nôi, trạng thái mắt và cờ điểm, tay trái/phải, box face và các box phone trong D05 của Khải.

Qua đối chiếu, tôi nhận ra lỗi đảo ghế trong bài mình và đã sửa hết. Khải cũng đã sửa theo bài của tôi; tôi xác nhận hai bên không còn bất đồng sau trao đổi. File export mới của Khải chưa có trong hồ sơ nên tôi giữ 140 khác biệt như số liệu lịch sử, không ghi thành kết quả diff mới bằng 0. Các cột followup trong nhật ký ghi nhận kết quả trao đổi sau lần phân tích ban đầu.

## 5 Ghi nhận về IR và danh sách lớp

Tôi hoàn thiện ir_log.csv với bốn dòng tại S02_ir, S07_ir, D04 và D06, phân biệt rõ IR_sim và real_IR.

| Khung | Nguồn | Nội dung đối chiếu từ export |
|---|---|---|
| S02_ir | IR_sim | Hai mắt đều unknown trên cả bản RGB và IR; không suy unknown thành closed. |
| S07_ir | IR_sim | object.type=box nhất quán giữa hai bản nhãn RGB và IR. |
| D04 | real_IR | Hai mắt unknown; tám điểm mắt có occluded=1. |
| D06 | real_IR | Mắt trái open, mắt phải unknown; bốn điểm mắt phải có outside=1. |

Những ghi nhận này dựa trên nhãn trong export. Tôi không suy thêm nguyên nhân như phản xạ kính, chói sáng hoặc mức mất chi tiết khi chưa xác nhận trực tiếp trên ảnh. IR mô phỏng của SVIRO không được dùng để kết luận các hiện tượng quang học của NIR thật.

Trong class_list_v0.md, tôi xây dựng 8 mục DMS và 8 mục OMS, nêu giá trị, mục đích sử dụng và khả năng quan sát trên IR. Tôi đặt hai câu hỏi leo thang có khung ví dụ: cách gán người trong ghế khi occupied=yes và cách áp dụng ngưỡng mặt rộng 60 px trên ảnh xoay.

## 6 Hồ sơ nộp và bài học rút ra

Tôi chuẩn bị đủ 11 file bắt buộc theo SUBMISSION.md, kèm export_final_v2.zip và check_final_v2.txt sau sửa. Báo cáo này, seat_review.csv và check_peer_khai.txt bổ sung bằng chứng QC. Ảnh gốc, gói dữ liệu và export nguồn của Khải không được đưa vào bộ bài nộp cá nhân.

check_cpA.txt và check_cpB.txt là các lần kiểm bù đúng phạm vi trên bản gán nhãn hoàn chỉnh, không phải log gốc tại thời điểm checkpoint trong lớp. Kết quả cuối xác nhận các lỗi cấu trúc, metadata và đảo ghế đã phát hiện đều được xử lý; việc đánh giá trực quan độ sát box, lớp vật thể và vị trí điểm mặt vẫn cần dựa vào ảnh.

Bài học chính của tôi là phải áp dụng đúng hệ quy chiếu trước khi chọn thuộc tính và kiểm lại cả cặp RGB/IR. Kết quả 0 lỗi của script hoặc sự thống nhất giữa hai người chỉ phản ánh các điều kiện đã kiểm, không tự chứng minh toàn bộ nhãn đúng. Tôi cần kết hợp guideline, kiểm tra dữ liệu và review chéo để giảm lỗi lặp lại.
