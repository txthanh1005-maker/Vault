---
weighttree: 0
contributors: 
  - Khanh k67
---
Direct Parent Connection: -> [[Load]], [[Source_of_System]]

# ELECTRIC VEHICLE (XE ĐIỆN VÀ TRẠM SẠC)
*(Hệ thống phương tiện giao thông tích hợp lưu trữ năng lượng di động, đóng vai trò như một mắt xích linh hoạt giữa mạng lưới điện và nhu cầu giao thông của người dùng).*

## 1. Bản chất & Tính chất Chung (Common Properties)
- Đều mang trong mình hệ thống lưu trữ hóa năng (Battery Pack) cỡ lớn.
- Đều thiết lập sự tương tác và trao đổi năng lượng với lưới điện thông qua các trạm sạc hoặc thiết bị đóng cắt cục bộ.
- Mang tính bất định cực cao về không gian và thời gian (phụ thuộc vào hành vi lái xe, SoC mục tiêu, và thời gian đến/rời đi).
- **Đặc tính Điều khiển qua Giá (Price-Controllable):** Lành tính và rất nhạy bén, dễ dàng bị điều hướng hành vi sạc/xả thông qua các tín hiệu giá điện (Time-of-Use, Real-Time Pricing) hoặc cơ chế tài chính. Điều này biến EV thành một thực thể Demand Response hoàn hảo.
- Đều có khả năng được gộp lại (Aggregation) để tham gia vào các cơ chế đáp ứng nhu cầu (Demand Response) hoặc tạo thành Nhà máy điện ảo (Virtual Power Plant).

## 2. Sơ đồ Phân loại (Taxonomy / Splitting)
*(Được phân nhánh dựa trên khả năng phần cứng, chiều luân chuyển năng lượng và đặc tính điều khiển).*

- **[[V1G_Electric_Vehicle]]:** (Xe điện sạc 1 chiều - Smart Charging). Đại diện cho các xe chỉ có khả năng hút năng lượng từ lưới điện (Thuần Tải). Tính linh hoạt nằm ở việc có thể điều chỉnh hoặc dịch chuyển thời gian/công suất sạc (Load shifting) nhưng luồng năng lượng vật lý là đơn hướng (không thể xả ngược).
- **[[V2G_Electric_Vehicle]]:** (Xe điện sạc/xả 2 chiều). Đại diện cho các xe mang biến tần 2 chiều (Bidirectional). Hoạt động linh hoạt như Tải (khi sạc) và như Nguồn (khi xả). Điểm mấu chốt là phải đối mặt với rào cản về hao mòn pin kép (Micro-cycling degradation) và độ trễ chuyển mạch (Dead-band).
- **[[Price_Responsive_EV]]:** (Xe điện phản ứng giá). Phân nhánh đặc thù chuyên phản ứng với các tín hiệu kinh tế thay vì mệnh lệnh điều độ vật lý. Điểm nhấn lõi là sự đánh đổi (Trade-off) phi tuyến tính giữa mức SoC mong muốn của người dùng và chi phí ẩn sinh ra từ việc nhồi dòng sạc giá rẻ.
- **[[EV_Sizing]]:** (Quy hoạch kích cỡ & Dung lượng). Nút mang tính chất Lập kế hoạch tĩnh (Planning Phase). Đại diện cho quá trình lựa chọn các giới hạn phần cứng (kW, kWh) dựa trên sự cân bằng giữa hàm chi phí CAPEX/OPEX, Lý thuyết hàng đợi bất định, và bị kìm hãm hoàn toàn bởi sức chứa Hosting Capacity tại điểm đấu nối PCC.