# 04 — Câu hỏi nghiên cứu, EDA và dự báo

## Sáu câu hỏi theo đề cương, diễn giải cho OULAD

| RQ trong DOCX | Câu hỏi sẽ kiểm tra trên OULAD | Lý do giữ / sửa / loại bỏ |
|---|---|---|
| RQ1 — Phân bố kết quả? | `final_result` phân bố ra sao theo module, presentation và region? | **Giữ và bổ sung**: Thêm region vì OULAD hỗ trợ vẽ Geographic Map theo vùng. |
| RQ2 — Attendance/Study Hours? | VLE engagement sớm (clicks, ngày hoạt động, loại tài nguyên) liên hệ với kết quả ra sao? | **Sửa**: OULAD không có attendance/giờ học. Dùng VLE click làm proxy (đại diện) cho sự tương tác. |
| RQ3 — Previous Performance? | `num_of_prev_attempts`, `highest_education`, và điểm bài đánh giá sớm liên hệ kết quả ra sao? | **Sửa**: Thay vì điểm khóa trước (không có), dùng điểm bài nộp sớm và số lần học lại. |
| RQ4 — Lifestyle/socioeconomic? | Kiểm tra khác biệt theo `imd_band`, `age_band`, `region`, disability. | **Sửa**: Bỏ "Lifestyle" (không có data về sleep/stress). Giữ lại "socioeconomic" thông qua proxy `imd_band`. |
| RQ5 — Tổ hợp rủi ro? | Tổ hợp tương tác VLE, bài đánh giá sớm, previous attempts và IMD có tạo At-Risk Rate cao? | **Sửa**: Điều chỉnh các yếu tố tổ hợp cho khớp với biến thực tế của OULAD đã xác định ở trên. |
| RQ6 — Logistic Regression? | Mô hình có nhận diện `At_Risk` đủ tốt và ổn định trước mốc dự báo không? | **Giữ**: Phù hợp với yêu cầu dùng classification model để nhận diện rủi ro sớm. |

## Danh sách 9 giả thuyết kiểm tra trên OULAD

Giả thuyết là việc **cần kiểm tra**, chưa phải kết luận. Không suy diễn nhân quả. Phân biệt rõ VLE clicks không phải là thời gian học (study hours) hay điểm danh (attendance).

### H01: Kết quả học tập có sự phân bố khác biệt đáng kể giữa các môn học và đợt trình bày.
- **Biến gốc/tạo:** `final_result` (gốc) → `At_Risk` (tạo), `code_module`, `code_presentation`.
- **Đơn vị phân tích:** Lượt học `(code_module, code_presentation, id_student)`.
- **Phép so sánh:** Tỷ lệ `At_Risk` giữa các nhóm module-presentation.
- **Mẫu số:** Tổng số lượt học trong từng module-presentation.
- **Hình/bảng dự kiến:** Stacked bar chart tỷ lệ `At_Risk` theo module-presentation.
- **Điều kiện kết luận:** Có sự chênh lệch đáng kể (vd > 10%) giữa các nhóm hoặc kiểm định thống kê có ý nghĩa.
- **Giới hạn:** Khác biệt có thể do độ khó môn học, cấu trúc đánh giá chứ không hoàn toàn do sinh viên.

### H02: Tương tác VLE trong giai đoạn đầu (trước mốc dự báo) thấp có liên hệ với tỷ lệ `At_Risk` cao hơn.
- **Biến gốc/tạo:** `sum_click` (gốc) → `Early_Engagement_Level` (tạo).
- **Đơn vị phân tích:** Lượt học.
- **Phép so sánh:** Phân bố tổng click giai đoạn đầu giữa nhóm `At_Risk` = 1 và 0.
- **Mẫu số:** Tổng số lượt học trong nhóm phân tích (loại trừ missing nếu có).
- **Hình/bảng dự kiến:** Boxplot tổng số click giai đoạn đầu; Bảng phân bố tỷ lệ `At_Risk` theo nhóm mức độ tương tác.
- **Điều kiện kết luận:** Trung vị số click nhóm `At_Risk` thấp hơn rõ rệt.
- **Giới hạn:** Click chỉ là proxy, không phản ánh thời gian học thực tế hay sự chú tâm.

