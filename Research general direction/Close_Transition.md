---
contributors: 
  - N.H.Anh k66
---
Direct Parent Connection: -> [[Seamless_Transition]]

# CLOSE TRANSITION (CHUYỂN ĐỔI KÍN / MAKE-BEFORE-BREAK)
*(Nút Thực thể Lớp 2: Cơ chế chuyển mạch đóng trước cắt sau, điều khiển khóa pha trước đồng bộ Pre-Synchronization Loop giữa Inverter và Lưới AC).*

---

## I. KHÂU ÂM (PURE ENTITY): BẢN CHẤT VẬT LÝ VÀ NGUYÊN LÝ HOẠT ĐỘNG
*(Phân tích độc lập nội tại bởi `pure_entity_researcher.md` — Không tham chiếu môi trường ngoại vi).*

### 1. Phương trình vòng điều khiển đồng bộ trước (Pre-synchronization Loop) trong hệ tọa độ $\alpha-\beta$ và $d-q$
Trước khi thực hiện đóng khóa chuyển mạch $S_{sw}$ nối mạch lọc $L_f - C_f$ với nguồn áp ngoại sinh $v_e(t)$, vector điện áp tụ lọc nội tại $\mathbf{v}_C = [v_{C\alpha}, v_{C\beta}]^T$ phải được đồng bộ hóa hoàn toàn với vector điện áp ngoại sinh $\mathbf{v}_e = [v_{e\alpha}, v_{e\beta}]^T$ trong hệ tọa độ tĩnh $\alpha-\beta$.

Thực hiện phép biến đổi Park (Park Transformation) dựa trên góc pha tức thời nội tại $\theta_C(t) = \int \omega_C(\tau) d\tau$ của bộ tạo dao động nghịch lưu:
$$\mathbf{T}(\theta_C) = \begin{bmatrix} \cos\theta_C & \sin\theta_C \\ -\sin\theta_C & \cos\theta_C \end{bmatrix}$$

Chuyển vector điện áp nguồn ngoại sinh $\mathbf{v}_e$ sang hệ tọa độ quay $d-q$ đồng bộ với góc pha nội tại $\theta_C$:
$$\begin{bmatrix} v_{ed} \\ v_{eq} \end{bmatrix} = \mathbf{T}(\theta_C) \begin{bmatrix} v_{e\alpha} \\ v_{e\beta} \end{bmatrix} = V_e \begin{bmatrix} \cos(\theta_e - \theta_C) \\ \sin(\theta_e - \theta_C) \end{bmatrix}$$

Trong điều kiện sai lệch góc pha ban đầu nhỏ ($\Delta \theta = \theta_e - \theta_C \ll 1 \, \text{rad}$), khai triển Taylor cho phép xấp xỉ tuyến tính hóa hệ phương trình vector điện áp trên trục $d-q$:
$$v_{ed} \approx V_e, \quad v_{eq} \approx V_e (\theta_e - \theta_C) = V_e \Delta \theta$$
Thành phần điện áp trục hoành $v_{ed}$ đại diện cho biên độ điện áp ngoại sinh, trong khi thành phần trục tung $v_{eq}$ tỷ lệ thuận trực tiếp với sai lệch góc pha giữa hai thực thể.

### 2. Hàm truyền và luật điều khiển PI triệt tiêu sai lệch pha ($\Delta \theta \to 0$), biên độ ($\Delta V \to 0$) và tần số ($\Delta f \to 0$)
Cấu trúc điều khiển đồng bộ trước (Pre-synchronization Controller) bao gồm hai vòng điều khiển phản hồi độc lập: vòng khóa pha-tần số (Phase-Frequency Loop) và vòng cân bằng biên độ (Voltage Magnitude Loop).

#### A. Vòng khóa pha và tần số (Phase & Frequency Synchronization Loop)
Tín hiệu sai lệch pha $\varepsilon_\theta(t) = \frac{v_{eq}(t)}{V_e} \approx \Delta \theta(t)$ được đưa qua bộ điều khiển tỷ lệ - tích phân (PI Controller) để tạo ra lượng bù tần số góc $\Delta \omega(t)$:
$$\Delta \omega(s) = G_{PI,\theta}(s) \varepsilon_\theta(s) = \left( k_{p,\theta} + \frac{k_{i,\theta}}{s} \right) \Delta \theta(s)$$

