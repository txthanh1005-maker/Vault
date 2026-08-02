---
contributors: ["N.H.Anh k66"]
---
Direct Parent Connection: -> [[Inverter]]

# Grid_Forming (Nghịch lưu Tạo lưới tự trị - VSM & Droop Control)

## I. KHÂU ÂM - NGUYÊN LÝ VẬT LÝ VÀ TOÁN HỌC THUẦN TÚY (PURE ENTITY)
*(AI Agent dự thảo: pure_entity_researcher.md - Chờ N.H.Anh k66 hiệu chỉnh)*

### 1. Phương trình vi phân Máy phát đồng bộ ảo (Virtual Synchronous Machine - VSM) và Swing Equation

#### a. Phương trình dao động cơ cơ bản (Swing Equation)
Bộ nghịch lưu tạo áp tự trị VSM thiết lập đặc tính động lực học của một máy phát đồng bộ ảo thông qua phương trình dao động cơ bản, mô phỏng mô-men quán tính ảo $J_v$ (Virtual Inertia) và hệ số cản ảo $D_v$ (Virtual Damping):
$$J_v \frac{d\omega_v(t)}{dt} = T_m(t) - T_e(t) - D_v \left( \omega_v(t) - \omega_0 \right)$$
Nhân cả hai vế với tần số góc danh định $\omega_0$ để chuyển từ phương trình mô-men sang phương trình công suất:
$$J_v \omega_0 \frac{d\omega_v(t)}{dt} = P_0 - P_e(t) - D_v \omega_0 \left( \omega_v(t) - \omega_0 \right)$$
trong đó:
- $P_0, P_e$: Công suất tác dụng tham chiếu định mức và công suất tác dụng điện ngõ ra tức thời.
- $\omega_v, \omega_0$: Tần số góc ảo nội tại của VSM và tần số góc danh định (rad/s).
- $J_v$: Mô-men quán tính ảo ($\text{kg}\cdot\text{m}^2$).
- $D_v$: Hệ số cản suy giảm dao động cơ ảo ($\text{N}\cdot\text{m}\cdot\text{s/rad}$).

#### b. Phương trình động học góc pha và từ thông ảo ($\psi_v$)
Góc pha quay nội tại $\theta_v(t)$ được sinh ra trực tiếp từ tích phân tần số góc ảo:
$$\frac{d\theta_v(t)}{dt} = \omega_v(t) \implies \theta_v(t) = \int_{0}^{t} \omega_v(\tau) d\tau + \theta_v(0)$$

Phương trình cân bằng từ thông ảo $\psi_v(t)$ và luật điều chỉnh kích từ ảo (Virtual Excitation Regulation) được mô tả bởi phương trình vi phân bậc nhất của bộ ổn áp:
$$\frac{d\psi_v(t)}{dt} = \frac{1}{\tau_f} \left[ \psi_0 + K_q \left( Q_0 - Q_e(t) \right) - \psi_v(t) \right]$$
hoặc mô hình điều khiển trực tiếp độ dốc biên độ điện áp:
$$E_v(t) = \omega_v(t) \psi_v(t) = E_0 - n_q \left( Q_e(t) - Q_0 \right)$$
Vector điện áp xoay chiều ba pha tham chiếu ngõ ra được tổng hợp nội tại:
$$\mathbf{e}_v(t) = \begin{bmatrix} e_{va}(t) \\ e_{vb}(t) \\ e_{vc}(t) \end{bmatrix} = E_v(t) \begin{bmatrix} \cos(\theta_v(t)) \\ \cos\left(\theta_v(t) - \frac{2\pi}{3}\right) \\ \cos\left(\theta_v(t) + \frac{2\pi}{3}\right) \end{bmatrix}$$

---

### 2. Phương trình điều khiển đặc tính độ dốc (Droop Control P-f và Q-V) cho mạng trở kháng tổng quát

