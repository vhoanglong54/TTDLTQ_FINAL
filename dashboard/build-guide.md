# T09 — Hướng dẫn khởi tạo Tableau

Tài liệu này ghi cách bắt đầu T09 sau quyết định chọn Tableau. Nó không chốt loại biểu đồ hoặc tuyên bố dashboard đã tồn tại.

## 1. Đầu vào

- Tableau Desktop Public Edition, Tableau Desktop hoặc môi trường Tableau được nhóm thống nhất.
- `data/processed/clean_dataset.csv` đã nghiệm thu ở T07.
- [Data Dictionary](../docs/09-data-dictionary.md), [baseline QA](qa-t09.md) và output model T13 khi có.

Không nhập bảy bảng raw vào workbook và không join bảng event nhiều dòng trong Tableau. Pipeline Python phải aggregate/validate dữ liệu trước khi bàn giao.

## 2. Data source

1. Kết nối `clean_dataset.csv` ở hạt một lượt học.
2. Kiểm tra kiểu dữ liệu, 32.593 dòng và khóa `(code_module, code_presentation, id_student)`.
3. Khi có kết quả T13, dùng relationship theo đủ ba khóa ở logical layer; giữ `model_name` làm dimension/filter và kiểm tra không nhân đôi KPI do mỗi lượt học có hai dự báo.
4. Đối chiếu KPI với baseline Python trước khi tạo storyline.

Không đưa file mô hình `.joblib`/`.pkl` vào Tableau. Python dùng Logistic Regression và Random Forest để sinh `model_name`, `risk_probability`, `predicted_status`, `actual_status`, `risk_band`, `threshold` và `model_version`; Tableau hiển thị các trường đó.

## 3. Ranh giới quyết định hiện tại

Đã chốt:

- Tableau là công cụ dashboard duy nhất.
- Python phụ trách data/EDA/model; Tableau phụ trách trực quan và tương tác.
- Giữ các tiêu chí bắt buộc của rubric và luồng nội dung dự kiến bốn phần.

Chưa chốt:

- Inventory và loại biểu đồ cụ thể.
- Layout cuối, theme/màu và cơ chế navigation.
- Geographic file/mapping dùng cho 13 region.
- Ngưỡng risk band và threshold mô hình.
- Định dạng bàn giao `.twb`, `.twbx` hay link Tableau Public.

Các mục này chỉ chốt sau EDA/Insight Log hoặc test kỹ thuật tương ứng và phải ghi vào [sổ quyết định](../docs/08-decisions-and-open-questions.md).

## 4. Bàn giao T09/T12

- Workbook mở được và chỉ dùng nguồn đã kiểm tra.
- Bảng relationship/calculated fields và công thức KPI có mô tả mẫu số.
- Kết quả test map, filter, drill-down, tooltip và cross-filtering có ảnh/bằng chứng.
- Version Tableau, checksum input và hạn chế kỹ thuật được ghi lại.

Không đánh dấu rubric dashboard hoàn thành chỉ vì đã tạo workbook hoặc worksheet.
