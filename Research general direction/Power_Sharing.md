---
contributors: ["N.H.Anh k66"]
---
Direct Parent Connection: -> [[Grid_Following]]

# Power_Sharing (Chia sẻ công suất song song - P-Q Current Sharing)

## I. KHÂU ÂM - NGUYÊN LÝ VẬT LÝ VÀ TOÁN HỌC THUẦN TÚY (PURE ENTITY)
*(AI Agent dự thảo: pure_entity_researcher.md - Chờ N.H.Anh k66 hiệu chỉnh)*

### 1. Phương trình mạch vòng điều khiển chia sẻ dòng điện (Proportional Current Sharing Loop / Master-Slave Current Tracking)
Xét hệ thống $n$ bộ nghịch lưu nguồn áp (Voltage Source Inverters - VSI) mắc song song vào một thanh cái xoay chiều chung (Common AC Bus). Gọi $\mathbf{i}_{o,i} = \left[ i_{od,i}, i_{oq,i} \right]^T$ là vector dòng điện ngõ ra trên khung tọa độ quay đồng bộ $d-q$ của bộ nghịch lưu thứ $i$ ($i \in \{1, 2, \dots, n\}$).

#### a. Phương trình dòng điện tổng và điều kiện chia sẻ tỷ lệ định mức
Tổng dòng điện ngõ ra cấp cho tổng trở tải chung:
$$\mathbf{i}_{\text{total}} = \sum_{i=1}^{n} \mathbf{i}_{o,i}$$

Để đảm bảo chia sẻ công suất tác dụng $P$ và phản kháng $Q$ theo đúng tỷ lệ định mức dung lượng $S_{\text{nom},i}$ của từng kênh nghịch lưu, điều kiện xác lập dòng điện ngõ ra chuẩn hóa (Normalized Output Current) được định nghĩa là:
$$\mathbf{i}_{\text{norm},1} = \mathbf{i}_{\text{norm},2} = \dots = \mathbf{i}_{\text{norm},n} \quad \text{với} \quad \mathbf{i}_{\text{norm},i} = \frac{\mathbf{i}_{o,i}}{k_i}$$
trong đó $k_i = \frac{S_{\text{nom},i}}{\sum_{j=1}^{n} S_{\text{nom},j}}$ là trọng số chia sẻ định mức ($\sum_{i=1}^{n} k_i = 1$).

#### b. Mô hình sai lệch dòng điện và luật điều khiển
Sai lệch dòng điện bám sát (Tracking Error Vector) của bộ nghịch lưu thứ $i$ so với dòng điện tham chiếu chuẩn hóa toàn cục $\mathbf{i}_{\text{ref}} = k_i \mathbf{i}_{\text{total}}$ được biểu diễn bởi:
$$\mathbf{e}_{i}(t) = k_i \mathbf{i}_{\text{total}}(t) - \mathbf{i}_{o,i}(t)$$

Trong cấu trúc điều khiển **Master-Slave Current Tracking**, bộ nghịch lưu Chủ (Master, $i=1$) định hình điện áp và cung cấp dòng tham chiếu $\mathbf{i}_{\text{master}}$, các bộ nghịch lưu Khách (Slave, $i \ge 2$) sử dụng luật điều khiển tỷ lệ - tích phân (PI) để bám theo dòng điện tham chiếu:
$$\mathbf{u}_{\text{curr},i}(t) = \mathbf{K}_{p,c} \mathbf{e}_i(t) + \mathbf{K}_{i,c} \int_{0}^{t} \mathbf{e}_i(\tau) d\tau - \omega_e \mathbf{J} \mathbf{L}_f \mathbf{i}_{o,i}(t)$$
trong đó $\mathbf{u}_{\text{curr},i} = \left[ u_{d,i}, u_{q,i} \right]^T$ là điện áp đặt nghịch lưu, $\mathbf{K}_{p,c}, \mathbf{K}_{i,c}$ là các ma trận độ lợi PI, $\mathbf{J} = \begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}$ là ma trận trực giao decoupling, $\omega_e$ là tần số góc điện, và $\mathbf{L}_f$ là ma trận cảm kháng bộ lọc.

---

