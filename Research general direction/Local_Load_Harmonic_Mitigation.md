---
weighttree: 0
contributors: ["N.H.Anh k66"]
---
Direct Parent Connection: -> [[Harmonic_Mitigation]]

# Local_Load_Harmonic_Mitigation (Bù sóng hài tải phi tuyến cục bộ / Local Nonlinear Load Harmonic Compensation)

## I. KHÂU ÂM - NGUYÊN LÝ VẬT LÝ VÀ TOÁN HỌC THUẦN TÚY (PURE ENTITY)
*(AI Agent dự thảo: pure_entity_researcher.md - Chờ N.H.Anh k66 hiệu chỉnh)*

### 1. Phân tích chuỗi Fourier của dòng điện tải chỉnh lưu phi tuyến
Mạch chỉnh lưu cầu bán dẫn phi tuyến (6 xung hoặc 12 xung sử dụng Diode/Thyristor) khi hoạt động dưới nguồn áp xoay chiều lý tưởng sẽ sinh ra dạng sóng dòng điện ngõ vào không hình sin do quá trình chuyển mạch gián đoạn từng khoảng dẫn của các van bán dẫn.

#### a. Chuỗi Fourier cho bộ chỉnh lưu cầu 6 xung (6-Pulse Rectifier)
Trong cấu hình cầu chỉnh lưu 6 xung hoàn toàn đối xứng (không có thành phần một chiều DC và không có hài chẵn), hàm biểu diễn tức thời của dòng điện ngõ vào $i_L(t)$ theo chuỗi Fourier được định nghĩa bởi:
$$i_L(t) = \sum_{h=1}^{\infty} \sqrt{2} I_{L,h} \sin\left(h\omega_0 t + \theta_h\right)$$
Trong đó:
- $I_{L,h}$ là giá trị hiệu dụng (RMS) của thành phần hài bậc $h$.
- $\omega_0 = 2\pi f_0$ là tần số góc cơ bản.
- $\theta_h$ là góc pha ban đầu của thành phần hài bậc $h$.

Với mạch chỉnh lưu 6 xung lý tưởng (độ tự cảm ngõ vào $L_s \to \infty$, tải nguồn dòng một chiều lý tưởng), phổ tần số tồn tại các bậc hài lẻ đặc trưng thỏa mãn điều kiện:
$$h \in \{6k \pm 1\} = \{5, 7, 11, 13, 17, 19, 23, 25, \dots\} \quad (k \in \mathbb{N}^*)$$
Biên độ lý thuyết của dòng điện hài bậc $h$ tỷ lệ nghịch với bậc của hài so với thành phần cơ bản $I_{L,1}$:
$$I_{L,h} = \frac{I_{L,1}}{h} \implies \text{THD}_i = \frac{\sqrt{\sum_{h \neq 1} I_{L,h}^2}}{I_{L,1}} = \sqrt{\frac{\pi^2}{9} - 1} \approx 31.08\%$$
Khi xét đến hiệu ứng trùng dẫn (Commutation Overlap Effect) với góc trùng dẫn $\mu > 0$ sinh ra bởi điện cảm chuyển mạch $L_c$, phổ biên độ suy giảm theo hệ số từ thông chuyển mạch:
$$I_{L,h}(\mu) = I_{L,h}^{(0)} \cdot \left[ \frac{\sin\left(h \frac{\mu}{2}\right)}{h \frac{\mu}{2}} \right]$$