Tần số góc tham chiếu của bộ nghịch lưu được cập nhật liên tục: $\omega_C(t) = \omega_0 + \Delta \omega(t)$, với $\omega_0$ là tần số trung tâm danh định. Phương trình vi phân động học của góc pha nội tại trong miền Laplace:
$$\theta_C(s) = \frac{1}{s} \omega_C(s) = \frac{1}{s} \left[ \omega_0(s) + \left( k_{p,\theta} + \frac{k_{i,\theta}}{s} \right) (\theta_e(s) - \theta_C(s)) \right]$$

Hàm truyền vòng kín của sai lệch pha $H_{\Delta \theta}(s) = \frac{\Delta \theta(s)}{\theta_e(s)}$ được mô tả bởi hệ thống bậc hai:
$$H_{\Delta \theta}(s) = \frac{s^2}{s^2 + k_{p,\theta} s + k_{i,\theta}}$$

Theo định lý giá trị cuối (Final Value Theorem) đối với đầu vào bước nhảy tần số $\Delta \omega_e / s$, sai lệch pha xác lập và sai lệch tần số xác lập tiến về 0 tuyệt đối:
$$\lim_{t \to \infty} \Delta \theta(t) = \lim_{s \to 0} s \cdot \frac{s^2}{s^2 + k_{p,\theta} s + k_{i,\theta}} \cdot \frac{\Delta \omega_e}{s^2} = 0 \implies \lim_{t \to \infty} \Delta f(t) = 0$$

#### B. Vòng đồng bộ biên độ điện áp (Voltage Magnitude Synchronization Loop)
Sai lệch biên độ giữa điện áp ngoại sinh và điện áp tụ lọc nội tại được xác định bởi:
$$\varepsilon_V(t) = \sqrt{v_{ed}^2(t) + v_{eq}^2(t)} - \sqrt{v_{Cd}^2(t) + v_{Cq}^2(t)} = V_e(t) - V_C(t)$$

Luật điều khiển PI biên độ điều chỉnh giá trị đặt điện áp ngõ ra của bộ nghịch lưu $\Delta V^*(t)$:
$$\Delta V^*(s) = \left( k_{p,V} + \frac{k_{i,V}}{s} \right) \varepsilon_V(s) \implies \lim_{t \to \infty} \varepsilon_V(t) = \lim_{s \to 0} s \cdot \frac{1}{1 + \left( k_{p,V} + \frac{k_{i,V}}{s} \right)} \cdot \frac{\Delta V_e}{s} = 0$$
Hệ thống đồng bộ trước đưa toàn bộ vector trạng thái sai lệch $\mathbf{e}_{sync} = [\Delta \theta, \Delta V, \Delta f]^T \to \mathbf{0}$ một cách tiệm cận ổn định.

### 3. Điều kiện giải tích để triệt tiêu dòng quá độ đóng mạch ($i_{rush} \approx 0$)
Tại thời điểm đóng khóa cơ điện $S_{sw}$ ($t = t_{close}$), phương trình vi phân mô tả dòng điện chạy quẩn quá độ $\mathbf{i}_{rush}(t)$ qua tổng trở ghép nối $\mathbf{Z}_c = R_c + j\omega L_c$ (bao gồm điện trở tiếp điểm $R_c$ và điện cảm đường dây $L_c$) trong hệ tọa độ $\alpha-\beta$ tuân theo phương trình:
$$L_c \frac{d\mathbf{i}_{rush}(t)}{dt} + R_c \mathbf{i}_{rush}(t) = \Delta \mathbf{v}(t) = \mathbf{v}_C(t) - \mathbf{v}_e(t)$$

Nghiệm tổng quát của phương trình vi phân dòng quá độ cho thời điểm $t \ge t_{close}$ được giải tích như sau:
$$\mathbf{i}_{rush}(t) = \mathbf{i}_{rush}(t_{close}^+) e^{-\frac{R_c}{L_c}(t - t_{close})} + \frac{1}{L_c} \int_{t_{close}}^t e^{-\frac{R_c}{L_c}(t - \tau)} \Delta \mathbf{v}(\tau) d\tau$$