### H03: Mức độ tương tác (số click) giảm dần theo tuần trước mốc dự báo có liên hệ với nguy cơ cao hơn.
- **Biến gốc/tạo:** `sum_click`, `date` (gốc) → `Engagement_Trend` (hệ số góc slope) (tạo).
- **Đơn vị phân tích:** Lượt học.
- **Phép so sánh:** Xu hướng click trung bình theo tuần giữa 2 nhóm `At_Risk`.
- **Mẫu số:** Số lượt học có phát sinh tương tác trong giai đoạn đó.
- **Hình/bảng dự kiến:** Line chart xu hướng click trung bình mỗi tuần.
- **Điều kiện kết luận:** Nhóm `At_Risk` có hệ số góc âm lớn hơn.
- **Giới hạn:** Có thể bị ảnh hưởng bởi lịch nộp bài tập (tương tác tăng đột biến gần hạn chót).

### H04: Số lần học lại môn học trước đó cao có liên hệ với tỷ lệ `At_Risk` cao hơn.
- **Biến gốc/tạo:** `num_of_prev_attempts` (gốc).
- **Đơn vị phân tích:** Lượt học.
- **Phép so sánh:** Tỷ lệ `At_Risk` giữa nhóm có lần học lại = 0 và > 0.
- **Mẫu số:** Tổng số lượt học theo từng nhóm số lần học lại.
- **Hình/bảng dự kiến:** Bar chart tỷ lệ `At_Risk` theo `num_of_prev_attempts`.
- **Điều kiện kết luận:** Tỷ lệ `At_Risk` tăng dần theo số lần học lại.
- **Giới hạn:** Biến này không chi tiết về kết quả các lần học trước.

### H05: Kết quả bài đánh giá sớm thấp có liên hệ với tỷ lệ nguy cơ cao hơn.
- **Biến gốc/tạo:** `score`, `date_submitted` (gốc) → `Early_Assessment_Score` (trung bình điểm sớm) (tạo).
- **Đơn vị phân tích:** Lượt học.
- **Phép so sánh:** Điểm trung bình bài đánh giá sớm giữa 2 nhóm `At_Risk`.
- **Mẫu số:** Lượt học có nộp bài đánh giá trước mốc.
- **Hình/bảng dự kiến:** Histogram điểm sớm theo 2 lớp `At_Risk`.
- **Điều kiện kết luận:** Phân bố điểm nhóm `At_Risk` lệch về mức thấp.
- **Giới hạn:** Không áp dụng được cho sinh viên chưa nộp bài nào trước mốc dự báo.

### H06: Tổ hợp tương tác VLE thấp và kết quả bài đánh giá sớm thấp xác định nhóm có nguy cơ cao hơn từng yếu tố đơn lẻ.
- **Biến gốc/tạo:** `sum_click`, `score` (gốc) → Phân nhóm tổ hợp (tạo).
- **Đơn vị phân tích:** Lượt học.
- **Phép so sánh:** Tỷ lệ `At_Risk` theo các ô tổ hợp (Tương tác Cao/Thấp × Điểm Cao/Thấp).
- **Mẫu số:** Số lượt học trong mỗi ô tổ hợp.
- **Hình/bảng dự kiến:** Heatmap tỷ lệ `At_Risk` theo 2 nhóm biến.
- **Điều kiện kết luận:** Tỷ lệ ở ô (Thấp, Thấp) cao nhất một cách có ý nghĩa.
- **Giới hạn:** Thiếu quan sát ở một số tổ hợp cực đoan.

### H07: Mức độ thiếu thốn khu vực (`imd_band`) kết hợp với mức tương tác VLE có liên hệ với tỷ lệ `At_Risk`.
- **Biến gốc/tạo:** `imd_band` (gốc), `sum_click` (gốc) → `Engagement_Level` (tạo).
- **Đơn vị phân tích:** Lượt học.
- **Phép so sánh:** Tỷ lệ `At_Risk` theo các dải `imd_band` và mức độ tương tác.
- **Mẫu số:** Tổng số lượt học theo tổ hợp `imd_band` và tương tác.
- **Hình/bảng dự kiến:** Clustered bar chart hoặc Heatmap.
- **Điều kiện kết luận:** Tác động của VLE có sự khác biệt giữa các dải `imd_band`.
- **Giới hạn:** `imd_band` là đặc trưng khu vực, không phản ánh chính xác thu nhập cá nhân.

