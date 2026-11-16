# Guideline gán nhãn — v0.2-research-draft — NguyenCongQuy_2A202603149

- Phiên bản: v0.2-research-draft, bản cá nhân chưa chốt.
- Ngày soạn khung: 2026-11-16.
- Thành viên: NguyenCongQuy_2A202603149.
- Trạng thái: NguyenCongQuy xác nhận đã xem đủ 40 ảnh. Các quyết định cá nhân còn cần hoàn thiện ở những mục được đánh dấu.
- Cơ sở: TEMPLATE_guideline.md, guideline_v0_starter.md, GUIDE.md, RUBRIC.md, taxonomy.json và QC_CHEATSHEET.md của repo nhóm.

> Đã xem đủ 40 ảnh. Các mục “CẦN BỔ SUNG” ghi phần quyết định cá nhân cần hoàn thiện trước khi khóa guideline chung.

## 1. Mục tiêu

Tạo nhãn bounding box cho mô hình phát hiện người và phương tiện trên bộ 40 ảnh đường phố của lab. Công cụ gán: CVAT; định dạng trao đổi: COCO 1.0.

Guideline cần giúp người gán quyết định nhất quán: vật nào thuộc phạm vi, lớp nào phù hợp, box bao phần nào và thuộc tính nào áp dụng. Mỗi ca khó phải truy lại được ảnh và lý do quyết định.

Trong Pha 1, người viết xem đủ 40 ảnh, không chỉ 26 ảnh được phân công cho lượt gán độc lập. Bản cá nhân được giữ nguyên sau cuộc họp; quyết định chung ghi ở file v1.0 riêng.

## 2. Lớp và thuộc tính

### 2.1. Bộ lớp ban đầu và điểm phải làm rõ

Bảng dưới ghi cơ sở v0, chưa thay thế định nghĩa do người viết tự hoàn thiện.

| Lớp | Cơ sở trong taxonomy v0 | CẦN BỔ SUNG vào bản cá nhân |
|---|---|---|
| `car` | Ô tô con, SUV. | Tiêu chí phân biệt với van/truck; ID ảnh ví dụ và phản ví dụ. |
| `truck` | Xe tải, xe có thùng hàng riêng. | Cách xử lý pickup và xe chở xe; ID ảnh minh họa, lý do. |
| `van` | Xe thùng kín cỡ nhỏ. | Dấu hiệu phân biệt với xe con hoặc xe tải kín; ca không đủ bằng chứng. |
| `motorcycle` | Xe máy, mô tô. | Ranh giới box xe khi có người ngồi, nhiều người ngồi hoặc vật chở kèm. |
| `person` | Người đi bộ hoặc người ngồi trên xe. | Quan hệ với lớp xe; số box và phần được bao khi người ngồi trên xe máy. |
| `ignore_region` | Vùng bỏ qua việc gán từng vật. | Ranh giới vùng, điều kiện sử dụng và cách tránh bỏ qua vật còn nhận diện được. |

Vật ngoài taxonomy: CẦN BỔ SUNG quyết định cá nhân và ca ảnh thật. Phân biệt loại có chủ đích theo quy tắc với trường hợp chưa đủ bằng chứng cần reviewer. Chưa sửa taxonomy chung ở Pha 1.

### 2.2. Thuộc tính

| Thuộc tính | Giá trị hiện có | Nghĩa trong v0 / việc cần hoàn thiện |
|---|---|---|
| `occluded_level` | `0`, `1`, `2`, `3` | V0 mô tả thấy rõ / che ít / che nhiều / không rõ. CẦN BỔ SUNG ngưỡng cụ thể, cách ước lượng và xử lý đúng tại ranh giới mức. |
| `truncated` | `no`, `yes` | Vật bị cắt ở mép ảnh; không đồng nhất với bị vật khác che. CẦN BỔ SUNG cách xác định phần vật cần xét và ca ví dụ. |
| `can_review` | true / false | Đánh dấu box còn chưa chắc để review; phải ghi điểm chưa rõ và lý do. Không có box khả thi thì ghi vấn đề cấp ảnh thay vì tạo box tùy ý. |

Phương án ngưỡng đề xuất để người viết đánh giá (chưa khóa, không phải chuẩn phần trăm chung của ngành):

| Mức | Cách xác định đề xuất |
|---|---|
| `0` | Không có bằng chứng bị vật khác che phần hình chiếu cần xét. |
| `1` | Có che khuất; ước lượng phần bị che lớn hơn 0% nhưng nhỏ hơn 50%. |
| `2` | Ước lượng phần bị che từ 50% trở lên; vẫn còn phần nhìn thấy và đủ bằng chứng xác định instance. |
| `3` | Không thể ước lượng đáng tin cậy mức che khuất; bật thêm `can_review` và nêu lý do. Không dùng làm giá trị “che nhiều nhất”. |

Tỷ lệ che khuất là ước lượng phần silhouette bị vật khác che so với silhouette toàn vật dự kiến trong khung ảnh; không phải tỷ lệ nền trong box. Phần vật nằm ngoài ảnh xét riêng bằng `truncated`. Không cần vẽ mask hoặc suy diễn amodal box để tính tỷ lệ: nếu quan sát không đủ tin cậy, chọn mức `3` và review. Ca sát mốc 50% mà không phân định được cũng chuyển review.

