# 05 — Đặc tả Power BI và kiểm thử tương tác

Dashboard có 4 trang theo DOCX. Mỗi trang nêu hạt, mẫu số và mốc dữ liệu trên tooltip hoặc phần giải thích. Màu cho `At_Risk`/`Not_At_Risk` dùng nhất quán.

| Trang | Nội dung bắt buộc | Bản thiết kế ban đầu |
|---|---|---|
| 1. Overview | Total Students, Average Score, Pass Rate, At-Risk Rate; Histogram, Bar, Map, Line | `Total Students` hiển thị **lượt học** hoặc tách thêm distinct students; `Average Score` là trung bình `studentAssessment.score` của bài đã nộp, gắn nhãn rõ; Pass Rate = `(Pass + Distinction)/lượt học`; At-Risk Rate = `(Fail + Withdrawn)/lượt học`. Histogram score bài đánh giá, bar kết quả, map rủi ro theo region, line engagement theo tuần. |
| 2. Factor Analysis | Scatter, Boxplot, Heatmap, Treemap, Stacked Bar | Engagement so với điểm bài sớm/kết quả; phân phối theo previous attempts/IMD; tương tác hai yếu tố; treemap theo loại tài nguyên hoặc module; kiểm tra cỡ mẫu. |
| 3. Risk Analysis | At-Risk Students; theo Attendance, Study Group, Previous Performance, Region | Dùng **engagement group**, **module/presentation hoặc nhóm hành vi**, **previous attempts/early assessment**, `region`. Không gọi là attendance/study group/previous grade nếu dữ liệu không có. |
| 4. Prediction | Predicted Risk, Risk Probability, Actual vs Predicted, High/Medium/Low | Phân phối xác suất, confusion matrix hoặc actual vs predicted, bảng nhóm nguy cơ, tooltip về mốc dự báo/ngưỡng/số lượng. |

## Kiểm kê loại biểu đồ dự kiến

Tối thiểu 8 **loại khác nhau**, không tính KPI card/biến thể màu là loại mới: (1) histogram, (2) bar, (3) line, (4) geographic map, (5) scatter, (6) boxplot, (7) heatmap/matrix có mã màu, (8) treemap, (9) stacked bar. TV3 xác nhận **cách hiện thực thực tế trong Power BI** và ảnh minh chứng; nếu custom visual bị hạn chế, thay bằng loại chart khác được rubric chấp nhận và ghi quyết định. Việc có 9 loại dự kiến tạo dư địa cho kiểm thử.

## Map và liên kết

Chọn **At-Risk Rate by Region** hoặc **Average Performance by Region** theo DOCX. `region` là vùng ở Anh, không có tọa độ trong OULAD; kiểm tra geocoding/boundary và đối chiếu danh sách region trước khi demo. Nếu Power BI geocode mơ hồ, dùng bảng ranh giới/vùng tin cậy có nguồn trích dẫn, giữ khóa mapping và kiểm tra đủ vùng; không gán tọa độ tự chế. Tooltip map nêu numerator, denominator và số lượt học.

Slicer nhiều cấp: module → presentation → region, thêm nhóm engagement/IMD nếu phù hợp. Drill-down phải có cấp rõ, ví dụ module → presentation → region hoặc region → module; người dùng có thể quay lại cấp trước. Cross-filter giữa các biểu đồ phải được kiểm tra trên từng trang; tooltip hover cho phép xem tỷ lệ, số lượng, nhóm và định nghĩa. Ghi rõ visual nào không nên bị filter để giữ baseline.

## QA dashboard

- Đối chiếu số lượt học, bốn lớp kết quả, pass/at-risk rate và model output giữa Python và Power BI trên cùng bộ filter.
- Kiểm tra 4 trang, 8 loại biểu đồ, map là geographic, tiêu đề/chú thích, filter nhiều cấp, drill-down, tooltip, cross-filter.
- Test tổ hợp module/presentation/region, nhóm không có dữ liệu, reset filter, hiệu năng với bảng VLE đã được tổng hợp.
- Lưu ảnh hoặc video minh chứng, file `.pbix` và bảng QA tại `dashboard/`; kiểm tra kích thước file trước khi commit (dùng Git LFS nếu cần).
