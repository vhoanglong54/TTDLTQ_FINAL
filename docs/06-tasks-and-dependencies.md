# 06 — Task board và quan hệ phụ thuộc

TV1 là **Khang**, TV2 là **Nadi**, TV3 là **leader**. Các task liên tiếp, cùng owner và cùng đầu ra được gom vào một Issue. Cột phụ thuộc cho biết điều kiện để bắt đầu/chốt; phần chuẩn bị có thể làm song song. Nhóm tự quản lý lịch và deadline.

## Bảng giao việc thực tế

| Thứ tự | Issue | Owner | Nhánh commit | Chỉ bắt đầu/chốt khi |
|---:|---|---|---|---|
| 1 | [#1 — T01: nguồn OULAD](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/1) | Khang | `data/T01-oulad-source` | Bắt đầu ngay |
| 2 | [#2 — T02: data dictionary](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/2) | Khang | `data/T02-data-dictionary` | Chốt sau #1 |
| 2 | [#3 — T03: RQ/hypothesis](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/3) | Nadi | `analysis/T03-research-questions` | Soạn ngay, chốt sau #1; dùng #2 |
| 2 | [#4 — T04: wireframe](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/4) | Leader | `bi-model/T04-dashboard-wireframe` | Phác ngay, chốt sau #1; dùng #2–#3 |
| 3 | [#5 — T05–T07: data pipeline](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/5) | Khang | `data/T05-T07-data-pipeline` | #1–#2 đã nghiệm thu |
| 4 | [#6 — T08, T10–T11: EDA/insight](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/6) | Nadi | `analysis/T08-T11-eda-insights` | #3 và #5 đã nghiệm thu |
| 4 | [#7 — T09, T12: dashboard v0](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/7) | Leader | `bi-model/T09-T12-dashboard-prototype` | Bắt đầu sau #4–#5; chốt sau #6 |
| 5 | [#8 — T13: Logistic Regression](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/8) | Leader | `bi-model/T13-logistic-regression` | #5–#6 đã nghiệm thu |
| 6 | [#9 — T14–T16, T21: dashboard QA](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/9) | Cả nhóm; leader chủ trì | `bi-model/T14-T16-T21-dashboard-qa` | #7–#8 đã nghiệm thu |
| 7 | [#10 — T17–T20, T22: báo cáo/demo](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/10) | Cả nhóm | `docs/T17-T22-report-demo` | Viết phần riêng khi đầu ra có; ghép/chốt sau #5–#6, #8–#9 |

Áp dụng duy nhất: **một Issue → một nhánh cụm task → một PR dùng `Closes #<issue>` kèm bằng chứng → leader quyết định merge → Issue tự đóng ngay**. Nhánh được tạo từ `main` mới nhất khi bắt đầu. Không gắn reviewer, mention nhắc duyệt hoặc lặp biên bản ở Issue.

## Nhóm nền tảng

| ID | Owner / phối hợp khi cần | Task và hiện vật nghiệm thu | Phụ thuộc |
|---|---|---|---|
| T01 | TV1 / TV2, TV3 | Tải OULAD từ nguồn chính thức; ghi link, license, ngày tải, checksum, số dòng từng bảng, xác nhận ≥5.000 dòng và ≥3 bảng trong `data/README.md`/Data Quality Report | — |
| T02 | TV1 / TV2 | Data dictionary cho 7 bảng: cột, kiểu, khóa, hạt, missing, ý nghĩa; sơ đồ quan hệ và danh sách biến dùng | T01 |
| T03 | TV2 / TV1 | Chốt RQ1–RQ6 theo OULAD, kiểm tra 8–10 hypothesis; lập mẫu Insight Log và sơ bộ related work | T01 |
| T04 | TV3 / TV2 | Khung nội dung dashboard và khả thi map/filter/drill-down. Sau quyết định Tableau, inventory visual phải chốt lại ở T12 sau EDA; chưa khóa loại biểu đồ tại T04. | T01 |

## Nhóm dữ liệu sạch

| ID | Owner / phối hợp khi cần | Task và hiện vật nghiệm thu | Phụ thuộc |
|---|---|---|---|
| T05 | TV1 / TV2 | `01_data_audit.ipynb`: shape, type, missing, duplicates, unique keys, outliers, invalid/categories; báo cáo chất lượng | T01–T02 |
| T06 | TV1 / TV2 | `02_cleaning.ipynb` + script: quy tắc missing/outlier/normalize/datatype; bảng sạch tái tạo được | T05 |
| T07 | TV1 / TV2 | Join 7 bảng hoặc ít nhất 3 bảng đủ rubric; kiểm cardinality/unmatched; tổng hợp VLE/assessment đúng hạt; calculated fields và `clean_dataset.csv` cục bộ | T06 |
| T08 | TV2 / TV1 | EDA cơ bản và 3–5 biểu đồ tĩnh đầu tiên; điều chỉnh định nghĩa nhóm/giả thuyết theo phân bố thực | T06–T07 |
| T09 | TV3 / TV1 | Khởi tạo Tableau với bảng sạch; ghi relationship/calculated fields, đối chiếu KPI bằng Python và thử mapping `region`; chưa chốt inventory visual | T07 |

**Cổng dữ liệu:** T01, T02, T05–T07 được leader nghiệm thu và bảng cho EDA/dashboard/model có schema ổn định. Nếu thiếu dữ liệu map hoặc khóa, ghi quyết định và tác động ngay.

## Nhóm phân tích sâu

| ID | Owner / phối hợp khi cần | Task và hiện vật nghiệm thu | Phụ thuộc |
|---|---|---|---|
| T10 | TV2 / TV1 | `03_eda.ipynb`: phân bố, temporal VLE, assessment, IMD/region, tương tác; kiểm nhóm nhỏ và khoảng thời gian | T07–T08 |
| T11 | TV2 / TV3 | Insight Log 5–7 insight chính, mỗi insight có bằng chứng, mẫu số và giới hạn; risk profile và storyline | T10 |
| T12 | TV3 / TV2 | Dựa trên EDA/Insight Log để chốt visual/layout, dựng dashboard Tableau v0 và kiểm tra cross-filter/prototype drill-down | T09–T11 |

**Cổng insight:** T10–T11 có đủ bằng chứng và được leader nghiệm thu; khi định nghĩa insight thay đổi, cập nhật dashboard/báo cáo tương ứng.

## Nhóm dự báo và dashboard

| ID | Owner / phối hợp khi cần | Task và hiện vật nghiệm thu | Phụ thuộc |
|---|---|---|---|
| T13 | TV3 / TV1, TV2 | Chốt mốc dự báo, train/test split, encoding; Logistic Regression; metric, leakage check, risk probability; xuất `actual_status`/`predicted_status` theo khóa lượt học | T07, T11 |
| T14 | TV3 / TV2, TV1 | Hoàn thiện 4 trang, ≥8 loại chart, map, multi-level filters, drill-down, tooltip, cross-filter và trang Prediction | T12–T13 |
| T15 | TV1 / TV3 | QA số liệu giao diện so với hàm Python: counts, ratios, joins, risk output, region; lưu bảng đối chiếu | T14 |
| T16 | TV2 / TV3 | QA insight, câu chuyện, tên chart/tooltip/nhóm; tránh nói nhân quả hoặc gọi proxy là đo trực tiếp | T14 |

## Nhóm báo cáo và bảo vệ

| ID | Owner / phối hợp khi cần | Task và hiện vật nghiệm thu | Phụ thuộc |
|---|---|---|---|
| T17 | TV1 / TV2 | Viết Dataset, Data Dictionary, Preprocessing, phần data pipeline và bảng chất lượng | T07 |
| T18 | TV2 / TV1 | Viết Introduction, Related Work, EDA, Insight/storytelling; trích dẫn IEEE | T11 |
| T19 | TV3 / TV2 | Viết Dashboard, Regression, Prediction, cách cài đặt/sử dụng | T13–T14 |
| T20 | Cả 3 / leader nghiệm thu | Ghép báo cáo ≥40 trang: sơ đồ hệ thống, logic chart, code/pseudocode, kết luận, tài liệu tham khảo, link video | T17–T19 |
| T21 | TV3 / TV1, TV2 | QA dashboard cuối; ảnh minh chứng 8 chart/map/tương tác, chạy demo thực tế | T14–T16 |
| T22 | Cả 3 / leader nghiệm thu | Slide, kịch bản Data Analyst, video backup tóm tắt và link, diễn tập vấn đáp cả 3 vai trò | T20–T21 |

**Cổng dashboard:** T21 qua QA và các thay đổi tiếp theo ghi rõ tác động lên demo/báo cáo. **Cổng bàn giao:** T20–T22 đã có đường dẫn hiện vật, tất cả mục rubric đã được đối chiếu.

## Phân việc song song theo DOCX

| Giai đoạn | TV1 | TV2 | TV3 |
|---|---|---|---|
| Dataset | Kiểm tra dữ liệu | Kiểm tra biến phân tích | Kiểm tra map/dashboard |
| Audit | Làm chính | Hypothesis | Wireframe |
| Cleaning | Làm chính | Related work | Khởi tạo Tableau/data source |
| Feature engineering | Làm chính | Định nghĩa nhóm | Chuẩn bị model |
| EDA/deep analysis | Hỗ trợ | Làm chính | Dashboard prototype/interaction |
| Model | Chuẩn bị dữ liệu | Diễn giải | Làm chính |
| Dashboard | QA số liệu | QA insight | Làm chính |
| Report/defense | Phần Data / cả nhóm | Phần Analysis / cả nhóm | Phần Dashboard, Model / cả nhóm |
