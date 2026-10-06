# 04 — Câu hỏi nghiên cứu, EDA và dự báo

## Sáu câu hỏi theo đề cương, diễn giải cho OULAD

| RQ trong DOCX | Câu hỏi sẽ kiểm tra trên OULAD | Lý do giữ / sửa / loại bỏ |
|---|---|---|
| RQ1 — Phân bố kết quả? | `final_result` phân bố ra sao theo module, presentation và region? | **Giữ và bổ sung**: Thêm region vì OULAD hỗ trợ vẽ Geographic Map theo vùng. |
| RQ2 — Attendance/Study Hours? | VLE engagement sớm (clicks, ngày hoạt động, loại tài nguyên) liên hệ với kết quả ra sao? | **Sửa**: OULAD không có attendance/giờ học. Dùng VLE click làm proxy (đại diện) cho sự tương tác. |
| RQ3 — Previous Performance? | `num_of_prev_attempts`, `highest_education`, và điểm bài đánh giá sớm liên hệ kết quả ra sao? | **Sửa**: Thay vì điểm khóa trước (không có), dùng điểm bài nộp sớm và số lần học lại. |
| RQ4 — Lifestyle/socioeconomic? | Kiểm tra khác biệt theo `imd_band`, `age_band`, `region`, disability. | **Sửa**: Bỏ "Lifestyle" (không có data về sleep/stress). Giữ lại "socioeconomic" thông qua proxy `imd_band`. |
| RQ5 — Tổ hợp rủi ro? | Tổ hợp tương tác VLE, bài đánh giá sớm, previous attempts và IMD có tạo At-Risk Rate cao? | **Sửa**: Điều chỉnh các yếu tố tổ hợp cho khớp với biến thực tế của OULAD đã xác định ở trên. |
| RQ6 — Logistic Regression? | Logistic Regression và Random Forest nhận diện `At_Risk` tốt đến đâu trên cùng dữ liệu dự báo sớm? | **Giữ và mở rộng**: Logistic Regression đáp ứng trực tiếp rubric; Random Forest là đối chứng phi tuyến để kiểm tra hiệu quả và trade-off giải thích. |

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

## So sánh Logistic Regression và Random Forest

**Target:** `At_Risk` từ `final_result`. Logistic Regression là mô hình bắt buộc theo rubric; Random Forest là mô hình đối chứng phi tuyến. `DummyClassifier` theo lớp phổ biến chỉ là baseline kiểm tra, không được gọi là mô hình chính thứ ba.

Hai mô hình phải dùng cùng một snapshot feature theo cutoff, cùng khóa lượt học và cùng train/validation/test split theo nhóm `id_student`. Mọi imputation, encoding, scaling, resampling hoặc chọn feature chỉ được fit trên train. Logistic Regression dùng pipeline tiền xử lý phù hợp cho biến số/phân loại; Random Forest nhận cùng thông tin đã impute/encode nhưng không được tiếp cận thêm cột hoặc thời điểm khác.

Pipeline dự kiến: clean raw → audit cutoff ứng viên 14/28/42 ngày → chốt cutoff và quy tắc biên → tạo feature đến cutoff → group split train/validation/test → fit baseline + Logistic Regression + Random Forest → chọn hyperparameter/threshold trên train-validation → khóa cấu hình → đánh giá một lần trên test → xuất xác suất và phân lớp của cả hai mô hình. Không đưa `final_result`, `At_Risk`, `Performance_Level`, `date_unregistration`, aggregate `*_all_time` hoặc điểm/nhấp chuột sau cutoff vào feature. Cân nhắc `is_banked` và availability khác nhau của assessment theo module/presentation.

So sánh trên cùng test set bằng confusion matrix, precision, recall và F1 cho lớp At-Risk, ROC-AUC, PR-AUC và calibration/Brier score khi phù hợp. **Không chọn mô hình chỉ theo accuracy.** Ưu tiên PR-AUC, recall/F1 lớp At-Risk và chi phí False Negative; đồng thời ghi trade-off về calibration, khả năng giải thích và độ ổn định. Hệ số Logistic Regression và feature importance/permutation importance của Random Forest chỉ mô tả liên hệ trong mô hình, không phải tác động nhân quả.

Đầu ra dự báo dùng dạng long, một dòng cho mỗi `(code_module, code_presentation, id_student, model_name)`, gồm `actual_status`, `predicted_status`, `risk_probability`, `risk_band`, `dataset_split`, `cutoff_day`, `threshold` và `model_version`. Bảng metric riêng có một dòng cho mỗi mô hình/tập/ngưỡng. Tableau phải lọc hoặc tách theo `model_name` để không nhân đôi KPI của bảng lượt học.

Nếu dùng `final_result` để định nghĩa `At_Risk`, đây là **dự báo kết quả cuối khóa từ dữ liệu sớm**, không phải nhận diện người đã biết kết quả. Kết quả thử nghiệm và tên mô hình tốt hơn chỉ được công bố sau khi pipeline thực sự chạy; Logistic Regression vẫn phải được trình bày đầy đủ ngay cả khi Random Forest có metric tốt hơn.

## Related Work (Trích dẫn ban đầu)

Tài liệu tham khảo dự kiến (theo chuẩn IEEE) phục vụ cho phần phân tích Learning Analytics:
[1] J. Kuzilek, M. Hlosta, and Z. Zdrahal, "Open university learning analytics dataset," *Scientific Data*, vol. 4, no. 1, p. 170171, 2017. [Online]. Available: https://www.nature.com/articles/sdata2017171. (Mô tả chi tiết OULAD).
[2] M. Hlosta, Z. Zdrahal, and J. Zendulka, "Ouroboros: early identification of at-risk students without models based on legacy data," in *Proceedings of the Seventh International Learning Analytics & Knowledge Conference*, 2017, pp. 6-15. (Cách xác định rủi ro sớm).
[3] A. F. Wise, "Designing pedagogical interventions to support student use of learning analytics," *Proceedings of the Sixth International Conference on Learning Analytics & Knowledge*, 2016, pp. 203-211. (Giá trị của tương tác VLE đối với kết quả học tập).
[4] G. Siemens and R. S. d. Baker, "Learning analytics and educational data mining: towards communication and collaboration," *Proceedings of the 2nd International Conference on Learning Analytics and Knowledge*, 2012, pp. 252-254. (Nền tảng của phân tích học tập dựa trên dữ liệu hệ thống).
