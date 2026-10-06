# Python pipeline

## T05–T07 — OULAD data pipeline

`oulad_pipeline.py` đọc 7 CSV cục bộ và không sửa raw. Nó cần Python 3.14, pandas 2.3.3 và NumPy 2.3.5 tại thời điểm T05–T07 được chạy.

```powershell
python src/oulad_pipeline.py audit data/raw
python src/oulad_pipeline.py clean data/raw
python src/oulad_pipeline.py build data/raw
python src/oulad_pipeline.py report
```

- `audit` ghi metrics cục bộ vào `data/interim/t05_audit_metrics.json`.
- `clean` tạo 7 CSV interim ở `data/interim/`; raw luôn bất biến.
- `build` aggregate hai event table trước khi left join về hạt `(code_module, code_presentation, id_student)`, rồi tạo `data/processed/clean_dataset.csv` dùng chung theo D16.
- `report` tạo hiện vật tracked [Data Quality Report](../reports/data-quality-report.md).

Các aggregate có hậu tố `*_all_time` là dữ liệu mô tả dùng cho EDA/dashboard và không được dùng làm feature dự báo sớm. `studentVle` được gom theo learner–resource–day bằng phép cộng `sum_click`, không xóa exact duplicate riêng lẻ. `final_result`, `At_Risk` và `date_unregistration` không phải feature model.

## T13 — Logistic Regression dự báo At-Risk

Toàn bộ mục tiêu, quy trình, lệnh chạy, feature contract, thí nghiệm, metric, artifact và cách bàn giao sang Tableau được quản lý tại một nguồn duy nhất: [kế hoạch phân tích và model](../docs/04-analysis-model-plan.md#model-logistic-regression--tài-liệu-chuẩn-duy-nhất).
