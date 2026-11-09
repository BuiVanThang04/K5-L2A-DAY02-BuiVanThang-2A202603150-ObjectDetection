# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp     | Định nghĩa của bạn                                     | Ví dụ/điểm phân biệt                                                                  |
| ------- | ------------------------------------------------------ | ------------------------------------------------------------------------------------- |
| `car`   | Nhóm xe con gồm sedan, SUV, taxi và xe bán tả          | Xe bán tải thuộc car, không xếp vào truck. Không bao gồm van, xe buýt hoặc mô tô      |
| `truck` | Nhóm xe tải chở hàng và xe đầu kéo                     | Xe tải thùng hoặc đầu kéo thuộc truck; không phân loại chỉ dựa vào kích thước xe      |
| `bus`   | Nhóm xe buýt và minibus chở khách                      | Phân biệt với van/minivan chở người thuộc van; không phải mọi xe chở khách đều là bus |
| `van`   | Nhóm xe van và minivan dùng để chở hàng hoặc chở người | Việc chở hàng không tự động khiến van thành truck                                     |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Mỗi xe hợp lệ có một box riêng, ôm sát phần thân xe nhìn thấy. Không lấy thêm nền đường, trời hoặc gộp hai xe vào cùng một box
- Vật thể bị che khuất: Vẫn gán nhãn nếu phần nhìn thấy đủ để nhận diện lớp. Vẽ box quanh phần nhìn thấy, không đoán phần bị che
- Vật thể bị cắt bởi rìa ảnh: Vẽ box quanh phần xe còn trong ảnh, cạnh box dừng tại rìa ảnh, không kéo ra ngoài
- Vật thể rất nhỏ/mơ hồ: Nếu không thể xác định chắc lớp, không tự đoán. Ghi tên ảnh và tình huống vào phần ca mơ hồ để hỏi giảng viên

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh             | Tình huống                                                                                                                              | Quyết định cuối                                                    | Lý do theo schema                                                                                          |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------- |
| `drive_012.jpg` | Xe màu kem, thân dài, phía trên bên phải tâm ảnh, cạnh xe buýt xanh; khó phân biệt minibus với minivan                                  | Chưa chốt; hỏi giảng viên cách phân biệt bus và van trong ảnh này  | Schema xếp minibus vào bus, minivan vào van, việc chở khách chưa đủ để phân biệt hai lớp                   |
| `drive_037.jpg` | Xe màu sáng gần giữa ảnh, bên phải xe bồn xanh, bị hàng cây che phần lớn thân và bị nhòe; khó xác định lớp từ phần còn thấy             | Chưa chôt, hỏi giảng viên liệu phần nhìn thấy đã đủ gán nhãn       | Chỉ gán xe bị che khi còn đủ thông tin nhận diện lớp; không suy đoán phần bị khuất                         |
| `drive_008.jpg` | Phần mái,kính của xe màu sáng nhô phía sau xe buýt dài, gần phía đuôi bên phải; phần nhìn thấy rất ít, khó xác định lớp và giới hạn box | Chưa chốt; hỏi giảng viên có đủ căn cứ gán một box riêng hay không | Mỗi xe hợp lệ cần một box riêng quanh phần nhìn thấy, nhưng không tự đoán lớp khi chỉ còn một mảnh quá nhỏ |
