# Day 07 — Thành viên và phân công

Nhóm 3 thành viên, dùng mã `101`, `102`, `103` trong file và Task. Bảng đối chiếu họ tên/MSHV được giữ riêng ở kênh nhóm/coach, không kèm gói nộp.

## Mã và sản phẩm từng người

| Mã | Số ảnh | Danh sách ảnh | Guideline cá nhân | COCO độc lập | Ghi chú cá nhân |
|---|---:|---|---|---|---|
| `101` | 27 | [101.txt](submission/qc/assignments/101.txt) | [guideline_101.md](submission/guidelines/individual/guideline_101.md) | `submission/labels/101.json` | `submission/individual_notes/101.md` |
| `102` | 27 | [102.txt](submission/qc/assignments/102.txt) | [guideline_102.md](submission/guidelines/individual/guideline_102.md) | `submission/labels/102.json` | `submission/individual_notes/102.md` |
| `103` | 26 | [103.txt](submission/qc/assignments/103.txt) | [guideline_103.md](submission/guidelines/individual/guideline_103.md) | `submission/labels/103.json` | `submission/individual_notes/103.md` |

- Người điều phối: `103`.
- Đầu mối chốt/sửa/rà bản cuối: `103` theo ủy quyền; chưa có xác nhận reviewer độc lập.
- Bảng chung: [assignment.csv](submission/qc/assignment.csv), đủ 40 ảnh, mỗi ảnh 2 người gán, tổng 80 lượt.
- Ngày 2026-11-17: chuyển định danh cũ sang mã tại chỗ. Danh sách ảnh của từng người được giữ nguyên, không chia lại. Số ảnh vẫn 27/27/26.

## Bàn giao hiện tại

Đã nhận đủ ba COCO và ghi chú. Cả ba xác nhận bộ tám lớp qua 103. Người điều phối xác nhận nhóm ủy quyền 103 chốt phương án calibration và hoàn thiện bộ bài ngày 2026-11-17.

Bản gán độc lập dùng v1.0, bản cuối dùng v1.1 (mô tô có sidecar). Không ghi đè COCO gốc. 103 là đầu mối sửa và rà theo ủy quyền; không ghi 101/102 đã review bản cuối. Bước review độc lập/người ngoài nhóm chưa có xác nhận.

## Lấy bản sửa và kiểm tra lỗi 101.json

```bash
git pull --ff-only origin main
python scripts/validate_labels.py submission/labels/101.json
```

Bảng phân công hiện nhận mã `101`, `102`, `103`. Không đặt tên COCO bằng định danh cũ. Class kỹ thuật là `bus`, `tuk_tuk`; nếu COCO vẫn ghi `Tuk-tuk`/`tuk-tuk`, đổi tên nhãn hiện có trong CVAT, Save và export lại.

**Không chạy lại `split_assign.py` sau khi đổi mã.** Thuật toán sắp xếp danh sách thành viên trước khi chia: cùng seed nhưng đổi bộ tên sang mã có thể phân lại ảnh cho người khác. Chỉ dùng bảng và ba danh sách đã được commit. Nếu cần ảnh gốc, lấy đúng tên trong danh sách từ `data/images/`; không xóa ảnh đang bị báo thừa trước khi kiểm mã.

## Trạng thái và bằng chứng

- Mốc sáu lớp cũ: `3e651e6a28645eec131b765bdea956c58c11f516`; bộ tám lớp trước IAA tham chiếu `4d557cf46301855da38780d937ff0d96babc76d0`. Giữ lịch sử, không tạo lại lượt độc lập.
- 101/102/103: đủ 27/27/26 ảnh, 80/80 lượt và đủ ghi chú. 102 được 103 xác nhận đã đọc/chốt nội dung ghi chú.
- IAA trước calibration: 303 cặp, IoU 0.806, cùng lớp 93.07%, chưa đạt; giữ nguyên số đo.
- Calibration: 861 quyết định gồm Q01–Q04 sau QC; có dấu vết nguồn và sửa. Nhãn cuối 40 ảnh, 557 annotation.
- QC: vòng 1 Rework 1/40 ảnh; vòng 2 Rework 3/40 ảnh; vòng 3 script Release 0/40 ảnh, 0/541 đối tượng lỗi phát hiện. Chi tiết và giới hạn tại QC_REPORT.md.
- Repo giữ public, không mời coach theo yêu cầu người điều phối; ZIP dùng cho bàn giao. Khác hướng dẫn nộp riêng tư của starter.
- Người ngoài nhóm chưa kiểm ngược; 103 đã yêu cầu tự kiểm thay, được ghi đúng là self_traceback.md, không gán người kiểm giả.
