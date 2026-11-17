# Guideline chung — v1.1 sau calibration

Ngày 2026-11-17. Phạm vi: 40 ảnh, bounding box nhìn thấy, tám lớp. Bản v1.0 và ba COCO độc lập được giữ nguyên theo checksum trong `../qc/iaa_summary.json`. Người điều phối 103 được nhóm ủy quyền chốt phương án calibration theo xác nhận của 103; không ghi 101/102 đã review bản cuối thay cho họ.

## 1. Mục tiêu và phạm vi

Gán người thật và phương tiện thật hiện diện trực tiếp. Mỗi instance đủ điều kiện có một box phần nhìn thấy. Đây là bộ thực hành ca biên, không đại diện dữ liệu vận hành. Không gán polygon, track, hình in, mô hình, tượng hoặc ảnh phản chiếu. Vật ngoài phạm vi không tự thành ignore.

Giữ ngưỡng: visible height 20 px; ghép IAA 0.5; IoU cặp lớn/nhỏ 0.85/0.70 tại 50 px; IAA trung bình 0.85 và cùng lớp 95%; QC ảnh lỗi tối đa 2%. Không thay số IAA trước calibration bằng kết quả sau sửa.

## 2. Taxonomy v1.1 và thuộc tính

| Lớp | Định nghĩa / ranh giới |
|---|---|
| car | Ô tô con/SUV thật, gồm xe đỗ, xe trưng bày thật và xe được chở. Không gồm pickup có thùng tách cabin, van thân hộp hoặc bus. |
| truck | Xe tải/pickup có sàn/thùng hàng riêng; nắp thùng cao không biến pickup thành van. |
| van | Xe cỡ nhỏ, khoang hộp kín liền thân cabin; không có thùng pickup riêng. |
| motorcycle | Xe máy/mô tô có động cơ, gồm xe tay ga và mô tô truyền thống gắn **thùng phụ cố định (sidecar)**. Người lái/người ngồi thùng là person riêng. Không phải tuk-tuk chỉ vì có bánh phụ. |
| person | Người thật nhìn trực tiếp, kể cả qua kính trong khi có bằng chứng đầu/thân/chi; không gán từ tựa ghế, bóng, hình in hoặc bản phản chiếu. |
| bus | Xe buýt/xe khách thân chuyên chở nhiều người; van nhỏ không tự là bus. |
| tuk_tuk | Auto-rickshaw/tuk-tuk có cấu trúc xe ba bánh nguyên bản. Không gồm xe đạp, xích lô đạp chân hoặc mô tô có sidecar. |
| ignore_region | Vùng đối tượng thuộc phạm vi nhưng visible height dưới 20 px, hoặc cụm không tách được cá thể/biên đáng tin. Không dùng để che lỗi hoặc loại tất cả vật tối/mờ. |

Đổi nghĩa duy nhất: mở rõ motorcycle cho mô tô có sidecar, áp dụng cả lô, trọng tâm img_022/img_025. Tên/số lớp và thuộc tính giữ nguyên; lưu riêng `../taxonomy_v1.1.json`, không sửa taxonomy v1.0.

`occluded_level`: 0 = không bị vật khác che; 1 = (0,30%]; 2 = (30,70%]; 3 = trên 70%. Tỷ lệ xét hình chiếu trong khung; phần ngoài ảnh là truncation. Đây là ước lượng có lý do, không phải phép đo từ diện tích box. Không dùng mức 3 thay unknown. `truncated=yes` khi vật thực sự bị mép ảnh cắt; không chỉ vì box chạm mép. Hai thuộc tính độc lập. `can_review` dùng cho bất định chưa giải quyết; không tắt chỉ để qua checker.

## 3. Quy tắc box

