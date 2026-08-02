---
contributors: ["N.H.Anh k66"]
---
Direct Parent Connection: -> [[Harmonic_Mitigation]]

# Grid_Harmonic_Mitigation (Bù méo hài phía áp lưới AC & Chống cộng hưởng LCL / Grid Voltage Harmonic Rejection)

## I. KHÂU ÂM - NGUYÊN LÝ VẬT LÝ VÀ TOÁN HỌC THUẦN TÚY (PURE ENTITY)
*(AI Agent dự thảo: pure_entity_researcher.md - Chờ N.H.Anh k66 hiệu chỉnh)*

### 1. Phương trình vi phân không gian trạng thái của bộ lọc LCL ngõ ra nghịch lưu
Bộ lọc cấu trúc bậc ba LCL (Inductor-Capacitor-Inductor) là bộ lọc thông thấp được tích hợp tại ngõ ra của các cầu nghịch lưu chuyển mạch tần số cao, có đặc tính suy giảm độ lợi $-60\text{ dB/dec}$ ở miền tần số lớn hơn tần số cắt.

#### a. Hệ phương trình vi phân mô tả mạch lọc LCL
Xét bộ lọc LCL ba pha đối xứng (phân tích trên một pha tương đương) gồm:
- $L_1, R_1$: Điện cảm và điện trở nội mạch phía cuộn cảm nghịch lưu (Inverter-side Inductor).
- $L_2, R_2$: Điện cảm và điện trở nội mạch phía cuộn cảm ngõ ra (Output-side Inductor).
- $C_f$: Điện dung tụ lọc song song (Filter Capacitor).
- $v_i(t)$: Điện áp chuyển mạch ngõ ra cầu nghịch lưu.
- $v_o(t)$: Điện áp tại điểm kết nối ngõ ra của bộ lọc.

Áp dụng định luật Kirchhoff về điện áp (KVL) và dòng điện (KCL), hệ phương trình vi phân liên tục theo thời gian $t$ được định lập:
$$\begin{cases} L_1 \frac{di_1(t)}{dt} = v_i(t) - v_c(t) - R_1 i_1(t) \\ L_2 \frac{di_2(t)}{dt} = v_c(t) - v_o(t) - R_2 i_2(t) \\ C_f \frac{dv_c(t)}{dt} = i_1(t) - i_2(t) \end{cases}$$
Trong đó $i_1(t)$ là dòng điện cuộn cảm phía nghịch lưu, $i_2(t)$ là dòng điện ngõ ra, và $v_c(t)$ là điện áp trên tụ lọc $C_f$.

#### b. Biểu diễn trong không gian trạng thái (State-Space Representation)
Bỏ qua điện trở ký sinh cực nhỏ ($R_1 = R_2 = 0$) để khảo sát động học cộng hưởng thuần túy, đặt véctơ biến trạng thái $\mathbf{x}(t) = \left[ i_1(t), i_2(t), v_c(t) \right]^T$, véctơ tín hiệu vào $\mathbf{u}(t) = \left[ v_i(t), v_o(t) \right]^T$ và véctơ ngõ ra $\mathbf{y}(t) = i_2(t)$. Hệ phương trình không gian trạng thái $\dot{\mathbf{x}}(t) = \mathbf{A}\mathbf{x}(t) + \mathbf{B}\mathbf{u}(t)$ được viết dưới dạng ma trận:
$$\frac{d}{dt} \begin{bmatrix} i_1(t) \\ i_2(t) \\ v_c(t) \end{bmatrix} = \begin{bmatrix} 0 & 0 & -\frac{1}{L_1} \\ 0 & 0 & \frac{1}{L_2} \\ \frac{1}{C_f} & -\frac{1}{C_f} & 0 \end{bmatrix} \begin{bmatrix} i_1(t) \\ i_2(t) \\ v_c(t) \end{bmatrix} + \begin{bmatrix} \frac{1}{L_1} & 0 \\ 0 & -\frac{1}{L_2} \\ 0 & 0 \end{bmatrix} \begin{bmatrix} v_i(t) \\ v_o(t) \end{bmatrix}$$
$$\mathbf{y}(t) = \begin{bmatrix} 0 & 1 & 0 \end{bmatrix} \mathbf{x}(t)$$