#### a. Phương trình truyền đạt công suất qua tổng trở tổng quát
Xét dòng công suất tức thời truyền từ nguồn điện áp ảo $V_1 \angle \theta_1$ qua đường dây có tổng trở $Z \angle \theta_z = R + jX$ đến điểm chung có điện áp $V_2 \angle \theta_2$. Đặt góc lệch pha $\delta = \theta_1 - \theta_2$.
Công suất tác dụng $P_e$ và phản kháng $Q_e$ truyền qua trở kháng:
$$P_e = \frac{V_1 V_2}{Z} \cos(\theta_z - \delta) - \frac{V_2^2}{Z} \cos\theta_z$$
$$Q_e = \frac{V_1 V_2}{Z} \sin(\theta_z - \delta) - \frac{V_2^2}{Z} \sin\theta_z$$

#### b. Luật điều khiển Droop với mạng cảm kháng ưu thế ($X \gg R, \theta_z \approx 90^\circ$)
Khi cảm kháng đường dây áp đảo ($R \approx 0, Z \approx X$), các phương trình công suất giản lược thành:
$$P_e \approx \frac{V_1 V_2}{X} \sin\delta, \quad Q_e \approx \frac{V_1 V_2 \cos\delta - V_2^2}{X}$$
Do $\delta$ nhỏ ($\sin\delta \approx \delta, \cos\delta \approx 1$), công suất tác dụng $P_e$ phụ thuộc tuyến tính vào góc lệch pha $\delta$ (tức tần số $\omega$), và công suất phản kháng $Q_e$ phụ thuộc tuyến tính vào chênh lệch biên độ điện áp.
**Phương trình Droop chuẩn ($P-f$ và $Q-V$):**
$$\omega_v = \omega_0 - m_p \left( P_e - P_0 \right)$$
$$V_v = V_0 - n_q \left( Q_e - Q_0 \right)$$
trong đó $m_p \ (\text{rad}/(\text{s}\cdot\text{W}))$ là hệ số suy giảm tần số theo công suất tác dụng, và $n_q \ (\text{V}/\text{VAr})$ là hệ số suy giảm điện áp theo công suất phản kháng.

#### c. Luật điều khiển Droop với mạng trở kháng ưu thế ($R \gg X, \theta_z \approx 0^\circ$)
Khi điện trở thuần áp đảo ($X \approx 0, Z \approx R$), các phương trình công suất chuyển đổi thành:
$$P_e \approx \frac{V_1 V_2 \cos\delta - V_2^2}{R}, \quad Q_e \approx -\frac{V_1 V_2}{R} \sin\delta$$
Trong trường hợp này, sự ghép chéo bị đảo ngược: công suất tác dụng $P_e$ phụ thuộc vào hiệu điện áp, còn công suất phản kháng $Q_e$ phụ thuộc vào góc lệch pha $\delta$.
**Phương trình Droop ngược (Inverse Droop / $P-V$ và $Q-f$):**
$$V_v = V_0 - m_v \left( P_e - P_0 \right)$$
$$\omega_v = \omega_0 + n_f \left( Q_e - Q_0 \right)$$
với $m_v, n_f$ là các hệ số suy giảm tĩnh cho mạng điện trở thuần.

#### d. Ma trận biến đổi Droop cho trở kháng góc $\theta_z$ bất kỳ (Universal Droop Decoupling)
Để giải trừ ghép chéo $P-Q$ (Decoupling) trên mạng có tỷ lệ $X/R$ bất kỳ, sử dụng biến đổi ma trận xoay công suất ảo $\left[ P', Q' \right]^T$:
$$\begin{bmatrix} P' \\ Q' \end{bmatrix} = \mathbf{T}(\theta_z) \begin{bmatrix} P_e \\ Q_e \end{bmatrix} = \begin{bmatrix} \sin\theta_z & -\cos\theta_z \\ \cos\theta_z & \sin\theta_z \end{bmatrix} \begin{bmatrix} P_e \\ Q_e \end{bmatrix}$$
Luật Droop tổng quát sau biến đổi decoupling:
$$\omega_v = \omega_0 - m_p \left( P' - P'_0 \right), \quad V_v = V_0 - n_q \left( Q' - Q'_0 \right)$$

