# Mini guideline - nhóm: K4-2A  |  người gán: Đào Duy Anh  |  ngày: 16/09/2026

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
| Hông của người mặc quần áo dài | `v = 1` nếu thấy hình dạng vùng hông qua vải; `v = 0` nếu bị che toàn bộ ngoài mép ảnh | Hông là khớp nối thân–chân; dù bị che bởi quần áo, vị trí giải phẫu vẫn suy luận được qua đường eo và đùi |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | `v = 1` — vẫn đặt chấm ở vị trí ước lượng sau vành tai | Tai còn trong khung hình, có thể suy đoán từ đường viền mặt và vị trí mắt |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp thấy được: `v = 2` hoặc `v = 1`; cổ chân/gối nằm ngoài mép: `v = 0` | Ra ngoài mép ảnh thì model không thể học được, phải đánh `Outside` để loại khỏi OKS |
| Cổ tay nằm sau tay lái / sau thân mình | `v = 1` — đặt chấm ở vị trí ước lượng dựa theo hướng cẳng tay | Cổ tay còn trong khung, bị che bởi vật thể; suy vị trí từ góc khuỷu và cẳng tay |
| Hai người chồng lên nhau | Gán đủ 17 điểm cho từng người; khớp bị người kia che → `v = 1` | Mỗi skeleton độc lập, không bỏ qua khớp; phân biệt trái/phải theo cơ thể từng người |
| Người nhỏ đến mức nào thì không gán nữa | Không gán nếu chiều cao skeleton < 50 px trên ảnh gốc | Quá nhỏ thì không định vị được 17 khớp chính xác; gán sai còn tệ hơn bỏ |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_01`, người thứ `1`, khớp `left_ear`

- Mơ hồ ở chỗ nào: Người quay nhẹ sang phải, tai trái bị tóc che gần hoàn toàn, chỉ thấy mép nhỏ của vành tai
- Bạn quyết thế nào: `v = 1`, đặt chấm ước lượng theo đường viền hộp sọ và vị trí tai phải làm mốc đối xứng
- Vì sao: Tai còn trong khung ảnh, không bị cắt mép; dù bị tóc che vẫn suy được vị trí giải phẫu
- Nếu người khác quyết ngược lại (`v = 0`): Model sẽ mất điểm OKS cho khớp này và bị dạy bỏ qua tai bên trái khi người quay nghiêng — làm giảm khả năng nhận diện tư thế quay đầu

### Ca 2 - ảnh `train_04`, người thứ `1`, khớp `left_elbow`

- Mơ hồ ở chỗ nào: Khuỷu tay trái nằm sát thân người, bóng đổ và trang phục tối màu làm ranh giới khuỷu–thân không rõ
- Bạn quyết thế nào: `v = 1`, đặt chấm theo vị trí ước lượng ở giữa cánh tay trên và cẳng tay (gold ghi `v = 2`)
- Vì sao: Khuỷu tay nhìn thấy được nhưng bóng làm khó xác định điểm giữa chính xác; chọn `v = 1` để thể hiện sự không chắc chắn
- Nếu người khác quyết ngược lại (`v = 2`): Model học rằng khuỷu tay trong tình huống này là rõ ràng, có thể làm giảm ngưỡng cảnh báo khi tư thế tương tự nhưng thực sự bị che

### Ca 3 - ảnh `train_13`, người thứ `2 và 3` (thiếu skeleton)

- Mơ hồ ở chỗ nào: Ảnh có 3 người nhưng chỉ gán 1 skeleton; 2 người còn lại đứng khuất sau/bên không thấy rõ toàn thân
- Bạn quyết thế nào: Chỉ gán người nhìn thấy rõ nhất, bỏ sót 2 người khuất
- Vì sao: Phán đoán sai rằng người bị che < 50% cơ thể thì không cần gán — nhưng luật bắt buộc là gán tất cả mọi người trong ảnh
- Nếu người khác quyết ngược lại (gán đủ 3): Model học đúng hơn; bài của tôi bị trừ điểm `thieu_nguoi` ở 2 skeleton trong gold

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: N/A (chưa kiểm chéo — thực hiện đơn lẻ)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: N/A
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Từ kết quả so với gold, thêm luật: *Khuỷu tay và hông nhìn thấy biên dạng cơ thể nhưng bị bóng/vải che mốc xương → chọn `v = 2` nếu vị trí suy ra dưới 0.5× bán kính dung sai; chọn `v = 1` nếu phải ước lượng từ khớp liền kề.*
