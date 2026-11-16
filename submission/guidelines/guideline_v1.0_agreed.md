# Guideline gán nhãn chung — v1.0

- Ngày tổng hợp: 2026-11-16.
- Phạm vi: 40 ảnh `data/images/img_001.jpg`–`img_040.jpg`; bounding box 2D; CVAT; COCO 1.0.
- Nguồn: ba bản cá nhân được giữ nguyên trong [individual/](individual/), đối chiếu tại commit 1090cd3d7c917d8c2a079b1a65e617d2e97a0194.
- Tổng hợp kỹ thuật theo yêu cầu người điều phối NguyenCongQuy; không coi việc tổng hợp tự động là bằng chứng cả ba người đã họp hoặc xác nhận.
- Trạng thái: bản quy tắc chung đã soạn và đồng bộ taxonomy; cả ba người cần đọc, thống nhất trước khi bắt đầu lượt gán độc lập. Chưa ghi nhận xác nhận của từng người. Khi bắt đầu Pha 3, khóa bản này và `submission/taxonomy.json`; thay đổi phát sinh xử lý ở calibration, không âm thầm sửa v1.0.
- Tài liệu có thẩm quyền về quy trình: [GUIDE.md](../../GUIDE.md), [RULES.md](../../RULES.md), [RUBRIC.md](../../RUBRIC.md).

## 1. Mục tiêu và phạm vi

Tạo nhãn huấn luyện mô hình phát hiện người thật và phương tiện thật thuộc các lớp đã chốt trên ảnh đường phố. Guideline phải giúp ba annotator đặt cùng loại box, hiểu cùng mức thuộc tính và ghi lại bất định.

Đầu ra của lượt này chỉ gồm box chữ nhật, lớp và ba thuộc tính. Không thêm polygon, segmentation, keypoint hoặc track ID. Không gộp người với xe thành một vật. Không thay đổi ảnh gốc, bảng phân công hoặc các ngưỡng trong mục `rules` của taxonomy.

Nhãn sai trên hình in/tượng gây box thừa; gộp nhiều người hoặc suy diễn phần khuất tạo sai lệch vị trí/lớp. IAA cao không chứng minh cả nhóm đúng: mọi người vẫn có thể cùng hiểu sai. Bản cá nhân là bằng chứng quyết định ban đầu, không sửa lại để giống bản chung.

## 2. Lớp và thuộc tính

### 2.1. Lớp — mỗi instance đích đúng một lớp

| Lớp | Định nghĩa áp dụng | Ví dụ | Phản ví dụ / ranh giới |
|---|---|---|---|
| `car` | Ô tô con/SUV chở người, không thuộc dạng thân hộp của van hay xe có thùng hàng riêng; gồm xe đỗ và xe được chở. | Ô tô trên xe chở trong `img_002.jpg`. | Pickup có thùng riêng → truck; bus ngoài phạm vi. |
| `truck` | Xe tải có sàn/thùng hàng tách cabin, gồm xe chở ô tô và pickup. Thùng có nắp cao không tự đổi xe thành van. | Xe chở trong `img_002.jpg`; pickup `img_037.jpg` khi thấy kết cấu thùng riêng. | Xe khoang kín liền thân dạng hộp → van; tượng xe không gán. |
| `van` | Xe cỡ nhỏ thân hộp, khoang hàng/hành khách kín phía sau liền thân với cabin; không có thùng pickup tách cabin. | Xe van thân hộp phía sau người đi xe trong `img_005.jpg`. | SUV → car; pickup gắn nắp thùng → truck; thiếu dấu hiệu phân biệt → review. |
| `motorcycle` | Xe máy/mô tô hai bánh có động cơ, gồm xe tay ga; người ngồi trên xe là instance riêng. | Xe máy trong `img_005.jpg`, `img_026.jpg`. | Xe đạp, xích lô, tuk-tuk ba bánh ngoài phạm vi v1.0. |
| `person` | Người thật hiện diện trực tiếp trong cảnh, gồm đi bộ, đứng, ngồi và người trên phương tiện, nếu đủ điều kiện kích thước/biên. | Hai người trên xe máy trong `img_026.jpg`. | Người in ở `img_018.jpg`, tượng, ma-nơ-canh, ảnh phản chiếu không gán như người thật. |
| `ignore_region` | Vùng có instance thuộc phạm vi nhưng dưới ngưỡng kích thước hoặc cụm quá dày không tách được đáng tin cậy; đây là vùng, không phải loại vật. | Vùng xe/người xa khi từng vật cao <20 px. | Vật thật ngoài taxonomy, poster, tượng hoặc mọi vùng tối không tự là ignore. |

