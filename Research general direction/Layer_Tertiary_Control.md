---
weighttree: 1
contributors: 
  - Khanh k67
  - Thanh K67
---
Direct Parent Connection: -> [[Real_Time_Balancing]]

# LAYER: TERTIARY CONTROL (TẦNG ĐIỀU KHIỂN TAM CẤP)
*(Trục Thời gian L1: Tầng điều phối chiến lược trong thời gian thực, có nhiệm vụ giải phóng dự phòng cho hệ thống tự động, khung thời gian từ 15 phút đến vài giờ).*

## 1. ĐẶC TÍNH NỘI TẠI (INTRINSIC CHARACTERISTICS)

### 1.1. Bản chất Vật lý của Tầng Điều khiển Tam cấp (Tertiary Control)
- **Định nghĩa Vật lý:** Quá trình tái thiết lập (restore) các dải dự phòng của Tầng điều khiển thứ cấp (Secondary Control / AGC) đã bị tiêu hao sau một sự cố mất cân bằng lớn hoặc do xu hướng tải thay đổi, đồng thời tối ưu hóa lại điểm làm việc của các tổ máy.
- **Trạng thái Mạng điện:** Lưới điện đang ở trạng thái xác lập mới nhưng không tối ưu về mặt kinh tế, hoặc các tổ máy tham gia AGC đang chạy ở giới hạn phát, làm mất khả năng dự phòng bảo vệ hệ thống trước các nhiễu động tiếp theo.
- **Hành động Vật lý:** 
  - Điều chỉnh điểm đặt (setpoints) của các tổ máy phát điện đang vận hành (Spinning Reserve).
  - Khởi động và hòa lưới các tổ máy phản ứng nhanh (Fast-start units) ở trạng thái chờ (Non-spinning Reserve) như thủy điện, tuabin khí chu trình đơn (OCGT), hoặc máy phát diesel.
  - Tái phân bổ trào lưu công suất (Power flow) để giải tỏa ách tắc cục bộ trên các đường dây liên kết.

### 1.2. Bản chất Toán học của Phân bổ Kinh tế Thực thời (Real-Time Economic Dispatch - RTED)
Tập trung giải quyết bài toán quy hoạch toán học tối ưu hóa điểm làm việc của các tổ máy đã được xác định trạng thái On/Off.

**Hàm mục tiêu (Objective Function):** 
Tối thiểu hóa chi phí nhiên liệu vận hành tức thời tại phân đoạn thời gian $t$:
$$ \min F_T = \sum_{i=1}^{N} C_i(P_i(t)) $$
Trong đó, hàm chi phí $C_i(P_i) = a_i + b_i P_i + c_i P_i^2$ mang bản chất tĩnh học ngắn hạn, KHÔNG bao gồm chi phí khấu hao hay đầu tư dài hạn.

**Hệ ràng buộc Cứng (Strict Constraints):**
1. **Nút thắt Năng lượng (Power Balance):**
   $$ \sum_{i=1}^{N} P_i(t) = P_{D}^{forecast}(t) + P_{loss}(t) $$
2. **Ràng buộc Biên kỹ thuật (Generation Limits):**
   $$ P_{i}^{\min} \le P_i(t) \le P_{i}^{\max} $$
3. **Quán tính Cơ - Nhiệt học (Ramp Rate Limits):**
   $$ -DR_i \cdot \Delta t \le P_i(t) - P_i(t-1) \le UR_i \cdot \Delta t $$
4. **Giới hạn An ninh Lưới (Network Limits / DC-OPF):**
   Lưu lượng truyền tải trên nhánh $l$ không vượt qua giới hạn nhiệt $\left| F_l \right| \le F_{l}^{\max}$.

**Bản chất giải tích:** Khi không chạm biên, hệ thống đạt trạng thái tối ưu khi tất cả tổ máy hoạt động ở cùng một **chi phí biên hệ thống (System Marginal Cost - $\lambda$)**: $\frac{dC_1}{dP_1} = \frac{dC_2}{dP_2} = ... = \lambda$.

