---
contributors: 
  - D.M.Hai K67
---
Direct Parent Connection: -> [[Algorithm]]

# PI Controller
Bộ điều khiển Tỷ lệ - Tích phân (Proportional-Integral Controller) là một cơ chế điều khiển phản hồi vòng kín cơ bản trong tự động hóa. Nhiệm vụ cốt lõi của PI là tính toán sai số giữa giá trị đo đạc và giá trị đặt trước, sau đó sinh ra tín hiệu điều khiển nhằm triệt tiêu sai số này dựa trên hai tác động độc lập: Tỷ lệ (P) và Tích phân (I).

## 1. Đặc tính Nội tại (Pure Entity View)
*(Phần Âm: Không liên quan đến môi trường bên ngoài)*
- **Người đảm nhiệm (Assignee):** D.M.Hai K67
- **Bản chất toán học (Mathematical Foundation):**
  - Phương trình chi phối miền thời gian:
    $$u(t) = K_p e(t) + K_i \int_{0}^{t} e(\tau) d\tau$$
    Trong đó:
    - $u(t)$: Tín hiệu điều khiển đầu ra (Control output).
    - $e(t) = r(t) - y(t)$: Sai số điều khiển (Error), hiệu số giữa giá trị đặt $r(t)$ và giá trị thực tế $y(t)$.
    - $K_p$: Hệ số tỷ lệ (Proportional gain) - quyết định độ nhạy tức thời.
    - $K_i$: Hệ số tích phân (Integral gain) - quyết định tốc độ triệt tiêu sai số xác lập.
  - Biểu diễn trong miền Laplace (Hàm truyền đạt nội tại):
    $$C(s) = \frac{U(s)}{E(s)} = K_p + \frac{K_i}{s} = K_p \left(1 + \frac{1}{T_i s}\right)$$
    Trong đó $T_i = K_p/K_i$ là hằng số thời gian tích phân.
- **Cơ chế hoạt động học:**
  - **Khâu P (Proportional):** Phản ứng trực tiếp và ngay lập tức với sai số hiện tại. Tuy nhiên, P đứng độc lập luôn để lại sai số xác lập (steady-state error) khác 0.
  - **Khâu I (Integral):** Lấy tích phân sai số theo thời gian. Nó "ghi nhớ" quá khứ và liên tục cộng dồn tín hiệu điều khiển cho đến khi $e(t) = 0$. Đây là gốc rễ tạo nên khả năng triệt tiêu hoàn toàn sai số xác lập của PI.
- **Hiện tượng suy hao/hạn chế nội tại:**
  - **Integral Windup (Bão hòa tích phân):** Khi hệ thống điều khiển chạm ngưỡng giới hạn vật lý (bão hòa), sai số $e(t)$ không thể giảm về 0, khiến khâu I tiếp tục tích lũy vô hạn. Khi hệ thống quay lại trạng thái bình thường, khâu I cần rất nhiều thời gian để "xả" lượng tích lũy này, gây ra vọt lố (overshoot) trầm trọng. Đòi hỏi phải tích hợp thêm thuật toán Anti-windup (Clamping, Back-calculation) vào lõi.
  - **Độ trễ pha (Phase lag):** Khâu I ($\frac{K_i}{s}$) đóng góp một độ trễ pha $-90^\circ$ vào miền tần số, làm giảm biên dự trữ pha (Phase Margin) của vòng hở và tiềm ẩn nguy cơ mất ổn định.

## 2. Tương tác Cục bộ (Local Interaction View)
*(Phần Dương: Chỉ phân tích tương tác với HỆ THỐNG CẤP CAO HƠN BỌC TRỰC TIẾP LẤY NÓ)*
- **Người đảm nhiệm (Assignee):** [Chờ grid_interaction_researcher phân tích sau]
- **Hệ tham chiếu (Container):** Algorithm
- Tín hiệu Đầu vào / Đầu ra giao tiếp với Container: [Chưa phân tích]
- Hàm truyền (Transfer function), tốc độ đáp ứng: [Chưa phân tích]
- Động lực học tương tác: [Chưa phân tích]

## 3. Điểm mạnh, Điểm yếu & Ứng dụng (Pros, Cons & Use Cases)
- **Điểm mạnh (Strengths):**
  - Cấu trúc đơn giản, thuật toán gọn nhẹ, cực kỳ dễ triển khai trên vi điều khiển (MCU) hoặc mạch Analog (Op-amp).
  - Cam kết triệt tiêu 100% sai số xác lập, đảm bảo độ chính xác tuyệt đối ở trạng thái tĩnh.
- **Điểm yếu (Weaknesses):**
  - Mù mờ với tương lai (Không có khâu Derivative để dự báo xu hướng sai số).
  - Khó kiểm soát ổn định khi hệ thống có Time Delay lớn.
- **Khi nào dùng (When to use):** 
  - Điều khiển các đối tượng có quán tính lớn, nhiễu tần số cao hoặc không cần đáp ứng quá nhanh (VD: Nhiệt độ, Mức chất lỏng, Vòng điều khiển dòng điện/điện áp cơ bản của Inverter).
- **Khi nào KHÔNG dùng (When NOT to use):** 
  - Hệ thống cần phản ứng cực nhanh với biến thiên đột ngột, hệ phi tuyến mạnh, hệ thống đa biến (MIMO) phức tạp có nhiều nhiễu.