---

### 3. Cấu trúc bộ tạo dao động điều khiển áp (VCO) tự lập góc pha và tần số không phụ thuộc PLL (PLL-Less Autonomous Operation)

#### a. Cơ chế tự lập góc pha nội tại thông qua VCO
Khác với các cấu trúc bám góc pha ngoại lai (Grid-Following), bộ nghịch lưu tạo lưới tự trị (Grid-Forming) thay thế hoàn toàn vòng khóa pha (PLL) bằng bộ dao động điều khiển áp nội tại (Voltage-Controlled Oscillator - VCO).

Phương trình trạng thái của VCO tự lập:
$$\frac{d\mathbf{\Theta}_v(t)}{dt} = \mathbf{A}_{\text{vco}} \mathbf{\Theta}_v(t) + \mathbf{B}_{\text{vco}} \Delta \omega_v(t)$$
với $\mathbf{\Theta}_v(t) = \left[ \theta_v(t), \omega_v(t) \right]^T$ là vector trạng thái góc pha và tần số ảo, và phương trình tích phân pha chuẩn:
$$\theta_v(t) = \theta_0 + \int_{0}^{t} \left[ \omega_0 - m_p \left( P_e(\tau) - P_0 \right) \right] d\tau$$

```
+-------------------+      e_v(t)       +-------------------+
|                   |------------------>|                   |
|   VCO Nội Tại     |  (Nguồn Áp Ảo)    |  Bộ Lọc LC Ngõ Ra |----> i_o(t) (Dòng Tải)
| (Angle Generator) |                   |    (L - C - L)    |
|                   |<------------------|                   |
+-------------------+   v_c(t), i_o(t)  +-------------------+
          ^                  |
          |                  |  (Đo lường tức thời v_c, i_o)
          |                  v
+---------------------------------------+
|  Khối Tính Toán Công Suất Tức Thời    |
|  P_e(t) = v_{cd} i_{od} + v_{cq} i_{oq} |
+---------------------------------------+
```

#### b. Chứng minh toán học sự ổn định không phụ thuộc PLL (PLL-Less Structural Invariance)
1. **Loại bỏ cực động học của PLL (Elimination of PLL Dynamic Poles):**
   Trong hệ thống có PLL, góc định hướng hệ tọa độ $d-q$ bị phụ thuộc vào điện áp đo lường $\mathbf{v}_c(t)$, tạo ra vòng phản hồi phi tuyến $\theta_{\text{pll}} = H_{\text{pll}}(s) v_{cq}(s)$.
   Trong cấu trúc VCO tự lập, góc tọa độ nội tại $\theta_v$ là **biến độc lập** được sinh ra từ chính động học Droop/VSM:
   $$\frac{\partial \theta_v}{\partial v_{cq}} = 0 \implies H_{\text{pll}}(s) \equiv 0$$
2. **Độc lập trở kháng ngõ ra và loại bỏ dẫn nạp âm (Zero Negative Admittance):**
   Do không có thành phần nhiễu động góc pha từ PLL ($\Delta \theta = 0$), ma trận dẫn nạp ra động $\mathbf{Y}_{\text{vco}}(s)$ của bộ nghịch lưu trên hệ tọa độ tự lập $\theta_v$ hoàn toàn không bị lệch trục:
   $$\mathbf{Y}_{\text{vco}}(s) = \mathbf{Z}_{\text{out}}^{-1}(s) = \left( s\mathbf{L}_f + \mathbf{R}_f \right)^{-1} \mathbf{I}_2$$
   Toàn bộ các phần tử đường chéo của $\mathbf{Y}_{\text{vco}}(s)$ là hàm hữu cực thực dương (Positive Real Functions - PRF):
   $$\text{Re}\left\{ Y_{dd}(j\omega) \right\} > 0, \quad \text{Re}\left\{ Y_{qq}(j\omega) \right\} > 0 \quad \forall \omega \in \mathbb{R}$$
   Do đó, hệ thống tự thiết lập nguồn áp ảo tự trị hoàn toàn loại bỏ nguy cơ mất ổn định tần số cao gây ra bởi tương tác PLL - trở kháng (PLL-Induced High-Frequency Instability) theo tiêu chuẩn ổn định Nyquist/Lyapunov.

