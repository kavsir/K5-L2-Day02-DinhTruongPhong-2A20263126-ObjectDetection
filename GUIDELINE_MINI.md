# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | xe con, sedan, SUV, taxi, xe bán tải | Xe taxi. |
| `truck` | xe tải chở hàng, xe đầu kéo | Xe tải thùng, xe tải ben. |
| `bus` | Xe buýt. | To hơn xe van. |
| `van` | Xe van hoặc minivan dùng để chở hàng hay chở người. | Nhỏ hơn xe buýt và có thể chở đồ. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: vẽ hình chữ nhật ôm sát phần xe nhìn thấy được, hạn chế khoảng trống thừa, không lấy thêm mặt đường hoặc xe bên cạnh. Mỗi xe có một box riêng.
- Vật thể bị che khuất: vẫn gán nhãn nếu phần nhìn thấy đủ để xác định lớp. Box bao quanh phần nhìn thấy, không suy đoán phần bị che. Hai xe chồng lên nhau được vẽ hai box riêng.
- Vật thể bị cắt bởi rìa ảnh: gán nhãn nếu xác định được lớp. Box chạm rìa ảnh tại phía bị cắt, không kéo ra ngoài ảnh hoặc đoán phần xe ngoài khung hình.
- Vật thể rất nhỏ/mơ hồ: không tự ý gán nhãn.
- Xe đang đỗ hoặc đang dừng: vẫn gán nhãn theo loại xe như xe đang di chuyển.
- Phạm vi gán nhãn: chỉ gán xe thật thuộc bốn lớp `car`, `truck`, `bus`, `van`. Không gán người, xe đạp, xe máy, biển báo, đèn giao thông, hình xe trên quảng cáo hoặc xe phản chiếu trong kính/gương.
- Giữ nguyên ID lớp: `0: car`, `1: truck`, `2: bus`, `3: van`. Áp dụng nhất quán các quy tắc và ghi lại quyết định cho những ca mơ hồ.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_001` | Xe quá xa. | Không vẽ box gán nhãn. | Khó nhận diện được. |
| `drive_001` | Xe bị che bởi vật thể. | Gán nhãn được vì có thể nhìn thấy rõ đó là xe gì. | Vì vẫn nhìn thấy xe. |
| `drive_009` | Xe chỉ nhìn thấy được một phần đầu. | Không gán nhãn. | Vì chỉ thấy phần đầu, có thể làm mô hình suy luận sai. |
