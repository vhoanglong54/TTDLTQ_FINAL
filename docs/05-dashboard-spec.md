# 05 — Đặc tả Power BI và kiểm thử tương tác

Dashboard có 4 trang theo DOCX. [Wireframe T04](../dashboard/wireframe.md) là thiết kế triển khai chính. Mỗi trang nêu hạt, mẫu số và mốc dữ liệu trên tooltip hoặc phần giải thích. Màu cho `At_Risk`/`Not_At_Risk` dùng nhất quán.

| Trang | Nội dung bắt buộc | Bản thiết kế ban đầu |
|---|---|---|
| 1. Overview | Total Students, Average Score, Pass Rate, At-Risk Rate; Histogram, Bar, Map, Line | `Total Students` hiển thị **lượt học** hoặc tách thêm distinct students; `Average Score` là trung bình `studentAssessment.score` của bài đã nộp, gắn nhãn rõ; Pass Rate = `(Pass + Distinction)/lượt học`; At-Risk Rate = `(Fail + Withdrawn)/lượt học`. Histogram score bài đánh giá, bar kết quả, map rủi ro theo region, line engagement theo tuần. |
| 2. Factor Analysis | Scatter, Boxplot, Heatmap, Treemap, Stacked Bar | Engagement so với điểm bài sớm/kết quả; phân phối theo previous attempts/IMD; tương tác hai yếu tố; treemap theo loại tài nguyên hoặc module; kiểm tra cỡ mẫu. |
| 3. Risk Analysis | At-Risk Students; theo Attendance, Study Group, Previous Performance, Region | Dùng **engagement group**, **module/presentation hoặc nhóm hành vi**, **previous attempts/early assessment**, `region`. Không gọi là attendance/study group/previous grade nếu dữ liệu không có. |
| 4. Prediction | Predicted Risk, Risk Probability, Actual vs Predicted, High/Medium/Low | Phân phối xác suất, confusion matrix hoặc actual vs predicted, bảng nhóm nguy cơ, tooltip về mốc dự báo/ngưỡng/số lượng. |

## Kiểm kê loại biểu đồ đã chốt ở T04

Thiết kế có 16 visual instances và 12 **loại khác nhau**, không tính KPI card/table: donut, clustered column, Azure Maps filled map, line, scatter/bubble, box-and-whisker, matrix heatmap, 100% stacked bar, treemap, decomposition tree, histogram và ribbon. Boxplot là custom có điều kiện; fallback là histogram + percentile/median. Heatmap dùng Matrix + conditional formatting. T14 chỉ đánh dấu rubric sau khi có ảnh kiểm thử visual thực tế.

## Map và liên kết

Chọn **At-Risk Rate by Region** theo D06. `region` là nhãn vùng OULAD, không có tọa độ; T09 phải kiểm tra geocoding/boundary đủ 13/13 region. Nếu Power BI geocode mơ hồ, dùng lookup có nguồn trích dẫn, giữ khóa mapping; không đổi nhãn hoặc gán tọa độ tự chế. Tooltip map nêu numerator, denominator và số lượt học.

Slicer nhiều cấp: module → presentation → region, thêm nhóm engagement/IMD nếu phù hợp. Drill-down phải có cấp rõ, ví dụ module → presentation → region hoặc region → module; người dùng có thể quay lại cấp trước. Cross-filter giữa các biểu đồ phải được kiểm tra trên từng trang; tooltip hover cho phép xem tỷ lệ, số lượng, nhóm và định nghĩa. Ghi rõ visual nào không nên bị filter để giữ baseline.

## QA dashboard

- Đối chiếu số lượt học, bốn lớp kết quả, pass/at-risk rate và model output giữa Python và Power BI trên cùng bộ filter.
- Kiểm tra 4 trang, 8 loại biểu đồ, map là geographic, tiêu đề/chú thích, filter nhiều cấp, drill-down, tooltip, cross-filter.
- Test tổ hợp module/presentation/region, nhóm không có dữ liệu, reset filter, hiệu năng với bảng VLE đã được tổng hợp.
- Lưu ảnh hoặc video minh chứng, file `.pbix` và bảng QA tại `dashboard/`; kiểm tra kích thước file trước khi commit (dùng Git LFS nếu cần).
