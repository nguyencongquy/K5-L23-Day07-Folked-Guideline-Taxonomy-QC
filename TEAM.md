# Day 07 — Thành viên và phân công

Nhóm có 3 thành viên. Phạm vi chung: 40 ảnh trong `data/images/`.

## Thành viên

| Họ tên dùng trong repo | Mã số học viên | Định danh dùng trong file | Số ảnh gán độc lập | Danh sách ảnh |
|---|---|---|---:|---|
| TranPhamHoangPhong | 2A202603198 | `TranPhamHoangPhong_2A202603198` | 27 | [Mở danh sách](submission/qc/assignments/TranPhamHoangPhong_2A202603198.txt) |
| PhungDanhKhoa | 2A202603206 | `PhungDanhKhoa_2A202603206` | 27 | [Mở danh sách](submission/qc/assignments/PhungDanhKhoa_2A202603206.txt) |
| NguyenCongQuy | 2A202603149 | `NguyenCongQuy_2A202603149` | 26 | [Mở danh sách](submission/qc/assignments/NguyenCongQuy_2A202603149.txt) |

- Người điều phối: NguyenCongQuy_2A202603149.
- Người review bản cuối: nhóm sẽ chốt trước vòng QC; ưu tiên người khác người sửa.
- Bảng chia chung: [assignment.csv](submission/qc/assignment.csv).
- Cách chia: seed `42`, mỗi ảnh 2 người gán độc lập, tổng 80 lượt; chênh lệch tải tối đa 1 ảnh.

## Việc mỗi thành viên cần làm

1. Xem toàn bộ 40 ảnh; tự viết guideline cá nhân, chưa xem bản của nhau. Gửi riêng theo kênh nhóm/coach; chỉ tập hợp khi cả 3 người đã nộp.
2. Cùng họp chốt `submission/guidelines/guideline_v1.0_agreed.md`, cập nhật `submission/taxonomy.json`, sinh bộ nhãn CVAT rồi khóa v1.0.
3. Mỗi người tạo Task CVAT riêng, upload đúng ảnh trong `work/<định-danh>/images/` và gán độc lập theo v1.0. Ca chưa rõ: dùng `can_review` hoặc ghi vấn đề cấp ảnh theo GUIDE.md.
4. Export COCO 1.0; gửi riêng. Chỉ tập hợp nhãn khi mọi người đã nộp, sau đó chạy IAA, calibration và QC.

| Thành viên | Guideline cá nhân | COCO độc lập | Ghi chú cá nhân |
|---|---|---|---|
| TranPhamHoangPhong | `submission/guidelines/individual/guideline_TranPhamHoangPhong_2A202603198.md` | `submission/labels/TranPhamHoangPhong_2A202603198.json` | `submission/individual_notes/TranPhamHoangPhong_2A202603198.md` |
| PhungDanhKhoa | `submission/guidelines/individual/guideline_PhungDanhKhoa_2A202603206.md` | `submission/labels/PhungDanhKhoa_2A202603206.json` | `submission/individual_notes/PhungDanhKhoa_2A202603206.md` |
| NguyenCongQuy | `submission/guidelines/individual/guideline_NguyenCongQuy_2A202603149.md` | `submission/labels/NguyenCongQuy_2A202603149.json` | `submission/individual_notes/NguyenCongQuy_2A202603149.md` |

Các đường dẫn trên là sản phẩm phải tự hoàn thành, chưa phải bằng chứng đã làm xong. Không đưa bản nháp độc lập lên nhánh chung khi thành viên khác chưa nộp.

## Chạy lại phân công

Từ thư mục gốc repo:

```bash
python scripts/split_assign.py --members TranPhamHoangPhong_2A202603198,PhungDanhKhoa_2A202603206,NguyenCongQuy_2A202603149 --seed 42 --copy-images
```

Không đổi seed hoặc chia lại ảnh sau khi bắt đầu gán nhãn. `work/` chỉ dùng cục bộ, không commit.

## Tùy chỉnh định danh của nhóm

Theo yêu cầu người sở hữu repo, nhóm dùng `Ten_MSHV` trong tên file và bảng chia. `scripts/common.py` và `scripts/split_assign.py` đã được cập nhật để hỗ trợ định dạng này, đồng thời vẫn nhận mã ba chữ số của starter. Thuật toán chia ảnh và các ngưỡng QC không thay đổi.

Đây là tùy chỉnh khác hướng dẫn mã ba chữ số/ẩn thông tin cá nhân trong starter, không phải yêu cầu của starter. Nhóm cần đối chiếu với hướng dẫn coach khi nộp; không xem việc script chạy được là xác nhận coach đã chấp thuận. Hình thức nộp vẫn theo `SUBMISSION.md`.

## Trạng thái và kết quả cuối bài

- Đã hoàn thành: khai báo thành viên, sinh bảng phân công và danh sách ảnh.
- Guideline cá nhân / v1.0: chưa tập hợp; từng thành viên tự thực hiện theo trình tự trên.
- Lượt gán hoàn tất / được giao: chưa ghi nhận / 80.
- IAA trước calibration (kèm mẫu số): chưa đo.
- Số ảnh phải calibration: chưa xác định / 40.
- Vòng QC và số ảnh đã duyệt / cả lô: chưa thực hiện.
- Tỉ lệ lỗi theo ảnh / đối tượng: chưa đo.
- Quyết định Release / Hold / Rework: chưa đánh giá bàn giao.
- Commit chốt bài: điền ở kênh nộp sau khi hoàn thành.
