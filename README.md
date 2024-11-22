Bộ công cụ OCR cho tài liệu tiếng Việt

<p align="left"> <a href=""><img src="https://img.shields.io/badge/python-3.7+-aff.svg"></a> </p>

Bộ công cụ này cung cấp một quy trình xử lý OCR cho các tài liệu tiếng Việt (như hóa đơn, giấy tờ tùy thân, giấy phép,...). Dự án hỗ trợ khả năng tùy chỉnh linh hoạt để thích nghi với các yêu cầu cụ thể.

:bookmark_tabs: Thông tin thêm:

Báo cáo: link
Youtube:

<div align="center">Hóa đơn (từ tập dữ liệu SROIE19)</div>

<div align="center">Giấy tờ cá nhân (ảnh từ internet)</div>

Chi tiết quy trình:
Sử dụng thuật toán phát hiện cạnh Canny và tìm các đường viền (contour).
Tách hóa đơn từ ảnh và chuẩn hóa kích thước.
Sử dụng Pixel Aggregation Network (PAN) để phát hiện các vùng chứa văn bản từ hóa đơn, sau đó cắt ra.
Dùng VietOCR để trích xuất văn bản từ các vùng đó, sau đó thực hiện sửa lỗi từ.
Trích xuất thông tin từ văn bản.
Notebook
Notebook để huấn luyện PAN:

Notebook để huấn luyện Transformer OCR:

Notebook để huấn luyện PhoBERT:

Notebook để suy luận:

Quy trình tổng quát

<div align="center">Quy trình chính</div>

<div align="center">Lưu đồ xử lý</div>

Quy trình bao gồm hai giai đoạn (cũng có thể chạy riêng giai đoạn thứ hai):

Giai đoạn đầu: Phát hiện và chỉnh sửa tài liệu trong ảnh, sau đó xác định hướng tốt nhất của tài liệu thông qua "lưu đồ xử lý".
Giai đoạn hai: Chuyển ảnh đã xoay qua toàn bộ "lưu đồ xử lý" để trích xuất thông tin.
Tập dữ liệu
MCOCR-2020 (cho phát hiện)
SROIE19 (cho OCR và trích xuất thông tin)
<img height="400" alt="screen" src="demo/data samples/mcocr_public_145013atlmq.jpg"> <img width="400" alt="screen" src="demo/data samples/mcocr_public_145013bcovr.jpg"> <img height="400" alt="screen" src="demo/data samples/mcocr_public_145014ckynq.jpg">
<img alt="screen" src="demo/data samples/sroie19_1.png"> <img alt="screen" src="demo/data samples/sroie19_2.png"> <img alt="screen" src="demo/data samples/sroie19_3.jpg">
Trọng số đã huấn luyện sẵn
Trọng số PAN trên SROIE19:
Mô hình Kích thước ảnh Trọng số MAP@0.5 Độ chính xác pixel IOU
PAN (cơ bản) 640 x 640 link 0.71 0.95 0.91
PAN (xoay) 640 x 640 link 0.66 0.93 0.88
Trọng số OCR trên MCOCR2021:
Mô hình Trọng số Độ chính xác (chuỗi đầy đủ) Độ chính xác (kí tự)
Transformer OCR link 0.890 0.981
Trọng số PhoBERT trên MCOCR2021:
Mô hình Trọng số Độ chính xác (huấn luyện) Độ chính xác (kiểm tra)
PhoBERT link 0.978 0.924
Suy luận
Cài đặt các phụ thuộc: pip install -r requirements.txt

Chạy toàn bộ quy trình:

css
Copy code
python run.py --input=<ảnh đầu vào> --output=<thư mục đầu ra>
Tham số bổ sung:
--debug: Lưu đầu ra của từng bước
--find_best_rotation: Xác định hướng tốt nhất trước
--do_retrieve: Trích xuất thông tin (dựa trên lớp định nghĩa trong config) hoặc chỉ OCR
Tài liệu tham khảo
https://github.com/WenmuZhou/PAN.pytorch
https://github.com/andrewdcampbell/OpenCV-Document-Scanner
https://github.com/pbcquoc/vietocr