#### c. Tần số cộng hưởng riêng và hàm truyền bộ lọc
Thực hiện biến đổi Laplace hệ phương trình vi phân với điều kiện đầu bằng 0, phương trình đặc trưng của hệ thống là đa thức mẫu số của định thức $|s\mathbf{I} - \mathbf{A}| = 0$:
$$\det(s\mathbf{I} - \mathbf{A}) = s \left[ s^2 + \frac{L_1 + L_2}{L_1 L_2 C_f} \right] = 0$$
Tần số góc cộng hưởng tự nhiên (Resonant Frequency) của mạch LCL được xác định tại cặp cực phức thuần ảo trên trục $j\omega$:
$$\omega_{res} = \sqrt{\frac{L_1 + L_2}{L_1 L_2 C_f}} \implies f_{res} = \frac{1}{2\pi} \sqrt{\frac{L_1 + L_2}{L_1 L_2 C_f}}$$
Hàm truyền liên kết từ áp ngõ ra nghịch lưu $v_i(s)$ đến dòng điện ngõ ra $i_2(s)$ (khi ngắn mạch ngõ ra $v_o(s) = 0$):
$$G_{LCL}(s) = \left. \frac{i_2(s)}{v_i(s)} \right|_{v_o=0} = \frac{1}{L_1 L_2 C_f s^3 + (L_1 + L_2)s} = \frac{1}{s(L_1 + L_2) \left( 1 + \frac{s^2}{\omega_{res}^2} \right)}$$
Tại lân cận tần số $\omega = \omega_{res}$, độ lợi hệ hở tiến đến vô cực ($|G_{LCL}(j\omega_{res})| \to \infty$) với chuyển dịch pha gắt $-180^\circ$, gây ra hiện tượng khuếch đại sóng hài tần số cao và dao động mất ổn định nếu không có cơ chế cản dịu (Damping).

---

### 2. Mô hình hóa ma trận dẫn nạp ngõ ra động $Y_{out}(s)$ và trễ thời gian
Khả năng triệt tiêu hoặc kích thích sóng hài tại điểm kết nối phụ thuộc vào dẫn nạp ngõ ra động (Dynamic Output Admittance / Output Impedance Model) nhìn từ cổng ngõ ra nghịch lưu.

```
       v_i*(s)   +--------+      v_i(s)  +-------------+  i_2(s)
----O----------->| G_d(s) |------------->| G_LCL(s)    |--------->
    ^ -          +--------+              +-------------+
    |                                           |
    |            +--------+                     |
    +------------| G_c(s) |<--------------------+
                 +--------+
```

#### a. Hàm truyền tổng hợp của trễ tính toán vi xử lý và điều chế PWM
Trong hệ thống điều khiển số tần số lấy mẫu $f_s = 1/T_s$, tổng độ trễ trong vòng lặp kín bao gồm trễ lấy mẫu/tính toán ($T_{calc} = T_s$) và trễ giữ mẫu điều chế độ rộng xung (PWM Zero-Order Hold, $T_{PWM} \approx 0.5 T_s$). Hàm truyền trễ tổng hợp $G_d(s)$ là một khâu trễ thuần túy:
$$G_d(s) = e^{-s T_d} \quad \text{với } T_d = T_{calc} + T_{PWM} = 1.5 T_s$$
Sử dụng xấp xỉ Padé bậc nhất (First-order Padé Approximation) hoặc định lý chuỗi Taylor cho vùng tần số thấp hơn tần số Nyquist ($\omega < \pi/T_s$):
$$G_d(s) = e^{-s T_d} \approx \frac{1 - \frac{T_d}{2}s}{1 + \frac{T_d}{2}s} \quad \text{hoặc xấp xỉ tần số thấp: } \quad G_d(s) \approx \frac{1}{1 + s T_d}$$

