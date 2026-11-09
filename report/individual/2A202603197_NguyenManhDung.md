# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Nguyễn Mạnh Dũng
- MSSV: 2A202603197
- Email: dung28092k3@gmail.com
- Lớp: 2A-LAB-C304
- Link repo bài làm: https://github.com/ngmanhdunggg/K5-L2A-DAY02-NguyenManhDung-2A202603197-ObjectDetection
- Link Colab đã chạy:  https://colab.research.google.com/drive/1jbGg1yCkG31XO6fDVAnKb2bOrcJmCexo#scrollTo=rIi8KtmtVgrf

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

Ba ca mơ hồ đã ghi nhận:
- `drive_002`: xe con bị che một phần; vẫn gán `car` nếu nhận diện được, box ôm sát phần nhìn thấy.
- `drive_028`: xe con bị cắt ở rìa ảnh; gán `car`, box chạm rìa ảnh và không suy đoán phần ngoài khung.
- `drive_041`: xe buýt khớp nối có hai phần thân; gán một box `bus` vì đây là cùng một xe.
Cần giảng viên làm rõ dấu hiệu khác biệt giữa ID lớp của gold và schema bài, cùng phạm vi gán nhãn xe đỗ sát lề ở một số ảnh.

## 3. Annotation và self-QC

- Số file nhãn train: 50 file
- Kết quả checker và lỗi đã xử lý: Chạy `python tools/check_yolo_labels.py`, kết quả đạt 50/50 file nhãn hợp lệ. Không phát hiện lỗi cấu trúc theo checker.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt:  `drive_001.jpg`, `drive_002.jpg`, `drive_003.jpg`, `drive_004.jpg`, `drive_005.jpg`, `drive_006.jpg`, `drive_007.jpg`, `drive_008.jpg`, `drive_009.jpg`, `drive_010.jpg`.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: - Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: Ở `drive_002`, phát hiện hai box gần trùng nhau trên xe trắng bị cắt ở rìa phải, đang mang nhãn `bus` và `van`. Cần giữ một box cho xe này và xác định lại lớp theo schema. Ngoài ra, xe khách trắng lớn giữa ảnh được gán `bus` nhưng gold là `van`; cần hỏi giảng viên để làm rõ. Chưa sửa nhãn sau khi đối chiếu.
- Commit khóa nhãn ban đầu: 70053cb

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 418 | |
| FP | 367 | |
| FN | 235 | |
| Precision | 0.5325 | |
| Recall | 0.6401 | |
| F1 | 0.5814 | |
| mean_iou_of_matches | 0.8530 | |
| passed | false | |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Google Colab, GPU T4, Ultralytics 8.4.208, Torch 2.14.1+cu130.
- Các tham số thay đổi so với notebook nếu có và lý do: Giữ nguyên cấu hình train của notebook: YOLO26n pretrained, epochs=50, imgsz=640, batch=-1 (AutoBatch), seed=20260909, device=0.
- Đường dẫn best checkpoint trong phiên chạy: `/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt`.

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.409064 |
| Recall | 0.233936 |
| mAP50 | 0.270667 |
| mAP50–95 | 0.199550 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
Theo log đánh giá Colab, lớp bus có recall thấp nhất, bằng 0 với 2 đối tượng trong tập eval. Ở drive_051.jpg, nhãn eval có một box bus phía trên bên phải, nhưng ảnh dự đoán tại confidence 0.25 không có box bus.
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
Cần kiểm tra box có ôm sát xe không: các cạnh có lệch, bao thêm nền hoặc cắt mất phần xe nhìn thấy không. Box có thể đạt IoU 0.50 nhưng không đạt các ngưỡng cao hơn. Chênh lệch này chưa đủ để kết luận nhãn train sai; cần xem trực tiếp box dự đoán và nhãn đối chiếu.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
Nhầm lẫn `car` và `van` có thể liên quan đến cách áp dụng guideline. Trong `drive_051.jpg`, xe đỏ ở phía dưới có dáng xe con nhưng được dự đoán là `van` với confidence 0.99. Cần rà lại tiêu chí phân biệt xe con/SUV với van/minivan và tính nhất quán của nhãn train. Đây là dấu hiệu cần kiểm tra, chưa chứng minh guideline là nguyên nhân duy nhất; dữ liệu ít và mất cân bằng lớp cũng có thể ảnh hưởng.
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
Không dùng 10 ảnh eval để train vì model có thể ghi nhớ các ảnh này, khiến kết quả đánh giá cao nhưng không phản ánh khả năng nhận diện trên ảnh mới. Trong notebook, chúng đóng vai trò validation để theo dõi model qua các epoch và chọn best checkpoint. Vì được dùng để chọn checkpoint, chúng không phải test set độc lập.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Kiểm tra nhãn với gold | F1 = 0.5814, thấp hơn yêu cầu 0.80 | Đối chiếu ảnh và nhãn để tìm box trùng, sai lớp và khác phạm vi gán nhãn | Phát hiện hai box gần trùng ở xe sát rìa phải ảnh `drive_002`; cần sửa và đánh giá lại |
| Đối chiếu phạm vi annotation | Một số xe đỗ trong `drive_001`, `drive_028` được gán nhãn nhưng không có trong gold | Đọc lại quy tắc xe đang đỗ vẫn được gán nhãn | Chưa rõ nguyên nhân khác biệt; cần giảng viên xác nhận |
| Đánh giá model | Recall của `bus` bằng 0 | Xem bảng chỉ số theo lớp và ảnh dự đoán eval | Cần rà tính nhất quán của nhãn và số lượng mẫu từng lớp; chưa xác định được nguyên nhân duy nhất |
| Kiểm tra ID lớp của gold | Có dấu hiệu thứ tự ID của gold khác `data.yaml`: gold có vẻ dùng `0=car, 1=van, 2=truck, 3=bus` | Đối chiếu vị trí box và loại xe ở `drive_002`, `drive_009`; thử ánh xạ trong phép kiểm tra, không sửa gold | Khi giả lập ánh xạ lại, số box khớp tăng từ 418 lên 518. Cần giảng viên xác nhận schema gold trước khi kết luận lỗi phân lớp |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

- Đã có 50 file nhãn train và chạy checker đạt cấu trúc 50/50.
- Đã hoàn thiện mini guideline với định nghĩa bốn lớp, quy tắc box và ba ca mơ hồ.
- Đã chạy đánh giá annotation: F1 = 0.5814, mean IoU = 0.8530. Chưa đạt ngưỡng F1 theo kết quả hiện tại; chưa có kết quả đánh giá lại sau sửa.
- Phát hiện dấu hiệu thứ tự ID lớp của gold khác schema bài và khác biệt phạm vi gán nhãn ở một số ảnh. Cần giảng viên xác nhận trước khi kết luận nguyên nhân FP/FN.
- Đã train YOLO26n trên Colab và đánh giá trên 10 ảnh eval; có metrics, log và ảnh dự đoán. 
- Commit chứa bộ nhãn ban đầu đã push: `70053cb`.

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.

- ChatGPT/Codex: hỗ trợ giải thích schema và chỉ số, cài đặt công cụ.
- Ultralytics YOLO26 Nano: dùng để tạo nhãn gợi ý trong CVAT, sau đó kiểm tra và chỉnh sửa thủ công.