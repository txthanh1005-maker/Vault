---
weighttree: 0
contributors: 
  - Khanh k67
---
Direct Parent Connection: -> [[Electric_Vehicle]]

# V2G ELECTRIC VEHICLE (XE ĐIỆN SẠC/XẢ HAI CHIỀU)
*(Thực thể xe điện được trang bị biến tần hai chiều, có khả năng vừa sạc (G2V - Phụ tải) vừa xả năng lượng ngược lại lưới (V2G - Nguồn phát phân tán).*

## 1. Đặc tính Nội tại (Pure Entity View)
*(Phần Âm: Không liên quan đến môi trường bên ngoài)*
- **Người đảm nhiệm (Assignee):** pure_entity_researcher
- **Cấu trúc Phần cứng:** Đòi hỏi thay đổi toàn diện. Giao tiếp số bậc cao (PLC/CAN). Bộ sạc tích hợp hai chiều (BOBC) thay thế Diode bằng mạch Active Front End (AFE) làm biến tần/chỉnh lưu, sử dụng Bi-directional CLLC hoặc Dual Active Bridge (DAB) ở tầng DC-DC cách ly. PDU đối xứng chịu hồ quang cả hai chiều. BMS nâng cấp bắt buộc có tính toán giới hạn xả (Deep-discharge limits) và SoH chuyên sâu.
- **Phản ứng Hóa học (Micro-cycling):** Sự luân phiên sạc-xả liên tục buộc ion Lithium bị bứt ngược khỏi Anode, tạo ra co bóp cơ học (mechanical expansion/contraction) tần số cao trên tinh thể Graphite.
- **Nhiệt động lực học & Ứng suất nhiệt:** Xung nhiệt (Thermal spikes) đảo chiều liên tục tạo ra các điểm nóng cục bộ (hot spots). Yêu cầu hệ thống làm mát chất lỏng chủ động (Active Liquid Thermal Management) cực nhạy để dập tắt dao động nhiệt, tránh trượt nhiệt (Thermal Runaway).
- **Hao mòn Vật lý (Accelerated Degradation):** Hao mòn kép. Màng SEI liên tục bị phá vỡ và tái sinh, tiêu hao lượng lớn ion tự do (Loss of Lithium Inventory - LLI). Cực hàn bị nung nóng/nguội liên tục làm tăng nội trở thuần (Internal Resistance - IR). Dung lượng (Capacity fade) suy giảm rất nhanh.

## 2. Tương tác Cục bộ (Local Interaction View)
*(Phần Dương: Chỉ phân tích tương tác với HỆ THỐNG CẤP CAO HƠN BỌC TRỰC TIẾP LẤY NÓ)*
- **Người đảm nhiệm (Assignee):** grid_interaction_researcher
- **Hệ tham chiếu (Container):** [[Electric_Vehicle]] (Đóng vai trò Trạm sạc hai chiều).
- **Vai trò Tương tác:** Bidirectional DER (Nguồn năng lượng phân tán hai chiều). Vừa làm tải tiêu thụ, vừa "biến hình" thành máy phát cục bộ, tạo ra vòng lặp trao đổi năng lượng qua lại.
- **Luồng Giao tiếp & Tín hiệu:** Tương tác vật lý và tín hiệu đều mang tính hai chiều (bơm ngược dòng).
  - Inputs (từ Container): `P_setpoint_t` ($>0$ sạc, $<0$ xả), `Q_setpoint_t` (điều áp cục bộ), `Discharge_Permission`.
  - Outputs (tới Container): `Current_SoC`, `Available_Discharge_Capacity`, `P_actual`, `Q_actual`.
- **Hàm truyền & Giới hạn:** Không gian truyền tải mở rộng sang miền âm: $[-P_{max\_discharge}, P_{max\_charge}]$. Đặc biệt, Động lực học quá độ (Transient Dynamics) bắt buộc hệ thống phải đi qua **khoảng thời gian chết (Dead-band / Transition delay)** tại $P = 0$ khi đổi dấu lệnh để chuyển mạch an toàn. Tốc độ bám (Ramp rate) khi bơm ngược bị ràng buộc gắt gao hơn để tránh gây nhiễu điện áp tại thanh cái.

## 3. Điểm mạnh, Điểm yếu & Ứng dụng (Pros, Cons & Use Cases)
- **Điểm mạnh (Strengths):** Mang lại lợi ích to lớn cho độ đàn hồi của lưới (Resilience), tạo doanh thu tài chính (Arbitrage) từ chênh lệch giá cho chủ xe.
- **Điểm yếu (Weaknesses):** Hao mòn pin cực kì khắc nghiệt, hạ tầng sạc đắt đỏ, thuật toán điều khiển rất phức tạp.
- **Khi nào dùng (When to use):** Điều tần sơ cấp (Primary Control), Vehicle-to-Home (V2H), cứu vãn quá tải lưới điện trong ngắn hạn.
- **Khi nào KHÔNG dùng (When NOT to use):** Không dùng khi tuổi thọ pin (SoH) đã giảm, hoặc mức năng lượng hiện tại dưới ngưỡng di chuyển.