#### b. Dẫn nạp ngõ ra động $Y_{out}(s)$ và phân tích thành phần phần thực âm
Xét bộ điều khiển dòng điện mạch vòng kín với bộ điều khiển $G_c(s)$ bám dòng ngõ ra $i_2(s)$. Định lý chồng chất cho phép biểu diễn phương trình dòng điện ngõ ra:
$$I_2(s) = G_{cl}(s) I_2^*(s) - Y_{out}(s) V_o(s)$$
Ma trận dẫn nạp ngõ ra động $Y_{out}(s) = -\frac{I_2(s)}{V_o(s)} \Big|_{I_2^* = 0}$ được dẫn xuất đầy đủ từ sơ đồ khối hệ thống:
$$Y_{out}(s) = \frac{1 + L_1 C_f s^2}{L_1 L_2 C_f s^3 + (L_1 + L_2)s + G_c(s) G_d(s)}$$
Tại dải tần số trung bình và cao ($\omega_{res} > \frac{\pi}{3 T_s}$), độ lệch pha do khâu trễ $e^{-j\omega T_d}$ vượt quá $-90^\circ$. Khi xét đáp ứng tần số phức của dẫn nạp ngõ ra $Y_{out}(j\omega) = G_{out}(\omega) + j B_{out}(\omega)$, phần thực của dẫn nạp ngõ ra (hoặc điện trở tương đương nhìn từ ngõ ra) bị chuyển sang giá trị âm:
$$\text{Re}\{Y_{out}(j\omega)\} < 0 \quad \text{khi } \omega \in \left( \frac{\pi}{2 T_d}, \frac{\pi}{T_d} \right)$$
Phần thực dẫn nạp âm ($\text{Re}\{Y_{out}(j\omega)\} < 0$) tương đương với một điện trở âm sinh năng lượng dao động hài tần số cao (Harmonic Instability) tại dải tần số cộng hưởng $\omega_{res}$.

---

### 3. Phương trình toán học các kỹ thuật Triệt tiêu cộng hưởng chủ động (Active Damping)
Để triệt tiêu đỉnh cộng hưởng vô cực $\omega_{res}$ và tái cấu trúc phần thực dẫn nạp ngõ ra $\text{Re}\{Y_{out}(j\omega)\} > 0$ mà không gây tổn hao công suất tác dụng như điện trở vật lý $R_d$, các phương pháp Cản dịu Chủ động (Active Damping) được định cấu trúc trong luật điều khiển.

#### a. Cản dịu chủ động bằng phản hồi dòng tụ lọc (Capacitor Current Feedback - CCF)
Kỹ thuật CCF đo hoặc ước lượng dòng điện đi qua tụ lọc $i_c(t) = C_f \frac{dv_c(t)}{dt}$ và phản hồi về bộ tổng điều chế điện áp với hệ số độ lợi tỷ lệ $K_{ad}$:
$$v_i^*(s) = G_c(s) \left[ I_2^*(s) - I_2(s) \right] - K_{ad} I_c(s)$$
Đưa luật điều khiển vào hệ phương trình LCL, phương trình đặc trưng vòng kín mới của hệ thống trở thành:
$$\Delta_{CCF}(s) = L_1 L_2 C_f s^3 + K_{ad} G_d(s) L_2 C_f s^2 + (L_1 + L_2)s + G_c(s) G_d(s) = 0$$
Bỏ qua độ trễ ($G_d(s) \approx 1$) để phân tích nguyên lý, hệ số cản dịu tương đương (Equivalent Damping Ratio) $\zeta_{eq}$ của cặp cực cộng hưởng được định lượng bởi:
$$\zeta_{eq} = \frac{K_{ad}}{2} \sqrt{\frac{C_f L_2}{L_1 (L_1 + L_2)}}$$
Hệ số $K_{ad}$ đóng vai trò toán học chính xác như một điện trở cản dịu ảo nối tiếp với cuộn cảm $L_1$ hoặc song song với tụ $C_f$, triệt tiêu hoàn toàn đỉnh nhọn của biểu đồ Bode tại $\omega_{res}$ mà tổn hao công suất tĩnh bằng 0 ($P_{loss} = 0$).

