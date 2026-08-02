---
weighttree: 0
contributors: 
  - Khanh k67
---
Direct Parent Connection: -> [[Exact_Solvers]]

# FORWARD DYNAMIC PROGRAMMING (FDP)
*(Thuật toán Quy hoạch động dạng tiến là cấu trúc toán học tối ưu hóa quỹ đạo dựa trên nguyên lý tối ưu Bellman, giải quyết các bài toán quyết định nhiều giai đoạn theo chiều thời gian tiến, từ trạng thái khởi tạo $k=0$ đến trạng thái đích $k=N$.)*

## 1. Đặc tính Nội tại (Phần Âm - Nội sinh)
- **Bản chất Cấu trúc:** FDP tính toán theo luồng thời gian thực tế, lan truyền thông tin chi phí từ quá khứ đến hiện tại trên một Đồ thị có hướng không chu trình (DAG). Tại bước $k$, nó không cần thông tin của bước $k+1$.
- **Tính chất Markov:** Quá trình ra quyết định chỉ phụ thuộc vào trạng thái hiện tại $x_k$. Mọi thông tin lịch sử đều được nén vào biến trạng thái và hàm giá trị.
- **Không gian Trạng thái & Ma trận chuyển trạng thái:** Không gian trạng thái $X_k$ và Không gian hành động $U_k$ bị chi phối bởi phương trình động lực học $x_{k+1} = f_k(x_k, u_k)$. 
- **Phương trình Bellman (Dạng tiến):** 
  $$V_{k}(x_k) = \min_{u_{k-1} \in U_{k-1}} \Big[ V_{k-1}(x_{k-1}) + C_{k-1}(x_{k-1}, u_{k-1}) \Big]$$
  Hàm $V_k(x_k)$ (Cost-from-Start) đại diện cho chi phí tối thiểu để đưa hệ thống từ trạng thái ban đầu đạt tới trạng thái $x_k$. Tại mỗi bước, thuật toán chỉ lưu giữ lại quỹ đạo tối ưu nhất, loại bỏ các quỹ đạo dư thừa.
- **Lời nguyền số chiều & Rời rạc hóa:** Khi số lượng biến trạng thái tăng, khối lượng tính toán bùng nổ cấp số nhân (Curse of Dimensionality). Để giải quyết, không gian trạng thái phải được "lượng tử hóa" thành lưới (State Grids). Việc này sinh ra sai số nội suy (Interpolation Error) khi $x_{k+1}$ rơi vào khoảng trống giữa các điểm lưới, phá vỡ tính tối ưu tuyệt đối.

## 2. Tương tác Cục bộ (Phần Dương - Giao tiếp với Container)
- **Container (Vỏ bọc trực tiếp):** Bài toán Lập lịch Năng lượng (Energy Scheduling Problem).
- **Vai trò Tương tác:** Container đóng vai trò "Môi trường đặc tả" thiết lập không gian và hàm mục tiêu. FDP đóng vai trò "Động cơ hộp đen" (Solver Engine), hoàn toàn bị động ở khâu định nghĩa và chỉ thực hiện quét không gian trạng thái.
- **Giao diện Tín hiệu Đầu vào (Container truyền cho FDP):**
  - *Tín hiệu chi phí biên:* Hàm mục tiêu chi phí cục bộ tại từng bước $t$ (ví dụ: chuỗi giá điện theo thời gian).
  - *Biên không gian trạng thái:* Các ma trận/vector giới hạn (ví dụ: $SOC_{min} \le SOC_t \le SOC_{max}$, $P_{min} \le P_t \le P_{max}$).
  - *Hàm chuyển đổi trạng thái & Điều kiện biên:* Phương trình động học (như $SOC_{t+1} = SOC_t + P_t$) và trạng thái khởi tạo/đích.
- **Đặc tính Truyền tải:** Tín hiệu từ Container buộc phải được lượng tử hóa thành lưới trước khi bơm cho FDP. Độ mịn của lưới là "nút thắt tương tác" quyết định sự đánh đổi giữa băng thông tính toán và độ chính xác.
- **Giao diện Tín hiệu Đầu ra (FDP phản hồi cho Container):**
  - *Quỹ đạo tối ưu (Optimal Trajectory):* Chuỗi các trạng thái $SOC^*_t$ trơn tru mô tả quỹ đạo năng lượng tối ưu.
  - *Chính sách hành động (Optimal Policy):* Vector tín hiệu hành động $P^*_t$ (công suất nạp/xả) tương ứng.
  - *Tín hiệu chi phí tới hạn (Value Function):* Tổng chi phí/lợi nhuận cực trị giúp Container định lượng được chất lượng kinh tế.

## 3. Điểm mạnh, Điểm yếu và Ứng dụng
- **Điểm mạnh:** Giải quyết xuất sắc các bài toán phi tuyến phức tạp có biến trạng thái phụ thuộc thời gian (như SoC của Pin) mà các bộ giải tuyến tính MILP gặp bế tắc. Phù hợp tuyệt đối với điều khiển thời gian thực do tối ưu thuận từ hiện tại về tương lai.
- **Điểm yếu:** Đòi hỏi cấu hình phần cứng lớn, dễ tràn RAM do Lời nguyền số chiều nếu hệ thống có quá nhiều tác nhân độc lập (không phù hợp để mô phỏng chi tiết hàng ngàn xe EV riêng lẻ).
- **Ứng dụng:** 
  - Lập lịch sạc xe điện tối ưu kinh tế (Cân bằng SoC mong muốn với giá điện động).
  - Quản lý vận hành BESS độc lập trong Microgrid.
  - Giải quyết bài toán Thủy điện tích năng nhiều hồ chứa tuần tự.