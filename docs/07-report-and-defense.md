# 07 — Báo cáo, demo và vấn đáp

Rubric yêu cầu báo cáo khoa học **ít nhất 40 trang**, demo chạy trực tiếp và video backup tóm tắt. Bảng dưới là khung mục lục, không cố định số trang; nhóm phân bổ để có đủ chiều sâu, hình minh họa và tài liệu tham khảo.

| Phần | Nội dung tối thiểu | Chủ trì |
|---|---|---|
| Introduction | Vấn đề, mục tiêu, 6 RQ, phạm vi dữ liệu và giới hạn suy luận | TV2 |
| Related Work | Nghiên cứu học tập/learning analytics, lý do chọn OULAD, nguồn trích IEEE | TV2 |
| Dataset & Data Dictionary | Nguồn/license, 7 bảng, hạt, khóa, sơ đồ quan hệ, biến, số dòng | TV1 |
| Preprocessing | Audit, missing/outlier/duplicate, chuẩn hóa, join, calculated fields, kiểm tra chất lượng | TV1 |
| EDA & Insights | 3–5 biểu đồ tĩnh tối thiểu, 8–10 hypothesis, 5–7 insight, interaction và storytelling | TV2 |
| Dashboard | Layout 4 trang, logic chọn ≥8 chart, map, luồng lọc/drill/tooltip/cross-filter, QA | TV3 |
| Model Comparison & Prediction | Target, cutoff, feature, group split; baseline, Logistic Regression, Random Forest; metric, threshold, hạn chế và kết quả so sánh | TV3 |
| Installation & Demo | Cách lấy data/tái tạo bảng/mở workbook Tableau, kịch bản demo, link video backup | TV3 + cả nhóm |
| Conclusion & References | Kết quả chính, giới hạn, hướng phát triển, tài liệu tham khảo chuẩn IEEE | Cả 3 |

Chèn sơ đồ pipeline **bài toán → nguồn OULAD → audit → cleaning → feature theo cutoff → EDA/interaction → Logistic Regression + Random Forest → so sánh/chọn mô hình → Tableau → báo cáo/demo**. Trình bày mã hoặc pseudocode đủ để giải thích quyết định xử lý, cách tính KPI, relationship/calculated field/cross-filter và logic mô hình; hình/chart có caption, nguồn và lý do chọn.

## Kịch bản demo đề xuất

1. Giới thiệu vấn đề, dữ liệu và hạt phân tích; chứng minh 7 bảng/khóa, số dòng, nguồn.
2. Overview: tỷ lệ kết quả, map vùng; đổi filter module/presentation/region và giải thích mẫu số.
3. Factor Analysis và Risk Analysis: dẫn 2–3 tương tác có bằng chứng, drill-down tới nhóm rủi ro, tooltip và cross-filter.
4. Prediction: giải thích mốc feature, xác suất, actual vs predicted và sai số; không hứa mô hình chẩn đoán cá nhân.
5. Tóm tắt giới hạn và khuyến nghị. Video backup phải đi qua cùng luồng chính và link nằm trong báo cáo.

## Vấn đáp

Mỗi người cần tự diễn giải: nguồn/giấy phép và khóa nối; cleaning và lý do giữ/xử lý missing; cách tính KPI; phân biệt association với causation; feature proxy; lý do tránh leakage; split và metric; cách filter/drill/cross-filter được lập trình. Tập thử câu hỏi chéo giữa TV1, TV2 và TV3. DOCX lưu ý bảo vệ tại lớp có thể quyết định điểm: trả lời yếu/ỷ lại hoặc dùng code/dashboard không hiểu có thể bị trừ tối đa 4 điểm, vi phạm liêm chính có thể bị hủy kết quả.