---

## II. KHÂU DƯƠNG: TƯƠNG TÁC NGOẠI VI & ĐIỀU PHỐI HỆ THỐNG (LOCAL INTERACTION)
*(AI Agent dự thảo: grid_interaction_researcher.md - Chờ N.H.Anh k66 hiệu chỉnh)*

Khâu Dương của một Nghịch lưu Tạo lưới (`[Grid_Forming](obsidian://open?file=Grid_Forming)` - GFM) phân tích chiều sâu tương tác vật lý, trường điện từ và thông lượng động lực học giữa nguồn điện tử công suất đóng vai trò "tạo nguồn áp" với tổng thể các tài sản vật lý lưới điện (`[Grid_Physical_Assets](obsidian://open?file=Grid_Physical_Assets)`), bao gồm máy phát đồng bộ truyền thống, đường dây truyền tải HVDC (`[HVDC](obsidian://open?file=HVDC)`), hệ thống lưu trữ lai (`[HESS](obsidian://open?file=HESS)`), và các thiết bị bù động (`[STATCOM](obsidian://open?file=STATCOM)` / `[FACTS](obsidian://open?file=FACTS)`).

### 1. Tương tác Vật lý với Lưới Điện khi Sự cố Sụt Tần số Đột ngột (`RoCoF Mitigation` & `Synthetic Inertia`)
Khi hệ thống lưới truyền tải/phân phối xảy ra sự cố mất máy phát lớn, nhảy tải sự cố hoặc nhảy đường dây truyền tải trọng yếu, sự suy giảm cơ năng trên các trục máy phát đồng bộ dẫn đến suy giảm tần số toàn hệ thống. Nghịch lưu GFM, được điều khiển mô phỏng Máy phát Đồng bộ Ảo (`[Virtual_Synchronous_Machine](obsidian://open?file=Virtual_Synchronous_Machine)` - VSM), đóng vai trò cấu trúc nâng đỡ quán tính tức thời cho toàn lưới.

#### a. Cơ chế Ghìm Tốc độ Suy giảm Tần số (RoCoF Mitigation - Rate of Change of Frequency)
* Khi xảy ra sai lệch công suất $\Delta P = P_{m} - P_{e}$ trên hệ thống có tổng công suất danh định $S_{sys}$ và hằng số quán tính hệ thống $H_{sys}$, tốc độ thay đổi tần số ban đầu (RoCoF) được xác định theo phương trình:

$$\text{RoCoF} = \left. \frac{df}{dt} \right|_{t=0^+} = \frac{f_0 \Delta P}{2 H_{sys} S_{sys}} \quad (\text{Hz/s})$$

* Khác với bộ nghịch lưu bám lưới `[Grid_Following](obsidian://open?file=Grid_Following)` (GFL) phải đợi vòng lặp khóa pha (`[Phase_Locked_Loop](obsidian://open?file=Phase_Locked_Loop)` - PLL) đo lường sự thay đổi tần số rồi mới phản hồi (độ trễ $20\,\text{ms} - 50\,\text{ms}$), **GFM giữ cố định vector sức điện động ảo ngõ ra $\vec{E}_{vsm}$ trong khoảnh khắc sub-cycle (< 5 ms)**.
* Sự sụt giảm tức thời của góc pha điện áp lưới $\theta_{g}$ so với góc pha ảo $\theta_{m}$ của GFM tạo ra một chênh lệch góc tải $\delta = \theta_{m} - \theta_{g}$, lập tức đẩy một dòng công suất tác dụng tự nhiên vào lưới mà không cần bất kỳ bộ trích mẫu tần số nào:

