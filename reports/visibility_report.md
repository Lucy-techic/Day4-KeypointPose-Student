# Visibility report

- Thư mục nhãn: `dataset/labels/train`
- 20 ảnh, 29 skeleton, trung bình 16.0 khớp có v > 0 mỗi người
- Tổng: v=2 351 | v=1 113 | v=0 29

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 24 | 5 | 0 | 17% |
| 1 | left_eye | 23 | 6 | 0 | 21% |
| 2 | right_eye | 22 | 7 | 0 | 24% |
| 3 | left_ear | 13 | 16 | 0 | 55% |
| 4 | right_ear | 16 | 13 | 0 | 45% |
| 5 | left_shoulder | 26 | 3 | 0 | 10% |
| 6 | right_shoulder | 27 | 2 | 0 | 7% |
| 7 | left_elbow | 23 | 6 | 0 | 21% |
| 8 | right_elbow | 26 | 3 | 0 | 10% |
| 9 | left_wrist | 20 | 8 | 1 | 28% |
| 10 | right_wrist | 19 | 10 | 0 | 34% |
| 11 | left_hip | 20 | 8 | 1 | 28% |
| 12 | right_hip | 22 | 6 | 1 | 21% |
| 13 | left_knee | 18 | 7 | 4 | 24% |
| 14 | right_knee | 22 | 3 | 4 | 10% |
| 15 | left_ankle | 14 | 6 | 9 | 21% |
| 16 | right_ankle | 16 | 4 | 9 | 14% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
