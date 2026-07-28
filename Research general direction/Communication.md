Direct Parent Connection: -> [[Power_Grid_System]]

# COMMUNICATION (VIỄN THÔNG & DỮ LIỆU)
*(Nút Không gian Lớp 1: Đóng vai trò là Hệ thần kinh truyền dẫn thông tin của Lưới điện Thông minh).*

## 1. Bản chất & Tính chất Chung (Common Properties)
- Làm nhiệm vụ thu thập, truyền tải và xử lý dữ liệu từ các thiết bị hiện trường về Trung tâm điều độ, đồng thời truyền lệnh điều khiển ngược lại.
- Các đặc tính kỹ thuật cốt lõi chi phối lớp này bao gồm: Băng thông (Bandwidth), Độ trễ (Latency), và Độ tin cậy (Reliability).
- Với sự bùng nổ của Smart Grid, lớp giao tiếp này mở ra một ranh giới tổn thương mới: An ninh mạng (Cybersecurity).

## 2. Sơ đồ Phân loại (Taxonomy / Splitting)
- **[[SCADA_System]]:** Hệ thống thu thập dữ liệu và điều khiển giám sát truyền thống (Tốc độ quét 2-4 giây). Đủ tốt cho vận hành thứ cấp nhưng quá chậm cho tính toán động học.
- **[[PMU_WAMS]]:** Hệ thống đo lường góc pha đồng bộ (Phasor Measurement Unit). Cung cấp độ phân giải tốc độ cực cao (50-60 mẫu/giây) cùng nhãn thời gian GPS, phục vụ phân tích động học sơ cấp.
- **[[IoT_Sensors]]:** Mạng lưới cảm biến Internet vạn vật phân tán tại các Microgrid và tải dân dụng.
- **[[Cybersecurity_Grid]]:** An ninh mạng. Săn tìm và chống lại các cuộc tấn công tiêm dữ liệu giả mạo (FDI Attacks) làm mù hệ thống SCADA, hoặc tấn công từ chối dịch vụ (DoS).