### 2. Mô hình toán học không gian trạng thái d-q triệt tiêu dòng quẩn (Circulating Current Zeroing)

#### a. Bản chất vật lý của dòng điện quẩn (Circulating Current)
Khi $n$ bộ nghịch lưu mắc song song, sai lệch tức thời về biên độ điện áp xung PWM, góc pha chuyển mạch hoặc độ lệch trở kháng dây nối sẽ sinh ra dòng điện quẩn nội bộ $\mathbf{i}_{\text{circ},i}$. Dòng điện quẩn của kênh thứ $i$ được định nghĩa là thành phần không đóng góp vào dòng tổng tải:
$$\mathbf{i}_{\text{circ},i}(t) = \mathbf{i}_{o,i}(t) - k_i \sum_{j=1}^{n} \mathbf{i}_{o,j}(t)$$
Đặc tính vật lý cơ bản: Tổng dòng quẩn trong toàn bộ hệ thống mắc song song luôn bằng không:
$$\sum_{i=1}^{n} \mathbf{i}_{\text{circ},i}(t) = \mathbf{0}$$

#### b. Phương trình vi phân không gian trạng thái trên khung d-q
Xét hệ thống lọc ngõ ra LCL ($L_{1i}, C_i, L_{2i}$) cho bộ nghịch lưu thứ $i$. Đặt vector biến trạng thái $\mathbf{x}_i = \left[ \mathbf{i}_{L1,i}^T, \mathbf{v}_{C,i}^T, \mathbf{i}_{o,i}^T, \mathbf{\xi}_{\text{circ},i}^T \right]^T \in \mathbb{R}^8$, trong đó $\mathbf{\xi}_{\text{circ},i} = \int \mathbf{i}_{\text{circ},i} dt$ là biến trạng thái tích phân của bộ triệt tiêu dòng quẩn. Phương trình vi phân trạng thái có dạng chuẩn:
$$\frac{d}{dt}\mathbf{x}_i(t) = \mathbf{A}_i \mathbf{x}_i(t) + \mathbf{B}_i \mathbf{u}_i(t) + \mathbf{E}_i \mathbf{v}_{\text{bus}}(t)$$

Ma trận động học hệ thống $\mathbf{A}_i$, ma trận đầu vào điều khiển $\mathbf{B}_i$ và ma trận liên kết nhiễu bus $\mathbf{E}_i$ được khai triển cụ thể:
$$\mathbf{A}_i = \begin{bmatrix}
-\frac{R_{1i}}{L_{1i}}\mathbf{I}_2 - \omega_e\mathbf{J} & -\frac{1}{L_{1i}}\mathbf{I}_2 & \mathbf{0}_2 & \mathbf{0}_2 \\
\frac{1}{C_i}\mathbf{I}_2 & -\omega_e\mathbf{J} & -\frac{1}{C_i}\mathbf{I}_2 & \mathbf{0}_2 \\
\mathbf{0}_2 & \frac{1}{L_{2i}}\mathbf{I}_2 & -\frac{R_{2i}}{L_{2i}}\mathbf{I}_2 - \omega_e\mathbf{J} & \mathbf{0}_2 \\
\mathbf{0}_2 & \mathbf{0}_2 & \left(\mathbf{I}_2 - k_i\mathbf{I}_2\right) & \mathbf{0}_2
\end{bmatrix}, \quad
\mathbf{B}_i = \begin{bmatrix}
\frac{V_{\text{dc},i}}{L_{1i}}\mathbf{I}_2 \\ \mathbf{0}_2 \\ \mathbf{0}_2 \\ \mathbf{0}_2
\end{bmatrix}, \quad
\mathbf{E}_i = \begin{bmatrix}
\mathbf{0}_2 \\ \mathbf{0}_2 \\ -\frac{1}{L_{2i}}\mathbf{I}_2 \\ \mathbf{0}_2
\end{bmatrix}$$
với $R_{1i}, R_{2i}$ là điện trở tương đương của cuộn cảm lọc $L_{1i}, L_{2i}$, $\mathbf{I}_2$ là ma trận đơn vị $2 \times 2$, và $\mathbf{v}_{\text{bus}}$ là vector điện áp thanh cái chung.

