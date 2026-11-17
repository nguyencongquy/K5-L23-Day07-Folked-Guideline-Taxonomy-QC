# Day 07 — Trạng thái bàn giao

Cập nhật 2026-11-17. Đã tập hợp ba guideline, ba COCO độc lập và ba ghi chú. Bản cuối đã calibration và QC ba vòng; kiểm kỹ thuật bằng check_submission vòng 3. Không coi số đo QC là chứng nhận đầy đủ quy trình hoặc độ chính xác tuyệt đối.

| Mục | Bằng chứng / kết quả |
|---|---|
| Phân công | 40 ảnh × 2 =80 lượt; 101/102/103 có 27/27/26 ảnh; không chia lại |
| Đặc tả đầu vào | v1.0 tám lớp, ba thành viên xác nhận qua người điều phối; giữ mốc sáu lớp cũ trong Git |
| Đầu vào | Ba COCO và 14 vấn đề cấp ảnh; checksum trong iaa_summary.json |
| IAA trước sửa | 303 cặp, IoU 0.806, cùng lớp 93.07%, chưa đạt; giữ nguyên số đo |
| Calibration | 861 quyết định: 850 ca IAA +7 ca đọc ảnh bổ sung +Q01–Q04; trace/changes và image_adjudication.csv |
| v1.1 | Sidecar thuộc motorcycle, khác tuk_tuk; tám tên lớp, thuộc tính và rules không đổi; rà cả lô |
| Nhãn cuối | 40 ảnh, 557 annotation =541 đối tượng +16 ignore; giữ 5 ảnh rỗng |
| QC vòng 1 | 40/40, một box trùng ở 024 →1/40=2.5%, REWORK; giữ snapshot |
| QC vòng 2 | Kiểm chéo phát hiện 3/40 ảnh lỗi, bốn vật bỏ sót; REWORK, giữ bằng chứng |
| QC vòng 3 | Sửa Q02–Q04, đối chiếu cả lô, 0/40 và 0/541 lỗi được phát hiện; script RELEASE |
| Giới hạn | Rà theo ủy quyền 103, chưa review độc lập; người ngoài nhóm chưa kiểm ngược. self_traceback.md không thay người ngoài nhóm |
| Kênh bàn giao | Giữ repo public theo yêu cầu 103; cung cấp ZIP. Khác khuyến nghị repo riêng tư của SUBMISSION.md |

Mở [báo cáo QC](submission/qc/QC_REPORT.md), [bản v1.1](submission/guidelines/guideline_v1.1_calibrated.md), [nhãn cuối](submission/qc/labels_calibrated.json), [quyết định](submission/qc/calibration_decisions.csv) và [tự kiểm ngược](submission/qc/self_traceback.md).

Bước kiểm ngược của người ngoài nhóm và điều kiện gán độc lập bởi con người trong RULES.md không thể được chứng nhận chỉ bằng việc đủ file hoặc checker 0 lỗi. Không ghi thay người chưa làm. Ba bản nhãn gốc, IAA và vòng QC cũ được giữ để người chấm kiểm tra.
