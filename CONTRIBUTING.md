# Cách nhóm làm việc

## Vai trò

| Vai trò | Phụ trách chính |
|---|---|
| TV1 — Data | Chọn nguồn, audit, cleaning, join, feature engineering, data dictionary |
| TV2 — Analysis | RQ, hypothesis, EDA, interaction, insight, storytelling, related work |
| TV3 — Dashboard + Model | Logistic Regression, Tableau, tương tác, dự báo, demo |

Tên thật được gán vào TV1–TV3 tại Issue đầu tiên. Thành viên phối hợp trực tiếp khi cần làm rõ đầu vào; leader là người duy nhất quyết định merge.

## Quy trình duy nhất

1. **Một Issue** mô tả cụm task, owner, nhánh, đầu ra và bằng chứng cần có.
2. **Một nhánh** cho cụm task, tạo từ `main` mới nhất.
3. **Một PR** dùng `Closes #<issue>`, tóm tắt thay đổi và đặt toàn bộ bằng chứng tại PR.
4. **Leader quyết định merge** sau khi kiểm tra; không yêu cầu review chéo, reviewer hay mention nhắc duyệt.
5. **Merge xong đóng Issue ngay**; `Closes #<issue>` sẽ tự đóng. Không viết thêm biên bản trùng lặp ở Issue.

## Nhánh

`main` là nhánh tích hợp ổn định. Tạo nhánh từ `main` theo `data/T01-...`, `analysis/T03-...`, `bi-model/T12-...`, `docs/T18-...` hoặc `fix/Txx-...`. Tên nhánh chứa task ID, chữ thường, dấu gạch ngang. Không push trực tiếp vào `main`.

Danh sách Issue, thứ tự và nhánh nằm trong [bảng giao việc](docs/06-tasks-and-dependencies.md). Task phụ thuộc tạo nhánh mới từ `main` sau khi Issue đầu vào đã đóng. Nếu task đổi định nghĩa chỉ số, target, ngưỡng hoặc hạt dữ liệu, cập nhật `docs/03-data-plan.md` và sổ quyết định trong cùng PR.

## Quy tắc nội dung

- `data/raw/` giữ nguyên 7 CSV tải từ nguồn chính thức trên máy từng người; ghi link, ngày tải, phiên bản/checksum trong Data Quality Report. Không sửa raw.
- `data/processed/clean_dataset.csv` được commit để nhóm dùng chung; các file raw/interim/processed khác không commit nếu chưa có quyết định mới.
- Script Python là nguồn tái tạo bảng sạch, feature, baseline KPI và output dự báo; notebook dùng để giải thích audit/EDA. Tableau dùng các output đã kiểm tra, không tự tạo phiên bản công thức lệch với data contract.
- Join theo đúng khóa và kiểm tra cardinality trước/sau. Với bảng sự kiện nhiều dòng, tổng hợp về hạt lượt học trước khi ghép vào bảng phân tích chính.
- Đặt tên biến và định nghĩa một lần trong data dictionary. Ghi rõ cửa sổ thời gian của feature, mẫu số của tỷ lệ và cách xử lý missing/outlier.
- EDA lưu biểu đồ tĩnh và Insight Log. Mỗi insight có câu hỏi, hình/bảng, số liệu, nhóm so sánh, giới hạn và tác động tới dashboard.
- Dashboard có chú thích, tiêu đề, tooltip và chỉ số có định nghĩa; kiểm thử filter, drill-down, cross-filter trên 4 trang.
- Tableau là công cụ dashboard duy nhất. Workbook/data source chỉ được thêm khi T09 có hiện vật thật; lựa chọn visual cụ thể chốt sau EDA/Insight Log, nhưng vẫn phải bao phủ map, filter nhiều cấp, drill-down, tooltip, cross-filtering và số loại biểu đồ tối thiểu theo rubric.
- Báo cáo dùng trích dẫn IEEE; không copy code/dashboard mà không hiểu; mỗi người trình bày được phần của mình và luồng tổng thể.

## Điều kiện để leader merge

Hiện vật đúng đường dẫn; tiêu chí backlog/rubric đạt; PR có bằng chứng phù hợp như số dòng/khóa, ảnh dashboard, kết quả model hoặc trang báo cáo; tài liệu liên quan đã cập nhật. Leader merge là xác nhận nghiệm thu, sau đó Issue phải đóng ngay.
