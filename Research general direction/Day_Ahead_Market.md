---
weighttree: 1
contributors: 
  - Khanh k67
  - Thanh K67
---
Direct Parent Connection: -> [[Power_System_Scheduling]]

# DAY-AHEAD MARKET (THỊ TRƯỜNG NGÀY TỚI)
*(Lớp Trung tâm của Trục Thời gian: Trung tâm giải thuật và kinh tế, quyết định trạng thái ON/OFF của máy phát và thiết lập Giá thanh toán bù trừ LMP).*

## 1. ĐẶC TÍNH NỘI TẠI (PURE ENTITY VIEW)
*(Bản chất Toán học và Vật lý nội tại của Day-Ahead Market)*

Về mặt bản chất cốt lõi, Day-Ahead Market (DAM) không hoạt động như một "thị trường" tài chính hay hàng hóa đàm phán tự do thông thường. Bản chất của DAM là **một bộ giải thuật toán quy hoạch toán học khổng lồ (Mathematical Programming Solver)** được thiết kế để ánh xạ các định luật vật lý của lưới điện (Định luật Kirchhoff, giới hạn nhiệt động lực học) vào một hàm mục tiêu kinh tế.

Dưới góc độ thuần vật lý và toán học, DAM là bài toán **Trào lưu công suất tối ưu (Optimal Power Flow - OPF)** kết hợp với **Cam kết tổ máy (**[[Unit_Commitment]]:**)**. 

### 1.1. Bài toán Tối ưu hóa Tổng thặng dư xã hội (Social Welfare Maximization)
Hàm mục tiêu của DAM là tối đa hóa Tổng thặng dư xã hội ($SW$):
$$ \max SW = \sum_{t=1}^{T} \left( \sum_{j \in D} C_{D,j,t}(P_{D,j,t}) - \sum_{i \in G} C_{G,i,t}(P_{G,i,t}) \right) $$
**Hệ phương trình Ràng buộc Vật lý (Physical Constraints):**
1. **Cân bằng nút (Kirchhoff 1):** $\sum_{i \in G_k} P_{G,i,t} - \sum_{j \in D_k} P_{D,j,t} = \sum_{l \in \Omega_k} F_{k,l,t} \quad (\lambda_{k,t})$
2. **Giới hạn truyền tải (Thermal/Stability):** $-F_l^{\max} \leq \frac{\theta_k - \theta_m}{X_l} \leq F_l^{\max} \quad (\nu_{l,t}^{\min}, \nu_{l,t}^{\max})$
3. **Chuyển động cơ học:** Giới hạn tốc độ thay đổi công suất (Ramping) và thời gian Min Up/Down Time.

### 1.2. Bản chất Cơ chế Khớp lệnh (Market Clearing Algorithm)
Cơ chế "khớp lệnh" là **quá trình hội tụ của thuật toán** (sử dụng Simplex, Interior Point, Branch-and-Bound) để tìm ra vector tối ưu $P^*$ thỏa mãn điều kiện KKT. Nó sinh ra một ma trận trạng thái vật lý duy nhất chỉ định máy phát điện nào quay ở tốc độ bao nhiêu (MW).

### 1.3. Định giá năng lượng ranh giới (Locational Marginal Pricing - LMP)
LMP là đạo hàm bậc nhất (biến đối ngẫu - Shadow Price) của hàm mục tiêu theo phương trình Kirchhoff.
$$ LMP_k = \lambda_{system} + LMP_{loss,k} + LMP_{cong,k} $$
- **Năng lượng cốt lõi ($\lambda_{system}$):** Giá trị cơ bản để tạo 1 MW tại Nút tham chiếu.
- **Tổn thất biên ($LMP_{loss,k}$):** Phản ánh tỏa nhiệt Joule $I^2R$.
- **Nghẽn mạch biên ($LMP_{cong,k}$):** Sự trừng phạt chi phí nội tại khi đường dây đạt giới hạn nhiệt ($F_l = F_l^{\max}$), buộc phải chạy máy đắt tiền hơn ở cục bộ.

---

## 2. TƯƠNG TÁC CỤC BỘ (LOCAL INTERACTION VIEW)
*(Cách DAM đóng vai trò "Bộ Giảm Chấn Thông Tin" bên trong Container: `Power_System_Scheduling`).*

Nằm trong không gian của `Power_System_Scheduling`, `Day_Ahead_Market` hoạt động như một "trung tâm giải thuật và kinh tế". Nút này không trực tiếp thao tác đóng/cắt vật lý, mà xử lý các luồng dữ liệu để tạo ra không gian trạng thái bản lề.

### 2.1. Nhận ràng buộc từ cấp trên (Top-Down từ `Layer_Tertiary_Control`)
Tiếp nhận các ràng buộc cứng (boundary constraints):
- **Ngân sách năng lượng:** Giá trị nước (Water Value) và hạn mức nhiên liệu (Fuel Quotas).
- **Quy mô dự phòng (Reserve Requirements):** Chỉ tiêu tổng dung lượng dự phòng công suất để giải bài toán Co-optimization.
- **Lịch bảo dưỡng (Maintenance Outages):** Trạng thái "Out of Service" bất biến.

### 2.2. Truyền tín hiệu điều khiển xuống cấp dưới (Bottom-Up tới `Layer_Secondary_Control`)
Thiết lập "trục cơ sở" (Baseline) cho Secondary/Real-time Dispatch qua các tín hiệu:
- **Lịch chạy máy chốt 24h (24h Unit Commitment Schedule):** Đẩy xuống ma trận trạng thái On/Off từng chu kỳ 1 giờ. Lớp Secondary chỉ được chạy Economic Dispatch trên tập hợp máy đã ON.
- **Bảng giá tham chiếu (Reference Price Table):** Chuyển LMP/SMP làm hệ quy chiếu kinh tế. Bất kỳ sai số (deviation) nào sinh ra trong Real-time sẽ bị phạt hoặc thanh toán bù trừ dựa trên Bảng giá này.
- **Dải công suất điều chỉnh (Regulation/Base Points):** Ấn định mức công suất phát ban đầu và khóa các dải công suất khả dụng (Up/Down limits) để Secondary biết dư địa kỹ thuật còn lại.