### H08: Đa dạng loại tài nguyên VLE được sử dụng liên hệ với kết quả tốt hơn.
- **Biến gốc/tạo:** `activity_type` (gốc) → `Resource_Diversity` (tạo).
- **Đơn vị phân tích:** Lượt học.
- **Phép so sánh:** Tỷ lệ `At_Risk` theo số lượng loại tài nguyên đã click.
- **Mẫu số:** Tổng số lượt học theo nhóm số loại tài nguyên.
- **Hình/bảng dự kiến:** Scatter plot (hoặc line chart) giữa `Resource_Diversity` trung bình và tỷ lệ `At_Risk`.
- **Điều kiện kết luận:** Tính đa dạng cao tỷ lệ nghịch với tỷ lệ `At_Risk` (đa dạng càng cao, rủi ro càng thấp).
- **Giới hạn:** Có loại tài nguyên bắt buộc và loại không bắt buộc, khó phân tách.

### H09: Tỷ lệ sinh viên có nguy cơ (At-Risk) có sự khác biệt rõ rệt giữa các khu vực địa lý (Region).
- **Biến gốc/tạo:** `region` (gốc), `final_result` (gốc) → `At_Risk` (tạo).
- **Đơn vị phân tích:** Lượt học.
- **Phép so sánh:** Tỷ lệ `At_Risk` giữa các vùng.
- **Mẫu số:** Tổng số lượt học ở từng vùng.
- **Hình/bảng dự kiến:** Geographic Map thể hiện tỷ lệ At-Risk hoặc Bar chart so sánh.
- **Điều kiện kết luận:** Có sự phân hóa rõ ràng về rủi ro giữa các vùng địa lý, đủ để làm insight trực quan hóa.
- **Giới hạn:** `region` ở Anh không đồng nhất về diện tích/dân số, cần kiểm tra cỡ mẫu ở mỗi vùng trước khi kết luận.

EDA sẽ có tối thiểu 3–5 biểu đồ tĩnh theo rubric. Nêu mẫu số/cỡ nhóm, tránh xem tương quan là nguyên nhân và không chốt insight trước khi xem dữ liệu.

## Các câu hỏi cần TV1 xác minh trong Data Dictionary/Audit

Để EDA diễn ra chính xác, TV1 cần xác minh các vấn đề sau khi thực hiện Data Audit:
1. `studentVle.csv` và `vle.csv`: Các mã `id_site` và `activity_type` có khớp nhau không? Có tài nguyên nào không được sinh viên nào click?
2. Bảng `studentAssessment.csv`: Có bao nhiêu bài được nộp sau hạn (`date_submitted` > `date` của bảng `assessments`)? Tỷ lệ điểm null là bao nhiêu? `is_banked` phân bố ra sao?
3. Bảng `studentRegistration.csv`: `date_unregistration` null có hoàn toàn tương đương với trạng thái chưa rút khỏi khóa học không?
4. Biến `imd_band` trong bảng `studentInfo`: Có tới 1.118 giá trị `?` (missing). Cần TV1 xác nhận cơ chế xử lý (giữ nguyên làm category "Unknown" hay điền giá trị) vì nó ảnh hưởng đến giả thuyết H07.
5. Khóa hạt dữ liệu: Việc tổng hợp từ bảng sự kiện (VLE, Assessment) về khóa `(code_module, code_presentation, id_student)` có làm mất quan sát nào không?

## Model Logistic Regression — tài liệu chuẩn duy nhất

**Task:** T13 / Issue #8.

**Trạng thái:** đã chạy và kiểm tra local trên dữ liệu OULAD mới nhất; chờ leader duyệt trước khi commit.

