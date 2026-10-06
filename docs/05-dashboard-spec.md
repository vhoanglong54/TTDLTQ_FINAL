# 05 — Đặc tả dashboard Tableau

Tableau là công cụ dashboard duy nhất theo D17. Python tiếp tục xử lý dữ liệu, EDA và Logistic Regression; Tableau kết nối bảng sạch/output model đã kiểm tra để trực quan hóa và tương tác.

Tài liệu này chỉ chốt kiến trúc và tiêu chí. **Loại biểu đồ, inventory visual, layout cuối và theme chưa được chốt**; chúng phải dựa trên EDA và Insight Log của Issue #6.

## Kiến trúc trách nhiệm

| Lớp | Trách nhiệm |
|---|---|
| Data pipeline | Python tái tạo `clean_dataset.csv`, kiểm tra schema, khóa và hạt một lượt học |
| Baseline KPI | Python tính số chuẩn để TV1 đối chiếu với Tableau |
| Mô hình | scikit-learn Logistic Regression; Python xuất xác suất, nhãn dự báo, split, threshold và model version |
| Dashboard | Tableau quản lý data source/relationship, calculated fields, worksheet/dashboard/story và tương tác |
| Bằng chứng | Workbook/link, ảnh/video, checklist QA, version công cụ và checksum input |

Không join raw event table trong Tableau và không train model lại khi người dùng đổi filter. Tableau chỉ trình bày output model đã được kiểm tra.

## Luồng nội dung dự kiến

| Phần | Câu hỏi |
|---|---|
| Overview | Quy mô dữ liệu và kết quả học tập tổng quan ra sao? |
| Factor Analysis | Những yếu tố và tương tác nào có liên hệ với kết quả? |
| Risk Analysis | Những nhóm nào có tỷ lệ At-Risk đáng chú ý? |
| Prediction | Logistic Regression nhận diện At-Risk tốt đến đâu và sai ở đâu? |

Đây là kiến trúc thông tin, không khóa số dashboard/sheet hoặc loại chart cuối.

## Câu hỏi nghiên cứu theo dữ liệu OULAD

| Câu hỏi trong đề cương | Cách kiểm tra trên OULAD | Điều chỉnh cần lưu ý |
|---|---|---|
| Phân bố kết quả? | So sánh `final_result` theo module, presentation và region. | Bổ sung region để phục vụ geographic map. |
| Attendance/Study Hours? | Dùng VLE clicks, ngày hoạt động và loại tài nguyên làm proxy tương tác sớm. | OULAD không có attendance hoặc giờ học; không được gọi click là thời gian học. |
| Previous Performance? | Dùng `num_of_prev_attempts` và điểm assessment sớm. | OULAD không có điểm học phần trước. |
| Lifestyle/socioeconomic? | So sánh theo `imd_band`, age, region và disability. | Bỏ lifestyle vì không có sleep/stress; `imd_band` chỉ mô tả khu vực. |
| Tổ hợp rủi ro? | Kết hợp VLE, assessment sớm, previous attempts và IMD. | Chỉ kết luận liên hệ quan sát, không khẳng định nhân quả. |
| Logistic Regression? | Đánh giá khả năng nhận diện `At_Risk` trước khi khóa học kết thúc. | Quy trình và metric nằm tại [tài liệu model](04-model.md). |

## Giả thuyết EDA và storytelling

Các giả thuyết dưới đây là điều cần kiểm tra, chưa phải insight đã được chứng minh. Mọi kết luận phải kèm mẫu số/cỡ nhóm, missing và giới hạn diễn giải.

| ID | Giả thuyết cần kiểm tra | Biến và visual phù hợp | Giới hạn chính |
|---|---|---|---|
| H01 | Kết quả khác nhau giữa module và presentation. | `final_result`, `At_Risk`; stacked bar theo module–presentation. | Khác biệt có thể do độ khó/cấu trúc đánh giá. |
| H02 | Tương tác VLE sớm thấp liên hệ với At-Risk cao hơn. | Click sớm; boxplot và bar tỷ lệ At-Risk. | Click chỉ là proxy tương tác. |
| H03 | Tương tác giảm theo tuần liên hệ với nguy cơ cao hơn. | Click theo tuần và slope; line chart theo nhóm. | Deadline assessment có thể tạo đỉnh tương tác. |
| H04 | Nhiều lần học lại liên hệ với tỷ lệ At-Risk cao hơn. | `num_of_prev_attempts`; bar chart. | Không cho biết kết quả cụ thể của lần học trước. |
| H05 | Điểm assessment sớm thấp liên hệ với nguy cơ cao hơn. | Điểm/nộp bài trước cutoff; histogram hoặc boxplot. | Phải nêu nhóm chưa có bài được chấm. |
| H06 | Tương tác thấp kết hợp điểm sớm thấp tạo nhóm rủi ro cao hơn từng yếu tố riêng. | Nhóm engagement × assessment; heatmap. | Kiểm tra cỡ mẫu từng ô tổ hợp. |
| H07 | Liên hệ giữa VLE và At-Risk khác nhau theo `imd_band`. | IMD × engagement; heatmap hoặc clustered bar. | IMD là đặc trưng khu vực, không phải thu nhập cá nhân. |
| H08 | Đa dạng tài nguyên VLE liên hệ với kết quả tốt hơn. | Số `activity_type`; scatter/line theo At-Risk rate. | Loại tài nguyên bắt buộc và tự chọn không đồng nhất. |
| H09 | Tỷ lệ At-Risk khác nhau giữa các region. | `region`; geographic map và bar đối chiếu. | Xác minh mapping 13/13 vùng và cỡ mẫu trước kết luận. |

EDA dùng Python để kiểm tra phân bố và giả thuyết trước khi chốt inventory Tableau. Không chọn biểu đồ chỉ để đủ số lượng và không biến tương quan thành quan hệ nhân quả.

## Ràng buộc từ rubric

Phương án visual sau EDA phải đáp ứng nguyên vẹn:

- Số loại biểu đồ tối thiểu theo rubric, lựa chọn phù hợp kiểu dữ liệu.
- Ít nhất một geographic map thực sự.
- Filter nhiều cấp, drill-down, tooltip và cross-filtering.
- Storytelling/insight có số liệu, mẫu số, cỡ mẫu và giới hạn diễn giải.
- Tích hợp kết quả Logistic Regression lên dashboard.

`region` là nhãn OULAD, chưa phải geometry. T09 phải kiểm tra geographic role hoặc spatial file/mapping có nguồn và coverage đủ 13/13 nhãn; không tự chế tọa độ hoặc polygon.

## Tích hợp Logistic Regression

Python xuất dữ liệu theo đủ khóa `(code_module, code_presentation, id_student)` với tối thiểu:

- `actual_status`
- `predicted_status`
- `risk_probability`
- `dataset_split`
- `prediction_threshold`
- `model_version`
- `risk_band`, `error_type`, `cutoff_day`

Tableau dùng relationship theo đủ khóa hoặc một bảng dashboard đã được Python nối và kiểm tra. File mô hình `.joblib`/`.pkl` không phải data source Tableau.

KPI đánh giá đọc từ `model_metrics.csv`; confusion matrix, ROC/PR, calibration và khoảng tin cậy lần lượt đọc các bảng output đã kiểm tra. Trang Prediction mặc định lọc `dataset_split = test` và phải ghi rõ cutoff 105, threshold 0,415, model version `lr-oulad-c105-s42-v4`. Khoảng tin cậy dùng `model_confidence_intervals.csv`; không tự tính lại model trong Tableau. Chỉ dùng output khi `model_verification.csv` có 9/9 dòng PASS.

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
