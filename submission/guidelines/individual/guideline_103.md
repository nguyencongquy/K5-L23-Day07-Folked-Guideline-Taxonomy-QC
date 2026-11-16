# Guideline cá nhân — 103

- Phiên bản v1.0, hoàn thiện ngày 2026-11-16 sau khi nhóm chốt quy tắc chung.
- Đã xem đủ 40 ảnh. Lượt gán độc lập được phân công 26 ảnh.
- Áp dụng [guideline nhóm v1.0](../guideline_v1.0_agreed.md) và [taxonomy](../../taxonomy.json).

## 1. Mục tiêu

Gán bounding box người và phương tiện để huấn luyện mô hình phát hiện vật thể trên ảnh đường phố. Dùng CVAT, xuất COCO 1.0. Box bao dư, gộp người với xe hoặc bỏ sót vật sẽ làm sai vị trí và lớp mà mô hình học.

## 2. Lớp và thuộc tính

### Lớp

| Lớp | Cách nhận biết | Ví dụ | Dễ nhầm / loại khỏi lớp |
|---|---|---|---|
| `car` | Ô tô con hoặc SUV thật. | Ô tô được chở trong `img_002`. | Van thân hộp; pickup có thùng hàng riêng. |
| `truck` | Xe có cabin và sàn/thùng hàng riêng, gồm pickup và xe chở ô tô. | Xe chở ở `img_002`; pickup ở `img_037`. | Van có khoang kín liền thân. Pickup có nắp thùng vẫn là truck. |
| `van` | Xe cỡ nhỏ thân hộp, khoang kín phía sau liền thân với cabin. | Van phía sau người đi xe ở `img_005`. | SUV, pickup và bus. Thiếu dấu hiệu phân biệt thì dùng review. |
| `motorcycle` | Xe máy hoặc mô tô hai bánh có động cơ, gồm xe tay ga. | Xe máy ở `img_005`, `img_026`. | Xe đạp, xích lô, tuk-tuk. Người ngồi trên xe có box riêng. |
| `person` | Người thật nhìn trực tiếp, gồm người đi bộ và người trên phương tiện. | Hai người trên xe ở `img_026`. | Chân dung in ở `img_018`, tượng, ma-nơ-canh, ảnh phản chiếu. |
| `ignore_region` | Vùng chứa vật thuộc phạm vi nhưng quá nhỏ hoặc quá dày để tách đáng tin cậy. | Cụm xe/người xa có từng box cao dưới 20 px. | Vật ngoài taxonomy hoặc vật rõ, cao từ 20 px trở lên. |

Bus, tuk-tuk, xe đạp và xích lô nằm ngoài bộ lớp v1.0. Bỏ qua phương tiện đó, vẫn xét riêng người thật trên xe. Khi chưa xác định được vật có thuộc phạm vi hay không, ghi vị trí và lý do để review.

### Thuộc tính

Ước lượng tỷ lệ phần vật bị vật khác che so với tổng phần nhìn thấy và phần bị che trong khung ảnh. Phần nằm ngoài mép ảnh được xét bằng `truncated`.

| `occluded_level` | Tỷ lệ bị che |
|---|---|
| `0` | 0%. |
| `1` | Trên 0% đến hết 30%. |
| `2` | Trên 30% đến hết 70%. |
| `3` | Trên 70%, còn phần nhìn thấy đủ điều kiện gán. |

- `truncated=yes` khi mép ảnh thực sự cắt qua vật; `no` khi vật nằm trọn trong ảnh. Một vật có thể vừa bị che vừa bị cắt mép.
- `can_review=true` khi đặt được box nhưng còn nghi ngờ lớp, biên hoặc mức che. Ghi ảnh, vị trí và lý do. Sát ngưỡng 30%/70% thì chọn mức có căn cứ và đánh dấu review.
- Nếu không đặt được box đáng tin cậy, ghi vấn đề cấp ảnh để nhóm xử lý. Riêng `ignore_region` có thể để trống các thuộc tính đối tượng.
- Trong CVAT, điền `occluded_level` trong Details. Cờ Occluded/phím Q là trường riêng và checker lab bỏ qua.

## 3. Quy tắc vẽ box

Mã quy tắc dưới đây khớp bản chung để tiện đối chiếu khi review.

