---
contributors:
  - H.T.Hải K67
---
Direct Parent Connection: -> [[Algorithm]]

# LOW PASS FILTER (BỘ LỌC THÔNG THẤP — LPF)
*(Nút Thực thể Lớp 2: Thuật toán xử lý tín hiệu miền thời gian chuyên trách lọc và phân tách dao động tần số công suất trong Hệ thống Lưu trữ Năng lượng Hỗn hợp - HESS).*

---

## I. KHÂU ÂM (PURE ENTITY): BẢN CHẤT TOÁN HỌC & LÝ THUYẾT TÍN HIỆU THUẦN TÚY
*(Phân tích độc lập nội tại bởi `pure_entity_researcher.md` — Không tham chiếu môi trường ngoại vi).*

### 1. Hàm truyền trong Miền phức Laplace & Phương trình Vi phân
Bộ lọc thông thấp bậc một (First-Order Low-Pass Filter) được định nghĩa bởi phương trình vi phân tuyến tính hệ số hằng trong miền thời gian liên tục:
$$\tau \frac{dy(t)}{dt} + y(t) = x(t)$$
Trong đó:
- $x(t)$: Tín hiệu đầu vào (Input signal).
- $y(t)$: Tín hiệu đầu ra đã lọc (Filtered output signal).
- $\tau$: Hằng số thời gian của bộ lọc (Time constant, đơn vị: giây).

Chuyển sang miền tần số phức Laplace ($s = \sigma + j\omega$):
$$H(s) = \frac{Y(s)}{X(s)} = \frac{1}{1 + \tau s} = \frac{\omega_c}{s + \omega_c}$$
Với $\omega_c = \frac{1}{\tau} = 2\pi f_c$ là **Tần số cắt góc (Cut-off Frequency)**. Tại $\omega = \omega_c$, biên độ suy giảm đúng $-3 \text{ dB}$ (tức $\frac{1}{\sqrt{2}}$ giá trị đỉnh) và độ trễ pha đạt $\phi(\omega_c) = -45^\circ$.

### 2. Phương trình Sai phân Rời rạc (Discrete-Time DSP Implementation)
Để thực thi trên bộ vi điều khiển số (DSP/FPGA), bộ lọc LPF liên tục được rời rạc hóa bằng phép biến đổi đổi hướng (Backward Euler / Tustin Bilinear Transformation) với chu kỳ lấy mẫu $\Delta t$:
$$y[n] = \alpha x[n] + (1 - \alpha) y[n-1]$$
Hệ số làm mượt $\alpha \in (0, 1)$ được tính bởi:
$$\alpha = \frac{\Delta t}{\tau + \Delta t}$$
- Khi $\alpha \to 0$ ($\tau \gg \Delta t$): Bộ lọc có độ quán tính lớn, loại bỏ triệt để nhiễu cao tần nhưng thời gian đáp ứng chậm.
- Khi $\alpha \to 1$: Tín hiệu đầu ra gần như bám sát tín hiệu đầu vào $y[n] \approx x[n]$.

### 3. Bộ lọc Butterworth Bậc cao (High-Order Butterworth Filter)
Để đạt độ dốc suy hao biên độ cực sâu ở vùng cản (Stop-band) mà không gây nhấp nhô ở vùng thông (Pass-band maximally flat), hàm độ lợi bậc $n$ được biểu diễn bởi:
$$|H(j\omega)|^2 = \frac{1}{1 + \left(\frac{\omega}{\omega_c}\right)^{2n}}$$
Độ suy giảm biên độ đạt $-20n \text{ dB/decade}$, cho phép cô lập hoàn toàn các thành phần phổ tần số vượt ngưỡng $\omega_c$.

---

## II. KHÂU DƯƠNG (LOCAL INTERACTION): CƠ CHẾ ĐIỀU PHỐI TẦN SỐ TRONG HỆ THỐNG HESS
*(Phân tích tương tác cục bộ bởi `grid_interaction_researcher.md` — Môi trường bọc: Hệ thống Điều khiển Lưu trữ Hỗn hợp HESS).*

### 1. Phân tách Tín hiệu Sai lệch Công suất (Power Deviation Decoupling)
Trong hệ thống HESS, bộ điều khiển trung tâm nhận tín hiệu sai lệch công suất tổng cần bù trừ:
$$\Delta P_{req}(t) = P_{load}(t) - P_{gen}(t)$$
Bộ lọc LPF đóng vai trò bộ tách dải tần số trong miền thời gian:
1. **Thành phần Tần số Thấp (Low-Frequency Component):**
   $$\Delta P_{low}(t) = \text{LPF}(\Delta P_{req}(t)) = \mathcal{L}^{-1}\left\{ \frac{1}{1 + \tau s} \cdot \Delta P_{req}(s) \right\}$$
   - Thành phần này có biên độ lớn, tốc độ biến thiên chậm (dao động chu kỳ phút đến giờ).
   - Được giao làm tín hiệu đặt công suất $P_{ref,slow}$ cho các thiết bị có mật độ năng lượng cao nhưng đáp ứng chậm như `[Hydrogen_Storage](obsidian://open?file=Hydrogen_Storage)` hoặc `[BESS](obsidian://open?file=BESS)`.
2. **Thành phần Tần số Cao (High-Frequency Component):**
   $$\Delta P_{high}(t) = \Delta P_{req}(t) - \Delta P_{low}(t)$$
   - Thành phần này chứa các xung công suất đột ngột, biến thiên siêu nhanh (RoCoF, mili-giây đến giây).
   - Được giao làm tín hiệu đặt công suất $P_{ref,fast}$ cho thiết bị mật độ công suất cao như `[Supercapacitor](obsidian://open?file=Supercapacitor)`.

### 2. Tối ưu hóa Hằng số Thời gian $\tau$ (Time-Constant Tuning)
- Việc chọn tần số cắt $f_c = \frac{1}{2\pi \tau}$ quyết định ranh giới chia sẻ công suất giữa Siêu tụ và Pin:
  - Nếu $\tau$ quá nhỏ: Pin BESS phải gánh chịu nhiều dao động cao tần, làm tăng ứng suất dòng điện RMS và đẩy nhanh thoái hóa điện hóa.
  - Nếu $\tau$ quá lớn: Siêu tụ phải duy trì dòng xả trong thời gian dài, dễ dẫn đến cạn kiệt trạng thái sạc (SoC saturation/depletion).
- *Thuật toán điều khiển LPF thích nghi (Adaptive LPF):* Đồng chí **D.M.Hai K67** đề xuất tự động điều chỉnh $\tau$ theo thời gian thực dựa trên độ lệch SoC của Siêu tụ nhằm giữ cân bằng giữa bảo vệ tuổi thọ Pin và an toàn dung lượng Siêu tụ.

---

## III. GHI CHÚ THẢO LUẬN & CHUYÊN GIA
- **Đóng góp chuyên môn:** Đồng chí **D.M.Hai K67** chủ trì phát triển thuật toán lọc miền thời gian cho HESS.
- **Trạng thái tài liệu:** *(AI Agent dự thảo: `pure_entity_researcher.md` & `grid_interaction_researcher.md` — Chờ đồng chí D.M.Hai K67 hiệu chỉnh và thẩm định công thức)*.
