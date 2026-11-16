# Day 07 — Thành viên và phân công

Nhóm 3 thành viên, dùng mã `101`, `102`, `103` trong file và Task. Bảng đối chiếu họ tên/MSHV được giữ riêng ở kênh nhóm/coach, không kèm gói nộp.

## Mã và sản phẩm từng người

| Mã | Số ảnh | Danh sách ảnh | Guideline cá nhân | COCO độc lập | Ghi chú cá nhân |
|---|---:|---|---|---|---|
| `101` | 27 | [101.txt](submission/qc/assignments/101.txt) | [guideline_101.md](submission/guidelines/individual/guideline_101.md) | `submission/labels/101.json` | `submission/individual_notes/101.md` |
| `102` | 27 | [102.txt](submission/qc/assignments/102.txt) | [guideline_102.md](submission/guidelines/individual/guideline_102.md) | `submission/labels/102.json` | `submission/individual_notes/102.md` |
| `103` | 26 | [103.txt](submission/qc/assignments/103.txt) | [guideline_103.md](submission/guidelines/individual/guideline_103.md) | `submission/labels/103.json` | `submission/individual_notes/103.md` |

- Người điều phối: `103`.
- Người review bản cuối: nhóm chốt trước vòng QC; ưu tiên người khác người sửa.
- Bảng chung: [assignment.csv](submission/qc/assignment.csv), đủ 40 ảnh, mỗi ảnh 2 người gán, tổng 80 lượt.
- Ngày 2026-11-17: chuyển định danh cũ sang mã tại chỗ. Danh sách ảnh của từng người được giữ nguyên, không chia lại. Số ảnh vẫn 27/27/26.

## Việc cần làm tiếp

1. Cả ba dùng cùng [guideline v1.0](submission/guidelines/guideline_v1.0_agreed.md), [taxonomy tám lớp](submission/taxonomy.json) và cấu hình nhãn CVAT tương ứng.
2. Save và export COCO 1.0, tắt Save images. Lấy JSON trong thư mục annotations của ZIP, đặt đúng mã ở bảng trên. Giữ ảnh không có box.
3. Chạy `python scripts/validate_labels.py submission/labels/101.json` (hoặc `102.json`, `103.json`). Tất cả đã hoàn thành lượt gán theo xác nhận của người điều phối; chỉ tập hợp sau khi cả ba khóa bản độc lập.
4. Khi đủ ba file: `validate_labels.py --all`, `qc_compare.py`, `viz_disagreement.py --images flagged`; cả nhóm chốt calibration rồi QC và bàn giao theo GUIDE.md/SUBMISSION.md.

Các đường dẫn COCO/ghi chú trong bảng là sản phẩm cần tập hợp, không phải bằng chứng file đã có đủ hoặc đã qua QC.

## Đồng bộ trước khi tập hợp nhãn

Người điều phối gửi cùng một commit tham chiếu cho cả ba. Từng người xác nhận đã đọc bộ chuẩn gồm guideline v1.0, taxonomy tám lớp, cấu hình CVAT và danh sách ảnh theo mã; không tự ghi thay xác nhận của người khác.

Riêng `102`: gói HOLD đã nhận là snapshot sáu lớp, không thay thế repo chung hiện tại. Giữ snapshot cũ; lấy bộ chuẩn mới, bổ sung bus/tuk_tuk và rà ignore trên đúng 27 ảnh được giao. Nếu đã sửa CVAT, chỉ cần export lại. Với Project/Task đã có annotation, giữ các nhãn/ID hiện có; sửa tên lớp hoặc thêm lớp/thuộc tính còn thiếu, không xóa rồi tạo lại cả bộ nhãn.

Bàn giao cá nhân cần `102.json`, ghi chú `102.md` do thành viên xác nhận, và các vấn đề cấp ảnh còn lại với annotator `102`. Guideline cá nhân gốc giữ quyết định ban đầu. Không yêu cầu `102` tự tạo IAA, calibration hay báo cáo QC của cả nhóm trước khi có đủ ba bản; các bước đó thực hiện chung sau khi khóa đầu vào.

## Lấy bản sửa và kiểm tra lỗi 101.json

```bash
git pull --ff-only origin main
python scripts/validate_labels.py submission/labels/101.json
```

Bảng phân công hiện nhận mã `101`, `102`, `103`. Không đặt tên COCO bằng định danh cũ. Class kỹ thuật là `bus`, `tuk_tuk`; nếu COCO vẫn ghi `Tuk-tuk`/`tuk-tuk`, đổi tên nhãn hiện có trong CVAT, Save và export lại.

**Không chạy lại `split_assign.py` sau khi đổi mã.** Thuật toán sắp xếp danh sách thành viên trước khi chia: cùng seed nhưng đổi bộ tên sang mã có thể phân lại ảnh cho người khác. Chỉ dùng bảng và ba danh sách đã được commit. Nếu cần ảnh gốc, lấy đúng tên trong danh sách từ `data/images/`; không xóa ảnh đang bị báo thừa trước khi kiểm mã.

## Trạng thái và bằng chứng

- Guideline cá nhân: đã tập hợp ba bản; lần đổi mã chỉ sửa định danh/đường dẫn, giữ quyết định annotation và lịch sử commit.
- Mốc xác nhận bản sáu lớp: cả ba đã đồng ý theo xác nhận người điều phối ngày 2026-11-16 tại commit `3e651e6a28645eec131b765bdea956c58c11f516`.
- Bổ sung tám lớp: theo hướng dẫn coach do `103` báo lại, thêm bus/tuk_tuk và làm rõ ignore. Taxonomy và cấu hình đã cập nhật; chưa ghi nhận xác nhận lại đầy đủ của nhóm cho bản bổ sung.
- CVAT local của `103`: Project #2 có tám lớp; job #16 đã lưu 471 box trên đủ 26 ảnh, 69 box có can_review. Chưa xác nhận nội dung export của `101`/`102`.
- Tiến độ gán: người điều phối báo cả ba hoàn thành; chưa kiểm đủ 80 lượt từ ba bản COCO.
- IAA: chưa đo. Calibration: chưa thực hiện. QC cuối: chưa thực hiện. Chưa có kết luận Release.
- Commit chốt bài: điền ở kênh nộp sau khi kiểm tra xong.

Lần chuẩn hóa mã không thay ngưỡng QC, tên class, thuộc tính hoặc tọa độ annotation. Không dùng số lượng file hoặc trạng thái Save/Finish để thay kết quả IAA/QC.