**Thuật toán theo rubric:** Logistic Regression. `DummyClassifier` chỉ là baseline kỹ thuật, không phải mô hình chính thứ hai.

Mục này là nguồn giải thích duy nhất cho toàn bộ model: mục tiêu, dữ liệu, feature, cách train, thí nghiệm, metric, kết quả, artifact và contract Tableau. Các README khác chỉ được phép trỏ về đây, không lặp lại nội dung model.

### Mục tiêu dự báo

Tại ngày thứ 98 tính từ đầu một presentation, model ước lượng khả năng một **lượt học** `(code_module, code_presentation, id_student)` sẽ kết thúc bằng `Fail/Withdrawn`:

- `At_Risk = 1`: `Fail` hoặc `Withdrawn`.
- `At_Risk = 0`: `Pass` hoặc `Distinction`.
- Đầu ra chính: `risk_probability`, `predicted_at_risk`, `predicted_status` và `risk_band`.
- Mục đích: ưu tiên hỗ trợ học tập; không dùng để chẩn đoán, xử phạt hoặc ra quyết định tự động.

Đây là phân loại nhị phân, không phải hồi quy dự báo điểm. Vì vậy, Accuracy, Precision, Recall, F1, ROC-AUC, PR-AUC, Brier và Log Loss là các chỉ số phù hợp. `R²` và RMSE của hồi quy liên tục không phù hợp; khi cần chỉ báo tương tự, phải ghi đúng tên **McFadden pseudo-R²** và **Probability RMSE**.

### Khái niệm cần hiểu

| Khái niệm | Ý nghĩa trong dự án |
|---|---|
| Lượt học / attempt | Một bản ghi có khóa `(code_module, code_presentation, id_student)`; một sinh viên có thể có nhiều lượt học. |
| Target | `At_Risk`, tạo từ kết quả cuối và chỉ dùng làm nhãn huấn luyện/đánh giá. |
| Feature | Thông tin model được phép thấy tại cutoff, như điểm assessment sớm và hoạt động VLE. |
| Cutoff | Ngày chụp dữ liệu; event sau ngày 98 bị che để mô phỏng dự báo khi khóa học chưa kết thúc. |
| Cohort eligible | Lượt đã đăng ký và chưa rút trước hoặc tại cutoff, tức còn phù hợp để cảnh báo. |
| Logistic Regression | Ước lượng log-odds rồi dùng sigmoid chuyển thành xác suất rủi ro từ 0 đến 1. |
| Threshold | Ngưỡng đổi xác suất thành nhãn; xác suất từ 0,465 trở lên được cảnh báo At-Risk. |
| Hệ số / odds ratio | Dấu hệ số thể hiện hướng liên hệ; odds ratio là `exp(coefficient)`, không chứng minh nhân quả. |
| Regularization L1 | Hạn chế overfit và có thể đưa hệ số ít hữu ích về 0. |
| `C` | Nghịch đảo mức regularization; `C` nhỏ phạt mạnh hơn, `C` lớn phạt nhẹ hơn. |
| Train / Validation / Test | Train fit pipeline; validation chọn biến thể và threshold; test chỉ đánh giá cuối. |
| Baseline | `DummyClassifier` dự báo theo tỷ lệ lớp để làm mốc tối thiểu. |

`risk_probability = 0,80` nghĩa là model ước lượng rủi ro 80% theo pattern lịch sử và dữ liệu tại cutoff, không có nghĩa người học chắc chắn thất bại.

### Dữ liệu đầu vào và lý do chọn cutoff 98

Model không đọc `clean_dataset.csv` chứa aggregate toàn thời gian. Snapshot được dựng trực tiếp từ bảy bảng sạch ở `data/interim/`, được tái tạo từ bảy CSV raw chính thức sau khi kiểm checksum và header.

Cleaning mới giữ nguyên tổng **39.605.099** click: 10.655.280 dòng `studentVle.csv` được gom theo learner–resource–day và cộng `sum_click` thành 8.459.320 sự kiện logic. Không xóa 787.170 dòng trùng toàn dòng vì mỗi dòng có thể là một đóng góp click hợp lệ. Báo cáo kiểm chứng dữ liệu nằm tại [Data Quality Report](../reports/data-quality-report.md).

