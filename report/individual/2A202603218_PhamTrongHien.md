# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên:Phạm Trọng Hiện
- MSSV:2A202603218
- Email:hienpt.uia@gmail.com
- Lớp:2A-LAB-C304
- Link repo bài làm:https://github.com/phmhienn/K5-L2-DAY02-PhamTrongHien-2A202603218-ObjectDetection
- Link Colab đã chạy:https://colab.research.google.com/drive/1Az4590TABsiyrQ-FKKuSB6E1yCxZcFFC

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- **Số file nhãn train:** 50 file nhãn định dạng .txt trong thư mục dataset/labels/train/
- **Kết quả checker và lỗi đã xử lý:**
  - Lệnh thực thi: `python tools/check_yolo_labels.py`
  - Kết quả: `ĐẠT cấu trúc: 50/50 file nhãn hợp lệ. Chờ ground truth để chấm chất lượng.`
  - Kiểm tra đạt đầy đủ các yêu cầu: Đủ 50 file khớp đúng basename, mỗi dòng có đúng 5 giá trị (`class_id x_center y_center width height`), `class_id` thuộc tập hợp `{0, 1, 2, 3}`, tọa độ normalized nằm trong khoảng `[0, 1]`, `width > 0, height > 0` và không có box nào vượt ra ngoài biên ảnh.
  - Lỗi đã xử lý khi self-QC: Trong quá trình gán nhãn ban đầu, một số box sát rìa ảnh bị kéo hơi quá mép đã được căn chỉnh và clip lại chính xác trong khung hình.
- **Danh sách ít nhất 10 ảnh đã rà bằng mắt:** drive_001, drive_005, drive_010, drive_014, drive_019, drive_024, drive_026, drive_030, drive_040, drive_046
- **Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:**
  -Trong các ảnh có mật độ phương tiện dày đặc hoặc góc nhìn xa, lúc đầu dễ bỏ sót các xe nhỏ ở cuối hàng hoặc xe bị che lấp một phần bởi xe khác. _Cách sửa:_ Phóng to ảnh (zoom in) để rà soát kỹ từng làn đường, bổ sung box cho tất cả các phương tiện còn nhận diện được đặc trưng.
- **Commit khóa nhãn ban đầu:** `054a2a17e86db6e1cdc4e797b787457c2bd91d02` (commit: `054a2a1` - Add files via upload)

## 4. Gold QC và rework

| Chỉ số              | Trước sửa | Sau sửa nếu có |
| ------------------- | --------: | -------------: |
| TP                  |       434 |                |
| FP                  |       441 |                |
| FN                  |       219 |                |
| Precision           |     0.496 |                |
| Recall              |    0.6646 |                |
| F1                  |    0.5681 |                |
| mean_iou_of_matches |    0.8506 |                |
| passed              |     false |                |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Python 3.12.10 Ultralytics:8.4.208
  PyTorch: 2.15.0+cu132
  GPU RTX 4050
- Các tham số thay đổi so với notebook nếu có và lý do:
- Đường dẫn best checkpoint trong phiên chạy: assets/yolo26_runs/vehicle_annotation/weights/best.pt

| Chỉ số trên 10 ảnh eval |             Giá trị |
| ----------------------- | ------------------: |
| Precision               | 0.39857324140308237 |
| Recall                  | 0.23040780141843972 |
| mAP50                   | 0.27150763609620676 |
| mAP50–95                | 0.19891278253882894 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
   Bus ảnh drive_058
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
   mAP50 cao nhưng mAP50-95 thấp cho thấy Bounding Box chưa định vị chính xác, đặc biệt ở các ngưỡng IoU cao.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
   Có dấu hiệu nhầm lẫn giữa car và van. Cần thống nhất guideline phân biệt truck, bus, van và car.
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?Không dùng ảnh eval để train vì gây Data Leakage, Overfitting và làm mất tính khách quan của kết quả đánh giá.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm   | Lỗi quan sát                                                         | Cách đã thử                                            | Kết quả/còn thiếu                                    |
| ---------------- | -------------------------------------------------------------------- | ------------------------------------------------------ | ---------------------------------------------------- |
| Đánh giá mô hình | Chưa xác định chính xác class có Recall thấp nhất chỉ từ ảnh dự đoán | Kiểm tra ảnh eval và đối chiếu metrics theo từng class | Cần bổ sung bảng Precision, Recall và mAP theo class |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.

- Dùng ChatGPT để nhận xét đánh giá mô hình theo dõi các chỉ số và giải thích các định nghĩa
- Dùng tool label bằng model YOLOv7 và có kiểm tra lại
