# 01 — Phạm vi đồ án

## Bài toán và kết quả mong muốn

Đề tài theo `TTDLTQ_script.docx`: **Nghiên cứu và phân tích các yếu tố ảnh hưởng đến kết quả học tập của sinh viên đại học**. Hướng đã chốt: mô tả kết quả học tập → tìm yếu tố liên quan → xem tương tác giữa nhiều yếu tố → nhận diện nhóm rủi ro → dự báo `At_Risk` → kể chuyện bằng dashboard Tableau. Vì dữ liệu là quan sát, phần phân tích dùng từ **liên hệ/khác biệt**, không kết luận tác động nhân quả.

Đơn vị phân tích chính là một **lượt học module-presentation của một sinh viên**. Kết quả gốc là `final_result` với bốn lớp `Pass`, `Distinction`, `Fail`, `Withdrawn`; nhãn mô hình là `At_Risk = 1` cho `Fail/Withdrawn`, `0` cho `Pass/Distinction`. Đây là kết quả **cuối khóa**; dự báo sớm cần chốt thời điểm chỉ dùng dữ liệu đã xuất hiện trước thời điểm đó.

## Quyết định đã chốt trong DOCX

| Thành phần | Quyết định |
|---|---|
| Dữ liệu | OULAD, 7 bảng CSV liên kết, hơn 5.000 dòng, có `region`, nguồn học thuật rõ |
| Xử lý/EDA | Python, Pandas/NumPy, Matplotlib/Seaborn |
| Dự báo | scikit-learn Logistic Regression, xác suất rủi ro và phân lớp |
| Trực quan tương tác | Tableau; bốn phần nội dung dự kiến: Overview, Factor Analysis, Risk Analysis, Prediction. Loại biểu đồ cụ thể chưa chốt. |
| Trọng tâm phân tích | Tương tác các yếu tố, risk profile, hành vi VLE theo thời gian |
| Nhóm | TV1 Data, TV2 Analysis, TV3 Dashboard + Model; leader điều phối, kiểm tra và nghiệm thu |
| Điều phối | Task theo quan hệ phụ thuộc; nhóm tự quản lý lịch và deadline |

## Phạm vi nghiên cứu thực tế với OULAD

OULAD có thông tin nhân khẩu học, vùng, `imd_band` (chỉ báo mức thiếu thốn của khu vực), lượt học, bài đánh giá, đăng ký và VLE clicks. `region` đủ để lập bản đồ phân bố theo vùng sau khi kiểm tra mapping địa lý. Dữ liệu **không ghi trực tiếp** sleep, stress, motivation, physical activity, attendance trên lớp, study hours hay điểm của học phần trước. Những mục này trong quy trình tổng quát của DOCX là gợi ý trước khi chọn data; xem phương án thay thế tại [kế hoạch dữ liệu](03-data-plan.md) và [đặc tả phân tích/Tableau](05-dashboard-spec.md).

Không tự tạo `Average Score` hay `Grade_Change` với ý nghĩa điểm cuối khóa nếu OULAD không có điểm số đó. Có thể tính điểm bài đánh giá đã nộp với tên và mẫu số tường minh.

## Tiêu chí hoàn tất đồ án

Đối chiếu từng hạng mục với [rubric](02-rubric-traceability.md); mỗi điều kiện có hiện vật, kiểm tra và người phụ trách. Báo cáo tối thiểu 40 trang, demo trực tiếp, video backup và vấn đáp của cả 3 thành viên là đầu ra bắt buộc, không chỉ là phụ lục.

## Nguồn

- Đề bài, barem và phương án nhóm: [TTDLTQ_script.docx](source/TTDLTQ_script.docx).
- [Trang OULAD của Open University](https://research.stem.open.ac.uk/ouanalyse/dataset/).
- [Bài công bố và mô tả 7 bảng của OULAD](https://www.nature.com/articles/sdata2017171), DOI: `10.1038/sdata.2017.171`.
- [Bản dữ liệu OULAD tại UCI](https://archive.ics.uci.edu/dataset/349/open+university+learning+analytics+dataset), DOI: `10.24432/C5KK69`, giấy phép CC BY 4.0.
