# Mini guideline - nhóm: ______  |  người gán: Ngọc Huyền Vũ  |  ngày: 17/09/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Vẫn suy ra vị trí hông theo đường thẳng nối vai-hông-gối, gắn v=1 | Trang phục dài (tạp dề, váy) không xoá bằng chứng giải phẫu, cơ thể vẫn có cấu trúc thẳng hàng để ước lượng |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Nếu còn thấy được rìa tai hoặc vị trí đầu rõ ràng, gắn v=1 và đặt chấm theo bên còn lộ; nếu bị che hoàn toàn 100% không còn dấu vết, vẫn ước lượng theo đối xứng với tai bên kia | Tóc/mũ là vật cản tạm thời, không làm mất cấu trúc đầu; đây là khớp có %v=1 cao nhất trong bộ dữ liệu (55% với tai trái) |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp từ đầu gối trở xuống gắn v=0 (Outside), không đoán bừa | Không còn bất kỳ bằng chứng giải phẫu nào (không thấy chân, không thấy vật tham chiếu) nên không đủ cơ sở để ước lượng toạ độ |
| Cổ tay nằm sau tay lái / sau thân mình | Vẫn đặt chấm theo hướng cẳng tay và vị trí bàn tay ước lượng được, gắn v=1 | Vẫn còn cơ sở từ khuỷu tay và hướng chuyển động của cánh tay để suy ra vị trí hợp lý |
| Hai người chồng lên nhau | Đọc kỹ tên nhãn hiện ở sidebar khi kéo từng điểm, xác nhận đúng người trước khi đặt, tránh nhầm cổ tay/khuỷu tay của người này sang người kia | Vùng chồng lấn (ví dụ hai tay cùng bưng một vật) rất dễ gây lỗi nhầm người (nham_nguoi) nếu không kiểm tra lại |
| Người nhỏ đến mức nào thì không gán nữa | Nếu vẫn phân biệt được ít nhất phần thân trên (đầu, vai) bằng mắt thường thì vẫn gán đủ 17 điểm; người quá nhỏ không phân biệt được bộ phận cơ thể thì bỏ qua, không tạo skeleton | Tránh tạo nhãn nhiễu (noise) không đáng tin cậy khi không còn đủ độ phân giải để xác định vị trí khớp |

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_01`, người thứ `1` (cô phục vụ), khớp `left_elbow`

- Mơ hồ ở chỗ nào: Khuỷu tay bị tay áo dài của áo blouse che gần hết, đồng thời một phần bị khay pizza cô đang bưng che thêm, chỉ còn thấy đường viền tay áo.
- Bạn quyết thế nào: Gắn v=1 (Occluded), đặt chấm theo đường cong tay áo ước lượng vị trí khuỷu tay thật.
- Vì sao: Vẫn còn đủ bằng chứng thị giác (hình dáng tay áo, tư thế cánh tay từ vai đến cổ tay) để suy luận vị trí hợp lý, khớp vẫn nằm trong khung ảnh.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu gắn v=0 thay vì v=1, model sẽ học rằng khuỷu tay "biến mất" trong tình huống bị che nhẹ, dẫn đến việc model không cố gắng dự đoán khớp khi gặp cản trở tương tự, làm giảm recall ở các ảnh có người mặc áo dài tay.

### Ca 2 - ảnh `train_04`, người thứ `1`, khớp `left_wrist`

- Mơ hồ ở chỗ nào: Cổ tay nằm sát mép ảnh, một phần bị vật thể trong ảnh che, ban đầu tôi phân vân giữa việc coi là "ra ngoài khung" hay "vẫn còn trong khung nhưng bị che".
- Bạn quyết thế nào: Ban đầu gắn v=0, sau khi chấm với gold phát hiện đây là lỗi (gold coi là v=1) nên đã cân nhắc lại nhưng ảnh này thuộc nhóm 2 lỗi ưu tiên cần sửa còn sót lại sau rework.
- Vì sao: Khớp vẫn nằm trong phạm vi ảnh nhìn thấy được, có đủ bằng chứng từ hướng cẳng tay để ước lượng, nên đúng ra phải là v=1 thay vì v=0.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Gắn sai v=0 khi thực chất là v=1 sẽ khiến khớp đó bị loại hoàn toàn khỏi tính điểm OKS và khỏi dữ liệu train, làm model mất đi ví dụ học cách xử lý trường hợp cổ tay bị che nhẹ.

### Ca 3 - ảnh `train_10`, người thứ `1`, khớp `left_hip`

- Mơ hồ ở chỗ nào: Khi kéo điểm ước lượng cho khớp bị che, tôi vô tình đặt điểm hơi lệch ra ngoài rìa dưới của khung ảnh (toạ độ vượt quá 100% chiều cao ảnh một chút) trong khi vẫn giữ trạng thái Occluded.
- Bạn quyết thế nào: Kéo lại điểm vào rõ ràng bên trong khung ảnh, giữ nguyên trạng thái v=1 vì khớp thực chất vẫn nằm trong phạm vi có thể suy luận được.
- Vì sao: Occluded (v=1) theo định nghĩa bắt buộc phải có toạ độ hợp lệ nằm trong ảnh; nếu điểm thực sự ở ngoài khung thì phải chuyển thành v=0, không được để toạ độ mâu thuẫn với trạng thái.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu giữ nguyên toạ độ ngoài khung với nhãn v=1, dữ liệu sẽ chứa thông tin sai lệch (khớp "nhìn thấy được ở ngoài ảnh" là vô nghĩa), có thể gây nhiễu khi model học vị trí tương đối của khớp hông so với các khớp khác.

## 4. Sau khi so visibility report với bạn cùng nhóm

*(Không thực hiện phần kiểm chéo với bạn cùng nhóm trong lần nộp này.)*

- Khớp lệch `%v=1` nhiều nhất: ______
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: ______
- Luật mới bổ sung vào mục 2 sau khi thống nhất: ______
