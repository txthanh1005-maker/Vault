# Load (L1)

**Direct Parent Connection:** -> [[Power_Grid_System]]

**Mô tả kết nối hướng tâm:** 
Đóng vai trò là điểm tiêu thụ năng lượng cuối cùng (Demand). Trong kỷ nguyên hiện đại, Nút Phụ tải không chỉ được nghiên cứu dưới góc độ "Tương tác với Lưới điện" (Demand Response, V2G) mà còn được bóc tách sâu vào cấp độ "Nhiệt động lực học nội tại" của từng thiết bị để tối ưu hóa hiệu suất từ lõi.

---
## ươm mầm Lớp Level 2 (Bản đồ Phân loại Phụ tải Toàn diện)

*(Theo quy tắc Hydra: Khi khối lượng nghiên cứu nội tại $W > W_{max}$, các đề mục dưới đây sẽ tự động bóc tách thành các Node file `.md` độc lập có chứa link hướng tâm `[[Load]]`)*

### PHẦN A: GÓC NHÌN ĐIỀU KHIỂN & TƯƠNG TÁC LƯỚI (GRID-LEVEL INTERACTION)
*Các mảng nghiên cứu khai thác Phụ tải để hỗ trợ ổn định hệ thống điện.*

- **1. Sự linh hoạt & Đáp ứng nhu cầu (Load Flexibility & DR):**
  - *Implicit DR (Theo Giá):* Dịch chuyển tải theo TOU, Real-Time Pricing.
  - *Explicit DR (Theo Động lực):* Điều độ trực tiếp (Direct Load Control) hoặc tải tự chào thầu dịch vụ phụ trợ.
- **2. Sự trỗi dậy của Phụ tải tích cực (Active Loads & EVs):**
  - *Mạng lưới V2G/V1G:* EV làm trạm lưu trữ di động cạo đỉnh (Peak Shaving) và điều tần.
  - *Prosumers & VPP:* Phụ tải tự phát điện (Mặt trời áp mái), gom nhóm thành Nhà máy điện ảo (Virtual Power Plant) giao dịch P2P.
- **3. Phân cấp Sinh tồn (Resilience & Load Shedding):**
  - *Tier 1 (Critical):* Bệnh viện, Quân sự (Bắt buộc giữ điện).
  - *Tier 2/Tier 3:* Công nghiệp liên tục / Phụ tải dân dụng (Bị cắt đầu tiên khi sụt tần số).
- **4. Mô hình hóa Vĩ mô (Macro Modeling):** Chuyển từ mô hình tĩnh ZIP sang mô hình phức hợp (CMLD) mang tính ngẫu nhiên (Stochastic) do tích hợp EV.

### PHẦN B: GÓC NHÌN VẬT LÝ & NHIỆT ĐỘNG LỰC HỌC NỘI TẠI (PURE ENTITY DYNAMICS)
*Nghiên cứu giới hạn vật lý độc lập bên trong thiết bị, tuyệt đối không lấn sân sang các bài toán lưới điện.*

- **5. Trạm sạc Xe điện (Power Electronics Limits):**
  - *Đặc tính:* Nhiệt độ tiếp diện ($T_j$) dao động mạnh gây nứt gãy lớp hàn bán dẫn (SiC MOSFET). Dòng gợn sóng (Ripple current) châm ngòi hiện tượng mạ Lithium cục bộ phá hủy pin.
  - *Hướng Báo cáo (Research Gap):* Đề xuất thuật toán điều chế (SVPWM) bất đối xứng để tự lừa dòng điện chạy vòng sang các van còn khỏe, tản nhiệt chủ động (Active Thermal Routing) giữ trạm sạc sống sót mà không thay phần cứng.
- **6. Hệ thống Điều hòa Trung tâm (HVAC - Fluid-Mechanics):**
  - *Đặc tính:* Giới hạn khí động học chòng chành/nghẽn (Surge/Choke). Bệnh "Thiếu dầu bôi trơn" (Oil Starvation) gây mài mòn máy nén vĩnh viễn khi chạy biến tần siêu thấp.
  - *Hướng Báo cáo (Research Gap):* Xây dựng mô hình Cơ-lưu chất và giải thuật "Bơm xung dầu" (Oil Return Pulse). Kích xung tốc độ cao trong vài giây để hút dầu về mà không phá vỡ tính ổn định nhiệt độ phòng.
- **7. Trung tâm Dữ liệu (Data Centers - Spatiotemporal Thermal):**
  - *Đặc tính:* Nhiệt độ rò rỉ tăng theo hàm mũ ($P_{leak} \propto e^{T_j}$). Các điểm đen khí động học (Bypass airflow) gây cháy vi mạch cục bộ.
  - *Hướng Báo cáo (Research Gap):* Lập lịch tác vụ theo không gian 3D (Thermal-aware Workload Allocation). Điều hướng lệnh tính toán nặng sang các CPU nằm ở luồng khí lạnh nhất để triệt tiêu hiệu ứng cộng hưởng nhiệt.
- **8. Hướng nghiên cứu chéo (Cross-Entity Ripple Mitigation):**
  - *Hướng Báo cáo (Research Gap):* Kỹ thuật điều chế tần số chuyển mạch động (Dynamic Switching Frequency Modulation) để khử dòng gợn (Active Ripple Compensation), bảo vệ tuổi thọ Tụ điện DC-link trong mọi loại biến tần phụ tải.
