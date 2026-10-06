# TTDLTQ FINAL — Phân tích kết quả học tập với OULAD

Đồ án cuối kỳ môn **Tương tác dữ liệu trực quan (IDV)** của nhóm 3 thành viên. Câu hỏi trung tâm: những yếu tố nào liên hệ với kết quả học tập và có thể nhận diện sớm lượt học có nguy cơ **Fail/Withdrawn** hay không? Bộ dữ liệu đã chọn trong đề cương là **Open University Learning Analytics Dataset (OULAD)**; công nghệ đã chốt là **Python + Logistic Regression + Tableau**.

Repo đã hoàn thành nền tảng dữ liệu T01–T07; EDA, mô hình, dashboard và báo cáo tiếp tục được phát triển theo task. Chỉ ghi hoàn thành khi có hiện vật và bằng chứng kiểm tra tương ứng.

## Đọc theo thứ tự

1. [Phạm vi và quyết định đã chốt](docs/01-project-charter.md)
2. [Checklist đối chiếu rubric](docs/02-rubric-traceability.md)
3. [Nguồn, bảng, khóa nối và biến](docs/03-data-plan.md)
4. [Câu hỏi nghiên cứu, giả thuyết và mô hình](docs/04-analysis-model-plan.md)
5. [Đặc tả dashboard Tableau](docs/05-dashboard-spec.md)
6. [Task và quan hệ phụ thuộc](docs/06-tasks-and-dependencies.md)
7. [Báo cáo, video và vấn đáp](docs/07-report-and-defense.md)
8. [Quy tắc nhánh, PR và bàn giao](CONTRIBUTING.md)

Backlog, người phụ trách, nhánh làm việc và quan hệ phụ thuộc được quản lý tại [task và quan hệ phụ thuộc](docs/06-tasks-and-dependencies.md) cùng GitHub Issues. Bằng chứng kỹ thuật được lưu trong tài liệu dữ liệu, model và Tableau tương ứng; repo không duy trì thêm nhật ký cá nhân trùng lặp.

Nguồn yêu cầu gốc: [TTDLTQ_script.docx](docs/source/TTDLTQ_script.docx). Khi tài liệu trong repo diễn giải một ví dụ không phù hợp với OULAD, [sổ quyết định](docs/08-decisions-and-open-questions.md) ghi rõ lý do và việc cần xác nhận bằng dữ liệu thực.

`docs/02-rubric-traceability.md` giữ nguyên barem, điểm số và toàn bộ tiêu chí chấm. Tên công cụ trong ma trận được đồng bộ thành Tableau theo D17; việc đổi tên công cụ không làm thay đổi yêu cầu hoặc tổng điểm rubric.

## Bố cục dự kiến

```text
TTDLTQ_FINAL/
├── .github/                 # Mẫu issue và pull request
├── data/                    # Raw/interim không commit; clean_dataset.csv dùng chung được theo dõi
├── notebooks/               # 01 audit, 02 cleaning, 03 EDA
├── src/                     # Script Python xử lý, kiểm tra và mô hình
├── dashboard/               # Đặc tả Tableau, workbook/data source tương lai và bằng chứng
├── reports/                 # Báo cáo >= 40 trang, slide, kịch bản và link video
├── docs/                    # Yêu cầu, thiết kế, kế hoạch, quyết định
├── AGENTS.md                # Quy tắc cho trợ lý làm việc trong repo
└── CONTRIBUTING.md          # Quy tắc cho cả nhóm
```

## Mốc hoàn thành theo đề bài

- Dữ liệu có nguồn và giấy phép rõ, ít nhất **5.000 dòng** và **3 bảng** thực sự liên kết; có data dictionary, audit, cleaning, join và calculated fields.
- EDA Python có ít nhất **3–5 biểu đồ tĩnh**; phân tích sâu các tương tác; chốt **5–7 insight** có bằng chứng.
- Dashboard Tableau phải đạt các yêu cầu rubric về số loại biểu đồ, bản đồ, lọc nhiều cấp, drill-down, tooltip và cross-filtering. Loại biểu đồ cụ thể chưa chốt, sẽ quyết định sau EDA/Insight Log.
- Logistic Regression dự báo `At_Risk`; kết quả và xác suất rủi ro xuất hiện trên dashboard, kèm đánh giá và cách tránh rò rỉ nhãn.
- Báo cáo **tối thiểu 40 trang**, trích dẫn IEEE, demo trực tiếp, video backup và cả 3 thành viên sẵn sàng vấn đáp.

Nguồn dữ liệu chính: [Open University OULAD](https://research.stem.open.ac.uk/ouanalyse/dataset/); mô tả cấu trúc và phương pháp: [Kuzilek et al., Scientific Data (2017)](https://www.nature.com/articles/sdata2017171); bản phát hành và giấy phép: [UCI OULAD](https://archive.ics.uci.edu/dataset/349/open+university+learning+analytics+dataset).
