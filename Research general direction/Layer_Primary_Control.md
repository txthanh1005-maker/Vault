Direct Parent Connection: -> [[Power_Grid_System]]

# LAYER: PRIMARY CONTROL (TẦNG ĐIỀU KHIỂN SƠ CẤP)
*(Trục Thời gian L1: Tầng đại diện cho các đáp ứng vật lý tự nhiên và điều khiển phân tán tức thời, hoàn toàn miễn nhiễm với quy luật kinh tế).*

## 1. Bản chất & Tính chất Chung (Common Properties)
- **Khung thời gian:** Tác động siêu tốc từ Micro-giây ($\mu s$) đến vài chục giây ($s$).
- **Mục tiêu cốt lõi:** Cứu lưới khỏi sụp đổ ngay khi có sự cố. Hãm đà rơi của tần số (RoCoF) và giữ các máy phát không bị mất đồng bộ góc Rotor.
- **Đặc trưng:** Là phản xạ tự nhiên vô điều kiện của máy móc vật lý. Quá trình này hãm được thảm họa nhưng luôn tạo ra sai số tĩnh (Steady-state error) chứ không đưa lưới về được điểm chuẩn ban đầu.

## 2. Sơ đồ Phân loại (Taxonomy / Splitting)
- **[[Electromagnetic_Transients]]:** (EMT) Quá trình lan truyền sóng điện từ, dòng xung kích, nhiễu loạn sóng mang (Micro-giây đến mili-giây).
- **[[Grid_Inertia]]:** Động năng tích lũy trong Rotor từ tính ($E = 0.5 J \omega^2$). Đóng vai trò là cái "phanh" tự nhiên.
- **[[Transient_Stability]]:** Ổn định quá độ góc Rotor dựa theo phương trình Swing. (Quyết định sinh tử trong nửa giây đầu tiên).
- **[[Droop_Control]]:** Điều khiển độ dốc phân tán tại từng máy phát ($\Delta P \propto \Delta f$) để chia sẻ tải chênh lệch.