#### c. Cơ chế chèn tín hiệu bù triệt tiêu dòng quẩn (CCSC - Circulating Current Suppression Controller)
Để triệt tiêu dòng quẩn mà không gây xung đột điện áp tổng, luật điều khiển điện áp điều chế xung PWM ngõ vào $\mathbf{u}_i(t)$ được bổ sung thành phần bù không gian thứ tự không (Zero-sequence / Circulating Current Compensation Voltage $\mathbf{u}_{\text{comp},i}$):
$$\mathbf{u}_i(t) = \mathbf{u}_{\text{ref},i}(t) - \mathbf{u}_{\text{comp},i}(t) = \mathbf{u}_{\text{ref},i}(t) - \left[ \mathbf{G}_{p,\text{circ}} \mathbf{i}_{\text{circ},i}(t) + \mathbf{G}_{i,\text{circ}} \int_{0}^{t} \mathbf{i}_{\text{circ},i}(\tau) d\tau \right]$$
Do tính chất $\sum_{i=1}^{n} \mathbf{u}_{\text{comp},i}(t) = \mathbf{0}$, tín hiệu điều khiển bù dòng quẩn hoàn toàn không làm biến dạng giá trị trung bình của điện áp chung trên thanh cái xoay chiều.

---

### 3. Hàm truyền vòng khóa pha SRF-PLL và điều kiện xác lập ổn định chia sẻ P-Q

#### a. Tuyến tính hóa hàm truyền SRF-PLL trong miền Laplace
Bộ vòng khóa pha khung tọa độ quay đồng bộ (SRF-PLL) thực hiện gióng thẳng vector điện áp thanh cái theo trục $d$ ($v_{q} = 0$). Sơ đồ động học tuyến tính quanh điểm làm việc xác lập của SRF-PLL gồm ba thành phần cơ bản:
1. **Bộ tách sóng pha (Phase Detector - PD):** xấp xỉ tuyến tính sai lệch góc pha $\Delta \theta = \theta - \hat{\theta}$, cho ngõ ra sai lệch điện áp trục q:
   $$v_q(t) \approx V_m \sin(\theta - \hat{\theta}) \approx V_m (\theta - \hat{\theta})$$
   với $V_m$ là biên độ điện áp thanh cái.
2. **Bộ lọc vòng PI (Loop Filter):**
   $$F(s) = k_{p,\text{pll}} + \frac{k_{i,\text{pll}}}{s} = \frac{k_{p,\text{pll}} s + k_{i,\text{pll}}}{s}$$
3. **Bộ dao động điều khiển áp (VCO - Integrator):**
   $$\hat{\theta}(s) = \frac{1}{s} \Delta\omega(s)$$

Hàm truyền vòng kín từ góc pha thực $\theta(s)$ đến góc pha ước lượng $\hat{\theta}(s)$ của PLL là hàm bậc hai chuẩn:
$$H_{\text{pll}}(s) = \frac{\hat{\theta}(s)}{\theta(s)} = \frac{V_m F(s)}{s + V_m F(s)} = \frac{k_{p,\text{pll}} V_m s + k_{i,\text{pll}} V_m}{s^2 + k_{p,\text{pll}} V_m s + k_{i,\text{pll}} V_m} = \frac{2\zeta\omega_n s + \omega_n^2}{s^2 + 2\zeta\omega_n s + \omega_n^2}$$
trong đó tần số dao động riêng $\omega_n = \sqrt{k_{i,\text{pll}} V_m}$ và hệ số tắt dần $\zeta = \frac{k_{p,\text{pll}}}{2} \sqrt{\frac{V_m}{k_{i,\text{pll}}}}$.

