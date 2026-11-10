# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Bùi Văn Thắng
- MSSV: 2A202603150
- Email: thangbv04@gmail.com
- Lớp: K5-L2A
- Link repo bài làm: https://github.com/BuiVanThang04/K5-L2A-DAY02-BuiVanThang-2A202603150-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1DEmcnybhf-VJmSuAND3Rq75rc7r3s09t

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

Ba ca sau rà: drive_012 gán bus vì có hình dáng minibus; drive_037 gán car quanh phần nhìn thấy phía trên hàng cây; drive_008 không thêm box cho mảnh mái/kính chưa đủ xác định lớp. Các quyết định được đối chiếu với quy tắc về xe bị che trong guideline.

## 3. Annotation và self-QC

- Số file nhãn train: 50
- Kết quả checker và lỗi đã xử lý: ĐẠT cấu trúc 50/50 file nhãn hợp lệ; đã sửa lỗi box vượt biên ảnh.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: drive_001, drive_008, drive_009, drive_014, drive_018, drive_024, drive_026, drive_027, drive_028, drive_031, drive_043, drive_050.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: Nhầm van thành car; thiếu xe; box trùng; gán nhãn cho vùng quá mờ hoặc bị che khuất. Đã rà lại lớp, bổ sung box cho xe nhận diện được và loại box trùng hoặc vật thể ngoài schema.
- Commit khóa nhãn ban đầu: 174e736f4d532c01220d0d9a3ce65aaa6dbcbdfa

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 449 | 392 |
| FP | 493 | 290 |
| FN | 204 | 261 |
| Precision | 0.4766 | 0.5748 |
| Recall | 0.6876 | 0.6003 |
| F1 | 0.563 | 0.5873 |
| mean_iou_of_matches | 0.8571 | 0.8471 |
| passed | false | false |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

Kết quả: [annotation_evaluation.json](../../outputs/annotation_evaluation.json).

| Ảnh | Thay đổi sau rà nhãn | Lý do |
|---|---|---|
| drive_001 | Bỏ box trùng, sửa lớp van | Mỗi xe một box, đúng loại xe |
| drive_005, drive_024 | Bỏ box car cho xe ba bánh | Không thuộc bốn lớp của bài |
| drive_009 | Đổi bus thành truck cho xe chở hàng | Nhầm lớp |
| drive_016 | Bỏ box trùng | Hai box cùng một xe |
| drive_026, drive_050 | Bổ sung xe bị bỏ sót | Xe nhìn thấy nhưng chưa có box |
| drive_041 | Sửa lớp car và van | Đối chiếu hình dạng thân xe |


## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Google Colab, Tesla T4 (14913 MiB), Python 3.13.16, Ultralytics 8.4.208, Torch 2.14.1+cu130, CUDA device 0.
- Các tham số thay đổi so với notebook nếu có và lý do: Giữ cấu hình train của notebook: YOLO26n, 50 epochs, imgsz=640, batch=-1, seed=20260909; 50 ảnh train và 10 ảnh eval.
- Đường dẫn best checkpoint trong phiên chạy: `/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt`.

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.4091 |
| Recall | 0.2339 |
| mAP50 | 0.2707 |
| mAP50–95 | 0.1996 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Kết quả từng lớp từ cell đánh giá best checkpoint:

| Lớp | Số vật thể | Precision | Recall | mAP50 | mAP50–95 |
|---|---:|---:|---:|---:|---:|
| car | 94 | 0.836 | 0.652 | 0.740 | 0.524 |
| truck | 5 | 0.663 | 0.200 | 0.240 | 0.206 |
| bus | 2 | 0.000 | 0.000 | 0.0141 | 0.00466 |
| van | 12 | 0.136 | 0.0833 | 0.0889 | 0.0635 |

Đầu ra: [10 ảnh dự đoán](assets/eval_predictions/visuals/), [log train](assets/yolo26_runs/vehicle_annotation/results.csv), [biểu đồ train](assets/yolo26_runs/vehicle_annotation/results.png) và [confusion matrix](assets/yolo26_runs/vehicle_eval/confusion_matrix.png).

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?

   Lớp bus có recall thấp nhất, bằng 0 trong bảng đánh giá. Trong [drive_051](assets/eval_predictions/visuals/drive_051.jpg), vật thể được nhãn eval đánh dấu class 2 ở phía trên bên phải không có dự đoán bus tương ứng. Đây là đối chiếu với nhãn eval được cung cấp; không suy ra mọi xe buýt trong toàn bộ ảnh đều không được phát hiện.