#### b. Chuỗi Fourier cho bộ chỉnh lưu cầu 12 xung (12-Pulse Rectifier)
Cấu hình 12 xung gồm hai bộ chỉnh lưu 6 xung ghép song song hoặc nối tiếp thông qua máy biến áp dịch pha $\Delta/\text{Y}-\Delta$ lệch pha nhau một góc điện $\delta = 30^\circ$ ($\pi/6\text{ rad}$). Dòng điện tổng hợp ngõ vào triệt tiêu các bậc hài $6k \pm 1$ với $k$ lẻ:
$$i_{L,12p}(t) = i_{L,Y}(t) + i_{L,\Delta}(t) = 2\sqrt{2} I_{L,1} \left[ \sin(\omega_0 t) - \frac{1}{11}\sin(11\omega_0 t) + \frac{1}{13}\sin(13\omega_0 t) - \dots \right]$$
Tập hợp các bậc hài đặc trưng của cấu hình 12 xung được thu gọn thành:
$$h \in \{12k \pm 1\} = \{11, 13, 23, 25, 35, 37, \dots\} \quad (k \in \mathbb{N}^*)$$

---

### 2. Lý thuyết công suất tức thời trên khung tọa độ tĩnh $\alpha-\beta$ và hệ tọa độ quay đồng bộ nhiều khung (Multi-SRF $d-q$)
Để bóc tách chính xác các thành phần dòng điện hài cần bù trừ, lý thuyết công suất tức thời Akagi (p-q Theory) và Lý thuyết công suất bảo toàn (Conservative Power Theory - CPT) cung cấp nền tảng toán học biến đổi véctơ dòng-áp.

#### a. Lý thuyết p-q trên khung tọa độ trực giao tĩnh $\alpha-\beta$
Áp dụng phép biến đổi Clarke bảo toàn công suất cho hệ ba pha $abc$ sang mặt phẳng trực giao $\alpha-\beta-0$:
$$\begin{bmatrix} x_\alpha(t) \\ x_\beta(t) \\ x_0(t) \end{bmatrix} = \sqrt{\frac{2}{3}} \begin{bmatrix} 1 & -\frac{1}{2} & -\frac{1}{2} \\ 0 & \frac{\sqrt{3}}{2} & -\frac{\sqrt{3}}{2} \\ \frac{1}{\sqrt{2}} & \frac{1}{\sqrt{2}} & \frac{1}{\sqrt{2}} \end{bmatrix} \begin{bmatrix} x_a(t) \\ x_b(t) \\ x_c(t) \end{bmatrix}$$
Công suất hữu công tức thời ($p$) và công suất vô công tức thời ($q$) được định nghĩa trên khung $\alpha-\beta$:
$$\begin{bmatrix} p(t) \\ q(t) \end{bmatrix} = \begin{bmatrix} v_\alpha(t) & v_\beta(t) \\ -v_\beta(t) & v_\alpha(t) \end{bmatrix} \begin{bmatrix} i_{L,\alpha}(t) \\ i_{L,\beta}(t) \end{bmatrix}$$
Phân tích phổ tần số của công suất tức thời cho thấy sự tách biệt rõ ràng giữa thành phần một chiều (DC - đại diện cho năng lượng cơ bản xác lập) và thành phần xoay chiều (AC - đại diện cho dao động hài):
$$p(t) = \bar{p} + \tilde{p}(t), \qquad q(t) = \bar{q} + \tilde{q}(t)$$
Trong đó:
- $\bar{p}$: Công suất hữu công cơ bản thuận (Thành phần DC truyền tải năng lượng thuần túy).
- $\bar{q}$: Công suất vô công cơ bản thuận (Thành phần DC tương tác từ trường cơ bản).
- $\tilde{p}(t)$: Dao động công suất hữu công tức thời sinh ra bởi tương tác giữa áp cơ bản và dòng hài (chu kỳ $6k\omega_0$).
- $\tilde{q}(t)$: Dao động công suất vô công tức thời sinh ra bởi các thành phần dòng hài phi tuyến.