Trong điều kiện mạch hở trước khi đóng khóa, dòng điện ban đầu bằng không: $\mathbf{i}_{rush}(t_{close}^-) = \mathbf{0}$. Để triệt tiêu hoàn toàn xung dòng điện đóng mạch ($i_{rush}(t) \approx 0, \forall t \ge t_{close}$), hàm kích thích áp $\Delta \mathbf{v}(t)$ và đạo hàm bậc nhất của nó phải đồng thời thỏa mãn hệ **Điều kiện giải tích không xung đột (Zero-Transient Make-Before-Break Analytical Conditions)**:
1. **Điều kiện đẳng thế vector thời điểm đóng (Zero Voltage Vector Mismatch):**
$$\Delta \mathbf{v}(t_{close}^-) = \mathbf{0} \iff \begin{cases} V_C(t_{close}^-) = V_e(t_{close}^-) \\ \theta_C(t_{close}^-) = \theta_e(t_{close}^-) \end{cases}$$
2. **Điều kiện đồng biến vi phân tần số - pha (Zero dV/dt Derivative Mismatch):**
$$\left. \frac{d\Delta \mathbf{v}(t)}{dt} \right|_{t=t_{close}} = \mathbf{0} \iff \omega_C(t_{close}^-) = \omega_e(t_{close}^-)$$

Khi hệ điều kiện giải tích (1) và (2) được thỏa mãn tuyệt đối tại điểm ngắt/mở tiếp điểm, thành phần dao động cưỡng bức (Forced Response) và thành phần dao động tự do (Natural Response) của phương trình vi phân dòng điện triệt tiêu hoàn toàn:
$$\mathbf{i}_{rush}(t_{close}^+) = \mathbf{0}, \quad \left. \frac{d\mathbf{i}_{rush}(t)}{dt} \right|_{t=t_{close}^+} = \mathbf{0} \implies \mathbf{i}_{rush}(t) \equiv \mathbf{0} \quad (\forall t \ge t_{close})$$
Đây là nền tảng giải tích cốt lõi cho cơ chế chuyển đổi kín êm ái (Make-before-break Bumpless Coupling), loại bỏ hoàn toàn ứng suất dòng điện lên các van bán dẫn công suất và tụ lọc LC.

---

## II. KHÂU DƯƠNG (LOCAL INTERACTION): TƯƠNG TÁC TRƯỜNG VẬT LÝ & LƯỚI ĐIỆN CỤC BỘ
*(Phân tích tương tác cục bộ bởi `grid_interaction_researcher.md` — Môi trường bọc: Lưới điện Không gian - Vật lý `Grid_Physical_Assets`).*

### 1. Quy trình khớp nối cơ cấu MCCB/SSR nối lưới & Điều kiện đồng bộ hoàn hảo
Quy trình tái kết nối lưới đòi hỏi một trình tự phối hợp cơ - điện tử nghiêm ngặt qua 3 bước cốt lõi giữa hệ thống nghịch lưu vi lưới, thiết bị đóng cắt tốc độ cao (máy cắt cơ khí `[MCCB](obsidian://open?file=MCCB)` hoặc rơ-le tĩnh bán dẫn `[SSR](obsidian://open?file=SSR)`) và thanh cái lưới:
1. **Bước 1 (Grid Sensing & Pre-Synchronization Trigger):** Khi điện áp lưới chính khôi phục và ổn định trong tối thiểu 60 giây (theo `[IEEE_1547](obsidian://open?file=IEEE_1547)`), bộ điều khiển vi lưới (`[MGCC](obsidian://open?file=MGCC)`) kích hoạt module điều khiển đồng bộ hóa trước (*Pre-synchronization Controller*) trên bộ nghịch lưu `[GFM_Inverter](obsidian://open?file=GFM_Inverter)`.
2. **Bước 2 (Phase, Frequency & Voltage Matching):** Vòng lặp PLL thứ cấp đo lường chính xác vectơ điện áp phía thượng nguồn lưới ($\mathbf{V}_{\text{grid}} = V_{\text{grid}} \angle \theta_{\text{grid}}$) và phía thanh cái vi lưới ($\mathbf{V}_{\text{MG}} = V_{\text{MG}} \angle \theta_{\text{MG}}$). Thuật toán PI đồng bộ hiệu chỉnh góc pha $\theta_{\text{MG}}$ và biên độ $V_{\text{MG}}$ của vi lưới theo phương thức bù tích phân:
   $$\Delta \omega_{\text{sync}} = k_{p,\theta} (\theta_{\text{grid}} - \theta_{\text{MG}}) + k_{i,\theta} \int (\theta_{\text{grid}} - \theta_{\text{MG}}) dt$$
   $$\Delta V_{\text{sync}} = k_{p,v} (V_{\text{grid}} - V_{\text{MG}}) + k_{i,v} \int (V_{\text{grid}} - V_{\text{MG}}) dt$$
