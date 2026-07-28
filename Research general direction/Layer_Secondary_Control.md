# Layer: Secondary Control & Real-time (Tầng Điều độ Ngắn hạn)

**Thuộc trục:** Trục Thời gian (Time-scale Dimension)
**Kết nối về gốc:** -> [[Power_Grid_System]]

**Mô tả:** Tầng giao thoa giữa Vật lý và Kinh tế. Nó hoạt động trong khung thời gian từ vài giây (s) đến một giờ (1h), với nhiệm vụ tối cao là dọn dẹp các tàn dư sai số do Tầng Primary để lại và bù đắp sự bất định của thiên nhiên.

## Các Đặc tính (Properties) & Giao điểm (Intersections)
*Mọi bài báo nghiên cứu được gán tag/link tới Node này đều đang giải quyết bài toán hiệu chỉnh thời gian thực.*

1. **Automatic Generation Control (AGC):** Điều khiển thứ cấp dựa trên sai số khu vực (Area Control Error - ACE). Mục tiêu: Xóa bỏ hoàn toàn sai số tĩnh của tần số (kéo về chuẩn 50/60Hz) và khôi phục dòng trao đổi liên kết (Tie-line) về chuẩn.
2. **Optimal Power Flow (OPF):** Bài toán Trào lưu công suất tối ưu (5-15 phút). Giải phương trình để chia tải cho các máy đang chạy sao cho Rẻ nhất, nhưng không được làm quá tải đường dây (Thermal Limits) hay sập điện áp (Voltage Limits).
3. **Intra-day Market (Thị trường trong ngày):** Sàn giao dịch hiệu chỉnh. Nơi các máy phát và phụ tải mua/bán khẩn cấp để bù đắp việc điện gió/mặt trời đột ngột mất đi so với dự báo ngày hôm qua.