#### b. Điều kiện xác lập ổn định chia sẻ công suất P-Q (Stability Criteria)
Công suất tác dụng $P_i$ và phản kháng $Q_i$ ngõ ra của bộ nghịch lưu thứ $i$ trên khung tọa độ quay đồng bộ:
$$P_i = \frac{3}{2}\left( v_{d} i_{od,i} + v_{q} i_{oq,i} \right), \quad Q_i = \frac{3}{2}\left( v_{q} i_{od,i} - v_{d} i_{oq,i} \right)$$
Do động học SRF-PLL tạo ra góc lệch pha quá độ $\Delta \theta_i(t)$ so với góc quay hệ thống, biến đổi Laplace của ma trận dẫn nạp ra động (Dynamic Output Admittance Matrix $\mathbf{Y}_{o,i}(s)$) bị ghép chéo bởi hàm truyền của PLL:
$$\begin{bmatrix} \Delta i_{od,i}(s) \\ \Delta i_{oq,i}(s) \end{bmatrix} = \mathbf{Y}_{o,i}(s) \begin{bmatrix} \Delta v_d(s) \\ \Delta v_q(s) \end{bmatrix} = \begin{bmatrix} Y_{dd,i}(s) & Y_{dq,i}(s) \\ Y_{qd,i}(s) & Y_{qq,i}(s) \end{bmatrix} \begin{bmatrix} \Delta v_d(s) \\ \Delta v_q(s) \end{bmatrix}$$
Thành phần dẫn nạp phi tuyến phát sinh từ động học PLL được mô tả bởi:
$$Y_{qq,i}(s) = Y_{\text{passive}}(s) - I_{od0,i} \cdot \frac{H_{\text{pll}}(s)}{V_{d0}}$$

**Điều kiện ổn định xác lập Routh-Hurwitz / Lyapunov:**
Để toàn bộ hệ thống $n$ bộ nghịch lưu song song chia sẻ $P-Q$ ổn định tiệm cận, phương trình đặc tính tổng dốc dẫn nạp của toàn mạch:
$$\det \left[ \mathbf{I} + \mathbf{Z}_{\text{bus}}(s) \sum_{i=1}^{n} \mathbf{Y}_{o,i}(s) \right] = 0$$
không được phép có bất kỳ nghiệm (cực hệ thống - poles) nào nằm trên nửa mặt phẳng phức bên phải (RHP - Right Half Plane). Nói cách khác, băng thông cắt $\omega_c$ của SRF-PLL phải nhỏ hơn nhiều so với băng thông điều khiển dòng điện ($\omega_{c,\text{pll}} < \frac{1}{5}\omega_{c,\text{curr}}$) để triệt tiêu ma trận trở kháng động âm (Negative Damping Impedance) gây dao động mạch vòng $P-Q$.

---

## II. KHÂU DƯƠNG: TƯƠNG TÁC NGOẠI VI & ĐIỀU PHỐI HỆ THỐNG (LOCAL INTERACTION)
*(AI Agent dự thảo: grid_interaction_researcher.md - Chờ N.H.Anh k66 hiệu chỉnh)*

Khâu Dương của một bộ nghịch lưu chia sẻ công suất (`[Power_Sharing](obsidian://open?file=Power_Sharing)`) khảo sát các cơ chế tương tác vật lý, điện từ và trường năng lượng giữa nhiều thiết bị điện tử công suất khi vận hành song song trên thanh cái vi lưới AC (`[Microgrid_Busbar](obsidian://open?file=Microgrid_Busbar)`) trong hệ sinh thái tài sản vật lý lưới (`[Grid_Physical_Assets](obsidian://open?file=Grid_Physical_Assets)`). Mọi hành vi cân bằng công suất, chia sẻ dòng tải và triệt tiêu dòng quẩn đều bị chi phối bởi trở kháng đường dây ngoại vi, kiến trúc truyền thông và các tiêu chuẩn tương tác DER.

### 1. Tương tác Điều phối Công suất trên Thanh cái AC Vi lưới (`Microgrid_Busbar`)
Khi nhiều bộ nghịch lưu bám lưới (`[Grid_Following](obsidian://open?file=Grid_Following)` - GFL) hoặc tạo lưới (`[Grid_Forming](obsidian://open?file=Grid_Forming)` - GFM) được ghép nối song song vào điểm ghép nối chung (`[Point_of_Common_Coupling](obsidian://open?file=Point_of_Common_Coupling)` - PCC), chúng phải chia sẻ tải tổng của toàn hệ thống theo tỷ lệ công suất định mức mà không gây rớt áp hoặc dao động tần số mất kiểm soát.