| Cutoff | Eligible attempts | At-Risk rate | Có VLE | Có assessment đã nộp | Ngày học còn lại trung bình |
|---:|---:|---:|---:|---:|---:|
| 28 | 27.522 | 44,12% | 96,71% | 74,13% | 227,95 |
| 42 | 27.012 | 43,06% | 97,39% | 84,36% | 213,96 |
| 56 | 26.513 | 41,98% | 97,76% | 87,67% | 200,00 |
| 84 | 25.720 | 40,19% | 98,03% | 93,18% | 172,01 |
| **98** | **25.349** | **39,31%** | **98,09%** | **93,41%** | **158,01** |
| 105 | 25.132 | 38,78% | 98,12% | 93,47% | 151,01 |
| 112 | 24.930 | 38,29% | 98,16% | 93,58% | 144,00 |

Ngày 98 là mốc sớm nhất trong nhóm ứng viên đã thử đạt mục tiêu Accuracy trên 80% ở validation và giữ được kết quả trên test, đồng thời trung bình còn 158 ngày để can thiệp. Chỉ event có `date <= 98` hoặc `date_submitted <= 98` được dùng. Attempt đăng ký sau cutoff hoặc đã rút trước/tại cutoff bị loại; `date_unregistration` chỉ xác định eligibility và không đi vào feature.

### Feature contract và chống leakage

Feature được dùng:

- Bối cảnh: module, presentation, tổ hợp module–presentation, highest education, số lần học trước, tín chỉ, ngày đăng ký và độ dài presentation.
- Assessment đến cutoff: số bài đến hạn/đã nộp/đã chấm, tỷ lệ hoàn thành, mean/weighted score, missing, late, banked và khoảng cách từ lần nộp cuối.
- VLE đến cutoff: event/click, ngày hoạt động, độ đa dạng resource/activity, click theo loại hoạt động, cửa sổ 7/28 ngày, mức thay đổi và khoảng cách từ hoạt động cuối.
- Count lệch mạnh dùng `log1p`; numeric median-impute và standardize; categorical điền `Unknown` và one-hot encode. Hai mươi tám tương tác cặp giúp Logistic Regression biểu diễn rủi ro kết hợp mà không đổi thuật toán.

Cột bị cấm: `id_student`, `final_result`, `At_Risk`, `Performance_Level`, `date_unregistration`, aggregate `*_all_time` và mọi assessment/VLE sau cutoff. `gender`, `region`, `imd_band`, `age_band`, `disability` chỉ dùng QA theo nhóm, không làm feature chính. Toàn bộ preprocessing chỉ được fit trên train.

OULAD không có timestamp công bố điểm. Pipeline giả định điểm của bài nộp trước/tại cutoff đã quan sát được tại cutoff; nếu nghiệp vụ công bố điểm trễ thì phải loại feature điểm hoặc bổ sung timestamp công bố thật rồi train lại.

### Luồng code và trách nhiệm

```text
7 CSV raw chính thức
  → verify_oulad_source.py: checksum/header/số dòng
  → oulad_pipeline.py: audit → clean → interim
  → at_risk_features.py: cohort ngày 98 + snapshot assessment/VLE
  → at_risk_experiments.py: so sánh biến thể bằng train-CV/validation
  → at_risk_model.py: split → preprocessing → tune → threshold → test
  → CSV/JSON/TXT kiểm chứng trong data/processed/model
  → Tableau chỉ đọc và trực quan hóa output đã kiểm tra
```

| File | Trách nhiệm |
|---|---|
| `src/at_risk_features.py` | Xác định cohort, feature được phép/cấm và tạo snapshot đúng hạt tại cutoff. |
| `src/at_risk_experiments.py` | So sánh baseline, feature bổ sung, spline và tương tác mà không dùng test để chọn. |
| `src/at_risk_model.py` | Group split, preprocessing, tuning Logistic Regression, threshold, test, bootstrap và xuất artifact. |
| `tests/test_at_risk_model.py` | Kiểm tra bảo toàn click, cohort, split, leakage, threshold và khoảng tin cậy. |

