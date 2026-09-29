# Quy tắc làm việc trong repo

- Đọc `README.md`, `docs/02-rubric-traceability.md`, `docs/03-data-plan.md` và task liên quan trước khi sửa.
- Giữ giai đoạn hiện tại là **khởi tạo**; không viết rằng pipeline, insight, mô hình hay dashboard đã có khi chưa có bằng chứng.
- DOCX gốc là yêu cầu; OULAD là nguồn quyết định các cột thực tế. Không tạo dữ liệu hoặc cột giả để làm đủ rubric. Ghi chênh lệch vào `docs/08-decisions-and-open-questions.md`.
- Mỗi thay đổi phải gắn một task ID từ `docs/06-tasks-and-dependencies.md` và cập nhật tài liệu liên quan. PR và bằng chứng kiểm tra là bắt buộc; leader kiểm tra, nghiệm thu và merge. Không yêu cầu review chéo.
- Không commit dữ liệu CSV gốc, dữ liệu xử lý lớn, thông tin định danh ngoài OULAD, hoặc notebook có output nặng. Ghi nguồn và cách tái tạo thay thế.
- Giữ `At_Risk` là nhãn từ `final_result`: `Fail/Withdrawn = 1`, `Pass/Distinction = 0`. Không dùng nhãn hoặc thông tin xảy ra sau mốc dự báo làm feature.
- Ghi rõ hạt dữ liệu là **một lượt học theo `(code_module, code_presentation, id_student)`**; không tự đồng nhất số lượt học với số sinh viên duy nhất.
- Insight là mối liên hệ trong dữ liệu quan sát; không viết quan hệ nhân quả nếu chưa có thiết kế chứng minh.
- Chỉ đánh dấu checklist rubric hoàn thành khi đã có đường dẫn tới hiện vật, bằng chứng kiểm tra và leader xác nhận.
