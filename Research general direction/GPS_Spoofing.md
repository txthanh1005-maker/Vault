---
contributors: 
  - D.M.Hai K67
---
Direct Parent Connection: -> [[Outside_Attack]]

# GPS Spoofing Attack
*(Tấn công giả mạo GPS: Chiếm quyền đồng bộ thời gian bằng cách chèn sóng RF giả mạo, làm sai lệch mốc thời gian của cảm biến đo lường)*

## 1. Đặc tính Nội tại (Pure Entity View)
- **Người đảm nhiệm (Assignee):** pure_entity_researcher
- **Bản chất Vật lý (Cơ chế RF):** Chiếm quyền điều khiển vòng khóa pha (PLL) và vòng khóa trễ (DLL) của bộ thu bằng cách phát xạ sóng vô tuyến mang mã giả ngẫu nhiên (PRN) giống hệt vệ tinh thật nhưng bị thao túng năng lượng và độ trễ. 
- **Cấu trúc Toán học:** Tín hiệu giả mạo $S_{sp, i}(t) = \sqrt{2P_{sp, i}} \cdot \hat{D}_i(t - \tau_{sp, i}) \cdot C_i(t - \tau_{sp, i}) \cdot \cos(2\pi (f_c + f_{d, sp}) t + \theta_{sp, i})$. Thông qua thao tác trễ $\tau_{sp, i}$, phương trình khoảng cách giả bị biến dạng thành $\tilde{\rho}_i = \rho_i + \Delta \rho_{sp, i}$.
- **Phân loại Cơ chế:**
  - *Asynchronous/Mù:* Phát xạ không đồng bộ, ép bộ thu rớt khóa rồi ép khóa lại.
  - *Synchronous:* Bám sát đỉnh hàm tương quan $|\tau_{sp}(t_0) - \tau_{gen}(t_0)| \to 0$, tăng nhẹ công suất để "nuốt chửng" tín hiệu thật rồi mới trôi dạt ra xa (Lift-off).
  - *Meaconing:* Ghi lại phổ RF nguyên bản và phát lại có trễ.
- **Giới hạn Nhiệt động lực học & Động học:**
  - *Cận trên Động học:* Gia tốc kéo lệch $\ddot{\tau}_{sp}$ phải bị giới hạn bởi băng thông vòng lặp $B_L$. Lỗi bám pha tĩnh $\theta_e \propto \frac{\ddot{\tau}_{sp}}{\omega_n^3}$ vượt ngưỡng sẽ làm vỡ vòng khóa.
  - *Cận trên Năng lượng:* Sự chênh lệch công suất $\Delta P = 10\log_{10}(P_{sp}/P_{gen})$ không được quá lớn (chỉ nên từ 1-10 dB). Nếu lớn hơn, mạch AGC (Automatic Gain Control) sẽ tự giảm Gain và tố cáo sự hiện diện của Spoofer.
  - *Hạn chế Không gian 3D:* Spoofer phát từ một ăng-ten vật lý duy nhất sẽ vi phạm tính toán học của sự đa dạng hướng tới (Angle of Arrival) từ nhiều vệ tinh thực.

## 2. Tương tác Cục bộ (Local Interaction View)
- **Người đảm nhiệm (Assignee):** grid_interaction_researcher
- **Hệ tham chiếu (Container):** Outside_Attack (Không gian vô tuyến RF / Lớp đồng bộ thời gian ngoài)
- **Tín hiệu In/Out Cục bộ:** 
  - Tín hiệu vào: Tín hiệu gốc $s(t)$ cộng nhiễu nền $n(t)$.
  - Bức xạ tác động: Spoofer phát $s_{spoof}(t)$ với công suất $P_{s,i} > P_i$ để "đè" sóng vệ tinh nguyên bản.
  - Hàm tổng hợp tại Ăng-ten thu: $y(t) = s(t) + s_{spoof}(t) + n(t)$.
- **Động lực học Tương tác:** Tương tác xảy ra qua 2 pha: (1) Pha Đồng bộ (Alignment), thiết lập $\Delta\tau(t_0) \approx 0$ để chèn tín hiệu giả mượt mà; (2) Pha Kéo lệch (Walk-off), dần dần đẩy hàm trôi $\Delta\tau(t)$. Kết quả đầu ra là biến số lỗi thời gian thuần túy $\Delta t_{PMU} \approx \Delta\tau(t)$ thoát khỏi không gian RF và đi vào lớp dữ liệu (Cyber). Quá trình này hoàn toàn không sinh ra công suất phá hoại vật lý trực tiếp.

## 3. Điểm mạnh, Điểm yếu & Ứng dụng (Pros, Cons & Use Cases)
- **Điểm mạnh (Strengths):** Tấn công từ không gian vật lý bên ngoài (Air-gap), không cần xâm nhập qua tường lửa hay mạng IT. Qua mặt hoàn toàn các phần mềm bảo mật mạng truyền thống.
- **Điểm yếu (Weaknesses):** Yêu cầu tiếp cận vật lý gần mục tiêu để duy trì ưu thế công suất (Line-of-Sight). Dễ bị khắc chế bởi hệ thống ăng-ten định hướng chùm tia (CRPA) hoặc thuật toán Anti-Spoofing kiểm tra góc tới (Angle of Arrival).
- **Khi nào dùng (When to use):** Gây nhiễu hệ thống bảo vệ trên diện rộng, làm PMU gán sai nhãn thời gian, từ đó khiến hệ thống SCADA ảo giác về một sự cố mất đồng bộ góc pha và tự động cắt Rơ-le phân rã lưới.
- **Khi nào KHÔNG dùng (When NOT to use):** Tại các cơ sở có trạm thu tín hiệu vệ tinh được bọc lồng Faraday bảo vệ điện từ hoặc giám sát an ninh vòng ngoài nghiêm ngặt.