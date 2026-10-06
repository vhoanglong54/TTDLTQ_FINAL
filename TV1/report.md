# Báo cáo công việc TV1 — Data

**Phạm vi:** nhật ký thực hiện các task TV1 và phần hỗ trợ. **Trạng thái dữ liệu hiện tại:** T01–T07 đã nghiệm thu; T15/T17 và các phần cuối chưa bắt đầu. Mỗi kết quả phải có đường dẫn hiện vật, lệnh/test hoặc số liệu xác minh. [Kế hoạch](plan.md) | [Backlog](../docs/06-tasks-and-dependencies.md).

## 29/09/2026 — Lập kế hoạch TV1 cho T01

| Mục | Nội dung thực tế |
|---|---|
| Nội dung đã làm | Đọc yêu cầu và backlog; tạo workflow, bảng giai đoạn, cổng kiểm tra, prompt chung và mẫu Progress Log cho TV1. Đây là công việc lập kế hoạch thuộc T01; chưa kiểm kê CSV. |
| Công nghệ/tính năng | Markdown, Mermaid để mô tả workflow; liên kết tới tài liệu repo. |
| Input | `README.md`, `docs/source/TTDLTQ_script.docx`, `docs/02-rubric-traceability.md`, `docs/03-data-plan.md`, `docs/06-tasks-and-dependencies.md`, `docs/08-decisions-and-open-questions.md`, `CONTRIBUTING.md`. |
| Output/kết quả | `TV1/plan.md` và `TV1/report.md` là tài liệu làm việc; tiêu chí nguồn, license, checksum, số dòng của T01 **chưa được kiểm chứng**. |
| Test/đánh giá | `git diff --check` không báo lỗi khoảng trắng; kiểm tra liên kết tương đối trong hai file không phát hiện đường dẫn hỏng. Chưa chạy test dữ liệu vì chưa có 7 CSV cục bộ. |
| Thủ công còn cần | Tải/giải nén 7 CSV RAW vào `data/raw/`; ghi nguồn, ngày tải, version/license và đối chiếu với Open University/UCI. |
| Bàn giao | Chưa bàn giao dữ liệu ở thời điểm lập kế hoạch. |
| Issue/branch/PR | [Issue #1](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/1) đang mở theo thông tin nhóm cung cấp; branch `data/T01-oulad-source`; PR: chưa ghi nhận. |
| Trạng thái/ghi chú | **Tại thời điểm ghi:** T01 chưa nghiệm thu. Sau đó PR #12 đã merge và Issue #1 đã đóng. |

## 29/09/2026 — T01: Kiểm kê nguồn và file OULAD cục bộ

| Mục | Nội dung thực tế |
|---|---|
| Nội dung đã làm | Kiểm kê trực tiếp 7 CSV tại `data/raw/`; đối chiếu với UCI dataset 349 và tài liệu `OULAD.names`; cập nhật `data/README.md`, D11 và kế hoạch T01. |
| Công nghệ/tính năng và phiên bản | Python 3.14, thư viện chuẩn `csv`, `hashlib`, `pathlib`; script tái tạo `src/verify_oulad_source.py` đọc theo luồng, không sửa CSV. |
| Input | 7 CSV cục bộ và `OULAD.names`; [UCI OULAD](https://archive.ics.uci.edu/dataset/349/open+university+learning+analytics+dataset). Ngày extract/kiểm kê: 29/09/2026; ngày tải archive chính xác chưa có bằng chứng cục bộ. |
| Output/kết quả | [Bảng kiểm kê T01](../data/README.md): 7 CSV, 10.900.970 dòng tổng, checksum SHA-256, số cột và header; `studentInfo` có 32.593 lượt học, 4 lớp `final_result`, 13 `region` khác null. |
| Test và bằng chứng | Chạy `python src/verify_oulad_source.py data/raw`: exit code 0; 7/7 file tồn tại và header bắt buộc PASS; quy tắc ≥5.000 dòng PASS. Chưa chạy uniqueness/cardinality/unmatched vì thuộc T02/T05/T07. |
| Đánh giá so với tiêu chí nghiệm thu | **Đã nghiệm thu theo Issue #1 đã closed:** nguồn UCI và CC BY 4.0 đã đối chiếu; 7 file, ≥5.000 dòng, ≥3 bảng, khóa header, `final_result` và `region` có bằng chứng. Giới hạn ngày tải archive/version vẫn theo dõi D11. |
| Việc thủ công đã làm/còn cần | Đã extract 7 CSV vào raw. PR #12 đã merge và Issue #1 đã closed; người tải vẫn có thể bổ sung bằng chứng ngày tải archive vào D11 nếu tìm được. |
| Bàn giao cho ai, nhận gì, thời điểm | TV2 nhận bảng kiểm kê, biến `final_result`/`region` và giới hạn proxy; TV3 nhận khóa header và 13 region để kiểm tra map/dashboard. Bàn giao qua PR #12. |
| Issue/branch/PR | [Issue #1](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/1); branch `data/T01-oulad-source`; [PR #12](https://github.com/vhoanglong54/TTDLTQ_FINAL/pull/12) đã merge; leader đã nghiệm thu. |
| Rủi ro, quyết định, ghi chú và bước tiếp theo | `OULAD.names` ghi 32.953 lượt học/đăng ký, còn hai CSV cục bộ có 32.593; D11 theo dõi. PR #12 đã merge và Issue #1 đã closed; T02 dùng file thực để chốt dictionary. |

## 29/09/2026 — Đối chiếu Issue #1 và #2 với kế hoạch TV1

| Mục | Nội dung thực tế |
|---|---|
| Nội dung đã làm | Bổ sung tiêu chí chi tiết của [T01 #1](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/1) và [T02 #2](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/2) vào bảng giai đoạn, checklist và prompt. |
| Công nghệ/tính năng | Markdown checklist và liên kết Issue để truy vết nghiệm thu. |
| Input | Nội dung hai Issue và ảnh chụp do nhóm cung cấp; `TV1/plan.md`, `TV1/report.md`, backlog và data plan. |
| Output/kết quả | Kế hoạch nêu đúng `docs/09-data-dictionary.md`, nhánh T02, bảng kiểm kê T01 gồm số cột và điều kiện PR merge/đóng Issue; ghi nhận chênh lệch Kaggle so với nguồn nêu ở Issue trong decision log. |
| Test/đánh giá | Kiểm tra cấu trúc Markdown, đường dẫn tương đối và đối chiếu checklist với nội dung hai Issue; chưa kiểm tra CSV, nguồn thực tế hoặc schema. |
| Việc thủ công còn cần | T01: tải/kiểm kê CSV và ghi bằng chứng. T02: có thể chuẩn bị khung tài liệu; chốt theo 7 file sau T01. |
| Bàn giao | T01 bàn giao nguồn; T02 bàn giao nghĩa biến và schema dùng cho dashboard/model. Đây là ghi nhận lịch sử trước khi leader rút gọn quy trình. |
| Issue/branch/PR | [#1](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/1) và [#2](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/2) đang mở theo thông tin nhóm cung cấp; PR chưa ghi nhận. |
| Trạng thái/ghi chú | Mới đồng bộ kế hoạch với Issue; cả T01 và T02 chưa nghiệm thu. |

## 29/09/2026 — T02: Hợp đồng dữ liệu OULAD

| Mục | Nội dung thực tế |
|---|---|
| Nội dung đã làm | Đọc yêu cầu/repo/DOCX và Issue #1/#2; xác nhận #1 đã closed. Lập [data dictionary](../docs/09-data-dictionary.md) cho 7 bảng/43 cột, sơ đồ quan hệ, hạt, khóa, biến và bàn giao T05/T07/T13. |
| Công nghệ/tính năng và phiên bản | Python 3.14, thư viện chuẩn `csv`, `collections`, `pathlib`, `re`; script chỉ-đọc [profile_oulad_contract.py](../src/profile_oulad_contract.py). Markdown và Mermaid cho hiện vật. |
| Input | 7 CSV cục bộ `data/raw/`, checksum/số dòng T01 tại [data README](../data/README.md), UCI/Open University và T01/#1 đã closed. Ngày tải archive/version chính xác vẫn chưa có bằng chứng cục bộ. |
| Output/kết quả | `docs/09-data-dictionary.md`; `data/README.md` dẫn tới từ điển; D12 ghi cơ chế `?`. Không tạo/commit CSV processed hay feature. |
| Test và bằng chứng | `python src/profile_oulad_contract.py data/raw`: exit 0; 7 bảng/43 cột; 22/32.593/32.593/206/173.912/6.364 candidate keys PASS; 8 quan hệ join 0 unmatched; component key `studentVle` 0 null. Không assert unique event key `studentVle`. |
| Đánh giá nghiệm thu | **Đạt:** dictionary/schema/hạt/khóa/join; PR #13 đã merge và leader đã kiểm tra lại trên bộ OULAD chính thức. Cleaning, calculated fields, QA dashboard, leakage theo mốc chưa chạy vì thuộc T05/T07/T13/T15. |
| Việc thủ công đã làm/còn cần | Không còn thao tác thủ công cho T02. |
| Bàn giao cho ai | TV2: nghĩa biến, proxy và giới hạn insight. TV3: grain, join, `final_result`/`region`, danh sách leakage và map/model inputs. T05 nhận D12/missing audit; T07 nhận quy tắc aggregate. |
| Issue/branch/PR | [Issue #2](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/2), branch `data/T02-data-dictionary`; [PR #13](https://github.com/vhoanglong54/TTDLTQ_FINAL/pull/13) đã merge; leader nghiệm thu. |
| Rủi ro, quyết định, ghi chú | `?` không phải blank; không tự impute/drop. `date_unregistration`, nhãn và event sau mốc bị cấm feature. Chênh lệch 32.953/32.593 vẫn ở D11. |

## 04/10/2026 — T05–T07: Audit, cleaning, join và calculated fields OULAD

| Mục | Nội dung thực tế |
|---|---|
| Nội dung đã làm | Chạy pipeline tái tạo từ 7 CSV raw: audit T05, cleaning T06, aggregate/join T07; tạo `src/oulad_pipeline.py`, hai notebook không output nặng và [Data Quality Report](../reports/data-quality-report.md). Raw không bị sửa/commit. |
| Công nghệ/tính năng và phiên bản | Python 3.14.0, pandas 2.3.3, NumPy 2.3.5; pandas `validate="many_to_one"`/`validate="one_to_one"`, aggregate `groupby`, nullable missing và test grain. |
| Input | T01/#1, T02/#2 đã nghiệm thu; 7 CSV `data/raw/`; dictionary 7 bảng/43 cột; branch `data/T05-T07-data-pipeline`; D04/D05 chưa chốt. |
| Output/kết quả | T05 audit 7 bảng. T06 tạo 7 CSV `data/interim/` cục bộ; `studentVle` 10.655.280 → 9.868.110 sau loại 787.170 duplicate toàn dòng. T07 tạo `data/processed/clean_dataset.csv` cục bộ: 32.593 dòng, 35 cột, hạt `(code_module, code_presentation, id_student)`. |
| Test và bằng chứng | `python src/oulad_pipeline.py audit data/raw`, `clean data/raw`, `build data/raw`, `report`: PASS. T07: assessment/VLE dimension, registration, courses đều 0 unmatched; duplicate attempt key sau join = 0; `At_Risk`: 0=15.385, 1=17.208 và mapping bốn `final_result` PASS. `python src/verify_oulad_source.py data/raw`: 7/7 header/checksum/row count PASS sau pipeline. |
| Đánh giá so với tiêu chí nghiệm thu | **Đạt và đã nghiệm thu:** audit, cleaning, join/aggregate, calculated fields và Data Quality Report có bằng chứng; PR #16–#18 đã merge. D04/D05 chuyển sang T11/T13, không chặn EDA/dashboard; `*_all_time` bị cấm khỏi model sớm. |
| Việc thủ công đã làm/còn cần | Không còn việc T05–T07. D13–D16 đã chốt; T13 phải chốt mốc dự báo và ngưỡng trước khi tạo feature model. |
| Bàn giao cho ai, nhận gì, thời điểm | TV2: `clean_dataset.csv` tái tạo cục bộ, schema/hạt và Data Quality Report cho T08/T10. TV3: mapping `At_Risk`, aggregate mô tả, guard leakage và schema cho T09/T13; không dùng `*_all_time` cho model sớm trước D04. |
| Issue/branch/PR | [Issue #5](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/5); branch `data/T05-T07-data-pipeline`; [PR #16](https://github.com/vhoanglong54/TTDLTQ_FINAL/pull/16) đã merge vào `main`. |
| Rủi ro, quyết định, ghi chú và bước tiếp theo | `studentVle` event-key lặp không tự là lỗi nên chỉ loại duplicate toàn dòng. `imd_band` giữ nullable missing, không impute; outlier IQR giữ nguyên. Issue #5 đã đóng; handoff cho #6–#8 ở `docs/10-t05-t07-handoff.md`. |

## Mẫu nhật ký cho ngày tiếp theo

Sao chép khối này sau mỗi phiên làm. Ghi số liệu và lệnh thực; dùng `Chưa chạy` nếu chưa có kết quả.

```text
## DD/MM/YYYY — Txx: Tên giai đoạn

| Mục | Nội dung thực tế |
|---|---|
| Nội dung đã làm | ... |
| Công nghệ/tính năng và phiên bản | ... |
| Input (nguồn, version, đường dẫn, checksum nếu có) | ... |
| Output/kết quả (đường dẫn, số dòng/schema/khóa) | ... |
| Test và bằng chứng (lệnh, expected, actual, pass/fail) | ... |
| Đánh giá so với tiêu chí nghiệm thu | Đạt/Chưa đạt/Chưa kiểm được; giải thích ... |
| Việc thủ công đã làm/còn cần | ... |
| Bàn giao cho ai, nhận gì, thời điểm | ... |
| Issue/branch/PR | ... |
| Rủi ro, quyết định, ghi chú và bước tiếp theo | ... |
```

## Progress Log

Một dòng cho mỗi task hoặc mốc hỗ trợ; cập nhật trạng thái khi có bằng chứng mới, không xóa lịch sử chi tiết phía trên. `Đã nghiệm thu` cần đường dẫn hiện vật, test đạt và leader xác nhận. Với task chung, TV1 ghi phần mình đã hỗ trợ và owner xác nhận.

| Ngày cập nhật | Giai đoạn/task | Nội dung/hiện vật | Trạng thái | Test/đánh giá | Phụ thuộc | Bàn giao | Việc thủ công/ghi chú |
|---|---|---|---|---|---|---|---|
| 29/09/2026 | T01 / [#1](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/1) — nguồn và kiểm kê | `data/README.md`, `src/verify_oulad_source.py`, D11; [PR #12](https://github.com/vhoanglong54/TTDLTQ_FINAL/pull/12) | Đã nghiệm thu theo Issue #1 closed | 7/7 header PASS; 10.900.970 dòng tổng; SHA-256 từng file; ≥5.000 dòng PASS | — | T02/T03/T04 nhận bàn giao | Ngày tải archive/version chưa có bằng chứng; theo dõi chênh lệch 32.953/32.593 |
| 30/09/2026 | T02 / [#2](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/2) — data dictionary | `docs/09-data-dictionary.md`, `src/profile_oulad_contract.py`, D13; [PR #13](https://github.com/vhoanglong54/TTDLTQ_FINAL/pull/13) | Đã nghiệm thu | 7 bảng/43 cột; candidate keys PASS; 9 kiểm tra quan hệ/khóa đều 0 lỗi; `?` đã ghi nhận | T01 #1 closed | TV2/TV3 dùng đầu ra cho phân tích, dashboard/model | Không cleaning/feature/dashboard; T05 xác nhận và xử lý missing |
| 05/10/2026 | T05 — audit | `src/oulad_pipeline.py`, `notebooks/01_data_audit.ipynb`, [Data Quality Report](../reports/data-quality-report.md), PR #16 | Đã nghiệm thu | 7 bảng audit; D13 `imd_band` `?`=1.111; raw source verify PASS | T01–T02 | TV2/TV3 nhận audit | Event-key lặp đã được giải thích |
| 05/10/2026 | T06 — cleaning | `src/oulad_pipeline.py`, `notebooks/02_cleaning.ipynb`, PR #16 | Đã nghiệm thu | Raw checksum/header PASS; loại 787.170 duplicate toàn dòng `studentVle`; giữ outlier | T05 | TV2/T07 | `?` → nullable missing; raw bất biến |
| 05/10/2026 | T07 — join/feature | `data/processed/clean_dataset.csv`, D14/D16, PR #16–#18 | Đã nghiệm thu | 32.593 output; 0 unmatched; 0 duplicate attempt key; At_Risk mapping PASS | T06 | TV2 T08/T10; TV3 T09/T13 | `*_all_time` chỉ EDA/dashboard; D04/D05 chốt ở T11/T13 |
| — | T03/T08/T09/T10/T13/T18 — hỗ trợ | Chưa có | Chưa bắt đầu | Chưa chạy | Theo từng task | TV2/TV3 | Không thay owner của task |
| — | T15 — QA dashboard Tableau | Chưa có | Chưa bắt đầu | Chưa chạy | T14 | TV3 cho T21 | Cần mở workbook và đối chiếu KPI với baseline Python |
| — | T17 — phần Data báo cáo | Chưa có | Chưa bắt đầu | Chưa chạy | T07 | TV2; cả nhóm cho T20 | Kiểm tra nguồn/trích dẫn IEEE |
| — | T20/T22 — tích hợp/bảo vệ | Chưa có | Chưa bắt đầu | Chưa chạy | T17–T21 theo backlog | Cả nhóm | Diễn tập vấn đáp thủ công; TV1 hỗ trợ T21 |

## Nguyên tắc ghi nhận

- Không ghi `Đạt` cho test chưa chạy; nêu lệnh, đầu vào, expected/actual và đường dẫn bằng chứng khi có.
- Khi thay đổi target, hạt, ngưỡng, mẫu số hoặc nguồn, cập nhật [data plan](../docs/03-data-plan.md) và [decision log](../docs/08-decisions-and-open-questions.md), sau đó nêu tác động tới TV2/TV3.
- Dữ liệu và notebook output nặng ở máy cục bộ; trong báo cáo ghi đường dẫn và cách tái tạo, không đưa CSV vào Git.
- Chỉ chuyển task sang `Đã nghiệm thu` sau khi tiêu chí backlog đạt, tài liệu liên quan cập nhật, hiện vật có đường dẫn, test đạt và leader xác nhận.
