# T04 — Wireframe Power BI bốn trang

**Issue:** [#4](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/4)  
**Owner:** TV3 / leader  
**Trạng thái:** thiết kế sẵn sàng bàn giao cho T09/T12; chưa phải dashboard `.pbix` và chưa đánh dấu các mục Power BI trong rubric.

## 1. Cơ sở dữ liệu và mục tiêu thiết kế

Wireframe dùng hạt chuẩn **một lượt học** `(code_module, code_presentation, id_student)`. Kiểm tra trên `data/processed/clean_dataset.csv` cho thấy:

| Điều kiện khả thi | Kết quả T04 |
|---|---:|
| Lượt học | 32.593 |
| Sinh viên phân biệt | 28.785 |
| Module / presentation / tổ hợp module-presentation | 7 / 4 / 22 |
| Region có dữ liệu | 13/13 |
| Lượt học có assessment được chấm | 25.820 |
| Lượt học có VLE event | 29.228 |
| `imd_band` missing | 1.111 |

Dashboard trả lời theo thứ tự: **điều gì đang xảy ra → yếu tố nào liên hệ → nhóm nào cần chú ý → mô hình dự báo ra sao**. Không dùng từ ngữ nhân quả và không gọi VLE click là attendance/study hours.

## 2. Quy ước toàn dashboard

- Khung thiết kế: 16:9; thanh tiêu đề trên cùng, thanh slicer ngay dưới, vùng nội dung theo lưới 12 cột.
- Slicer đồng bộ bốn trang: `code_module`, `code_presentation`, `region`; trang Factor/Risk có thêm `imd_band` và `final_result` khi phù hợp.
- Màu cố định: At-Risk `#C43D3D`, Not At-Risk `#237A78`, trung tính `#667085`, missing/unknown `#B7BDC8`. Luôn kèm nhãn hoặc số, không truyền ý nghĩa chỉ bằng màu.
- Tiêu đề visual phải nêu measure và đơn vị; tooltip phải có filter context, numerator, denominator và `N`.
- Một nút **Reset filters** dùng bookmark; slicer hiển thị trạng thái chọn hiện tại.
- `*_all_time` chỉ dùng mô tả/BI. Trang Prediction chỉ nhận feature giới hạn cutoff và output từ T13.

## 3. KPI và mẫu số

Tên dưới đây là tên hiển thị; công thức là DAX định hướng để T09 hiện thực và đối chiếu với Python.

| KPI | Định nghĩa | Cảnh báo diễn giải |
|---|---|---|
| Learning Attempts | `COUNTROWS(clean_dataset)` | Không gọi là số sinh viên. |
| Distinct Learners | `DISTINCTCOUNT(id_student)` | Một sinh viên có thể có nhiều lượt học. |
| At-Risk Count | `SUM(At_Risk)` | Nhãn lịch sử Fail/Withdrawn. |
| At-Risk Rate | `DIVIDE([At-Risk Count], [Learning Attempts])` | Mẫu số thay đổi theo filter context. |
| Pass Rate | `(Pass + Distinction) / Learning Attempts` | Nêu rõ Distinction được tính là đạt. |
| Average Assessment Score | `SUM(assessment_score_sum_all_time) / SUM(assessment_scored_count)` | Chỉ trên bài đã được chấm; không phải final score và không average cột mean. |
| Avg VLE Clicks per Attempt | `SUM(vle_total_clicks_all_time) / Learning Attempts` | Proxy tương tác toàn kỳ, không phải thời gian học. |
| Predicted At-Risk Rate | Tỷ lệ `predicted_status = 1` từ T13 | Chưa có trước T13; tooltip ghi model/cutoff/threshold. |

## 4. Trang 1 — Overview

**Mục tiêu:** cho biết quy mô, kết quả tổng quan, khác biệt theo module/region và xu hướng engagement.

```text
┌──────────────────────────────────────────────────────────────────────────┐
│ OVERVIEW          Module ▾  Presentation ▾  Region ▾      [Reset]       │
├────────────┬────────────┬────────────┬────────────┬──────────────────────┤
│ Attempts   │ Learners   │ Pass Rate  │ At-Risk % │ Avg Assessment Score │
├────────────┴────────────┼────────────┴────────────┴──────────────────────┤
│ V01 Donut: result mix   │ V02 Clustered column: result by module        │
├─────────────────────────┼────────────────────────────────────────────────┤
│ V03 Azure filled map: At-Risk Rate by region                            │
├───────────────────────────────────┬──────────────────────────────────────┤
│ V04 Line: weekly VLE engagement   │ Ghi chú KPI, grain và proxy         │
└───────────────────────────────────┴──────────────────────────────────────┘
```

Luồng đọc: KPI → cơ cấu kết quả → module → địa lý → xu hướng. V04 cần bảng aggregate tuần từ T09; không lấy cột `*_all_time` để giả xu hướng.

## 5. Trang 2 — Factor Analysis

**Mục tiêu:** mô tả phân bố và các mối liên hệ quan sát giữa assessment, VLE và background.

```text
┌──────────────────────────────────────────────────────────────────────────┐
│ FACTOR ANALYSIS     Module ▾  Presentation ▾  Region ▾  IMD ▾ [Reset]  │
├────────────────────────────────────┬─────────────────────────────────────┤
│ V05 Scatter: clicks × avg score    │ V06 Boxplot: score by result       │
├────────────────────────────────────┼─────────────────────────────────────┤
│ V07 Heatmap: IMD × engagement      │ V08 100% stacked bar: result mix   │
├────────────────────────────────────┴─────────────────────────────────────┤
│ V09 Treemap: learning attempts by module, color = At-Risk Rate          │
└──────────────────────────────────────────────────────────────────────────┘
```

- Scatter dùng bubble size = số lượt học sau khi bin/aggregate; tránh vẽ chồng 32.593 điểm nếu khó đọc.
- Boxplot là custom visual có điều kiện. Nếu môi trường không cho custom visual, thay bằng histogram + bảng percentile/median; không tuyên bố boxplot đã có trước khi thử.
- Heatmap dùng Matrix + conditional formatting, có `N` trong tooltip/cell và nhóm missing riêng.

## 6. Trang 3 — Risk Analysis

**Mục tiêu:** tìm phân khúc có tỷ lệ rủi ro cao và cho phép drill-down mà không khẳng định nguyên nhân.

```text
┌──────────────────────────────────────────────────────────────────────────┐
│ RISK ANALYSIS       Module ▾  Presentation ▾  Region ▾      [Reset]    │
├────────────────────────────────────┬─────────────────────────────────────┤
│ V10 Decomposition tree             │ V11 Histogram: VLE clicks by risk  │
│ At-Risk Rate → module → region     │                                     │
│ → education → previous attempts    │                                     │
├────────────────────────────────────┴─────────────────────────────────────┤
│ V12 Ribbon: ranking At-Risk Rate của module qua presentation            │
├──────────────────────────────────────────────────────────────────────────┤
│ Bảng nhóm cần chú ý: N, At-Risk Count/Rate, score, clicks, missing       │
└──────────────────────────────────────────────────────────────────────────┘
```

Decomposition tree và bảng luôn hiển thị `N` để tránh nhấn mạnh nhóm quá nhỏ. Engagement band chỉ thêm sau khi D05 được chốt ở T11/T13.

## 7. Trang 4 — Prediction

**Mục tiêu:** trình bày chất lượng Logistic Regression và danh sách dự báo có thể hành động.

```text
┌──────────────────────────────────────────────────────────────────────────┐
│ PREDICTION       Model version · Cutoff day · Threshold      [Reset]    │
├──────────────┬──────────────┬──────────────┬─────────────────────────────┤
│ Recall risk  │ Precision    │ F1           │ ROC-AUC / PR-AUC            │
├────────────────────────────────────┬─────────────────────────────────────┤
│ V13 Histogram: risk probability    │ V14 Confusion-matrix heatmap       │
├────────────────────────────────────┼─────────────────────────────────────┤
│ V15 Clustered bar: actual/predicted│ V16 ROC or PR line                 │
├────────────────────────────────────┴─────────────────────────────────────┤
│ High-risk table: attempt key, probability, predicted/actual, context     │
└──────────────────────────────────────────────────────────────────────────┘
```

Trang này giữ placeholder cho tới T13. Không tạo xác suất hoặc metric giả. Bảng dùng đủ khóa lượt học, không chỉ `id_student`; cột actual chỉ phục vụ đánh giá hồi cứu.

## 8. Inventory visual

KPI card và bảng chi tiết không được tính vào yêu cầu đa dạng. Thiết kế có **16 visual instances, 12 loại chart**, gồm **11 loại không tính map**.

| Loại | Visual ID | Trang | Field/measure chính | Lý do và tính khả thi |
|---:|---|---|---|---|
| 1. Donut | V01 | Overview | `final_result`, attempts | Cơ cấu bốn lớp; native. |
| 2. Clustered column | V02, V15 | Overview, Prediction | module/result; actual/predicted | So sánh nhóm; native. |
| 3. Azure Maps filled map | V03 | Overview | `region`, At-Risk Rate | Đúng dữ liệu địa lý; phải thử geocoding 13/13 ở T09. |
| 4. Line | V04, V16 | Overview, Prediction | tuần/clicks; FPR/TPR hoặc recall/precision | Xu hướng có thứ tự; native; cần bảng tuần/model output. |
| 5. Scatter/bubble | V05 | Factor | VLE clicks, avg score, N | Quan hệ hai biến số; native. |
| 6. Box-and-whisker | V06 | Factor | score, result | Phân bố/outlier; custom có điều kiện, có fallback. |
| 7. Matrix heatmap | V07, V14 | Factor, Prediction | IMD × engagement; actual × predicted | Matrix + conditional formatting; native. |
| 8. 100% stacked bar | V08 | Factor | nhóm background, result share | So sánh tỷ trọng; native. |
| 9. Treemap | V09 | Factor | module, attempts, At-Risk Rate | Quy mô và phân cấp; native. |
| 10. Decomposition tree | V10 | Risk | At-Risk Rate và các dimension | Drill khám phá phân khúc; native. |
| 11. Histogram | V11, V13 | Risk, Prediction | binned clicks/probability | Phân bố; hiện thực bằng numeric bins + column visual. |
| 12. Ribbon | V12 | Risk | presentation, module ranking | Thay đổi thứ hạng qua presentation; native. |

## 9. Thiết kế tương tác

| Cơ chế | Thiết kế phải hiện thực | Kiểm tra ở T12/T14 |
|---|---|---|
| Multi-level filter | Ba slicer đồng bộ module → presentation → region; IMD/page-specific khi cần | Chọn từng cấp, tổ hợp không dữ liệu, reset và đồng bộ bốn trang. |
| Drill-down | V02: module → presentation; V12: presentation → module; map: region → module qua drill hierarchy nếu hoạt động đúng | Drill xuống/lên, tiêu đề động và mẫu số không đổi sai. |
| Tooltip | Tooltip page chung: tên nhóm, attempts, distinct learners, At-Risk Count/Rate, numerator/denominator, missing | Hover trên V02/V03/V05/V07/V10; đối chiếu số với Python. |
| Cross-filter | Chọn module ở V02 lọc V01/V03/V04; chọn region ở V03 lọc các visual trang; chọn ô V07 lọc V05/V08/V09 | Kiểm tra hướng tương tác bằng **Edit interactions** và khả năng bỏ chọn. |
| Prediction interaction | Chọn probability bin V13 lọc confusion matrix và high-risk table | Không để actual label ảnh hưởng risk probability/model score. |

## 10. Phương án map và quyết định D06

- Chọn **At-Risk Rate by Region** vì đúng câu hỏi rủi ro và có numerator/denominator rõ; không dùng Average Performance làm measure chính.
- Dùng Azure Maps filled map; `region` đặt đúng Data Category, thêm country/region context khi cần để giảm nhập nhằng.
- T09 phải kiểm tra đủ 13 nhãn OULAD. Đặc biệt `Ireland` không được tự đổi thành địa danh khác nếu chưa có nguồn xác nhận.
- Nếu geocoding sai, tạo bảng lookup có nguồn và khóa mapping; không tự chế tọa độ. Chỉ đánh dấu rubric map sau ảnh/test geographic visual thực tế.

## 11. Bằng chứng hoàn thành T04

- [x] Bốn trang có mục tiêu, bố cục, KPI, visual và luồng đọc.
- [x] Inventory 16 visual instances / 12 loại, không tính KPI card; có map riêng.
- [x] Mỗi visual có field, mục đích, phụ thuộc và phương án kỹ thuật.
- [x] Có thiết kế slicer nhiều cấp, drill-down, tooltip và cross-filter cụ thể.
- [x] KPI phân biệt lượt học/sinh viên, assessment/final score và engagement/attendance.
- [x] D06–D08 được chốt ở mức thiết kế; map, `.pbix` và tương tác thực tế chuyển sang T09/T12/T14.

## 12. Tài liệu kỹ thuật Power BI

- [Microsoft — Visualization types in Power BI](https://learn.microsoft.com/en-us/power-bi/visuals/power-bi-visualization-types-for-reports-and-q-and-a)
- [Microsoft — Conditional formatting for tables and matrices](https://learn.microsoft.com/en-us/power-bi/desktop-conditional-table-formatting)
- [Microsoft — Filters and highlighting](https://learn.microsoft.com/en-us/power-bi/create-reports/power-bi-reports-filters-and-highlighting)
- [Microsoft — Azure Maps geocoding](https://learn.microsoft.com/en-gb/azure/azure-maps/power-bi-visual-geocode)
- [Microsoft — Azure Maps filled map](https://learn.microsoft.com/en-us/azure/azure-maps/power-bi-visual-filled-map)

