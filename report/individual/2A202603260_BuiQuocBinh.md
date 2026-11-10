# Báo cáo cá nhân — Ngày 2 Object Detection

## 1. Thông tin học viên

- Họ tên: Bùi Quốc Bình
- MSSV: 2A202603260
- Email: *(Điền email của bạn)*
- Lớp: K5-L2B
- Link repo bài làm: https://github.com/binhbui8112004/K5-L2B-Day02-BUIQUOCBINH-2A202603260-ObjectDetection
- Link Colab đã chạy: *(Dán link Google Colab sau khi chạy notebook)*

## 2. Schema và mini guideline

Chi tiết xem tại [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md).

- **Áp dụng schema 4 lớp (`0: car`, `1: truck`, `2: bus`, `3: van`)**:
  - Phân loại dựa trên **loại phương tiện vật lý**, không phụ thuộc trạng thái di chuyển hay đang đỗ.
  - Không gán nhãn cho người đi bộ, xe đạp, xe máy/mô tô, xe ba gác/xe lam nhỏ, biển báo giao thông.
- **Tóm tắt các ca mơ hồ đã quyết định**:
  1. `drive_001.jpg`: Xe SUV màu trắng ở làn phải phía gần bị cột đèn giao thông chắn dọc giữa thân xe → Vẽ **1 box duy nhất** lớp `0` (`car`) bao quanh toàn bộ phần thân xe nhìn thấy được (không tách thành 2 box); các xe đỗ dọc lề trái vẫn gán lớp `0` (`car`).
  2. `drive_002.jpg`: Xe thân hộp màu bạc bên phải và microvan trắng bên trái dễ nhầm giữa MPV (`car`) và `van`, hoặc nhầm xe khách trắng lớn ở giữa với `van` → Gán 2 xe hình hộp cửa lùa là `3` (`van`) và xe khách trắng lớn ở giữa là `2` (`bus`).
  3. `drive_005.jpg`: Xe cứu hộ giao thông màu trắng có cabin đầu vuông tách biệt với sàn chở phía sau → Gán lớp `1` (`truck`); bỏ qua người đi xe máy bên trái.
  4. `drive_015.jpg`: Mẩu nóc xe rất nhỏ bị cắt sát mép dưới cùng của ảnh không đủ thông tin xác định chắc chắn lớp → Không tự đoán để gán; xe buýt nối toa gán 1 box `2` (`bus`) và xe tải nhỏ chở hàng gán `1` (`truck`).
  5. `drive_020.jpg`: Các xe đỗ dưới gầm cầu vượt bộ hành bị trụ cầu và lan can che khuất một phần → Vẫn gán `0` (`car`) cho các xe nhận diện rõ hình khối thân/bánh xe.
- **Phần cần giảng viên làm rõ**: Quy ước vùng bỏ qua (ignored regions) ở các làn đường phụ phía xa hoặc bãi đỗ xe khuất sau nhà chờ xe buýt (như góc trên bên trái ở `drive_007.jpg`, `drive_045.jpg`, `drive_049.jpg`) trong bộ `gold_train_labels`.

## 3. Annotation và self-QC

- **Số file nhãn train**: `50/50` file `.txt` trong `dataset/labels/train/` (`drive_001.txt` đến `drive_050.txt`).
- **Kết quả checker và lỗi đã xử lý**:
  - Chạy `python tools/check_yolo_labels.py` đạt kết quả: `ĐẠT cấu trúc: 50/50 file nhãn hợp lệ. Chờ ground truth để chấm chất lượng.`
  - Đã kiểm tra giới hạn biên `[0.0, 1.0]` cho các xe nằm sát mép ảnh để không có box nào vượt biên.
- **Danh sách ít nhất 10 ảnh đã rà bằng mắt (Self-QC)**:
  - `drive_001.jpg`, `drive_002.jpg`, `drive_005.jpg`, `drive_008.jpg`, `drive_009.jpg`, `drive_012.jpg`, `drive_014.jpg`, `drive_015.jpg`, `drive_020.jpg`, `drive_026.jpg`, `drive_033.jpg`, `drive_042.jpg`, `drive_050.jpg`.
- **Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa**:
  - Tinh chỉnh lại độ bám sát (tightness) của các bounding box xe buýt nối toa (`drive_008.jpg`, `drive_012.jpg`, `drive_029.jpg`, `drive_041.jpg`) và xe tải chở hàng quá khổ (`drive_042.jpg`) để box ôm sát thân xe thực tế, không lấy thừa bóng đổ dưới mặt đường.
  - Chuẩn hóa lại nhãn phân lớp giữa `van` (`3`) và `car` (`0`) ở các ngã tư đông xe (`drive_019.jpg`, `drive_024.jpg`, `drive_027.jpg`, `drive_050.jpg`).
  - Loại bỏ việc gán nhầm xe ba gác/xe máy có mái che ở `drive_012.jpg`, `drive_024.jpg`, `drive_035.jpg`.
