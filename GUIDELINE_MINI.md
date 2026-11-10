# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe con cá nhân và xe chở khách cỡ nhỏ thông dụng (sedan, hatchback, coupe, SUV, crossover, taxi, xe bán tải cabin nhỏ). | Có nắp capo và cốp/khoang hành lý thấp hơn `van`/`bus`; bao gồm cả xe taxi, SUV và xe đang đỗ bên lề. Không gộp xe van thân hộp cao hoặc xe tải có thùng chở hàng riêng. |
| `truck` | Xe tải chở hàng, xe tải thùng kín/thùng bạt/thùng lửng, xe cứu hộ chở hàng hoặc xe đầu kéo. | Có phần cabin đầu xe tách biệt rõ với thùng chở hàng hoặc sàn chở hàng phía sau (ví dụ xe tải nhỏ chở hàng ở `drive_015.jpg`, xe cứu hộ ở `drive_005.jpg`). |
| `bus` | Xe buýt công cộng (1 tầng, 2 tầng, xe buýt nối toa) và xe khách/minibus cỡ trung trở lên dùng chở nhiều hành khách. | Thân xe dài, cao, dãy cửa sổ hành khách kéo dài dọc thân xe, đầu xe phẳng (ví dụ xe buýt du lịch trắng ở `drive_002.jpg`, xe buýt nối toa ở `drive_012.jpg`, `drive_015.jpg`). |
| `van` | Xe van, minivan hoặc microvan dạng hình hộp khép kín dùng chở người hoặc chở hàng nhẹ. | Đầu xe ngắn, thân hình hộp cao hơn sedan/SUV nhưng ngắn hơn `bus` và có cửa lùa bên hông (ví dụ xe van bạc bên phải ở `drive_002.jpg` hoặc microvan trắng). |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Bốn cạnh của bounding box chạm sát điểm ngoài cùng phần thân xe thực tế nhìn thấy được (bao gồm bánh xe, cản trước/sau, nóc xe), không lấy thừa mặt đường, bóng đổ dưới gầm, vệt mờ chuyển động quá dài hay bầu trời/cây cối xung quanh.
- Vật thể bị che khuất: Nếu xe bị cột đèn, hàng rào, dải phân cách, cầu vượt hoặc xe khác che một phần nhưng vẫn nhận diện chắc chắn loại xe thuộc 4 lớp, vẽ 1 bounding box bao quanh phần thân xe nhìn thấy được (không suy đoán phần bị khuất hoàn toàn ở rìa, riêng trường hợp bị cột đèn mảnh cắt ngang thân như `drive_001.jpg` thì vẽ 1 box bao toàn bộ thân xe để không tách 1 xe thành 2 box). Hai xe chồng lấp nhau phải vẽ 2 box riêng biệt cho từng xe.
- Vật thể bị cắt bởi rìa ảnh: Cạnh bounding box dừng đúng tại biên ảnh (`[0, 1]`), tuyệt đối không suy đoán phần thân xe nằm ngoài khung hình. Chỉ gán nhãn khi phần còn lại trong ảnh đủ rõ để xác định chắc chắn lớp của xe.
- Vật thể rất nhỏ/mơ hồ: Các xe ở quá xa gần đường chân trời (chỉ còn vài pixel mờ), hoặc vật thể bị cắt khung chỉ còn mẩu nóc/mũi xe rất nhỏ không đủ căn cứ xác định lớp (`car`/`van`/`truck`), thì không tự đoán để gán mà ghi nhận vào bảng ca mơ hồ bên dưới. Không gán người đi bộ, xe đạp, xe máy/mô tô.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_001.jpg` | Xe SUV màu trắng ở làn phải phía gần bị cột đèn giao thông lớn chắn dọc chính giữa thân xe; đồng thời có dãy xe đỗ trên vỉa hè/làn phụ bên trái. | Vẽ **1 box duy nhất** lớp `0` (`car`) bao trùm hai nửa thân xe SUV trắng nhìn thấy được; vẫn gán đầy đủ lớp `0` (`car`) / `3` (`van`) cho các xe đang đỗ bên trái. | Mỗi xe hợp lệ chỉ được gán 1 box duy nhất (không tách 1 xe thành 2 box khi bị cột mảnh che giữa). Schema phân loại theo loại vật thể chứ không theo trạng thái đang chạy hay đang đỗ. |
| `drive_002.jpg` | Xe màu bạc ở rìa phải giữa ảnh và xe trắng nhỏ bên trái có thân hình hộp cao, dễ nhầm giữa `car` (MPV/SUV) và `van`, hoặc nhầm xe khách trắng lớn ở giữa với `van`. | Gán xe hình hộp màu bạc bên phải và microvan trắng bên trái là `3` (`van`); gán xe khách lớn màu trắng ở giữa đường là `2` (`bus`). | Xe bạc và xe trắng nhỏ có thiết kế thân hộp cửa lùa đặc trưng của `van`/microvan; trong khi xe trắng ở giữa có khoang hành khách dài, kính chắn gió cao và nhiều hàng ghế thuộc `bus`. |
| `drive_005.jpg` | Xe màu trắng ở làn giữa bên phải có cabin đầu vuông và sàn chở/kéo phía sau (dạng xe tải cứu hộ giao thông); bên trái đường có người đi xe máy. | Gán xe trắng là `1` (`truck`); **không gán** người đi xe máy ở làn bên trái. | Xe có cấu trúc cabin tải tách biệt với phần sàn công tác/chở hàng phía sau nên thuộc `truck`. Xe máy/mô tô nằm ngoài 4 lớp của schema nên bỏ qua. |
| `drive_015.jpg` | Ở sát mép dưới cùng của ảnh (gần vị trí `x ≈ 0.60, y ≈ 0.98`) chỉ lộ một phần rất nhỏ nóc xe màu tối bị cắt khung; phía trên đường có xe tải nhỏ chở hàng bọc bạt trắng/vàng và xe buýt nối toa. | **Không gán** mẩu nóc xe ở sát đáy ảnh (hoặc cần xác nhận nếu gold set tính cả mẩu cắt >90%); gán xe chở hàng là `1` (`truck`) và nguyên chiếc xe buýt nối toa là **1 box** `2` (`bus`). | Theo quy tắc: nếu vật thể bị cắt khung chỉ còn mảnh rất nhỏ ở mép dưới, khó xác định chắc chắn lớp (`car` hay `van`) thì không tự đoán. Xe buýt nối toa là một phương tiện duy nhất nên vẽ 1 box bao toàn bộ thân xe. |
| `drive_020.jpg` | Các xe ô tô đỗ dưới gầm cầu thang bộ vượt đường (góc trên bên trái) bị lan can cầu thang, trụ cầu và tán cây che khuất nhiều phần. | Vẫn gán lớp `0` (`car`) cho các xe nhìn rõ hình dáng thân/bánh xe dưới cầu thang; bỏ qua các mảng tối phía xa trong bóng râm không nhận diện rõ hình khối xe. | Vật thể bị che khuất một phần nhưng vẫn nhận diện chắc chắn loại xe (`car`) thì phải vẽ box quanh phần nhìn thấy được; phần quá tối/mờ không xác định chắc chắn thì không đoán. |
