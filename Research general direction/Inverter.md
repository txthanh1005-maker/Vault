---
contributors: 
  - N.H.Anh k66
---
Direct Parent Connection: -> [[Power_Electronics]]

# INVERTER (NGHỊCH LƯU / BI-DIRECTIONAL INVERTER)
*(Nút Thực thể Lớp 2: Bộ biến đổi điện áp một chiều sang xoay chiều DC/AC hai chiều, cổng giao tiếp công suất chủ chốt cho Nguồn phân tán, Lưu trữ và Xe điện).*

---

## I. KHÂU ÂM (PURE ENTITY): BẢN CHẤT VẬT LÝ BÁN DẪN & ĐIỀU CHẾ XUNG NGHỊCH LƯU
*(Phân tích độc lập nội tại bởi `pure_entity_researcher.md` — Không tham chiếu môi trường ngoại vi).*

### 1. Cấu trúc Mạch van Nghịch lưu Cầu H & Ba pha (H-Bridge & 3-Phase VSI Topology)
Bộ biến đổi nguồn áp ba pha (Voltage Source Inverter - VSI) được cấu tạo từ 6 van bán dẫn công suất cao (IGBT, MOSFET hoặc SiC-MOSFET), ký hiệu $S_1 \dots S_6$, mắc thành 3 nhánh cầu song song với nguồn điện áp một chiều $V_{dc}$. Mỗi van được tích hợp một đi-ốt phản hồi nghịch (Freewheeling Diode) để dẫn dòng cảm kháng ngược.

Hàm chuyển mạch (Switching Function) của nhánh pha $x \in \{a, b, c\}$ được định nghĩa:
$$S_x = \begin{cases} 1, & \text{Van trên dẫn (Top switch ON), van dưới ngắt} \\ 0, & \text{Van dưới dẫn (Bottom switch ON), van trên ngắt} \end{cases}$$
Điện áp pha đối với điểm trung tính tải $n$ (trong trường hợp tải cân bằng) được xác định bởi phương trình ma trận:
$$\begin{bmatrix} v_{an} \\ v_{bn} \\ v_{cn} \end{bmatrix} = \frac{V_{dc}}{3} \begin{bmatrix} 2 & -1 & -1 \\ -1 & 2 & -1 \\ -1 & -1 & 2 \end{bmatrix} \begin{bmatrix} S_a \\ S_b \\ S_c \end{bmatrix}$$

### 2. Kỹ thuật Điều chế Độ rộng Xung Không gian Vectơ (Space Vector PWM - SVPWM)
Trên mặt phẳng phức tựa tĩnh $\alpha-\beta$ (qua phép biến đổi Clarke), 8 tổ hợp trạng thái chuyển mạch $(S_a, S_b, S_c)$ tạo ra 6 vectơ điện áp tích cực không gian $V_1 \dots V_6$ có độ lớn $\frac{2}{3}V_{dc}$ cách nhau $60^\circ$ và 2 vectơ không $V_0(0,0,0), V_7(1,1,1)$ tại gốc tọa độ.

Vectơ điện áp tham chiếu $V_{ref}$ trong một hình quạt (Sector) được tổng hợp bằng thời gian tác động của 2 vectơ kề liền $V_k, V_{k+1}$ và vectơ không trong chu kỳ đóng cắt $T_s$:
$$\int_{0}^{T_s} V_{ref} \, dt = T_k V_k + T_{k+1} V_{k+1} + T_0 V_0$$
Với $T_k + T_{k+1} + T_0 = T_s$. Kỹ thuật SVPWM cho phép tận dụng điện áp một chiều cực đại đạt $\frac{V_{dc}}{\sqrt{3}} \approx 0.577 V_{dc}$ (tăng $115.47\%$ so với điều chế SPWM hình sin chuẩn).