### 1.3. Cơ chế kích hoạt Dự phòng thay thế (Replacement Reserves - RR)
- **Bản chất Tín hiệu:** Điều khiển rời rạc (Open-loop), gửi bởi EMS chu kỳ 10-15 phút/lần, tách biệt với tín hiệu liên tục của AGC.
- **Ràng buộc Toán học của RR:** Dự phòng thay thế $R_i^{RR}$ cạnh tranh trực tiếp không gian phát với công suất tác dụng:
  $$ P_i(t) + R_i^{RR}(t) \le P_{i}^{\max} $$
  $$ \sum_{i \in \text{Fast Start units}} R_i^{RR}(t) \ge Req^{Tertiary} $$
- **Trình tự Kích hoạt Vật lý:** Khi AGC cạn kiệt dải điều chỉnh, EMS kích hoạt RTED. Các tổ máy RR bơm công suất vào, ép tần số nhích lên, khiến tín hiệu ACE tự động giảm lệnh phát của máy AGC, trả lại dải dự phòng quay.

### 1.4. Ranh giới Không - Thời gian (Space-Time Boundary)
- **Khung thời gian vận hành:** Xét từ **15 phút đến vài giờ**.
- **Đóng băng Tư duy Dài hạn:** Bỏ qua hoàn toàn bài toán CAPEX, cấu trúc lưới điện cố định. Mô hình phụ tải là chuỗi nhiễu động ngẫu nhiên siêu ngắn hạn.

---

## 2. TƯƠNG TÁC CỤC BỘ (LOCAL INTERACTION VIEW)
*(Đóng vai trò điều phối chiến lược bên trong Container: `Real_Time_Balancing`).*

**Định vị:** `Layer_Tertiary_Control` không phản ứng chớp nhoáng với sai lệch tức thời, mà là "lực lượng dự bị" hỗ trợ trực tiếp cho lớp Điều khiển Thứ cấp (AGC) nhằm duy trì tính bền vững của toàn bộ không gian cân bằng.

### 2.1. Tiếp nhận tín hiệu đầu vào từ Container (Input)
- **Giám sát trạng thái dự phòng:** Thu thập tín hiệu về dung lượng **Dự phòng quay (Spinning Reserve)** đang được khai thác bởi AGC.
- **Kích hoạt khi cạn kiệt:** Nhận được tín hiệu cảnh báo từ AGC: *Băng thông dự phòng sắp cạn kiệt* do mất cân bằng kéo dài.

### 2.2. Xử lý logic nội bộ (Internal Processing)
- **Nhận định xu hướng:** Đánh giá sự xê dịch công suất hiện tại là sự thay đổi kéo dài về điểm cân bằng, không còn là nhiễu động ngẫu nhiên.
- **Thuật toán phân bổ (Dispatch Logic):** Tính toán lệnh tái phân bổ, nhắm vào các tổ máy phát chạy nền (Base-load) – có tốc độ chậm nhưng biên độ lớn và tối ưu kinh tế. 

### 2.3. Phát tín hiệu đầu ra trả về Container (Output)
- **Lệnh "Thế chỗ" (Replacement Dispatch):** Xuất lệnh Set-point yêu cầu các máy phát nền nạp thêm công suất vào lưới.
- **Giải phóng Secondary Control:** Áp lực cân bằng được chuyển giao từ AGC sang các máy phát nền. Quá trình này ép tín hiệu sai lệch (ACE) về 0, cho phép AGC giảm dần công suất bơm thêm.
- **Đóng vòng tương tác:** Secondary Control được "Reset" hoàn toàn về vạch xuất phát. Dung lượng điều tần dự phòng quay được giải phóng, sẵn sàng 100% để đón đỡ nhiễu động tiếp theo của Container.