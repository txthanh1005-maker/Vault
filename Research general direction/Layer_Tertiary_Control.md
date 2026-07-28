Direct Parent Connection: -> [[Power_Grid_System]]

# LAYER: TERTIARY CONTROL (TẦNG LẬP LỊCH DÀI HẠN)
*(Trục Thời gian L1: Tầng vĩ mô nhất của hệ thống, hoạt động hoàn toàn dựa trên Toán Kinh tế vĩ mô và phớt lờ nhiễu động vật lý tức thời).*

## 1. Bản chất & Tính chất Chung (Common Properties)
- **Khung thời gian:** Hoạt động trong vùng từ 24h (Day-ahead) đến nhiều ngày, nhiều tháng.
- **Mục tiêu cốt lõi:** Hoạch định chiến lược vận hành vĩ mô, ấn định lịch chạy máy và định giá năng lượng nhằm tối đa hóa lợi ích kinh tế xã hội và đảm bảo anượng (Dự phòng).
- **Đặc trưng:** Là một ván cờ vua kinh điển của các bài toán Quy hoạch nguyên hỗn hợp (MILP), phụ thuộc sống còn vào độ chính xác của các bản đồ dự báo thời tiết và phụ tải.

## 2. Sơ đồ Phân loại (Taxonomy / Splitting)
- **[[Unit_Commitment]]:** (UC) Bài toán tổ hợp máy phát (MIP) ấn định lịch BẬT/TẮT của hàng trăm tổ máy cho ngày mai. Chịu ràng buộc chặt chẽ bởi phí sấy lò, thời gian chạy/nghỉ tối thiểu, và lượng công suất dự phòng bắt buộc.
- **[[Day_Ahead_Market]]:** Thị trường điện Ngày tới. Khớp lệnh mua/bán tập trung để định hình Giá thanh toán bù trừ (Market Clearing Price - MCP).
- **[[Macro_Forecasting]]:** Hệ thống dự báo vĩ mô. Dự báo phụ tải đỉnh và sản lượng điện tái tạo dài hạn dựa trên dữ liệu khí tượng học.