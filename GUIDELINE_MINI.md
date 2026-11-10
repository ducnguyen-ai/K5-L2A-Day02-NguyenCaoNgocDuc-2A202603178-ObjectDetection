# Mini guideline — bài làm cá nhân

> Bản tổng hợp sau khi gán nhãn, có AI hỗ trợ diễn đạt và rà ảnh ngày 10/11/2026. Học viên cần xác nhận các quyết định dưới đây; không coi đây là bằng chứng đã lập guideline trước khi annotation. Nhãn ban đầu được giữ nguyên.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe con, sedan, SUV, taxi, bán tải | Phân loại theo kiểu xe, kể cả xe đang đỗ; bán tải vẫn là car theo schema bài. |
| `truck` | Xe tải hàng, đầu kéo | Có kết cấu cabin/thùng hàng; không dùng kích thước box để phân biệt với xe con. |
| `bus` | Xe buýt, minibus chở khách | Thân xe phục vụ chở nhiều hành khách; phân biệt với van bằng cấu trúc thân/cửa/kính khi nhìn rõ. |
| `van` | Van/minivan chở người hoặc hàng | Thân liền dạng hộp; không suy lớp chỉ từ màu xe hoặc việc xe có cửa kính. |

## Quy tắc chung

- Box bám sát phần xe nhìn thấy; không gộp hai xe, không bao bóng đổ hoặc khoảng nền rộng.
- Xe bị che khuất: gán khi phần nhìn thấy đủ nhận ra lớp; không tự suy ra toàn bộ phần khuất.
- Xe bị cắt bởi rìa ảnh: box kết thúc ở biên ảnh, không vượt khung.
- Xe nhỏ/mơ hồ: phóng to ảnh để kiểm tra; nếu chưa xác định được lớp, ghi nhận để hỏi giảng viên. Không tự đặt ngưỡng kích thước mới hoặc lấy lớp theo phỏng đoán.
- ID cố định: car=0, truck=1, bus=2, van=3. Mô tô, người và cột đường không thuộc bốn lớp.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_001.jpg` | Dãy xe bên kia dải phân cách; một số xe bị che một phần hoặc đang đỗ | Đề xuất giữ nhãn xe đủ nhận dạng, không loại chỉ vì đang đỗ; học viên xác nhận lại | Schema mô tả loại vật thể, không mô tả trạng thái đỗ/chạy. |
| `drive_009.jpg` | Nhiều xe buýt chồng lấp, xe con nằm sát hoặc phía trước xe buýt | Đề xuất mỗi xe một box, bus=2 cho thân xe buýt rõ; rà phần nhìn thấy của xe bị che | Tránh gộp xe hoặc bao cả phần khuất vào một box; cần kiểm hình ảnh cụ thể trước khi sửa. |
| `drive_041.jpg` | Xe buýt lớn bị cắt ở mép trái/dưới; có xe nhỏ che một phần thân | Đề xuất bus=2, giới hạn box trong khung và theo phần nhìn thấy; học viên xác nhận lại | Cắt khung không làm đổi lớp; xe con che phía trước vẫn là vật thể riêng. |

Ảnh đối chiếu: [10 ảnh chồng nhãn hiện tại](report/individual/assets/self_qc/contact_sheet.jpg). Xanh lá: car; cam: truck; cyan: bus; tím: van. Đây là gợi ý rà soát, chưa phải biên bản học viên tự xác nhận hay vòng rework đã hoàn thành.
