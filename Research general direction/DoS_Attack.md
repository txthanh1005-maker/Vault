---
contributors: 
  - D.M.Hai K67
---
Direct Parent Connection: -> [[Inside_Attack]]

# Denial of Service Attack (DoS)
*(Tấn công Từ chối Dịch vụ: Dạng tấn công cạn kiệt tài nguyên mạng và hệ thống hàng đợi bằng lưu lượng dữ liệu rác, nhằm đánh sập kênh giao tiếp)*

## 1. Đặc tính Nội tại (Pure Entity View)
- **Người đảm nhiệm (Assignee):** pure_entity_researcher
- **Cấu trúc Hàng đợi (Queueing Model):** Node mạng bị tấn công hoạt động theo hệ thống $M/M/c/K$. DoS ép $\lambda \to \infty$ khiến $\rho = \frac{\lambda}{c \mu} \ge 1$. Hàng đợi $Q(t)$ dâng cao chạm đỉnh $K$, đưa hệ thống vào trạng thái bất ổn định (unstable state).
- **Vật lý cạn kiệt tài nguyên:** Khủng hoảng ngắt phần cứng (Interrupt Storm/Livelock). Khi $\lambda_{IRQ} > \frac{F_{cpu}}{C_{ctx}}$, 100% thời gian CPU bị kẹt ở Kernel-space, đóng băng hoàn toàn User-space. Đối với Stateful DoS (như TCP SYN), số lượng kết nối $S(t)$ hội tụ lên đỉnh dung lượng RAM, ép hệ thống thả (drop) hoàn toàn chức năng mạng.
- **Nhiệt động lực học Thông tin:** Dựa trên Nguyên lý Landauer. Tốc độ bơm Entropy cực đại từ luồng rác DoS vượt xa tốc độ giảm Entropy (xử lý dữ liệu có trật tự) của CPU, tạo ra nút thắt cổ chai tính toán triệt để.

## 2. Tương tác Cục bộ (Local Interaction View)
- **Người đảm nhiệm (Assignee):** grid_interaction_researcher
- **Hệ tham chiếu (Container):** Inside_Attack (Mạng liên lạc/dữ liệu nội bộ)
- **Tín hiệu In/Out Cục bộ:** 
  - Đầu vào từ Container: Băng thông khả dụng $BW_{avail}$, Kích thước bộ đệm $Q_{max}$. 
  - Tín hiệu của DoS: Tốc độ bơm $\lambda_{DoS}(t)$ (Packets/second).
  - Tín hiệu Đầu ra cục bộ: Tín hiệu trễ thời gian $\tau(t)$ và Tín hiệu suy hao dữ liệu $\eta(t)$.
- **Động lực học Hàm truyền:** Hàm truyền mạng ở trạng thái bình yên là khâu trễ tĩnh $H_{ideal}(s) = e^{-s \tau_0}$. DoS biến đổi nó thành hệ phi tuyến bất định $H_{cyber}(s, t) = \big(1 - \eta(t)\big) \cdot e^{-s \cdot \tau(t)}$. Sự bành trướng của $\lambda_{DoS}$ bóp nghẹt biên độ truyền đạt $(1-\eta) \to 0$ và kéo dãn trễ pha tới vô cùng $\tau(t) \to \infty$.

## 3. Điểm mạnh, Điểm yếu & Ứng dụng (Pros, Cons & Use Cases)
- **Điểm mạnh (Strengths):** Không cần hiểu cấu trúc ma trận topology của lưới điện (Black-box attack). Sức tàn phá diện rộng, cắt đứt hoàn toàn dòng chảy Dữ liệu (Availability).
- **Điểm yếu (Weaknesses):** Thiếu tính ẩn mình (Stealth). Lưu lượng khổng lồ (Traffic Spike) rất dễ bị thiết bị lọc Firewall/IDS phát hiện và khóa IP.
- **Khi nào dùng (When to use):** Ngăn chặn luồng dữ liệu PMU gửi về Control Center, khiến các thuật toán như PI/MPC bị "mù", làm đứt gãy vòng lặp điều khiển kín.
- **Khi nào KHÔNG dùng (When NOT to use):** Khi mục đích tấn công là thao túng con số để trục lợi thị trường (FDI) mà không để lộ hành tung.