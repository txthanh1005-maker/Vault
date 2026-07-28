---
contributors: 
  - Thanh K67
---
Direct Parent Connection: -> [[MILP]]

# MILP XÉT NHIỀU NÚT (MULTI NODE MILP - POWER FLOW TOPOLOGY)
Mô hình quy hoạch tuyến tính nguyên hỗn hợp nhúng các định luật vật lý của mạch điện (Kirchhoff, giới hạn nhiệt) vào không gian tối ưu để giải bài toán định tuyến công suất và vận hành trên một đồ thị mạng lưới đa nút.

## 1. Đặc tính Nội tại (Pure Entity View)
- **Người đảm nhiệm (Assignee):** pure_entity_researcher
- **Phân loại theo Mô hình Dòng công suất:** Sử dụng DCOPF (Tuyến tính hóa hoàn toàn $R \approx 0, |V| \approx 1$) hoặc Lin-ACPF (Tuyến tính hóa từng khúc - Piecewise).
- **Phân loại theo Không gian Biến:** Cấu trúc B-Matrix (sử dụng biến góc pha $\theta_i$) hoặc cấu trúc PTDF (sử dụng ma trận độ nhạy, triệt tiêu góc pha).
- **Đặc tính Vật lý & Biên Toán học:**
  - *Định luật Kirchhoff 1 (KCL):* Cân bằng nút $\sum P_{gen} - \sum P_{load} = \sum P_{ij}$. Nút điện không có khả năng tích trữ năng lượng.
  - *Định luật Kirchhoff 2 (KVL):* Ràng buộc luồng nhánh $P_{ij} = \frac{1}{x_{ij}} (\theta_i - \theta_j)$. Công suất không đi theo con đường ngắn nhất, mà đi theo con đường có trở kháng nhỏ nhất.
  - *Ranh giới nhiệt (Thermal Limits):* $-F_{ij}^{max} \le P_{ij} \le F_{ij}^{max}$. Vi phạm dẫn đến trạng thái Infeasible của toàn hệ thống.
  - *Bản chất Đa diện Rời rạc (Discrete Polyhedral):* Cấu trúc liên kết (Network Topology) kết hợp với biến nguyên làm độ phức tạp tăng theo cấp số nhân (NP-Hardness), giới hạn tốc độ hội tụ của Branch-and-Bound.

## 2. Tương tác Cục bộ (Local Interaction View)
- **Người đảm nhiệm (Assignee):** grid_interaction_researcher
- **Hệ tham chiếu (Container):** Mạng Lưới Phân Phối (Distribution Network) có nhiều đường dây.
- **Vai trò tương tác:** Đóng vai trò là "Bộ điều phối luồng" (Flow Coordinator). Tiếp nhận khả năng của mạng lưới và trả về các quyết định định tuyến công suất sao cho không phá vỡ giới hạn vật lý.
- **Tín hiệu Đầu vào (Inputs):** Dữ liệu lưới (sơ đồ, khoảng cách nhánh), Giới hạn công suất truyền tải (Ampacity limits), và Trạng thái nguồn/ tải (Nodal injection) tại từng nút.
- **Tín hiệu Đầu ra (Outputs):** Vector chỉ lệnh phân bổ luồng công suất (Power Flow Dispatch) và Lệnh giới hạn luồng (các quyết định đóng/cắt tại các giao cắt haowcj bật tắt tổ máy).
- **Hàm truyền tương tác:** Thuật toán là ma trận biến đổi $\mathcal{F}$ ánh xạ trạng thái tải thành ma trận luồng nhánh. Tính đáp ứng định tuyến tức thời: Ngay khi Container báo có đường dây nguy cơ quá tải, thuật toán MILP sẽ lập tức dội dòng (reroute) để lách qua lộ trình khác an toàn hơn.

## 3. Điểm mạnh, Điểm yếu & Ứng dụng (Pros, Cons & Use Cases)
- **Điểm mạnh (Strengths):** Tối ưu hóa toàn cục, khắc phục tuyệt đối "điểm mù" không gian của MILP 1 nút. Giải quyết được bài toán nghẽn mạch (Congestion).
- **Điểm yếu (Weaknesses):** Bùng nổ tổ hợp (Curse of Dimensionality). Cực kỳ chậm chạp với hệ thống lớn (nhiều biến nhị phân). Đánh đổi độ chính xác của điện áp và công suất phản kháng do phải tuyến tính hóa phương trình AC.
- **Khi nào dùng (When to use):** Quy hoạch vị trí thiết bị (Siting & Sizing), Tái cấu trúc mạng phân phối (Network Reconfiguration), Lập lịch thị trường có xét ràng buộc lưới.
- **Khi nào KHÔNG dùng (When NOT to use):** Điều khiển tần số thời gian thực (Micro-giây), hoặc tối ưu hóa quá sâu vào chu kỳ sạc xả hóa học của một viên pin.
