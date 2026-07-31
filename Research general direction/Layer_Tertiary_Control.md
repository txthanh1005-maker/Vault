---
contributors: 
  - Khanh k67
  - Thanh K67
---
Direct Parent Connection: -> [[Power_System_Scheduling]]

# LAYER: TERTIARY CONTROL (TẦNG LẬP LỊCH DÀI HẠN)
*(Trục Thời gian L1: Tầng vĩ mô nhất của hệ thống, hoạt động hoàn toàn dựa trên Toán Kinh tế vĩ mô và phớt lờ nhiễu động vật lý tức thời).*

## 1. ĐẶC TÍNH NỘI TẠI (PURE ENTITY VIEW)
*(Báo cáo phân tích bản chất vật lý và cấu trúc toán học nội tại của mô hình MILP trong tối ưu tổ máy).*

### 1.1. Bản chất toán học của mô hình MILP (Mixed Integer Linear Programming)
Mô hình MILP phản ánh trạng thái đóng/cắt gián đoạn của tổ máy, biểu diễn không gian trạng thái (state space) bằng sự kết hợp khép kín giữa các biến rời rạc và biến liên tục.
- **Biến nhị phân (Binary Variables):**
  - $u_{i,t} \in \{0, 1\}$: Trạng thái cốt lõi của tổ máy $i$ tại bước thời gian $t$. ($1$: Đang vận hành sinh công, $0$: Ngừng máy).
  - $v_{i,t} \in \{0, 1\}$: Xung động động học khởi động. $v_{i,t} = 1$ khi tổ máy chuyển mức từ $0$ sang $1$.
  - $w_{i,t} \in \{0, 1\}$: Xung động động học ngừng máy. $w_{i,t} = 1$ khi tổ máy xả từ $1$ về $0$.
- **Mối liên hệ vi phân rời rạc:**
  $$v_{i,t} - w_{i,t} = u_{i,t} - u_{i,t-1}$$
  $$v_{i,t} + w_{i,t} \le 1$$

### 1.2. Bản chất Hàm mục tiêu nội tại
Cấu trúc chi phí cực tiểu hóa tổn hao năng lượng biến đổi và tĩnh:
$$\min \sum_{t=1}^T \sum_{i=1}^N \left[ C_i^p(P_{i,t}) + C_{i,t}^{su} + C_{i,t}^{sd} \right]$$
- **Tuyến tính hóa hàm hao phí nhiên liệu (Piecewise Linearization):**
  $$C_i^p(P_{i,t}) = A_i u_{i,t} + \sum_{k=1}^K s_{i,k} P_{i,t,k}$$

### 1.3. Bản chất Toán-Lý của Chi phí sấy lò ($C_{i,t}^{su}$)
Sự tiêu hao năng lượng sấy lò phụ thuộc mức độ hao hụt nội năng (hàm phân rã mũ $e^{-\tau / t_c}$). Để đưa vào MILP, trạng thái thất thoát nhiệt được chia thành $S$ phân đoạn mức năng lượng (Staircase Formulation).
- **Cơ chế mặt phẳng cắt (Cutting planes mechanism):**
  $$C_{i,t}^{su} \ge K_{i,s} \left[ u_{i,t} - \sum_{n=1}^{s} u_{i, t-n} \right] \quad \forall s \in \{1, 2, ..., S\}$$

### 1.4. Ràng buộc Động học: Thời gian lưu quán tính (Min Up/Down Time)
Khắc phục ứng suất nhiệt-cơ (Thermo-mechanical Stress) gây mỏi vật liệu chu kỳ thấp (low-cycle fatigue).
- **Ràng buộc duy trì trạng thái nóng tối thiểu (Min Up Time):**
  $$\sum_{k=t-UT_i+1}^t v_{i,k} \le u_{i,t}$$
- **Ràng buộc duy trì trạng thái xả nhiệt tối thiểu (Min Down Time):**
  $$\sum_{k=t-DT_i+1}^t w_{i,k} \le 1 - u_{i,t}$$

---

## 2. TƯƠNG TÁC CỤC BỘ (LOCAL INTERACTION VIEW)
*(Đóng vai trò "Người thiết lập giới hạn" bên trong Container: `Power_System_Scheduling`).*

**Vai trò của Nút:**
Layer Tertiary Control thiết lập các "đường bao ràng buộc" (boundary conditions) và định cỡ không gian vận hành khả thi.

**Phân tích Tín hiệu Đầu ra (Truyền xuống các Tầng dưới):**
1. **Truyền tín hiệu "Lịch chạy máy dự kiến dài hạn" xuống Day-Ahead:**
   - **Bản chất tín hiệu:** Các vector kế hoạch huy động nguồn cấp tuần/tháng và điều kiện biên về dự trữ năng lượng.
   - **Cơ chế tương tác:** Đóng vai trò ràng buộc cứng/mềm đối với Day-Ahead UC. Chống tình trạng "cận thị" (myopic) trong tối ưu hóa ngắn hạn.
2. **Truyền tín hiệu "Biên độ đầu tư CAPEX" xuống Secondary và Day-Ahead:**
   - **Bản chất tín hiệu:** Các đánh giá ngưỡng chi phí cơ hội và giới hạn ngân sách đầu tư nguồn dự phòng linh hoạt.
   - **Cơ chế tương tác:** Xác định "quy mô băng thông dự trữ" (Reserve Capacity Bandwidth) mà Secondary có quyền sử dụng cho AGC mà không phá vỡ trần OPEX/CAPEX.

**Nguyên tắc Giới hạn (Isolation Rule):**
Cách ly hoàn toàn khỏi các thực thể vật lý. Chỉ tương tác qua luồng trao đổi Data Models và Cost Functions.

---

## 3. Sơ đồ Phân loại (Taxonomy / Splitting)
- **[Unit Commitment](obsidian://open?file=Unit_Commitment):** (UC) Bài toán tổ hợp máy phát (MIP) ấn định lịch BẬT/TẮT dài hạn của hàng trăm tổ máy. 
- **[Macro Forecasting](obsidian://open?file=Macro_Forecasting):** Hệ thống dự báo vĩ mô. Dự báo phụ tải đỉnh và sản lượng điện tái tạo dài hạn dựa trên dữ liệu khí tượng học.