$$\Delta P_{GFM} = \frac{E_{vsm} V_{g}}{X_{L}} \sin(\delta_0 + \Delta \delta) - P_{0} \approx \frac{E_{vsm} V_{g}}{X_{L}} \Delta \delta$$

* **Hiệu ứng kìm hãm RoCoF:** Lượng công suất bơm tức thời này làm giảm giá trị sai lệch tải $\Delta P$ hiệu dụng trên toàn lưới, ép giá trị $|\text{RoCoF}|$ nằm dưới ngưỡng cắt rơ-le bảo vệ chống rã lưới (theo chuẩn ENTSO-E: $|\text{RoCoF}|_{max} \le 1.0\,\text{Hz/s}$ với dải đo 500 ms, hoặc $\le 2.0\,\text{Hz/s}$ cho hệ thống vi lưới/lưới đảo nhỏ).

#### b. Cơ chế Bơm Quán tính Tức thời (Synthetic / Virtual Inertia Injection)
* Phương trình động lực học chuyển động quay rotor ảo (Swing Equation) trong thuật toán VSM của nghịch lưu GFM định nghĩa mối tương tác năng lượng giữa cơ năng ảo và điện năng thực tế:

$$J_{vsm} \omega_{0} \frac{d\omega_{m}}{dt} = P_{ref} - P_{e} - D_{vsm} (\omega_{m} - \omega_{g})$$

$$\text{hay: } \quad 2 H_{vsm} S_{rated} \frac{d(\Delta \omega_{m})}{dt} = \Delta P_{ref} - \Delta P_{e} - D_{vsm} \Delta \omega_{m}$$

* Trong đó:
  * $J_{vsm}$ là mô-men quán tính ảo ($\text{kg}\cdot\text{m}^2$), tương ứng với hằng số quán tính ảo $H_{vsm} = \frac{J_{vsm} \omega_0^2}{2 S_{rated}}$ (thường được lập trình linh hoạt trong dải $H_{vsm} \in [2.0, \ 8.0]\,\text{giây}$).
  * $D_{vsm}$ là hệ số cản ảo (Virtual Damping Factor), có nhiệm vụ dập tắt các dao động công suất tần số thấp (Low-Frequency Oscillations - LFO, dải $0.2\,\text{Hz} - 2.5\,\text{Hz}$) giữa GFM và các máy phát đồng bộ trong lưới `[Grid_Physical_Assets](obsidian://open?file=Grid_Physical_Assets)`.
* **Nguồn cung cấp năng lượng vật lý cho quán tính ảo:** Để thực hiện việc bơm công suất quán tính $\Delta P_{inertia} = -2 H_{vsm} S_{rated} \frac{d\omega_g}{dt}$, nghịch lưu GFM phải trích xuất năng lượng thực từ hệ thống lưu trữ điện lai `[HESS](obsidian://open?file=HESS)` (siêu tụ điện đáp ứng xung công suất tức thời + pin `[BESS](obsidian://open?file=BESS)` duy trì năng lượng) hoặc giải phóng năng lượng tĩnh điện lưu trữ trên tụ DC-link ($C_{dc}$), cho phép điện áp bus DC dao động trong dải cho phép ($\pm 5\% V_{dc,rated}$).

---

### 2. Khả năng Hỗ trợ Điện áp Sơ cấp và Điều tần Sơ cấp (FFR & Primary Voltage Support)
Nghịch lưu GFM là nền tảng chịu trách nhiệm giữ ổn định điện áp và tần số cho các hệ thống lưới có tỷ lệ thâm nhập năng lượng tái tạo cao (lưới yếu - Weak Grid với tỷ số ngắn mạch $SCR < 2.0$).

