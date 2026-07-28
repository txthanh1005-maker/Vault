# Source of System (L1)

**Direct Parent Connection:** -> [[Power_Grid_System]]

**Mô tả kết nối hướng tâm:** 
Đóng vai trò là gốc rễ năng lượng của toàn bộ mạng lưới. Trong kỷ nguyên hiện đại, việc nghiên cứu Nguồn phát không chỉ dừng lại ở cách tạo ra điện (Physical Energy), mà còn đi sâu vào cách chúng kết nối và giao tiếp với lưới điện thông qua các linh kiện công suất (Grid Integration).

---
## ươm mầm Lớp Level 2 (Bản đồ Phân loại Nguồn phát Toàn diện)

*(Theo quy tắc Hydra: Khi khối lượng nghiên cứu nội tại $W > W_{max}$, các đề mục dưới đây sẽ tự động bóc tách thành các Node file `.md` độc lập có chứa link hướng tâm `[[Source_of_System]]`)*

### PHẦN A: THEO BẢN CHẤT NĂNG LƯỢNG (PHYSICAL SOURCES)
*Săn tìm các đề tài về công nghệ vật liệu, mô hình hóa động học và tối ưu hóa hiệu suất.*

- **1. Năng lượng Tái tạo cốt lõi (Renewable Energy):**
  - *Solar PV:* Thuật toán MPPT dưới bóng râm cục bộ, dự báo chuỗi thời gian bức xạ mặt trời (Forecasting).
  - *Wind Energy:* Khí động học tuabin, điều khiển góc quay cánh quạt (Pitch control), tác động luồng gió thức (Wake effect) trong trang trại gió.
- **2. Nhà máy điện lai (Hybrid Power Plants):**
  - *Wind-Solar Synergy:* Phối hợp gió - mặt trời tại chung điểm đấu nối (PCC) để triệt tiêu tính bất định.
  - *RE-Hydrogen Coupling:* Dùng điện tái tạo dư thừa chạy máy điện phân (Electrolyzer) tạo Hydro xanh tại chỗ.
- **3. Năng lượng Mới & Đột phá (Emerging Clean Sources):**
  - *Hydrogen Fuel Cells:* Dùng Pin nhiên liệu Hydro làm nguồn phát nền tĩnh (Baseload) thay thế Diesel.
  - *Flexible Biomass:* Máy phát sinh khối thay đổi công suất nhanh (Fast Ramping) để bù đắp hụt điện mặt trời.
  - *Ocean Energy:* Năng lượng sóng biển/thủy triều cấp điện cho Microgrid hải đảo.

### PHẦN B: THEO CÔNG NGHỆ KẾT NỐI & ĐIỀU KHIỂN LƯỚI (GRID INTEGRATION)
*Săn tìm các đề tài về thiết kế mạch, điều khiển biến tần và duy trì ổn định hệ thống điện.*

- **4. Nguồn phát qua Biến tần (Inverter-Based Resources - IBRs):**
  - *Grid-Following (GFL - Bám lưới):* Hành xử như nguồn dòng (Current Source), chạy vòng lặp PLL. Gặp bất ổn định trên lưới điện yếu.
  - *Grid-Forming (GFM - Tạo lưới):* Hành xử như nguồn áp (Voltage Source). Tự thiết lập tần số/điện áp, cung cấp Quán tính ảo (Virtual Inertia). Ứng dụng kỹ thuật: VSG (Virtual Synchronous Generator), VOC.
- **5. Nguồn Phân tán & Lưu trữ Bơm ngược (Active DERs):**
  - *V2G (Vehicle-to-Grid):* Xe điện hoạt động như nguồn phát di động (Mobile DERs) bơm ngược điện lên lưới cạo đỉnh.
  - *BESS Discharging:* Hệ thống lưu trữ khi xả điện (kết hợp GFM) đóng vai trò là nguồn phát khẩn cấp duy trì Microgrid.
- **6. Máy phát Cơ khí Kỷ nguyên mới (Mechanical Resilience):**
  - *Synchronous Condensers (Máy bù đồng bộ):* Máy phát cũ tháo tuabin, chỉ giữ Rotor chạy không tải. Không phát công suất nhưng cung cấp **Quán tính vật lý tuyệt đối (Physical Inertia)** và dòng ngắn mạch khổng lồ để cứu lưới.
