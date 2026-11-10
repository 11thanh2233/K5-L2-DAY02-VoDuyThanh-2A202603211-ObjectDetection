# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp     | Định nghĩa của bạn                                                                                                                                                 | Ví dụ/điểm phân biệt                                                                                                                                                      |
| ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `car`   | Xe con chở người, thân thấp, 4–6 chỗ, gầm thấp, không có thùng hàng riêng biệt.                                                                                    | car có nóc thấp, đuôi xe ngắn, cửa sổ hai bên đều quanh thân.                                                                                                             |
| `truck` | Xe tải/xe chở hàng: có cabin riêng và thùng hàng phía sau.                                                                                                         | Dấu hiệu chính: cabin tách khỏi thùng hàng, thân dài/cao, gầm cao.                                                                                                        |
| `bus`   | Thân dài, nhiều hàng cửa sổ liên tục, nóc cao, thường ≥ 16 chỗ.                                                                                                    | Phân biệt với `van`: bus dài hơn nhiều, có dãy cửa sổ lớn dọc thân, cửa lên xuống riêng.                                                                                  |
| `van`   | Xe tải nhẹ/xe chở người dạng hộp (minivan, MPV cỡ lớn, van chở hàng, minibus nhỏ ~7–12 chỗ): thân liền khối, nóc cao hơn car, đuôi vuông, không có cabin tách rời. | Phân biệt với `car`: thân hộp, nóc cao, đuôi vuông, phần sau thường kín (ít/không có cửa sổ ở van chở hàng). Phân biệt với `truck`: thân liền khối, không có thùng riêng. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: hình chữ nhật nhỏ nhất bao trọn phần **nhìn thấy** của thân xe (từ mép ngoài cùng trái–phải, nóc–bánh/gầm xe), căn đúng mép pixel, không chừa viền thừa. Không tính bóng đổ, phản chiếu, gương chiếu hậu thò xa, cần ăng-ten; có tính các bộ phận gắn liền thân xe.
- Vật thể bị che khuất: nếu còn nhìn thấy khoảng ≥ 30–40% thân xe và đủ để nhận ra lớp thì gán nhãn, box chỉ bao **phần nhìn thấy** (không suy đoán phần bị che). Nếu bị che gần hết (< ~30%) hoặc không thể xác định lớp thì không gán nhãn.
- Vật thể bị cắt bởi rìa ảnh: vẫn gán nhãn nếu phần còn lại đủ nhận ra lớp (≥ ~30–40%); box kéo sát đến rìa ảnh, không vượt ra ngoài khung ảnh.
- Vật thể rất nhỏ/mơ hồ: đặt ngưỡng tối thiểu. Xe quá nhỏ/xa, nhòe, không phân biệt được lớp thì bỏ qua. Nếu vừa đủ ngưỡng nhưng phân vân lớp, chọn lớp theo hình dáng nổi trội và ghi vào bảng ca mơ hồ.
- Các đối tượng không thuộc 4 lớp (xe máy, xe đạp, người đi bộ, xe ba gác/xe đặc chủng khác…): không gán nhãn theo schema này.
- Xe đang đỗ, xe ở xa hoặc xe đối hướng đều gán nhãn như xe đang chạy nếu thỏa các điều kiện trên.
- Nhất quán là ưu tiên số 1: khi đã chọn một quyết định cho một kiểu ca, áp dụng y như vậy cho mọi ảnh sau đó.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh         | Tình huống                                                                      | Quyết định cuối | Lý do theo schema                                                                                                                                                                       |
| ----------- | ------------------------------------------------------------------------------- | --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `drive_003` | hình ảnh phương tiện mơ hồ, không xác định được loại phương tiện                | Không gán lớp   | Do phương tiện này nhiều khả năng không thuộc schema nào do hình dáng phương tiện không khớp mô tả cũng như khả năng nhìn thấy phương tiện < 30%                                        |
| `drive_008` | phương tiện ở giữa 2 chiếc car màu đen ở giữa màn hình là car hay van?          | `van`           | thân phương tiện khá dài và liền khối, chở người, có đuôi vuông và nóc cao hơn car thông thường                                                                                         |
| `drive_012` | phương tiện có nửa thân dưới màu xanh và nửa thân trên màu trắng là bus hay van | `bus`           | dù thân phương tiện dài, liền khối, chở nhiều người, đuôi vuông và các đặc điểm của một chiếc xe van nhưng phương tiện này lại dài và cao hơn nhiều so với van nên chon bus theo schema |

