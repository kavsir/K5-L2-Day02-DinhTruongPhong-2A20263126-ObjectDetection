# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Đinh Trường Phong
- MSSV:2A202603126
- Email: 26ai.phongdt@vinuni.edu.vn
- Lớp: C204
- Link repo bài làm: https://github.com/kavsir/K5-L2-Day02-DinhTruongPhong-2A20263126-ObjectDetection.git
- Link Colab đã chạy: chạy trên máy cá nhân vì colab lỗi( đã báo cho lab lab coach)

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train: 50
- Kết quả checker và lỗi đã xử lý: ĐẠT cấu trúc, 50/50 file nhãn hợp lệ; không phát hiện lỗi định dạng YOLO trong lần kiểm tra hiện tại.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt:  drive_001, drive_002, drive_003, drive_004, drive_005, drive_006, drive_007, drive_008, drive_009, drive_010.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: xoá box mờ, chỉnh lại sát vật thể
- Commit khóa nhãn ban đầu: gắn nhãn, chạy 2 took

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 382 | 392 |
| FP | 363 | 385 |
| FN | 271 | 261 |
| Precision | 0.5128 | 0.5045 |
| Recall | 0.5850 | 0.6003 |
| F1 | 0.5465 | 0.5483 |
| mean_iou_of_matches | 0.8186 | 0.8183 |
| passed | false | false |

Kết quả hiện tại lưu tại `outputs/annotation_evaluation.json`.
F1 tăng nhẹ nhưng chưa đạt ngưỡng 0.80; mean IoU đạt ngưỡng 0.70.

Khi đối chiếu trực quan drive_009 và drive_041, phát hiện dấu hiệu không khớp mapping lớp và một số box bất thường trong bộ gold.
Cần giảng viên xác nhận lại bộ gold trước khi kết luận nguyên nhân của các lỗi FP/FN.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Chạy trên máy cá nhân Windows, Python 3.12.6; GPU NVIDIA GeForce RTX 4070 (12 GB VRAM), CUDA:0; Ultralytics 8.4.208; Torch 2.11.0+cu128.
- Các tham số thay đổi so với notebook nếu có và lý do: Chuyển từ Google Colab sang máy cá nhân do Colab gặp lỗi, đã báo Lab Coach.
- Đường dẫn best checkpoint trong phiên chạy: `D:\K5-L2-Day02-DinhTruongPhong-2A20263126-ObjectDetection\notebooks\runs\detect\Day2-Lab\outputs\yolo26_runs\vehicle_annotation\weights\best.pt`

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.4 |
| Recall | 0.23 |
| mAP50 | 0.27 |
| mAP50–95 | 0.2 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất?  Ảnh nào minh họa?
Theo bảng kết quả đã chạy, lớp bus có recall thấp nhất, bằng 0 , ảnh trong notebook , kết quả cell train
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
Cần kiểm tra độ chính xác vị trí và kích thước box: box có bị lệch, quá rộng, quá nhỏ hoặc cắt mất phần xe không. Đồng thời kiểm tra cách vẽ box cho xe bị che khuất hoặc cắt khung có nhất quán giữa nhãn train và eval không. Box có thể đạt IoU 0,50 nhưng không đạt các ngưỡng cao hơn.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
Nhầm lẫn bus–van có thể liên quan đến guideline còn dùng mô tả đơn giản như “to hơn xe van” và “nhỏ hơn xe buýt”. Kích thước trên ảnh phụ thuộc khoảng cách nên chưa đủ để phân biệt lớp. Confusion matrix đã lưu có 11 đối tượng mang nhãn thật van được dự đoán là bus
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
Không dùng 10 ảnh eval để cập nhật trọng số vì sẽ gây rò rỉ dữ liệu, khiến kết quả đánh giá không phản ánh khả năng xử lý ảnh chưa dùng để học. Trong notebook, chúng được dùng làm validation để theo dõi quá trình train và chọn best.pt, sau đó đánh giá lại checkpoint này. Vì đã tham gia chọn mô hình, đây không phải test set độc lập.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Gán nhãn và self-QC | Một số xe nhỏ, mờ hoặc bị che khuất gây khó xác định lớp và ranh giới box. | Rà lại ảnh trong CVAT, chỉnh box sát vật thể và xem xét các box khó nhận diện. | Checker đạt 50/50 file; vẫn cần kiểm tra trực quan vì checker chỉ xác nhận cấu trúc. |
| Đối chiếu gold | Phát hiện dấu hiệu khác mapping lớp và box bất thường trong gold ở `drive_009`, `drive_041`. | Xem ảnh chồng box của hai bộ nhãn để đối chiếu; giữ nguyên gold. | Cần giảng viên xác nhận mapping và chất lượng bộ gold; F1 hiện chưa đạt yêu cầu. |
| Chạy notebook | Google Colab gặp lỗi. | Báo Lab Coach và chuyển sang chạy trên máy cá nhân với GPU RTX. | Đã train và đánh giá được mô hình, lưu checkpoint và kết quả. |
| Đánh giá mô hình | Kết quả tổng thể thấp, đặc biệt ở các lớp truck, bus và van. | Xem chỉ số từng lớp, ảnh dự đoán và confusion matrix. | Cần kiểm tra thêm mapping nhãn eval, phân bố lớp train và chất lượng annotation để xác định nguyên nhân. |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

## 8. Công cụ AI và tài liệu tham khảo

không dùng
chỉ dùng codex làm rỏ các chữ tiếng anh để hiểu lại và sửa lại tiếng việt chính tả và ghi có dấu, check các class trên gg để định nghĩa để gắn nhãn, 
