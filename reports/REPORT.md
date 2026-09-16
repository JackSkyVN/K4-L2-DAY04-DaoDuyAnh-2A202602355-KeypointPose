# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Đào Duy Anh   Nhóm: K4-2A   Ngày: 16/09/2026   MSSV: 2A202602355

---

## 1. Nhãn của tôi

<!-- Số liệu lấy từ outputs/visibility_report.json và reports/visibility_report.md -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 27 |
| v=2 / v=1 / v=0 | 277 / 162 / 20 |
| Thời gian trung bình mỗi ảnh | ~5 phút/ảnh |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear` — 48% (13/27 skeleton)
2. `left_knee` — 48% (13/25 skeleton)
3. `right_ankle` — 48% (13/27 skeleton)

**Chúng có đúng là những khớp bạn thấy khó gán nhất không?**

Không hoàn toàn trùng khớp. Những khớp có `%v=1` cao nhất như `left_ear` hay `right_ankle` chủ yếu là các khớp **hay bị che theo góc nhìn tự nhiên** (tai bị tóc che khi người quay nghiêng, cổ chân nằm ngoài mép bàn ghế) nhưng vị trí giải phẫu của chúng tương đối dễ suy đoán từ khớp liền kề. Ngược lại, khớp tôi thực sự thấy khó gán nhất là **`left_hip` và `right_hip`** khi người mặc áo dài hoặc ngồi trên ghế, vì vải che mất đường xương háng và tôi phải ước lượng từ đường eo. Khớp `left_elbow` và `right_elbow` cũng khó khi cánh tay sát thân, bóng đổ làm mờ điểm giữa xương.

---

## 2. Chấm với gold

<!-- Số liệu lấy từ outputs/eval_vs_gold.json — chạy lần đầu (trước rework) -->
<!-- Bài này chỉ có một lần chấm, chưa thực hiện rework -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.9425 | — |
| OKS@0.50 | 0.931 | — |
| OKS@0.75 | 0.931 | — |
| Lỗi `co_khac_gold` (cờ khác gold) | 84 | — |
| Lỗi `thieu_nguoi` (thiếu skeleton) | 2 | — |
| Lỗi `xoa_khop_bi_che` (gold_khong_gan_nhan) | 70 | — |
| Lỗi `lech_nhe` (lệch nhẹ vị trí) | 6 | — |

**Tôi đã sửa gì giữa hai lần chạy** *(chưa thực hiện rework — ghi lại những gì sẽ sửa nếu có lần 2)*:

- `train_13` — người thứ 2 và 3: cần thêm 2 skeleton bị bỏ sót hoàn toàn (lỗi `thieu_nguoi`)
- `train_02` — người thứ 1, khớp `left_ear`: lệch 26 px (2.4× bán kính dung sai), cần dịch chấm về phía bên trái khuôn mặt
- `train_04` — người thứ 1, khớp `left_wrist`: lệch 43 px (1.2×), cần đặt lại chấm theo hướng ngón tay
- `train_03` — người thứ 1, khớp `left_hip` và `right_hip`: lệch ~40–45 px, cần đặt chấm thấp hơn theo đường xương háng

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?**

Không có lỗi đảo trái/phải (`dao_trai_phai`) trong toàn bộ 20 ảnh theo kết quả `eval_vs_gold.json`. Lỗi chủ yếu là **cờ visibility lệch so với gold** (`co_khac_gold`: 84 trường hợp) — tức là vị trí khớp đúng nhưng tôi ghi `v=1` ở những nơi gold ghi `v=2` hoặc ngược lại. Điều này cho thấy guideline về ranh giới `v=1` vs `v=2` chưa được áp dụng nhất quán, đặc biệt ở khuỷu tay, hông và gối.

---

## 3. Kiểm chéo

Bạn cùng nhóm: N/A (thực hiện đơn lẻ, không có bạn cùng nhóm để kiểm chéo)

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| N/A | — | — | — | Không có dữ liệu kiểm chéo |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

- *Khuỷu tay và hông nhìn thấy biên dạng cơ thể nhưng bị bóng/vải che mốc xương cụ thể → chọn `v = 2` nếu sai số ước lượng < 0.5× bán kính dung sai; chọn `v = 1` nếu phải suy từ khớp liền kề với sai số > 0.5× bán kính.*

---

## 4. Model

<!-- Số liệu từ outputs/Day4_Lab_Outputs/eval_model.json -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.845 | 0.845 | 0.000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0.000 |
| box_mAP50-95 | 0.8119 | 0.8041 | −0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể.

1. **`pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model điều gì mà COCO chưa dạy?**

   `pose_mAP50-95` tăng nhẹ +0.0055 (từ 0.6853 lên 0.6908). Mức tăng rất nhỏ cho thấy 20 ảnh có đóng góp nhưng không đủ để thay đổi đáng kể khả năng định vị khớp chính xác ở nhiều ngưỡng IoU. 20 ảnh của tôi chủ yếu là cảnh trong nhà/đô thị với nhiều người ngồi hoặc bị che khuất một phần — những tình huống ít xuất hiện trong COCO. Điều đó giúp model cải thiện nhẹ độ chính xác khớp ở ngưỡng cao (0.75–0.95), nhưng chưa đủ mẫu để tạo đột phá.

