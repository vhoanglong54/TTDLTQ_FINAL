# 04 — Câu hỏi nghiên cứu, EDA và dự báo

## Sáu câu hỏi theo đề cương, diễn giải cho OULAD

| RQ trong DOCX | Câu hỏi sẽ kiểm tra trên OULAD |
|---|---|
| RQ1 — Phân bố kết quả? | `final_result` phân bố ra sao theo module, presentation và region? |
| RQ2 — Attendance/Study Hours? | VLE engagement sớm (clicks, ngày hoạt động, loại tài nguyên) liên hệ với kết quả ra sao? Đây là **proxy**, không phải attendance/giờ học. |
| RQ3 — Previous Performance? | `num_of_prev_attempts`, `highest_education`, và điểm bài đánh giá **đã có trước mốc** liên hệ với kết quả ra sao? Không gọi đó là điểm học phần trước. |
| RQ4 — Lifestyle/socioeconomic? | OULAD không có lifestyle. Kiểm tra khác biệt theo `imd_band`, `age_band`, `region`, disability và bối cảnh học tập; không quy kết nguyên nhân. |
| RQ5 — Tổ hợp rủi ro? | Những tổ hợp engagement, bài đánh giá sớm, previous attempts và IMD có At-Risk Rate cao hơn? |
| RQ6 — Logistic Regression? | Dùng feature trước mốc dự báo, mô hình có nhận diện `At_Risk` đủ tốt và ổn định giữa các nhóm/module? |

## Danh sách 8 giả thuyết kiểm tra trên OULAD

Giả thuyết là việc **cần kiểm tra**, chưa phải kết luận. Không suy diễn nhân quả. Phân biệt rõ VLE clicks không phải là thời gian học (study hours) hay điểm danh (attendance).

### H01: Kết quả học tập có sự phân bố khác biệt đáng kể giữa các môn học và đợt trình bày.
- **Biến gốc/tạo:** `final_result` (gốc) → `At_Risk` (tạo), `code_module`, `code_presentation`.
- **Đơn vị phân tích:** Lượt học `(code_module, code_presentation, id_student)`.
- **Phép so sánh:** Tỷ lệ `At_Risk` giữa các nhóm module-presentation.
- **Mẫu số:** Tổng số sinh viên đăng ký trong từng module-presentation.
- **Hình/bảng dự kiến:** Stacked bar chart tỷ lệ `At_Risk` theo module-presentation.
- **Điều kiện kết luận:** Có sự chênh lệch đáng kể (vd > 10%) giữa các nhóm hoặc kiểm định thống kê có ý nghĩa.
- **Giới hạn:** Khác biệt có thể do độ khó môn học, cấu trúc đánh giá chứ không hoàn toàn do sinh viên.

### H02: Tương tác VLE trong giai đoạn đầu (trước mốc dự báo) thấp có liên hệ với tỷ lệ `At_Risk` cao hơn.
- **Biến gốc/tạo:** `sum_click` (gốc) → `Early_Engagement_Level` (tạo).
- **Đơn vị phân tích:** Lượt học.
- **Phép so sánh:** Phân bố tổng click giai đoạn đầu giữa nhóm `At_Risk` = 1 và 0.
- **Mẫu số:** Tổng sinh viên có trong nhóm phân tích (loại trừ missing nếu có).
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
- **Biến gốc/tạo:** `imd_band` (gốc).
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
- **Điều kiện kết luận:** Tính đa dạng cao đồng biến nghịch với tỷ lệ `At_Risk`.
- **Giới hạn:** Có loại tài nguyên bắt buộc và loại không bắt buộc, khó phân tách.

EDA sẽ có tối thiểu 3–5 biểu đồ tĩnh theo rubric. Nêu mẫu số/cỡ nhóm, tránh xem tương quan là nguyên nhân và không chốt insight trước khi xem dữ liệu.

## Các câu hỏi cần TV1 xác minh trong Data Dictionary/Audit

Để EDA diễn ra chính xác, TV1 cần xác minh các vấn đề sau khi thực hiện Data Audit:
1. `studentVle.csv` và `vle.csv`: Các mã `id_site` và `activity_type` có khớp nhau không? Có tài nguyên nào không được sinh viên nào click?
2. Bảng `studentAssessment.csv`: Có bao nhiêu bài được nộp sau hạn (`date_submitted` > `date` của bảng `assessments`)? Tỷ lệ điểm null là bao nhiêu? `is_banked` phân bố ra sao?
3. Bảng `studentRegistration.csv`: `date_unregistration` null có hoàn toàn tương đương với trạng thái chưa rút khỏi khóa học không?
4. Khóa hạt dữ liệu: Việc tổng hợp từ bảng sự kiện (VLE, Assessment) về khóa `(code_module, code_presentation, id_student)` có làm mất quan sát nào không?

## Logistic Regression

**Target:** `At_Risk` từ `final_result`; **đầu ra:** khóa lượt học, `actual_status`, `predicted_status`, `risk_probability`, thời điểm/cửa sổ feature và phiên bản model. Có thể xuất `id_student` riêng để đối chiếu, nhưng **không** dùng một `student_id` đơn lẻ làm khóa bản ghi vì một người có thể có nhiều lượt học.

Pipeline dự kiến: clean data → chốt mốc dự báo tính từ ngày bắt đầu học phần (ví dụ ngày thứ 28, chỉ là ứng viên) → tạo feature đến mốc đó → chia train/test theo nhóm `id_student` hoặc presentation phù hợp → fit imputation/encoding/scaling trên train → Logistic Regression → đánh giá → chọn ngưỡng → xuất xác suất và phân lớp. Không đưa `final_result`, `At_Risk`, `date_unregistration` tương lai, điểm/nhấp chuột sau mốc vào feature. Cân nhắc `is_banked` và availability của bài đánh giá khi tính feature.

Đánh giá ít nhất confusion matrix, precision, recall, F1 cho lớp At-Risk, ROC-AUC hoặc PR-AUC và calibration/xác suất; so sánh với baseline đa số. Báo cáo tỷ lệ lớp, split, mốc dự báo, số dòng mỗi tập, ngưỡng phân lớp, sai số False Negative và khác biệt theo module/region nếu cỡ mẫu cho phép. Hệ số Logistic Regression được giải thích trong điều kiện mã hóa và chuẩn hóa; không gọi là hệ số tác động nhân quả.

Nếu dùng `final_result` để định nghĩa `At_Risk`, đây là **dự báo kết quả cuối khóa từ dữ liệu sớm**, không phải nhận diện người đã biết kết quả. Chọn mốc trước khi tạo feature để tránh rò rỉ. Kết quả thử nghiệm chỉ được công bố sau khi dữ liệu và mô hình thực sự chạy.

## Related Work (Trích dẫn ban đầu)

Tài liệu tham khảo dự kiến (theo chuẩn IEEE):
[1] J. Kuzilek, M. Hlosta, and Z. Zdrahal, "Open university learning analytics dataset," *Scientific Data*, vol. 4, no. 1, p. 170171, 2017. [Online]. Available: https://www.nature.com/articles/sdata2017171.
[2] M. Hlosta, Z. Zdrahal, and J. Zendulka, "Ouroboros: early identification of at-risk students without models based on legacy data," in *Proceedings of the Seventh International Learning Analytics & Knowledge Conference*, 2017, pp. 6-15.
