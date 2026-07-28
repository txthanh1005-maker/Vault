# Layer: Tertiary Control & Day-ahead (Tầng Lập lịch Dài hạn)

**Thuộc trục:** Trục Thời gian (Time-scale Dimension)
**Kết nối về gốc:** -> [[Power_Grid_System]]

**Mô tả:** Tầng vĩ mô nhất của hệ thống điện, hoạt động trong khung thời gian từ 24h đến nhiều ngày/tháng. Tầng này là một cuộc chơi cờ vua bằng Toán Kinh tế vĩ mô (Macro-economics), hoàn toàn phớt lờ các nhiễu động vật lý tức thời.

## Các Đặc tính (Properties) & Giao điểm (Intersections)
*Mọi bài báo nghiên cứu được gán tag/link tới Node này đều đang tính toán chiến lược vận hành ở cấp độ Ngày/Tuần.*

1. **Unit Commitment (UC):** Bài toán Tối ưu hóa nguyên hỗn hợp (MIP) để ấn định lịch BẬT/TẮT của hàng trăm tổ máy cho ngày mai. Bị kìm kẹp bởi: Phí sấy lò, thời gian chạy/nghỉ tối thiểu, và lượng công suất dự phòng bắt buộc phải có.
2. **Day-ahead Market (Thị trường Ngày tới):** Khớp lệnh mua/bán tập trung. Nơi định hình nên Giá thanh toán bù trừ (Market Clearing Price - MCP) quyết định sống còn tới doanh thu của các nhà máy điện.
3. **Macro-Forecasting:** Dự báo phụ tải, dự báo năng lượng tái tạo dài hạn dựa trên dữ liệu khí tượng học. Đầu vào tối quan trọng để chạy bài toán UC.