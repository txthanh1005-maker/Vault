---
weighttree: 1
contributors: 
  - Khanh k67
---
Direct Parent Connection: -> [[EV_Sizing]]

# STANDALONE EV SIZING (ĐỊNH CỠ TRẠM SẠC ĐỘC LẬP / OFF-GRID)

## 1. Đặc tính Nội tại (Phần Âm - Khối lượng, Năng lượng, Cấu trúc)
- **Bản chất hệ thống:** Là trạm sạc xe điện hoàn toàn cách ly khỏi lưới điện quốc gia (Off-grid). Do không có nguồn điện dự phòng từ lưới, hệ thống bắt buộc phải tích hợp **Nguồn năng lượng tái tạo (RE)** (thường là điện mặt trời PV và/hoặc turbine gió) cùng **Hệ thống lưu trữ năng lượng (BESS)**.
- **Biến số cốt lõi:**
  - Quy mô mảng PV / Turbine gió (kW/MW).
  - Dung lượng và công suất xả của BESS (kWh/MWh, kW).
  - Phụ tải trạm sạc (EV charging demand).
- **Mâu thuẫn nội tại (Trade-off chính):** Bài toán thiết kế phải giải quyết sự đánh đổi khốc liệt giữa:
  - **CAPEX khổng lồ:** Chi phí đầu tư ban đầu cho BESS và hệ thống nguồn RE đủ lớn để duy trì hoạt động liên tục là rất cao.
  - **Xác suất mất điện (LPSP - Loss of Power Supply Probability):** Để LPSP đạt chuẩn (độ tin cậy cao, thường < 1-5%), dung lượng lưu trữ và nguồn phát phải được tính toán dư thừa (oversizing), kéo theo CAPEX tăng vọt.
- **Cơ chế vận hành:** Các thành phần cân bằng dòng năng lượng tức thời thông qua bộ điều khiển vi lưới tự trị (Autonomous Microgrid Controller), ưu tiên sạc BESS khi dư thừa năng lượng và xả BESS để bù đắp khi RE thiếu hụt.

## 2. Tương tác Cục bộ (Phần Dương - Vai trò, Vị trí trong môi trường)
- **Container (Môi trường chứa đựng):** Hệ thống không gắn với lưới điện, do đó "Container" ở đây chính là **Môi trường địa lý độc lập** (vùng sâu vùng xa, hải đảo, trạm nghỉ đường cao tốc hoang mạc).
- **Ranh giới tương tác:**
  - **Không có Hosting Capacity:** Hệ thống hoàn toàn không bị ràng buộc bởi các vấn đề kỹ thuật của lưới điện phân phối (không lo sụt áp, quá tải nhánh, hay giới hạn công suất đẩy ngược).
  - **Tương tác Đầu vào (Khí tượng):** Bị chi phối thuần túy bởi tính ngẫu nhiên của thời tiết (bức xạ mặt trời, tốc độ gió, nhiệt độ môi trường ảnh hưởng đến hiệu suất pin).
  - **Tương tác Đầu ra (Nhu cầu người dùng):** Đáp ứng nhu cầu sạc xe điện ngẫu nhiên theo hành vi di chuyển tại vị trí địa lý đó.
- **Tín hiệu tương tác:** Trạng thái hệ thống (đặc biệt là dung lượng pin - SOC của BESS) dao động theo nhịp điệu của thiên nhiên và hành vi con người, đòi hỏi các chiến lược điều khiển năng lượng (EMS) dự báo được chu kỳ ngày/đêm và thời tiết.

## 3. Phân tích Điểm mạnh, Điểm yếu & Ứng dụng
- **Điểm mạnh (Strengths):**
  - Tự chủ năng lượng 100%, không bị gián đoạn bởi các sự cố rã lưới hoặc cắt điện diện rộng.
  - Năng lượng xanh hoàn toàn (Zero-emission), thân thiện với môi trường.
  - Triển khai linh hoạt ở những nơi có địa hình hiểm trở mà việc kéo đường dây truyền tải tốn kém hoặc bất khả thi.
- **Điểm yếu (Weaknesses):**
  - Chi phí đầu tư ban đầu (CAPEX) cực kỳ đắt đỏ.
  - Chi phí vòng đời (OPEX) cao do chu kỳ suy thoái (degradation) và thay thế của BESS.
  - Độ tin cậy (LPSP) nhạy cảm và dễ tổn thương trước các chuỗi ngày thời tiết cực đoan (ví dụ: mây mù/mưa kéo dài).
  - Đòi hỏi diện tích quỹ đất lớn để lắp đặt tấm pin mặt trời hoặc tua-bin gió.
- **Ứng dụng thực tiễn (Applications):**
  - Các trạm nghỉ dọc các tuyến cao tốc xuyên quốc gia/hoang mạc cách xa lưới điện.
  - Hạ tầng sạc xe điện cho các khu du lịch sinh thái, resort, đảo ngọc yêu cầu tĩnh lặng và không phát thải.
  - Các căn cứ quân sự độc lập hoặc trạm ứng phó thảm họa di động.