# Guideline gán nhãn — v0 cá nhân — PhungDanhKhoa_2A202603206

- Định danh thành viên `PhungDanhKhoa_2A202603206`
- Phân công gán độc lập 27 ảnh
- Ngày 2026-11-16
- Công cụ gán nhãn CVAT
- Định dạng xuất COCO 1.0
- Trạng thái bản nháp cá nhân để rà ảnh trước khi gửi riêng

Các mã ảnh được đối chiếu với tài liệu lab và bản nháp cục bộ trước đó. Quyết định dưới đây là đề xuất. Thành viên PhungDanhKhoa cần kiểm lại trên ảnh gốc và xác nhận hoặc sửa trước khi gửi.

## 1. Mục tiêu

Huấn luyện mô hình phát hiện người và phương tiện thuộc phạm vi taxonomy trên ảnh đường phố. Gán hộp chữ nhật 2D trong CVAT và xuất COCO 1.0. Bỏ sót người bị che làm giảm ví dụ huấn luyện cho tình huống khó. Gán người trên bảng quảng cáo hoặc gộp người và xe vào cùng một box tạo tín hiệu sai về lớp và biên đối tượng.

## 2. Lớp và thuộc tính

| Lớp | Tiêu chí nhận biết | Ví dụ | Phản ví dụ |
|---|---|---|---|
| `car` | Ô tô con hoặc SUV, gồm xe đỗ và xe đang được chở nếu đủ điều kiện. | Xe con trên sàn xe chở ô tô trong `img_002.jpg`. | Pickup có thùng hàng riêng thuộc `truck`. |
| `truck` | Xe tải có khoang hoặc sàn hàng tách biệt với cabin, gồm xe chở ô tô và pickup. | Xe chở ô tô trong `img_002.jpg`. | Xe có khoang hàng kín liền thân với cabin cần xét `van`. |
| `van` | Xe cỡ nhỏ, thân hộp và khoang hàng hoặc khoang hành khách phía sau liền thân với cabin. | Xe giao hàng thân liền có cửa khoang hàng phía sau. | Pickup giữ thùng hàng riêng dù có nắp cao, ca đối chiếu `img_037.jpg`. |
| `motorcycle` | Xe máy hoặc mô tô hai bánh có động cơ, gồm xe tay ga. | Xe máy trong ca người đi xe máy `img_005.jpg`. | Xe đạp và tuk-tuk ba bánh nằm ngoài phạm vi v0. |
| `person` | Người thật nhìn thấy trực tiếp trong cảnh, gồm người đi bộ và người trên xe. | Người đi xe máy trong `img_005.jpg`. | Chân dung in trên bảng quảng cáo trong `img_018.jpg`. |
| `ignore_region` | Vùng có vật thuộc phạm vi nhưng quá nhỏ hoặc quá dày để tách box theo R-04. | Cụm xe ở xa khi từng xe cao dưới 20 px. | Phản chiếu đứng riêng hoặc xe buýt nhận diện rõ. |

Giữ các lớp của taxonomy v0. Rà người thật nhìn thấy trên một phương tiện ngoài taxonomy theo tiêu chí `person`. Tuk-tuk và xe buýt là hai ứng viên thảo luận ở Pha 2 nếu nhóm muốn mô hình phát hiện riêng.

### Thuộc tính

- `occluded_level` đo tỷ lệ diện tích hình chiếu của đối tượng **trong khung ảnh** bị vật khác che. Ước lượng diện tích bị che chia cho tổng diện tích nhìn thấy và diện tích bị che trong khung. Với hai phần nhìn thấy ngắt quãng, tính phần đối tượng bị vật khác che ở giữa. Phần nằm ngoài khung được xử lý bằng `truncated`. Nhập thuộc tính này trong CVAT. Phím Q thao tác cờ riêng của CVAT.

  | Giá trị | Phần diện tích bị che |
  |---|---|
  | `0` | 0% |
  | `1` | Trên 0% đến hết 30% |
  | `2` | Trên 30% đến hết 70% |
  | `3` | Trên 70%, còn đủ dấu hiệu nhận diện lớp và xác định biên phần nhìn thấy |