**Vật ngoài taxonomy:** giữ sáu lớp trên, không thêm `rider`, `bus`, `tuk_tuk` hoặc lớp khác trong lượt v1.0. Loại có chủ đích xe buýt, tuk-tuk, xe đạp/xích lô khỏi bộ lớp phương tiện hiện tại; vẫn xét riêng người thật trên chúng. Không ép sang car/truck/motorcycle và không phủ ignore chỉ vì ngoài taxonomy. Nếu chưa biết vật thuộc hay ngoài phạm vi, ghi review/vấn đề cấp ảnh. Mở rộng lớp chỉ là đề xuất cho lần chốt phiên bản khác.

### 2.2. Thuộc tính bắt buộc của các lớp đối tượng

Mỗi box đối tượng phải có `occluded_level`, `truncated`, `can_review` như [taxonomy](../taxonomy.json). Riêng `ignore_region` không yêu cầu thuộc tính đối tượng. Giá trị mặc định của Project không thay cho việc kiểm từng box.

**`occluded_level` — quy ước định lượng của v1.0:**

Gọi r là tỷ lệ diện tích hình chiếu của vật bị **vật khác che** trong khung ảnh: phần bị che chia cho tổng phần nhìn thấy và phần bị che trong khung. Ước lượng theo hình dạng vật; không tính tỷ lệ nền trong box, không dùng chiều cao phần nhìn thấy để thay diện tích. Phần ở ngoài ảnh thuộc truncation. Không bắt annotator vẽ mask/amodal box để tính r; đây là ước lượng có lý do, không phải phép đo chính xác từ visible bbox.

| Giá trị | Ngưỡng | Diễn giải |
|---|---|---|
| `0` | r = 0% | Không bị vật khác che. |
| `1` | 0% < r <= 30% | Che ít. Đúng 30% thuộc mức 1. |
| `2` | 30% < r <= 70% | Che vừa/nhiều. Đúng 70% thuộc mức 2. |
| `3` | r > 70%, còn phần nhìn thấy đủ điều kiện gán | Che nặng; KHÔNG có nghĩa “không biết”. |

Các mốc 30/70 là quy ước của bản chung, lấy phương án định lượng từ bản Khoa; không tuyên bố đây là chuẩn quốc tế. Nếu ước lượng được nhưng sát ngưỡng, chọn mức có căn cứ nhất, bật `can_review` và ghi lý do. Nếu hoàn toàn không đủ căn cứ chọn mức hoặc không đặt được box đáng tin cậy, ghi vấn đề cấp ảnh để review; không ép `3` thành mã unknown hay tạo dữ kiện giả. Vật không còn phần nhìn thấy không được gán chỉ nhờ suy đoán.

**`truncated`:** `yes` khi mép ảnh thực sự cắt qua vật; `no` khi vật nằm trọn trong ảnh, kể cả vừa chạm mép. Vừa bị che vừa bị cắt: điền cả hai trạng thái độc lập.

**`can_review`:** `true` khi vẫn đặt được box có căn cứ nhưng còn nghi vấn lớp, biên, mức che, nguồn ảnh thật/giả hoặc phạm vi ignore. Ghi ảnh, vị trí, điểm chưa rõ và lý do; không dùng để thay toàn bộ công việc gán. Không vẽ được box hoặc không có lớp hợp lệ để gán tạm: ghi `image,annotator,region,note` riêng, thu vào `submission/qc/image_issues.csv` sau khi khóa lượt nhãn. Mỗi annotator giữ ghi chú riêng đến khi nộp, tránh trao đổi đáp án trong lượt độc lập.

**Lưu ý CVAT:** thuộc tính `occluded_level` phải điền trong Details. Phím Q/cờ Occluded sẵn của CVAT là trường khác, checker lab bỏ qua; chỉ bật Q không đủ.

