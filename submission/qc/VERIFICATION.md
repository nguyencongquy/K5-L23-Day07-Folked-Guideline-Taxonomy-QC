# Kiểm kỹ thuật — 2026-11-17

Các lệnh chạy từ thư mục gốc với Python và thư viện trong requirements.txt.

| Kiểm | Kết quả thực tế |
|---|---|
| `validate_labels.py --all` | 101:27, 102:27, 103:26; hợp lệ về cấu trúc, lớp, thuộc tính và phân công |
| `make_cvat_labels.py --final` | Schema hợp lệ, sinh labels_final.json cho v1.1 |
| `prepare_calibration.py` + `build_final.py` | Đủ 861 quyết định, phạm vi 40 ảnh, đúng export phiên bản, 557 annotation |
| `qc_report.py --round 1` | REWORK; 1/40 ảnh, 1/538 đối tượng có lỗi phát hiện |
| `qc_report.py --round 2` | REWORK; kiểm chéo thêm nguồn tìm 3/40 ảnh lỗi, bốn vật bỏ sót |
| `qc_report.py --round 3` | RELEASE; 40/40 ảnh đã có phiếu, 0/40 và 0/541 lỗi phát hiện |
| `check_submission.py --round 3` | Tổng kết 0 lỗi; exit code 0 |
| `python -m unittest discover -s tests -v` | 23/23 test đạt; exit code 0 |
| `git -c core.whitespace=cr-at-eol diff --check` | Không lỗi khoảng trắng; giữ CRLF chuẩn CSV để không đổi checksum |

Đã đối chiếu hash đầu vào IAA giữ nguyên, rules taxonomy không đổi, nhãn/CSV/manifest/snapshot/phiếu đúng phiên bản; vòng 1 không bị sửa thành vòng sạch. Vòng 2 sửa box trùng ở img_024, rồi kiểm chéo nguồn phát hiện thêm 011/025/034. Vòng 3 sửa ba ảnh này; 37 ảnh còn lại giữ nguyên mọi tuple lớp/bbox/thuộc tính. Giữ cả ba vòng và kết quả ban đầu trước kiểm chéo ở before_targeted_review_*.

Đây là kiểm kỹ thuật và kết quả lượt rà theo ủy quyền 103, không chứng nhận đã có kiểm độc lập hoặc mọi điều kiện quy trình của RULES.md. Xem QC_REPORT.md và HANDOFF.md.