Phương trình toán học xác định véctơ dòng điện hài tham chiếu cần triệt tiêu trong miền $\alpha-\beta$:
$$\begin{bmatrix} i_{c,\alpha}^*(t) \\ i_{c,\beta}^*(t) \end{bmatrix} = \frac{1}{\Delta_v} \begin{bmatrix} v_\alpha(t) & -v_\beta(t) \\ v_\beta(t) & v_\alpha(t) \end{bmatrix} \begin{bmatrix} -\tilde{p}(t) \\ -q(t) \end{bmatrix}, \quad \text{với } \Delta_v = v_\alpha^2(t) + v_\beta^2(t)$$

#### b. Phân tách hài trên hệ tọa độ quay đồng bộ nhiều khung (Multi-SRF $dq$)
Để tránh hiện tượng trễ pha của các bộ lọc thông thấp (LPF) trong miền $\alpha-\beta$, cấu trúc biến đổi Park đồng bộ đa hệ quy chiếu (Multi-Synchronous Reference Frame - MSRF) chuyển hóa từng bậc hài tự do thành một thành phần DC trên khung quay tương ứng:
$$\mathbf{T}_{dq}^{+h}(\theta_0) = \frac{2}{3} \begin{bmatrix} \cos(h\theta_0) & \cos\left(h\theta_0 - \frac{2\pi}{3}\right) & \cos\left(h\theta_0 + \frac{2\pi}{3}\right) \\ -\sin(h\theta_0) & -\sin\left(h\theta_0 - \frac{2\pi}{3}\right) & -\sin\left(h\theta_0 + \frac{2\pi}{3}\right) \\ \frac{1}{2} & \frac{1}{2} & \frac{1}{2} \end{bmatrix}$$
Khi chiếu véctơ dòng điện $i_{\alpha\beta}(t)$ lên hệ trục tọa độ quay với vận tốc góc $+h\omega_0$ hoặc $-h\omega_0$:
- Thành phần hài bậc $+h$ (thứ tự thuận) hoặc $-h$ (thứ tự nghịch) chuyển hóa thành véctơ tĩnh DC: $\mathbf{I}_{dq}^{h} = \left[ I_d^{h}, I_q^{h} \right]^T$.
- Các thành phần hài bậc $m \neq h$ biểu hiện dưới dạng dao động xoay chiều AC với tần số góc $(m \mp h)\omega_0$.
- Việc tách dòng hài bậc $h$ được thực hiện thông qua bộ lọc tích phân trung bình trượt (Moving Average Filter - MAF) có chu kỳ $T_0 = 2\pi/\omega_0$, loại bỏ hoàn toàn nhiễu chéo giữa các bậc hài.

---

### 3. Hàm truyền bộ điều khiển cộng hưởng tỷ lệ đa tần (Multi-Resonant PR Controller)
Để bám sát véctơ dòng tham chiếu hài xoay chiều với sai lệch xác lập bằng 0, bộ điều khiển Tỷ lệ - Cộng hưởng đa tần (Multi-Resonant Proportional-Resonant - MR-PR) được áp dụng trực tiếp trong miền tĩnh $\alpha-\beta$.

#### a. Cấu trúc hàm truyền toán học trong miền phức $s$
Hàm truyền tổng quát của bộ điều khiển MR-PR là sự chồng chất tuyến tính của khâu tỷ lệ $K_p$ và các bộ cộng hưởng song song tại tần số cơ bản và các bậc hài đặc trưng:
$$G_{PR}(s) = K_p + \sum_{h \in H} G_{R,h}(s) = K_p + \sum_{h \in H} \frac{2 K_{i,h} \omega_c s}{s^2 + 2\omega_c s + (h \omega_0)^2}$$
Trong đó:
- $K_p$: Độ lợi tỷ lệ, quyết định băng thông động học toàn cục của vòng lặp dòng điện.
- $K_{i,h}$: Độ lợi tích phân tại tần số hài bậc $h$, quyết định tốc độ hội tụ và độ cứng động học tại tần số $h\omega_0$.
- $\omega_c$: Băng thông cắt của bộ cộng hưởng (Cut-off frequency bandwidth, thường chọn $\omega_c \ll \omega_0$), nhằm tạo độ suy giảm hữu hạn xung quanh đỉnh cộng hưởng, hạn chế mất ổn định do sai lệch trôi tần số góc $\Delta\omega$.
- $H = \{1, 5, 7, 11, 13, 17, 19, \dots\}$: Tập hợp các bậc hài đặc trưng được chọn để bám hoặc triệt tiêu.