## 3. Quy tắc vẽ box

| Rule ID | Quy tắc dùng chung |
|---|---|
| R-01 | Box chữ nhật nhỏ nhất bao toàn bộ các phần nhìn thấy xác định được của cùng instance. Không suy diễn chân/tay/đuôi xe bị khuất. Nếu phần nhìn thấy rời nhau nhưng chắc cùng vật, dùng một box bao các phần đó. |
| R-02 | Mỗi người/xe đủ điều kiện là một instance riêng. Box khác vật được phép chồng; không thu nhỏ/cắt box để né chồng. Vùng nền/vật che có thể nằm trong chữ nhật, không có nghĩa thuộc instance đó. |
| R-03 | Tách người lái, từng người ngồi sau và xe. Một người + một xe: một person và một motorcycle; hai người + một xe: hai person và một motorcycle, nếu từng instance đủ điều kiện. Box người bao chi/trang phục/mũ đang đội nhìn thấy; box xe bao phần xe, không cố tình bao người. |
| R-04 | Xe chở xe: box xe chở gồm cabin và sàn/khung chở gắn cố định nhìn thấy; từng xe được chở có box riêng nếu đủ điều kiện. Không kéo box xe chở theo xe/hàng hóa xếp bên trên. Gương, bánh, nắp thùng gắn cố định thuộc xe; bao tải, hàng rời và xe được chở không thuộc silhouette xe chở. |
| R-05 | Đo chiều cao box phần nhìn thấy bằng pixel ảnh gốc, không bằng mức zoom. Cao >=20 px, xác định được lớp/biên: gán từng vật. Cao <20 px hoặc cụm thuộc phạm vi không tách được: ignore_region nhỏ nhất hợp lý. Cố giữ vật đủ điều kiện ngoài ignore; nếu buộc phủ vào vật đủ rõ, phải ghi ca cho review, không dùng ignore để che lỗi bỏ sót. |
| R-06 | Chỉ gán vật thật trực tiếp. Loại poster, hình in, ảnh trong màn hình, tượng/ma-nơ-canh/mô hình và ảnh phản chiếu; không phủ ignore cho mỗi vật bị loại. Ảnh đen trắng, xe cũ/lạ, xe đứng yên hoặc đang được chở không tự là vật giả. Không loại chỉ vì vật nằm trên bệ; ca chưa đủ bằng chứng phải review. |
| R-07 | Xe chủ/ego chỉ lộ phần gắn với góc camera như nắp ca-pô, gương: không gán phần ấy. Vật thật khác nhìn trực tiếp vẫn xét độc lập. Nếu cùng vật có ảnh phản chiếu, chỉ xét instance trực tiếp; không tạo thêm box cho bản phản chiếu. |
| R-08 | Phân lớp phương tiện theo kiểu thân như mục 2, không theo màu/chữ trên xe, hàng chở hoặc chỉ kích thước ảnh. Pickup có thùng riêng → truck dù có nắp; không thấy đủ dấu hiệu để phân biệt → review/vấn đề cấp ảnh, không tự thêm lớp. |
| R-09 | Vật khác che và mép ảnh cắt là hai hiện tượng riêng. Box chặn tại mép ảnh; không kéo ra ngoài ảnh để hoàn thiện hình vật. Không dùng mã che thay cho cắt mép. |
| R-10 | Mờ/tối là chất lượng ảnh, không tự là occlusion. Phóng to/đổi hiển thị để xem nếu cần, giữ nguyên dữ liệu gốc. Chọn biên vật hỗ trợ được; không bao bóng đổ, vệt phản sáng hoặc toàn bộ quầng nhòe. Còn bất định: review; không đủ căn cứ: vấn đề cấp ảnh. Không dùng chi tiết AI tái tạo làm bằng chứng. |
| R-11 | Khung cảnh đông vẫn gán từng instance tách được và đạt ngưỡng; không gộp nhiều xe/người vào một box đối tượng. Chỉ dùng ignore theo R-05, có vùng và lý do. |
| R-12 | Vật ngoài phạm vi loại theo mục 2; vật có thể thuộc phạm vi nhưng chưa xác định được đưa review. Hai tình huống khác nhau, phải ghi được lý do; không dùng một class “gần giống” để vượt validator. |