#### b. Trở kháng ảo / Điện trở ảo (Virtual Resistor / Virtual Impedance Damping)
Phương pháp Trở kháng ảo định hình trực tiếp dẫn nạp ngõ ra thông qua việc thêm một khâu hàm truyền điện trở ảo $R_v(s)$ vào luật điều khiển dòng điện hoặc điện áp trong miền phức:
$$V_{i,mod}(s) = G_c(s) E(s) - Z_v(s) I_2(s) = G_c(s) E(s) - \left[ R_v + s L_v \right] I_2(s)$$
Khi cấu hình Trở kháng ảo song song với tụ lọc (Virtual Resistor in Parallel with Capacitor - $R_{v,p}$), hàm truyền điều chỉnh dẫn nạp ngõ ra vòng kín được biến đổi thành:
$$Y_{out}^{AD}(s) = \frac{1 + L_1 C_f s^2 + \frac{s L_1}{R_{v,p}}}{L_1 L_2 C_f s^3 + \left( \frac{L_1 L_2}{R_{v,p}} + K_{ad} L_2 C_f \right) s^2 + (L_1 + L_2)s + G_c(s) G_d(s)}$$
Sự hiện diện của thành phần trở kháng ảo $R_v(s)$ làm thay đổi góc pha của ma trận dẫn nạp, dịch chuyển vùng pha của $Y_{out}^{AD}(j\omega)$, đảm bảo tiêu chuẩn ổn định thụ động (Passivity Criterion):
$$\text{Re}\{Y_{out}^{AD}(j\omega)\} \ge 0 \quad \forall \omega \in \left[ 0, \frac{\pi}{T_s} \right]$$
Nhờ đó, toàn bộ phổ sóng hài tần số cao tại lân cận $\omega_{res}$ bị cản dịu hoàn toàn theo cơ chế tự ổn định tiệm cận Lyapunov.

---

## II. KHÂU DƯƠNG - TƯƠNG TÁC LƯỚI ĐIỆN YẾU & ỔN ĐỊNH CỘNG HƯỞNG HÀI HỆ THỐNG (LOCAL INTERACTION)
*(AI Agent dự thảo: grid_interaction_researcher.md - Chờ N.H.Anh k66 hiệu chỉnh)*

Khâu Dương của một nghịch lưu có chức năng triệt tiêu sóng hài áp lưới (`[Grid_Harmonic_Mitigation](obsidian://open?file=Grid_Harmonic_Mitigation)`) phân tích chiều sâu động lực học tương tác giữa bộ lọc đầu ra $LCL$ của thiết bị với tổng thể hệ thống tài sản vật lý lưới điện (`[Grid_Physical_Assets](obsidian://open?file=Grid_Physical_Assets)`), đặc biệt khi hòa lưới điện yếu (`[Weak_Grid](obsidian://open?file=Weak_Grid)`) có trở kháng đường dây cao và tích hợp nhiều bộ lọc thụ động/tụ bù tĩnh truyền thống.

### 1. Hiện tượng Dịch chuyển Tần số Cộng hưởng trong Môi trường Lưới Điện Yếu (`[Weak_Grid](obsidian://open?file=Weak_Grid)`)
Khi bộ nghịch lưu nối lưới công suất lớn (như điện gió ngoài khơi `[Offshore_Wind](obsidian://open?file=Offshore_Wind)`, truyền tải dòng một chiều điện áp cao `[HVDC](obsidian://open?file=HVDC)` hoặc trạm lưu trữ `[BESS](obsidian://open?file=BESS)` quy mô lớn) tích hợp vào **Lưới Điện Yếu (`[Weak_Grid](obsidian://open?file=Weak_Grid)`)**, tương tác động lực học trở kháng giữa bộ lọc đầu ra và đường dây phía lưới trở thành yếu tố quyết định độ ổn định của toàn hệ thống.

