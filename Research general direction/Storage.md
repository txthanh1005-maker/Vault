# Storage (L1)

**Direct Parent Connection:** -> [[Power_Grid_System]]

**Mô tả kết nối hướng tâm:** 
Trong cấu trúc này, Nút Lưu trữ (Storage) tập trung 100% vào **Hệ thống Lưu trữ Năng lượng Pin (BESS)**. BESS được bóc tách sâu sắc theo tư duy "Thái Cực" (Duality), loại bỏ hoàn toàn các hướng dẫn viết bài báo rườm rà, chỉ giữ lại sự tinh khiết của **Phân loại** và **Đặc tính cốt lõi**.

---
## ươm mầm Lớp Level 2 (Bản đồ Phân loại & Đặc tính BESS)

*(Theo quy tắc Hydra: Khi khối lượng nghiên cứu nội tại $W > W_{max}$, các đề mục dưới đây sẽ tự động bóc tách thành các Node file `.md` độc lập có chứa link hướng tâm `[[Storage]]`)*

### PHẦN A: THEO GÓC NHÌN LƯỚI ĐIỆN & TƯƠNG TÁC (GRID-LEVEL INTERACTION)
*Xem BESS như một "Hộp đen" tương tác với Hệ thống Điện.*

**1. Phân loại Vai trò Lưới điện (Grid Role Classification):**
- *Energy Shifting (Dịch chuyển Năng lượng):* Tích trữ giờ thấp điểm, xả giờ cao điểm để san phẳng biểu đồ tải (Peak Shaving) và kinh doanh chênh lệch giá (Energy Arbitrage).
- *Ancillary Services (Dịch vụ Phụ trợ):* Tham gia điều tần (Primary/Secondary Frequency Regulation) và hỗ trợ điện áp cục bộ (Voltage Support).
- *Grid Resilience (Tăng cường Sinh tồn):* Hoạt động như nguồn áp mồi trong kịch bản Khởi động đen (Black-start) và duy trì đảo lưới (Islanding).

**2. Đặc tính Tương tác (Interaction Characteristics):**
- *Khả năng 4 góc phần tư (4-Quadrant Operation):* Inverter của BESS có thể điều khiển độc lập việc bơm/rút Công suất tác dụng (P) và Công suất phản kháng (Q) ở cả 4 chiều.
- *Tốc độ đáp ứng (Response Time):* Khả năng đảo chiều công suất từ sạc sang xả (hoặc ngược lại) siêu tốc (dưới 20ms), vượt xa giới hạn động học của máy phát cơ khí.
- *Quán tính ảo (Virtual Inertia):* Bằng thuật toán Grid-forming, BESS bẻ cong đáp ứng tức thời để bắt chước động năng quay của tuabin cơ khí.

---

### PHẦN B: THEO GÓC NHÌN VẬT LÝ & NỘI TẠI (PURE ENTITY DYNAMICS)
*Phá vỡ Hộp đen, đi sâu vào cấu trúc vật lý nội bộ của BESS.*

**3. Phân loại Cấu trúc Nội bộ (Internal Topology Classification):**
- *Công nghệ Hóa chất (Cell Chemistry):* Phân rã thành LFP (An toàn, tuổi thọ cao), NMC (Mật độ năng lượng cao), và LTO (Sạc siêu tốc, chịu lạnh tốt).
- *Cấu trúc Tản nhiệt (Cooling Systems):* Hệ thống làm mát bằng gió (Air cooling - rẻ, rủi ro nhiệt cao), bằng chất lỏng (Liquid cooling - tản nhiệt đều), và Vật liệu chuyển pha (Phase Change Material).
- *Topology Biến tần (Inverter Topology):* Biến tần trung tâm (Centralized) vs Biến tần chuỗi/mô-đun đa bậc (String / MMC).

**4. Đặc tính Vật lý & Nhiệt động lực học (Physical Characteristics):**
- *Ranh giới Điện hóa:* Bị chi phối bởi Định luật Fick (Khuếch tán ion) và phương trình Butler-Volmer. Hiện tượng trễ (Hysteresis) giữa điện áp mạch hở (OCV) lúc sạc và xả.
- *Ranh giới Nhiệt động:* Tổn hao Joule và sự thay đổi Entropy tạo ra gradient nhiệt cục bộ bên trong cell, gây ứng suất cơ học nứt vỡ vật liệu.
- *Cơ chế Lão hóa (Degradation Mechanism):* 
  - *Cyclic Aging:* Tuổi thọ chu kỳ giảm nhanh do phát triển màng SEI, nứt vi mô, và mạ Lithium (Lithium Plating) khi ép sạc ở C-rate cao.
  - *Calendar Aging:* Suy hao tự nhiên theo thời gian, tuân theo định luật nhiệt động Arrhenius.
