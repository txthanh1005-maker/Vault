---
weighttree: 1
contributors: 
  - Thanh K67
---
Direct Parent Connection: -> [[MILP]]

# MILP XÉT 1 NÚT (SINGLE NODE MILP)
Thuật toán tối ưu hóa tuyến tính nguyên hỗn hợp tập trung giải quyết bài toán cân bằng và vận hành nội tại tại một điểm nút duy nhất, bỏ qua hoàn toàn cấu trúc vật lý của hệ thống lưới điện truyền tải.

## 1. Đặc tính Nội tại (Pure Entity View)
- **Người đảm nhiệm (Assignee):** pure_entity_researcher
- **Lõi Trạng thái Động (Dynamic State Core):** Đại diện bởi biến liên tục $x_t \in \mathbb{R}$. Mang tính chất Markovian ($x_t$ phụ thuộc $x_{t-1}$).
- **Không gian Quyết định Rời rạc (Discrete Decision Manifold):** Biến nhị phân $u_t \in \{0, 1\}$ đóng vai trò như các "công tắc" logic chia cắt không gian giải pháp thành các đa diện lồi.
- **Trục Thời gian Tích hợp:** Toàn bộ độ phức tạp nằm ở liên kết chuỗi thời gian: $x_t = \alpha x_{t-1} + \beta P_t^{in} - \gamma P_t^{out}$.
- **Đặc tính Vật lý & Nhiệt động lực học:**
  - *Ranh giới sức chứa:* $X_{min} \le x_t \le X_{max}$.
  - *Tốc độ biến thiên (Ramp-rates):* $-R_{down} \le P_t - P_{t-1} \le R_{up}$.
  - *Hao hụt không thuận nghịch:* Hiệu suất $\eta \in (0,1)$ chi phối dòng vào/ra.
  - *Tính loại trừ tương hỗ:* Logic cấm hai trạng thái mâu thuẫn xảy ra đồng thời ($u_t^{in} + u_t^{out} \le 1$).

## 2. Tương tác Cục bộ (Local Interaction View)
- **Người đảm nhiệm (Assignee):** grid_interaction_researcher
- **Hệ tham chiếu (Container):** Hệ thống Quản lý Năng lượng Cục bộ (LEMS) / Microgrid cô lập.
- **Vai trò tương tác:** Đóng vai trò là "Bộ xử lý Quyết định Cục bộ" (Local Decision Engine). Hoàn toàn "mù" về nhận thức đối với các nút lân cận hay bất kỳ cấu trúc lưới điện nào bên ngoài.
- **Tín hiệu Đầu vào (Inputs):** Tín hiệu trạng thái nội bộ (Initial SoC), Tín hiệu Biên (Dự báo tải, năng lượng tái tạo) và Ràng buộc thiết bị (Inverter Limits).
- **Tín hiệu Đầu ra (Outputs):** Lệnh điều độ biến thiên (Charge/Discharge) và Lệnh điều khiển trạng thái nhị phân (bật/tắt dự phòng, sa thải phụ tải).
- **Hàm truyền tương tác:** Thuật toán hoạt động như một bộ lọc phi tuyến. Nó hấp thụ sự biến động ngẫu nhiên của phụ tải/nguồn, bóp méo/điều hướng chúng để tạo ra một đường cong công suất trơn tru cho Container xử lý. Chịu hiện tượng "đáp ứng trễ theo pha" do rời rạc hóa thời gian.

## 3. Điểm mạnh, Điểm yếu & Ứng dụng (Pros, Cons & Use Cases)
- **Điểm mạnh (Strengths):** Mô hình rất nhẹ, tốc độ hội tụ cực nhanh. Rất hiệu quả để lập lịch vận hành nội bộ (Self-scheduling) tham gia thị trường điện.
- **Điểm yếu (Weaknesses):** Điểm mù về năng lực truyền tải. Nếu kết quả bơm/rút đẩy lên lưới gây nghẽn mạch, bản thân thuật toán này sẽ không thể tự khắc phục được do không có ma trận nhánh.
- **Khi nào dùng (When to use):** Tối ưu hóa Trạm sạc EV, HEMS (Home Energy Management System), Microgrid cô lập không có vấn đề về truyền tải đi xa.
- **Khi nào KHÔNG dùng (When NOT to use):** Giải bài toán OPF (Optimal Power Flow), tái cấu trúc lưới điện, hay định tuyến dòng công suất.