3. **Bước 3 (Seamless Clasping & Mechanical/Solid-State Closing):** Khi sai lệch vectơ điện áp lọt vào "Cửa sổ đồng bộ hoàn hảo" (*Seamless Synchronization Window*), lệnh kích hoạt được gửi tới cuộn đóng của `[MCCB](obsidian://open?file=MCCB)` (với thời gian bù trễ cơ khí $t_{\text{mech\_delay}} \approx 35 \text{ ms}$) hoặc phát xung cổng IGBT/Thyristor của `[SSR](obsidian://open?file=SSR)` (thực hiện đóng mạch tức thời trong $< 1 \text{ ms}$).

### 2. Phối hợp bảo vệ chống dòng đảo ngược & Triệt tiêu xung đột nguồn áp
Trong cơ chế `Make-before-break`, rủi ro lớn nhất là hiện tượng **Xung đột nguồn áp (Voltage Source Clashing)**. Khi hai nguồn điện áp có tổng trở nhỏ ($Z_{\text{grid}}$ và $Z_{\text{inv}}$) đấu song song trực tiếp, một sai lệch rất nhỏ về biên độ hay góc pha sẽ gây ra dòng tuần hoàn quá độ (*Circulating Inrush Current*) có biên độ cực lớn:
$$I_{\text{circ}}(t) = \frac{\mathbf{V}_{\text{grid}} - \mathbf{V}_{\text{MG}}}{Z_{\text{grid}} + Z_{\text{inv}} + Z_{\text{line}}} = \frac{\Delta V \angle \Delta \theta}{R_{\text{total}} + j \omega L_{\text{total}}}$$

- **Bảo vệ dòng công suất đảo ngược (Reverse Power Protection theo IEC 60255-149):** Tại `[PCC_Switch](obsidian://open?file=PCC_Switch)`, rơ-le định hướng công suất (ANSI 32R/32F) giám sát luồng công suất tác dụng/phản kháng. Nếu quá trình đóng nối mạch gây ra hiện tượng nạp ngược công suất ngắn mạch không kiểm soát từ lưới vào bộ nghịch lưu hoặc ngược lại ($P_{\text{rev}} > 5\% P_{\text{rated}}$ trong thời gian $> 15 \text{ ms}$), rơ-le lập tức gửi tín hiệu *Trip* để ngắt mạch an toàn.
- **Chuyển dịch chế độ điều khiển mềm (Soft Mode Transition):** Ngay thời điểm tiếp điểm phụ (*Auxiliary Contact* $52a/52b$) của `[PCC_Switch](obsidian://open?file=PCC_Switch)` xác nhận khép kín, bộ nghịch lưu `[GFM_Inverter](obsidian://open?file=GFM_Inverter)` thực hiện chuyển đổi mượt từ chế độ Điều khiển áp (VC-GFM) sang chế độ Điều khiển dòng/công suất (`[GFL_Inverter](obsidian://open?file=GFL_Inverter)` - PQ Control) hoặc chế độ tự đồng bộ tổng trở ảo (*Virtual Impedance Droop*) nhằm triệt tiêu hoàn toàn sự tranh chấp nguồn áp trên thanh cái.
- **Bộ cản trở quá độ ảo (Virtual Transient Damper):** Mạch điều khiển bổ sung giá trị điện trở ảo định kỳ $R_v$ vào khâu phản hồi dòng điện bộ nghịch lưu trong suốt $50 \text{ ms}$ sau thời điểm đóng mạch để dập tắt nhanh các thành phần dòng một chiều (DC offset) và dao động dòng tuần hoàn.