#### a. Điều tần Sơ cấp & Phản hồi Tần số Nhanh (Fast Frequency Response - FFR)
* Theo yêu cầu của bộ quy tắc lưới điện hiện đại (National Grid UK GC0137, EirGrid, AEMO Australia), dịch vụ **FFR (Fast Frequency Response)** yêu cầu nguồn phát phát ra công suất đáp ứng cực nhanh nhằm chặn đứng điểm rớt tần số thấp nhất (Nadir Frequency) trước khi rơ-le sa thải phụ tải dưới tần số (UFLS) bị kích hoạt.
* **Thời gian đáp ứng của GFM:** Trong khi bộ điều tốc cơ học của nhà máy nhiệt điện/thủy điện mất $1.0 - 5.0\,\text{giây}$ để tăng công suất, GFM cung cấp phản hồi FFR đạt 100% công suất yêu cầu trong vòng **$\le 100\,\text{ms}$** (hiệu năng điển hình đạt **$20\,\text{ms} - 40\,\text{ms}$**).
* **Đặc tính kiểm soát tĩnh độ dốc tần số ($P-f$ Droop Control):**

$$\Delta P_{FFR} = - \frac{1}{R_{d}} (\Delta f - \Delta f_{db}) \cdot P_{rated}$$

* $R_{d}$ là độ dốc tĩnh (Droop Regulation), tiêu chuẩn lưới hiện đại quy định $R_{d} \in [2.0\%, \ 5.0\%]$.
* $\Delta f_{db}$ là vùng chết tần số (Frequency Deadband), được thiết lập rất hẹp tại $\pm 10\,\text{mHz} - \pm 15\,\text{mHz}$ để duy trì độ nhạy điều tần.

#### b. Hỗ trợ Điện áp Sơ cấp (Primary Voltage Control / Dynamic Reactive Power)
* Tại điểm ghép nối chung `[Point_of_Common_Coupling](obsidian://open?file=Point_of_Common_Coupling)`, GFM tương tác như một bộ điều áp tự động (AVR) lý tưởng. Lượng công suất phản kháng bơm vào/hấp thụ khỏi lưới tuân theo luật điều khiển độ dốc điện áp $Q-V$:

$$\Delta Q = - \frac{1}{k_{q}} (V_{PCC} - V_{ref}) \cdot Q_{rated} \quad \text{hoặc} \quad E_{vsm} = E_{0} - n_{q} (Q - Q_{0})$$

* **Sự phối hợp với thiết bị bù truyền thống (`[STATCOM](obsidian://open?file=STATCOM)` / `[FACTS](obsidian://open?file=FACTS)`):**
  * Tại các nút lưới có tụ bù dọc hoặc thiết bị FACTS, phản ứng bù điện áp quá nhanh của STATCOM có thể gây dao động tương tác hài tần số cao với bộ lọc LC của GFM.
  * GFM giải quyết triệt để vấn đề này bằng cách tạo ra một điểm tựa điện áp ổn định với trở kháng ngõ ra thấp (Low Thevenin Output Impedance), làm tăng điện áp ngắn mạch nút và nâng hệ số SCR của hệ thống lên $SCR > 3.0$, cho phép các thiết bị STATCOM và nghịch lưu PV (`[PV](obsidian://open?file=PV)`) GFL hoạt động mà không bị mất đồng bộ PLL.

---

### 3. Khả năng Xuyên qua Sự cố Điện áp Thấp (LVRT) và Đóng góp Dòng Ngắn mạch
Khả năng sinh tồn và hỗ trợ lưới truyền tải trong điều kiện sự cố ngắn mạch nghiêm trọng là ranh giới kỹ thuật lớn nhất phân biệt giữa GFM và GFL.