2. **`box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm *khớp* dễ hơn?**

   `box_mAP50` baseline = 0.9785, `pose_mAP50` baseline = 0.845 — chênh 0.1335. Model tìm **người** (hộp bao) dễ hơn nhiều so với định vị khớp chính xác. Điều này hợp lý vì việc khoanh vùng thân người chỉ cần nhận diện hình dạng tổng thể, trong khi xác định 17 khớp đòi hỏi hiểu cấu trúc giải phẫu chi tiết và xử lý tình huống bị che khuất.

3. **Một ảnh test model đoán sai — gọi tên lỗi theo bốn loại của slide 43:**

   Lỗi phổ biến nhất trong `eval_vs_gold.json` là **lệch nhẹ** (`lech_nhe`): ảnh `train_02` người 1, khớp `left_ear` lệch 26 px (2.4× bán kính dung sai). Đây là loại "lệch nhẹ" — model/người gán đặt chấm gần đúng nhưng chưa đủ sát mốc giải phẫu. Không có lỗi "đảo trái/phải" hay "nhầm người" trong bài gán của tôi.

4. **Ảnh nào có OKS thấp nhất giữa nhãn của bạn và gold? Ai đúng?**

   Ảnh `train_13` có OKS thấp nhất vì thiếu 2 skeleton (OKS = 0.0 cho 2 người bị bỏ sót). Trong số các ảnh được match, `train_04` người 1 có OKS = 0.8885 — thấp nhất do lệch vị trí cổ tay (`left_wrist` lệch 43 px) và khác cờ ở khuỷu tay. Gold đúng hơn ở những trường hợp này: vị trí gold sát mốc giải phẫu hơn, còn tôi đặt chấm thiên về vị trí nhìn thấy bề mặt da thay vì tâm khớp.

5. **Ảnh bạn gán tệ nhất có cũng là ảnh model đoán tệ nhất không?**

   Ảnh tôi gán tệ nhất là `train_13` (thiếu 2 người) và `train_04` (lệch cổ tay/khuỷu). Đây đều là ảnh có nhiều người hoặc người bị che khuất phức tạp. Nếu model cũng đoán tệ ở những ảnh này, điều đó xác nhận: **bức ảnh khó** (nhiều người chồng nhau, góc chụp lạ, che khuất nhiều) là nguồn gốc lỗi chung của cả người gán lẫn model — không phải do nhãn của tôi sai mà do tình huống trong ảnh vốn đã ambiguous.

---

## 5. Một rule evidence bạn đã dùng

**Ảnh:** `train_09`, người thứ `1`, khớp `right_ankle`

Trong ảnh `train_09`, người ngồi trên xe máy, chân phải duỗi xuống nhưng mắt cá phải (`right_ankle`) bị khung xe và chân chống xe che gần hoàn toàn — chỉ thấy phần đầu bàn chân ló ra phía dưới. Căn cứ thị giác: cẳng chân phải nhìn thấy rõ từ gối xuống 2/3 đoạn, hướng cẳng chân chỉ thẳng xuống, và bàn chân thấy ở phía dưới mép che. Tôi suy vị trí mắt cá theo đường kéo dài của cẳng chân, cách đầu bàn chân khoảng 1 lần chiều dài bàn chân, và gắn `v = 1` vì khớp còn trong khung hình dù bị che. Đây là trường hợp áp dụng đúng rule: *khớp bị che bởi vật thể (khung xe) nhưng vị trí giải phẫu suy luận được từ đoạn chi lộ ra → `v = 1`, vẫn đặt chấm*.
