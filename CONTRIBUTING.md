# Cách nhóm làm việc

## Vai trò và review

| Vai trò | Phụ trách chính | Người review ưu tiên |
|---|---|---|
| TV1 — Data | Chọn nguồn, audit, cleaning, join, feature engineering, data dictionary | TV2; TV3 kiểm tra đầu ra dùng cho BI/model |
| TV2 — Analysis | RQ, hypothesis, EDA, interaction, insight, storytelling, related work | TV1 kiểm tra số liệu; TV3 kiểm tra cách thể hiện |
| TV3 — BI + Model | Logistic Regression, Power BI, tương tác, dự báo, demo | TV1 kiểm tra dữ liệu; TV2 kiểm tra diễn giải |

Tên thật được gán vào TV1–TV3 tại issue/PR đầu tiên. Mỗi phần phải được **ít nhất một người khác review**; người review cần hiểu logic đủ để giải thích khi vấn đáp.

## Nhánh

`main` là nhánh tích hợp và mốc demo ổn định. Tạo nhánh ngắn từ `main` theo `data/T01-...`, `analysis/T03-...`, `bi-model/T12-...`, `docs/T18-...` hoặc `fix/Txx-...`. Tên nhánh chứa task ID, chữ thường, dấu gạch ngang. Các nhánh vai trò dài hạn, nếu được tạo để chia việc, chỉ là nhánh tập hợp tạm; vẫn đưa từng task vào `main` bằng PR nhỏ và xóa nhánh task sau merge. Sau khi thiết lập sườn và nhánh khởi đầu, không push trực tiếp vào `main`.

Ba nhánh khởi đầu cùng xuất phát từ `main`: `data/T01-oulad-source` (TV1), `analysis/T03-research-questions` (TV2), `bi-model/T04-dashboard-wireframe` (TV3). Mỗi người cập nhật nhánh của mình trước khi mở PR; các task kế tiếp tạo nhánh mới từ `main` hiện hành.

Trình tự: nhận task → tạo nhánh → cập nhật hiện vật và checklist → PR nêu task ID, nguồn dữ liệu, kiểm tra đã làm, ảnh/chứng cứ khi liên quan → một thành viên khác review → merge. Nếu task đổi định nghĩa chỉ số, target, ngưỡng hoặc hạt dữ liệu, cập nhật `docs/03-data-plan.md` và sổ quyết định trước khi merge.

## Quy tắc nội dung

- `data/raw/` giữ nguyên 7 CSV tải từ nguồn chính thức trên máy từng người; ghi link, ngày tải, phiên bản/checksum trong Data Quality Report. Không sửa raw.
- Script Python là nguồn tái tạo bảng sạch và feature; notebook dùng để giải thích audit/EDA. Nếu kết quả tính lại trong Power BI, phải đối chiếu với Python.
- Join theo đúng khóa và kiểm tra cardinality trước/sau. Với bảng sự kiện nhiều dòng, tổng hợp về hạt lượt học trước khi ghép vào bảng phân tích chính.
- Đặt tên biến và định nghĩa một lần trong data dictionary. Ghi rõ cửa sổ thời gian của feature, mẫu số của tỷ lệ và cách xử lý missing/outlier.
- EDA lưu biểu đồ tĩnh và Insight Log. Mỗi insight có câu hỏi, hình/bảng, số liệu, nhóm so sánh, giới hạn và tác động tới dashboard.
- Dashboard có chú thích, tiêu đề, tooltip và chỉ số có định nghĩa; kiểm thử filter, drill-down, cross-filter trên 4 trang.
- Báo cáo dùng trích dẫn IEEE; không copy code/dashboard mà không hiểu; mỗi người trình bày được phần của mình và luồng tổng thể.

## Definition of Done cho task

Task chỉ đóng khi: hiện vật đúng đường dẫn, tiêu chí nghiệm thu trong backlog đạt, có bằng chứng kiểm tra (số dòng/khóa, ảnh dashboard, kết quả model hoặc trang báo cáo phù hợp), tài liệu liên quan cập nhật và có review chéo. Các [cổng kiểm tra](docs/06-tasks-and-dependencies.md) xác nhận đầu ra đủ chất lượng để phần phụ thuộc sử dụng; nếu định nghĩa thay đổi, nêu tác động lên các phần đó.