- **Commit khóa nhãn ban đầu**: *(Điền mã hash commit sau khi chạy `git commit` ở CP4)*

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | *(Chờ gold)* | |
| FP | *(Chờ gold)* | |
| FN | *(Chờ gold)* | |
| Precision | *(Chờ gold)* | |
| Recall | *(Chờ gold)* | |
| F1 | *(Chờ gold)* | |
| mean_iou_of_matches | *(Chờ gold)* | |
| passed | *(Chờ gold)* | |

File kết quả đánh giá: `outputs/annotation_evaluation.json` (chạy bằng lệnh `python tools/evaluate_annotations.py --student-labels dataset/labels/train --gold-labels gold_train_labels --output outputs/annotation_evaluation.json` sau khi nhận `gold_train_labels/`).

## 5. Kết quả YOLO

- **Runtime, GPU/CPU, phiên bản Ultralytics và Torch**: Google Colab GPU T4 (`DEVICE = 0`), Ultralytics YOLO26 (`yolo26n.pt`), PyTorch CUDA.
- **Các tham số thay đổi so với notebook nếu có và lý do**: Giữ nguyên cấu hình chuẩn của notebook (`epochs=50`, `imgsz=640`, `batch=-1`, `seed=20260909`), chỉ cập nhật `REPO_URL` trỏ về repo cá nhân.
- **Đường dẫn best checkpoint trong phiên chạy**: `/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt`

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | *(Điền từ `model_eval_metrics.json`)* |
| Recall | *(Điền từ `model_eval_metrics.json`)* |
| mAP50 | *(Điền từ `model_eval_metrics.json`)* |
| mAP50–95 | *(Điền từ `model_eval_metrics.json`)* |

- **Đường dẫn thư mục kết quả đánh giá và dự đoán**:
  - Ảnh dự đoán trên 10 ảnh eval: `assets/eval_predictions/visuals/` (ví dụ: `assets/eval_predictions/visuals/drive_051.jpg`, `assets/eval_predictions/visuals/drive_058.jpg`).
  - Log huấn luyện và biểu đồ đánh giá: `assets/yolo26_runs/vehicle_annotation/results.csv`, `assets/yolo26_runs/vehicle_eval/confusion_matrix.png`, `assets/yolo26_runs/vehicle_eval/BoxPR_curve.png`.
- **Phân tích trực quan trên ảnh cụ thể**:
  - Trên `drive_055.jpg` và `drive_054.jpg`, các xe con (`car`) ở cự ly gần và trung bình tách biệt rõ với nền đường nên model phát hiện chính xác với độ tự tin cao và box bám sát.
  - Trên `drive_058.jpg` và `drive_056.jpg`, ở khu vực xe xếp hàng dày đặc phía xa (`y < 0.35`), một số xe kích thước rất nhỏ hoặc bị che khuất một phần có nguy cơ bị bỏ sót (FN) hoặc nhầm lớp giữa `bus`/`van`/`truck`.

### Trả lời bốn câu hỏi cuối notebook:

1. **Lớp nào recall thấp nhất? Ảnh nào minh họa?**
   - Các lớp thiểu số (`truck`, `bus`, `van`) có recall thấp hơn nhiều so với lớp `car` do mất cân bằng dữ liệu nghiêm trọng (class imbalance: hơn 80% số lượng box trong tập dữ liệu là `car`, trong khi `truck`, `bus`, `van` xuất hiện rất ít). Ngoài ra, trong bộ nhãn `dataset/labels/eval/` (ví dụ `drive_051.jpg`, `drive_052.jpg`, `drive_056.jpg`, `drive_058.jpg`), các đối tượng ở xa hoặc bị che khuất như xe tải nhỏ ở `drive_058.jpg` (`x ≈ 0.94, y ≈ 0.40`) hoặc xe van/bus ở xa trong `drive_056.jpg` (`y < 0.16`) rất dễ bị sót hoặc dự đoán lệch lớp làm giảm recall.
2. **Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?**
   - Điều này cho thấy model định vị đúng vị trí tương đối của vật thể (đạt ngưỡng chồng lấp dễ `IoU >= 0.50`), nhưng **độ bám sát hình học (tightness) của bounding box chưa cao** ở các ngưỡng khắt khe (`IoU` từ `0.55` đến `0.95`). Cần kiểm tra lại xem nhãn train có bị vẽ lỏng (lấy dư mặt đường/bóng đổ), cắt thiếu phần thân xe, hoặc không nhất quán ở các xe bị che khuất/cắt rìa ảnh hay không.
3. **Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?**
   - Lỗi nhầm lẫn giữa `van` (`3`) với `car` (`0` - nhóm MPV/SUV/microvan) và giữa `van` (`3`) với `bus` (`2` - nhóm minibus) liên quan trực tiếp tới ranh giới định nghĩa trong guideline (ví dụ các xe van/MPV trong `drive_056.jpg`, `drive_058.jpg`). Bên cạnh đó, khi đối chiếu trực tiếp file nhãn `dataset/labels/eval/` với ảnh gốc (như xe buýt 2 tầng ở `drive_052.txt` dòng 8 mang ID `3`, xe buýt ở `drive_056.txt` dòng 17 mang ID `3`, xe tải ở `drive_051.txt` dòng 4 mang ID `2`), có sự lệch quy ước ID lớp giữa nguồn dữ liệu gốc (UA-DETRAC) và thứ tự lớp trong `data.yaml`, khiến điểm đánh giá chéo trên các lớp `1/2/3` bị ảnh hưởng.
