---
weighttree: 0
contributors: 
  - N.H.Anh k66
---
Direct Parent Connection: -> [[Seamless_Transition]]

# OPEN TRANSITION (CHUYỂN ĐỔI HỞ / BREAK-BEFORE-MAKE)
*(Nút Thực thể Lớp 2: Cơ chế chuyển mạch ngắt cứng mạch nối lưới trước khi tái thiết lập điện áp tự trị bằng Grid-Forming trong Inverter & Microgrid).*

---

## I. KHÂU ÂM (PURE ENTITY): BẢN CHẤT VẬT LÝ VÀ NGUYÊN LÝ HOẠT ĐỘNG
*(Phân tích độc lập nội tại bởi `pure_entity_researcher.md` — Không tham chiếu môi trường ngoại vi).*

### 1. Cấu trúc vật lý và Phương trình vi phân quá độ khi ngắt khóa cơ điện MCCB (Hard Switching)
Xét cấu trúc bộ nghịch lưu bán dẫn nguồn áp (Voltage-Source Inverter - VSI) kết nối với nguồn điện áp xoay chiều ngoại sinh thông qua bộ lọc thông thấp $L_f - C_f$ (hệ số cảm kháng bộ lọc $L_f$, điện trở cuộn dây $R_f$, điện dung tụ lọc $C_f$) và khóa chuyển mạch cơ điện MCCB ($S_{sw}$) có điện cảm ký sinh đường dây/tiếp điểm $L_{stray}$. 

Tại thời điểm trước ngắt khóa $t = t_0^-$, dòng điện chạy qua khóa $i_o(t)$ và điện áp tụ lọc $v_C(t)$ xác lập theo phương trình cân bằng định luật Kirchhoff (KVL/KCL):
$$L_f \frac{di_L(t)}{dt} + R_f i_L(t) + v_C(t) = v_i(t)$$
$$i_L(t) = C_f \frac{dv_C(t)}{dt} + i_o(t)$$
$$L_{stray} \frac{di_o(t)}{dt} + R_{stray} i_o(t) + v_e(t) = v_C(t)$$
trong đó $v_i(t)$ là điện áp ngõ ra của cầu nghịch lưu bán dẫn, $v_e(t)$ là điện áp nguồn ngoại sinh.

Khi khóa chuyển mạch MCCB thực hiện ngắt cứng (Hard Switching) tại thời điểm $t = t_0$, tiếp điểm cơ khí mở ra đột ngột buộc dòng điện ngõ ra phải triệt tiêu: $i_o(t_0^+) = 0$. Do tốc độ biến thiên dòng điện cực lớn ($\frac{di_o}{dt} \to -\infty$), toàn bộ năng lượng từ trường lưu trữ trong điện cảm ký sinh $L_{stray}$ phóng thích ngược vào mạch lọc, gây ra hiện tượng phóng điện hồ quang hoặc xung điện áp quá độ (Voltage Spike) tuân theo phương trình vi phân phi tuyến:
$$v_{sw}(t) = v_C(t) - v_e(t) - L_{stray} \frac{di_o(t)}{dt}$$

Bỏ qua tổn hao tiếp điểm ($R_{stray} \approx 0$), bước nhảy điện áp vọt lố đặt lên tụ lọc $C_f$ ngay sau thời điểm ngắt mạch được xác định theo định luật bảo toàn năng lượng từ-điện trường:
$$\frac{1}{2} L_{stray} i_o^2(t_0^-) = \frac{1}{2} C_f \left[ \Delta V_{peak}^2 + 2 V_{C0} \Delta V_{peak} \right]$$
$$\Delta V_{peak} = \sqrt{V_{C0}^2 + \frac{L_{stray}}{C_f} i_o^2(t_0^-)} - V_{C0} \approx i_o(t_0^-) \sqrt{\frac{L_{stray}}{C_f}}$$
với $V_{C0} = v_C(t_0^-)$ là giá trị điện áp tụ thời điểm trước khi mở tiếp điểm.

