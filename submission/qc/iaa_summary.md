# Tóm tắt IAA (taxonomy v1.0)

- Ảnh được so sánh (có >=2 người gán): **40**; số người gán: 3
- Lượt gán đủ **80/80**.
- IoU trung bình và % cùng lớp **chỉ tính trên các cặp box đã ghép** (IoU >= 0.5, ghép 1-1 không phân biệt lớp). Box chưa ghép được thống kê riêng ở cột bỏ sót/thừa.
- Mẫu số: 303 cặp box ghép (tổng mọi cặp người gán, ảnh chung).

| Cặp | Ảnh | Cặp box ghép | IoU TB | Cùng lớp | Cùng occluded_level | Cùng truncated | Chưa ghép (A/B) | Bỏ qua do ignore | Đạt IAA |
|---|---|---|---|---|---|---|---|---|---|
| **TỔNG** | 40 | 303 | 0.806 | 93.1% | 49.8% | 95.7% | 53/237 | 0 | CHƯA ĐẠT |
| 101|102 | 14 | 60 | 0.778 | 95.0% | 50.0% | 98.3% | 25/18 | 0 | CHƯA ĐẠT |
| 101|103 | 13 | 132 | 0.852 | 89.4% | 38.6% | 95.5% | 11/124 | 0 | CHƯA ĐẠT |
| 102|103 | 13 | 111 | 0.766 | 96.4% | 63.1% | 94.6% | 17/95 | 0 | CHƯA ĐẠT |

Ngưỡng IAA: IoU TB >= 0.85, đồng thuận lớp >= 95%.

Số ca bất thường: 850 ({'bo_sot_thua': 272, 'co_can_review': 169, 'iou_thap': 151, 'sai_thuoc_tinh': 157, 'can_review_anh': 14, 'bat_dong_ignore': 66, 'sai_lop': 21}), trên 40 ảnh cần calibration.
