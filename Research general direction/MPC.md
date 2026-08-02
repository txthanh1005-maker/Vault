---
weighttree: 1
contributors: 
  - Thanh K67
---
Direct Parent Connection: -> [[Algorithm]]

# MODEL PREDICTIVE CONTROL (MPC)
Thuật toán Điều khiển Dự báo Mô hình (MPC) là một chiến lược điều khiển vòng kín tối ưu hóa động (dynamic closed-loop optimization). Đặc trưng lớn nhất của nó là khả năng "Nhìn về tương lai, Hành động ở hiện tại, và Cuốn chiếu liên tục" nhằm bù trừ nhiễu loạn trong thời gian thực.

## 1. Bản chất Toán học (Pure Entity View)
- **Người đảm nhiệm (Assignee):** pure_entity_researcher
- **Mô hình Động học (State-Space):** Lõi của MPC là một bộ giả lập nội tại dự đoán quá trình tiến hóa: $x(k+1) = Ax(k) + Bu(k)$. Nó thuần túy là toán học, cô lập với bên ngoài.
- **Không gian Thời gian (Horizons):** 
  - *Chân trời dự đoán ($N_p$):* Độ xa mà MPC "nhìn thấy".
  - *Chân trời điều khiển ($N_c$):* Số bước cho phép biến đầu vào dao động ($N_c \le N_p$).
- **Hàm Năng lượng (Cost Function):** $J = \sum_{i=1}^{N_p} \|x - x_{ref}\|^2_Q + \sum_{i=0}^{N_c-1} \|u\|^2_R$. MPC cố gắng triệt tiêu độ lệch quỹ đạo (Q) bằng ít nỗ lực điều khiển nhất (R).
- **Cơ chế Cuốn chiếu (Receding Horizon):** Giải bài toán cho $N_p$ bước, nhưng **chỉ lấy nghiệm đầu tiên $u(k)$** để thực thi. Quỹ đạo còn lại bị vứt bỏ. Chu kỳ sau giải lại từ đầu.
- **Ràng buộc Tính toán (Bottleneck):** Thời gian giải (Solver time $T_{solve}$) phải luôn nhỏ hơn chu kỳ lấy mẫu ($T_s$). Nếu không, hệ thống sẽ sụp đổ.

## 2. Tương tác Cục bộ (Local Interaction View)
- **Người đảm nhiệm (Assignee):** grid_interaction_researcher
- **Hệ tham chiếu (Container):** Hệ thống điều khiển cục bộ Microgrid (Microgrid Local Control).
- **Luồng Tín hiệu (I/O):** 
  - *Inputs:* Lấy mẫu liên tục dòng điện ($I$), điện áp ($V$), tần số ($f$), SOC, và nhiễu (mây che Solar, tải đột biến).
  - *Outputs:* Tín hiệu Duty Cycle, Setpoints P/Q truyền xuống Inverter.
- **Động lực học Bù trừ Nhiễu (Disturbance Rejection):** MPC không chờ sụt áp/tần số diễn ra sâu như PID. Nó "nhìn" thấy quỹ đạo sụp đổ trong tương lai thông qua hàm $J$ và lập tức bơm công suất BESS bẻ gãy quỹ đạo sai lệch đó.
- **Cuộc chiến Tốc độ:** Chu kỳ tương tác phải duy trì ở vài **mili-giây (ms)**. Bất kỳ sự chậm trễ nào đều gây cộng hưởng phá vỡ lưới điện.

## 3. Liên kết trong Zettelkasten
- Khối MPC này là công cụ hoàn hảo cho **[Resilience_Short_Term](obsidian://open?file=Resilience_Short_Term):**  đặc biệt khi cần phản ứng siêu tốc (Fast DR, BESS Pulse) trong 15-60 phút.
- MPC thường đóng vai trò ở lớp **[Layer_Tertiary_Control](obsidian://open?file=Layer_Tertiary_Control):**  hoặc **[Layer_Secondary_Control](obsidian://open?file=Layer_Secondary_Control):** do tốc độ đáp ứng cực cao (từ ms đến vài giây).