#### a. Phương trình cân bằng công suất nút tại PCC
Tổng công suất tác dụng ($P$) và công suất phản kháng ($Q$) từ $n$ bộ nghịch lưu bơm vào thanh cái chung (`[Microgrid_Busbar](obsidian://open?file=Microgrid_Busbar)`) phải thỏa mãn điều kiện cân bằng định luật Kirchhoff:

$$\sum_{i=1}^{n} P_{i} = P_{load} + P_{loss} = P_{total}, \quad \sum_{i=1}^{n} Q_{i} = Q_{load} + Q_{loss} = Q_{total}$$

Trong đó, công suất của mỗi nghịch lưu thứ $i$ truyền qua trở kháng đường dây ngoại vi $Z_{i} = R_{i} + jX_{i} = |Z_{i}|\angle \theta_{i}$ đến thanh cái có điện áp $V_{PCC}\angle 0^\circ$ được biểu diễn bởi:

$$P_{i} = \frac{E_{i} V_{PCC}}{|Z_{i}|} \cos(\theta_{i} - \delta_{i}) - \frac{V_{PCC}^2}{|Z_{i}|} \cos \theta_{i}$$

$$Q_{i} = \frac{E_{i} V_{PCC}}{|Z_{i}|} \sin(\theta_{i} - \delta_{i}) - \frac{V_{PCC}^2}{|Z_{i}|} \sin \theta_{i}$$

* Với $E_{i}\angle \delta_{i}$ là điện áp ngõ ra của nghịch lưu thứ $i$ tại đầu nối sau bộ lọc LC/LCL.
* $\delta_{i}$ là góc lệch pha điện áp giữa nghịch lưu thứ $i$ và thanh cái vi lưới.

#### b. Tương tác trở kháng đường dây và giải trừ liên cực $P-Q$ (Power Decoupling)
* **Trong lưới truyền tải ($X_{i} \gg R_{i}, \theta_{i} \approx 90^\circ$):** Công suất tác dụng $P_{i}$ phụ thuộc chủ yếu vào góc lệch pha $\delta_{i}$ (điều khiển tần số/pha), trong khi công suất phản kháng $Q_{i}$ phụ thuộc vào chênh lệch biên độ điện áp $(E_{i} - V_{PCC})$.
* **Trong lưới phân phối và vi lưới hạ áp ($R_{i} \gg X_{i}$ hoặc $R_{i} \approx X_{i}$):** Sự ghép chéo (cross-coupling) giữa $P_{i}$ và $Q_{i}$ trở nên gay gắt, khiến việc sử dụng bộ điều khiển độ dốc truyền thống ($P-\omega$ và $Q-V$) gây ra mất ổn định và dao động dòng tải song song.
* **Giải pháp tương tác Trở kháng Ảo (Virtual Impedance Loop):** Nghịch lưu chủ động bơm một trở kháng ảo $Z_{v} = R_{v} + jX_{v}$ vào chuỗi lệnh điều khiển điện áp ngõ ra để quy chuẩn hóa tổng trở xuất hiện tại đầu cực thiết bị ($Z_{total,i} = Z_{i} + Z_{v}$), ép tỷ lệ $X_{total}/R_{total} \gg 1$ nhằm khôi phục đặc tính phân tách $P-Q$ độc lập:

$$\vec{E}_{cmd,i} = \vec{E}_{ref,i} - Z_{v} \cdot \vec{I}_{o,i} = \vec{E}_{ref,i} - (R_{v} + j \omega_{0} L_{v}) \vec{I}_{o,i}$$

#### c. Hiện tượng Dòng điện Quẩn (Circulating Current - $I_{circ}$)
Khi nhiều nguồn năng lượng phân tán như hệ thống điện mặt trời (`[PV](obsidian://open?file=PV)`), pin lưu trữ (`[BESS](obsidian://open?file=BESS)`), và lưu trữ lai (`[HESS](obsidian://open?file=HESS)`) chạy song song, mọi sai lệch nhỏ về biên độ $\Delta E_{ij} = E_{i} - E_{j}$ hoặc pha điện áp $\Delta \delta_{ij} = \delta_{i} - \delta_{j}$ đều tạo ra dòng điện quẩn nội bộ chạy giữa các bộ nghịch lưu mà không đi ra tải:

