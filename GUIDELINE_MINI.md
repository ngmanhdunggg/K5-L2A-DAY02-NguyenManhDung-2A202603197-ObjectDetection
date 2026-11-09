# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe con, sedan, SUV, taxi và xe bán tải | Xe bán tải thuộc `car` theo schema của bài, không xếp vào `truck` |
| `truck` | Xe tải chở hàng và xe đầu kéo | Phân biệt với xe bán tải thuộc `car` và xe van thuộc `van` |
| `bus` | Xe buýt và minibus chở khách | Nếu khó phân biệt minibus với minivan, ghi lại và hỏi giảng viên |
| `van` | Xe van hoặc minivan chở hàng hay chở người | Không gồm xe con thông thường hoặc xe buýt; ca khó phân biệt cần hỏi giảng viên |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: vẽ hình chữ nhật ôm sát phần xe nhìn thấy được, hạn chế lấy thêm nền hoặc xe bên cạnh. Mỗi xe có một box riêng.
- Vật thể bị che khuất: vẫn gán nhãn nếu đủ đặc điểm nhận diện lớp; box bao quanh phần nhìn thấy, không suy đoán phần bị che.
- Vật thể bị cắt bởi rìa ảnh: chỉ vẽ box cho phần nằm trong ảnh, cạnh box chạm rìa ảnh tương ứng; không kéo box ra ngoài ảnh.
- Vật thể rất nhỏ/mơ hồ: nếu không xác định chắc lớp thì không tự đoán; ghi tên ảnh, mô tả tình huống và hỏi giảng viên.
- Xe đang đỗ vẫn được gán nhãn theo loại xe.
- Không gán người, xe máy, xe đạp, hình xe trên quảng cáo hoặc ảnh phản chiếu trong gương/kính.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_002` | Xe con bị xe khác che một phần nhưng vẫn nhận diện được loại xe | Gán `car`, box ôm sát phần nhìn thấy | Xe bị che vẫn phải gán nếu nhận diện được lớp; không đoán phần bị che |
| `drive_028` | Xe con màu bạc ở góc dưới bên phải bị cắt bởi rìa ảnh | Gán `car`, box ôm sát phần xe nhìn thấy và chạm rìa ảnh | Phần còn lại đủ nhận diện lớp; không suy đoán phần xe nằm ngoài ảnh |
| `drive_041` | Xe buýt khớp nối gồm hai phần thân, dễ nhầm thành hai vật thể | Gán một box `bus` bao quanh toàn bộ phần xe nhìn thấy | Hai phần thân nối liền thuộc cùng một xe; mỗi xe chỉ có một box |