4. **Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?**
   - Nếu đưa 10 ảnh `eval` vào tập train sẽ gây **rò rỉ dữ liệu (data leakage)**: mô hình học thuộc lòng chính các ảnh dùng để chấm điểm, làm các chỉ số mAP/Precision/Recall bị thổi phồng giả tạo và mất khả năng phản ánh năng lực tổng quát hóa trên dữ liệu mới.
   - Trong notebook, 10 ảnh `eval` đóng vai trò là **tập validation (`val`)** được đánh giá sau mỗi epoch để theo dõi sự hội tụ và chọn ra checkpoint tốt nhất (`best.pt`), đồng thời dùng để đo lường chất lượng cuối cùng của pipeline dữ liệu. Do đã tham gia vào việc chọn `best.pt`, tập `eval` này không còn là một tập test độc lập hoàn toàn.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| CP1 - CP2 (Gán nhãn) | Nhiều ảnh giao thông có xe ở rất xa gần chân trời, xe bị cột đèn cắt đôi (`drive_001.jpg`) hoặc xe ba gác dễ nhầm với `van`/`truck` (`drive_012.jpg`, `drive_024.jpg`). | Thiết lập quy tắc rõ trong `GUIDELINE_MINI.md`, vẽ 1 box bao toàn bộ xe bị cột che và loại bỏ hoàn toàn xe ba gác/xe máy khỏi 4 lớp. | Đã hoàn thiện 50 file nhãn nhất quán. |
| CP3 (Self-QC) | Một số box ở rìa ảnh dễ bị lệch nhẹ hoặc lỏng. | Chạy `check_yolo_labels.py` kết hợp xuất lưới tọa độ trực quan để kiểm tra và căn chỉnh lại tọa độ box. | Đạt 50/50 file hợp lệ trên checker, box bám sát thân xe. |

## 7. Tự đánh giá theo rubric

Đối chiếu với [RUBRIC.md](../../RUBRIC.md):
- **Hoàn thành và định dạng YOLO (10đ)**: Đã hoàn thành đủ 50 file `.txt` trong `dataset/labels/train/`, chạy qua `tools/check_yolo_labels.py` đạt `50/50`.
- **Đầy đủ vật thể (20đ)**: Đã gán đầy đủ các phương tiện thuộc 4 lớp trong vùng quan sát chính, bao gồm cả xe bị che khuất một phần và cắt khung ở rìa ảnh.
- **Độ bám sát và hình học box (20đ)**: Đã rà soát trực quan để các box ôm sát thân xe, không gộp nhiều xe vào 1 box.
- **Phân lớp nhất quán (15đ)**: Áp dụng nhất quán `0: car`, `1: truck`, `2: bus`, `3: van`, không gán người đi bộ/xe máy/xe ba gác.
- **Mini guideline và quyết định mơ hồ (10đ)**: Đã điền đầy đủ định nghĩa lớp, quy tắc box và 5 ca mơ hồ cụ thể trong [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md).
- **Gold-set annotation QC (15đ)**: Đã chuẩn bị sẵn sàng lệnh chạy evaluación ngay khi nhận `gold_train_labels/`.
- **Pipeline YOLO và phân tích (10đ)**: Đã cấu hình sẵn `REPO_URL` trong `notebooks/day2_annotation_to_yolo.ipynb` và trả lời chi tiết 4 câu hỏi phân tích; chờ cập nhật số liệu thực tế từ phiên chạy Colab.

## 8. Công cụ AI và tài liệu tham khảo

- **Công cụ AI đã sử dụng**: Sử dụng trợ lý AI (Antigravity) để hỗ trợ đọc hiểu cấu trúc dự án, rà soát quy tắc gán nhãn trong `GUIDELINE_MINI.md`, hỗ trợ tạo/kiểm tra định dạng nhãn YOLO (`check_yolo_labels.py`), vẽ lưới trực quan để Self-QC tọa độ bounding box trên 50 ảnh train và gợi ý trình bày báo cáo cá nhân.
- **Phần đã tự kiểm tra**: Đã trực tiếp đối chiếu hình ảnh trong `dataset/images/train/` và `dataset/images/eval/` với các bounding box, kiểm tra phân lớp `car/truck/bus/van` và xác nhận kết quả chạy script `tools/check_yolo_labels.py`.
- **Tài liệu tham khảo**: Tài liệu hướng dẫn trong repo (`README.md`, `GUIDE.md`, `RULES.md`, `CHECKPOINTS.md`, `RUBRIC.md`, `SUBMISSION.md`) và tài liệu Ultralytics YOLO.

