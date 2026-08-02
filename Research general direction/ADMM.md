---
weighttree: 1
contributors: 
  - Thanh K67
---
Direct Parent Connection: -> [[P2P_Energy_Trading]]

# ADMM (Alternating Direction Method of Multipliers)
ADMM (Alternating Direction Method of Multipliers - Phương pháp Nhân tử Hướng luân phiên) là một thuật toán tối ưu hóa phân tương, kết hợp giữa sự phân rã (decomposition) của phương pháp nhân tử Lagrange và sự hội tụ mạnh mẽ của phương pháp đạo hàm đối ngẫu (Dual Ascent). Thuật toán này thường đóng vai trò là "động cơ toán học" phía sau các mô hình giao dịch P2P, cho phép tính toán phân tán mà không làm lộ thông tin cá nhân.

## 1. Đặc tính Nội tại (Pure Entity View)
- **Người đảm nhiệm (Assignee):** pure_entity_researcher
- **Cấu trúc Bài toán (Primal Structure):** Phân rã mục tiêu thành 2 khối không gian độc lập: $\min_{x,z} f(x) + g(z)$, với ràng buộc vật lý $Ax + Bz = c$.
- **Hàm Năng lượng Nội tại (Augmented Lagrangian):** $L_{\rho}(x, z, y) = f(x) + g(z) + y^T(Ax + Bz - c) + \frac{\rho}{2}\|Ax + Bz - c\|_2^2$. Bao gồm lõi nguyên thủy (primal), lõi đối ngẫu (lực phản hồi từ biến $y$), và lõi phạt (penalty core $\rho$).
- **Bước lặp Luân phiên (Alternating Iteration Topology):** Tại chu kỳ $k$, thực hiện 3 vi bước theo nguyên lý Gauss-Seidel:
  1. *Khối cập nhật X:* $x^{k+1} = \arg\min_x L_{\rho}(x, z^k, y^k)$
  2. *Khối cập nhật Z:* $z^{k+1} = \arg\min_z L_{\rho}(x^{k+1}, z, y^k)$
  3. *Khối cập nhật Dual:* $y^{k+1} = y^k + \rho(Ax^{k+1} + Bz^{k+1} - c)$
- **Đặc tính Vật lý & Toán học:**
  - *Động lực học Hội tụ:* Sự hội tụ là quá trình tiêu tán năng lượng, đo lường bằng Primal Residual ($r^k \to 0$, mức độ vi phạm vật lý) và Dual Residual ($s^k \to 0$, khoảng cách đến cân bằng).
  - *Quán tính của hệ và Hình phạt Động (Dynamic Penalty $\rho$):* $\rho$ hoạt động như hằng số lò xo cơ học. Nếu $\rho$ tĩnh quá lớn sẽ gây dao động cứng (stiff system), quá nhỏ khiến tốc độ triệt tiêu vi phạm vật lý rất chậm. Khắc phục bằng cơ chế Dynamic Penalty: hệ thống liên tục so sánh trực tiếp Primal Residual ($r^k$) và Dual Residual ($s^k$). Nếu $r^k \gg s^k$, hệ thống tự động tăng $\rho$ để ép chặt sự tuân thủ ràng buộc gốc; ngược lại, nếu $s^k \gg r^k$, hệ thống giảm $\rho$ để mở rộng không gian cho biến đối ngẫu. Sự phối hợp này duy trì cân bằng động lực học và tăng tốc độ hội tụ đáng kể.
  - *Ranh giới Không gian Lồi (Convexity Boundaries):* Bắt buộc $f(x)$ và $g(z)$ phải lồi. Ở vùng phi lồi, hàm Lyapunov mất đi tính suy giảm đơn điệu, dễ mắc kẹt ở cực tiểu cục bộ (local minima).

## 2. Tương tác Cục bộ (Local Interaction View)
- **Người đảm nhiệm (Assignee):** grid_interaction_researcher
- **Hệ tham chiếu (Container):** Hệ thống Giao tiếp Multi-Agent / Mạng Viễn thông (Communication Network).
- **Vai trò Tương tác:** ADMM đóng vai trò là "Thực thể trao đổi trạng thái". Môi trường viễn thông là "Bộ điều phối truyền dẫn", áp đặt các giới hạn vật lý logic (trễ, băng thông) lên luồng thông điệp.
- **Luồng Giao tiếp Cục bộ (In/Out Signals):**
  - *Output:* Nút ADMM "bơm" Biến trạng thái cục bộ $x_i^{(k)}$ vào kênh truyền dưới dạng gói dữ liệu (packet) theo chu kỳ.
  - *Input:* ADMM thu nhận lại Biến trạng thái lân cận $x_j^{(k)}$ và Biến đồng thuận $z^{(k)}$ làm "nguyên liệu" cho bước lặp tiếp theo.
- **Hàm truyền tương tác (Transfer Function & Dynamics):**
  - *Động lực học Biến Đồng thuận (Consensus Dynamics):* Biến đối ngẫu tạo ra lực ép qua môi trường mạng kéo trạng thái cục bộ và trạng thái mạng lại gần nhau. Khả năng truyền dẫn của Container ảnh hưởng trực tiếp đến tốc độ lan truyền của lực đàn hồi này.
  - *Tác động Băng thông (Communication Overhead):* Tần số lặp khổng lồ của ADMM xung đột với băng thông hữu hạn. Tắc nghẽn mạng (congestion) sẽ ép ADMM phải hạ thấp tốc độ tính toán (tần số phát output).
  - *Độ trễ và Tính Bất đồng bộ (Latency & Asynchronous Shift):* Kênh truyền hoạt động như một bộ trễ ($e^{-sT}$). Sự chậm trễ (delay) hoặc mất gói (packet loss) buộc ADMM phải tính toán trên dữ liệu "cũ" (outdated input), chuyển mô hình từ đồng bộ (synchronous) sang bất đồng bộ (asynchronous) và làm biến dạng quỹ đạo hội tụ.