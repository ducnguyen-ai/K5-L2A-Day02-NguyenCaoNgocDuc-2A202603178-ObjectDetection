# Báo cáo cá nhân — Ngày 2 Object Detection

> Bản nháp có AI hỗ trợ tổng hợp từ repo và kết quả QC thật. Học viên cần tự xác nhận phần guideline và phân tích trước khi nộp. Gold QC hiện chưa đạt; lần chạy YOLO được yêu cầu ở chế độ thử nghiệm.

## 1. Thông tin học viên

- Họ tên: Nguyễn Cao Ngọc Đức.
- MSSV: 2A202603178.
- Email: [26ai.ducncn@vinuni.edu.vn](mailto:26ai.ducncn@vinuni.edu.vn).
- Lớp: K5-L2A.
- [Repo bài làm](https://github.com/ducnguyen-ai/K5-L2A-Day02-NguyenCaoNgocDuc-2A202603178-ObjectDetection).
- [Mở notebook trên Colab](https://colab.research.google.com/github/ducnguyen-ai/K5-L2A-Day02-NguyenCaoNgocDuc-2A202603178-ObjectDetection/blob/main/notebooks/day2_annotation_to_yolo.ipynb). Link này mở mã nguồn. [Phiên thử nghiệm trên Drive](https://colab.research.google.com/drive/1ffYqPHqhVnkxo3nR8j2tSafWGl36itzn).

## 2. Schema và mini guideline

[Mini guideline](../../GUIDELINE_MINI.md) giữ nguyên ID car=0, truck=1, bus=2, van=3. Ba ca cần xác nhận là xe đỗ/bị che ở drive_001, xe buýt chồng lấp ở drive_009 và xe buýt bị cắt khung ở drive_041. Nội dung được tổng hợp sau annotation; không xác nhận đã hoàn thành CP1 trước khi gán nhãn. Cần hỏi giảng viên khi phần nhìn thấy không đủ phân biệt van/minibus.

## 3. Annotation và self-QC

- Checker thực chạy ngày 10/11/2026: **50/50 nhãn train hợp lệ**, **10/10 nhãn eval hợp lệ**.
- Train: 1.145 box, gồm car 917, truck 34, bus 115, van 79. Eval: 113 box, gồm car 94, truck 5, bus 2, van 12.
- Car chiếm khoảng 80,1% box train. Số mẫu giữa các lớp không cân bằng; đây là thống kê dữ liệu, chưa chứng minh nguyên nhân lỗi model.
- AI đã tạo và xem [ảnh chồng nhãn](assets/self_qc/contact_sheet.jpg) cho drive_001, drive_003, drive_008, drive_009, drive_018, drive_024, drive_028, drive_041, drive_048, drive_050. Học viên chưa xác nhận đã tự rà đủ 10 ảnh.
- Chưa sửa nhãn trong lần hỗ trợ này. Việc checker đạt chỉ chứng minh định dạng; không chứng minh đầy đủ xe, đúng lớp hoặc box sát.
- Commit nhãn ban đầu: `2c0c67b8c3dc47ef3c289b84ea0429fc5438955a` (data label). Đây là commit nhãn đã có trên main; thời điểm khóa chính thức cần học viên xác nhận.

## 4. Gold QC và rework

Nguồn: [annotation_evaluation.json](../../outputs/annotation_evaluation.json), đối chiếu kết quả học viên cung cấp và chạy lại evaluator ngày 10/11/2026 cho kết quả giống hoàn toàn. Matching cùng lớp, IoU ≥ 0,50.

| Chỉ số | Trước sửa | Sau sửa |
|---|---:|---|
| TP | 457 | Chưa rework |
| FP | 688 | Chưa rework |
| FN | 196 | Chưa rework |
| Precision | 0,3991 | Chưa rework |
| Recall | 0,6998 | Chưa rework |
| F1 | 0,5083 | Chưa rework |
| mean_iou_of_matches | 0,8542 | Chưa rework |
| passed | false | Chưa rework |

Mean IoU vượt 0,70 nhưng F1 dưới 0,80, nên **chưa đạt CP5**. Mean IoU chỉ tính trên các cặp khớp, không đại diện cho toàn bộ box. Không lấy mAP model để thay thế F1 annotation.

Ví dụ cần ưu tiên đối chiếu bằng mắt: drive_028 có 22 box học viên, 3 gold và 3 matches; drive_041 có 22 box học viên, 24 gold và 8 matches. Các số này cho biết nơi cần rà; chưa đủ để kết luận box thừa, sai lớp hay gold có phạm vi khác. Cần kiểm quy tắc, ảnh và xác nhận với giảng viên trước khi sửa. Không sao chép gold thành nhãn học viên và không sửa gold.

## 5. Kết quả YOLO

**Đã chạy thử nghiệm đủ 50 epoch trên Google Colab ngày 10/11/2026 theo yêu cầu học viên khi gold QC chưa đạt.** Kết quả model không thay thế CP5.

- Runtime: Python 3.13.16, GPU Tesla T4; Torch 2.14.1+cu130; Ultralytics 8.4.208.
- Nguồn dữ liệu/mã lúc chạy: commit `39dee24b7dd833096980eabe2ed38830b2f06a8f`.
- YOLO26n pretrained; epochs=50, imgsz=640, batch=-1 trên GPU, seed=20260909, workers=2. Workers=2 được đặt rõ cho runtime Colab; model và split được giữ nguyên.
- Log ghi 50 epoch hoàn tất trong 0,023 giờ; [results.csv](assets/yolo26_runs/vehicle_annotation/results.csv) có 50 dòng epoch.
- Best checkpoint trong runtime: `/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt`. Không đưa checkpoint vào repo.
- [Notebook đã chạy, có đầu ra](../../notebooks/day2_annotation_to_yolo.ipynb); [phiên Colab](https://colab.research.google.com/drive/1ffYqPHqhVnkxo3nR8j2tSafWGl36itzn).

Các số dưới đây lấy từ **cell đánh giá riêng của best_model.val** trên 10 ảnh eval, lưu tại [model_eval_metrics.json](assets/model_eval_metrics.json). Không lấy dòng epoch cuối làm số đánh giá best checkpoint. Số val tự động cuối train có chênh lệch nhẹ; báo cáo dùng nhất quán JSON của lần val riêng.

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.1881 |
| Recall | 0.4671 |
| mAP50 | 0.3047 |
| mAP50–95 | 0.2011 |

| Lớp | Precision | Recall | mAP50 | mAP50–95 |
|---|---:|---:|---:|---:|
| car | 0.1972 | 0.9149 | 0.7164 | 0.4666 |
| truck | 0.5028 | 0.2034 | 0.4454 | 0.3029 |
| bus | 0.0050 | 0.5000 | 0.0330 | 0.0231 |
| van | 0.0475 | 0.2500 | 0.0240 | 0.0120 |

Đầu ra kèm theo: [10 ảnh dự đoán eval](assets/eval_predictions/visuals/), [log/biểu đồ train](assets/yolo26_runs/vehicle_annotation/), [biểu đồ eval](assets/yolo26_runs/vehicle_eval/), [confusion matrix](assets/yolo26_runs/vehicle_eval/confusion_matrix.png), [PR curve](assets/yolo26_runs/vehicle_eval/BoxPR_curve.png). Prediction trực quan dùng conf=0,25; không đồng nhất ngưỡng này với ngưỡng dùng tính P/R hoặc confusion matrix trong val.

Phân tích từ kết quả, cần học viên tự đọc và xác nhận:

1. **Truck có recall thấp nhất: 0,2034.** Trong [drive_053.jpg](assets/eval_predictions/visuals/drive_053.jpg), vùng gold truck có tâm chuẩn hóa khoảng (0,2773; 0,4000), ở làn đối diện phía trái, lại mang dự đoán `car 0.81`. Log predict không có truck ở conf=0,25 trên ảnh này. Đây là ví dụ cụ thể của sai lớp; không tự sửa gold dựa trên dự đoán. Lớp truck chỉ có 34 box train và 5 box eval, nên kết quả nhạy với ít mẫu.
2. **mAP50=0,3047 và mAP50–95=0,2011**, chênh 0,1036. Khi yêu cầu IoU cao hơn, chất lượng giảm; cần kiểm box lỏng, lệch tâm và box trên xe che/cắt khung. mAP50 hiện cũng thấp, nên không mô tả kết quả là “mAP50 cao”. Mean IoU annotation=0,8542 chỉ tính các cặp khớp, không bảo đảm model định vị tốt trên eval.
3. Confusion matrix của lần val ghi 2 truck thành car và 3 truck thành bus; 8 van thành bus. Trong [drive_053](assets/eval_predictions/visuals/drive_053.jpg), xe buýt lớn phía dưới còn bị đặt nhiều box car lên các vùng kính/mái thay vì một box bus bám toàn bộ xe. Cần rà lại guideline phân biệt cấu trúc thân xe và quy tắc mỗi xe một box. Mất cân bằng lớp và gold QC thấp là các giả thuyết có dữ kiện hỗ trợ, nhưng chưa chứng minh nguyên nhân duy nhất; cần rework rồi so sánh trong cùng cấu hình. Confusion matrix thuộc lần val, không diễn giải các FP của nó như đếm box ở conf=0,25.
4. Mười ảnh eval có nhãn được dùng làm validation trong train và chọn best checkpoint. Đưa chúng vào train gây rò rỉ dữ liệu; chúng không phải test set độc lập do đã tham gia chọn checkpoint. Giữ 50 train/10 eval và không sửa gold eval để cải thiện điểm.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách xử lý | Kết quả/còn thiếu |
|---|---|---|---|
| Gold QC | F1=0,5083, passed=false | Giữ nguyên kết quả, đề xuất rà theo từng ảnh | Chưa rework; cần học viên kiểm lại |
| Notebook / 10-11-2026 | URL repo còn là mẫu | Thay URL cá nhân và branch main | Đã sửa mã nguồn |
| Notebook / 10-11-2026 | Clone lại có thể ghi đè thay đổi; fallback token không phù hợp | Kiểm origin, từ chối cập nhật khi có thay đổi; bỏ token trong URL | Đã sửa mã nguồn |
| Xuất ZIP | Gom cả outputs có thể kéo theo checkpoint/ZIP cũ | Chỉ chọn ảnh, log và metrics | Đã chạy Colab và tải ZIP; xác nhận 10 ảnh eval, log 50 epoch, không có checkpoint/ZIP lồng nhau |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): đã có đủ 50 nhãn đúng cấu trúc và kết quả gold QC. Chất lượng annotation chưa đạt cổng F1; guideline và rà ảnh cần học viên xác nhận. Pipeline đã chạy đủ 50 epoch, có notebook kèm output, metrics, 10 ảnh eval và biểu đồ; được ghi rõ là thử nghiệm khi gold QC chưa đạt. Không tự nhận điểm cho các bước chưa chạy hoặc chưa tự kiểm.

## 8. Công cụ AI và tài liệu tham khảo

Codex hỗ trợ đọc code, chạy checker, thống kê nhãn, sửa pipeline notebook, tạo ảnh chồng nhãn và tổng hợp bản nháp này. AI điều khiển Colab để chạy train/eval và tải kết quả thật; không sửa bộ nhãn hoặc gold. Phần học viên đã tự kiểm cần được học viên xác nhận trước khi nộp; không trình bày nội dung AI gợi ý như phân tích đã tự thực hiện.

Tài liệu: README.md, RULES.md, GUIDE.md, GUIDELINE_MINI.md, CHECKPOINTS.md, RUBRIC.md, SUBMISSION.md và mẫu báo cáo trong repo. Không đính kèm gold train hoặc thông tin đăng nhập.
