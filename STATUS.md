# Day 07 — Checklist theo pha

Cập nhật 2026-11-17. **Đang nhận/chốt đầu vào Pha 3; chưa hoàn thành IAA, calibration hoặc QC cuối.** Đây là tiến độ, không phải kết quả kiểm chất lượng.

## Ghi chú cá nhân: không điền trước kết quả

- `101.md`, `102.md`, `103.md` là ghi chú xuyên suốt bài, khác guideline cá nhân và khác COCO.
- Khi giao nhãn Pha 3, ghi ca khó, quyết định ban đầu, trao đổi quy tắc, thống kê và ca phân vân đã có.
- Mục Pha 4–5 ghi **“Chưa thực hiện — bổ sung sau IAA/calibration”** nếu chưa có kết quả. Không yêu cầu hoàn tất mục này trước khi nhận COCO để chạy IAA.
- Sau IAA/calibration, bổ sung ca lệch, nguyên nhân và quyết định thực tế; sau QC bổ sung kết quả kiểm lại và bài học. Không tạo số liệu để lấp chỗ trống.
- `can_review` là cờ cần xem, không tự là lỗi; không buộc xóa hết cờ trước IAA.

## Mốc hiện có trên main

- Có ba file guideline cá nhân; việc có file chưa thay cho kiểm nội dung và bằng chứng cá nhân.
- Guideline chung đúng tên, taxonomy tám lớp và cấu hình CVAT đã đồng bộ. Cần xác nhận cùng mốc áp dụng của cả ba thành viên; không lấy xác nhận bản cũ thay cho bổ sung mới.
- Có bảng 40 ảnh × 2 người = 80 lượt, chia 27/27/26 cho mã 101/102/103. Không chạy chia lại.
- Kiểm thử kỹ thuật: 23/23 đạt. Đây là kiểm script bằng fixture, không chứng nhận nhãn đúng hay lab đã hoàn thành.
- Chưa tập hợp ba COCO đã khóa vào `submission/labels/`. Không ghi đè nhãn gốc khi sửa.
- Ghi chú 103 có phần trước IAA; chưa tập hợp ghi chú 101/102 trên main.

## Việc còn lại, theo đúng thứ tự

| Pha | Điều kiện/sản phẩm | Người phụ trách |
|---|---|---|
| Chốt đầu vào | Xác nhận cùng guideline/taxonomy và cách giữ bản cũ/mới; đối chiếu quy trình với RULES.md và hướng dẫn coach | Cả nhóm, 103 điều phối |
| 3 | Từng COCO đúng phân công/lớp/thuộc tính; giữ ảnh 0 box; rà ảnh bằng mắt, ghi ca chưa rõ | Từng thành viên |
| 3 | `submission/labels/101.json`, `102.json`, `103.json`; `image_issues.csv` nếu có ca chưa đặt được box | Từng người gửi riêng, 103 tập hợp khi cả ba khóa bản |
| 4 | Validate toàn bộ; `iaa_summary.json/.md`, `calibration_candidates.csv`, `flagged_images.txt`, `consensus_draft.json`, `image_scores.csv` | 103 chạy script |
| 4 | Xem ảnh chồng nhãn, giải thích lệch; đối chiếu ca tham chiếu coach để tìm khả năng hai người cùng sai | Cả nhóm |
| 5 | `calibration_decisions.csv` và `calibration_context.json`; mỗi ca có quyết định, Rule ID, người chốt, vết sửa, kiểm lại và phạm vi | Cả nhóm chốt, phân công sửa/kiểm |
| 5 | `guideline_v1.1_calibrated.md`, `submission/guideline_change.json`; `taxonomy_v1.1.json` và nhãn CVAT cuối nếu đổi taxonomy | Người được nhóm giao |
| 5 | Sửa đúng phạm vi, giữ COCO sau sửa theo tên có hash do `prepare_calibration.py` in ra nếu cần | Người sửa |
| 5 | `labels_calibrated.json`, `labels_calibrated.csv`, `batch_manifest.json` | 103 build khi đủ đầu vào |
| 6 | Vòng `reviews/round-NN/`: phiếu QC, mẫu, snapshot nhãn/manifest/quyết định và review manifest | Người chuẩn bị vòng |
| 6 | Duyệt 40/40 ảnh; ghi đúng từng lỗi/ảnh sạch; ưu tiên reviewer khác người sửa | Reviewer được phân công |
| 6 | `qc_result.json`, `qc_report_auto.md`, `QC_REPORT.md` đúng vòng; Rework thì giữ vòng cũ và mở vòng mới | 103 tổng hợp, reviewer xác nhận |
| Cuối bài | Hoàn thiện cả ba ghi chú cá nhân bằng kết quả thật | Từng thành viên |
| Bàn giao | `check_submission.py --round <vòng cuối>` 0 lỗi; người ngoài nhóm lần ngược một ca | 103 tổ chức |
| Nộp | Một repo riêng tư có quyền coach hoặc ZIP theo kênh được công bố; đúng tên gói, giữ ATTRIBUTION, không kèm work/ hoặc thông tin đăng nhập | Người đại diện nhóm |

Chưa có các artifact Pha 4–6 trên main tại lần cập nhật này. Chưa đến bước tạo không có nghĩa phải điền sẵn; khi đến pha tương ứng phải thực hiện và lưu bằng chứng thật.

## Quy tắc tránh nhầm

- Validator không phát hiện đủ mọi vật bị bỏ sót hoặc class sai thực tế. Nó không thay việc đọc ảnh.
- Không sửa guideline cá nhân cho giống bản chung rồi coi đó là bản ban đầu; giữ lịch sử và ghi rõ bổ sung sau họp.
- Không đổi âm thầm đầu vào sau khi đo IAA; script khóa hash để truy vết.
- Ví dụ 177 cặp, 45 ca và walkthrough trong tài liệu là số liệu minh họa, không phải kết quả nhóm.
- Các ngưỡng QC giữ nguyên; chưa có số đo thì không kết luận Release.

Nguồn: [GUIDE.md](GUIDE.md), [SUBMISSION.md](SUBMISSION.md), [RUBRIC.md](RUBRIC.md), [CHECKPOINTS.md](CHECKPOINTS.md), [RULES.md](RULES.md).