Mờ/tối không tự động đồng nghĩa bị che. Nếu biết vật không bị che nhưng biên khó xác định: giữ mức che tương ứng, dùng `can_review` cho độ bất định. Vật hoàn toàn không nhìn thấy không được tạo box chỉ nhờ suy đoán.

CẦN XÁC NHẬN: người viết có chấp nhận cách ước lượng và mốc 50% này sau khi tự thử trên ảnh không. Chưa có số đo occlusion cho bất kỳ instance nào trong ba ảnh tham khảo.

## 3. Quy tắc vẽ box — phương án đề xuất

Các quy tắc sau là phương án cá nhân để đối chiếu mục tiêu lab và tài liệu tham khảo. Các điểm còn chưa chốt được ghi riêng để thảo luận nhóm.

| Rule ID | Phương án đề xuất và giới hạn |
|---|---|
| R-01 | Một instance, một box chữ nhật nhỏ nhất bao các phần nhìn thấy xác định được của instance đó. Không kéo cạnh tới vị trí chi bị che chỉ vì suy đoán. Phần nhìn thấy rời nhau vẫn thuộc cùng instance nếu có bằng chứng liên kết. |
| R-02 | Các box của hai vật có thể chồng nhau do hình học chữ nhật. Không cắt bớt box chỉ để tránh chồng. Nền hoặc vật phía trước có thể nằm bên trong hình chữ nhật; điều đó không đồng nghĩa chúng thuộc instance đang gán. |
| R-03 | Một người trên một xe máy: một `person` và một `motorcycle`; hai người trên một xe: hai `person` và một `motorcycle`, nếu từng instance đạt điều kiện gán. Box người bao phần người nhìn thấy và trang phục/mũ đang đội; box xe không cố tình mở rộng để bao người. Không tự thêm lớp `rider` vào taxonomy chung. |
| R-04 | `occluded_level` phản ánh vật khác che; `truncated=yes` khi bản thân vật tiếp tục ra ngoài mép ảnh, không chỉ vì box chạm mép. Hai trạng thái có thể cùng xuất hiện. |
| R-05 | Mờ/tối: xem ảnh gốc và phóng to để kiểm bằng chứng. Chỉ đặt cạnh theo biên hỗ trợ được; không bao bóng đổ hoặc quầng nhòe xa vật. Có box khả thi nhưng chưa chắc: `can_review`. Không xác định được lớp/biên đủ tin cậy: ghi ảnh, vùng và lý do vào vấn đề cấp ảnh; không tạo box giả. Giữ nguyên ảnh gốc và dựa vào chi tiết quan sát được khi đặt box. |
| R-06 | Tượng, hình in, đồ chơi và ảnh phản chiếu không tự động là vật thật cần phát hiện. Đối chiếu tiêu chí vật thật trong mục tiêu lab; ghi rõ lý do loại hoặc chuyển review. Không mặc định mọi ca bị loại đều phải phủ `ignore_region`. |
| R-07 | Vật có box phần nhìn thấy cao < 20 px: áp dụng quy tắc `ignore_region` của lab, giới hạn vùng hợp lý. Không gom cả đám đông nếu vẫn tách được các instance đạt điều kiện. |
| R-08 | Xe chở xe: xét xe mang hàng và từng xe được chở như các instance riêng nếu đủ bằng chứng, không gộp thành một box. Vật ngoài taxonomy phải ghi là ngoài phạm vi/chưa chốt, không ép sang lớp gần giống; đề xuất đổi taxonomy đưa ra Pha 2. |

BBox bao phần nhìn thấy là lựa chọn cho lab này, dựa trên khung đề. Không suy ra rằng mọi bộ dữ liệu khác cũng dùng quy tắc đó. Segmentation, keypoint và tracking là các nhiệm vụ khác; không thêm vào đầu ra COCO bbox của lab.

## 4. Ba ca khó tham khảo cần người viết tự đối chiếu

**Đã xem đủ 40 ảnh. Ba ảnh dưới đây được dùng làm ca tham khảo để trình bày các điểm cần thống nhất về box, lớp và thuộc tính.**

