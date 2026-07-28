# Layer: Primary Control & Physics (Tầng Vật lý Tức thời)

**Thuộc trục:** Trục Thời gian (Time-scale Dimension)
**Kết nối về gốc:** -> [[Power_Grid_System]]

**Mô tả:** Tầng này đại diện cho các đáp ứng vật lý tự nhiên và điều khiển phân tán tức thời của Lưới điện trong khung thời gian từ Micro-giây (µs) đến vài chục giây (s). Lớp vỏ này miễn nhiễm hoàn toàn với các quy luật kinh tế hay thị trường điện.

## Các Đặc tính (Properties) & Giao điểm (Intersections)
*Mọi bài báo nghiên cứu được gán tag/link tới Node này đều đang giải quyết bài toán vật lý, diễn ra trong chớp mắt.*

1. **Electromagnetic Transients (EMT):** Quá trình lan truyền sóng điện từ, dòng xung kích (inrush), nhiễu loạn sóng mang Inverter (Micro-giây đến mili-giây).
2. **Inertia (Quán tính tự nhiên):** Động năng tích lũy trong Rotor từ tính ($E = \frac{1}{2}J\omega^2$). Đóng vai trò là cái "phanh" hãm tốc độ sụt tần số đột ngột (RoCoF).
3. **Transient Stability (Ổn định quá độ):** Sự đồng bộ của góc Rotor dựa theo phương trình Swing. Nếu mất đồng bộ trong nửa giây, lưới sẽ rã.
4. **Droop Control:** Vòng điều khiển sơ cấp cục bộ trên từng máy phát ($\Delta P \propto \Delta f$). Nó phanh đà rơi tần số thành công nhưng luôn để lại một khoảng sai số tĩnh (Steady-state error).