#### b. Chứng minh khả năng triệt tiêu sai lệch tĩnh theo Nguyên lý mô hình nội (IMP)
Theo Nguyên lý Mô hình Nội (Internal Model Principle - Francis & Wonham), một hệ thống điều khiển phản hồi kín có khả năng triệt tiêu sai lệch xác lập đối với một tín hiệu vào rập khuôn nếu hàm truyền hệ hở chứa cực kẹp trùng khớp với cực của bộ tạo tín hiệu vào (Exogenous Signal Generator).

Xét sai lệch bám dòng điện trong miền $s$:
$$E(s) = I_{c,\alpha\beta}^*(s) - I_{c,\alpha\beta}(s) = \frac{1}{1 + G_{PR}(s) G_{plant}(s)} I_{c,\alpha\beta}^*(s)$$
Tại tần số kích thích của hài bậc $h$, tức $s = j h \omega_0$:
$$\lim_{s \to j h \omega_0} |G_{R,h}(s)| = \lim_{s \to j h \omega_0} \left| \frac{2 K_{i,h} \omega_c s}{s^2 + 2\omega_c s + (h\omega_0)^2} \right| = K_{i,h} \to \text{Hữu hạn nhưng cực đại khi } \omega_c \to 0 \implies |G_{PR}(j h\omega_0)| \to \infty$$
Khi $\omega_c \to 0$ (bộ cộng hưởng lý tưởng):
$$\lim_{s \to j h \omega_0} G_{PR}(s) = \infty \implies \lim_{s \to j h \omega_0} \frac{1}{1 + G_{PR}(s) G_{plant}(s)} = 0$$
Do đó, độ lợi đường thuận tại từng tần số hài $h\omega_0$ tiến đến vô cùng, buộc sai lệch xác lập tiến triệt để về không:
$$\lim_{t \to \infty} e_h(t) = 0 \quad (\forall h \in H)$$
Đảm bảo triệt tiêu hoàn toàn thành phần hài bậc $h$ của tải phi tuyến mà không gây dịch pha tín hiệu điều khiển tại các dải tần số lân cận.

---

## II. KHÂU DƯƠNG - TƯƠNG TÁC VẬT LÝ CỤC BỘ VÀ CHẤT LƯỢNG ĐIỆN NĂNG TẠI PCC (LOCAL INTERACTION)
*(AI Agent dự thảo: grid_interaction_researcher.md - Chờ N.H.Anh k66 hiệu chỉnh)*

Khâu Dương của bộ bù hài tải phi tuyến cục bộ (`[Local_Load_Harmonic_Mitigation](obsidian://open?file=Local_Load_Harmonic_Mitigation)`) phân tích cơ chế tương tác trực tiếp của nghịch lưu đa chức năng (`[Multifunctional_Inverter](obsidian://open?file=Multifunctional_Inverter)`) với hệ thống tài sản lưới `[Grid_Physical_Assets](obsidian://open?file=Grid_Physical_Assets)` tại Điểm kết nối chung (`[Point_of_Common_Coupling](obsidian://open?file=Point_of_Common_Coupling)` - PCC), trong bối cảnh tích hợp mật độ cao tải phi tuyến (lò hồ quang, trạm sạc điện cực nhanh `[EV](obsidian://open?file=EV)`, truyền động biến tần công nghiệp).