| Rule ID | Cách làm |
|---|---|
| R-01 | Vẽ hình chữ nhật nhỏ nhất bao mọi phần nhìn thấy của cùng một vật. Phần khuất không được suy diễn để nới box. |
| R-02 | Mỗi vật một box. Box của các vật khác nhau được phép chồng nhau. |
| R-03 | Tách từng người và xe. Hai người trên một xe máy có hai box person và một box motorcycle, nếu từng vật đủ điều kiện. |
| R-04 | Tách xe chở và từng xe được chở. Box xe chở gồm phần xe, khung/sàn cố định; hàng rời và ô tô được chở được xét riêng. |
| R-05 | Đo chiều cao box theo pixel ảnh gốc. Từ 20 px trở lên và đủ rõ thì gán từng vật; dưới 20 px hoặc không tách được cụm thì dùng vùng ignore nhỏ hợp lý. |
| R-06 | Chỉ gán vật thật nhìn trực tiếp. Bỏ hình in, màn hình, tượng, ma-nơ-canh, đồ chơi và phản chiếu. |
| R-07 | Bỏ phần xe chủ chỉ lộ theo góc camera như nắp ca-pô hoặc gương. Tiếp tục xét các vật thật khác trong cảnh. |
| R-08 | Phân biệt car/van/truck bằng kiểu thân xe. Khi thiếu dấu hiệu, ghi review. |
| R-09 | Chặn box ở mép ảnh và xét truncated riêng với occlusion. |
| R-10 | Ảnh tối hoặc mờ vẫn xét theo biên nhìn được. Loại bóng đổ, vệt phản sáng và quầng nhòe khỏi phần cần bao. |
| R-11 | Cảnh đông vẫn tách từng vật đủ rõ và đủ kích thước. Vùng ignore cần tránh phủ vật còn gán được. |
| R-12 | Vật ngoài bộ lớp được loại theo phạm vi. Vật còn nghi ngờ được ghi review, kèm lý do. |

## 4. Ca khó và cách xử lý

| Ảnh | Tình huống | Quyết định áp dụng | Rule ID và lý do |
|---|---|---|---|
| `img_005.jpg` | Người và xe máy che nhau. | Tách từng người và từng xe nhận ra được; box bám phần nhìn thấy. Xét mức che riêng cho từng vật. | R-01–R-03. Tránh gộp người với xe hoặc đoán thêm vật hoàn toàn khuất. |
| `img_026.jpg` | Hai người trên một xe trong hầm tối. | Cụm chính có hai person và một motorcycle nếu từng vật đạt điều kiện. Mờ/tối được xét khi chọn biên, mức che xét theo vật che thật. | R-03, R-10. Giữ đúng đơn vị từng vật. |
| `img_007.jpg` | Xe trong kho, sàn phản sáng, xe cắt mép. | Bao thân/bánh thấy được; bỏ vệt phản sáng trên sàn. Vật thực sự bị cắt mép nhận truncated=yes. | R-09, R-10. Tránh box rộng theo bóng hoặc phản sáng. |
| `img_018.jpg` | Chân dung in trên biển. | Bỏ chân dung in, tiếp tục rà người và xe thật trong cảnh. | R-06. Hình biểu diễn bị loại theo quy tắc vật thật. |
| `img_035.jpg` | Tuk-tuk chở hàng. | Loại tuk-tuk khỏi lớp phương tiện v1.0; xét riêng người thật và xe khác thuộc bộ lớp. | R-12. Giữ phạm vi lớp thống nhất. |
| `img_037.jpg` | Pickup có nắp cao gần xe buýt. | Pickup là truck khi thấy cabin/thùng riêng; bus ngoài phạm vi. Chưa rõ kiểu thân thì đánh review. | R-08, R-12. Phân loại theo cấu trúc xe. |

## 5. QC và phiên bản

- Trước khi xuất, rà bỏ sót, box trùng/gộp, lớp, độ sát biên và ba thuộc tính. Các ca mơ hồ được nhóm chốt trong calibration.
- Xuất COCO 1.0 và kiểm đủ 26 ảnh bằng `python scripts/validate_labels.py submission/labels/103.json`.
- Ghép IAA dùng IoU từ 0.5. Ngưỡng cặp box cao trung bình từ 50 px là 0.85, thấp hơn là 0.70. Mục tiêu IoU trung bình từ 0.85 và đồng thuận lớp từ 95%, báo kèm số cặp ghép được.
- QC duyệt đủ 40 ảnh và báo tỷ lệ lỗi theo ảnh/đối tượng kèm tử số, mẫu số. Ngưỡng ảnh lỗi là 2%; một ảnh lỗi trên 40 tương đương 2.5%.
- Reviewer xác nhận Hold khi thiếu bằng chứng, Rework khi vượt ngưỡng hoặc có lỗi lặp, Release khi đủ điều kiện. Số liệu được điền sau khi thực hiện.

| Phiên bản | Ngày | Nội dung |
|---|---|---|
| v1.0 | 2026-11-16 | Hoàn thiện định nghĩa lớp, thuộc tính, quy tắc và ca khó theo bản nhóm đã chốt; dùng mức che 30%/70%. Các bản trước được lưu trong lịch sử Git. |
