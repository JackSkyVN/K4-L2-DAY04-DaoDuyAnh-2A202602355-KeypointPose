# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 27 skeleton, trung bình 16.26 khớp có v > 0 mỗi người
- Tổng: v=2 277 | v=1 162 | v=0 20

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 21 | 6 | 0 | 22% |
| 1 | left_eye | 19 | 8 | 0 | 30% |
| 2 | right_eye | 20 | 7 | 0 | 26% |
| 3 | left_ear | 14 | 13 | 0 | 48% |
| 4 | right_ear | 17 | 10 | 0 | 37% |
| 5 | left_shoulder | 23 | 4 | 0 | 15% |
| 6 | right_shoulder | 25 | 2 | 0 | 7% |
| 7 | left_elbow | 17 | 10 | 0 | 37% |
| 8 | right_elbow | 16 | 11 | 0 | 41% |
| 9 | left_wrist | 17 | 10 | 0 | 37% |
| 10 | right_wrist | 15 | 11 | 1 | 41% |
| 11 | left_hip | 15 | 12 | 0 | 44% |
| 12 | right_hip | 18 | 9 | 0 | 33% |
| 13 | left_knee | 12 | 13 | 2 | 48% |
| 14 | right_knee | 13 | 13 | 1 | 48% |
| 15 | left_ankle | 9 | 10 | 8 | 37% |
| 16 | right_ankle | 6 | 13 | 8 | 48% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
