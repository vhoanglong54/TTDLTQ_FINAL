# 03 — Kế hoạch dữ liệu và hợp đồng bảng

## Nguồn và quyền sử dụng

Nguồn chọn trong DOCX là **OULAD**. Tải từ [Open University](https://research.stem.open.ac.uk/ouanalyse/dataset/) hoặc [UCI](https://archive.ics.uci.edu/dataset/349/open+university+learning+analytics+dataset); ghi URL cụ thể, ngày tải và checksum trong Data Quality Report. UCI công bố giấy phép **CC BY 4.0**; báo cáo cần trích dẫn cả nguồn dữ liệu và [bài mô tả](https://www.nature.com/articles/sdata2017171). Theo bài mô tả, `studentInfo` có 32.593 dòng và `studentVle` có 10.655.280 dòng; nhóm phải kiểm tra lại kích thước các file thực tế khi nghiệm thu task dữ liệu.

Không commit 7 CSV gốc hoặc bảng interim. Đặt raw trong `data/raw/`, giữ nguyên tên. Nhóm theo dõi duy nhất `data/processed/clean_dataset.csv` để dùng chung theo D16; các output khác phải tái tạo bằng script và không commit.

## 7 bảng và hạt dữ liệu

| CSV | Hạt / nội dung | Khóa và quan hệ chính |
|---|---|---|
| `courses.csv` | Một module-presentation; độ dài khóa học | `(code_module, code_presentation)` |
| `studentInfo.csv` | Một lượt học; nhân khẩu học, `region`, `imd_band`, `final_result` | `(code_module, code_presentation, id_student)` |
| `studentRegistration.csv` | Đăng ký/rút khỏi một lượt học | cùng khóa lượt học |
| `assessments.csv` | Một bài đánh giá của module-presentation | `id_assessment`; đối chiếu thêm module-presentation |
| `studentAssessment.csv` | Một bài nộp của một sinh viên | `(id_assessment, id_student)`; nối `assessments` để lấy module-presentation |
| `vle.csv` | Một tài nguyên VLE của module-presentation | `id_site` và module-presentation; kiểm tra uniqueness thực tế |
| `studentVle.csv` | Tương tác theo sinh viên, tài nguyên và ngày tương đối | `(code_module, code_presentation, id_student, id_site, date)` có thể lặp; **không giả định unique** |

**Hạt bảng phân tích chính:** `(code_module, code_presentation, id_student)`. Cần tổng hợp `studentAssessment` và `studentVle` về hạt này **trước** khi nối với `studentInfo`. Nối trực tiếp hai bảng sự kiện nhiều dòng sẽ nhân bản bản ghi và sai tỷ lệ/KPI. Kiểm tra khóa, mối quan hệ, số dòng unmatched và số dòng sau join trong audit.

## Biến gốc và biến tạo

| Nhóm | Biến có thể dùng | Ý nghĩa / lưu ý |
|---|---|---|
| Kết quả | `final_result` | Bốn lớp: Pass, Distinction, Fail, Withdrawn; chỉ dùng làm nhãn/kết quả, không làm feature mô hình |
| Học tập trước thời điểm hiện tại | `num_of_prev_attempts`, `studied_credits`, `highest_education` | Số lần thử module, tổng tín chỉ, trình độ đầu vào; **không phải điểm quá khứ** |
| Bối cảnh | `gender`, `age_band`, `disability`, `region`, `imd_band` | `imd_band` là mức thiếu thốn của **khu vực**, không suy ra thu nhập cá nhân |
| Đăng ký | `date_registration`, `date_unregistration` | Ngày tương đối so với bắt đầu khóa; `date_unregistration` có thể tiết lộ Withdrawn, không dùng làm feature dự báo sớm |
| Bài đánh giá | `assessment_type`, `date`, `weight`, `date_submitted`, `is_banked`, `score` | Tính số bài đã có hạn/đã nộp, điểm bài đã có trước mốc; không diễn giải trung bình `score` là điểm cuối khóa |
| VLE | `activity_type`, `date`, `sum_click` | Tính click/tuần, số ngày hoạt động, đa dạng tài nguyên, thay đổi engagement; click **không phải** giờ học hay attendance |

Calculated fields dự kiến, phải chốt ngưỡng bằng EDA và ghi lại trong data dictionary:

- `At_Risk`: `1` nếu `final_result` là `Fail`/`Withdrawn`, `0` nếu `Pass`/`Distinction`.
- `Performance_Level`: giữ bốn lớp `final_result`, hoặc nhóm gộp với mapping được ghi rõ; không tạo thang điểm không có nguồn.
- `Engagement_Level`: nhóm mức hoạt động VLE (thay cho `Attendance_Level` gợi ý trong DOCX); có thể dựa trên clicks/ngày hoạt động trong cùng giai đoạn.
- `Study_Intensity_Proxy`: cường độ click VLE trong cửa sổ thời gian xác định; là **proxy** tương tác, không phải study hours.
- `Assessment_Trend`: thay đổi điểm giữa các bài đánh giá đã nộp theo thời gian nếu đủ quan sát; thay cho `Grade_Change = Current_Score - Previous_Score`, vốn không có biến điểm cuối/điểm trước tương ứng.
- `Risk_Probability`, `Predicted_Status`, `Risk_Band`: đầu ra model; ngưỡng Low/Medium/High phải ghi rõ và kiểm định trước khi dùng trong dashboard.

`Sleep_Category` và biến lối sống khác không thể tạo từ OULAD. Không suy diễn hay bổ sung dữ liệu giả. Hướng tương tác khả thi: `VLE engagement × prior attempts`, `early assessment × VLE engagement`, `imd_band × activity_type/engagement`, `region × engagement` (kiểm tra cỡ mẫu).

## Audit → cleaning → feature

1. Kiểm tra shape, kiểu dữ liệu, missing, duplicate, unique key, outlier, invalid value và category không đồng nhất cho **từng bảng**.
2. Phân biệt missing có cấu trúc (`date_unregistration` trống khi không rút, bài không nộp không có dòng) với lỗi dữ liệu; ghi lý do drop/impute/giữ nguyên.
3. Chuẩn hóa kiểu cho ngày **tương đối** và chuỗi; kiểm tra score `[0,100]`, clicks không âm, khóa không null, thời gian phù hợp. Outlier được xem xét theo nghiệp vụ, không xóa máy móc.
4. Nối các dimension và tổng hợp sự kiện theo hạt mục tiêu; kiểm tra cardinality, unmatched keys, số dòng và phân bố kết quả trước/sau.
5. Tạo calculated fields và bảng cho EDA/Power BI/model; lưu data dictionary và Data Quality Report.

Đầu ra theo DOCX: `notebooks/01_data_audit.ipynb`, `notebooks/02_cleaning.ipynb`, `data/processed/clean_dataset.csv` (file cục bộ), Data Quality Report. Script trong `src/` sẽ là nguồn tái tạo khi bước này được triển khai.

## Thực thi T05–T07 trên raw đã xác minh

T05–T07 đã được chạy cục bộ ngày 04/10/2026 bằng `src/oulad_pipeline.py`; hiện vật và các lệnh tái tạo nằm trong [Data Quality Report](../reports/data-quality-report.md). Script giữ raw bất biến, ghi seven bảng interim và `clean_dataset.csv` vào các thư mục bị `.gitignore`.

- T06 chuẩn hóa mã `?` thành nullable missing, không impute và chỉ loại duplicate **toàn dòng** khi có bằng chứng. `studentVle` giảm từ 10.655.280 xuống 9.868.110 event sau khi loại 787.170 duplicate toàn dòng; outlier IQR được giữ để diễn giải, không xóa tự động.
- T07 aggregate `studentAssessment` và `studentVle` trước khi left join. Đầu ra giữ 32.593 lượt học, 0 unmatched dimension/registration/courses và 0 duplicate attempt key.
- `*_all_time` là aggregate mô tả cho EDA/BI, không phải feature dự báo sớm. D04 (cutoff) và D05 (ngưỡng nhóm) chưa được leader chốt; `final_result`, `At_Risk` và `date_unregistration` vẫn bị cấm khỏi feature model.
