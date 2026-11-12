# Báo cáo thực hành Day 3

- Mã học viên theo lớp: 2A202603311
- Ngày / CVAT local: 11/11/2026 / CVAT local
- Công cụ đã dùng: Brush (mask, export RLE) và Polygon. [XÁC NHẬN: có dùng Intelligent Scissors / SAM]

## 1. Bài đã nộp

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Cả 9 ZIP đều có dữ liệu và đúng format: semantic xuất Segmentation mask 1.1, instance và panoptic xuất COCO 1.0. Không có export lỗi.

## 2. Một quyết định trước khi dùng gợi ý

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: `medium_instance`, ảnh `000000181542.jpg`, xe máy bên trái, nửa dưới ảnh (object 1, lớp `motorcycle`). [XÁC NHẬN: đây đúng là object bạn vẽ đầu tiên]
- Class và quy tắc tôi dùng để chọn biên: lớp `motorcycle`. Mask chỉ phủ phần xe nhìn thấy (thân, yên, hai bánh). Người ngồi trên xe là một `person` riêng (object 8) nên không gộp vào mask xe máy. Tôi không vẽ đoán phần xe bị người đi bộ phía trước che.
- Nếu dùng gợi ý sau đó: [XÁC NHẬN: "Không dùng gợi ý tự động cho object này" hoặc ghi vùng gợi ý sai/đúng, bạn sửa hay giữ và lý do]

## 3. Một lỗi tôi tìm thấy và sửa

- Task/ảnh/vùng: `cp5_occlusion`, ảnh `000000336232.jpg`, xe tay ga có người lái ở góc dưới bên trái (object 2). Xe tay ga nhỏ phía sau người lái (object 17) cũng cần kiểm lại.
- Lỗi thuộc loại: sai lớp.
- Bằng chứng tôi nhìn thấy: khi rà lại file export, object 2 đang mang nhãn `bicycle`. Trên ảnh phóng to, đây là xe có động cơ, yên dài và hộp chở hàng phía sau, không có bàn đạp. `classes.json` của task có sẵn lớp `motorcycle`.
- Quy tắc và hành động sửa: tên lớp phải theo `classes.json` và chọn theo loại xe, không theo kích thước. Tôi đổi nhãn object 2 từ `bicycle` sang `motorcycle`, giữ nguyên mask.
- Sau sửa đã Save và export lại chưa? [XÁC NHẬN: nếu đã làm thì ghi "Đã Save, export lại cp5_occlusion.zip và upload lên submissions/"; nếu chưa kịp thì ghi "Chưa — ZIP nộp vẫn còn lỗi này, tôi báo coach"]

Điểm tự đánh giá (Actions/scorer): [XÁC NHẬN: "chưa có điểm" nếu chưa xem Summary]. Scorecard ba tier tối đa 82, không phải điểm cuối. Tôi không tự ghi PASS, top 3 hay bonus.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| `cp2_slice`, `000000017627.jpg`, mép trái ảnh | Xe buýt xanh sát camera là `bus` cần gán, hoặc chỉ là dải hẹp nên bỏ | Quy tắc rìa ảnh: vật bị cắt vẫn gán nếu nhận được lớp. Ở đây chỉ thấy khoảng 60 px bề ngang (gương, khung cửa, thân xe). | Hiện chưa gán. Xin coach xác nhận dải hẹp này có phải gán `bus` không. |
| `hard_panoptic`, `000000460147.jpg`, xe chở ô tô giữa ảnh | Một mask `truck` phủ cả khung xe chở, hoặc các ô tô chất trên sàn là `car` riêng | Panoptic cần thing tách instance. Hiện object 50 (`truck`) chỉ phủ thành xe bên trái (khoảng 2,6k px), phần sàn, đuôi và các ô tô trên xe chưa phủ. | Tôi thấy mask xe tải còn thiếu và cần mở rộng. Xin coach xác nhận các ô tô trên sàn tính `car` riêng hay nằm trong `truck`. |
| `cp6_coverage`, `7daa6479-67988f3f`, xe buýt giữa ảnh | Gán `car` cho xe buýt, hoặc để trống vì schema không có `bus` | Schema cp6 chỉ có building, car, person, road, sidewalk, sky, vegetation. Hiện toàn bộ xe buýt đang là `car`, xe tải bên trái (khoảng 95% vùng) và viền quanh xe buýt còn trống. Tổng khoảng 17% pixel chưa gán. | Tôi chọn `car` để tránh khe trống lớn. Xin coach xác nhận có nên để trống vật thể ngoài schema, vì đề dặn không tô ép vùng chưa chắc. |
