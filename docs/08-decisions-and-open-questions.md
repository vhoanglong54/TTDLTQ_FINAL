# 08 — Quyết định và điểm cần xác nhận

Tài liệu này tách **quyết định đã chốt trong DOCX** khỏi **giả định phải kiểm chứng** khi chạy trên OULAD. Mỗi thay đổi quan trọng thêm ngày, người đề xuất, bằng chứng và tác động tới data, EDA, model, dashboard, báo cáo.

| ID | Trạng thái | Nội dung và lý do | Task xác nhận |
|---|---|---|---|
| D01 | Chốt theo DOCX, công cụ cập nhật tại D17 | OULAD; Python; Logistic Regression; `At_Risk = 1` cho Fail/Withdrawn. Công cụ dashboard hiện theo D17. | T01, T13 |
| D02 | Chốt khi khởi tạo | Hạt phân tích là `(code_module, code_presentation, id_student)` để tránh đếm/merge sai khi người học có nhiều lượt học. | T02, T07 |
| D03 | Chốt khi khởi tạo | Sleep, stress, motivation, physical activity, attendance, study hours và previous grade không có trong OULAD; dùng proxy có nhãn riêng hoặc bỏ. Không chế dữ liệu. | T03, T07–T11 |
| D04 | Cần xác nhận | Mốc dự báo sớm tính từ ngày bắt đầu học phần (ứng viên: ngày thứ 28) và cửa sổ feature; kiểm tra coverage, thời gian assessment, bài nộp và VLE. | T13 |
| D05 | Cần xác nhận | Ngưỡng Engagement_Level, Study_Intensity_Proxy, Risk_Band và ngưỡng phân lớp; định nghĩa trước khi công bố insight hoặc chạy model. | T11, T13 |
| D06 | Mở lại sau quyết định Tableau | Rubric bắt buộc geographic map, nhưng measure/loại map chưa chốt. T09 kiểm tra geographic role hoặc spatial mapping đủ 13 nhãn, nguồn/giấy phép và trường hợp `Ireland`; T12 chốt nội dung sau EDA. | T09, T12 |
| D07 | Chốt thiết kế T04 | KPI dùng tên **Average Assessment Score** = tổng score / số assessment được chấm trong filter context; không gọi Average Score/final score và không average các mean theo lượt học. | T04, T09 |
| D08 | Hủy phương án visual cũ; cần chốt lại | Inventory của phương án công cụ cũ không còn là quyết định triển khai. Chưa chốt loại biểu đồ, layout hoặc theme; T12 chỉ chọn sau EDA/Insight Log và phải bao phủ nguyên vẹn yêu cầu rubric. | T12, T14 |
| D09 | Cần xác nhận | Cách split train/test theo `id_student` hoặc presentation và độ cân bằng nhãn; quyết định dựa trên audit và mục tiêu tổng quát hóa. | T13 |
| D10 | Chốt theo yêu cầu nhóm | Lịch gợi ý trong DOCX không áp dụng cho repo; nhóm quản lý theo task, phụ thuộc và cổng kiểm tra chất lượng, tự quyết deadline. | T01–T22 |
| D11 | Cần xác nhận | T01 kiểm kê 7 CSV tại `data/raw/` ngày 29/09/2026: 10.900.970 dòng tổng, header/khóa cần thiết đạt và checksum ghi tại `data/README.md`. `OULAD.names` cho thấy bản UCI; UCI công bố CC BY 4.0. Ngày tải archive/version chính xác chưa được lưu riêng. `OULAD.names` ghi 32.953 lượt học/đăng ký, trong khi hai CSV cục bộ đều có 32.593 dòng; dùng số liệu file thực và giữ chênh lệch này để T02/T05 xác minh, TV3 leader duyệt trước khi đóng Issue. | T01 / [Issue #1](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/1) |
| D12 | Chốt theo quyết định leader | Quy trình duy nhất: một Issue → một nhánh cụm task → một PR dùng `Closes #issue` và chứa bằng chứng → leader quyết định merge → Issue đóng ngay. Không review chéo, reviewer, mention nhắc duyệt hoặc biên bản lặp lại. | Áp dụng T01–T22 từ 05/10/2026 |
| D13 | Chốt khi nghiệm thu T05–T07 | T05 kiểm lại raw: `?`, không phải ô trống, gồm `imd_band` **1.111** (khác số 1.118 ghi khi T02), `date_registration` 45, `date_unregistration` 22.521, `assessments.date` 11, `studentAssessment.score` 173, `vle.week_from`/`week_to` mỗi cột 5.243. T06 chuyển `?` thành nullable missing ở interim, không impute; raw giữ nguyên. | T02, T05, T06 |
| D14 | Chốt khi nghiệm thu T05–T07 | T05–T07 chạy ngày 04/10/2026: `studentVle` có 787.170 duplicate toàn dòng bị loại ở T06 (10.655.280 → 9.868.110 event); event-key lặp không tự xem là lỗi và được aggregate trước join. `clean_dataset.csv` có 32.593 lượt học, 0 duplicate attempt key/0 unmatched dimension. Các aggregate `*_all_time` chỉ mô tả EDA/dashboard; không dùng cho model trước khi D04/D05 được chốt. | T05–T07, T13 |
| D15 | Chốt theo audit T05 | `date_unregistration = ?` không hoàn toàn tương đương không Withdrawn: 93 lượt `Withdrawn` missing trường này, 10.063 lượt `Withdrawn` có ngày rút. T06 giữ nullable missing, không suy diễn/impute; `date_unregistration` bị cấm khỏi feature model. | T05, T06, T13 |
| D16 | Chốt theo quyết định leader | Theo dõi duy nhất `data/processed/clean_dataset.csv` (32.593 dòng, 35 cột, khoảng 5,8 MB) để nhóm dùng chung. Raw, interim và output processed khác không commit; file dùng chung vẫn phải có script tái tạo và checksum. | T07 và các task phía sau |
| D17 | Chốt theo quyết định leader ngày 06/10/2026 | Công cụ dashboard duy nhất là **Tableau**. Python tiếp tục dùng cho data, EDA và Logistic Regression; output model được đưa vào Tableau dưới dạng bảng đã kiểm tra. Giữ nguyên dữ liệu, model, toàn bộ tiêu chí rubric và file `docs/02-rubric-traceability.md`; chỉ chốt công cụ, **chưa chốt phương án biểu đồ/layout**. Mọi scaffold của phương án công cụ cũ bị loại khỏi source of truth. | T09–T22 / Issue #7 trở đi |

## Mẫu ghi quyết định mới

`Ngày — ID — Người đề xuất — Bối cảnh — Phương án chọn — Bằng chứng — Phần bị ảnh hưởng — Task/PR`.
