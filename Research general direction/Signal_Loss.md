---
weighttree: 1
contributors: 
  - D.M.Hai K67
---
Direct Parent Connection: -> [[Communication]]

# Signal Loss (Mất mát Tín hiệu / Rớt gói tin)
Mất mát tín hiệu (Signal Loss / Packet Drop) xảy ra khi một hoặc nhiều gói dữ liệu đang truyền tải qua mạng lưới thông tin không thể đến được trạm thu, gây gián đoạn luồng thông tin.

## 1. Đặc tính Nội tại (Pure Entity View)
- **Người đảm nhiệm (Assignee):** AI
- **Bản chất vật lý & Mạng (Internal Physics):**
  - **Nghẽn mạng (Congestion):** Khi bộ đệm (buffer) của thiết bị mạng (router, switch) bị đầy, các gói dữ liệu đến sau sẽ bị loại bỏ (drop) để giải tỏa lưu lượng.
  - **Suy hao vật lý:** Tín hiệu đi qua cáp dẫn hoặc không gian vô tuyến bị suy hao năng lượng, nhiễu điện từ (EMI) hoặc rớt kết nối vật lý, khiến dữ liệu sinh lỗi CRC, không qua được bước xác minh gói tin và bị xóa bỏ ở lớp mạng.
- **Mô hình toán học:** Việc rớt gói thường được mô phỏng ngẫu nhiên qua biến Bernoulli (độc lập) hoặc chuỗi Markov (mất gói liên tiếp, cụm burst loss), tạo ra sự không chắc chắn trong chu kỳ cập nhật tín hiệu.

## 2. Tương tác Cục bộ (Local Interaction View)
- **Người đảm nhiệm (Assignee):** AI
- **Hệ tham chiếu (Container):** Hệ thống giám sát và điều khiển lưới điện (Smart Grid Control System).
- **Tương tác với lưới điện (Grid Interaction):**
  - **Mù thông tin đo lường:** SCADA và PMU gửi các biến trạng thái lưới điện nhưng bị rớt dọc đường, khiến Control Center thiếu dữ liệu thực, "nhắm mắt" điều khiển trong khoảng thời gian nhất định.
  - **Phá vỡ vòng lặp điều khiển (Control Loops):** Chuyển đổi một hệ điều khiển rời rạc có chu kỳ lấy mẫu cố định (uniform sampling) thành bất định (non-uniform). Nếu mất gói liên tục đủ dài, thuật toán điều khiển xuất tín hiệu sai (hoặc bằng 0), tạo xung lực ảo gây sốc tần số/điện áp cho thiết bị đầu cuối như Inverter hoặc Turbine.
  - **Động lực học tương tác:** Kích hoạt các cơ chế bù đắp dữ liệu. Nếu không được phục hồi, hệ điện cơ bị cộng hưởng với sai số của bộ điều khiển, dẫn tới phân rã lưới.

## 3. Điểm mạnh, Điểm yếu & Ứng dụng (Pros, Cons & Use Cases)
- **Điểm yếu cốt lõi:** Phá vỡ tính liên tục của dữ liệu bảo vệ và điều khiển, tạo ra tính bất định lớn nhất trong hệ Cyber-Physical của lưới điện.
- **Môi trường tác động:** Phổ biến ở các đường truyền vô tuyến (Wireless, IoT, vệ tinh) dễ bị nhiễu, hoặc hệ thống mạng bị tấn công DoS.
- **Cách khắc phục:** Đòi hỏi các module Khôi phục tín hiệu (Signal Recovery) như bộ lọc Kalman, nội suy dữ liệu, dự báo AI, hoặc thiết kế bộ điều khiển có khả năng chuyển mode an toàn (failsafe) khi rớt gói dài hạn.