| Ảnh | Quan sát trực tiếp | V0 còn mơ hồ | Phương án xử lý tham khảo | Lý do |
|---|---|---|---|---|
| `img_005.jpg` | Nhiều người đội mũ ngồi trên các xe máy che nhau; phía sau có xe lớn. | Có gộp người với xe không; phần khuất có được suy diễn không; box có được chồng nhau không? | R-01–R-04: tách từng người và xe có thể phân biệt; bao phần nhìn thấy, giữ box chồng khi cần. Từng instance chỉ còn một phần quá ít/chưa phân định được phải review, không tự đoán số người/xe bị khuất. | Tránh gộp hai loại vật và tránh thu nhỏ box để né chồng. Không biến che khuất thành một instance mới. |
| `img_026.jpg` | Hai người ngồi cùng xe máy trong hầm; ánh sáng yếu và có nhòe, một số phần thân/xe bị người kia che; nền có hình in lớn. | Một người + xe hay các instance tách biệt? Mờ có phải occluded? Vật trong hình in có được gán như vật thật? | Với các instance chính: đề xuất hai box `person`, một box `motorcycle`, bám phần nhìn thấy; kiểm riêng mờ, che và cắt mép. Bật review khi cạnh không chắc; không coi hình in là vật thật. Không quyết định phần trăm che chỉ từ tên file. | Mỗi người là một instance; nhòe là chất lượng ảnh, không phải mức che. Người viết cần kiểm lại từng cạnh trước khi chốt. |
| `img_007.jpg` | Xe trong kho, vùng dưới xe tối và sàn có phản sáng; xe bên phải tiếp tục ra ngoài khung ảnh. | Bóng/phản sáng có thuộc box xe không; tối có phải bị che; chạm mép có đồng nghĩa truncated? | R-01, R-04, R-05: đặt cạnh theo thân/bánh/chi tiết thuộc xe nhận ra được; không bao vệt phản sáng trên sàn. Xe thực sự bị khung ảnh cắt đánh truncated; vùng tối khó chắc dùng review, không kéo box xuống theo bóng. | Tách hình học cắt mép khỏi điều kiện ánh sáng; tránh box phình do bóng và phản chiếu. |

CẦN NGƯỜI VIẾT XÁC NHẬN cho từng hàng: đồng ý / sửa quyết định / chưa chốt, cùng lý do quan sát. Chưa ghi tọa độ box, mức occlusion cụ thể hay kết quả QC cho các ảnh này.

## 5. QC và phiên bản

### 5.1. Ngưỡng có sẵn trong lab

Các số dưới đây lấy từ taxonomy và QC_CHEATSHEET, không phải số đo của bài làm:

- Mốc vật nhỏ: chiều cao < 20 px; áp dụng `ignore_region` theo quy tắc vật nhỏ đã làm rõ.
- Ngưỡng so box: cao >= 50 px dùng IoU 0.85; thấp hơn dùng 0.7.
- Ghép cặp IAA: 1–1, IoU ghép >= 0.5; báo box chưa ghép riêng.
- IAA mục tiêu: IoU trung bình >= 0.85 và đồng thuận lớp >= 95%, tính trên các cặp box ghép được; không có cặp ghi N/A.
- Độ phủ: phải đủ lượt theo bảng phân công trước khi đo IAA.
- QC bàn giao: duyệt đủ 40 ảnh; tỉ lệ ảnh lỗi <= 2% và không có lỗi lặp mới đủ điều kiện xét Release. Một ảnh lỗi trên 40 là 2.5%, vượt ngưỡng.
- Báo cả tỉ lệ lỗi theo ảnh và theo đối tượng, có tử số/mẫu số. Script không tự chứng minh box đúng vật hoặc ôm sát.
- Hold khi thiếu bằng chứng hoặc guideline/dữ liệu còn vấn đề. Rework khi vượt ngưỡng hoặc có lỗi lặp. Reviewer xác nhận quyết định cuối bằng bằng chứng thực tế.

### 5.2. Ca mơ hồ và lịch sử

- Pha 1: giữ quyết định ban đầu hoặc ghi chưa chốt; trao đổi khi cả ba bản cá nhân đã nộp riêng.
- Pha 2: nhóm chốt v1.0 và taxonomy khớp; không sửa bản cá nhân để che mất bất đồng ban đầu.
- Pha 3: giữ v1.0 trong lượt gán độc lập; ghi `can_review`/vấn đề cấp ảnh.
- Chưa thực hiện: đo IAA, calibration, duyệt QC hoặc kết luận Release.

| Phiên bản | Thay đổi | Cơ sở và trạng thái |
|---|---|---|
| v0.1-draft | Dựng khung 5 phần theo đề. | Chưa có ca ảnh và quyết định cá nhân. |
| v0.2-research-draft | Thêm tài liệu tham khảo, quy tắc đề xuất và ba ca minh họa. | NguyenCongQuy đã xác nhận xem đủ 40 ảnh; các quyết định còn đánh dấu cần hoàn thiện. |

Tài liệu đối chiếu trong repo: [GUIDE](https://github.com/nguyencongquy/K5-L23-Day07-Folked-Guideline-Taxonomy-QC/blob/main/GUIDE.md), [mẫu](https://github.com/nguyencongquy/K5-L23-Day07-Folked-Guideline-Taxonomy-QC/blob/main/guideline/TEMPLATE_guideline.md), [rubric](https://github.com/nguyencongquy/K5-L23-Day07-Folked-Guideline-Taxonomy-QC/blob/main/RUBRIC.md), [QC](https://github.com/nguyencongquy/K5-L23-Day07-Folked-Guideline-Taxonomy-QC/blob/main/docs/QC_CHEATSHEET.md).

Bản nghiên cứu và nguồn ngoài: [Nghiên cứu quy tắc annotation](../../../docs/research/day07_annotation_rules.md).
