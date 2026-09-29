# Dữ liệu

`raw/` giữ nguyên nguồn cục bộ. `interim/` là bảng tạm; `processed/` là bảng phân tích và đầu ra dự báo có thể tái tạo. CSV ở cả ba thư mục bị `.gitignore`; chỉ commit mã, tài liệu, schema, data dictionary và báo cáo chất lượng.

## T01 — Xác minh nguồn OULAD

**Task/Issue:** [T01 / #1](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/1)<br>
**Ngày extract và kiểm kê cục bộ:** 29/09/2026<br>
**Vị trí raw cục bộ:** `data/raw/`<br>
**Nguồn phân phối được đối chiếu:** [UCI Machine Learning Repository, dataset 349](https://archive.ics.uci.edu/dataset/349/open+university+learning+analytics+dataset), DOI [10.24432/C5KK69](https://doi.org/10.24432/C5KK69). UCI nêu nguồn gốc là Open University Learning Analytics Dataset; tài liệu nguồn là [Open University](https://research.stem.open.ac.uk/ouanalyse/dataset/).<br>
**Giấy phép:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/), theo trang UCI.
**Ngày tải archive:** chưa có bằng chứng độc lập trong thư mục cục bộ; ngày 29/09/2026 là ngày extract/kiểm kê. Cần xác nhận lại với người đã tải nếu Issue yêu cầu ngày tải chính xác.

### Kiểm kê 7 CSV thực tế

Kiểm kê được tạo bằng lệnh sau, chạy trực tiếp trên file cục bộ và không thay đổi raw:

```powershell
python src/verify_oulad_source.py data/raw
```

| File | Số dòng dữ liệu | Số cột | SHA-256 | Header khóa/biến T01 |
|---|---:|---:|---|---|
| `assessments.csv` | 206 | 6 | `9ce92381e4a0ac457f8e251b2eb2c179ed51e023ab30d1d671a0268bd62316ba` | `code_module`, `code_presentation`, `id_assessment` |
| `courses.csv` | 22 | 3 | `4f16eee7454b15e109b0a21a0e43be820e6846ed6f9301bb7feb5ab5ad737a75` | `code_module`, `code_presentation` |
| `studentAssessment.csv` | 173,912 | 5 | `b1839e17618a5de36b6d189a242d3123f7939a56c8c9ccef0c17b9199c614a6b` | `id_assessment`, `id_student` |
| `studentInfo.csv` | 32,593 | 12 | `815fbc3e2de29f79900bc63f0345a35db62c9146afaa1d86e7107b8b3beffc60` | `code_module`, `code_presentation`, `id_student`, `final_result`, `region` |
| `studentRegistration.csv` | 32,593 | 5 | `bbc87a0de1fe2a9ec6decce1ca6c1a02cf17b54ad3bdcc673fc6413b41c61d47` | `code_module`, `code_presentation`, `id_student` |
| `studentVle.csv` | 10,655,280 | 6 | `52668253d876c5becbcb72185977152700cecab2942aca807fecc3dd54b937f0` | `code_module`, `code_presentation`, `id_student`, `id_site`, `date` |
| `vle.csv` | 6,364 | 6 | `d7a146497edd0e47ef536144b58796c07b554002d3753e848b8e4cd8a56f4c38` | `id_site`, `code_module`, `code_presentation` |

Tổng số dòng của 7 CSV là **10.900.970**, nên đáp ứng điều kiện tối thiểu 5.000 dòng. Có 7 bảng và các header cung cấp khóa nối tự nhiên; kiểm tra uniqueness, cardinality và unmatched thuộc T02/T05/T07. CSV gốc không được commit.

### Bằng chứng phù hợp đề tài và rubric

- `studentInfo.csv` có 32.593 lượt học theo `(code_module, code_presentation, id_student)`, chứa `final_result` và `region`.
- `final_result` có bốn lớp: `Distinction` 3.024, `Fail` 7.052, `Pass` 12.361, `Withdrawn` 10.156. Mapping đã chốt cho các task sau là `At_Risk = 1` với `Fail/Withdrawn`, `0` với `Pass/Distinction`.
- `region` có 13 giá trị khác null, tạo đầu vào để TV3 kiểm tra geographic map. Đây chưa phải bằng chứng geocoding hoặc map hoạt động.
- Các bảng assessment, registration và VLE cho phép phân tích kết quả, hành vi học trực tuyến, bối cảnh và liên kết nhiều bảng. Không có biến đo trực tiếp sleep, study hours, attendance hoặc previous grade.

### Giới hạn và bàn giao

- File `OULAD.names` đi kèm ghi 32.953 lượt học/đăng ký, còn `studentInfo.csv` và `studentRegistration.csv` cục bộ đều có 32.593 dòng. Chênh lệch tài liệu này được theo dõi ở [decision log](../docs/08-decisions-and-open-questions.md); không thay đổi dữ liệu ở T01.
- Bằng chứng hiện có xác nhận file cục bộ và phù hợp dữ liệu/rubric. Issue #1 đã closed sau PR #12; giới hạn ngày tải archive/version và chênh lệch tài liệu vẫn được giữ ở decision log.
- T02 có thể dùng bảng kiểm kê này, nhưng phải lập `docs/09-data-dictionary.md` theo schema thực tế; chưa suy luận uniqueness hay độ sạch trước T05.

## T02 — Hợp đồng dữ liệu

Từ điển đủ 7 bảng/43 cột, hạt, khóa, sơ đồ quan hệ, test join và bàn giao T05 nằm tại [docs/09-data-dictionary.md](../docs/09-data-dictionary.md). Tái tạo kiểm tra từ raw:

```powershell
python src/profile_oulad_contract.py data/raw
```

T02 phát hiện `?` là mã thiếu/không biết tại một số cột; chưa làm sạch hay tạo feature.
