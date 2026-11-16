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

Nhóm sử dụng họ tên và mã số học viên để đối chiếu người thực hiện với từng sản phẩm khi chấm bài. Định danh `Ten_MSHV` được áp dụng thống nhất trong bảng chia, danh sách ảnh, guideline, COCO và ghi chú cá nhân.

## Trạng thái và kết quả cuối bài

- Đã hoàn thành: khai báo thành viên, sinh bảng phân công và danh sách ảnh.
- Guideline cá nhân: đã tập hợp ba file trên main. NguyenCongQuy xác nhận đã xem đủ 40 ảnh; Quý và Khoa còn các mục nháp/đề xuất cần hoàn thiện.
- Guideline chung: [v1.0](submission/guidelines/guideline_v1.0_agreed.md) đã tổng hợp, taxonomy/CVAT đã đồng bộ; cần ba người đọc/thống nhất trước khi vẽ, chưa ghi nhận xác nhận thực tế.
- Mốc khóa v1.0 (ngày/commit và xác nhận thực tế của nhóm): chưa ghi nhận.
- Lượt gán hoàn tất / được giao: chưa ghi nhận / 80.
- IAA trước calibration (kèm mẫu số): chưa đo.
- Số ảnh phải calibration: chưa xác định / 40.
- Vòng QC và số ảnh đã duyệt / cả lô: chưa thực hiện.
- Tỉ lệ lỗi theo ảnh / đối tượng: chưa đo.
- Quyết định Release / Hold / Rework: chưa đánh giá bàn giao.
- Commit chốt bài: điền ở kênh nộp sau khi hoàn thành.

## Xác nhận khóa guideline v1.0

Mỗi thành viên đọc guideline chung, taxonomy và phản hồi trong kênh nhóm. Phản hồi ghi rõ phiên bản/commit được đồng ý; nếu còn bất đồng, ghi Rule ID và phương án sửa để nhóm chốt trước lượt gán độc lập.

| Thành viên | Xác nhận phạm vi đã xem | Xác nhận áp dụng guideline v1.0 | Ngày và commit khóa |
|---|---|---|---|
| TranPhamHoangPhong_2A202603198 | Chưa ghi nhận trong bảng này | Chờ xác nhận | Chưa chốt |
| PhungDanhKhoa_2A202603206 | Chưa ghi nhận trong bảng này | Chờ xác nhận | Chưa chốt |
| NguyenCongQuy_2A202603149 | Đã xem đủ 40 ảnh, theo xác nhận của thành viên | Chờ xác nhận | Chưa chốt |

Các điểm cần cùng hiểu gồm sáu lớp, box phần nhìn thấy, tách người và xe, mức che 0/30/70, cắt mép, ngưỡng chiều cao 20 px và cách dùng can_review/ignore_region. Khi đủ xác nhận, người điều phối điền ngày và commit khóa thực tế; cả nhóm dùng cùng bản trong lượt gán độc lập.