### 2. Phương trình phóng/nạp tụ lọc và dao động tự do (Natural Response) khi mất tham chiếu ngoại sinh
Ngay sau thời điểm cô lập ($t > t_0, i_o(t) = 0$), mạch lọc $L_f - C_f$ chuyển sang chế độ dao động tự do không tải được kích thích bởi điều kiện đầu trạng thái $i_L(t_0^+) = i_{L0}$ và $v_C(t_0^+) = v_{C0}$. Hệ phương trình vi phân không gian trạng thái (State-Space Representation) của mạch lọc nội tại được biểu diễn dưới dạng:
$$\frac{d}{dt} \mathbf{x}(t) = \mathbf{A} \mathbf{x}(t) + \mathbf{B} v_i(t)$$
$$\mathbf{x}(t) = \begin{bmatrix} i_L(t) \\ v_C(t) \end{bmatrix}, \quad \mathbf{A} = \begin{bmatrix} -\frac{R_f}{L_f} & -\frac{1}{L_f} \\ \frac{1}{C_f} & 0 \end{bmatrix}, \quad \mathbf{B} = \begin{bmatrix} \frac{1}{L_f} \\ 0 \end{bmatrix}$$

Khi tín hiệu điều khiển mất điện áp tham chiếu ngoại sinh, phương trình đặc trưng (Characteristic Equation) của ma trận hệ thống $\mathbf{A}$ xác định tần số dao động và độ suy giảm tự do của hệ bậc hai:
$$\det(s\mathbf{I} - \mathbf{A}) = s^2 + \frac{R_f}{L_f} s + \frac{1}{L_f C_f} = 0$$

Hệ số suy giảm (Damping ratio) $\zeta$ và tần số góc tự nhiên không suy giảm (Undamped natural frequency) $\omega_n$ được định nghĩa:
$$\omega_n = \frac{1}{\sqrt{L_f C_f}}, \quad \zeta = \frac{R_f}{2} \sqrt{\frac{C_f}{L_f}}$$

Trong điều kiện mạch lọc có ma sát yếu ($\zeta < 1$), nghiệm dao động điện áp tự do (Natural response) của tụ lọc $C_f$ trong miền thời gian là một dao động điều hòa suy giảm mũ:
$$v_C(t) = e^{-\zeta \omega_n (t - t_0)} \left[ v_{C0} \cos(\omega_d (t - t_0)) + \frac{\zeta \omega_n v_{C0} + \frac{1}{C_f} i_{L0}}{\omega_d} \sin(\omega_d (t - t_0)) \right]$$
trong đó tần số góc dao động có suy giảm là $\omega_d = \omega_n \sqrt{1 - \zeta^2}$. Nếu dòng cuộn cảm và điện áp tụ lệch pha nhau tại thời điểm ngắt, hiệu ứng cộng hưởng nội tại gây ra dao động quá độ áp/dòng tự do (Free Ringing) trước khi bộ điều khiển chuyển sang chế độ tự lập áp.

### 3. Thuật toán khởi động tạo áp tự trị (Autonomous Voltage-Forming Initialization) giảm thiểu vọt lố điện áp
Để ngăn chặn vọt lố điện áp quá độ ($\text{Overvoltage Overshoot}, \%OS \to 0$) do sự gián đoạn của biến trạng thái khi chuyển từ chế độ bám dòng ngoại sinh sang chế độ tạo áp tự trị tại thời điểm $t = t_{init}$, bộ điều khiển áp đặt quỹ đạo khởi động mềm (Soft-start Trajectory / C2-Continuous Reference Initialization).

Thay vì áp dụng bước nhảy nấc thang cho điện áp tham chiếu ngõ ra, hàm tham chiếu điện áp tụ $\mathbf{v}_{C,ref}^*(t)$ trong không gian $\alpha-\beta$ được thiết kế theo luật quy hoạch quỹ đạo hàm mũ điều chỉnh theo hằng số thời gian $\tau_{init}$:
$$\mathbf{v}_{C,ref}^*(t) = V_m^* \left[ 1 - e^{-\frac{t - t_{init}}{\tau_{init}}} \right] \begin{bmatrix} \cos(\omega^* t + \theta_0) \\ \sin(\omega^* t + \theta_0) \end{bmatrix} + \mathbf{v}_C(t_{init}^-) e^{-\frac{t - t_{init}}{\tau_{init}}}$$
trong đó $\theta_0$ và $\mathbf{v}_C(t_{init}^-)$ là góc pha và vector điện áp tụ lọc được lấy mẫu ngay tại thời điểm ngắt mạch.

