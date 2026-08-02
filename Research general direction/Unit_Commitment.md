---
weighttree: 0
contributors: 
  - Thanh K67
---
Direct Parent Connection: -> [[Algorithm]]

# UNIT COMMITMENT (UC)
Thuật toán Lập lịch Tổ máy (Unit Commitment) là bài toán tối ưu hóa nhằm xác định trạng thái đóng/cắt (On/Off) và mức phát điện của các tổ máy trong hệ thống qua các chu kỳ thời gian liên tiếp để cực tiểu hóa tổng chi phí vận hành, đồng thời đáp ứng các ràng buộc vật lý, nhiệt động lực học và yêu cầu hệ thống.

## 1. Đặc tính Nội tại (Pure Entity View)
- **Người đảm nhiệm (Assignee):** pure_entity_researcher
- **Không gian Biến Quyết định (Decision Space):** 
  - *Không gian rời rạc:* Biến trạng thái $u_{i,t} \in \{0,1\}$ (On/Off), biến phụ trợ khởi động ($v_{i,t}$) và dừng ($w_{i,t}$).
  - *Không gian liên tục:* Biến sản lượng $P_{i,t} \ge 0$, hình thành bài toán Phân bổ Kinh tế (Economic Dispatch) bên trong.
- **Trường phái Thuật toán (Algorithmic Topology):** MILP/MINLP, Quy hoạch động (Dynamic Programming), Nới lỏng Lagrange (Lagrangian Relaxation), Siêu heuristic (Meta-heuristics).
- **Hàm Mục tiêu (Objective Function):** Sự kết tinh toán học của các dòng tiêu hao năng lượng nội tại, bao gồm chi phí nhiên liệu (hàm đa thức), chi phí khởi động (hàm bậc thang/mũ), và chi phí dừng máy (hãm động lượng). Ngoài ra còn là sự phối hợp của NLTT, BESS, và các thành phần tải khác như DR hay EV. 
- **Đặc tính Vật lý & Nhiệt động lực học:**
  - *Ranh giới Nhiệt động lực học:* $P_{min} \le P \le P_{max}$. Bị giới hạn bởi ngưỡng nhiệt độ tối đa, cấu trúc tuabin, và tính ổn định ngọn lửa (flame stability). Tồn tại vùng cấm (Prohibited Operating Zones) do cộng hưởng cơ học.
  - *Ứng suất Nhiệt (Thermal Stress) - Ramp rate:* Tốc độ tăng giảm tải bị giới hạn cực kỳ khắt khe để tránh sốc nhiệt gây nứt gãy tuabin.
  - *Quán tính Trễ (MUT/MDT):* Giới hạn thời gian đóng/cắt tối thiểu (Min Up/Down Time) để khối kim loại co ngót nhiệt và giải phóng ứng suất dư.
  - *Suy biến Nhiệt (Cold/Warm/Hot Start):* Năng lượng khởi động (Ignition thermal requirements) tăng theo hàm mũ (định luật làm mát Newton) tính từ lúc tắt máy.

## 2. Tương tác Cục bộ (Local Interaction View)
- **Người đảm nhiệm (Assignee):** grid_interaction_researcher
- **Hệ tham chiếu (Container):** Trung tâm Điều độ Hệ thống Điện (ISO/TSO).
- **Vai trò tương tác:** UC đóng vai trò là "Bộ xử lý Ra quyết định" (Decision Engine) hoạt động độc lập với thế giới vật lý thực, thực hiện chuyển đổi dữ liệu (Data-to-Decision) bên trong phần mềm của ISO/TSO.
- **Tín hiệu Đầu vào (Inputs):** Dữ liệu dự báo tải, Ma trận thông số kỹ thuật (Pmax, Ramp-rate, MUT/MDT), Tín hiệu kinh tế (hồ sơ chi phí, bản chào giá), và Tín hiệu biên an toàn (Dự trữ quay / Spinning Reserve).
- **Tín hiệu Đầu ra (Outputs):** Kế hoạch vận hành $U$ (Lịch Bật/Tắt) và $P$ (Công suất nền), cùng các Cờ cảnh báo vô nghiệm (Infeasibility Flags) để Container nới lỏng ràng buộc.
- **Hàm truyền tương tác (Transfer Function & Dynamics):**
  - *Cơ chế Hộp đen:* UC cô lập tính phức tạp tổ hợp và cung cấp trạng thái khả thi tĩnh cho hệ thống của ISO.
  - *Giới hạn Độ trễ Giao tiếp (Latency limit):* UC phải hội tụ trong một khung thời gian hữu hạn nghiêm ngặt (vd: Gate Closure của Day-Ahead Market). Nếu UC tính quá chậm so với nhịp điều độ, toàn bộ hệ thống ISO/TSO sẽ bị đình trệ.
  - *Vòng lặp Nới lỏng (Feedback Loop):* Nếu tín hiệu đầu vào từ ISO/TSO quá khắt khe (dẫn đến không đủ dự trữ quay) khiến Output trả về là Infeasible, ISO/TSO phải kích hoạt cơ chế nới lỏng ràng buộc (Constraint Relaxation) và gọi UC tính toán lại từ đầu.
