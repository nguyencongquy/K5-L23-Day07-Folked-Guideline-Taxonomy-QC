# Tự kiểm ngược — N001 và Q01

Ngày 2026-11-17, thực hiện theo ủy quyền 103. Đây không phải kiểm bởi người ngoài nhóm.

1. Mở data/images/img_024.jpg: xe vàng phía trên có thân, bánh và kích thước xe thật; không phải xe đồ chơi. Ảnh nguồn giữ đúng hash trong batch_manifest.json.
2. N001 → R-04/R-06 của guideline v1.1: xe trưng bày/được chở vẫn được gán; không lấy vị trí đặt cao làm dấu hiệu giả.
3. Vòng 1: labels_snapshot.json có xe vàng #359 và box mảnh #351. qc_review.csv đánh #351 thua; tỷ lệ 1/40 =2.5%; qc_result.json REWORK.
4. Q01: bỏ source 103:314 (box mảnh), giữ source NEW của toàn xe vàng. Vòng 2 có xe vàng #358; vòng 3 labels_snapshot.json có xe vàng #359, bbox [241,43,465,201], class car, occluded_level=0, truncated=no, can_review=false. Không còn box mảnh cũ.
5. Nhãn hiện hành SHA-256 1ad177c9b4e2c589140c8d34bbec689c53b6cd30d38cc57a91c019aca823dfbf; manifest và qc_result vòng 3 cùng bản. Tự kiểm nối được báo cáo → quyết định → rule → ảnh → nhãn.

Giới hạn: chưa có một người ngoài nhóm xác nhận lại đường truy vết này. Không ghi thay họ.
