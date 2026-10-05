# T09 — Hướng dẫn dựng Power BI skeleton

Tài liệu này biến [wireframe T04](wireframe.md) thành `.pbix` ban đầu. T09 chỉ dựng data model, KPI, map test và skeleton; T12 mới chốt storyline sau Issue #6.

## 1. Điều kiện đầu vào

- Power BI Desktop trên Windows.
- `data/processed/clean_dataset.csv` từ `main`.
- [Core measures](measures.dax), [theme](theme.json) và [baseline QA](qa-t09.md).

Không import bảy CSV raw vào Power BI và không join event table tại đây. T07 đã aggregate đúng hạt lượt học.

## 2. Import và data model

1. Mở Power BI Desktop → **Get data → Text/CSV** → chọn `data/processed/clean_dataset.csv`.
2. Chọn **Transform Data**, đặt query/table name là `clean_dataset`.
3. Xác nhận 32.593 dòng, 35 cột; không bật Auto date/time nếu nó tạo hierarchy sai cho các cột ngày tương đối.
4. Đặt kiểu dữ liệu:
   - Text: module, presentation, gender, region, education, IMD, age, disability, result và performance level.
   - Whole number: `id_student`, attempt/credit/count/date-relative fields, `At_Risk` và cột flag.
   - Decimal number: các cột score mean/min/max khi Power Query không suy ra đúng.
5. **Close & Apply**. T09 dùng mô hình một bảng nên không tạo relationship giả.
6. Tạo một bảng `_Measures` bằng **Enter data** nếu muốn gom measure; ẩn cột placeholder.
7. Tạo lần lượt các measure trong `measures.dax`, định dạng rate là Percentage và score là Decimal number.
8. **View → Themes → Browse for themes** và chọn `dashboard/theme.json`.

## 3. Dựng Page 1 — Overview trước

1. Tạo ba slicer `code_module`, `code_presentation`, `region` và nút Reset bằng bookmark.
2. Tạo card: Learning Attempts, Distinct Learners, Pass Rate, At-Risk Rate, Average Assessment Score.
3. Donut: Legend=`final_result`, Values=`Learning Attempts`.
4. Clustered column: Axis=`code_module`, Legend=`final_result`, Values=`Learning Attempts`; hierarchy thêm `code_presentation` để drill-down.
5. Azure Maps filled map:
   - Location=`region`.
   - Color/measure=`At-Risk Rate`.
   - Tooltip=`Learning Attempts`, `At-Risk Count`, `At-Risk Rate`.
   - Kiểm tra từng region với baseline; không tự đổi `Ireland` hoặc gán tọa độ.
6. Vị trí line chart giữ placeholder “Weekly VLE table required”; không giả lập tuần từ `*_all_time`.

## 4. Dựng skeleton Page 2–4

### Factor Analysis

- Scatter: X=`Average VLE Clicks per Attempt` hoặc aggregate clicks; Y=`Average Assessment Score`; Details/Legend theo nhóm phù hợp; Size=`Learning Attempts`.
- Matrix heatmap: Rows=`imd_band`, Columns=nhóm engagement sau D05, Values=`At-Risk Rate`; trước D05 để placeholder.
- 100% stacked bar: Axis=`highest_education` hoặc `age_band`, Legend=`final_result`, Values=`Learning Attempts`.
- Treemap: Group=`code_module`, Values=`Learning Attempts`, tooltip thêm At-Risk Rate.
- Boxplot để placeholder nếu custom visual chưa được phép; fallback ghi trong wireframe.

### Risk Analysis

- Decomposition tree: Analyze=`At-Risk Rate`; Explain by module → region → education → previous attempts.
- Histogram: tạo numeric bins cho `vle_total_clicks_all_time`; legend=`At_Risk`.
- Ribbon: Axis=`code_presentation`, Legend=`code_module`, Values=`At-Risk Rate`.
- Luôn hiển thị `N`; không kết luận nhóm nhỏ như một quy luật.

### Prediction

- Tạo trang, tiêu đề và khung visual theo wireframe.
- Chỉ đặt thông báo **Awaiting T13 model output**.
- Không tạo probability, predicted status hoặc metric giả.

## 5. Tương tác mẫu bắt buộc ở T09

- Sync ba slicer chính trên bốn trang.
- Dùng **Edit interactions**: chọn cột module trên Page 1 phải lọc donut và map.
- Bật drill hierarchy module → presentation cho clustered column.
- Tạo tooltip chứa Dynamic Filter Context, Attempts, At-Risk Count/Rate và Distinct Learners.
- Kiểm tra reset bookmark trở về trạng thái không filter.

## 6. Lưu và bàn giao

1. Lưu file thành `dashboard/TTDLTQ_OULAD_v0.pbix`.
2. Chụp ít nhất Overview, map 13 region, drill-down và một trạng thái cross-filter.
3. Lưu ảnh vào `dashboard/evidence/` theo quy ước tên file.
4. Điền checklist `qa-t09.md` và ghi phiên bản Power BI Desktop.
5. Chưa mở PR/đóng Issue #7 cho đến khi T12 tích hợp insight từ Issue #6 và đủ bốn trang v0.

