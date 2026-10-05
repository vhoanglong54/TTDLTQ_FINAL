# Dashboard Power BI

Đích là 4 trang Overview, Factor Analysis, Risk Analysis và Prediction; ≥8 loại chart có map, filter nhiều cấp, drill-down, tooltip và cross-filtering. [Wireframe T04](wireframe.md) chốt bố cục, KPI, 16 visual instances/12 loại và tương tác; xem thêm [đặc tả](../docs/05-dashboard-spec.md). T09 tạo prototype; T12/T14 hoàn thiện; T15/T16/T21 kiểm tra.

## T09 — Bộ dựng dashboard

- [Hướng dẫn dựng Power BI skeleton](build-guide.md)
- [DAX measures](measures.dax)
- [Theme JSON](theme.json)
- [Baseline và checklist QA](qa-t09.md)
- [Quy ước ảnh bằng chứng](evidence/README.md)

Nhánh T09 đã chuẩn bị phần có thể tái tạo bằng Git. File `.pbix`, thử map và ảnh tương tác phải thực hiện thủ công trong Power BI Desktop; Issue #7 chỉ đóng sau T12.

Khi có `.pbix`, lưu file chính và ảnh/bảng QA ở đây. Nếu file vượt giới hạn GitHub, thống nhất Git LFS hoặc nơi lưu bản phát hành, ghi link và cách mở trong repo. Không đưa dữ liệu OULAD thô vào commit.
