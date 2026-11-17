# Báo cáo QC — Folked

Ngày 2026-11-17. Ba thành viên 101/102/103; quyết định calibration và việc rà bản cuối thực hiện theo ủy quyền của người điều phối 103. Không ghi 101/102 hoặc người ngoài nhóm đã kiểm bản cuối. Kết quả dưới đây mô tả lượt rà có hỗ trợ công cụ; không phải chứng nhận không còn lỗi tuyệt đối.

## 1. Mục đích, dữ liệu và phiên bản

- Mục đích và giới hạn sử dụng: Thực hành đặc tả, calibration và QC nhãn box trên 40 ảnh ca biên; chưa đánh giá độ đại diện hoặc chất lượng cho vận hành thực tế.
- Nguồn và phép sử dụng dữ liệu: Giữ nguyên 40 ảnh starter và data/ATTRIBUTION.csv; nguồn/giấy phép theo từng ảnh, không thêm ảnh mới.
- Manifest và checksum nhãn cuối: submission/qc/batch_manifest.json; labels_calibrated.json SHA-256 1ad177c9b4e2c589140c8d34bbec689c53b6cd30d38cc57a91c019aca823dfbf.
- Guideline và taxonomy khi gán / khi bàn giao: Lượt độc lập giữ v1.0 tám lớp; bàn giao guideline_v1.1_calibrated.md và taxonomy_v1.1.json. Sidecar được mở rõ trong motorcycle; tám tên lớp, ba thuộc tính và toàn bộ rules không đổi. Bản v1.0 không bị ghi đè.

## 2. Phân công và đồng thuận trước calibration

- Lượt hoàn thành / được giao: 80/80; 101:27/27, 102:27/27, 103:26/26; mỗi ảnh hai người, không chia lại.
- Số cặp box ghép, IoU TB, đồng thuận lớp và mẫu số: 303 cặp ghép tại IoU >=0.5; trung bình 0.805872; cùng lớp 282/303 = 93.07%, chưa đạt 0.85/95%. Cùng occluded_level 151/303 = 49.83%; cùng truncated 290/303 = 95.71%. Không thay số đo trước sửa bằng so sánh nhãn cuối với chính nó.
- Box chưa ghép, loại do ignore, bất đồng vùng ignore: 53/237 chưa ghép phía A/B; 0 bỏ qua do cả hai vùng ignore; 66 dòng bat_dong_ignore. Có 850 dòng IAA trên 40 ảnh, nhiều dòng có thể cùng một instance; không coi là 850 lỗi độc lập.
- Ca hai người có thể cùng sai sau khi đối chiếu tham chiếu: img_024.jpg, car vàng thật phía trên chưa được gán đúng ở cả hai bản: 101 phủ ignore sai, 103 thiếu box toàn xe. Đối chiếu ảnh gốc starter, R-04/R-06 và N001: xe trưng bày/được chở vẫn là vật thật; không loại chỉ vì đặt cao. Đây là kiểm trên ảnh gốc, chưa có một nhãn chuẩn riêng do coach cấp để đối chiếu.

## 3. Calibration và kiểm lại

- Ca đã xử lý, rule ID, bản sửa và người xác nhận: calibration_decisions.csv có 861 dòng = 850 ca IAA + N001–N007 và Q01–Q04. Tất cả có quyết định, luật, phiên bản, người chốt 103, dấu vết sửa và kiểm lại. calibration_trace.json nối source ID/box tới ID cuối; calibration_changes.json lưu trước/sau. Quyết định theo ủy quyền do 103 thông báo, không giả định 101/102 đã làm reviewer.
- Tác động v1.1 lên nhãn và ảnh: R-04/R-08 làm rõ mô tô có sidecar khác tuk-tuk, C500/C566 phạm vi lo; rà lại cả 40 ảnh. Giữ người riêng, bỏ xe đạp/đồ giả/phản chiếu, sửa box trùng và lớp/biên/thuộc tính. Nhãn cuối: 541 box đối tượng + 16 vùng ignore = 557 annotation; 5 ảnh rỗng 017/020/027/033/038 vẫn có mặt.
- Tham chiếu bằng chứng khi Rework: round-01 ghi thua ở img_024 #351, là một phần cùng xe vàng #359. 1/40 = 2.5% vượt 2%; kết quả REWORK được giữ nguyên. Q01 bỏ box trùng; bản phiên bản 30c9ac857da596f5 tạo vòng 2. Kiểm chéo vòng 2 phát hiện bốn vật bỏ sót, một box thiếu và một vùng ignore thừa trên ba ảnh (3/40=7.5%); Q02–Q04 sửa và bản 89582a660acc1048 tạo vòng 3. Xe vàng cuối là #359. Giữ cả ba export và snapshot.