Khi một chiếc xe che người phía sau, phần xe phía trước không tự bị occluded vì nó là vật che. Đánh giá từng instance riêng. Kính trong suốt không tự là vật che nếu chi tiết phía sau vẫn nhìn thấy; không chắc đường nhìn trực tiếp hay phản chiếu thì review.

## 4. Ca khó và đối chiếu ba bản

### 4.1. Ca ảnh tham chiếu cho quy tắc

Các hàng là phương án trong đặc tả nhóm được tổng hợp/kiểm tra từ bản cá nhân và ảnh minh họa; không phải kết quả gán, số box toàn ảnh hoặc dữ liệu QC đã đo.

| Ảnh | Tình huống | Rule ID | Cách áp dụng v1.0 | Lý do |
|---|---|---|---|---|
| `img_005.jpg`, `img_026.jpg` | Người đi xe máy che nhau; thử cùng quy tắc trên hai ảnh. | R-01–R-03, R-09 | Tách từng người và xe đạt điều kiện. Ở cụm chính img_026: xét hai person và một motorcycle. Không suy diễn instance hoàn toàn khuất; đánh che/cắt mép từng vật. | Tránh gộp người với xe hoặc dùng amodal box ngoài yêu cầu. |
| `img_002.jpg` | Ô tô trên hai tầng xe chở. | R-04, R-05, R-08 | Truck cho xe chở; xét riêng từng xe con đủ điều kiện, gồm xe đỗ/được chở. Khung chở thuộc truck, ô tô không làm nới silhouette truck. | Xe được chở vẫn là instance đích. |
| `img_018.jpg` | Chân dung in trên biển; xe thật phía dưới. | R-05, R-06 | Loại chân dung in; vẫn rà xe/người thật, kể cả bị hàng rào che. | Không bỏ cả ảnh chỉ vì có poster. |
| `img_020.jpg` | Ảnh qua gương và gương của xe chủ. | R-06, R-07 | Không gán phương tiện trong phản chiếu hoặc phần gương ego. Vẫn giữ ảnh trong COCO dù không có đối tượng đủ điều kiện. | Phản chiếu không tạo thêm instance trực tiếp. |
| `img_027.jpg` | Ca tượng/đài tưởng niệm hình pickup. | R-06 | Loại hình tượng trong ca này. Không suy rộng thành mọi xe trên bệ đều là tượng; chưa chắc vật thật hay mô hình thì review. | Quyết định dựa bản chất biểu diễn, không chỉ vị trí đỗ. |
| `img_035.jpg` | Tuk-tuk chở hàng, cạnh các phương tiện/người khác. | R-04, R-12 | Tuk-tuk ngoài sáu lớp v1.0; không ép motorcycle/truck. Người thật và xe khác thuộc lớp vẫn xét riêng. | Giữ phạm vi nhất quán; hàng rời không phải phần xe. |
| `img_037.jpg` | Pickup có nắp cao cạnh xe buýt. | R-08, R-12 | Pickup giữ truck khi thấy cabin/thùng riêng; bus ngoài phạm vi. Chưa chắc kết cấu → can_review. | Nắp cao không tự đổi pickup thành van. |
| `img_007.jpg` | Kho tối, sàn phản sáng, xe cắt mép. | R-09, R-10 | Box bám thân/bánh/chi tiết xe thấy được; không bao bóng/phản sáng. Cắt mép thật → truncated=yes; tối không tự là che. | Tách ba loại nguyên nhân gây khó. |

### 4.2. Tổng hợp khác biệt và lựa chọn kỹ thuật

P = bản Phong; K = bản Khoa; Q = bản nghiên cứu của Quý. Q và K còn ghi trạng thái đề xuất; bảng sau không giả định tác giả đã phê duyệt mọi dòng. Đây là lựa chọn tổng hợp để nhóm đọc/chốt, không phải biên bản cuộc họp đã diễn ra.