Lưới điện yếu được mô hình hóa theo cấu trúc Thevenin với tỷ số ngắn mạch thấp ($\text{SCR} = \frac{S_{sc}}{P_{\text{nom}}} < 2.0$) và trở kháng đường dây phía lưới $Z_g(s) = R_g + s L_g$ có giá trị cảm kháng $L_g$ rất cao. Khi nghịch lưu sử dụng bộ lọc đầu ra loại $LCL$ (với cảm kháng phía nghịch lưu $L_1$, tụ lọc $C_f$, và cảm kháng phía lưới $L_2$), tổng trở kháng phía lưới mà hệ thống nhìn thấy tại PCC bị biến đổi:
$$L_{\text{grid,total}} = L_2 + L_g$$
Sự gia tăng của cảm kháng lưới $L_g$ làm **dịch chuyển tần số cộng hưởng riêng** của hệ thống từ giá trị danh định $\omega_{\text{res,nom}}$ (khi lưới vô cùng cứng, $L_g \to 0$) xuống vùng tần số thấp hơn rất nhiều $\omega_{\text{res}}'$:
$$\omega_{\text{res}}' = \sqrt{\frac{L_1 + (L_2 + L_g)}{L_1 (L_2 + L_g) C_f}} < \omega_{\text{res,nom}} = \sqrt{\frac{L_1 + L_2}{L_1 L_2 C_f}}$$
**Hệ quả tương tác vật lý:** Khi tần số cộng hưởng $\omega_{\text{res}}'$ dịch chuyển xuống dải tần số từ $300\text{ Hz} - 1500\text{ Hz}$ (tương đương các bậc hài $h = 7, 11, 13, 17, 19, 23, 25$), điểm cộng hưởng này lọt vào trong băng thông điều khiển dòng điện (Current Control Loop Bandwidth) hoặc băng thông của vòng khóa pha (`[PLL](obsidian://open?file=PLL)`). Sụt giảm độ dự trữ pha (Phase Margin $\text{PM} < 30^\circ$) tại $\omega_{\text{res}}'$ dẫn đến hiện tượng dao động không suy chấn (Undamped Harmonic Resonance) và quá điện áp/quá dòng méo nghiêm trọng tại PCC.

---

### 2. Cơ chế Ngăn chặn Mất ổn định Cộng hưởng Hài (Harmonic Resonance Instability & Spillover)
Hiện tượng mất ổn định cộng hưởng hài (Harmonic Resonance Instability hay Harmonic Spillover) bắt nguồn từ tương tác kín đa hướng giữa 3 thực thể: **Vòng khóa pha (`[PLL](obsidian://open?file=PLL)`) - Vòng điều khiển dòng điện nghịch lưu - Trở kháng nền lưới yếu ($Z_g$)**.

1. **Cơ chế lan truyền tràn hài (Spillover):** Khi dòng hài $\Delta i_g(t)$ phát ra từ nghịch lưu chảy qua cảm kháng lớn $L_g$, nó gây ra méo điện áp tức thời tại PCC: $\Delta v_{\text{PCC}}(t) = L_g \frac{d}{dt}\Delta i_g(t)$. Méo điện áp này thâm nhập vào khâu đo lường của `[PLL](obsidian://open?file=PLL)`, tạo ra dao động góc pha tham chiếu $\Delta \theta(t)$. Góc pha bị nhiễu tiếp tục làm sai lệch phép biến đổi $dq0$, tạo ra các thành phần dòng điều khiển giả (Spurious Current Harmonics) tại các tần số song biên (Sideband Frequencies), kích thích cộng hưởng hệ thống lan rộng.
2. **Giải pháp tương tác Khâu Dương — Tạo dáng Trở kháng và Giảm chấn Tích cực (`[Active_Damping](obsidian://open?file=Active_Damping)`):**
   - Thay vì bổ sung điện trở tắt chấn thụ động (Passive Damping) gây tổn hao công suất nhiệt lớn, nghịch lưu thực hiện phản hồi ảo bằng cơ chế **Giảm chấn Tích cực (`[Active_Damping](obsidian://open?file=Active_Damping)`)** dựa trên phản hồi dòng tụ lọc $i_{cf}$ hoặc điện áp PCC kết hợp bộ lọc thông cao/dẫn pha.
   - Để đảm bảo ổn định toàn cục theo tiêu chuẩn Nyquist tỷ số trở kháng (Impedance-Based Nyquist Stability Criterion), bộ điều khiển Khâu Âm thực hiện tạo dáng trở kháng đầu ra của nghịch lưu ($Z_{\text{out}}(s)$) sao cho thỏa mãn điều kiện tiêu tán năng lượng (Passivity Requirement) trên toàn dải tần số nguy cơ:
   $$\text{Re}\{Z_{\text{out}}(j\omega)\} + \text{Re}\{Z_g(j\omega)\} > 0 \quad \forall \omega \in \left[ \omega_1, \frac{\omega_{sw}}{2} \right]$$
   Điều kiện này đảm bảo tổng trở kháng nhìn từ nút PCC không có phần thực âm (Negative Resistance), triệt tiêu hoàn toàn các cực kín nằm ở nửa mặt phẳng phải (RHP Poles), ngăn chặn hiện tượng tràn sóng hài (Harmonic Spillover) dưới mọi kịch bản biến động trở kháng lưới $Z_g$.

