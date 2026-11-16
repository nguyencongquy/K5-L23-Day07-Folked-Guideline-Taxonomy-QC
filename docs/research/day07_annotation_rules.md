# Nghiên cứu tham khảo — chọn quy tắc annotation cho Day 07

Ngày nghiên cứu: 2026-11-16. Tài liệu so sánh các quy tắc annotation và nguồn tham khảo. Khi gán nhãn, áp dụng guideline chung và taxonomy của phiên bản được nhóm chốt.

## 1. Kết luận phù hợp với bài

Bài hiện tại là bounding box 2D, xuất COCO 1.0. Đề và rubric không yêu cầu bổ sung segmentation, keypoint hoặc tracking vào 40 ảnh. Giữ phạm vi này giúp đo IAA cùng một loại nhãn.

Thứ tự quyết định: mục tiêu lab và quy tắc bắt buộc → phương án cá nhân có lý do → thảo luận nhóm chốt v1.0 → gán độc lập → calibration. Tài liệu bên ngoài giúp so sánh thiết kế; không tự thay mục rules của taxonomy hoặc coi thông lệ dataset khác là yêu cầu của giảng viên.

## 2. Nguồn gốc và điều thực sự được xác nhận

| Nguồn chính | Nội dung xác nhận | Hệ quả cho lựa chọn của nhóm |
|---|---|---|
| [CityPersons — mã chính thức](https://github.com/mcordts/cityscapesScripts/blob/master/cityscapesscripts/helpers/labels_cityPersons.py) | Có cả bbox toàn thân và bbox phần nhìn thấy; có lớp rider riêng. | Không có một cách đặt box duy nhất cho mọi benchmark. Chọn cách khớp mục tiêu lab, không trộn hai kiểu box trong cùng lượt gán. |
| [BDD100K — định dạng chính thức](https://github.com/ucbdrive/bdd100k/blob/master/doc/format.md) | Vật giao thông dùng box2d; có person/rider; occluded/truncated là boolean; vùng chạy được và vạch đường dùng poly2d. | Lớp và thuộc tính phụ thuộc schema. Không sao chép nguyên bộ lớp hoặc mã occlusion sang lab này. |
| [NVIDIA TAO — KITTI format](https://docs.nvidia.com/tao/tao-toolkit/7.3.0/text/data_annotation_format.html#object-detection-kitti-format) | Mã che 0: rõ, 1: một phần, 2: che nhiều, 3: không biết; truncation tách riêng. | Mức 3 không phải “che nhiều nhất”. Bảng này không đặt mốc che 50%; mốc phần trăm nếu dùng phải là quy ước dự án. |
| [Open Images — định dạng](https://storage.googleapis.com/openimages/web/download.html) | Phân biệt bị vật khác che, vượt mép ảnh, nhóm vật và hình biểu diễn bằng các thuộc tính riêng. | Không đồng nhất mờ/tối, che, cắt mép hoặc hình in. |
| [Open Images V5 — mô tả segmentation](https://storage.googleapis.com/openimages/web/factsfigures_v5.html) | Lựa chọn instance cho mask đòi hỏi biên đủ tin cậy; bỏ qua instance quá mờ/tối trong quy trình mask đó. | Đây là điều kiện của nhãn segmentation V5; không được suy rộng thành “mọi bbox tối/mờ đều bỏ”. |
| [COCO — định dạng chính thức](https://github.com/cocodataset/cocodataset.github.io/blob/master/dataset/format-data.htm) | Một instance mask có thể gồm nhiều polygon khi bị che. Keypoint có cờ visibility. Panoptic dùng ảnh segment-ID với void=0. | Không nối polygon xuyên vật che để tạo một đường viền giả; không dùng mã visibility keypoint làm mã occlusion bbox. |
| [Cityscapes — labeling policy](https://www.cityscapes-dataset.com/dataset-overview/) | Chính sách foreground không có lỗ; có quy tắc riêng cho vùng trộn, cửa kính và group. | Quy tắc lỗ/hở nhỏ phụ thuộc dataset, không có lời hứa “mọi pixel phải vẽ riêng” cho mọi dự án. |

Các liên kết được đọc ngày nghiên cứu. Không sao chép số đo, tỷ lệ hay ngưỡng nghiệm thu của bộ dữ liệu ngoài để thay ngưỡng lab.

## 3. BBox, segmentation, keypoint khác nhau ở đâu?

| Nhiệm vụ | Đơn vị cần gán | Cách xử lý che khuất / chồng |
|---|---|---|
| Bounding box | Hình chữ nhật của từng instance. | Box khác vật có thể chồng nhau; không cắt box để chống chồng. Chọn visible hay amodal theo đặc tả dự án. |
| Semantic segmentation | Lớp tại từng pixel được gán. | Kết quả mask cuối phải có chủ sở hữu lớp rõ; vùng không đủ bằng chứng theo chính sách void/ignore của dataset. Chồng polygon lúc biên tập khác với hai nhãn đồng thời trên một pixel mask cuối. |
| Instance segmentation | Mask riêng cho từng instance. | Theo kiểu visible: chỉ phần nhìn thấy thuộc vật; instance có thể có các vùng rời. Kiểu amodal là nhiệm vụ khác, phải được yêu cầu rõ. |
| Panoptic segmentation | Lớp và segment/instance-ID trên từng pixel. | Bản đồ ID cuối cho mỗi pixel một segment-ID hoặc void; không giữ hai segment-ID cùng pixel. |
| Keypoint | Vị trí các mốc theo skeleton của dự án. | Theo COCO: v=0 chưa gán; v=1 có tọa độ nhưng không nhìn thấy; v=2 nhìn thấy. Chỉ dùng v=1 khi có cơ sở đặt mốc, không bịa tọa độ cho mọi phần bị khuất. |
| Tracking | Instance nối qua thời gian. | Cần chuỗi ảnh và quy tắc giữ ID; 40 ảnh đường phố rời trong lab này không tự tạo thành bài tracking. |

Khuyến nghị segmentation/keypoint ở đây là kiến thức so sánh, không phải danh sách nhãn phải thêm vào lab Day 07.

## 4. Phương án khuyến nghị cho Day 07

### Box và người trên xe

Chọn visible bbox cho thống nhất với khung đề: box nhỏ nhất bao các phần nhìn thấy đáng tin cậy của một instance, không suy diễn chân/tay/đuôi xe bị che. Các phần rời có thể thuộc một vật; box chữ nhật có thể chứa nền hoặc vật che ở giữa. Điều đó không biến các pixel đó thành bộ phận của vật.

Giữ schema hiện có: người ngồi trên xe vẫn là person, xe máy là motorcycle. N người phân biệt được trên một xe → N box person + 1 box motorcycle, nếu các instance đạt điều kiện gán. Không gộp người và xe; không thêm rider chỉ vì BDD/CityPersons có lớp đó. Nếu muốn thay taxonomy, nhóm thảo luận trước v1.0.

### Che khuất, cắt mép và chất lượng ảnh

- Occlusion: do vật khác che. Truncation: do khung ảnh cắt phần vật. Hai điều kiện có thể cùng có.
- Mờ/tối làm giảm độ chắc chắn, không tự là occlusion. Kiểm biên và lớp trước; dùng can_review hoặc vấn đề cấp ảnh nếu bằng chứng không đủ.
- Có thể phóng to/đổi hiển thị để xem; giữ nguyên ảnh gốc và tọa độ. Không dùng chi tiết do mô hình tái tạo làm bằng chứng.
- Quầng sáng, bóng đổ và vệt phản sáng không tự thuộc box. Không thêm khoảng đệm để bao cả vệt nhòe.
- Không suy đoán một vật hoàn toàn vô hình để tạo box; không bỏ mọi vật trong ảnh chỉ vì một vùng quá tối.

### Mức occluded_level đề xuất

Giữ ý nghĩa 0/1/2/3 của schema khởi đầu, dùng mốc dự án để hai người phân biệt 1 và 2:

| Giá trị | Quy ước đề xuất |
|---|---|
| 0 | Không bị vật khác che. |
| 1 | Có che, ước lượng >0% và <50%. |
| 2 | Ước lượng >=50%, vẫn nhận diện được instance và còn phần nhìn thấy. |
| 3 | Không thể ước lượng mức che đáng tin cậy; kèm can_review và lý do. |

Mốc 50% là đề xuất đơn giản cho lab, KHÔNG phải chuẩn quốc tế hoặc mốc bắt buộc của KITTI. Tỷ lệ xét trên silhouette dự kiến trong ảnh, không phải tỷ lệ nền trong box; phần ngoài khung xét bằng truncated. Không thể suy ra tỷ lệ chính xác từ visible bbox đơn thuần. Ca sát ngưỡng/không chắc phải review; cần thử độ nhất quán trên các ví dụ thật trước khi nhóm khóa v1.0.

## 5. Ba ca trực tiếp đã xem

1. img_005.jpg — người và xe che nhau: kiểm tách instance, visible bbox và chấp nhận box chồng. Không suy ra toàn bộ số vật khuất.
2. img_026.jpg — hai người trên xe máy, hầm thiếu sáng và hơi nhòe: phương án 2 person + 1 motorcycle cho ba instance chính; tách vấn đề lớp, biên, occlusion và hình in nền. Chưa chốt tọa độ hay mức occlusion.
3. img_007.jpg — kho có vùng tối, phản sáng và xe ra ngoài khung: phân biệt bóng với hình xe, tối với bị che, cắt mép với box chỉ chạm mép. Không dùng cả vệt phản sáng làm phần xe.

Ba ca trên minh họa những điểm cần thống nhất về lớp, biên box và thuộc tính. NguyenCongQuy đã xác nhận xem đủ 40 ảnh; các quyết định còn chưa chốt được đối chiếu trong bản cá nhân và cuộc thảo luận nhóm.
