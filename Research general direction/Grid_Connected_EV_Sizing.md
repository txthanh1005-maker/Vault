---
contributors: 
  - Khanh k67
---
Direct Parent Connection: -> [[EV_Sizing]]

# GRID-CONNECTED EV SIZING (ĐỊNH CỠ TRẠM SẠC NỐI LƯỚI)

## 1. Đặc tính Nội tại (Phần Âm - Nội sinh)
Hệ thống Grid-Connected EV Sizing (định cỡ trạm sạc nối lưới) tập trung chủ yếu vào việc xác định và tối ưu hóa hai thành phần chính:
- **Trụ sạc (Charging Stations):** Quyết định công suất (kW) và số lượng trụ sạc để đáp ứng nhu cầu sạc (Charging Demand) của xe điện.
- **Hệ thống lưu trữ năng lượng (BESS - Battery Energy Storage System):** Đóng vai trò tùy chọn cốt lõi để "gọt đỉnh" (Peak Shaving). BESS tích trữ năng lượng vào giờ thấp điểm (giá điện thấp) và xả ra vào giờ cao điểm (giá điện cao), qua đó tối ưu hóa chi phí vận hành (OPEX) dựa trên chênh lệch giá điện.

## 2. Tương tác Cục bộ (Phần Dương - Giao tiếp với Container)
- **Container (Môi trường chứa đựng):** Điểm đấu nối PCC (Point of Common Coupling) giao tiếp trực tiếp với lưới điện phân phối.
- **Ranh giới tĩnh (Constraint):** Sức chứa của lưới điện (Hosting Capacity) tại vị trí đấu nối.
- **Động học tương tác:** Nếu việc định cỡ (sizing) tổng công suất quá lớn, vượt qua sức chứa của lưới điện, hệ thống sẽ gây ra các tác động tiêu cực cục bộ đến Container:
  - **Sụt áp (Voltage Drop):** Điện áp tại PCC sụt giảm xuống dưới mức tiêu chuẩn.
  - **Quá tải nhiệt (Thermal Overloading):** Gây ra quá dòng, làm nóng và có nguy cơ gây hỏng hóc cho cáp ngầm hoặc máy biến áp phân phối cục bộ.

## 3. Điểm mạnh, Điểm yếu và Ứng dụng
- **Điểm mạnh (Strengths):**
  - Tối ưu hóa được chi phí vận hành (OPEX) nhờ chiến lược Peak Shaving và biểu giá chênh lệch thời gian (Time-of-Use pricing).
  - Cấu trúc hệ thống đơn giản hơn so với mô hình phức hợp có nguồn phát tái tạo (như PV), giúp dễ dàng tích hợp tại các khu vực đô thị.
- **Điểm yếu (Weaknesses):**
  - Phụ thuộc hoàn toàn vào giới hạn Hosting Capacity của lưới điện địa phương. Tại các khu vực có lưới điện yếu, sẽ đòi hỏi chi phí nâng cấp hạ tầng đáng kể.
  - Chi phí đầu tư ban đầu (CAPEX) cho BESS có thể rất cao.
- **Ứng dụng thực tiễn (Applications):**
  - **Trạm sạc nhanh công cộng (DC Fast Charging):** Đặt tại các trung tâm thương mại hoặc khu đô thị nơi quỹ đất hẹp, không thể lắp đặt năng lượng mặt trời.
  - **Trạm sạc kết hợp BESS tại văn phòng/chung cư:** Nhằm tránh tiền phạt công suất cực đại (Demand Charge) và tận dụng giá điện giờ thấp điểm.