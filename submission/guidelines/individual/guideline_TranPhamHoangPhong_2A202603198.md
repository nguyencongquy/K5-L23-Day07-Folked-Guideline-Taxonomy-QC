# Guideline gán nhãn — v0 (bản khởi đầu)

> **Đây là bản do giảng viên viết nhanh. Nó CHƯA đủ dùng.**
> Đừng sửa file này. Hãy copy sang `submission/guidelines/individual/` rồi sửa bản của bạn
> (xem `GUIDE.md`, Pha 1).

- Phiên bản: v0
- Loại nhãn: bounding box (hộp chữ nhật) trên ảnh đường phố
- Công cụ: CVAT, xuất COCO 1.0

---

## 1. Mục tiêu

Nhãn dùng để huấn luyện mô hình phát hiện **người và phương tiện** trên ảnh đường phố
(ví dụ cho camera giao thông hoặc xe tự lái).

## 2. Lớp và thuộc tính

### Lớp (mỗi vật thuộc đúng một lớp)

| Lớp | Định nghĩa |
|---|---|
| `car` | Ô tô con. |
| `truck` | Xe tải. |
| `van` | Xe thùng kín cỡ nhỏ. |
| `motorcycle` | Xe máy, mô tô. |
| `person` | Người. |
| `ignore_region` | Vùng không cần gán từng vật. |

### Thuộc tính

| Thuộc tính | Giá trị | Ý nghĩa |
|---|---|---|
| `occluded_level` | `0`, `1`, `2`, `3` | Mức bị che: 0 là thấy rõ, 1 là bị che ít, 2 là bị che nhiều, 3 là không rõ. |
| `truncated` | `no`, `yes` | Vật bị cắt ở mép ảnh. |
| `can_review` | tick / không | Tick khi bạn không chắc, để người khác xem lại. |

## 3. Quy tắc vẽ box
3.1 Quy tắc chung
1. Gán box cho mọi người và phương tiện trong ảnh.
2. Box ôm sát vật.
3. Chọn đúng lớp và thuộc tính.
4. Không vẽ vật thể xuất hiện trên biển quảng cáo, trên bức ảnh.
5. Không tự suy đoán phần bị khuất hoặc không quan sát được của đối tượng.

3.2 Phân biệt đối tượng thực và hình ảnh được tái hiện.

    Đây là quy tắc quan trọng nhằm tránh gán nhãn nhầm giữa vật thể thực tế và hình ảnh của vật thể.
1. Đối tượng thực tế — PHẢI gán nhãn
    
    * Ô tô, xe máy, xe tải đang xuất hiện trong cảnh đường phố.

    * Người đang đi bộ, đứng hoặc di chuyển trong môi trường thực.

    * Đối tượng có hình ảnh đen trắng hoặc thiếu màu do điều kiện camera, ánh sáng hoặc chất lượng ảnh, nhưng vẫn xác định được là vật thể thực tế.
2. Đối tượng được tái hiện qua hình ảnh — KHÔNG gán nhãn.

    * Ô tô hoặc người xuất hiện trên biển quảng cáo.
    * Hình người, phương tiện trong poster, banner hoặc tranh vẽ.

    * Hình ảnh xuất hiện trên màn hình TV, màn hình LED hoặc màn hình điện tử.

    * Ảnh chụp được treo trên tường, đặt trong khung ảnh hoặc xuất hiện bên trong một bức ảnh khác.
## 4. Ca khó

4.1 Vật thể bị che khuất : Che khuất ít, vẫn nhận diện rõ


| Tình huống | Quy định |
|:---:|:---:|
| Xe thật xuất hiện trong ảnh đen trắng | Gán nhãn |
| Xe xuất hiện trên biển quảng cáo | Không gán nhãn |
| Người xuất hiện trên poster | Không gán nhãn |
| Xe xuất hiện trên màn hình LED | Không gán nhãn |
| Không xác định được vật thể thật hay hình ảnh | `can_review = true` |

4.2 Vật thể bị cắt bởi biên ảnh (Truncation)
|Tình huống| Quy định|
|---|---|
|Xe bị cắt ở cạnh trái/phải ảnh|`truncated = yes`|
|Người bị cắt ở cạnh trên/dưới ảnh|`truncated = yes`|
|Xe nằm hoàn toàn trong ảnh|`truncated = no'|

4.3 Các đối tượng chồng lấn hoặc đứng gần nhau
1. Mỗi đối tượng riêng biệt phải có một Bounding Box riêng.
2. Không gộp nhiều xe thành một Bounding Box.
3. Khi hai đối tượng chồng lấn, box của từng đối tượng vẫn được vẽ độc lập.
4. Không tự suy đoán đường biên bị che khuất.

4.4 Đối tượng nhỏ hoặc khó nhận diện

1. Nếu đối tượng vẫn nhận diện được là người hoặc phương tiện: gán nhãn.
2. Nếu quá nhỏ hoặc không đủ thông tin để phân loại: đánh dấu can_review = true.
3. - Không tự đoán class dựa trên hình dáng không rõ ràng.

4.5 Phân loại phương tiện không rõ ràng

|Tình huống| Quy định|
|---|---|
|Ô tô con|car|
|Xe tải|truck|
|Xe thùng kín cỡ nhỏ|van|
|xe máy, mô tô|motorcycle|
|Không xác định được loại xe|can_review= true|


## 5. QC và phiên bản

- Mọi thay đổi làm đổi nhãn đều tăng phiên bản guideline.
- Ca mơ hồ: tick `can_review`.
1. Mỗi ảnh phải được kiểm tra không bỏ sót đối tượng.



2. Không có Bounding Box dư thừa trên poster, quảng cáo hoặc màn hình.



3. Không gộp nhiều đối tượng vào cùng một box.



4. Kiểm tra tính nhất quán giữa các annotator.



5. Các trường hợp can_review = true phải được reviewer xác nhận trước khi đưa vào Ground Truth cuối cùng.
