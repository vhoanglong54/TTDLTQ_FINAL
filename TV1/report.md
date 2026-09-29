# Báo cáo công việc TV1 — Data

**Phạm vi:** nhật ký thực hiện các task TV1 và phần hỗ trợ/review. **Trạng thái dữ liệu hiện tại:** chưa có bằng chứng T01–T17 đã nghiệm thu. Ghi sự kiện theo ngày thực tế; nếu nhiều lần làm trong một ngày, thêm mục riêng. Mỗi kết quả phải có đường dẫn hiện vật, lệnh/test hoặc số liệu xác minh và người review khi báo hoàn thành. [Kế hoạch và prompt chung](plan.md) | [Backlog](../docs/06-tasks-and-dependencies.md).

## 29/09/2026 — Lập kế hoạch TV1 cho T01

| Mục | Nội dung thực tế |
|---|---|
| Nội dung đã làm | Đọc yêu cầu và backlog; tạo workflow, bảng giai đoạn, cổng kiểm tra, prompt chung và mẫu Progress Log cho TV1. Đây là công việc lập kế hoạch thuộc T01; chưa kiểm kê CSV. |
| Công nghệ/tính năng | Markdown, Mermaid để mô tả workflow; liên kết tới tài liệu repo. |
| Input | `README.md`, `docs/source/TTDLTQ_script.docx`, `docs/02-rubric-traceability.md`, `docs/03-data-plan.md`, `docs/06-tasks-and-dependencies.md`, `docs/08-decisions-and-open-questions.md`, `CONTRIBUTING.md`. |
| Output/kết quả | `TV1/plan.md` và `TV1/report.md` là tài liệu làm việc; tiêu chí nguồn, license, checksum, số dòng của T01 **chưa được kiểm chứng**. |
| Test/đánh giá | `git diff --check` không báo lỗi khoảng trắng; kiểm tra liên kết tương đối trong hai file không phát hiện đường dẫn hỏng. Chưa chạy test dữ liệu vì chưa có 7 CSV cục bộ. |
| Thủ công còn cần | Tải/giải nén 7 CSV RAW vào `data/raw/`; ghi nguồn, ngày tải, version/license và đối chiếu với Open University/UCI. |
| Bàn giao/review | `@nadinedatalab` (TV2) cần review; TV3 leader cần duyệt lựa chọn dataset; chưa bàn giao dữ liệu. |
| Issue/branch/PR | [Issue #1](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/1) đang mở theo thông tin nhóm cung cấp; branch `data/T01-oulad-source`; PR: chưa ghi nhận. |
| Trạng thái/ghi chú | **Tài liệu chưa được review; T01 chưa nghiệm thu.** Không cập nhật checklist rubric. |

## 29/09/2026 — T01: Kiểm kê nguồn và file OULAD cục bộ

| Mục | Nội dung thực tế |
|---|---|
| Nội dung đã làm | Kiểm kê trực tiếp 7 CSV tại `data/raw/`; đối chiếu với UCI dataset 349 và tài liệu `OULAD.names`; cập nhật `data/README.md`, D11 và kế hoạch T01. |
| Công nghệ/tính năng và phiên bản | Python 3.14, thư viện chuẩn `csv`, `hashlib`, `pathlib`; script tái tạo `src/verify_oulad_source.py` đọc theo luồng, không sửa CSV. |
| Input | 7 CSV cục bộ và `OULAD.names`; [UCI OULAD](https://archive.ics.uci.edu/dataset/349/open+university+learning+analytics+dataset). Ngày extract/kiểm kê: 29/09/2026; ngày tải archive chính xác chưa có bằng chứng cục bộ. |
| Output/kết quả | [Bảng kiểm kê T01](../data/README.md): 7 CSV, 10.900.970 dòng tổng, checksum SHA-256, số cột và header; `studentInfo` có 32.593 lượt học, 4 lớp `final_result`, 13 `region` khác null. |
| Test và bằng chứng | Chạy `python src/verify_oulad_source.py data/raw`: exit code 0; 7/7 file tồn tại và header bắt buộc PASS; quy tắc ≥5.000 dòng PASS. Chưa chạy uniqueness/cardinality/unmatched vì thuộc T02/T05/T07. |
| Đánh giá so với tiêu chí nghiệm thu | **Chưa nghiệm thu:** nguồn UCI và CC BY 4.0 đã đối chiếu; 7 file, ≥5.000 dòng, ≥3 bảng, khóa header, `final_result` và `region` có bằng chứng. Cần xác nhận ngày tải archive/version và TV2 review, TV3 leader duyệt. |
| Việc thủ công đã làm/còn cần | Đã extract 7 CSV vào raw. Còn cần: người tải xác nhận ngày tải archive; TV2 review; TV3 duyệt dataset; mở PR và dẫn link vào Issue #1. |
| Bàn giao cho ai, nhận gì, thời điểm | TV2 nhận bảng kiểm kê, biến `final_result`/`region` và giới hạn proxy; TV3 nhận khóa header và 13 region để kiểm tra map/BI. Bàn giao sau review qua PR. |
| Issue/branch/PR/reviewer | [Issue #1](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/1); branch `data/T01-oulad-source`; [PR #12](https://github.com/vhoanglong54/TTDLTQ_FINAL/pull/12) đã mở; đã yêu cầu `@nadinedatalab` review và `@vhoanglong54` duyệt lựa chọn dataset với vai trò TV3 leader. |
| Rủi ro, quyết định, ghi chú và bước tiếp theo | `OULAD.names` ghi 32.953 lượt học/đăng ký, còn hai CSV cục bộ có 32.593; D11 theo dõi. Chờ TV2 review, TV3 leader duyệt, xác nhận ngày tải archive/version; sau đó merge PR #12, bình luận nghiệm thu và đóng Issue #1. Có thể tạo khung T02, chưa chốt dictionary trước review T01. |

## 29/09/2026 — Đối chiếu Issue #1 và #2 với kế hoạch TV1

| Mục | Nội dung thực tế |
|---|---|
| Nội dung đã làm | Bổ sung tiêu chí chi tiết của [T01 #1](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/1) và [T02 #2](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/2) vào bảng giai đoạn, checklist và prompt. |
| Công nghệ/tính năng | Markdown checklist và liên kết Issue để truy vết nghiệm thu. |
| Input | Nội dung hai Issue và ảnh chụp do nhóm cung cấp; `TV1/plan.md`, `TV1/report.md`, backlog và data plan. |
| Output/kết quả | Kế hoạch nêu đúng `docs/09-data-dictionary.md`, nhánh T02, bảng kiểm kê T01 gồm số cột, reviewer cụ thể, điều kiện PR merge/đóng Issue; ghi nhận chênh lệch Kaggle so với nguồn nêu ở Issue trong decision log. |
| Test/đánh giá | Kiểm tra cấu trúc Markdown, đường dẫn tương đối và đối chiếu checklist với nội dung hai Issue; chưa kiểm tra CSV, nguồn thực tế hoặc schema. |
| Việc thủ công còn cần | T01: tải/kiểm kê CSV và ghi bằng chứng. T02: có thể chuẩn bị khung tài liệu; chốt theo 7 file sau T01. |
| Bàn giao/review | T01: TV2 `@nadinedatalab` review, TV3 leader duyệt dataset. T02: TV2 review nghĩa biến, TV3 kiểm tra BI/model. Chưa có review thực tế. |
| Issue/branch/PR | [#1](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/1) và [#2](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/2) đang mở theo thông tin nhóm cung cấp; PR chưa ghi nhận. |
| Trạng thái/ghi chú | Mới đồng bộ kế hoạch với Issue; cả T01 và T02 chưa nghiệm thu. |

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
| Issue/branch/PR/reviewer | ... |
| Rủi ro, quyết định, ghi chú và bước tiếp theo | ... |
```

## Progress Log

Một dòng cho mỗi task hoặc mốc hỗ trợ; cập nhật trạng thái khi có bằng chứng mới, không xóa lịch sử chi tiết phía trên. `Đã nghiệm thu` cần đường dẫn hiện vật, test đạt và reviewer. Với task chung, TV1 ghi phần mình đã hỗ trợ và owner xác nhận.

| Ngày cập nhật | Giai đoạn/task | Nội dung/hiện vật | Trạng thái | Test/đánh giá | Phụ thuộc | Bàn giao/reviewer | Việc thủ công/ghi chú |
|---|---|---|---|---|---|---|---|
| 29/09/2026 | T01 / [#1](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/1) — nguồn và kiểm kê | `data/README.md`, `src/verify_oulad_source.py`, D11; [PR #12](https://github.com/vhoanglong54/TTDLTQ_FINAL/pull/12) | Chờ review; chưa nghiệm thu | 7/7 header PASS; 10.900.970 dòng tổng; SHA-256 từng file; ≥5.000 dòng PASS | — | Đã yêu cầu `@nadinedatalab` review và `@vhoanglong54` duyệt lựa chọn dataset; sau đó T02/T03/T04 nhận bàn giao | Xác nhận ngày tải archive/version; theo dõi chênh lệch 32.953/32.593; merge/đóng Issue sau review |
| 29/09/2026 | T02 / [#2](https://github.com/vhoanglong54/TTDLTQ_FINAL/issues/2) — data dictionary | `docs/09-data-dictionary.md` chưa tạo | Chưa bắt đầu; có thể chuẩn bị khung | Chưa chạy; phải khớp 7 CSV sau T01 | T01 để nghiệm thu | `@nadinedatalab` review nghĩa biến; TV3 kiểm tra BI/model | Tạo khung trên nhánh `data/T02-data-dictionary`, chốt cột/missing/khóa sau T01 |
| — | T05 — audit | Chưa có | Chưa bắt đầu | Chưa chạy | T01–T02 | TV2 | Cần xem bất thường theo nghiệp vụ |
| — | T06 — cleaning | Chưa có | Chưa bắt đầu | Chưa chạy | T05 | TV2; sau đó T07 | Cần duyệt quy tắc missing/outlier |
| — | T07 — join/feature | Chưa có | Chưa bắt đầu | Chưa chạy | T06 | TV2 cho EDA; TV3 cho BI/model | Chốt nghĩa và cửa sổ feature |
| — | T03/T08/T09/T10/T13/T18 — hỗ trợ/review | Chưa có | Chưa bắt đầu | Chưa chạy | Theo từng task | TV2/TV3 | Không thay owner của task |
| — | T15 — QA Power BI | Chưa có | Chưa bắt đầu | Chưa chạy | T14 | TV3 cho T21 | Cần mở Power BI và đối chiếu Python |
| — | T17 — phần Data báo cáo | Chưa có | Chưa bắt đầu | Chưa chạy | T07 | TV2; cả nhóm cho T20 | Kiểm tra nguồn/trích dẫn IEEE |
| — | T20/T22 — tích hợp/bảo vệ | Chưa có | Chưa bắt đầu | Chưa chạy | T17–T21 theo backlog | Cả nhóm | Diễn tập vấn đáp thủ công; TV1 hỗ trợ T21 |

## Nguyên tắc ghi nhận

- Không ghi `Đạt` cho test chưa chạy; nêu lệnh, đầu vào, expected/actual và đường dẫn bằng chứng khi có.
- Khi thay đổi target, hạt, ngưỡng, mẫu số hoặc nguồn, cập nhật [data plan](../docs/03-data-plan.md) và [decision log](../docs/08-decisions-and-open-questions.md), sau đó nêu tác động tới TV2/TV3.
- Dữ liệu và notebook output nặng ở máy cục bộ; trong báo cáo ghi đường dẫn và cách tái tạo, không đưa CSV vào Git.
- Chỉ chuyển task sang `Đã nghiệm thu` sau khi tiêu chí backlog đạt, tài liệu liên quan cập nhật, hiện vật có đường dẫn và ít nhất một thành viên khác review.