### 1. Cơ chế Bơm dòng Bù hài Ngược pha (Shunt APF) tại `[Point_of_Common_Coupling](obsidian://open?file=Point_of_Common_Coupling)` (PCC)
Trong cấu trúc Bộ lọc Tích cực Song song (Shunt Active Power Filter - APF), định luật Kirchhoff về dòng điện (KCL) mô tả tương tác tức thời tại PCC giữa dòng điện phía lưới AC $\mathbf{i}_g(t)$, dòng tải phi tuyến $\mathbf{i}_L(t)$ và dòng nghịch lưu bơm ra $\mathbf{i}_{\text{inv}}(t)$:
$$\mathbf{i}_g(t) = \mathbf{i}_L(t) + \mathbf{i}_{\text{inv}}(t) = \left[ \mathbf{i}_{L,1}(t) + \sum_{h=2}^{\infty} \mathbf{i}_{L,h}(t) \right] + \left[ \mathbf{i}_{\text{inv},1}(t) + \mathbf{i}_{\text{comp}}(t) \right]$$
Trong đó:
- $\mathbf{i}_{L,1}(t)$ và $\sum_{h=2}^{\infty} \mathbf{i}_{L,h}(t)$ lần lượt là thành phần dòng tần số cơ bản ($50/60\text{ Hz}$) và tổng các bậc sóng hài dòng tải ($h = 5, 7, 11, 13, \dots$).
- $\mathbf{i}_{\text{inv},1}(t)$ là dòng phát công suất hữu công/phản kháng cơ bản từ nguồn phân tán (`[DER](obsidian://open?file=DER)`).
- $\mathbf{i}_{\text{comp}}(t)$ là dòng bù sóng hài tích cực được nghịch lưu bơm vào thanh cái PCC.

Để đảm bảo dòng phía lưới $\mathbf{i}_g(t)$ duy trì dạng sóng sin chuẩn (pure sinusoidal) và đồng pha với điện áp lưới, bộ điều khiển Khâu Âm điều khiển van bán dẫn tạo ra thành phần dòng bù ngược pha hoàn toàn với các bậc hài tải cục bộ:
$$\mathbf{i}_{\text{comp}}(t) = -\sum_{h=2}^{\infty} \mathbf{i}_{L,h}(t) \implies \mathbf{i}_g(t) = \mathbf{i}_{L,1}(t) + \mathbf{i}_{\text{inv},1}(t)$$
Sự tương tác này triệt tiêu hoàn toàn sự lan truyền dòng sóng hài từ tải cục bộ lên quy mô lưới phân phối phía trên, bảo vệ máy biến áp và tài sản vật lý `[Grid_Physical_Assets](obsidian://open?file=Grid_Physical_Assets)` khỏi hiện tượng phát nhiệt tổn hao xoáy (Eddy-current losses) và rung động từ giảo.

---

### 2. Phân bổ Dung lượng Nhiệt và Dòng định mức Van IGBT/SiC MOSFET trong Đồng phát Công suất ($P_{\text{gen}}$) & Bù Sóng hài
Sự tương tác cục bộ của nghịch lưu bị giới hạn ràng buộc tuyệt đối bởi khả năng chịu tải nhiệt và bao hình dòng điện hiệu dụng cực đại của van bán dẫn công suất (`[SiC_MOSFET](obsidian://open?file=SiC_MOSFET)` hoặc IGBT), ký hiệu là $I_{\text{inv,max}}$.