---

### 3. Phối hợp Tương tác giữa Nghịch lưu Bù hài (APF) với Tài sản Lưới truyền thống (`[Grid_Physical_Assets](obsidian://open?file=Grid_Physical_Assets)`)
Trong hệ thống truyền tải và phân phối thực tế, bộ nghịch lưu chức năng bù hài (APF hoặc Multifunctional DER) không hoạt động cô lập mà phải tương tác và phối hợp vận hành với các tài sản lưới truyền thống (`[Grid_Physical_Assets](obsidian://open?file=Grid_Physical_Assets)`) như: **Tụ bù tĩnh (Capacitor Banks), Bộ lọc thụ động LC (Passive Filters), và bộ bù tĩnh FACTS/STATCOM (`[STATCOM](obsidian://open?file=STATCOM)` / `[FACTS](obsidian://open?file=FACTS)`)**.

```
       +-----------------------------------------------------------------------+
       |                         LƯỚI ĐIỆN YẾU / AC GRID                       |
       |                        Z_g = R_g + j*omega*L_g                        |
       +-----------------------------------+-----------------------------------+
                                           |  i_g(t) = Sin chuẩn (THD_i < 5%)
                                           v
       +-----------------------------------+-----------------------------------+
       |                 ĐIỂM KẾT NỐI CHUNG (PCC - 22kV / 110kV)               |
       +-------+---------------------------+---------------------------+-------+
               | i_L(t) (Méo hài)          | i_inv(t)                  | i_pf(t)
               v                           v                           v
     +---------+---------+       +---------+---------+       +---------+---------+
     |   TẢI PHI TUYẾN   |       |  NGHỊCH LƯU APF / |       | BỘ LỌC THỤ ĐỘNG / |
     | (Lò hồ quang, EV, |       |    BESS / STATCOM |       |   TỤ BÙ CAP BANK  |
     |   Chỉnh lưu 12P)  |       |  [Active_Damping] |       |  (LC Shunt Filter)|
     +-------------------+       +-------------------+       +-------------------+
               |                           |                           |
               +--- Tương tác Cộng hưởng --+--- Trở kháng Ảo (Damping)-+
```

1. **Nguy cơ Cộng hưởng Song song và Nối tiếp (Parallel & Series Resonance Risk):**
   - Khi tụ bù tĩnh hoặc bộ lọc LC thụ động kết nối vào thanh cái PCC, dung kháng $C_{pf}$ của tụ bù tương tác với cảm kháng hệ thống $L_g$, tạo ra **điểm cộng hưởng song song (Parallel Resonance Peak)** tại tần số $\omega_p = \frac{1}{\sqrt{L_g C_{pf}}}$.
   - Nếu nghịch lưu APF hoặc các tải phi tuyến tiêm dòng sóng hài có tần số trùng hoặc lân cận với $\omega_p$, tổng trở kháng song song cực đại sẽ gây ra khuếch đại điện áp méo hài ($V_{\text{PCC},h} = Z_{\parallel,h} I_{\text{inj},h} \gg 1.0\text{ p.u.}$). Quá áp và quá dòng hài cường độ cao dẫn đến nổ van tụ bù, phá hủy van chống sét (Surge Arresters) và đánh thủng cách điện máy biến áp.

