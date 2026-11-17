# Day 07 — Gói bàn giao Folked

Ngày 2026-11-17. Mã nhóm: 101/102/103; 40 ảnh, 80 lượt gán. Đọc `submission/qc/QC_REPORT.md` trước khi dùng kết quả.

## Mở bài

- Đặc tả trước gán: `submission/guidelines/guideline_v1.0_agreed.md`, `submission/taxonomy.json`.
- Đặc tả bàn giao: `submission/guidelines/guideline_v1.1_calibrated.md`, `submission/taxonomy_v1.1.json`.
- Nhãn độc lập: `submission/labels/101.json`, `102.json`, `103.json`; không ghi đè sau IAA.
- IAA: `submission/qc/iaa_summary.md`, 303 cặp, IoU 0.806, cùng lớp 93.07%; giữ kết quả chưa đạt trước sửa.
- Quyết định: `submission/qc/calibration_decisions.csv`, 861 dòng gồm 850 ca IAA, 7 ca đọc ảnh bổ sung và Q01–Q04 sau QC.
- Nhãn cuối: `submission/qc/labels_calibrated.json` và `.csv`, 40 ảnh, 541 đối tượng +16 ignore. SHA và nguồn ảnh trong `batch_manifest.json`.
- QC: giữ `reviews/round-01/`, `round-02/` (Rework) và `round-03/` (script Release); các overlay minh chứng tại `submission/qc/evidence/`.
- Ghi chú: đủ ba file tại `submission/individual_notes/`; phần kết quả chung bổ sung bởi 103 được ghi rõ, không viết thay cảm nhận cá nhân.

## Kiểm sau giải nén

Từ thư mục có README.md, cài Python 3.9+ và thư viện trong requirements.txt:

```bash
python -m pip install -r requirements.txt
python scripts/validate_labels.py --all
python scripts/check_submission.py --round 3
```

Hai lệnh kiểm đã chạy trên workspace và được chạy lại từ bản giải nén khi tạo ZIP. Không chạy chia ảnh lại. Không chạy lại IAA trên đầu vào khác rồi dùng bảng quyết định cũ.

Muốn tạo lại ảnh QC, dùng bản nhãn/manifest và drawing/viz của starter. Thư mục `work/` không có trong gói nộp; 40 ảnh gốc và ATTRIBUTION được kèm để kiểm được ngay.

## Phạm vi kết luận

- Vòng 1 phát hiện một box trùng: 1/40 ảnh =2.5%, Rework. Vòng 2 kiểm chéo thêm nguồn tìm 3/40 ảnh lỗi và bốn vật bỏ sót; Q02–Q04 sửa. Vòng 3 không phát hiện lỗi mới: 0/40 ảnh, 0/541 đối tượng. Không tuyên bố nhãn đúng tuyệt đối.
- Chốt/sửa/rà bản cuối theo ủy quyền do 103 thông báo. Chưa có reviewer độc lập hoặc người ngoài nhóm kiểm ngược; `self_traceback.md` ghi đúng là tự kiểm thay theo yêu cầu 103.
- Kết quả kiểm kỹ thuật không chứng minh lượt gán đáp ứng điều kiện con người tự quyết định/vẽ trong RULES.md hoặc mọi yêu cầu ghi chú cá nhân. Bản ghi chú nguồn và lịch sử lượt gán không được viết lại thành bằng chứng độc lập giả.
- Repo giữ public, không mời coach theo yêu cầu 103; khác phương án repo riêng tư trong SUBMISSION.md. ZIP có thể bàn giao qua LMS theo kênh coach công bố.

Giữ nguyên nguồn ảnh, giấy phép và các ngưỡng. Không có thông tin đăng nhập, môi trường ảo, Git metadata hoặc work/ trong gói ZIP.