### Split, tuning và chọn phương án

Split theo nhóm `id_student`, bảo đảm một sinh viên chỉ xuất hiện trong một tập:

| Split | Attempts | Sinh viên | At-Risk | At-Risk rate |
|---|---:|---:|---:|---:|
| Train | 18.106 | 16.614 | 7.118 | 39,31% |
| Validation | 3.621 | 3.319 | 1.423 | 39,30% |
| Test | 3.622 | 3.321 | 1.424 | 39,32% |

Grid search trên train dùng 5-fold `StratifiedGroupKFold`, chọn theo PR-AUC với `C ∈ {0.01, 0.1, 1, 10}`, `class_weight ∈ {None, balanced}` và L1/L2. Test không tham gia chọn feature, hyperparameter hoặc threshold.

| Biến thể | CV PR-AUC | Validation Accuracy | Recall | F1 | Validation PR-AUC | Threshold |
|---|---:|---:|---:|---:|---:|---:|
| Baseline v2 | 0,8552 | 0,8122 | 0,7266 | 0,7525 | 0,8643 | 0,435 |
| Thêm feature an toàn + module–presentation | 0,8566 | 0,8158 | 0,7056 | 0,7507 | 0,8667 | 0,465 |
| Phương án trên + spline | 0,8567 | 0,8117 | 0,7041 | 0,7461 | 0,8658 | 0,460 |
| **Phương án trên + tương tác cặp (v3)** | **0,8577** | **0,8210** | **0,7133** | **0,7580** | **0,8687** | **0,465** |

V3 được chọn trước khi mở test vì tốt nhất về CV PR-AUC, validation Accuracy, F1 và PR-AUC trong các phương án giữ Recall tối thiểu 0,70. Cấu hình thắng: Logistic Regression L1/liblinear, `C=1`, `class_weight=None`; threshold 0,465 tối đa hóa Accuracy trên validation dưới ràng buộc Recall ≥ 0,70.

### Kết quả test và cách đọc độ tin cậy

| Metric | Logistic Regression | Dummy baseline |
|---|---:|---:|
| Accuracy | **0,8084** | 0,6068 |
| Balanced Accuracy | **0,7912** | 0,5000 |
| Precision At-Risk | **0,7821** | 0,0000 |
| Recall At-Risk | **0,7107** | 0,0000 |
| F1 At-Risk | **0,7447** | 0,0000 |
| ROC-AUC | **0,8747** | 0,5000 |
| PR-AUC | **0,8564** | 0,3932 |
| Brier Score, thấp hơn tốt hơn | **0,1325** | 0,2386 |

Confusion matrix: TN 1.916, FP 282, FN 412, TP 1.012. Model đúng 80,84% trên 3.622 lượt test và cao hơn baseline 20,15 điểm phần trăm. Chất lượng xác suất: Probability RMSE 0,3639; Probability MAE 0,2607; Log Loss 0,4134; McFadden pseudo-R² 0,3831. Mức đồng thuận nhãn: MCC 0,5936 và Cohen's Kappa 0,5919.

| Metric | Câu hỏi được trả lời |
|---|---|
| Accuracy | Bao nhiêu dự báo đúng trên toàn test? Có thể đẹp giả nếu một lớp chiếm đa số. |
| Balanced Accuracy | Khả năng nhận đúng hai lớp có cân bằng không? |
| Precision | Trong số lượt bị cảnh báo, bao nhiêu thực sự At-Risk? |
| Recall | Trong số lượt thực sự At-Risk, model tìm được bao nhiêu? |
| F1 | Precision và Recall có cân bằng không? |
| ROC-AUC | Model xếp hạng hai lớp tốt đến đâu qua mọi threshold? |
| PR-AUC | Chất lượng Precision–Recall của lớp At-Risk so với baseline 0,3932? |
| Brier/Log Loss | Xác suất có gần kết quả thật và có bị tự tin sai không? |
| MCC/Kappa | Mức đồng thuận khi xét đủ bốn ô confusion matrix và cơ hội ngẫu nhiên? |