$$\vec{I}_{circ,ij} = \frac{\vec{E}_{i} - \vec{E}_{j}}{Z_{i} + Z_{j}} = \vec{I}_{circ,ij}^{P} + j \vec{I}_{circ,ij}^{Q}$$

* **Dòng quẩn hữu công ($I_{circ}^{P}$):** Phát sinh do sai lệch pha/tần số tức thời, gây quá tải bộ tranzito công suất (IGBT/SiC MOSFET) và làm biến dạng dòng điện bơm lưới.
* **Dòng quẩn vô công ($I_{circ}^{Q}$):** Phát sinh do sai lệch biên độ điện áp ngõ ra hoặc sai lệch trở kháng nhánh đường dây $Z_{i} \neq Z_{j}$, làm sụt giảm hệ số công suất và tổn hao nhiệt trên cuộn cảm bộ lọc. Vòng điều khiển chia sẻ dòng phản kháng (Reactive Current Sharing) sử dụng tín hiệu tương tác ngoại vi phải tự động bù trừ độ lệch này.

---

### 2. So sánh Đặc tính Tương tác giữa Điều phối Có đường truyền và Không đường truyền
Hệ thống điều phối chia sẻ công suất giữa các DER (`[DER](obsidian://open?file=DER)`) trong vi lưới được phân loại dựa trên mức độ phụ thuộc vào mạng thông tin liên lạc ngoại vi (Communication Network).

| Tiêu chí Tương tác | Kiến trúc Có đường truyền thông tin<br>*(Communication-based: EMS / CAN / Modbus Master-Slave)* | Kiến trúc Không đường truyền<br>*(Communication-less: Decentralized Droop Control / `[Droop_Control](obsidian://open?file=Droop_Control)`)* |
| :--- | :--- | :--- |
| **Kiến trúc điều khiển** | **Tập trung / Phân tán có trợ giúp (Master-Slave / Centralized EMS):** Bộ điều khiển trung tâm tính toán $P_{ref}, Q_{ref}$ hoặc $I_{ref}$ và truyền lệnh xuống từng Slave qua đường bus. | **Hoàn toàn phi tập trung (Decentralized Autonomous):** Từng nghịch lưu tự đo lường điện áp, dòng điện tại đầu cực và tự điều chỉnh tần số, biên độ áp ngõ ra theo đường cong dốc. |
| **Độ trễ tương tác (Latency)** | **Cao ($10\,\text{ms} - 500\,\text{ms}$):** Phụ thuộc vào tốc độ đường truyền (CAN bus, RS485 Modbus RTU/TCP, hoặc Ethernet IEC 61850 GOOSE/SV). | **Bằng 0 (Zero Communication Latency):** Đáp ứng tức thời theo hằng số thời gian của bộ lọc đo lường công suất ($T_{f} \approx 5\,\text{ms} - 20\,\text{ms}$). |
| **Độ tin cậy & Điểm lỗi đơn (Single-Point-of-Failure)** | **Thấp - Trung bình:** Nếu bộ Master hoặc đường truyền bus gặp sự cố (đứt cáp, nhiễu EMI, mất gói tin), toàn bộ vi lưới có thể mất đồng bộ và rã lưới. | **Tuyệt đối cao (High Reliability):** Không có điểm lỗi đơn. Một nghịch lưu mất kết nối hoặc hư hỏng không làm ảnh hưởng đến khả năng duy trì tải của các bộ còn lại. |
| **Khả năng mở rộng (Scalability & Plug-and-Play)** | **Hạn chế:** Khi bổ sung một DER mới (`[PV](obsidian://open?file=PV)` / `[EV](obsidian://open?file=EV)`), cần lập trình lại Master EMS và khai báo lại địa chỉ/bản đồ thanh ghi giao tiếp. | **Tuyệt đối (True Plug-and-Play):** Các DER có thể kết nối hoặc ngắt kết nối linh hoạt khỏi thanh cái vi lưới mà không cần cấu hình lại hệ thống. |
| **Độ chính xác chia sẻ $P/Q$** | **Rất cao (Sai số $< 0.1\%$):** Do tín hiệu dòng/công suất tham chiếu được chia sẻ đồng bộ từ một nguồn chuẩn duy nhất. | **Trung bình - Thấp (Sai số $3\% - 8\%$ đối với $Q$):** Phụ thuộc mạnh vào sai lệch trở kháng đường dây ngoại vi ($Z_{i} \neq Z_{j}$) và độ trôi cảm biến đo lường. |
| **Chất lượng Điện áp & Tần số (Steady-state Deviation)** | **Hoàn hảo ($f = f_0, V = V_0$):** Không có sự sụt giảm tĩnh ở chế độ xác lập do có vòng lặp bù lỗi tích phân từ EMS trung tâm. | **Có độ suy giảm tĩnh (Steady-state Droop Deviation):** Tần số và điện áp thanh cái bị sụt giảm theo tải: $\Delta f \le 0.5\,\text{Hz}, \Delta V \le 5\%$. |
| **Cơ chế phục hồi thứ cấp (Secondary Restoration)** | Tích hợp sẵn trong vòng điều khiển trung tâm (Centralized Secondary Control). | Đòi hỏi phải tích hợp thêm một lớp điều khiển thứ cấp phân tán (Distributed Secondary Control qua consensus protocol) để khôi phục $f$ và $V$. |