Đồng thời, để triệt tiêu vi phân bậc nhất của sai lệch điện áp ($\left. \frac{d\mathbf{e}_v(t)}{dt} \right|_{t=t_{init}^+} = \mathbf{0}$), trạng thái tích phân nội bộ $\mathbf{x}_{int}$ của bộ điều khiển PI điện áp (Voltage Loop Controller) được tái khởi tạo (State-Space Pre-conditioning) đúng bằng dòng điện cuộn cảm tham chiếu tức thời:
$$\mathbf{x}_{int}(t_{init}^+) = \mathbf{i}_L(t_{init}^-) - \mathbf{K}_{p,v} \left( \mathbf{v}_{C,ref}^*(t_{init}^+) - \mathbf{v}_C(t_{init}^-) \right) - \mathbf{C}_f \omega^* \mathbf{J} \mathbf{v}_C(t_{init}^-)$$
với $\mathbf{J} = \begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}$ là ma trận trực giao xoay góc $\frac{\pi}{2}$. Luật điều khiển khởi động tự trị này đảm bảo tính liên tục bậc hai (C2-continuity) cho vector điện áp ra, triệt tiêu hoàn toàn thành phần kích thích dao động tự do $\omega_n$, duy trì quá độ điện áp êm ái (Bumpless Transition).

---

## II. KHÂU DƯƠNG (LOCAL INTERACTION): TƯƠNG TÁC TRƯỜNG VẬT LÝ & LƯỚI ĐIỆN CỤC BỘ
*(Phân tích tương tác cục bộ bởi `grid_interaction_researcher.md` — Môi trường bọc: Lưới điện Không gian - Vật lý `Grid_Physical_Assets`).*

### 1. Động lực học chuyển tiếp chế độ từ Bám lưới (GFL) sang Tạo lưới (GFM) trong Microgrid
Khi phát hiện sự cố ngắn mạch, mất điện hay biến động bất thường ở phía nguồn lưới chủ đạo, cơ cấu `[PCC_Switch](obsidian://open?file=PCC_Switch)` (công tắc tĩnh bán dẫn Static Transfer Switch - STS hoặc máy cắt cơ khí tốc độ cao MCCB) mở ra nhằm cô lập vi lưới. Bản chất "Break-before-make" làm cho vi lưới rơi vào trạng thái cô lập tạm thời trước khi hệ thống lưu trữ hoàn tất tái định hình nguồn áp tham chiếu. Động lực học chuyển tiếp xảy ra qua 4 giai đoạn cụ thể:
1. **Giai đoạn 1 ($t = 0^-$ - Tiền chuyển tiếp):** Bộ nghịch lưu cất trữ (`[BESS_Inverter](obsidian://open?file=BESS_Inverter)`) vận hành ở chế độ Bám lưới (`[GFL_Inverter](obsidian://open?file=GFL_Inverter)`), bơm công suất theo dòng tham chiếu $(P_{\text{ref}}, Q_{\text{ref}})$ đồng bộ với lưới thông qua vòng lặp khóa pha (`[PLL](obsidian://open?file=PLL)`):
   $$I_{\text{ref}, d} = \frac{2}{3} \cdot \frac{P_{\text{ref}}}{V_d}, \quad I_{\text{ref}, q} = -\frac{2}{3} \cdot \frac{Q_{\text{ref}}}{V_d}$$
