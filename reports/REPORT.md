# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Ngọc Huyền Vũ   Nhóm: ______   Ngày: 17/09/2026

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 351 / 113 / 29 |
| Thời gian trung bình mỗi ảnh | ~7 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. left_ear (55%)
2. right_ear (45%)
3. right_wrist (34%)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Đúng nhưng không đáng kể. Tai thường bị tóc hoặc góc nghiêng đầu che một phần, còn cổ tay hay bị chính vật đang cầm (pizza, ghi-đông xe) hoặc tay còn lại che khuất. Đây là các khớp có tỉ lệ bị che tự nhiên cao trong ảnh đời thường (do tư thế, góc chụp), không phải do gán thiếu cẩn thận.

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.918 | 0.921 |
| OKS@0.50 | 1.000 | 1.000 |
| OKS@0.75 | 1.000 | 1.000 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 2 | 1 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- train_10.jpg, người #1, khớp left_hip: toạ độ đang nằm ngoài khung ảnh (y=1.038) trong khi trạng thái vẫn để Occluded — kéo điểm vào lại bên trong khung ảnh, giữ nguyên trạng thái Occluded, vì khớp vẫn còn trong vùng nhìn thấy được của ảnh.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ, bạn nghĩ vì sao mình vẫn sai?

Không có lỗi đảo trái/phải nào bị evaluator tính điểm trừ trong kết quả chấm với gold. Công cụ `check_pose_labels.py` có cảnh báo nghi ngờ đảo trái/phải ở train_02 và train_16 (dựa trên vị trí vai/hông so với hai mắt), nhưng đây là 2 ảnh có tư thế nghiêng người/xoay người khó (người đạp xe nhìn qua vai, hai người nhảy bắt đĩa) nên khó xác định chắc chắn bằng mắt thường; kết quả chấm với gold không liệt các ảnh này vào lỗi đảo trái/phải thực sự.

## 3. Kiểm chéo

*(Không thực hiện phần này.)*

## 4. Model

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

1. `pose_mAP50-95` tăng 0.0055 (không giảm). Với chỉ 20 ảnh, mức tăng nhỏ là hợp lý — model học thêm một chút đặc điểm hình dáng người trong bối cảnh cụ thể (góc chụp, trang phục, tư thế) mà 20 ảnh này mang lại, khác với phân phối ảnh gốc của COCO.

2. `box_mAP50-95` (0.8041) cao hơn `pose_mAP50-95` (0.6908) khoảng 0.1133. Model tìm người (detection) dễ hơn tìm khớp (pose) — phát hiện một người trong ảnh đơn giản hơn nhiều so với định vị chính xác 17 điểm khớp nhỏ, nhất là khi khớp bị che hoặc ở tư thế phức tạp.

3. *(cần bổ sung sau khi xem thêm ảnh dự đoán của model)*

4. *(cần bổ sung sau khi xem thêm ảnh dự đoán của model)*

5. *(cần bổ sung — hai tập đánh giá train/test khác nhau nên chưa so sánh trực tiếp được)*

## 5. Một rule evidence bạn đã dùng

Khớp `left_elbow` của người #1 (cô phục vụ) trong ảnh `train_01` bị tay áo dài che gần hết, đồng thời một phần bị khay pizza cô đang bưng che thêm. Tôi vẫn thấy được đường cong cánh tay từ vai xuống đến cổ tay, nên suy ra được vị trí khuỷu tay hợp lý dựa vào hình dáng tay áo và tư thế cánh tay. Vì khớp vẫn nằm trong khung ảnh và còn cơ sở giải phẫu để đặt điểm, tôi chọn v=1 (Occluded) thay vì bỏ qua hay đặt v=0.