Khoảng tin cậy 95% dùng 2.000 bootstrap theo `id_student`:

| Metric | Ước lượng | Cận dưới | Cận trên |
|---|---:|---:|---:|
| Accuracy | 0,8084 | 0,7955 | 0,8209 |
| Balanced Accuracy | 0,7912 | 0,7767 | 0,8050 |
| Precision | 0,7821 | 0,7593 | 0,8044 |
| Recall | 0,7107 | 0,6858 | 0,7342 |
| F1 | 0,7447 | 0,7250 | 0,7632 |
| ROC-AUC | 0,8747 | 0,8622 | 0,8859 |
| PR-AUC | 0,8564 | 0,8408 | 0,8707 |
| Brier | 0,1325 | 0,1257 | 0,1396 |

Kết luận đúng là “điểm ước lượng test đạt 80,84%”; không được nói Accuracy thật luôn trên 80% vì cận dưới hiện là 79,55%. Độ tin cậy còn đến từ group split, preprocessing chỉ fit trên train, threshold chỉ chọn trên validation, leakage guard, baseline, kiểm thử và khả năng tái tạo.

V3 cải thiện Accuracy (+0,50 điểm %), Precision, PR-AUC, Brier và MCC so với v2, nhưng Recall và F1 giảm nhẹ. Threshold cao hơn giảm cảnh báo nhầm nhưng bỏ sót thêm 38 trường hợp At-Risk. Đây là trade-off phải công bố, không được che giấu.

### Hệ số nổi bật

Hệ số phản ánh liên hệ sau preprocessing, không chứng minh nhân quả:

| Feature | Coefficient | Odds ratio | Hướng liên hệ |
|---|---:|---:|---|
| `code_module_GGG` | -2,1044 | 0,1219 | rủi ro thấp hơn |
| `code_module_AAA` | -1,4575 | 0,2328 | rủi ro thấp hơn |
| `assessment_weighted_score_cutoff` | -0,6548 | 0,5195 | rủi ro thấp hơn |
| `code_module_FFF` | 0,6442 | 1,9045 | rủi ro cao hơn |
| `code_module_DDD` | 0,5882 | 1,8007 | rủi ro cao hơn |
| `vle_days_since_last_activity` | 0,5594 | 1,7496 | rủi ro cao hơn |
| `assessment_completion_rate_cutoff` | -0,5128 | 0,5988 | rủi ro thấp hơn |
| `assessment_late_count_cutoff` | 0,3945 | 1,4836 | rủi ro cao hơn |

### Artifact và contract cho Tableau

Các artifact tái tạo được nằm trong `data/processed/model/`; bundle Python ở `models/logistic_regression.joblib`. Tableau không đọc `.joblib` và không train model, mà chỉ đọc CSV:

| File | Vai trò |
|---|---|
| `cutoff_audit.csv` | So sánh coverage giữa các cutoff. |
| `feature_snapshot.csv` | Snapshot ngày 98, một dòng mỗi attempt eligible. |
| `model_predictions.csv` | Actual, predicted, probability, risk band, split và error type. |
| `model_metrics.csv` | Metric Logistic/baseline theo split. |
| `model_confusion_matrix.csv` | Dữ liệu dựng confusion matrix. |
| `model_coefficients.csv` | Hệ số và odds ratio. |
| `model_curve_points.csv` | Điểm ROC và Precision–Recall. |
| `model_calibration.csv` | Xác suất dự báo và observed rate theo bin. |
| `model_confidence_intervals.csv` | Khoảng tin cậy bootstrap. |
| `model_subgroup_metrics.csv` | QA theo module, presentation và demographic. |
| `threshold_selection.csv` | Metric theo threshold và dòng được chọn. |
| `cv_results.csv` | Kết quả grid search. |
| `model_metadata.json` | Version, feature contract, cấu hình và SHA-256 artifact. |
| `model_evaluation.txt` | Báo cáo chạy tự sinh cục bộ; không phải tài liệu dự án thứ hai. |

