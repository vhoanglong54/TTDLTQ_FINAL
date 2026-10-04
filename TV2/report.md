# Báo cáo Nhật ký Thực thi (Report/Progress) của TV2

Tài liệu này ghi lại chi tiết từng phiên làm việc, hiện vật đầu ra, kết quả kiểm tra và quá trình giải quyết vấn đề của TV2 (Nadi). Các nhật ký ở đây giúp nhóm dễ dàng truy vết lại những thay đổi hoặc các quyết định phân tích.

## Progress Log Tổng Hợp
- **T03 (Chốt RQ & Giả thuyết):** Đã hoàn thành (30/09/2026). PR đã mở.
- **T08 (EDA cơ bản):** Chưa bắt đầu.
- **T10 (Phân tích sâu):** Chưa bắt đầu.
- **T11 (Insight Log):** Chưa bắt đầu.
- **T16 (QA Dashboard):** Chưa bắt đầu.
- **T18 (Báo cáo):** Chưa bắt đầu.

---

## Chi tiết các phiên làm việc

### Ngày 30/09/2026 — Thực hiện Task T03 (Chốt câu hỏi nghiên cứu & giả thuyết)
- **Nhiệm vụ:** Rà soát và chi tiết hóa các câu hỏi nghiên cứu, giả thuyết kiểm chứng trên dữ liệu OULAD; tạo biểu mẫu Insight Log dùng cho các task sau.
- **Input:** Kế hoạch gốc tại `docs/04-analysis-model-plan.md` và `docs/03-data-plan.md`.
- **Công nghệ / Tính năng:** Edit file Markdown thuần túy.
- **Output / Hiện vật:** 
  - `docs/04-analysis-model-plan.md` (Đã thay thế bảng tóm tắt bằng chi tiết 8 giả thuyết rõ ràng về biến gốc, biến tạo, đơn vị phân tích, mẫu số, và giới hạn).
  - `docs/insight-log.md` (Đã khởi tạo file biểu mẫu chuẩn để ghi nhận kết quả sau EDA).
- **Quyết định phân tích quan trọng:** 
  - Loại bỏ các biến ảo không tồn tại trong OULAD (sleep, motivation, study hours thực tế).
  - Quán triệt nguyên tắc: Việc đếm click VLE chỉ là *proxy* tương tác, không đại diện cho thời gian học.
  - Thêm phần yêu cầu TV1 xác minh dữ liệu ở bước Data Audit.
- **Lệnh / Test đã chạy:** `git add`, `git commit -m "Refs #3..."`, `git push`.
- **Đánh giá / Bàn giao:** Đạt điều kiện nghiệm thu của T03. Dữ liệu văn bản logic và bám sát thực tế OULAD.
- **Branch / PR:** Đã đẩy nhánh `analysis/T03-research-questions` và chờ Leader (TV3) nghiệm thu/Merge thông qua Issue #3.
