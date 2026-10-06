# 08 — Quyết định và điểm cần xác nhận

Tài liệu này tách **quyết định đã chốt trong DOCX** khỏi **giả định phải kiểm chứng** khi chạy trên OULAD. Mỗi thay đổi quan trọng thêm ngày, người đề xuất, bằng chứng và tác động tới data, EDA, model, BI, báo cáo.

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
| D11 | Cần xác nhận | T01 kiểm kê 7 CSV tại `data/raw/` ngày 29/09/2026: 10.900.970 dòng tổng, header/khóa cần thiết đạt và checksum ghi tại `data/README.md`. `OULAD.names` cho thấy bản UCI; UCI công bố CC BY 4.0. Ngày tải archive/version chính xác chưa được lưu riêng. `OULAD.names` ghi 32.953 lượt học/đăng ký, trong khi hai CSV cục bộ đều có 32.593 dòng; dùng số liệu file thực và giữ chênh lệch này để T02/T05 xác minh, TV3 leader duyệt trước khi đóng Issue. | T01 / [Issue #1](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/1) |
| D12 | Chốt theo quyết định leader | PR và bằng chứng kiểm tra vẫn bắt buộc để truy vết. Không yêu cầu review chéo; leader tự kiểm tra, nghiệm thu, merge và đóng Issue. Không gắn reviewer, mention hoặc bình luận nhắc duyệt để tránh thông báo không cần thiết. | Áp dụng T01–T22 từ 30/09/2026 |
| D13 | Cần leader xác nhận | T05 kiểm lại raw: `?`, không phải ô trống, gồm `imd_band` **1.111** (khác số 1.118 ghi khi T02), `date_registration` 45, `date_unregistration` 22.521, `assessments.date` 11, `studentAssessment.score` 173, `vle.week_from`/`week_to` mỗi cột 5.243. T06 chuyển `?` thành nullable missing ở interim, không impute; raw giữ nguyên. | T02, T05, T06 |
| D14 | Cần leader xác nhận | T05–T07 chạy cục bộ ngày 04/10/2026: `studentVle` có 787.170 duplicate toàn dòng bị loại ở T06 (10.655.280 → 9.868.110 event); event-key lặp không tự xem là lỗi và được aggregate trước join. `clean_dataset.csv` có 32.593 lượt học, 0 duplicate attempt key/0 unmatched dimension. Các aggregate `*_all_time` chỉ mô tả EDA/BI; không dùng cho model trước khi D04/D05 được chốt. | T05–T07, T13 |
| D15 | Chốt theo audit T05 | `date_unregistration = ?` không hoàn toàn tương đương không Withdrawn: 93 lượt `Withdrawn` missing trường này, 10.063 lượt `Withdrawn` có ngày rút. T06 giữ nullable missing, không suy diễn/impute; `date_unregistration` bị cấm khỏi feature model. | T05, T06, T13 |
| D16 | Chờ leader nghiệm thu | Hotfix T07 Tableau 06/10/2026: rebuild từ raw một `clean_dataset.csv`; chuẩn hóa `imd_band` `10-20` → `10-20%`, thêm `imd_band_display` với `Unknown` cho 1.111 missing, fill 0 có chọn lọc cho count/tổng event, bỏ `has_registration_record` zero variance. Không fill score/day/date, không xóa outlier, không thêm prediction. Cần leader xác nhận schema v2 trước khi TV2/TV3 dùng rộng. | T07, T09, T15 |

## Mẫu ghi quyết định mới

`Ngày — ID — Người đề xuất — Bối cảnh — Phương án chọn — Bằng chứng — Phần bị ảnh hưởng — Task/PR`.