### 3. Động học Hàm truyền Bộ lọc Đầu ra (LCL Output Filter Dynamics)
Để triệt tiêu các tần số hài đóng cắt cao tần ($f_{sw} \ge 10 \text{ kHz}$), đầu ra nghịch lưu được phối hợp bộ lọc L-C-L bậc ba ($L_1, C, L_2$). Phương trình hàm truyền từ điện áp cầu van $v_i(s)$ đến dòng điện đầu ra $i_g(s)$ là:
$$G_{LCL}(s) = \frac{i_g(s)}{v_i(s)} = \frac{1}{L_1 L_2 C s^3 + R_d(L_1 + L_2)C s^2 + (L_1 + L_2)s}$$
Tần số cộng hưởng tự nhiên của bộ lọc được xác định:
$$\omega_{res} = \sqrt{\frac{L_1 + L_2}{L_1 L_2 C}}$$
Bộ lọc phải áp dụng kỹ thuật giảm chấn thụ động (Passive Damping qua điện trở $R_d$) hoặc giảm chấn tích cực (Active Damping qua phản hồi dòng tụ điện) để ngăn hiện tượng dao động không suy giảm tại vùng tần số cộng hưởng.

---

## II. KHÂU DƯƠNG (LOCAL INTERACTION): GIAO TIẾP VÀ ĐIỀU KHIỂN NGHỊCH LƯU TRONG LƯỚI AC
*(Phân tích tương tác cục bộ bởi `grid_interaction_researcher.md` — Môi trường bọc: Lưới điện Không gian - Vật lý `Grid_Physical_Assets`).*

### 1. Chế độ Bám lưới (Grid-Following Inverter - GFL) & Vòng khóa pha PLL
Trong chế độ GFL, bộ nghịch lưu đóng vai trò một nguồn dòng điều khiển (Controlled Current Source) bơm công suất vào lưới điện AC đã có sẵn điện áp và tần số ổn định:
- **Đồng bộ pha (SRF-PLL):** Vòng khóa pha trong hệ tọa độ quay đồng bộ $d-q$ liên tục bám đuổi góc pha lưới $\theta_{grid}$ bằng cách điều khiển thành phần điện áp vuông góc $v_q = 0$.
- **Điều khiển dòng công suất độc lập:** Công suất tác dụng $P$ và công suất phản kháng $Q$ bơm vào lưới được giải điều chế trực tiếp qua dòng điện $i_d, i_q$:
  $$P = \frac{3}{2} v_d i_d, \quad Q = -\frac{3}{2} v_d i_q$$

### 2. Chế độ Tạo lưới & Quán tính ảo (Grid-Forming Inverter - GFM / Virtual Synchronous Machine)
Khi tỷ lệ máy phát đồng bộ truyền thống giảm, bộ nghịch lưu GFM (hoặc Máy phát đồng bộ ảo - VSM) đóng vai trò nguồn áp điều khiển (Controlled Voltage Source), tự thiết lập điện áp và tần số nút lưới:
- **Phương trình Quán tính ảo (Virtual Inertia Emulation):** Mô phỏng phương trình chuyển động rô-to máy phát đồng bộ (Swing Equation):
  $$J_v \frac{d\omega}{dt} = P_{set} - P_{e} - D_v(\omega - \omega_0)$$
  Trong đó $J_v$ là mô-men quán tính ảo, $D_v$ là hệ số cản suy giảm (Damping factor).
- **Đáp ứng RoCoF:** Khi xảy ra sự cố sụt tần số lưới, GFM lập tức xả động năng điện từ nội tại trong mili-giây để ghìm tốc độ suy giảm tần số (RoCoF), giữ ổn định Lớp điều khiển sơ cấp `[Layer_Primary_Control](obsidian://open?file=Layer_Primary_Control)`.

### 3. Khớp nối Nguồn Tài nguyên Phân tán (DERs) & Xe điện
- Là điểm nút giao tiếp phần cứng kết nối nguồn mặt trời (`[PV]`), pin lưu trữ (`[BESS](obsidian://open?file=BESS)`) và xe điện (`[Electric_Vehicle](obsidian://open?file=Electric_Vehicle)`) vào lưới truyền tải/phân phối.
- Hỗ trợ đầy đủ chức năng đẩy công suất hai chiều V2G (Vehicle-to-Grid) và tham gia dịch vụ phụ trợ (Ancillary Services) theo lệnh từ bộ điều phối trung tâm.

---

## III. GHI CHÚ THẢO LUẬN & CHUYÊN GIA
- **Đóng góp chuyên môn:** Đồng chí **N.H.Anh k66** chủ trì nghiên cứu thiết bị Điện tử công suất trong hệ thống lưới điện hiện đại.
- **Trạng thái tài liệu:** *(AI Agent dự thảo: `pure_entity_researcher.md` & `grid_interaction_researcher.md` — Chờ đồng chí N.H.Anh k66 hiệu chỉnh và thẩm định công thức)*.
