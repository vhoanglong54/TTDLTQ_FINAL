# 05 — Đặc tả dashboard Tableau

Tableau là công cụ dashboard duy nhất theo D17. Python xử lý dữ liệu, EDA, Logistic Regression và Random Forest theo D18; Tableau kết nối bảng sạch/output model đã kiểm tra để trực quan hóa và tương tác.

Tài liệu này chỉ chốt kiến trúc và tiêu chí. **Loại biểu đồ, inventory visual, layout cuối và theme chưa được chốt**; chúng phải dựa trên EDA và Insight Log của Issue #6.

## Kiến trúc trách nhiệm

| Lớp | Trách nhiệm |
|---|---|
| Data pipeline | Python tái tạo `clean_dataset.csv`, kiểm tra schema, khóa và hạt một lượt học |
| Baseline KPI | Python tính số chuẩn để TV1 đối chiếu với Tableau |
| Mô hình | Logistic Regression bắt buộc + Random Forest đối chứng; Python xuất xác suất, nhãn, split, threshold và version của từng mô hình |
| Dashboard | Tableau quản lý data source/relationship, calculated fields, worksheet/dashboard/story và tương tác |
| Bằng chứng | Workbook/link, ảnh/video, checklist QA, version công cụ và checksum input |

Không join raw event table trong Tableau và không train model lại khi người dùng đổi filter. Tableau chỉ trình bày output model đã được kiểm tra.

## Luồng nội dung dự kiến

| Phần | Câu hỏi |
|---|---|
| Overview | Quy mô dữ liệu và kết quả học tập tổng quan ra sao? |
| Factor Analysis | Những yếu tố và tương tác nào có liên hệ với kết quả? |
| Risk Analysis | Những nhóm nào có tỷ lệ At-Risk đáng chú ý? |
| Prediction | Hai mô hình nhận diện At-Risk tốt đến đâu, sai ở đâu và mô hình nào phù hợp mục tiêu hơn? |

Đây là kiến trúc thông tin, không khóa số dashboard/sheet hoặc loại chart cuối.

## Ràng buộc từ rubric

Phương án visual sau EDA phải đáp ứng nguyên vẹn:

- Số loại biểu đồ tối thiểu theo rubric, lựa chọn phù hợp kiểu dữ liệu.
- Ít nhất một geographic map thực sự.
- Filter nhiều cấp, drill-down, tooltip và cross-filtering.
- Storytelling/insight có số liệu, mẫu số, cỡ mẫu và giới hạn diễn giải.
- Tích hợp kết quả của Logistic Regression và Random Forest; không làm mất bằng chứng Logistic Regression bắt buộc theo rubric.

`region` là nhãn OULAD, chưa phải geometry. T09 phải kiểm tra geographic role hoặc spatial file/mapping có nguồn và coverage đủ 13/13 nhãn; không tự chế tọa độ hoặc polygon.

## Tích hợp kết quả hai mô hình

Python xuất bảng dự báo dạng long theo khóa `(code_module, code_presentation, id_student, model_name)` với tối thiểu:

- `actual_status`
- `predicted_status`
- `risk_probability`
- `dataset_split`
- `prediction_threshold`
- `model_version`
- `model_name`

Tableau dùng relationship ở logical layer theo ba khóa lượt học và giữ `model_name` làm dimension/filter. Không physical join bảng long vào bảng lượt học rồi cộng KPI, vì mỗi lượt học có hai dòng dự báo và sẽ bị nhân đôi. File mô hình `.joblib`/`.pkl` không phải data source Tableau.

## Cổng chốt visual

Chỉ chốt inventory/layout sau khi:

1. Issue #6 có EDA và 5–7 insight được nghiệm thu.
2. T09 xác nhận data source, KPI và map khả thi trong Tableau.
3. T13 xác nhận output/metric/threshold cho phần Prediction.
4. Leader ghi quyết định mới vào `docs/08-decisions-and-open-questions.md`.

## QA

- Đối chiếu attempts, distinct learners, kết quả, pass/at-risk rate và output model giữa Python với Tableau trên cùng filter context.
- Kiểm tra relationship không nhân dòng và phân biệt lượt học với distinct learner.
- Kiểm tra filter, drill-down, tooltip, cross-filter, reset và trạng thái không có dữ liệu.
- Lưu workbook/link, ảnh/video, version Tableau, checksum input và bảng QA trong `dashboard/`/báo cáo.

Không đánh dấu mục rubric hoàn thành chỉ vì đã chọn Tableau hoặc tạo workbook rỗng.
