# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Võ Duy Thành
- MSSV: 2A202603211
- Email: 11thanh2233@gmail.com
- Lớp: L2A - E402
- Link repo bài làm: https://github.com/11thanh2233/K5-L2-DAY02-VoDuyThanh-2A202603211-ObjectDetection.git
- Link Colab đã chạy: https://colab.research.google.com/drive/1zyMDi_ZLRGvsQQJQWLnhEV2I0vn9D5aY?usp=sharing

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train: 50
- Kết quả checker và lỗi đã xử lý: Đạt
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: tất cả 50 ảnh
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: không có
- Commit khóa nhãn ban đầu: /dataset/labels/train/

## 4. Gold QC và rework

| Chỉ số              | Trước sửa | Sau sửa nếu có |
| ------------------- | --------: | -------------: |
| TP                  |       480 |   Không rework |
| FP                  |       602 |   Không rework |
| FN                  |       173 |   Không rework |
| Precision           |    0.4436 |   Không rework |
| Recall              |    0.7351 |   Không rework |
| F1                  |    0.5533 |   Không rework |
| mean_iou_of_matches |    0.8291 |   Không rework |
| passed              |     false |   Không rework |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Ultralytics 8.4.208  Python-3.13.16 torch-2.14.1+cu130 CUDA:0 (Tesla T4, 14913MiB)
- Các tham số thay đổi so với notebook nếu có và lý do: Không
- Đường dẫn best checkpoint trong phiên chạy: '/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt'

| Chỉ số trên 10 ảnh eval |             Giá trị |
| ----------------------- | ------------------: |
| Precision               |  0.7179020214617489 |
| Recall                  |   0.275995544260992 |
| mAP50                   | 0.34344794189084793 |
| mAP50–95                |  0.2330877562681147 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa? > `truck` có recall thấp nhất vì mAP50 của lớp này là thấp nhất với 0.0137. Ảnh minh họa là 'drive_057.jpg' và 'drive_058.jpg'.
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box? > Cần phân loại và recall của các lớp thiểu số vì box đã chạm đúng phương tiện nhưng chưa khít khi ngưỡng IoU tăng dần lên 0.75-0.95
3. Lỗi class nào có thể liên quan guideline? > Dữ kiện nào hỗ trợ nhận xét? ranh giới giữa `van` so với `car` và `truck` so với `bus`/`van` . Dữ kiện hỗ trợ nhận xét là: FP = 602, FN = 173 và precision = 0.4436
4. Vì sao không dùng 10 ảnh eval để train? > Vai trò validation của chúng trong notebook là gì? Nguy cơ rò rỉ dữ liệu cũng như mất công cụ đo duy nhất, sẽ không còn cách phát hiện overfitting và cuối cùng là lợi ích quá nhỏ bởi thêm 10 ảnh vào 50 ảnh train gần như không cải thiện điều gì. 10 ảnh eval giữ vai trò validation/held-out để đo khả năng tổng quát hóa và chọn checkpoint. Nếu đưa chúng vào train thì mất thước đo độc lập, và không còn so sánh công bằng được trước và sau khi sửa nhãn.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm        | Lỗi quan sát                                                                                    | Cách đã thử                            | Kết quả/còn thiếu                                                                                         |
| --------------------- | ----------------------------------------------------------------------------------------------- | -------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| Train, epoch 3-8      | mAP50 trên val giảm từ 0.272 (epoch 1) xuống 0.0807 (epoch 5) dù cls_loss giảm từ 4.8 xuống 1.7 | Giữ nguyên cấu hình, train đủ 50 epoch | mAP50 hồi lại khoảng 0.32 từ epoch 9-11 rồi đi ngang. Còn thiếu: chưa kiểm tra nguyên nhân                |
| Train, cấu hình batch | `batch=-1` (AutoBatch) chọn batch 44 với 50 ảnh, chỉ có 2 iteration mỗi epoch, ít bước cập nhật | Dùng mặc định AutoBatch                | Val mAP50-95 đi ngang quanh 0.21 từ epoch 11. Còn thiếu: chưa thử batch nhỏ hơn (8-16) hoặc train lâu hơn |

## 7. Tự đánh giá theo rubric

1. Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.
2. Đã hoàn thành và định dạng YOLO
3. Đầy đủ vật thể
4. Box sát phần vật thể nhìn thấy, không lấn nền/gộp vật thể
5. Phân biệt `car/truck/bus/van`, không gán vật thể ngoài scope
6. Đã hoàn thành Mini guideline và quyết định mơ hồ
7. Gold-set annotation QC có F1 ~ 0.55 và mean IoU > 0.8
8. Colab chạy được, không dùng eval để train, có metrics/ảnh eval và nhận xét lỗi
## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.

Tất cả đều được thực hiện bằng tay, chỉ có mini guideline có 1 vài sự hỗ trợ từ AI