- `truncated=yes` khi đường biên ảnh cắt qua đối tượng. Đặt `no` khi toàn bộ đối tượng nằm trong khung, kể cả khi đối tượng vừa chạm mép. Khi vừa bị che vừa bị cắt, điền cả hai thuộc tính.
- `can_review=true` khi có thể đặt box nhưng còn nghi vấn về lớp, biên hoặc mức che. Chọn giá trị tạm có căn cứ nhất và ghi tên ảnh, vị trí, điểm cần xem lại. Khi thiếu căn cứ để đặt box, ghi vấn đề cấp ảnh để nhóm thu vào `image_issues.csv`.
- `ignore_region` dùng hình chữ nhật và không yêu cầu thuộc tính đối tượng.

## 3. Quy tắc vẽ box

Sau khi nhóm khóa v1.0, tạo Task CVAT `PhungDanhKhoa_2A202603206` trong Project dùng bộ nhãn từ taxonomy đã chốt. Tải 27 ảnh trong `work/PhungDanhKhoa_2A202603206/images/`, mở Job và kiểm tra đủ 27 frame. Giữ cả frame không có đối tượng.

1. Chọn công cụ **Rectangle** và lớp tương ứng trong CVAT. Vẽ một hình chữ nhật cho mỗi đối tượng đủ điều kiện. Dùng lớp `ignore_region` cho vùng bỏ qua.
2. Chọn box trong bảng **Objects** rồi điền `occluded_level`, `truncated` và `can_review` theo mục 2. Với box mơ hồ, tick `can_review` ngay trên box đó.
3. Lưu thay đổi trong CVAT trước khi xuất.

| Mã | Quy tắc |
|---|---|
| R-01 | Dùng hình chữ nhật nhỏ nhất ôm các phần nhìn thấy trực tiếp của một đối tượng. Với hai phần nhìn thấy của cùng vật, bao cả hai trong một box. Mép ảnh cắt vật thì chặn box tại mép ảnh. Giữ phần gắn cố định, ví dụ gương xe thật. Loại bóng đổ, hàng hóa và vật rời khỏi biên đối tượng. |
| R-02 | Người lái, người ngồi sau và xe là các đối tượng riêng. Đánh giá từng đối tượng theo lớp, độ nhìn thấy và ngưỡng kích thước. Hai box có thể chồng lên nhau. Phần người bao thân, chi và trang phục nhìn thấy. Phần xe bao các bộ phận của xe. |
| R-03 | Xe chở xe dùng box riêng cho xe chở, gồm sàn hoặc khung chở gắn với xe. Mỗi xe được chở có box riêng khi nhận diện được và đạt ngưỡng. Đo mức che của từng xe được chở riêng. |
| R-04 | Đo chiều cao phần nhìn thấy theo pixel ảnh gốc. Từ 20 px trở lên, gán từng vật nếu nhận diện lớp và tách được biên. Với vật thuộc phạm vi cao dưới 20 px hoặc cụm dày không thể tách từng vật, vẽ vùng `ignore_region` nhỏ nhất bao phần cần bỏ qua. Cố gắng giữ các vật đủ điều kiện ở ngoài vùng này và ghi vấn đề cấp ảnh nếu phạm vi khó xác định. |
| R-05 | Chỉ gán người và phương tiện thật nhìn thấy trực tiếp. Bỏ qua hình in, tượng, ma-nơ-canh, mô hình, ảnh trên màn hình và ảnh phản chiếu. Gán vật thật xuất hiện trực tiếp một lần dù có thêm ảnh phản chiếu. Bỏ qua xe chủ khi chỉ thấy phần gắn với góc camera như nắp ca-pô hoặc gương. |
| R-06 | Phân biệt lớp theo kiểu thân nhìn thấy. Cabin có thùng riêng là `truck` kể cả có nắp thùng. Khoang kín liền thân dạng hộp là `van`. Nếu biên giữa hai lớp chưa rõ nhưng vẫn vẽ được box, đặt `can_review` và ghi dấu hiệu cần đối chiếu. Xe buýt, tuk-tuk và xe không động cơ nằm ngoài phạm vi hiện tại. |

## 4. Ca khó