#### Phân tích toán học cơ chế Droop không đường truyền:
Mỗi thiết bị tự động điều chỉnh tần số góc $\omega_{i}$ và biên độ điện áp ngõ ra $V_{i}$ tuân theo phương trình đặc tính độ dốc:

$$\omega_{i} = \omega_{0} - m_{p,i} (P_{i} - P_{0,i}), \quad V_{i} = V_{0} - n_{q,i} (Q_{i} - Q_{0,i})$$

Trong đó, hệ số độ dốc công suất tác dụng $m_{p,i}$ và công suất phản kháng $n_{q,i}$ được thiết lập tỷ lệ nghịch với công suất biểu kiến định mức $S_{rated,i}$ nhằm bảo đảm chia sẻ tải tỷ lệ theo công suất thiết bị:

$$m_{p,1} S_{rated,1} = m_{p,2} S_{rated,2} = \dots = m_{p,n} S_{rated,n} = \Delta \omega_{max}$$

$$n_{q,1} S_{rated,1} = n_{q,2} S_{rated,2} = \dots = n_{q,n} S_{rated,n} = \Delta V_{max}$$

---

### 3. Tiêu chuẩn IEEE 1547 & IEC 61850 về Độ lệch Công suất Tối đa giữa các DER Song song
Việc tương tác giữa các bộ nghịch lưu chia sẻ công suất với tài sản vật lý lưới (`[Grid_Physical_Assets](obsidian://open?file=Grid_Physical_Assets)`) phải tuân thủ nghiêm ngặt các quy định về khả năng tương tác, giới hạn sai số và thời gian phản hồi theo tiêu chuẩn IEEE và IEC.

#### a. Tiêu chuẩn IEEE 1547-2018 (IEEE Standard for Interconnection and Interoperability of DERs)
* **Độ lệch công suất chia sẻ tối đa cho phép (Maximum Allowable Power Sharing Deviation):**
  * **Công suất tác dụng ($P$):** Sai số chia sẻ công suất tác dụng giữa các mô-đun DER vận hành song song tại PCC không được vượt quá **$\pm 5.0\%$** công suất định mức ($P_{rated}$) khi tải tổng hệ thống vượt quá $20\%$ công suất danh định.
  * **Công suất phản kháng ($Q$):** Độ lệch chia sẻ công suất phản kháng và độ lớn dòng vô công quẩn giữa các nghịch lưu không được vượt quá **$\pm 7.5\%$** công suất phản kháng danh định ($Q_{rated}$) hoặc mức giới hạn làm cho tổng trở sai lệch điện áp vượt quá $\pm 2.5\%$ so với điện áp danh định.