## 4. Kết quả duyệt

- Vòng QC: 3
- Người review và ngày: 103 theo ủy quyền, 2026-11-17; cùng đầu mối sửa và rà có công cụ. Chưa có review độc lập của 101/102 hoặc người ngoài nhóm.
- Đã duyệt / cả lô: 40/40. Vòng 1 xem toàn cảnh/crop/overlay; vòng 2 kiểm chéo nguồn phát hiện thêm bỏ sót/biên/ignore ở 011/025/034. Vòng 3 mở lại cả ba ảnh đã sửa, đối chiếu snapshot chứng minh 37 ảnh còn lại không đổi lớp, hình học, thuộc tính. recheck_scope.json ghi phương pháp/phạm vi.
- Ảnh lỗi / ảnh đã duyệt: 0/40 = 0.0% lỗi phát hiện trong lượt cuối; không diễn giải là cam kết độ chính xác 100%.
- Annotation đối tượng hiện có bị lỗi / annotation đối tượng đã duyệt: 0/541 = 0.0%; không đưa 16 ignore vào mẫu số đối tượng.
- Vật bỏ sót, ảnh và vùng: 0 trường hợp mới phát hiện trong lượt cuối; IAA không phát hiện được vật mà cả hai cùng bỏ sót, vì vậy đã đối chiếu ảnh gốc.
- Ca khó / phần còn lại, lỗi lặp và ví dụ: cả 40 ảnh đều là ca_kho theo IAA; 0/40 ảnh lỗi ở vòng 3. Vòng 1 có một lỗi trùng; vòng 2 có lỗi bỏ sót lặp, đã rà các box chưa được giữ lại và sửa Q02–Q04. Xem qc_review.csv và ba báo cáo tự động.
- Một lỗi máy chặn được và một lỗi cần người đọc ảnh: Validator chặn lớp tuk-tuk không trùng tuk_tuk, thiếu ảnh/thuộc tính và box ra ngoài ảnh. Cả hai bỏ car vàng hoặc cùng gán một mảnh thân thành xe riêng cần đọc ảnh, validator không chứng minh được.

## 5. Quyết định bàn giao

- Quyết định: Release
- Lý do, người nhận và phạm vi sử dụng: Bộ script QC vòng 3 trả RELEASE với 40/40 phiếu và bản nhãn/manifest khớp. Bàn giao nhóm/coach để chấm bài thực hành, không dùng làm ground truth vận hành. Đây là kết luận của lượt rà theo ủy quyền 103, không chứng nhận đã thực hiện review độc lập hay mọi điều kiện quy trình học tập.
- Nếu còn việc: Theo GUIDE.md, kiểm ngược bởi người ngoài nhóm chưa thực hiện. 103 yêu cầu tự kiểm thay nên đã có self_traceback.md nhưng không đổi thành bằng chứng người ngoài nhóm. RULES.md yêu cầu con người tự quyết định/vẽ lượt độc lập; không dùng kết quả kiểm kỹ thuật để chứng minh điều kiện đó. Repo giữ public theo yêu cầu 103, khác khuyến nghị nộp riêng tư; ZIP là gói bàn giao riêng.
- Kiểm ngược từ báo cáo tới ảnh, rule và nhãn: N001/Q01 → img_024.jpg → R-04/R-06/R-02 → labels_calibrated.json #359. Đã kiểm lại ảnh, source hash, ba snapshot, lý do bỏ #351 vòng 1 và box xe vàng vòng 3. Xem self_traceback.md; chưa gán tên người ngoài nhóm.
