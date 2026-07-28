---
contributors:
  - Thanh K67
---
Direct Parent Connection: -> [[Algorithm]]

# P2P ENERGY TRADING (GIAO DỊCH NĂNG LƯỢNG NGANG HÀNG)
Bài toán định tuyến dòng tiền và dòng năng lượng trực tiếp giữa các hộ tiêu thụ và hộ sản xuất (Prosumers) trong cùng một lưới điện khu vực, bỏ qua hoặc giảm thiểu vai trò của công ty điện lực độc quyền truyền thống.

## 1. Đặc tính Nội tại (Pure Entity View)
- **Người đảm nhiệm (Assignee):** pure_entity_researcher
- **Cơ chế Toán học (Mathematical Clearing Mechanisms):** 
  - Đấu giá kép (Double Auction).
  - Lý thuyết Trò chơi (Game Theory): Hợp tác (Shapley Value) hoặc Không hợp tác (Nash Equilibrium).
  - Tối ưu hóa phân tán: Phân rã kép (Dual Decomposition), ADMM.
- **Ràng buộc Mạng Vật lý (Physical Network Constraints):** Tổng năng lượng bán phải bằng tổng năng lượng mua cộng với tổn thất truyền tải nội bộ ($P_{loss} = \sum R_{ij} |I_{ij}|^2$). Chịu sự kiềm tỏa chặt chẽ của giới hạn nhiệt dây dẫn ($|I_{ij}| \le I_{ij}^{max}$) và điện áp ($V_{min} \le V_i \le V_{max}$).
- **Giới hạn Hội tụ & Trễ Dữ liệu:** Thuật toán bị giới hạn bởi vận tốc hội tụ $\rho$ và độ trễ viễn thông $\tau$, dẫn đến rủi ro không đạt Cân bằng nếu chu kỳ giao dịch quá ngắn so với độ trễ truyền tin.

## 2. Tương tác Cục bộ (Local Interaction View)
- **Người đảm nhiệm (Assignee):** grid_interaction_researcher
- **Hệ tham chiếu (Container):** Microgrid / Distribution Network (Lưới điện vi mô / Mạng phân phối cục bộ).
- **Vai trò Tương tác:** Hoạt động như một "bộ tự cân bằng nội động" (internal self-balancer). Nhận trạng thái cung/cầu rời rạc, đối khớp dựa trên tín hiệu thị trường và định tuyến công suất nội bộ để triệt tiêu biến động trước khi trào ra lưới điện bên ngoài.
- **Tín hiệu Đầu vào (Inputs):** Dữ liệu cung/cầu cục bộ, số liệu đo đếm xác thực từ Smart Meter, và các ràng buộc mạng (Network Constraints) của Microgrid để chặn hiện tượng nghẽn mạch cục bộ.
- **Tín hiệu Đầu ra (Outputs):** Tín hiệu giá năng lượng cục bộ (Local Dynamic Pricing) và Lệnh điều phối công suất ngang hàng (P2P Dispatch Setpoints).
- **Hàm truyền tương tác:** $P_{dispatch}(t) = \Phi( Supply, Demand, Price\_Signal ) * K_{constraint}$. Hệ số cản $K_{constraint}$ từ Container sẽ lập tức bóp nghẹt (phạt) các lệnh giao dịch kinh tế nếu chúng vi phạm giới hạn vật lý.

## 3. Phả hệ Cấu trúc & Tính chất Kế thừa (Bottom-Up Tree Properties)
P2P Energy Trading đóng vai trò là "Nút Gốc" (Parent Node), dung nạp toàn bộ các tính chất nội tại và tương tác từ các "Nút Con" (Children Nodes) của nó:
- **Kế thừa từ [[ADMM]] (Động cơ Toán học):** P2P mang tính chất phân rã của ADMM. Giao dịch P2P được bảo mật tuyệt đối vì chỉ trao đổi Biến đồng thuận (Consensus variables) chứ không lộ dữ liệu gốc. Tuy nhiên, P2P mang theo "quán tính" của hệ $\rho$: quá trình chốt đơn hàng P2P nhanh hay chậm phụ thuộc vào cơ chế Dynamic Penalty của ADMM.
- **Kế thừa từ [[Mesh_P2P]] (Cấu trúc Lưới):** Nếu P2P chạy trên nền Mesh, nó hấp thụ tính **Đàn hồi (Resilience)** và **Đa liên kết**. Dòng tiền/năng lượng có thể đi vòng qua các sự cố đứt cáp. Tuy nhiên, P2P ở dạng này sẽ gánh chịu nhược điểm **Ngập lụt truyền tin (Data Flooding)** do sự quảng bá thông điệp quá mức.
- **Kế thừa từ [[Radial_P2P]] (Cấu trúc Tia):** Nếu P2P chạy trên nền mạng Radial truyền thống, nó mang tính chất toán học lồi hóa hoàn hảo (SOCP Relaxation) giúp tìm nghiệm cực nhanh. Nhưng về mặt vật lý, P2P Radial dính tử huyệt: **Nút thắt cổ chai** tại các ngã rẽ và **Độ nhạy bất đối xứng** (Giao dịch P2P ở cuối đường dây dễ dàng gây sụt áp lan truyền làm sập lưới).

## 4. Điểm mạnh, Điểm yếu & Ứng dụng
- **Điểm mạnh:** Khuyến khích năng lượng tái tạo phân tán. Giảm tổn thất truyền tải. Tối đa hóa phúc lợi xã hội (Social Welfare).
- **Điểm yếu:** Xung đột cực kỳ gay gắt giữa Giao dịch kinh tế (ảo) và Dòng công suất vật lý (thật).
- **Ứng dụng:** Vận hành Cộng đồng năng lượng thông minh, Microgrid độc lập. Tránh dùng ở khu vực không có hạ tầng lưới điện thông minh hoặc băng thông viễn thông chập chờn.