| Vấn đề | Nội dung đầu vào | Lựa chọn bản chung và lý do |
|---|---|---|
| Phạm vi lớp | P: tên lớp cơ bản; K: định nghĩa kiểu thân và loại phương tiện ngoài lớp; Q: giữ schema, còn điểm cần bổ sung. | Giữ sáu lớp, dùng định nghĩa kiểu thân K và ghi ngoài phạm vi rõ. Tránh đổi lớp tùy người. |
| Box khuất/chồng | P: không suy đoán, box độc lập; K/Q: bao các phần nhìn thấy, chấp nhận box chồng. | R-01/R-02 giữ điểm chung, làm rõ box chữ nhật có thể chứa nền. |
| Người trên xe | P: một vật một box nhưng chưa quy định số; K/Q: tách người và xe. | R-03 quy định số instance có điều kiện, không gộp hoặc thêm rider. |
| Che khuất | P/v0: mức chưa có ngưỡng; K: 30/70, mức 3 che nặng; Q: 50%, mức 3 unknown. | Chọn K 30/70 để đủ bốn mức định lượng. Không trộn hai nghĩa của mức 3; bất định dùng review/vấn đề cấp ảnh. |
| Vật nhỏ | P: còn nhận ra thì gán, chưa có mốc; K/Q: 20 px theo lab. | R-05 giữ ngưỡng bắt buộc của lab, làm rõ đúng 20 px được xét gán. |
| Vật thật/hình in | P: nhấn mạnh vật thật, ảnh đen trắng vẫn gán; K/Q: thêm tượng/phản chiếu/ego. | R-06/R-07 kết hợp, không lấy màu hoặc riêng chiếc bệ làm bằng chứng loại. |
| Vật ngoài lớp | K: bus/tuk-tuk không thuộc v0; Q: cần chốt phạm vi. | Giữ ngoài v1.0, vẫn rà person trên chúng; không biến vật ngoài taxonomy thành ignore. |
| Không đặt được box | P: can_review cho ca mơ hồ; K/Q: phân biệt box mơ hồ với vấn đề cấp ảnh. | Bổ sung đường ghi image_issues; không ép lớp/mã hoặc tạo box khi thiếu bằng chứng. |
| Mờ/tối | P: chất lượng ảnh không loại vật thật; Q: tách khỏi occlusion, bóng và quầng nhòe. | R-10 giữ vật có bằng chứng, chuyển ca không chắc review, không dựng chi tiết giả. |
| QC | P: rà bỏ sót/thừa/tách vật; K/Q: thêm mẫu số, IAA, duyệt toàn lô. | Kết hợp checklist với ngưỡng/script chính thức, không tự tạo kết quả. |

## 5. QC, phiên bản và bàn giao sang lượt gán độc lập

### 5.1. Cổng bắt đầu Pha 3

1. Cả ba người đọc mục 2–4 và xác nhận cùng hiểu phạm vi sáu lớp, visible bbox, người/xe và mã che 30/70. Không ghi thay xác nhận hoặc kết quả thử của người chưa thực hiện.
2. Cùng giải thích R-03 trên img_005 và img_026 trước lượt độc lập. Đây là kiểm hiểu quy tắc ở Pha 2; sau khi khóa lượt gán, không xem/trao đổi nhãn của nhau.
3. Ghi mốc chốt/commit vào ghi chú nhóm khi đã thật sự xác nhận. Giữ v1.0 và taxonomy cố định cho cả lượt. Không có bằng chứng nhóm đã chốt thì chưa tuyên bố hoàn thành Pha 2.
4. `python scripts/make_cvat_labels.py` → [labels_generated.json](../../cvat/labels_generated.json). Cấu hình này sinh từ taxonomy v1.0, không sửa tay JSON CVAT.
5. Tạo Project CVAT dùng bộ lớp trên; mỗi người tạo Task riêng, tải đúng `work/<Ten_MSHV>/images/`. Phong 27, Khoa 27, Quý 26 ảnh, theo [assignment.csv](../qc/assignment.csv). Giữ frame không có vật; không upload cả 40 ảnh vào mỗi Task gán độc lập.

Tất cả đang chờ guideline theo thông tin người điều phối, chưa có nhãn được ghi nhận cho lượt này. Không cần xóa/tạo lại nhãn. Nếu phát hiện Task đã có box theo quy tắc khác, lưu bản cũ riêng, đối chiếu khác biệt và rà trước khi coi là nhãn theo v1.0.

### 5.2. Tự kiểm và xuất nhãn

