Direct Parent Connection: -> [[Power_Grid_System]]

# LAYER: SECONDARY CONTROL (TẦNG ĐIỀU ĐỘ NGẮN HẠN)
*(Trục Thời gian L1: Tầng giao thoa giữa Vật lý và Kinh tế, có nhiệm vụ dọn dẹp các tàn dư sai số do Tầng Sơ cấp để lại).*

## 1. Bản chất & Tính chất Chung (Common Properties)
- **Khung thời gian:** Hoạt động từ vài giây (s) đến một giờ (1h).
- **Mục tiêu cốt lõi:** Xóa bỏ hoàn toàn sai số tĩnh của tần số (kéo về chuẩn định mức 50/60Hz) và khôi phục dòng trao đổi trên các đường dây liên kết (Tie-line) về định mức.
- **Đặc trưng:** Hoạt động dựa trên Toán học Tối ưu hóa (Optimization). Hệ thống giải phương trình để chia tải cho các máy đang chạy sao cho Rẻ nhất, nhưng không được làm quá tải đường dây (Thermal Limits) hay sập điện áp (Voltage Limits).

## 2. Sơ đồ Phân loại (Taxonomy / Splitting)
- **[[Automatic_Generation_Control]]:** (AGC) Điều khiển thứ cấp tập trung dựa trên sai số khu vực (Area Control Error - ACE).
- **[[Optimal_Power_Flow]]:** (OPF) Trào lưu công suất tối ưu (5-15 phút). Giải phương trình tối ưu hóa hàm mục tiêu kinh tế/tổn thất.
- **[[Intra_Day_Market]]:** Thị trường điện hiệu chỉnh trong ngày. Nơi các máy phát và phụ tải mua/bán khẩn cấp để bù đắp sai lệch dự báo của năng lượng tái tạo.