Khi nghịch lưu thực hiện nhiệm vụ kép: (1) Bơm công suất hữu công $P_{\text{gen}}$ từ `[PV](obsidian://open?file=PV)`/`[BESS](obsidian://open?file=BESS)`, (2) Bù công suất phản kháng $Q_1$, và (3) Bù dòng méo hài $I_{\text{comp},h}$, tổng dòng hiệu dụng RMS đầu ra của nghịch lưu được tính theo tổng vectơ trong không gian Hilbert:
$$I_{\text{inv,rms}} = \sqrt{I_{\text{inv},1}^2 + I_{\text{comp,rms}}^2} = \sqrt{\left( \frac{P_{\text{gen}}}{\sqrt{3} V_{\text{PCC}}} \right)^2 + \left( \frac{Q_1}{\sqrt{3} V_{\text{PCC}}} \right)^2 + \sum_{h=2}^{\infty} I_{\text{comp},h}^2} \le I_{\text{inv,max}}$$
Khả năng mang tải biểu kiến tổng hợp $S_{\text{inv}}$ tại nút kết nối thỏa mãn phương trình miền công suất định mức:
$$S_{\text{inv}} = \sqrt{P_{\text{gen}}^2 + Q_1^2 + H^2} \le S_{\text{inv,max}} = \sqrt{3} V_{\text{PCC,rms}} I_{\text{inv,max}}$$
Trong đó $H = \sqrt{3} V_{\text{PCC,rms}} \sqrt{\sum_{h=2}^{\infty} I_{\text{comp},h}^2}$ là **Công suất Hài bù trừ (Harmonic Power)**.

**Chiến lược Tương tác Suy giảm Ưu tiên (Priority Derating Interaction Strategy):**
- Trong các thời điểm bức xạ mặt trời cao hoặc tải xả đỉnh, nếu tổng dòng yêu cầu vượt ngưỡng $I_{\text{inv,max}}$, hệ thống áp dụng cơ chế suy giảm có chọn lọc.
- **Ưu tiên 1:** Duy trì công suất tác dụng $P_{\text{gen}}$ nhằm đảm bảo hiệu quả kinh tế năng lượng.
- **Ưu tiên 2:** Sử dụng dải dung lượng dòng điện tự do còn lại $\Delta I_{\text{avail}} = \sqrt{I_{\text{inv,max}}^2 - I_{\text{inv},1}^2}$ để bù ưu tiên các bậc hài bậc thấp có biên độ lớn và gây nguy hại nhất cho lưới ($h = 5, 7$), trong khi giảm độ lợi bù cho các bậc hài bậc cao hơn ($h \ge 11$) nhằm tránh bão hòa từ và quá nhiệt van bán dẫn.

---

### 3. Tuân thủ Tiêu chuẩn Chất lượng Điện năng IEEE 519-2014 và IEC 61000-3-2/4
Sự tương tác giữa thiết bị bù hài cục bộ và lưới điện `[Grid_Physical_Assets](obsidian://open?file=Grid_Physical_Assets)` phải tuân thủ nghiêm ngặt các ranh giới kỹ thuật theo tiêu chuẩn IEEE 519-2014 và hệ thống tiêu chuẩn IEC 61000:

