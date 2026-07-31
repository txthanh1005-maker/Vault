---
contributors: 
  - Khanh k67
  - Thanh K67
---
Direct Parent Connection: -> [[Power_System_Scheduling]]

# LAYER: PRIMARY CONTROL (TẦNG ĐIỀU KHIỂN SƠ CẤP)
*(Trục Thời gian L3: Khung thời gian vật lý tức thời, nơi các định luật Newton và phương trình động lực học bảo vệ sinh mệnh của Lưới điện khỏi sự sụp đổ).*

## 1. ĐẶC TÍNH NỘI TẠI (PURE ENTITY VIEW)
*(Bản chất vật lý của Quán tính đồng bộ và phương trình động lực học Rotor)*

### 1.1. Bản chất vật lý của Quán tính đồng bộ (Synchronous Inertia)
- **Lưu trữ năng lượng động học (Kinetic Energy Storage):** Quán tính đồng bộ bắt nguồn từ khối lượng cơ học của các rotor máy phát đồng bộ và turbine quay cùng tốc độ đồng bộ. Năng lượng động học nội tại được lưu trữ: $E_k = \frac{1}{2}J\omega_m^2$.
- **Hằng số quán tính ($H$):** Được chuẩn hóa thành hằng số $H$ (đơn vị: giây), đại diện cho thời gian hệ thống quay có thể cung cấp công suất định mức $S_{rated}$ hoàn toàn từ động năng dự trữ: $H = \frac{J \omega_{sm}^2}{2 S_{rated}}$.
- **Phản ứng tức thời, không trễ (Instantaneous & Delay-free Response):** Sự phản kháng tự nhiên của vật chất chống lại sự thay đổi trạng thái chuyển động. Hấp thụ công suất bơm vào lưới một cách tức thời, hoàn toàn không phụ thuộc vào vòng lặp đo lường hay điều khiển.

### 1.2. Phương trình động lực học Rotor (Swing Equation)
- **Mô hình cốt lõi:** Phương trình vi phân bậc hai biểu diễn sự tương tác giữa cơ năng và điện năng: $J \frac{d\omega_m}{dt} = T_m - T_e$.
- **Phương trình hệ đơn vị tương đối (pu) theo Góc công suất:**
  $$\frac{2H}{\omega_s} \frac{d^2\delta}{dt^2} + D\frac{d\delta}{dt} = P_m - P_e$$
  - $\frac{2H}{\omega_s} \frac{d^2\delta}{dt^2}$: Gia tốc của rotor (RoCoF). Khối lượng $H$ càng lớn, biên độ dao động càng bị kìm hãm.
  - $D\frac{d\delta}{dt}$ (Thành phần cản nội tại - Damping): Hấp thụ năng lượng dao động.

### 1.3. Bản chất toán học của Hàm truyền Droop Control (Độ võng tĩnh)
- **Bản chất hàm truyền P (Proportional Control):** Mối quan hệ đại số tuyến tính nghịch biến giữa sai lệch tần số ($\Delta f$) và lượng công suất điều chỉnh ($\Delta P$):
  $$\Delta P = -\frac{1}{R} \Delta f$$
  Trong đó $R$ là hệ số độ võng (Droop setting).
- **Sai số xác lập tĩnh (Steady-State Error):** Đặc tính toán học của khâu Tỷ lệ (P) là bắt buộc phải tồn tại sai số tĩnh. Tần số sẽ hội tụ tại một giá trị xác lập mới:
  $$\Delta f_{ss} = \frac{-\Delta P_{load}}{D + \sum \frac{1}{R_i}}$$
- **Tính chất phân tán (Decentralization):** Biến đầu vào duy nhất $\Delta f$ là một biến số toàn cục đo lường cục bộ, không cần hệ thống truyền tin.

---

## 2. TƯƠNG TÁC CỤC BỘ (LOCAL INTERACTION VIEW)
*(Cách Nút Primary tương tác với Container: `Power_System_Scheduling`).*

### 2.1. Luồng tín hiệu đầu vào (Nhận từ Container / Cấp trên)
- **Nguồn gốc:** Nhận lệnh trực tiếp từ `Layer_Secondary_Control` (hoặc chức năng AGC trong phạm vi Scheduling).
- **Nội dung tín hiệu:** Lệnh giá trị đặt (Set-points) về công suất phát cơ sở và tần số tham chiếu danh định.
- **Đặc điểm tương tác:** `Layer_Primary_Control` tiếp nhận Set-points như một điểm neo tĩnh. Bộ điều khiển sơ cấp không có quyền thay đổi Set-points mà chỉ giám sát độ lệch tần số cục bộ để bơm/giảm công suất trên nền của Set-point này.

### 2.2. Luồng tín hiệu đầu ra (Gửi ngược lên Container)
- **Chức năng cốt lõi:** Phản ứng tự động để hãm đà suy giảm tần số (ngăn rớt nadir) ngay khi mất cân bằng.
- **Đặc tính đáp ứng:** Hoạt động trong dải mili-giây (Inertia) đến vài giây (Droop control).
- **Kết quả đầu ra cục bộ:** Lượng công suất bù tức thời $\Delta P$ được đưa ngược trở lại Container `Power_System_Scheduling` như một thông số trạng thái mới. Hệ thống đạt điểm cân bằng mới nhưng vẫn tồn tại sai số tĩnh, báo hiệu cho Secondary Control chuẩn bị can thiệp.

### 2.3. Ràng buộc nội bộ (Constraints & Limits)
- **Giới hạn dự phòng sơ cấp:** Biên độ $\Delta P$ bị giới hạn cứng bởi dung lượng dự phòng sơ cấp (Primary Reserve) mà Scheduling đã phân bổ. Khi đạt giới hạn, bộ điều khiển bão hòa.
- **Tính thiển cận có chủ đích:** Nút này được thiết kế để "không quan tâm" triệt tiêu hoàn toàn sai lệch, chỉ tập trung 100% vào tốc độ đáp ứng để ổn định trạng thái cục bộ nhanh nhất.