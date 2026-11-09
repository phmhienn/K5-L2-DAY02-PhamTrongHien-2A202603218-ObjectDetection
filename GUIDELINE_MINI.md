# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp     | Định nghĩa của bạn                                                                                                             | Ví dụ/điểm phân biệt                             |
| ------- | ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------ |
| `car`   | Phương tiện cơ giới có bốn bánh hoặc nhiều hơn, chủ yếu dùng để chở người, thường có khoang hành khách kín.                    | Sedan, SUV, hatchback, coupe.                    |
| `truck` | Phương tiện cơ giới được thiết kế chủ yếu để vận chuyển hàng hóa, thường có khoang chở hàng hoặc thùng hàng riêng biệt.        | Xe tải thùng, xe tải ben, xe đầu kéo.            |
| `bus`   | Phương tiện cơ giới có kích thước lớn, được thiết kế để vận chuyển nhiều hành khách cùng lúc.                                  | Xe buýt thành phố, xe khách, xe buýt trường học. |
| `van`   | Phương tiện cơ giới có thân xe dạng hộp, nóc tương đối cao, không gian bên trong rộng, có thể dùng để chở người hoặc hàng hóa. | Passenger van, cargo van, minivan.               |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Vẽ khung hình chữ nhật bao quanh toàn bộ phần vật thể có thể quan sát được, xác định chính xác các điểm ngoài cùng bên trái, bên phải, phía trên và phía dưới. Hạn chế lấy thừa phần nền xung quanh.
- Vật thể bị che khuất: Vẫn đánh dấu vật thể nếu có thể nhận diện được class và xác định tương đối vị trí, kích thước. Box chỉ bao quanh phần nhìn thấy được, không tự vẽ thêm phần bị che khuất.
- Vật thể bị cắt bởi rìa ảnh:Vẫn đánh dấu nếu vật thể còn đủ đặc điểm để nhận diện. Box kéo sát đến mép ảnh tại vị trí vật thể bị cắt, không mở rộng ra ngoài khung hình.
- Vật thể rất nhỏ/mơ hồ: Phóng to ảnh để kiểm tra các đặc điểm nhận dạng. Nếu đủ thông tin thì tiến hành gán nhãn; nếu không thể xác định chắc chắn loại vật thể hoặc class thì đánh dấu để reviewer kiểm tra, không tự suy đoán.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh         | Tình huống                                 | Quyết định cuối                                   | Lý do theo schema                              |
| ----------- | ------------------------------------------ | ------------------------------------------------- | ---------------------------------------------- |
| `drive_014` | không biết là có phải xe bus không         | Cho nó là xe bus vì hình dạng nhận biết khá giống | vì schema mô tả xe bus hình dạng giống với ảnh |
| `drive_044` | các xe ở xa khá nhỏ ko biết là van hay car | cho là car                                        | vì theo mô tả khá giống với car                |
| `drive_040` | xe bị xe bus to xe mất                     | cho là car                                        | dựa vào 1 phần của chiếc xe đối chiếu          |