| Thông số Chỉ tiêu Kỹ thuật | Tiêu chuẩn Áp dụng | Điều kiện Kết nối Lưới / Tải tại PCC | Giới hạn Cho phép (Limit) | Ý nghĩa Tương tác Vật lý |
| :--- | :--- | :--- | :--- | :--- |
| **Tổng độ méo hài dòng điện ($\text{THD}_i$ / $\text{TDD}$)** | IEEE 519-2014 | Tỷ số ngắn mạch $I_{SC} / I_L > 20$ (Lưới đủ mạnh) | **$\text{TDD} < 5.0\%$** | Ngăn chặn sụt áp méo hài trên tổng trở lưới gây suy giảm tuổi thọ máy biến áp phân phối |
| **Hài dòng riêng lẻ bậc thấp ($h < 11$)** | IEEE 519-2014 | Các bậc hài lẻ $h = 5, 7$ (Từ tải chỉnh lưu 6 xung) | **$I_h / I_L < 4.0\%$** | Tránh kích thích cộng hưởng cơ - điện trên động cơ không đồng bộ trong cùng phân đoạn lưới |
| **Hài dòng riêng lẻ bậc trung ($11 \le h < 17$)** | IEEE 519-2014 | Các bậc hài lẻ $h = 11, 13$ (Từ tải 12 xung / nghịch lưu) | **$I_h / I_L < 2.0\%$** | Giảm thiểu tổn hao dòng xoáy trong cuộn dây và lõi thép máy biến áp kết nối |
| **Hài dòng riêng lẻ bậc cao ($17 \le h < 35$)** | IEEE 519-2014 | Các bậc hài $h = 17, 19, 23, 25$ | **$I_h / I_L < 1.5\%$** | Ngăn ngừa can nhiễu thông tin liên lạc và tín hiệu đường dây PLC (Power Line Carrier) |
| **Hài dòng riêng lẻ siêu cao ($35 \le h \le 50$)** | IEEE 519-2014 | Các bậc hài tần số chuyển mạch thấp | **$I_h / I_L < 0.3\%$** | Bảo đảm ổn định cho tụ bù công suất phản kháng cục bộ, tránh quá dòng tần số cao |
| **Dòng phát xa hài (Supraharmonics $2 \text{ kHz} - 150 \text{ kHz}$)** | IEC 61000-2-2 / IEC 61000-3-4 | Tương tác tần số chuyển mạch van `[SiC_MOSFET](obsidian://open?file=SiC_MOSFET)` | **Tuân thủ đường cong giới hạn CISPR 11/14** | Tránh gây cộng hưởng ký sinh với hệ thống cáp ngầm và thiết bị bảo vệ rò RCD/GFCI |

Sự tuân thủ này đòi hỏi bộ lọc đầu ra $LCL$ của nghịch lưu phải được phối hợp trở kháng chính xác với lưới cục bộ, đồng thời các vòng điều khiển dòng điện (PR Controller / Multi-synchronous Reference Frame) phải có độ chiết giảm biên độ (Attenuation) đủ lớn tại vùng tần số đóng cắt $f_{sw} \ge 20\text{ kHz}$.

---

## III. ĐẠO - TRIẾT LÝ VẬT LÝ & KIẾN TRÚC HỆ THỐNG
*(AI Agent dự thảo: Meta-Agent - Chờ N.H.Anh k66 hiệu chỉnh)*

1. **Sự hợp nhất của Âm và Dương trong Bù sóng hài Tải cục bộ:**
   Khâu Âm xác lập bản thể dòng hài bằng chuỗi Fourier, tách dải phổ bằng lý thuyết công suất tức thời p-q/CPT và chế ngự sai số tại đúng tần số bằng bộ điều khiển cộng hưởng MR-PR. Khâu Dương mang trật tự toán học đó ra nút tải `[Point_of_Common_Coupling](obsidian://open?file=Point_of_Common_Coupling)`, phân bổ khôn ngoan giới hạn nhiệt của bán dẫn `[SiC_MOSFET](obsidian://open?file=SiC_MOSFET)` để vừa bán năng lượng $P$, vừa giữ sạch dòng lưới theo IEEE 519-2014.
2. **Triết lý "Lấy độc trị độc" (Phase-Opposite Cancellation):**
   Méo dạng sóng không thể xóa bỏ bằng cách chặn lại, mà phải được dập tắt bởi chính một dạng sóng bù có biên độ tương đương và góc pha đối lập ($180^\circ$). Đó là sự hoàn hảo của nguyên lý cân bằng tự nhiên trong hệ thống điện tử công suất.
3. **Ý nghĩa Bản thể trong Vũ trụ Lưới điện:**
   `[Local_Load_Harmonic_Mitigation](obsidian://open?file=Local_Load_Harmonic_Mitigation)` là lớp màng lọc bảo vệ đầu tiên của hệ sinh thái `[Grid_Physical_Assets](obsidian://open?file=Grid_Physical_Assets)`, giữ cho sự hỗn loạn của các phụ tải phi tuyến hiện đại không xâm lấn và phá vỡ cấu trúc hài hòa của lưới điện truyền tải.