- **R-01:** Box nhỏ nhất bao các phần nhìn thấy chắc thuộc cùng vật; không đoán phần khuất, không bao bóng/vệt nhòe. Kính trong không tự cắt mất vật nhìn rõ phía sau.
- **R-02:** Một vật một box. Box của vật khác nhau được chồng; không nhân đôi đầu và toàn thân của cùng người. Không gộp hai instance vì chúng sát nhau.
- **R-03:** Người lái, người ngồi sau, người trong sidecar và phương tiện là các instance riêng nếu đủ điều kiện. Xe đạp/xích lô ngoài phạm vi phương tiện; người trên xe vẫn xét riêng.
- **R-04:** Bao phần cố định của xe: bánh, gương, khung chở, thùng phụ cố định. Loại hàng rời, bao tải, người và xe được chở khỏi silhouette xe chở. Xe được chở gán riêng. Mô tô + sidecar cố định là một motorcycle; hàng trong thùng không nới box xe.
- **R-05:** Đo chiều cao trên ảnh gốc. Từ 20 px và có biên/lớp đáng tin thì gán riêng; thấp hơn hoặc không tách được cụm thì vùng ignore sát nhất hợp lý. Cố tránh phủ vật tiền cảnh rõ; rectangle có nền/che vật khác thì ghi được lý do, không mặc định tất cả thứ bên trong cùng bị loại.
- **R-06:** Chỉ vật thật trực tiếp. Xe thật trong bảo tàng/được chở vẫn là đích; không loại vì đứng cao, không chạy, cũ hoặc ảnh đen trắng. Tượng/người in/đồ chơi loại theo bằng chứng bản chất vật, không phủ ignore cho chúng.
- **R-07:** Loại bản phản chiếu và mảnh xe ego gắn với camera. Không tạo instance từ một chi tiết sát camera chưa đủ biên xác lập vật riêng; giữ vật thật khác trong ảnh.
- **R-08:** Phân lớp theo kết cấu thân: pickup/thùng riêng → truck; thân hộp liền → van; auto-rickshaw → tuk_tuk; mô tô có sidecar → motorcycle. Không dùng màu, chữ hoặc số bánh nhìn thấy đơn lẻ để suy lớp. Chưa đủ bằng chứng thì giữ ca review, không chọn lớp để vượt validator.
- **R-09:** Che bởi vật khác và cắt mép độc lập. Người/xe là vật che không vì vậy tự bị occluded. Box dừng ở ảnh; không vẽ hoàn thiện vật ra ngoài khung.
- **R-10:** Tối/nhòe không tự là occlusion. Dùng biên lõi có thể quan sát, không cả vệt kéo dài hoặc quầng đèn. Không có cấu trúc instance đích thì không tạo box giả từ bóng/thùng rác/tựa ghế. Ca chưa giải quyết phải ghi review, không tự coi sạch.
- **R-11:** Đông người không tự là ignore. Giữ cá thể còn tách được (img_032); khoanh riêng cụm chen kín không tách tin cậy (img_019). Không giữ đồng thời box của chính người đã đưa vào cụm ignore; nhóm rời dùng vùng rời.
- **R-12:** Loại vật ngoài phạm vi, gồm xe đạp/xích lô đạp chân. Không ép thành motorcycle hoặc tuk_tuk. Giữ người thật đủ điều kiện trên/đi cùng phương tiện đó.

## 4. Ca calibration quan trọng

| Ảnh | Phương án chốt | Luật |
|---|---|---|
| 003 | Bỏ box xe đạp dắt bởi người áo đỏ; thu box mô tô phía sau khỏi bánh xe đạp; giữ người. | R-01/R-12 |
| 004 | Hai tuk-tuk, hai người lái; thu box lái bên trái về phần thực sự thấy qua kính. | R-01/R-03/R-09 |
| 005 | Không thêm người từ tựa ghế/bóng sau kính van khi không thấy bằng chứng cá thể. | R-06/R-10 |
| 007/012/016/037 | Pickup có thùng riêng là truck, kể cả nắp cao; 016 bổ sung pickup bị tuk-tuk che. | R-04/R-08 |
| 014 | Giữ người/xe có silhouette lõi dù nhòe; tách người và xe, không bao cả vệt chuyển động. | R-01/R-03/R-10 |
| 015/018 | Bỏ thùng/thiết bị nhầm người ở 015; bỏ box trùng một phần mặt trước van ở 018. | R-02/R-10 |
| 017/020/027/033/038 | Giữ ảnh rỗng: tượng, phản chiếu, ma-nơ-canh và xe đồ chơi không tạo instance hoặc ignore. | R-06/R-07 |
| 019/021/032 | Cụm chen kín 019 dùng ignore; 021 giữ người/xe tiền cảnh, ignore dải xa; 032 giữ từng người tách được. | R-05/R-11 |
| 022/025 | Thùng phụ cơ khí thuộc mô tô; người trong thùng xét riêng; không tính hàng rời. | R-03/R-04/R-08 |
| 024 | Thêm ô tô vàng thật trưng bày phía trên; không loại vì ở bảo tàng hoặc đang được chở. Đây là ca cả hai bản cùng bỏ sót. | R-04/R-06 |
| 030 | Dãy xe là motorcycle, không phải car; bỏ box nhỏ trùng đầu/vai người áo xanh. | R-02/R-08 |
| 035 | Giữ hai tuk-tuk, tách người và mô tô; bỏ box trùng mũ/cổ, không kéo box tuk-tuk theo bao tải. | R-01/R-02/R-04/R-08 |

## 5. Bằng chứng, QC và giới hạn

Từng ca trong `../qc/calibration_decisions.csv` dẫn nguồn, lựa chọn, luật, vị trí sửa và kiểm lại; `../qc/calibration_trace.json` dẫn đến source ID và ID nhãn cuối. 103 chốt theo ủy quyền được thông báo ngày 2026-11-17. Không ghi 101/102 hoặc người ngoài nhóm đã review khi chưa có xác nhận.

Đổi nghĩa sidecar cần rà lại cả 40 ảnh; giữ mọi ngưỡng và 80 lượt độc lập. Bản sửa theo đường dẫn phiên bản do `prepare_calibration.py` sinh. Mỗi vòng QC giữ snapshot, phiếu và manifest riêng. Số đo IAA trước sửa phục vụ chẩn đoán; kết quả bàn giao căn cứ QC bản cuối. Vật nhỏ, nhòe và tỷ lệ che vẫn có giới hạn quan sát; không tuyên bố độ chính xác tuyệt đối hoặc tương đương ground truth vận hành.