2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?

   Cần kiểm tra box có ôm sát phần xe nhìn thấy không, có lấy thêm nền, cắt mất thân xe hoặc gộp hai xe không. Khi yêu cầu IoU cao hơn, box lệch cạnh có thể không còn khớp. Kết quả thực tế mAP50=0.2707 và mAP50–95=0.1996; mAP50 của phiên này chưa cao.

3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?

   Nhầm car và van có thể liên quan cách phân biệt thân xe trong guideline và tính nhất quán của nhãn train. Ở [drive_051](assets/eval_predictions/visuals/drive_051.jpg), xe con màu đỏ lớn phía dưới được dự đoán van với confidence 0.99, trong khi nhãn eval là car. Cần rà tiêu chí phân biệt car/van; một ví dụ này chưa đủ để kết luận guideline là nguyên nhân duy nhất. Tập eval cũng rất nhỏ: chỉ 2 bus và 5 truck, nên chỉ số các lớp này nhạy với từng lỗi.

4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?

   Đưa ảnh eval vào train sẽ gây rò rỉ dữ liệu, khiến kết quả đánh giá không phản ánh khả năng dự đoán trên ảnh chưa dùng để cập nhật trọng số. Trong notebook, 10 ảnh eval được dùng làm validation và chọn best checkpoint, sau đó đánh giá lại trên cùng tập; đây không phải tập test độc lập.


## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| CP3 | Box vượt biên ở một số ảnh | Chỉnh cạnh box và chạy lại checker | Đạt 50/50 file hợp lệ |
| CP5 | Nhầm lớp, box trùng và bỏ sót xe | Rà ảnh, sửa nhãn, chạy lại evaluator | F1=0.5873; mean IoU=0.8471; còn lỗi phân lớp và bỏ sót |
| Tích hợp detector | PyTorch trên Windows lỗi shm.dll; Docker/WSL lỗi Engine; Nuclio kẹt building | Khôi phục Docker/WSL và triển khai lại detector | YOLO11s chạy được trong CVAT |
| Xuất kết quả Colab | Trình giải nén hỏi mật khẩu | Kiểm tra ZIP bằng Python và bỏ ZIP con 89 byte bị lồng bên trong | ZIP ngoài không mã hóa, kiểm tra CRC đạt; đã lấy đủ 10 ảnh và log |
| CP6 | Recall thấp ở bus và van | Đọc chỉ số từng lớp, đối chiếu ảnh dự đoán với nhãn eval | Bus recall=0; van recall=0.0833; có ví dụ car bị dự đoán thành van |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

| Tiêu chí | Kết quả tự đối chiếu |
|---|---|
| Hoàn thành và định dạng YOLO | Có 50 file nhãn, checker đạt 50/50. |
| Đầy đủ vật thể | Đã có rework, nhưng FN=261 cho thấy còn các trường hợp không khớp; không kết luận chỉ do thiếu box. |
| Độ bám sát và hình học box | Mean IoU của các box khớp là 0.8471; chỉ số này không đại diện cho mọi box. |
| Phân lớp nhất quán | Vẫn có nhầm car/van và lỗi ở các lớp ít mẫu, cần tiếp tục rà. |
| Mini guideline và ca mơ hồ | Có định nghĩa, quy tắc và ba ca đã ghi quyết định: drive_012 là bus; drive_037 là car theo phần nhìn thấy; drive_008 không thêm box cho mảnh xe chưa đủ nhận diện. |
| Gold-set annotation QC | Có JSON trước/sau và giải thích rework; giữ nguyên số liệu và kết luận của evaluator. |
| Pipeline YOLO và phân tích | Đã train 50 epochs trên Colab, giữ split 50 train/10 eval, có metrics, 10 ảnh prediction và nhận xét lỗi ở mục 5. |

Chưa tự quy đổi thành điểm; phần đánh giá cuối cùng theo rubric và hướng dẫn của giảng viên.


## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.

- Ultralytics Platform và YOLO11s/CVAT: tạo nhãn gợi ý; học viên rà và sửa trên CVAT.
- Codex: hỗ trợ kiểm tra, trực tiếp sửa một phần nhãn, chạy evaluator và điền số liệu báo cáo.