Data source chính của Tableau là `model_predictions.csv`, hạt một attempt eligible. Nếu liên kết với bảng phân tích phải dùng đủ ba khóa `(code_module, code_presentation, id_student)` và không physical join event raw. Trang đánh giá mặc định `dataset_split = test`.

- KPI lấy từ `model_metrics.csv`: Accuracy, Precision, Recall, F1, ROC-AUC, PR-AUC.
- Xác suất/danh sách rủi ro lấy từ `model_predictions.csv`.
- Actual vs Predicted dùng `actual_status`, `predicted_status`, `error_type`.
- Confusion matrix, ROC/PR curve, calibration và QA theo nhóm dùng các CSV tương ứng.
- Risk band: `High` nếu probability ≥ 0,465; `Medium` nếu 0,2325 đến dưới 0,465; `Low` nếu thấp hơn 0,2325.
- Tooltip phải ghi cutoff 98, threshold 0,465 và model version `lr-oulad-c98-s42-v3`.

Output đánh giá lịch sử có actual label. Khi chấm một lượt học mới, input chỉ gồm feature quan sát đến cutoff; tuyệt đối không truyền `final_result` hoặc target vào model.

### Tái tạo và kiểm tra

```powershell
python -m pip install -r requirements.txt
python src/verify_oulad_source.py data/raw
python src/oulad_pipeline.py audit data/raw
python src/oulad_pipeline.py clean data/raw
python src/oulad_pipeline.py build data/raw
python src/oulad_pipeline.py report
python src/at_risk_model.py audit-cutoffs
python src/at_risk_experiments.py --n-jobs -1
python src/at_risk_model.py train --cutoff 98 --n-jobs -1
python src/at_risk_model.py validate
python -m unittest discover -s tests -v
```

`validate` kiểm tra grain/khóa, xác suất 0–1, threshold/label, error type, split không trùng sinh viên, metric test, tổng confusion matrix và checksum trong metadata. Kết quả local hiện tại: `MODEL_OUTPUT_VALIDATION=PASS`; 7/7 unit test pass.

### Giới hạn và ranh giới nghiệm thu

- Đây là dự báo trên dữ liệu quan sát lịch sử, không phải quan hệ nhân quả hoặc chẩn đoán cá nhân.
- Coverage assessment khác nhau giữa module/presentation; kết quả phụ thuộc cutoff.
- Test split từng được xem ở v2. V3 chỉ được chọn bằng train-CV/validation, nhưng muốn đánh giá hoàn toàn nguyên sơ cần holdout theo thời gian/presentation hoặc dữ liệu ngoài OULAD.
- Có code, output và metric local không đồng nghĩa T13 hoàn thành. Chỉ commit và đóng Issue sau khi leader duyệt code, lệnh tái tạo, metric, leakage checklist, cutoff/threshold và contract Tableau.

## Related Work (Trích dẫn ban đầu)

Tài liệu tham khảo dự kiến (theo chuẩn IEEE) phục vụ cho phần phân tích Learning Analytics:
[1] J. Kuzilek, M. Hlosta, and Z. Zdrahal, "Open university learning analytics dataset," *Scientific Data*, vol. 4, no. 1, p. 170171, 2017. [Online]. Available: https://www.nature.com/articles/sdata2017171. (Mô tả chi tiết OULAD).
[2] M. Hlosta, Z. Zdrahal, and J. Zendulka, "Ouroboros: early identification of at-risk students without models based on legacy data," in *Proceedings of the Seventh International Learning Analytics & Knowledge Conference*, 2017, pp. 6-15. (Cách xác định rủi ro sớm).
[3] A. F. Wise, "Designing pedagogical interventions to support student use of learning analytics," *Proceedings of the Sixth International Conference on Learning Analytics & Knowledge*, 2016, pp. 203-211. (Giá trị của tương tác VLE đối với kết quả học tập).
[4] G. Siemens and R. S. d. Baker, "Learning analytics and educational data mining: towards communication and collaboration," *Proceedings of the 2nd International Conference on Learning Analytics and Knowledge*, 2012, pp. 252-254. (Nền tảng của phân tích học tập dựa trên dữ liệu hệ thống).
