# Báo cáo cá nhân — Ngày 2 Object Detection

> Bản nháp có AI hỗ trợ tổng hợp từ repo và kết quả QC thật. Học viên cần tự xác nhận phần guideline và phân tích trước khi nộp. Gold QC hiện chưa đạt; lần chạy YOLO được yêu cầu ở chế độ thử nghiệm.

## 1. Thông tin học viên

- Họ tên: Nguyễn Cao Ngọc Đức.
- MSSV: 2A202603178.
- Email: Chưa được học viên cung cấp để ghi vào bài nộp.
- Lớp: K5-L2A.
- [Repo bài làm](https://github.com/ducnguyen-ai/K5-L2A-Day02-NguyenCaoNgocDuc-2A202603178-ObjectDetection).
- [Mở notebook trên Colab](https://colab.research.google.com/github/ducnguyen-ai/K5-L2A-Day02-NguyenCaoNgocDuc-2A202603178-ObjectDetection/blob/main/notebooks/day2_annotation_to_yolo.ipynb). Link này mở mã nguồn; chưa phải bằng chứng phiên chạy đã hoàn tất.

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

Nguồn: [annotation_evaluation.json](../../outputs/annotation_evaluation.json), đối chiếu kết quả học viên cung cấp. Matching cùng lớp, IoU ≥ 0,50.

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

**Chế độ thử nghiệm theo yêu cầu học viên khi QC chưa đạt.** Không coi lần chạy này là đã vượt cổng annotation.

- Cấu hình dự kiến theo notebook: YOLO26n, 50 epochs, imgsz=640, seed=20260909; batch tự chọn trên GPU, batch=8 nếu CPU; workers=2.
- Runtime, GPU, Torch, Ultralytics: Chờ kết quả phiên chạy thực tế.
- Best checkpoint trong runtime: Chờ kết quả; không commit `.pt` vào repo.

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---|
| Precision | Chưa có kết quả chạy hoàn tất |
| Recall | Chưa có kết quả chạy hoàn tất |
| mAP50 | Chưa có kết quả chạy hoàn tất |
| mAP50–95 | Chưa có kết quả chạy hoàn tất |

Chờ bổ sung `assets/eval_predictions/`, `assets/yolo26_runs/`, metrics và link phiên Colab đã chạy.

Các điểm phân tích cần học viên hoàn thiện sau khi có kết quả:

1. Chưa xác định lớp có recall thấp nhất; phải đọc metric từng lớp và kiểm một ảnh eval cụ thể.
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm độ chính xác định vị và độ sát của box ở các ngưỡng IoU cao. Chưa kết luận tình trạng này xảy ra trong phiên chạy.
3. Chưa có dự đoán để xác nhận nhầm truck/bus/van/car. Cần đối chiếu confusion matrix với ảnh và guideline; class imbalance chỉ là một giả thuyết cần dữ kiện hỗ trợ.
4. Mười ảnh eval có nhãn được dùng làm validation trong train và để chọn best checkpoint. Đưa chúng vào train gây rò rỉ dữ liệu; chúng không phải test set độc lập, do đã tham gia chọn checkpoint.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách xử lý | Kết quả/còn thiếu |
|---|---|---|---|
| Gold QC | F1=0,5083, passed=false | Giữ nguyên kết quả, đề xuất rà theo từng ảnh | Chưa rework; cần học viên kiểm lại |
| Notebook / 10-11-2026 | URL repo còn là mẫu | Thay URL cá nhân và branch main | Đã sửa mã nguồn |
| Notebook / 10-11-2026 | Clone lại có thể ghi đè thay đổi; fallback token không phù hợp | Kiểm origin, từ chối cập nhật khi có thay đổi; bỏ token trong URL | Đã sửa mã nguồn |
| Xuất ZIP | Gom cả outputs có thể kéo theo checkpoint/ZIP cũ | Chỉ chọn ảnh, log và metrics | Đã sửa mã nguồn, chờ kiểm phiên Colab |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): đã có đủ 50 nhãn đúng cấu trúc và kết quả gold QC. Chất lượng annotation chưa đạt cổng F1; guideline và rà ảnh cần học viên xác nhận. Pipeline đã chỉnh nhưng chưa có bằng chứng train/eval hoàn tất. Không tự nhận điểm cho các bước chưa chạy hoặc chưa tự kiểm.

## 8. Công cụ AI và tài liệu tham khảo

Codex hỗ trợ đọc code, chạy checker, thống kê nhãn, sửa pipeline notebook, tạo ảnh chồng nhãn và tổng hợp bản nháp này. AI không sửa bộ nhãn hoặc gold. Phần học viên đã tự kiểm cần được học viên xác nhận trước khi nộp; không trình bày nội dung AI gợi ý như phân tích đã tự thực hiện.

Tài liệu: README.md, RULES.md, GUIDE.md, GUIDELINE_MINI.md, CHECKPOINTS.md, RUBRIC.md, SUBMISSION.md và mẫu báo cáo trong repo. Không đính kèm gold train hoặc thông tin đăng nhập.
