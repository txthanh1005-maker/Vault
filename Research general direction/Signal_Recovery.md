---
weighttree: 1
contributors: 
  - D.M.Hai K67
---
Direct Parent Connection: -> [[Communication]]

# Signal Recovery
*(Cơ chế khôi phục, bù trừ tín hiệu bị mất hoặc trễ nhằm duy trì độ tin cậy của thông tin đo lường)*

## 1. Đặc tính Nội tại (Pure Entity View)
- **Người đảm nhiệm (Assignee):** D.M.Hai K67
- Cấu trúc và Thuật toán: Sử dụng các phương pháp nội suy (Interpolation - tuyến tính, spline), ngoại suy (Extrapolation), và dự báo (Prediction) bằng Mạng nơ-ron nhân tạo (ANN), Lọc Kalman để tái tạo các mẫu dữ liệu bị mất (packet loss) hoặc bù đắp cho sự chậm trễ (delay compensation).
- Phương trình chi phối: Các phương trình hàm truyền, dự báo chuỗi thời gian, và cập nhật trạng thái ước lượng.
- Suy hao nội tại: Dữ liệu khôi phục luôn có sai số (estimation error) so với giá trị thực tế. Sai số này sẽ tỷ lệ thuận với độ dài của khoảng thời gian mất tín hiệu.

## 2. Tương tác Cục bộ (Local Interaction View)
- **Người đảm nhiệm (Assignee):** D.M.Hai K67
- **Hệ tham chiếu (Container):** Communication
- Tương tác đầu vào: Được kích hoạt bởi tín hiệu cảnh báo từ module Signal Detection.
- Tương tác đầu ra: Phục hồi sự trọn vẹn và độ chính xác của tín hiệu từ PMU, sau đó cung cấp tín hiệu "sạch" cho các vòng điều khiển ([[PI_Control]], [[MPC]]).
- Động lực học tương tác: Việc khôi phục kịp thời là điều kiện sống còn của hệ thống điều khiển:
  - **Đối với [[PI_Control]]:** Giúp khâu tích phân (Integral) không bị cộng dồn sai số dị thường khi tín hiệu bị đứt quãng.
  - **Đối với [[MPC]]:** Giúp ma trận dự báo tương lai của MPC không bị phá sản. Nếu dữ liệu bị hổng, hàm mục tiêu của MPC sẽ giải ra nghiệm sai. Do đó, Signal Recovery đóng vai trò nền tảng để MPC hoạt động ổn định.

## 3. Điểm mạnh, Điểm yếu & Ứng dụng (Pros, Cons & Use Cases)
- **Điểm mạnh (Strengths):** Khắc phục được nhược điểm của mạng truyền thông kém chất lượng, bảo toàn hiệu năng của vòng điều khiển.
- **Điểm yếu (Weaknesses):** Nếu mất tín hiệu quá lâu (burst loss), dữ liệu tái tạo có thể hoàn toàn sai lệch, khiến bộ điều khiển hành động sai lầm nghiêm trọng hơn.
- **Khi nào dùng (When to use):** Dùng cho các hệ thống WAMC có đường truyền dễ bị nhiễu, packet loss, kết hợp với các bộ điều khiển cao cấp như [[PI_Control]], [[MPC]] yêu cầu dữ liệu liên tục.
- **Khi nào KHÔNG dùng (When NOT to use):** Khi hệ thống trải qua sự cố mất truyền thông diện rộng (Blackout truyền thông); lúc này nên chuyển hướng sang cơ chế điều khiển cục bộ (Local Control).