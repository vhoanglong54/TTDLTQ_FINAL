# Dữ liệu

`raw/`: đặt nguyên 7 CSV OULAD (`courses`, `studentInfo`, `studentRegistration`, `assessments`, `studentAssessment`, `studentVle`, `vle`). `interim/`: các bảng sạch/tổng hợp tạm. `processed/`: bảng phân tích và đầu ra dự báo tái tạo được. CSV trong các thư mục này được `.gitignore`; chỉ commit mã, schema, data dictionary và báo cáo chất lượng.

T01 phải ghi nguồn tải, ngày tải, giấy phép, checksum, số dòng và tên 7 file; T02 tạo data dictionary. T05–T07 tạo Data Quality Report với audit, quyết định cleaning, số dòng trước/sau join và sơ đồ quan hệ. Xem [hợp đồng dữ liệu](../docs/03-data-plan.md).