### 3. Tiêu chuẩn chất lượng điện năng & Ổn định góc lệch công suất tác dụng (IEEE 1547-2018)
Quá trình hòa lưới kín lại phải bảo đảm tuân thủ nghiêm ngặt các giới hạn thông số đồng bộ theo tiêu chuẩn **`[IEEE_1547](obsidian://open?file=IEEE_1547)` (IEEE 1547-2018 Clause 4.10 - Table 5: DER Synchronization Parameter Limits)**:

| Tổng công suất DER định mức | Sai lệch tần số ($\Delta f = |f_{\text{MG}} - f_{\text{grid}}|$) | Sai lệch điện áp ($\Delta V = \frac{|V_{\text{MG}} - V_{\text{grid}}|}{V_{\text{nom}}}$) | Sai lệch góc pha ($\Delta \theta = |\theta_{\text{MG}} - \theta_{\text{grid}}|$) |
| :--- | :--- | :--- | :--- |
| **$0 \text{ kVA} - 500 \text{ kVA}$** | $\le 0.3 \text{ Hz}$ | $\le 10\%$ | $\le 20^\circ$ |
| **$> 500 \text{ kVA} - 1500 \text{ kVA}$** | **$\le 0.2 \text{ Hz}$** | **$\le 5\%$** | **$\le 15^\circ$** |
| **$> 1500 \text{ kVA}$ (hoặc cụm DER lớn)** | **$\le 0.1 \text{ Hz}$** | **$\le 3\%$** | **$\le 10^\circ$** |

**Ổn định góc lệch công suất tác dụng (Active Power Angle Damping & Stability):**
Phương trình dao động công suất tác dụng giữa vi lưới và lưới chính ngay sau thời điểm khép tiếp điểm PCC được mô tả theo phương trình vi phân góc rô-to ảo của bộ `[GFM_Inverter](obsidian://open?file=GFM_Inverter)`:
$$J_{\text{vsm}} \frac{d^2 \Delta \delta}{dt^2} + D_{\text{vsm}} \frac{d \Delta \delta}{dt} + \frac{V_{\text{MG}} V_{\text{grid}}}{X_{\text{eq}}} \cos(\delta_0) \Delta \delta = \Delta P_{\text{in}}$$
trong đó:
- $J_{\text{vsm}}$ là quán tính ảo của bộ tạo lưới, $D_{\text{vsm}}$ là hệ số cản dao động công suất.
- $\delta_0 = \theta_{\text{MG}} - \theta_{\text{grid}}$ là góc lệch ban đầu ngay trước thời điểm tiếp xúc mạch.
- $X_{\text{eq}} = X_{\text{grid}} + X_{\text{inv}} + X_{\text{line}}$ là tổng điện kháng cảm tương đương mạch vòng.

**Điều kiện ổn định và chất lượng điện năng:**
- Hệ số cản $D_{\text{vsm}}$ được thiết kế tối ưu hóa để đảm bảo tỷ số tắt dần (Damping Ratio) $\zeta = \frac{D_{\text{vsm}}}{2 \sqrt{J_{\text{vsm}} \cdot \frac{V_{\text{MG}} V_{\text{grid}}}{X_{\text{eq}}} \cos \delta_0}} \ge 0.707$, triệt tiêu hoàn toàn dao động góc lệch công suất trong vòng $100 \text{ ms} - 200 \text{ ms}$.
- Đảm bảo độ méo dạng tổng sóng hài điện áp (`[THD](obsidian://open?file=THD)`) trên thanh cái vi lưới đạt $< 5\%$ và không gây hiện tượng nhấp nháy điện áp (*Flicker*) vượt tiêu chuẩn IEC 61000-3-3 trong toàn bộ chu kỳ hòa lưới lại.

---

## III. GHI CHÚ THẢO LUẬN & CHUYÊN GIA
- **Đóng góp chuyên môn:** Đồng chí **N.H.Anh k66** chủ trì nghiên cứu thiết bị Điện tử công suất trong hệ thống lưới điện hiện đại.
- **Trạng thái tài liệu:** *(AI Agent dự thảo: `pure_entity_researcher.md` & `grid_interaction_researcher.md` — Chờ đồng chí N.H.Anh k66 hiệu chỉnh và thẩm định công thức)*.