* **Thời gian đáp ứng động lực học (Step Response Time & Settling Time):**
  * Theo phân loại **Category B** (dành cho các DER điện tử công suất có khả năng hỗ trợ lưới cao), khi xảy ra biến động bước của phụ tải ($ \Delta P_{load} \ge 50\% P_{rated} $), thời gian lập lại cân bằng chia sẻ công suất (settling time với dải sai số $\pm 5\%$) phải đạt trong phạm vi **$100\,\text{ms} - 500\,\text{ms}$**.
  * Thời gian phản hồi ban đầu (Open-loop response time) không được chậm hơn **$100\,\text{ms}$**.

#### b. Tiêu chuẩn IEC 61850-7-420 (Communication networks and systems for power utility automation - Logical nodes for DERs)
* **Mô hình hóa Nút Logic Tương tác (Logical Node Modeling):**
  * Nút `DPGC` *(DER Power Generation Control)*: Điều khiển và quản lý lệnh tham chiếu công suất chia sẻ tác dụng ($P_{ref}$) và phản kháng ($Q_{ref}$).
  * Nút `DOPM` *(DER Operating Mode)*: Xử lý trạng thái chuyển đổi liền mạch (Seamless Transfer) giữa chế độ chia sẻ nối lưới (Grid-Connected P-Q Sharing) và chế độ chia sẻ tự trị vi lưới (Islanded Droop Sharing).
  * Nút `ZBAT` và `ZGEN`: Giám sát thông số an toàn thực tế từ pin lưu trữ `[BESS](obsidian://open?file=BESS)` và nguồn phát tự động.
* **Yêu cầu lớp hiệu năng thông tin (Communication Performance Class theo IEC 61850-5):**
  * Đối với tương tác bảo vệ khóa chéo và cắt dòng quẩn nguy cấp: Sử dụng giao thức **GOOSE** *(Generic Object Oriented Substation Events)* thuộc lớp **P1/P2** với thời gian trễ truyền tin tối đa **$\tau_{comm} \le 4\,\text{ms}$**.
  * Đối với tương tác điều phối chia sẻ công suất song song đồng bộ nhanh (Fast Power Sharing Coordination): Sử dụng gói tin SV/GOOSE với thời gian trễ tối đa **$\tau_{comm} \le 10\,\text{ms}$**.

---

## III. ĐẠO - TRIẾT LÝ VẬT LÝ & KIẾN TRÚC HỆ THỐNG
*(AI Agent dự thảo: Meta-Agent - Chờ N.H.Anh k66 hiệu chỉnh)*

1. **Sự hợp nhất của Âm và Dương trong Chia sẻ Công suất:**
   Khâu Âm xác lập trật tự dòng điện ngõ ra thông qua mô hình không gian trạng thái $d-q$, luật điều khiển chia sẻ tỷ lệ định mức và bộ triệt tiêu dòng quẩn (CCSC) tự trị bên trong bán dẫn. Khâu Dương mang trật tự đó ra môi trường vi lưới `[Microgrid_Busbar](obsidian://open?file=Microgrid_Busbar)`, giải quyết xung đột trở kháng đường dây $X/R$ bằng trở kháng ảo và duy trì độ sai lệch theo nghiêm ngặt tiêu chuẩn IEEE 1547-2018.
2. **Triết lý Dân chủ & Tập trung trong Cân bằng Năng lượng:**
   - Khi vận hành **Master-Slave (EMS tập trung - Có đường truyền)**, hệ thống đạt độ chính xác chia sẻ tuyệt đối $< 0.1\%$ nhưng đánh đổi bằng điểm lỗi đơn và độ trễ thông tin.
   - Khi vận hành **Droop Control phi tập trung (Không đường truyền)**, các bộ nghịch lưu tương tác tự trị qua tần số và điện áp thanh cái theo cơ chế "cân bằng tự nhiên", mang lại độ tin cậy tuyệt đối (Plug-and-Play) và không có điểm lỗi đơn.
3. **Ý nghĩa Bản thể trong Vũ trụ Lưới điện:**
   `[Power_Sharing](obsidian://open?file=Power_Sharing)` là nền tảng biến các đơn vị nguồn năng lượng phân tán (`[DER](obsidian://open?file=DER)`) từ những cá thể rời rạc thành một khối thống nhất trong `[Grid_Physical_Assets](obsidian://open?file=Grid_Physical_Assets)`, đảm bảo sự ổn định dòng điện và công suất trước những biến động không ngừng của phụ tải.
