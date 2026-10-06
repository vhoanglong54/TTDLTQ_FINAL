# T04 — Trạng thái bố cục dashboard sau khi chọn Tableau

**Issue gốc:** [#4](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/4)

**Owner:** TV3 / leader

**Trạng thái:** công cụ đã chuyển sang Tableau; phương án biểu đồ trước đây không còn là đặc tả triển khai. Inventory visual và layout chi tiết **chưa chốt**.

## Nguyên tắc giữ lại

- Hạt dữ liệu: một lượt học `(code_module, code_presentation, id_student)`.
- Storyline dự kiến: **điều gì đang xảy ra → yếu tố nào có liên hệ → nhóm nào cần chú ý → mô hình dự báo ra sao**.
- Không dùng ngôn ngữ nhân quả với dữ liệu quan sát; không gọi VLE click là attendance hoặc study hours.
- Mọi KPI phải ghi rõ measure, mẫu số, filter context và `N`.
- Output Logistic Regression do Python sinh; Tableau chỉ trực quan hóa và cho phép khám phá kết quả.

## Cấu trúc nội dung dự kiến

| Phần | Câu hỏi cần trả lời | Trạng thái visual |
|---|---|---|
| Overview | Quy mô dữ liệu và kết quả học tập tổng quan ra sao? | Chưa chốt |
| Factor Analysis | Những yếu tố và tương tác nào liên hệ với kết quả? | Chờ EDA/Insight Log |
| Risk Analysis | Những nhóm nào có tỷ lệ At-Risk đáng chú ý? | Chờ EDA/Insight Log và định nghĩa nhóm |
| Prediction | Logistic Regression nhận diện At-Risk tốt đến đâu và sai ở đâu? | Chờ T13 |

Bốn phần trên là kiến trúc thông tin dự kiến, không phải cam kết số dashboard/sheet hoặc loại biểu đồ cuối.

## Điều kiện khi chốt visual

Phương án sau EDA phải:

- Bao phủ đầy đủ yêu cầu bắt buộc trong rubric, gồm số loại biểu đồ tối thiểu, geographic map và các tương tác.
- Chọn chart dựa trên kiểu dữ liệu/câu hỏi, không chọn để đủ số lượng một cách hình thức.
- Có mapping region có nguồn, giấy phép và kiểm tra coverage.
- Có bằng chứng filter nhiều cấp, drill-down, tooltip và cross-filtering chạy đúng.
- Truy được mỗi insight về số liệu, mẫu số và Insight Log.

Quyết định visual/layout chính thức sẽ được ghi ở D18 hoặc quyết định kế tiếp sau khi Issue #6 hoàn thành.