2. **Cơ chế Phối hợp Lọc Lai Tích cực - Thụ động (Hybrid Active-Passive Coordinated Mitigation):**
   - **Phân chia không gian tần số (Frequency-Domain Decoupling):** Bộ lọc thụ động LC được thiết kế và định chuẩn để hấp thụ các bậc hài có năng lượng lớn, tần số cố định bậc thấp ($h = 5, 7$) nhằm giảm tải công suất định mức cho van `[SiC_MOSFET](obsidian://open?file=SiC_MOSFET)` của bộ nghịch lưu.
   - **Đóng vai trò Trở kháng Giảm chấn Tích cực (Virtual Harmonic Conductance Damper):** Tại các tần số cộng hưởng song song $\omega_p$ do tụ bù truyền thống gây ra, bộ nghịch lưu APF / `[STATCOM](obsidian://open?file=STATCOM)` được lập trình chiến lược điều khiển **"Đảo pha tổng trở" (Active Harmonic Conductance Command)**. Nghịch lưu mô phỏng một điện trở giảm chấn ảo (Virtual Damping Resistor $R_v$) đấu song song tại PCC đối với các tần số cao:
     $$\mathbf{i}_{\text{inv},h}^* = G_v \cdot \mathbf{v}_{\text{PCC},h} = \frac{1}{R_v} \cdot \mathbf{v}_{\text{PCC},h}$$
   - Sự xuất hiện của độ dẫn tích cực ảo $G_v$ triệt tiêu hoàn toàn đỉnh cộng hưởng song song của hệ thống $L_g - C_{pf}$, làm bẹt biên độ trở kháng thanh cái (Impedance Flattening), bảo vệ tuyệt đối an toàn cho các cụm Tụ bù tĩnh, Máy biến áp và thiết bị bảo vệ rơ-le trong toàn bộ môi trường `[Grid_Physical_Assets](obsidian://open?file=Grid_Physical_Assets)`.

---

## III. ĐẠO - TRIẾT LÝ VẬT LÝ & KIẾN TRÚC HỆ THỐNG
*(AI Agent dự thảo: Meta-Agent - Chờ N.H.Anh k66 hiệu chỉnh)*

1. **Sự hợp nhất của Âm và Dương trong Chống cộng hưởng Áp Lưới:**
   Khâu Âm chỉ ra gốc rễ của bất ổn định tại tần số cao chính là phần thực dẫn nạp ngõ ra bị chuyển sang âm ($\text{Re}\{Y_{out}(j\omega)\} < 0$) do trễ thời gian trong tính toán và chuyển mạch bán dẫn. Khâu Dương mang nhận thức đó vào môi trường Lưới điện yếu (`[Weak_Grid](obsidian://open?file=Weak_Grid)`), giải quyết sự dịch chuyển tần số $\omega_{res}'$ và ngăn chặn sự tàn phá của các đỉnh cộng hưởng song song với tụ bù tĩnh bằng Giảm chấn tích cực (`[Active_Damping](obsidian://open?file=Active_Damping)`).
2. **Triết lý từ "Đối đầu" sang "Đồng hóa trở kháng":**
   Khi lưới yếu ($SCR < 2.0$), việc cố gắng bám áp một cách vô điều kiện sẽ đẩy nghịch lưu vào dao động tràn hài (Harmonic Spillover). Nghịch lưu khôn ngoan không chọn đối đầu, mà tự "biến hình" trở kháng đầu ra của chính mình ($R_v, Z_{out}(s)$) để duy trì tính thụ động tiêu tán năng lượng (Passivity), hài hòa với mọi biến động trở kháng đường dây $Z_g$.
3. **Ý nghĩa Bản thể trong Vũ trụ Lưới điện:**
   `[Grid_Harmonic_Mitigation](obsidian://open?file=Grid_Harmonic_Mitigation)` là lá chắn bảo vệ sự toàn vẹn phổ tần số của toàn bộ Trụ cột `[Grid_Physical_Assets](obsidian://open?file=Grid_Physical_Assets)`, giúp biến tần từ một "kẻ tiêu thụ/phát thải sóng hài tiềm ẩn" trở thành chiếc neo giữ vững sự trong sạch cho dạng sóng điện áp lưới.
