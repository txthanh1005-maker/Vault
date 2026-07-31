---
contributors: 
  - D.M.Hai K67
---
Direct Parent Connection: -> [[Communication]]

# Signal Detection
*(Thuật toán phát hiện sự cố mất tín hiệu hoặc trễ truyền tải trong hệ thống truyền thông lưới điện)*

## 1. Đặc tính Nội tại (Pure Entity View)
- **Người đảm nhiệm (Assignee):** D.M.Hai K67
- Cấu trúc và Thuật toán: Dựa vào giám sát Timestamp, Sequence Number của gói tin (Packet) để phát hiện trễ (delay) hoặc mất gói (packet loss). Các thuật toán nâng cao sử dụng Lọc Kalman (Kalman Filter), học máy (Machine Learning) hoặc phân tích chuỗi thời gian để phát hiện bất thường trong dữ liệu thời gian thực.
- Phương trình toán học chi phối: Sai số dự đoán tín hiệu (Prediction error), phân tích phương sai, và các ngưỡng (threshold) thống kê.
- Tính chất: Độ chính xác và tốc độ phát hiện, khả năng phân biệt giữa biến động tín hiệu do hệ thống vật lý và biến động do lỗi truyền thông.

## 2. Tương tác Cục bộ (Local Interaction View)
- **Người đảm nhiệm (Assignee):** D.M.Hai K67
- **Hệ tham chiếu (Container):** Communication
- Tương tác đầu vào: Nhận dữ liệu truyền từ các thiết bị đo (như PMU) chứa các đặc trưng mạng.
- Tương tác đầu ra: Cung cấp cờ cảnh báo (Flags) cho module Signal Recovery, kích hoạt cơ chế xác thực của [[Blockchain]], hoặc báo cho Control Center về tình trạng độ trễ / mất mát.
- Động lực học tương tác: Nếu tốc độ nhận diện quá chậm, độ trễ sẽ lan truyền trực tiếp vào vòng điều khiển, có thể dẫn đến mất ổn định. Nó đóng vai trò "cảm biến" về chất lượng đường truyền, đồng thời đóng vai trò là chốt chặn đầu tiên để phát hiện dữ liệu độc hại trước khi đưa vào sổ cái Blockchain.

## 3. Điểm mạnh, Điểm yếu & Ứng dụng (Pros, Cons & Use Cases)
- **Điểm mạnh (Strengths):** Giúp hệ thống sớm nhận biết lỗi mạng trước khi bộ điều khiển đưa ra quyết định sai lầm.
- **Điểm yếu (Weaknesses):** Có thể xảy ra cảnh báo giả (False Positive) nếu nhiễu tín hiệu quá cao hoặc ngưỡng chọn không hợp lý.
- **Khi nào dùng (When to use):** 
  - Ứng dụng trong WAMC hoặc các hệ thống điều khiển kín (Closed-loop) yêu cầu phản hồi nhanh, chính xác.
  - Kết hợp làm tín hiệu Trigger (Kích hoạt) cho các Smart Contract của [[Blockchain]] để tự động cách ly các gói tin bất thường.
- **Khi nào KHÔNG dùng (When NOT to use):** Các hệ thống giám sát chậm offline không yêu cầu thời gian thực.