2. **Giai đoạn 2 ($t = 0^+$ đến $t_{\text{detect}}$ - Phát hiện sự cố cô lập):** Tín hiệu phát hiện cô lập lưới chủ động/bị động (theo tiêu chuẩn `[IEEE_1547](obsidian://open?file=IEEE_1547)` / IEC 62116) hoặc tín hiệu bảo vệ rơ-le dưới/quá tần số/điện áp (81U/O, 27/59) nhận diện mất áp chủ lực.
3. **Giai đoạn 3 ($t = t_{\text{open}}$ - Cắt vật lý tại PCC):** Khóa cổng bán dẫn STS tại `[PCC_Switch](obsidian://open?file=PCC_Switch)` mở mạch tại điểm phẩy không dòng (*Zero-Crossing Cutting*) hoặc cuộn ngắt MCCB mở tiếp điểm cơ khí. Vi lưới chính thức bị tách rời (*Islanded*).
4. **Giai đoạn 4 ($t = t_{\text{trans}}$ - Quá trình chuyển sang Tạo lưới):** Bộ điều khiển sơ cấp thực hiện chuyển tiếp cấu trúc (*Mode Switching*) từ chế độ điều khiển dòng (`[GFL_Inverter](obsidian://open?file=GFL_Inverter)`) sang tự định hình áp - tần số (`[GFM_Inverter](obsidian://open?file=GFM_Inverter)`) theo đặc tính độ dốc (*Droop Control*) hoặc Máy đồng bộ ảo (`[VSM](obsidian://open?file=VSM)`). Phương trình mô tả tần số và điện áp tự trị trên thanh cái vi lưới:
   $$\omega_{\text{MG}} = \omega_0 - k_p (P_{\text{meas}} - P_0) + \Delta \omega_{\text{sec}}$$
   $$V_{\text{MG}} = V_0 - k_q (Q_{\text{meas}} - Q_0) + \Delta V_{\text{sec}}$$
   trong đó $k_p, k_q$ lần lượt là hệ số điều chỉnh độ dốc tần số - công suất tác dụng và điện áp - công suất phản kháng; $\Delta \omega_{\text{sec}}, \Delta V_{\text{sec}}$ là tín hiệu hiệu chỉnh từ điều khiển thứ cấp.

### 2. Ràng buộc thời gian gián đoạn & Đường cong chịu đựng phụ tải nhạy cảm (ITIC/CBEMA & IEEE 1547)
Trong cấu trúc chuyển đổi hở, tương tác của nguồn tạo áp (`[GFM_Inverter](obsidian://open?file=GFM_Inverter)`) với các phụ tải công nghiệp nhạy cảm (`[Sensitive_Loads](obsidian://open?file=Sensitive_Loads)`), máy chủ trung tâm dữ liệu và thiết bị điều khiển quy trình sản xuất được giới hạn chặt chẽ theo tiêu chuẩn `[IEEE_1547](obsidian://open?file=IEEE_1547)` (IEEE 1547-2018 Clause 8.2) và Đường cong khả năng chịu đựng của thiết bị công nghệ thông tin (**ITIC Curve** - kế thừa từ đường cong **CBEMA**):

| Thông số / Chỉ tiêu kỹ thuật | Tiêu chuẩn tham chiếu | Giới hạn định lượng cho phép | Đánh giá tương tác hệ thống |
| :--- | :--- | :--- | :--- |
| **Thời gian mất áp tối đa ($\Delta t_{\text{interruption}}$)** | ITIC / CBEMA Curve | **$\le 20 \text{ ms}$** (1 chu kỳ @ 50 Hz) | Nếu $> 20 \text{ ms}$, tụ bus DC của phụ tải điện tử bị xả cạn dưới ngưỡng UVLO gây reset/sập hệ thống. |
| **Thời gian phát hiện cô lập ($t_{\text{detect}}$)** | IEEE 1547-2018 / IEC 62116 | $2 \text{ ms} - 5 \text{ ms}$ | Thuật toán nhận dạng nhanh RoCOF hoặc Vector Shift. |
| **Thời gian tác động ngắt mạch ($t_{\text{switch}}$)** | STS (`[PCC_Switch](obsidian://open?file=PCC_Switch)`) | $< 4 \text{ ms} - 8 \text{ ms}$ | Ngắt bằng thyristor ngược song song tại Zero-Crossing. |
| **Thời gian định hình áp GFM ($t_{\text{GFM\_settle}}$)** | IEC 62477 / IEEE 1547 | $5 \text{ ms} - 10 \text{ ms}$ | Vòng lặp điện áp (Voltage Loop) thiết lập lại từ thông ảo. |
| **Độ sụt áp quá độ tối đa ($\Delta V_{\text{transient}}$)** | IEEE 1547-2018 | $\ge 70\% V_{\text{nom}}$ trong $20 \text{ ms}$ | Bảo đảm nằm trong vùng làm việc an toàn của đường cong ITIC. |

