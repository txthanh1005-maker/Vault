---
weighttree: 0
contributors: 
  - D.M.Hai K67
---
Direct Parent Connection: -> [[Communication]]

# Communication Delay
Độ trễ truyền thông (Communication Delay) là thời gian cần thiết để một gói dữ liệu hoặc tín hiệu di chuyển từ nút gửi (như cảm biến đo lường PMU/SCADA) đến nút nhận (như bộ điều khiển trung tâm) qua hệ thống mạng truyền tải.

## 1. Đặc tính Nội tại (Pure Entity View)
- **Người đảm nhiệm (Assignee):** D.M.Hai K67
- **Bản chất vật lý & Mạng (Internal Physics):**
  - **Thời gian xử lý & xếp hàng (Queueing & Processing):** Độ trễ do bộ định tuyến (router) và switch xử lý gói dữ liệu trong hệ thống mạng.
  - **Thời gian truyền tải (Transmission Latency):** Phụ thuộc trực tiếp vào băng thông kênh truyền và kích thước của gói tin.
  - **Thời gian lan truyền (Propagation Delay):** Tốc độ truyền dẫn trên cáp quang hoặc không gian vật lý dựa trên khoảng cách địa lý.
- **Mô hình toán học:** Tổng độ trễ $T_{delay} = T_{processing} + T_{queueing} + T_{transmission} + T_{propagation}$. Các gói tin có thể chịu độ trễ cố định (constant delay) hoặc biến thiên ngẫu nhiên (time-varying delay).

## 2. Tương tác Cục bộ (Local Interaction View)
- **Người đảm nhiệm (Assignee):** D.M.Hai K67
- **Hệ tham chiếu (Container):** Hệ thống giám sát và điều khiển lưới điện (Smart Grid Control System).
- **Tương tác với lưới điện (Grid Interaction):**
  - **Làm sai lệch tín hiệu điều khiển:** Dữ liệu đo lường liên tục từ PMU về bộ điều khiển bị trễ, khiến các quyết định điều khiển (tần số, điện áp) được đưa ra dựa trên trạng thái quá khứ của lưới điện, không phản ánh thực tại.
  - **Phá vỡ vòng lặp điều khiển (Control Loops):** Trong các hệ thống đáp ứng nhanh (như Power System Stabilizer - PSS), độ trễ làm giảm biên độ pha (phase margin), gây nguy cơ mất ổn định, dẫn đến dao động tần số diện rộng.
  - **Động lực học tương tác:** Yêu cầu các thuật toán (MPC, PI được tinh chỉnh) phải có khả năng dự đoán trước quỹ đạo hệ thống hoặc bù trễ (delay compensation) để triệt tiêu ảnh hưởng của Communication Delay đến trạng thái vật lý của Grid.

## 3. Điểm mạnh, Điểm yếu & Ứng dụng (Pros, Cons & Use Cases)
- **Điểm yếu cốt lõi:** Suy giảm hiệu suất động học, đe dọa sự ổn định toàn cục của lưới.
- **Môi trường tác động:** Gặp nhiều ở WAMS (Wide-Area Measurement Systems) có độ phủ địa lý lớn, mạng chia sẻ lưu lượng hoặc bị sự cố định tuyến mạng.
- **Cách khắc phục:** Ứng dụng các bộ điều khiển dung sai trễ (Delay-Tolerant Control) hoặc kết hợp với mạng truyền thông ưu tiên băng thông thấp-độ trễ siêu tốc (như 5G URLLC).