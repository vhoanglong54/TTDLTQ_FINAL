# Dashboard Tableau

Tableau là công cụ dashboard duy nhất của dự án theo quyết định D17. Python tiếp tục chịu trách nhiệm xử lý dữ liệu, EDA, Logistic Regression và xuất bảng kết quả; Tableau đọc bảng đã kiểm tra để trực quan hóa và tương tác.

## Trạng thái hiện tại

- Chưa có workbook Tableau được nghiệm thu.
- Mọi scaffold của công cụ cũ đã được loại khỏi source of truth.
- Chưa chốt danh sách hoặc loại biểu đồ cụ thể.
- Chỉ giữ các yêu cầu bắt buộc của rubric: đủ số loại biểu đồ, có geographic map, filter nhiều cấp, drill-down, tooltip và cross-filtering.
- Cấu trúc bốn phần Overview, Factor Analysis, Risk Analysis và Prediction được giữ làm luồng nội dung dự kiến; visual chỉ chốt sau EDA và Insight Log.

## Đầu vào dự kiến

- `data/processed/clean_dataset.csv`: bảng phân tích ở hạt một lượt học `(code_module, code_presentation, id_student)`.
- Output T13: xác suất rủi ro, nhãn thực/dự báo, split, threshold và model version theo đủ ba khóa.
- Geometry/mapping vùng có nguồn và giấy phép nếu dùng geographic map.

Tableau không train lại Logistic Regression theo filter. Python train/đánh giá mô hình và xuất kết quả; Tableau dùng kết quả đó để trình bày KPI, xác suất, phân lớp và sai số.

## Hiện vật sẽ tạo ở T09/T12

- Workbook Tableau `.twb` hoặc `.twbx` và hướng dẫn mở.
- Data source/extract có thể tái tạo; không commit raw data hoặc extract lớn khi chưa có quyết định.
- Bảng công thức KPI, khóa relationship và baseline đối chiếu Python.
- Ảnh/video và checklist tương tác trong `dashboard/evidence/`.

Xem [hướng dẫn bàn giao Tableau](build-guide.md), [trạng thái bố cục](wireframe.md), [baseline QA](qa-t09.md) và [đặc tả](../docs/05-dashboard-spec.md).