| Ảnh | Tình huống | Mã | Quyết định đề xuất | Lý do |
|---|---|---|---|---|
| `img_005.jpg`, đối chiếu `img_026.jpg` | Người trên xe máy, người và xe có thể bị che hoặc cắt mép khác nhau. | R-01, R-02 | Dùng box `person` và `motorcycle` riêng cho từng vật đủ điều kiện. Áp dụng cùng cách tách ở cả hai ảnh, đánh giá che và cắt mép từng box. | Phần xe nhìn được và phần người nhìn được có biên riêng. |
| `img_002.jpg` | Ô tô trên hai tầng của xe tải. | R-03, R-04 | Gán `truck` cho xe chở. Xét riêng từng ô tô trên hai tầng và gán `car` nếu nhận diện được, đủ 20 px. | Phương tiện được chở vẫn là đối tượng đích, kể cả khi đứng yên. |
| `img_018.jpg` | Chân dung in trên bảng quảng cáo. | R-05 | Bỏ qua chân dung. Rà riêng người thật trong cảnh. | Hình in có dáng người nhưng không phải người tham gia cảnh đường phố. |
| `img_020.jpg` | Ô tô xuất hiện qua gương chiếu hậu. | R-05 | Bỏ qua ảnh trong gương và phần gương của xe chủ. | Gương chứa hình phản chiếu và thuộc góc nhìn từ xe chủ. |
| `img_027.jpg` | Tượng xe bán tải trên bệ. | R-05 | Bỏ qua tượng dù hình dạng giống pickup. | Bệ trưng bày là dấu hiệu vật được dựng để trưng bày. |
| `img_035.jpg` | Tuk-tuk chở bao tải trên nóc. | R-01, R-06 | Tạm loại xe khỏi v0 theo phạm vi lớp. Nếu nhóm thêm lớp tuk-tuk, đề xuất box ôm thân xe và giá cố định, loại bao tải. | Bao tải có kích thước thay đổi độc lập với thân xe. |
| `img_037.jpg` | Pickup gắn nắp cao có dáng gần giống van. | R-06 | Chọn `truck` nếu nhìn thấy khoang hàng tách cabin. Nếu đường tách chưa rõ, đặt `can_review` và đưa ca ra họp. | Nắp thùng có thể che mất dấu hiệu phân biệt kiểu thân xe. |

## 5. QC và phiên bản

- Trong Task CVAT, chọn **Actions → Export task dataset → COCO 1.0** và tắt **Save images**. Tải bản xuất ở **Requests**, giải nén và lấy JSON trong thư mục `annotations/`. Lưu thành `submission/labels/PhungDanhKhoa_2A202603206.json`.
- Chạy `python scripts/validate_labels.py submission/labels/PhungDanhKhoa_2A202603206.json` từ thư mục gốc. Trong CVAT, đối chiếu ảnh gốc để rà box sát biên phần nhìn thấy, nhãn sai lớp, box trùng và vật bỏ sót.
- IAA dùng ghép một-một theo IoU ≥ 0,50 và tính IoU trung bình cùng đồng thuận lớp trên cặp ghép được. Mục tiêu IoU trung bình ≥ 0,85 và cùng lớp ≥ 95%. Ngưỡng IoU theo cặp là 0,85 khi chiều cao trung bình của hai box ≥ 50 px và 0,70 khi thấp hơn. Ghi mẫu số và thống kê box chưa ghép riêng.
- QC ghi số ảnh có lỗi trên số ảnh đã duyệt và số annotation hiện có bị lỗi trên số annotation đối tượng đã duyệt. Đếm vật bỏ sót riêng. Release cần duyệt đủ lô, tỷ lệ ảnh lỗi ≤ 2%, không có lỗi lặp theo tiêu chí của lab và đủ bằng chứng hiện hành.
- Thành viên PhungDanhKhoa chuyển ca chưa rõ cho người điều phối để đưa vào họp thống nhất. Sau khi nhóm khóa v1.0, ca phát sinh đi vào review và calibration.

| Phiên bản | Quy tắc bổ sung | Ảnh hoặc lý do |
|---|---|---|
| v0 cá nhân | Định nghĩa lớp theo kiểu thân, ngưỡng che định lượng, quy tắc gán phần nhìn thấy và xử lý vật ngoài phạm vi. | `img_002.jpg`, `img_005.jpg`, `img_018.jpg`, `img_020.jpg`, `img_026.jpg`, `img_027.jpg`, `img_035.jpg`, `img_037.jpg`. |
