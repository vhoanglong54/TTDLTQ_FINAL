# Quy tắc làm việc trong repo

- Đọc `README.md`, `docs/02-rubric-traceability.md`, `docs/03-data-plan.md` và task liên quan trước khi sửa.
- Ghi đúng trạng thái thực tế; không viết rằng insight, mô hình hay dashboard đã có khi chưa có bằng chứng.
- DOCX gốc là yêu cầu; OULAD là nguồn quyết định các cột thực tế. Không tạo dữ liệu hoặc cột giả để làm đủ rubric. Ghi chênh lệch vào `docs/08-decisions-and-open-questions.md`.
- Không sửa `docs/02-rubric-traceability.md` hoặc PHẦN II trong DOCX. Công nghệ dashboard hiện hành là **Tableau** theo D17. T13 phải giữ Logistic Regression để đáp ứng rubric và so sánh thêm Random Forest theo D18; Tableau chỉ nhận bảng sạch và output model đã kiểm tra.
- Quy trình duy nhất: một Issue mô tả việc → một nhánh cho cụm task → một PR có tóm tắt và bằng chứng → leader quyết định merge → Issue đóng ngay. Không yêu cầu review chéo, reviewer, mention nhắc duyệt hay biên bản lặp lại.
- Không commit 7 CSV gốc, dữ liệu trung gian, thông tin định danh ngoài OULAD hoặc notebook có output nặng. Chỉ `data/processed/clean_dataset.csv` được theo dõi để nhóm dùng chung; phải giữ script tái tạo và checksum.
- Giữ `At_Risk` là nhãn từ `final_result`: `Fail/Withdrawn = 1`, `Pass/Distinction = 0`. Không dùng nhãn hoặc thông tin xảy ra sau mốc dự báo làm feature.
- Ghi rõ hạt dữ liệu là **một lượt học theo `(code_module, code_presentation, id_student)`**; không tự đồng nhất số lượt học với số sinh viên duy nhất.
- Insight là mối liên hệ trong dữ liệu quan sát; không viết quan hệ nhân quả nếu chưa có thiết kế chứng minh.
- Dashboard Tableau chỉ đọc bảng đã aggregate/output model đã kiểm tra; KPI phải có công thức và baseline Python để đối chiếu. Map cần geometry/mapping có nguồn, không tự chế tọa độ hoặc polygon. Không chốt inventory biểu đồ trước khi EDA/Insight Log được nghiệm thu; chỉ giữ các yêu cầu bắt buộc của rubric.
- Chỉ đánh dấu checklist rubric hoàn thành khi đã có đường dẫn tới hiện vật, bằng chứng kiểm tra và leader xác nhận.