**Phương trình điều kiện ổn định không gián đoạn tải ưu tiên:**
$$\Delta t_{\text{interruption}} = t_{\text{detect}} + t_{\text{switch}} + t_{\text{GFM\_settle}} \le 20 \text{ ms}$$

### 3. Chiến lược sa thải tải không ưu tiên & Duy trì điện áp thanh cái
Khi cô lập lưới theo cơ chế `Open_Transition`, vi lưới phải đối mặt với thâm hụt công suất tức thời giữa tổng tải đang tiêu thụ ($P_{\text{load\_total}}$) và công suất phát cực đại tức thời của nguồn cất trữ (`[BESS](obsidian://open?file=BESS)`):
$$\Delta P_{\text{deficit}} = P_{\text{load\_total}} - \sum P_{\text{DER\_max}}$$
Nếu $\Delta P_{\text{deficit}} > 0$, tần số $\omega_{\text{MG}}$ và điện áp $V_{\text{MG}}$ sẽ suy giảm nhanh theo quán tính tổng tương đương $H_{\text{eq}}$. Để bảo vệ phụ tải ưu tiên cấp 1 (`[Critical_Loads](obsidian://open?file=Critical_Loads)`), vi lưới kích hoạt hệ thống tự động sa thải tải phụ (`[Load_Shedding](obsidian://open?file=Load_Shedding)`) theo ma trận ưu tiên:
1. **Nhóm Tải không ưu tiên (Non-critical / Deferrable Loads - Cấp 3):** Bao gồm trạm sạc xe điện (`[EV_Charging_Station](obsidian://open?file=EV_Charging_Station)`), bình nước nóng, điều hòa không khí (HVAC) tải nhiệt. Bộ điều khiển trung tâm vi lưới (`[MGCC](obsidian://open?file=MGCC)`) gửi tín hiệu ngắt tức thời qua hệ thống công tắc thông minh (`[Smart_Switch](obsidian://open?file=Smart_Switch)`) trong vòng $10 \text{ ms}$ đầu tiên kể từ khi $S_{\text{PCC}} \to 0$.
2. **Nhóm Tải thương mại / Công nghiệp bán ưu tiên (Cấp 2):** Sử dụng thuật toán sa thải theo ngưỡng tần số/điện áp thấp (UFLS / UVLS):
   $$\Delta P_{\text{shed}} \ge \Delta P_{\text{deficit}} + \frac{2 H_{\text{eq}} S_{\text{base}}}{f_0} \cdot \left| \frac{df}{dt} \right|_{\text{limit}}$$
3. **Nhóm Tải ưu tiên tuyệt đối (Critical Loads - Cấp 1):** Trung tâm xử lý dữ liệu, thiết bị y tế, tự động hóa an ninh. Được duy trì liên tục bởi đặc tính bơm dòng cực nhanh từ nguồn siêu tụ (`[Supercapacitor](obsidian://open?file=Supercapacitor)`) tích hợp trong `[HESS](obsidian://open?file=HESS)` kết hợp với bộ điều khiển điện trở ảo bổ trợ (*Virtual Impedance & Voltage Feedforward*) trên bộ nghịch lưu `[GFM_Inverter](obsidian://open?file=GFM_Inverter)`, giữ độ sụt áp thanh cái $< 10\% V_{\text{nom}}$.

---

## III. GHI CHÚ THẢO LUẬN & CHUYÊN GIA
- **Đóng góp chuyên môn:** Đồng chí **N.H.Anh k66** chủ trì nghiên cứu thiết bị Điện tử công suất trong hệ thống lưới điện hiện đại.
- **Trạng thái tài liệu:** *(AI Agent dự thảo: `pure_entity_researcher.md` & `grid_interaction_researcher.md` — Chờ đồng chí N.H.Anh k66 hiệu chỉnh và thẩm định công thức)*.
