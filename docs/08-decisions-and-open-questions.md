# 08 — Quyết định và điểm cần xác nhận

Tài liệu này tách **quyết định đã chốt trong DOCX** khỏi **giả định phải kiểm chứng** khi chạy trên OULAD. Mỗi thay đổi quan trọng thêm ngày, người đề xuất/review và tác động tới data, EDA, model, BI, báo cáo.

| ID | Trạng thái | Nội dung và lý do | Task xác nhận |
|---|---|---|---|
| D01 | Chốt theo DOCX | OULAD; Python + Power BI; Logistic Regression; `At_Risk = 1` cho Fail/Withdrawn. | T01, T13 |
| D02 | Chốt khi khởi tạo | Hạt phân tích là `(code_module, code_presentation, id_student)` để tránh đếm/merge sai khi người học có nhiều lượt học. | T02, T07 |
| D03 | Chốt khi khởi tạo | Sleep, stress, motivation, physical activity, attendance, study hours và previous grade không có trong OULAD; dùng proxy có nhãn riêng hoặc bỏ. Không chế dữ liệu. | T03, T07–T11 |
| D04 | Cần xác nhận | Mốc dự báo sớm tính từ ngày bắt đầu học phần (ứng viên: ngày thứ 28) và cửa sổ feature; kiểm tra coverage, thời gian assessment, bài nộp và VLE. | T13 |
| D05 | Cần xác nhận | Ngưỡng Engagement_Level, Study_Intensity_Proxy, Risk_Band và ngưỡng phân lớp; định nghĩa trước khi công bố insight. | T07, T13 |
| D06 | Cần xác nhận | Map theo At-Risk Rate by Region hay Average Performance by Region; kiểm tra mapping vùng UK và cỡ mẫu. | T04, T09 |
| D07 | Cần xác nhận | `Average Score` trên Overview là điểm bài đã nộp hay thay KPI khác nếu gây hiểu nhầm; giữ rõ mẫu số và không gọi là điểm cuối khóa. | T04, T09 |
| D08 | Cần xác nhận | 8 loại visual khả thi trong Power BI, đặc biệt boxplot/heatmap; phương án thay thế nếu custom visual không khả dụng. | T04, T14 |
| D09 | Cần xác nhận | Cách split train/test theo `id_student` hoặc presentation và độ cân bằng nhãn; quyết định dựa trên audit và mục tiêu tổng quát hóa. | T13 |
| D10 | Chốt theo yêu cầu nhóm | Lịch gợi ý trong DOCX không áp dụng cho repo; nhóm quản lý theo task, phụ thuộc và cổng kiểm tra chất lượng, tự quyết deadline. | T01–T22 |
| D11 | Cần xác nhận | T01 kiểm kê 7 CSV tại `data/raw/uci-open-university-learning-analytics-dataset/` ngày 29/09/2026: 10.900.970 dòng tổng, header/khóa cần thiết đạt và checksum ghi tại `data/README.md`. Thư mục và `OULAD.names` cho thấy bản UCI; UCI công bố CC BY 4.0. Ngày tải archive/version chính xác chưa được lưu riêng. `OULAD.names` ghi 32.953 lượt học/đăng ký, trong khi hai CSV cục bộ đều có 32.593 dòng; dùng số liệu file thực và giữ chênh lệch này để T02/T05 xác minh, TV3 leader duyệt trước khi đóng Issue. | T01 / [Issue #1](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/1) |

## Mẫu ghi quyết định mới

`Ngày — ID — Người đề xuất/review — Bối cảnh — Phương án chọn — Bằng chứng — Phần bị ảnh hưởng — Task/PR`.
