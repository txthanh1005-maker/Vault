---
weighttree: 0
contributors: 
  - Thanh K67
---
Direct Parent Connection: -> [[Demand_Response]]

# INCENTIVE-BASED DEMAND RESPONSE (IBDR)
Đáp ứng nhu cầu dựa trên khuyến khích (IBDR) là cơ chế thay đổi phụ tải mang tính bắt buộc hoặc bán bắt buộc, trong đó người tham gia nhận được thù lao tài chính cố định hoặc thanh toán theo hiệu suất khi cam kết cắt giảm công suất lúc hệ thống yêu cầu.

## 1. Đặc tính Nội tại (Pure Entity View)
- **Người đảm nhiệm (Assignee):** pure_entity_researcher
- **Phân loại Cấu trúc Điều khiển:**
  - *Điều khiển Trực tiếp (Direct Load Control - DLC):* Dải điều chế tín hiệu Master-to-Slave gián đoạn (On/Off) can thiệp thẳng vào rơ-le thiết bị tải.
  - *Logic Hợp đồng (Interruptible/Curtailable):* Ranh giới logic cứng (Hard boundaries) đi kèm với hàm phạt (Penalty function) phi tuyến nếu độ lệch $\Delta P < P_{commit}$.
  - *Cấu trúc Đấu giá (Demand Bidding):* Đa tác tử tìm kiếm điểm cân bằng Nash qua ma trận tối ưu hóa ngân sách (vd: thuật toán đấu giá VCG).
- **Đặc tính Vật lý & Nhiệt động lực học:**
  - *Bảo toàn Entanpi & Hiệu ứng Rebound:* Năng lượng dịch chuyển tạo ra xung Dirac (Cold Load Pickup) sau khi lệnh IBDR kết thúc, do hệ thống tải (như HVAC) phải "hút" năng lượng cực mạnh để hồi phục trạng thái ban đầu.
  - *Độ thỏa dụng biên giảm dần (Diminishing Marginal Utility):* Càng kích thích thù lao $I(t)$ mạnh, sự phản hồi $\Delta P$ càng tiệm cận giới hạn bão hòa $P_{max\_response}$.
  - *Giới hạn Dải chết Nhiệt (Thermal Deadband):* Khả năng khả dụng của IBDR bị khóa chặt trong phương trình truyền nhiệt $T_{min} < T < T_{max}$. Vi phạm ranh giới này khiến IBDR vô hiệu lực.
  - *Suy hao cơ điện (Fatigue limits):* Tần số can thiệp $f_{IBDR}$ cao gây bào mòn tuổi thọ thiết bị.

## 2. Tương tác Cục bộ (Local Interaction View)
- **Người đảm nhiệm (Assignee):** grid_interaction_researcher
- **Hệ tham chiếu (Container):** Trung tâm Điều độ Lưới điện (ISO) / Aggregator.
- **Vai trò tương tác:** Đóng vai trò là một "Khối tài nguyên linh hoạt" (Flexible Resource Block) hay "Dự phòng ảo" (Virtual Reserve). Aggregator không cần biết bên trong Hộp đen cắt giảm máy móc gì, chỉ cần biết đầu ra tổng hợp có đạt cam kết không.
- **Tín hiệu Đầu vào (Inputs):** Tín hiệu điều khiển (Yêu cầu mức giảm $\Delta P_{req}$, thời gian $t_{start}, T_{duration}$), Tín hiệu thù lao $I(t)$, và đường cơ sở ảo (Baseline).
- **Tín hiệu Đầu ra (Outputs):** Độ lệch công suất thực tế $\Delta P_{actual}(t)$ và Tín hiệu xác nhận dung lượng (Opt-in/Opt-out / Capacity Status).
- **Hàm truyền tương tác (Transfer Function & Dynamics):**
  - *Tính Bất định & Biến động:* $\Delta P_{actual}$ từ IBDR mang tính xác suất chứ không tất định như máy phát điện truyền thống. Aggregator phải xử lý một sai số cục bộ $\epsilon$ do sự phập phù của Baseline.
  - *Giới hạn năng lượng (Ramp-rate & Duration Limits):* IBDR không thể duy trì $\Delta P_{actual}$ vô hạn. Nó bị giới hạn thời gian chạy (Duration); khi đạt ngưỡng năng lượng giới hạn, tín hiệu đầu ra sẽ tự động suy giảm hoặc ngắt.
