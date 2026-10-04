# 10 — Bàn giao dữ liệu T05–T07 cho EDA, Power BI và model

**Nguồn bàn giao:** T05–T07 / [Issue #5](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/5).  
**Hạt chuẩn:** một lượt học `(code_module, code_presentation, id_student)`.  
**Trạng thái:** pipeline đã merge vào `main`; các kết luận EDA, dashboard và model chưa được nghiệm thu trong tài liệu này.

## Cách mỗi máy tái tạo dữ liệu

Từng thành viên phải có bảy CSV raw tại `data/raw/`, sau đó chạy từ root repo:

```powershell
python src/oulad_pipeline.py audit data/raw
python src/oulad_pipeline.py clean data/raw
python src/oulad_pipeline.py build data/raw
python src/oulad_pipeline.py report
```

`data/processed/clean_dataset.csv` chỉ nằm local và không commit. Kiểm tra nhanh trước khi dùng: 32.593 dòng, 35 cột, 0 duplicate attempt key. Chi tiết audit/cleaning/join ở [Data Quality Report](../reports/data-quality-report.md); schema đầy đủ ở [processed contract](../data/processed/README.md).

## Bàn giao cho TV2 — Issue #6, T08/T10/T11

### TV2 cần dùng file và hiện vật nào

| Việc | Đường dẫn / thao tác | Điều phải giữ đúng |
|---|---|---|
| Tạo data local | Chạy bốn lệnh trên | Không copy/commit CSV raw, interim hoặc processed. |
| Đọc data contract | `data/processed/README.md`, `docs/09-data-dictionary.md` | Đơn vị mặc định là **lượt học**, không phải `id_student` unique. |
| Làm EDA | Tạo/cập nhật `notebooks/03_eda.ipynb` trên branch `analysis/T08-T11-eda-insights` | Ghi filter, mẫu số, số dòng và version input cho mỗi biểu đồ. |
| Ghi insight | Cập nhật `docs/insight-log.md` theo Issue #6 | Mỗi insight cần số liệu, nhóm so sánh, giới hạn và chỉ nói liên hệ quan sát. |

### Biến TV2 có thể dùng ngay

- Kết quả mô tả: `final_result`, `At_Risk`, `Performance_Level`, `code_module`, `code_presentation`, `region`.
- Background: `gender`, `highest_education`, `imd_band`, `age_band`, `num_of_prev_attempts`, `studied_credits`, `disability`.
- Assessment mô tả: `assessment_event_count`, `assessment_scored_count`, `assessment_score_sum_all_time`, `assessment_score_mean_all_time`, `assessment_banked_count`, `assessment_late_submission_count_all_time`.
- VLE mô tả: `vle_event_count`, `vle_total_clicks_all_time`, `vle_active_days_all_time`, `vle_resource_count_all_time`, `vle_activity_type_count_all_time`, `vle_first_event_day`, `vle_last_event_day`.

### TV2 phải tránh và cần báo lại TV1/leader

- `imd_band` có 1.111 missing; ghi rõ loại/giữ missing trong mẫu số, không xem đây là thu nhập cá nhân.
- `assessment_score_mean_all_time` chỉ là mean theo từng lượt học; khi cần mean chung phải dùng tổng score/chia tổng scored count.
- VLE click là proxy tương tác, không phải attendance, study hours hay nguyên nhân kết quả.
- `*_all_time` được dùng EDA mô tả, nhưng không được gọi là engagement **sớm** hay dùng để suy luận dự báo trước D04.
- Không tạo sleep/lifestyle/previous-grade giả. Khi phát hiện thiếu schema, denominator hoặc cần đổi định nghĩa nhóm, ghi vào `docs/08-decisions-and-open-questions.md` và báo TV1 trước khi chốt insight.

## Bàn giao cho TV3 — Issue #7, T09/T12 Power BI

### T09: dựng skeleton dữ liệu và KPI

| Mục TV3 làm | Input/hướng dẫn | Điều TV3 cần điều chỉnh hoặc kiểm tra |
|---|---|---|
| Import | Import **một** `data/processed/clean_dataset.csv` local cho dashboard v0. | Không join hai raw event table trong Power BI; aggregate đã làm ở T07. |
| Grain | Một row = một lượt học. | KPI Total Learning Attempts = count rows; Distinct Learners = distinct count `id_student`; không đổi tên/gộp hai KPI. |
| Risk KPI | `At_Risk` là nhãn lịch sử. | At-Risk Count = sum `At_Risk`; At-Risk Rate = sum `At_Risk` / count rows trong cùng filter context. |
| Result KPI | `final_result`/`Performance_Level`. | Pass Rate phải định nghĩa rõ, khuyến nghị `(Pass + Distinction) / total learning attempts`; không gọi là điểm trung bình. |
| Assessment KPI | `assessment_score_sum_all_time`, `assessment_scored_count`. | Average Assessment Score = `SUM(score_sum) / SUM(scored_count)` nếu mẫu số > 0; **không** average `assessment_score_mean_all_time`. Nhãn phải là assessment score, không phải final score. |
| Engagement KPI | VLE aggregate `*_all_time`. | Gọi là VLE engagement/click proxy; nêu mẫu số và không gọi attendance/study hours. |
| Map | `region`. | Thử geocoding thật cho 13 region; nếu Power BI không nhận đúng UK regions, tạo bảng mapping có nguồn/bằng chứng, không tự gán tọa độ. |

### T12: dashboard v0 sau insight #6

- Giữ filter context nhất quán: `code_module`, `code_presentation`, `region`; khi thêm `id_student`, ghi rõ đây là distinct learner hay attempt row.
- Dùng insight được TV2 chốt trong #6; không lấy con số trực tiếp từ biểu đồ mà không đối chiếu Python trên cùng filter.
- Lưu `.pbix`/link theo `dashboard/README.md`, ảnh prototype và bảng KPI/measure. Không đưa raw CSV vào Git.
- Mọi chênh lệch KPI, lỗi map hoặc filter phải ghi để TV1 đối chiếu ở T15, không tự chỉnh mẫu số im lặng.

## Chuẩn bị TV3 cho Issue #8 — T13 Logistic Regression

T13 chỉ bắt đầu/chốt sau #6. Trước khi tạo model, leader/TV3 cần ghi quyết định D04/D05:

1. Mốc dự báo là ngày nào tính từ đầu presentation và event đúng ngày mốc có được tính không.
2. Danh sách feature được phép, gồm rule availability theo module/presentation.
3. Split theo `id_student` hay presentation, và baseline/metric/threshold.

Khi D04 được chốt, TV1 sẽ kiểm tra/bổ sung aggregate theo cutoff. Không dùng trực tiếp các cột sau làm feature model: `final_result`, `At_Risk`, `Performance_Level`, `date_unregistration`, mọi `*_all_time` chưa giới hạn cutoff, hoặc assessment/VLE sau cutoff. `At_Risk` chỉ là target.

## Điểm TV3 cần phản hồi cho TV1

- Kết quả thử map `region`: nhận được/không nhận được, cách mapping và nguồn.
- Danh sách KPI/DAX cùng định nghĩa mẫu số; nhất là Pass Rate, At-Risk Rate và Average Assessment Score.
- Quyết định D04/D05 để TV1 tạo/kiểm tra feature đúng thời điểm.
- Mọi cột dashboard/model cần thêm, cùng mục đích EDA/BI/model và mốc thời gian. TV1 không thêm cột giả hoặc cột có leakage.

## Ranh giới trách nhiệm

TV1 duy trì pipeline, kiểm tra grain/schema/KPI và hỗ trợ leakage. TV2 sở hữu EDA/insight/storyline; TV3 sở hữu Power BI, model, map test và quyết định model. Leader nghiệm thu các Issue/PR; tài liệu bàn giao không phải bằng chứng TV2/TV3 đã hoàn thành task tiếp theo.
