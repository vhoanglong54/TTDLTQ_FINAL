# Hợp đồng bàn giao bảng processed — T07

`clean_dataset.csv` được Git theo dõi để cả nhóm dùng chung theo quyết định D16. File vẫn phải tái tạo được từ raw bằng các lệnh:

```powershell
python src/oulad_pipeline.py audit data/raw
python src/oulad_pipeline.py clean data/raw
python src/oulad_pipeline.py build data/raw
```

## Hạt, kích thước và khóa

- Hạt: một lượt học `(code_module, code_presentation, id_student)`.
- Kích thước lần chạy 04/10/2026: 32.593 dòng, 35 cột, 0 duplicate attempt key; `clean_dataset.csv` SHA-256 `4b3250f59456d9eb546291c52bd7e3ff5b4afe0a942b54df961cd193a8137701`.
- Base là `studentInfo`; `studentRegistration` và `courses` left join one-to-one/many-to-one. `studentAssessment` và `studentVle` được aggregate trước left join nên không nhân dòng.
- `At_Risk`: `Fail`/`Withdrawn` = 1, `Pass`/`Distinction` = 0. Đây là nhãn, không phải feature model.

## Schema

| Nhóm | Cột |
|---|---|
| Khóa và raw student | `code_module`, `code_presentation`, `id_student`, `gender`, `region`, `highest_education`, `imd_band`, `age_band`, `num_of_prev_attempts`, `studied_credits`, `disability`, `final_result` |
| Registration/course | `date_registration`, `date_unregistration`, `has_registration_record`, `module_presentation_length` |
| Assessment aggregate | `assessment_event_count`, `assessment_scored_count`, `assessment_score_missing_count`, `assessment_score_sum_all_time`, `assessment_score_mean_all_time`, `assessment_score_min_all_time`, `assessment_score_max_all_time`, `assessment_banked_count`, `assessment_late_submission_count_all_time`, `assessment_type_nunique` |
| VLE aggregate | `vle_event_count`, `vle_total_clicks_all_time`, `vle_active_days_all_time`, `vle_resource_count_all_time`, `vle_activity_type_count_all_time`, `vle_first_event_day`, `vle_last_event_day` |
| Calculated result | `At_Risk`, `Performance_Level` |

## Handoff guardrails

- Tỷ lệ/KPI dùng mẫu số **lượt học**, không suy ra số sinh viên unique nếu chưa deduplicate theo `id_student` theo định nghĩa riêng.
- `*_all_time` chỉ là aggregate mô tả cho EDA/BI. D04 chưa chốt nên không dùng chúng làm feature dự báo sớm.
- Với Average Assessment Score theo filter, dùng `SUM(assessment_score_sum_all_time) / SUM(assessment_scored_count)` khi mẫu số lớn hơn 0; không dùng trung bình trực tiếp của `assessment_score_mean_all_time` vì sẽ sai trọng số.
- Tuyệt đối loại khỏi model feature: `final_result`, `At_Risk`, `date_unregistration` và bất cứ assessment/VLE nào sau cutoff được leader chốt.
- `imd_band` missing vẫn nullable; không tự diễn giải là thu nhập cá nhân. VLE click là proxy tương tác, không phải attendance/study hours.
- Chi tiết số dòng, missing, duplicate, join và test: [Data Quality Report](../../reports/data-quality-report.md).
