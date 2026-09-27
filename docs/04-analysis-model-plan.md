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

## Danh sách 9 giả thuyết khởi đầu

Giả thuyết là việc **cần kiểm tra**, chưa phải kết luận. TV2 cập nhật direction, số liệu, biểu đồ, giới hạn trong Insight Log.

| ID | Giả thuyết | Biến / phép so sánh |
|---|---|---|
| H01 | Kết quả khác nhau giữa các module-presentation | `final_result × code_module × code_presentation` |
| H02 | Tương tác VLE ít trong giai đoạn đầu liên hệ với At-Risk Rate cao hơn | early clicks/active days × `At_Risk` |
| H03 | Engagement giảm theo tuần trước mốc có liên hệ với nguy cơ | slope/trend clicks theo tuần × `At_Risk` |
| H04 | Số lần học lại module cao liên hệ với nguy cơ cao hơn | `num_of_prev_attempts × At_Risk` |
| H05 | Kết quả bài đánh giá sớm thấp liên hệ với nguy cơ cao hơn | early score/late submission × `At_Risk` |
| H06 | Kết hợp engagement thấp và kết quả bài sớm thấp xác định nhóm có nguy cơ cao hơn từng yếu tố đơn lẻ | hai nhóm feature × `At_Risk` |
| H07 | Tương tác giữa `imd_band` và engagement có khác biệt về nguy cơ | deprivation band × engagement × `At_Risk` |
| H08 | Loại tài nguyên VLE được sử dụng liên hệ với kết quả | `activity_type`/diversity × `final_result` |
| H09 | Tỷ lệ At-Risk thay đổi theo region, sau khi quan sát module mix/cỡ mẫu | `region × module × At_Risk` |

EDA sẽ có tối thiểu 3–5 biểu đồ tĩnh theo rubric; kế hoạch DOCX nhắm 5–7 insight chính, gồm overview, academic behavior, prior attempts/early assessment, bối cảnh, interaction, risk profile và prediction. Xem xét phân phối, xu hướng tuần, box/scatter, heatmap tương quan và bảng tỷ lệ theo tổ hợp. Nêu mẫu số/cỡ nhóm, tránh xem tương quan là nguyên nhân và không chốt insight trước khi xem dữ liệu.

## Logistic Regression

**Target:** `At_Risk` từ `final_result`; **đầu ra:** khóa lượt học, `actual_status`, `predicted_status`, `risk_probability`, thời điểm/cửa sổ feature và phiên bản model. Có thể xuất `id_student` riêng để đối chiếu, nhưng **không** dùng một `student_id` đơn lẻ làm khóa bản ghi vì một người có thể có nhiều lượt học.

Pipeline dự kiến: clean data → chốt mốc dự báo tính từ ngày bắt đầu học phần (ví dụ ngày thứ 28, chỉ là ứng viên) → tạo feature đến mốc đó → chia train/test theo nhóm `id_student` hoặc presentation phù hợp → fit imputation/encoding/scaling trên train → Logistic Regression → đánh giá → chọn ngưỡng → xuất xác suất và phân lớp. Không đưa `final_result`, `At_Risk`, `date_unregistration` tương lai, điểm/nhấp chuột sau mốc vào feature. Cân nhắc `is_banked` và availability của bài đánh giá khi tính feature.

Đánh giá ít nhất confusion matrix, precision, recall, F1 cho lớp At-Risk, ROC-AUC hoặc PR-AUC và calibration/xác suất; so sánh với baseline đa số. Báo cáo tỷ lệ lớp, split, mốc dự báo, số dòng mỗi tập, ngưỡng phân lớp, sai số False Negative và khác biệt theo module/region nếu cỡ mẫu cho phép. Hệ số Logistic Regression được giải thích trong điều kiện mã hóa và chuẩn hóa; không gọi là hệ số tác động nhân quả.

Nếu dùng `final_result` để định nghĩa `At_Risk`, đây là **dự báo kết quả cuối khóa từ dữ liệu sớm**, không phải nhận diện người đã biết kết quả. Chọn mốc trước khi tạo feature để tránh rò rỉ. Kết quả thử nghiệm chỉ được công bố sau khi dữ liệu và mô hình thực sự chạy.