#### a. Khả năng Xuyên qua Sự cố Điện áp Thấp (`[Fault_Ride_Through](obsidian://open?file=Fault_Ride_Through)` - LVRT/ZVRT)
* **Quy chuẩn Grid Codes hiện đại (IEEE 2800-2022 / VDE-AR-N 4120 / ENTSO-E):**
  * Khi điện áp lưới tại PCC giảm sâu xuống **$0\% V_{nominal}$** (sự cố ngắn mạch ba pha chạm đất - Zero Voltage Ride-Through ZVRT), nghịch lưu GFM **tuyệt đối không được ngắt kết nối** trong khoảng thời gian ít nhất **$150\,\text{ms}$**.
  * Nếu điện áp phục hồi về mức $\ge 85\% V_{nominal}$ sau $150\,\text{ms}$, GFM phải tiếp tục vận hành liền mạch mà không bị nhảy bảo vệ quá dòng.
* **Tương tác Vật lý & Thách thức Quá dòng trong GFM:**
  * Do GFM vận hành theo cơ chế nguồn áp (Voltage Source), khi điện áp lưới $V_{g} \to 0$, chênh lệch điện áp lớn giữa sức điện động ảo $\vec{E}_{vsm}$ và điện áp lưới $\vec{V}_{g}$ sẽ tạo ra dòng tràn cực đại qua cuộn cảm lọc L:

$$\vec{I}_{fault} = \frac{\vec{E}_{vsm} - \vec{V}_{g}}{j \omega L_{f} + R_{f}} \approx \frac{\vec{E}_{vsm}}{j \omega L_{f}} \gg I_{rated}$$

  * Do van bán dẫn (IGBT / SiC MOSFET) có quán tính nhiệt cực nhỏ và chỉ chịu được dòng quá tải cao gấp **$1.2 - 1.5 \, I_{rated}$**, GFM không thể phát dòng ngắn mạch lớn gấp $5 - 8$ lần định mức như máy phát truyền thống.
* **Cơ chế Hạn chế Dòng Thông minh (Virtual Impedance & Threshold-based Current Limiting):**
  * Khi phát hiện $|I_{g}| > I_{thresh}$ (ngưỡng 1.1 pu), vòng điều khiển Khâu Dương của GFM tự động kích hoạt **Trở kháng Ảo ngắn hạn (Transient Virtual Impedance - $Z_{v,fault}$)** hoặc chuyển vùng bộ điều khiển sang chế độ bão hòa dòng định hướng áp (Voltage-Oriented Current Saturation):

$$\vec{E}_{cmd} = \vec{E}_{vsm} - Z_{v,fault} \vec{I}_{g} \quad \text{với } Z_{v,fault} = f(|I_{g}| - I_{rated})$$

  * Thuật toán này hạn chế đỉnh dòng điện tức thời dưới ngưỡng an toàn bán dẫn ($1.3 - 1.5 \, pu$) trong vòng chưa tới **$1.5\,\text{ms}$** (sub-cycle transient limitation) mà vẫn duy trì góc pha của nguồn áp ảo để không bị mất đồng bộ sau khi rơ-le cắt sự cố.

#### b. Đóng góp Dòng Ngắn mạch (Short-Circuit Current Contribution & FFCI)
* Trong suốt quá trình xảy ra sự cố ngắn mạch (Fault Active Period), GFM có nghĩa vụ bơm dòng điện ngắn mạch hỗ trợ bảo vệ lưới theo tiêu chuẩn **FFCI (Fast Fault Current Injection)**:
  * **Thời gian kích hoạt dòng hỗ trợ:** Đạt độ lớn dòng yêu cầu trong vòng **$\le 20\,\text{ms}$** (khoảng 1 chu kỳ lưới) kể từ thời điểm sụt áp.
  * **Đặc tính dòng công suất phản kháng bơm lưới:** Dòng ngắn mạch phản kháng $\Delta I_Q$ bơm ra được lập trình tỷ lệ thuận với độ sâu sụt áp:

$$\Delta I_Q = k_{vq} \cdot \left( \frac{V_0 - V_{PCC}}{V_0} \right) \cdot I_{rated}, \quad \text{với } k_{vq} \in [2.0, \ 4.0]$$

  * Nếu điện áp sụt dưới $50\%$, GFM phải bơm $100\%$ dòng định mức toàn bộ là dòng phản kháng cảm tính ($I_Q = 1.0 \, pu$, lùi pha $90^\circ$ so với điện áp) để duy trì hướng dòng ngắn mạch rõ ràng.
* **Tương tác với Hệ thống Rơ-le Bảo vệ Lưới (Protective Relay Coordination):**
  * Các rơ-le quá dòng có hướng (Directional Overcurrent - 67) và rơ-le khoảng cách (Distance Relay - 21) trên lưới truyền tải `[Grid_Physical_Assets](obsidian://open?file=Grid_Physical_Assets)` phụ thuộc vào dòng ngắn mạch cảm tính lớn để định vị điểm sự cố.
  * GFM thông qua việc duy trì góc pha nguồn áp ảo và xuất dòng lỗi ổn định ($1.2 - 1.5 \, pu$) giúp hệ thống rơ-le bảo vệ phân biệt chính xác sự cố nội bộ vùng bảo vệ và sự cố ngoài vùng, ngăn ngừa hiện tượng rơ-le từ chối cắt (blinding) hoặc cắt sai (sympathetic tripping) thường gặp trong lưới chỉ sử dụng toàn nghịch lưu GFL.

---

## III. ĐẠO - TRIẾT LÝ VẬT LÝ & KIẾN TRÚC HỆ THỐNG
*(AI Agent dự thảo: Meta-Agent - Chờ N.H.Anh k66 hiệu chỉnh)*

1. **Sự hợp nhất của Âm và Dương trong Tạo lưới Tự trị:**
   Khâu Âm kiến tạo một Máy phát đồng bộ ảo (`[Virtual_Synchronous_Machine](obsidian://open?file=Virtual_Synchronous_Machine)`) bên trong vi xử lý điều khiển bằng các phương trình vi phân cơ - từ ($J_v, D_v, \psi_v$) và dao động nội tại VCO không phụ thuộc PLL. Khâu Dương mang nguồn áp ảo này hòa vào mạng lưới truyền tải `[Grid_Physical_Assets](obsidian://open?file=Grid_Physical_Assets)`, giải quyết những thách thức khắc nghiệt của thế giới thực: kìm hãm RoCoF theo tiêu chuẩn ENTSO-E, xuyên sự cố ZVRT theo IEEE 2800-2022 và bơm dòng ngắn mạch FFCI an toàn cho van bán dẫn.
2. **Triết lý từ "Nô lệ" (Following) sang "Chủ nhân" (Forming):**
   - **`[Grid_Following](obsidian://open?file=Grid_Following)` (GFL):** Là người đi theo, luôn cần một góc pha lưới có sẵn từ PLL để bơm dòng điện theo định hướng áp. Khi lưới yếu hoặc rã lưới, GFL bất lực.
   - **`[Grid_Forming](obsidian://open?file=Grid_Forming)` (GFM):** Là người kiến tạo, tự đứng vững trên đôi chân dao động nội tại VCO, tự lập định mức áp và tần số, đóng vai trò chiếc neo ổn định quán tính cho toàn bộ hệ thống nguồn năng lượng tái tạo tương lai.
3. **Ý nghĩa Bản thể trong Vũ trụ Lưới điện:**
   `[Grid_Forming](obsidian://open?file=Grid_Forming)` chính là "trái tim tự trị" của Trụ cột `[Grid_Physical_Assets](obsidian://open?file=Grid_Physical_Assets)` — chuyển hóa điện tử công suất tĩnh từ một thành phần thụ động thành nguồn phát năng lượng chủ động, bảo vệ hệ thống trước mọi suy biến tần số và điện áp.
