# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên:
- MSSV:
- Email:
- Lớp:
- Link repo bài làm:
- Link Colab đã chạy:

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train: 50
- Kết quả checker và lỗi đã xử lý: Đạt cấu trúc 50/50 sau khi sửa các box vượt biên; đã chạy lại bằng Python với UTF-8.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt:
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:
- Commit khóa nhãn ban đầu:174e736f4d532c01220d0d9a3ce65aaa6dbcbdfa

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa hiện tại |
|---|---:|---:|
| TP | 449 | 392 |
| FP | 493 | 290 |
| FN | 204 | 261 |
| Precision | 0.4766 | 0.5748 |
| Recall | 0.6876 | 0.6003 |
| F1 | 0.563 | 0.5873 |
| mean_iou_of_matches | 0.8571 | 0.8471 |
| passed | false | false |

Kết quả lần đầu được ghi ở cột Trước sửa; cột Sau sửa hiện tại lấy từ [annotation_evaluation.json](../../outputs/annotation_evaluation.json). `passed` là kết luận của script theo ngưỡng cũ trong repo.

### Nhật ký lỗi rework do học viên quan sát

Trong quá trình kiểm tra trên CVAT, tôi ghi nhận các lỗi sau. Đây là các vấn đề cần rà lại, không có nghĩa mọi trường hợp đã được xử lý hết.

| Ảnh                                        | Lỗi đã quan sát                                                                            |
| ------------------------------------------ | ------------------------------------------------------------------------------------------ |
| drive_008                                  | Nhầm van thành car; gán car/truck cho xe bị che ở biên, cần kiểm tra lại mức độ nhận diện. |
| drive_009                                  | Nhầm van thành car; bỏ sót car và truck.                                                   |
| drive_014                                  | Gán car cho xe bị che ở biên; nhầm van thành car; phân lớp sai ở vùng ảnh mờ.              |
| drive_024                                  | Bỏ sót các xe car.                                                                         |
| drive_026                                  | Bỏ sót car và bus; nhầm van thành car.                                                     |
| drive_027                                  | Bỏ sót car; nhầm van thành car.                                                            |
| drive_028                                  | Gán car khi ảnh mờ/che lấp, chưa chắc lớp; có box trùng.                                   |
| drive_050                                  | Gán nhiều xe bị che khuất, không nhìn rõ.                                                  |
| drive_001, drive_018, drive_031, drive_043 | Gán car khi chưa đủ chắc chắn về lớp.                                                      |

### Các thay đổi sau khi rà nhãn

Bản nhãn hiện tại đã được hỗ trợ kiểm tra và sửa: bổ sung 43 box bị bỏ sót, đổi 22 nhãn lớp, bỏ 8 box trùng hoặc nhận nhầm, chỉnh 1 box gộp xe. Một số ví dụ:

| Ảnh | Thay đổi | Lý do |
| --- | --- | --- |
| drive_001 | Bỏ box trùng và sửa lớp xe van | Mỗi xe chỉ có một box; phân lớp theo hình dạng thân xe |
| drive_005, drive_024 | Bỏ box gán car cho xe ba bánh | Vật thể không thuộc bốn lớp của bài |
| drive_009 | Đổi bus thành truck cho xe chở hàng | Phân biệt xe tải chở hàng với xe buýt |
| drive_016 | Bỏ box trùng | Không gán hai box cho cùng một xe |
| drive_026, drive_050 | Bổ sung các xe nhìn thấy nhưng chưa có box | Giảm bỏ sót vật thể |
| drive_041 | Sửa nhầm lớp car và van | Đối chiếu hình dạng xe với schema |

Chi tiết phần công cụ hỗ trợ được ghi ở mục 8. Checker sau sửa đạt 50/50 file hợp lệ. Bộ nhãn giữ schema `car=0, truck=1, bus=2, van=3`; gold và script đánh giá không thay đổi.

### Kết quả theo ngưỡng cập nhật

Theo thông báo mới tôi nhận được, ngưỡng F1 của CP5 là **F1 > 0.50**. Kết quả hiện tại **F1 = 0.5873** và **mean IoU = 0.8471**, đáp ứng ngưỡng F1 mới và ngưỡng mean IoU >= 0.70 trong repo.

Trường `passed` trong JSON vẫn là `false` vì script nguyên bản kiểm tra F1 >= 0.80. Tôi giữ nguyên kết quả xuất từ script và ghi riêng ngưỡng cập nhật ở đây.

Sau rework, precision tăng từ 0.4766 lên 0.5748, nhưng recall giảm từ 0.6876 xuống 0.6003. F1 tăng từ 0.5630 lên 0.5873; vẫn còn box không khớp và vật thể bị bỏ sót hoặc phân loại sai, nên kết quả đạt ngưỡng không đồng nghĩa bộ nhãn hoàn toàn chính xác.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch:
- Các tham số thay đổi so với notebook nếu có và lý do:
- Đường dẫn best checkpoint trong phiên chạy:

| Chỉ số trên 10 ảnh eval | Giá trị |
| ----------------------- | ------: |
| Precision               |         |
| Recall                  |         |
| mAP50                   |         |
| mAP50–95                |         |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
| -------------- | ------------ | ----------- | ----------------- |
|                |              |             |                   |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.

### Công cụ đã sử dụng trong quá trình CP5

- Ultralytics Platform: tạo nhãn gợi ý ban đầu; học viên kiểm tra và chỉnh trong CVAT.
- Ultralytics YOLO11s tích hợp CVAT qua Nuclio: tạo bản nhãn gợi ý mới; model không có lớp van riêng.
- Codex: hỗ trợ đọc lỗi, kiểm tra cấu trúc, rà ảnh và trực tiếp sửa một phần nhãn (các thay đổi tóm tắt ở mục 4), chạy evaluator và tổng hợp số liệu vào báo cáo. Phần nhãn được Codex chỉnh là phần có AI hỗ trợ, không ghi nhận là thao tác tự gán của học viên. Không dùng gold để huấn luyện, không sửa gold hoặc ngưỡng chấm.
- Sự cố triển khai: PyTorch trên Windows báo lỗi nạp shm.dll; Docker/WSL gặp lỗi Engine và thiết bị bận; function Nuclio bị kẹt building. Sau khôi phục Docker và triển khai lại, detector đạt ready và chạy được trên CVAT. Kết quả annotation được đánh giá riêng bằng evaluator ở mục 4.