- Rà từng ảnh: đủ instance thuộc phạm vi, box sát phần nhìn thấy, không gộp/trùng, không gán hình in/tượng, thuộc tính đúng; ignore không che vật đủ rõ.
- Save trong CVAT; export **COCO 1.0**, tắt Save images. Giải nén ZIP, lấy `annotations/instances_*.json`; không đổi đuôi ZIP thành JSON.
- Lưu `submission/labels/TranPhamHoangPhong_2A202603198.json`, `PhungDanhKhoa_2A202603206.json` hoặc `NguyenCongQuy_2A202603149.json` theo người làm.
- Mỗi người chạy `python scripts/validate_labels.py submission/labels/<Ten_MSHV>.json`. Script kiểm cấu trúc/lớp/thuộc tính/phân công, không chứng minh box đúng vật/không bỏ sót. Nhãn và ghi chú gửi riêng; tập hợp sau khi cả ba đã nộp.

### 5.3. Ngưỡng và quy trình sau khi nộp đủ

Giữ nguyên tất cả giá trị `rules` trong taxonomy. Các số sau là tiêu chí, không phải kết quả đã đo:

| Tiêu chí | Cách áp dụng |
|---|---|
| Ngưỡng vật nhỏ | chiều cao phần nhìn thấy <20 px theo R-05. |
| Ghép box IAA | ghép 1–1 không phân biệt lớp, IoU >=0.5; báo unmatched riêng. |
| Ngưỡng cặp box | chiều cao trung bình hai box >=50 px: IoU 0.85; thấp hơn: 0.70, đúng cách script hiện hành tính. |
| IAA | IoU trung bình >=0.85; đồng thuận lớp >=95% trên cặp ghép được; có tử/mẫu, không có cặp ghi N/A. |
| Độ phủ | đủ 80 lượt hợp lệ theo phân công hiện tại; không nhầm với tổng 40 ảnh. |
| QC bàn giao | duyệt toàn bộ 40 ảnh; ảnh lỗi/ảnh đã duyệt <=2%, không có lỗi lặp, đủ bằng chứng và reviewer xác nhận. Một ảnh lỗi/40 =2.5%, vượt ngưỡng. |
| Lỗi đối tượng | annotation hiện có bị lỗi/annotation đối tượng đã duyệt; báo vật bỏ sót riêng, không tự thêm vật bỏ sót vào mẫu số annotation hiện có. |

Khi cả ba đã nộp: chạy validate --all, qc_compare, viz_disagreement. Mỗi ca bất thường ghi quyết định calibration riêng, lý do, người chốt, bản sửa và kiểm lại. Tạo v1.1 cùng guideline_change.json đúng thay đổi thực; đổi nghĩa quy tắc cần rà phạm vi cả lô. Không sửa bản IAA trước calibration hoặc tạo nhãn/QC khi chưa làm.

Hold khi thiếu bằng chứng, chưa chốt quy tắc hoặc có vấn đề dữ liệu. Rework khi vượt ngưỡng hoặc lỗi lặp. Release chỉ khi reviewer xác nhận bằng chứng đủ và khớp bản nhãn hiện hành. Chưa có số đo, phiếu QC hoặc kết luận Release ở thời điểm tổng hợp guideline.

### 5.4. Lịch sử và xác nhận

| Phiên bản | Thay đổi | Nguồn/lý do |
|---|---|---|
| v0 | Bản khởi đầu: lớp ngắn, “ôm sát”, mức che chưa định lượng. | Starter của lab. |
| v1.0 | Định nghĩa sáu lớp, R-01–R-12, che 30/70, tách người/xe, vật nhỏ, ngoài taxonomy và đường review; đồng bộ taxonomy/nhãn CVAT. | Tổng hợp ba bản cá nhân tại 1090cd3d7c917d8c2a079b1a65e617d2e97a0194, giải quyết khác biệt ở mục 4.2; không thay ngưỡng lab. |

Xác nhận sử dụng trước lượt gán: chưa ghi nhận từng thành viên trong bản tổng hợp này. Người điều phối thu xác nhận thực tế rồi ghi ngày/commit trong ghi chú nhóm; không tự ghi “đã họp”, “cả nhóm đồng ý” hoặc số đo chưa có.
