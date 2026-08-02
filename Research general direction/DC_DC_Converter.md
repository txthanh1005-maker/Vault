---
weighttree: 0
contributors: 
  - N.H.Anh k66
---
Direct Parent Connection: -> [[Power_Electronics]]

# DC-DC CONVERTER (BỘ BIẾN ĐỔI DC-DC)
*(Nút Thực thể Lớp 2: Bộ biến đổi điện áp một chiều sang một chiều DC/DC, điều chỉnh trở kháng và phối hợp mức áp trên Bus DC và hệ thống lưu trữ hỗn hợp HESS).*

---

## I. KHÂU ÂM (PURE ENTITY): BẢN CHẤT MẠCH BIẾN ĐỔI DC-DC & PHƯƠNG TRÌNH ĐỘNG HỌC LƯU TRỮ
*(Phân tích độc lập nội tại bởi `pure_entity_researcher.md` — Không tham chiếu môi trường ngoại vi).*

### 1. Cấu trúc Mạch Buck, Boost và Bi-directional Half-Bridge
Bộ biến đổi một chiều - một chiều (DC-DC Converter) sử dụng linh kiện bán dẫn (MOSFET/IGBT) đóng cắt tần số cao ($f_s \ge 20 \text{ kHz}$) phối hợp với phần tử lưu trữ năng lượng thụ động (cuộn cảm $L$, tụ điện $C$) để thay đổi mức điện áp trung bình.

Trong chế độ dẫn dòng liên tục (Continuous Conduction Mode - CCM), với tỷ số chu kỳ làm việc $D \in (0, 1)$ ($D = \frac{t_{on}}{T_s}$):
- **Phương trình Cân bằng Điện áp Cuộn cảm (Inductor Volt-Second Balance):**
  $$\langle v_L(t) \rangle = \frac{1}{T_s} \int_{0}^{T_s} v_L(t) \, dt = 0$$
- **Tỷ số biến đổi điện áp tĩnh:**
  $$\frac{V_{out}}{V_{in}} = D \text{ (Buck Converter)}, \quad \frac{V_{out}}{V_{in}} = \frac{1}{1 - D} \text{ (Boost Converter)}$$
  $$\frac{V_{out}}{V_{in}} = \frac{D}{1 - D} \text{ (Buck-Boost / Bi-directional Converter)}$$

### 2. Bộ biến đổi Cầu chủ động kép (Dual Active Bridge - DAB Converter)
Trong các ứng dụng công suất cao yêu cầu cách ly điện áp DC hai chiều, bộ biến đổi DAB được cấu tạo bởi hai cầu H van bán dẫn nối với nhau thông qua biến áp cách ly tần số cao $T_{HF}$ và cuộn cảm rò $L_k$.

Nguyên lý điều khiển lệch pha đơn (Single-Phase Shift - SPS) với góc dịch pha $\phi \in \left[-\frac{\pi}{2}, \frac{\pi}{2}\right]$ giữa hai cầu H quyết định chiều và độ lớn dòng công suất truyền tải:
$$P_{DAB} = \frac{n V_1 V_2}{2\pi f_s L_k} \phi \left(1 - \frac{|\phi|}{\pi}\right)$$
Trong đó $n$ là tỷ số biến áp, $V_1, V_2$ là điện áp bus DC hai phía. DAB cho phép linh kiện đóng cắt đạt trạng thái chuyển mạch điện áp không (Zero-Voltage Switching - ZVS), triệt tiêu tối đa tổn hao chuyển mạch ở tần số cao.

### 3. Phương trình Động học & Giới hạn Ripple Dòng/Áp
Độ nhấp nhô dòng điện cuộn cảm ($\Delta i_L$) và điện áp tụ điện ($\Delta v_C$) trong mạch Buck hai chiều được định nghĩa:
$$\Delta i_L = \frac{V_{out}(1 - D)}{L f_s}, \quad \Delta v_C = \frac{\Delta i_L}{8 C f_s}$$
Để đảm bảo mạch luôn hoạt động trong CCM, giá trị điện cảm tới hạn $L_{crit}$ phải thỏa mãn $L > L_{crit} = \frac{(1-D)R_{load}}{2 f_s}$.

---

## II. KHÂU DƯƠNG (LOCAL INTERACTION): ĐIỀU PHỐI BUS DC VÀ HỆ THỐNG LƯU TRỮ HỖN HỢP HESS
*(Phân tích tương tác cục bộ bởi `grid_interaction_researcher.md` — Môi trường bọc: Lưới điện Không gian - Vật lý `Grid_Physical_Assets`).*

### 1. Biến đổi Trở kháng & Phối hợp Mức điện áp Bus DC (DC Bus Voltage Regulation)
Trong Lưới điện vi mô một chiều (DC Microgrid) hoặc trạm sạc xe điện siêu nhanh:
- Bộ biến đổi DC-DC làm nhiệm vụ khớp nối trở kháng giữa các phần tử có dải điện áp làm việc biến thiên rộng (Pin sạc/xả từ $600 \text{ V} \dots 800 \text{ V}$, Siêu tụ từ $250 \text{ V} \dots 750 \text{ V}$) vào một thanh cái DC chung có điện áp cố định (e.g., $1000 \text{ V DC}$).
- Điều khiển vòng lặp kép (Dual-loop Control): Vòng ngoài điều chỉnh điện áp Bus DC ($V_{dc,ref}$), vòng trong điều chỉnh dòng điện cuộn cảm ($I_{L,ref}$) với băng thông cực nhanh để chống sụt áp khi tải bước.

### 2. Phối hợp Điều khiển Lưu trữ Hỗn hợp HESS (Supercapacitor & BESS Integration)
Trong kiến trúc HESS của đồng chí **D.M.Hai K67**:
- **Nhánh Siêu tụ (`[Supercapacitor](obsidian://open?file=Supercapacitor)`):** Bộ DC-DC hai chiều nhận tín hiệu đặt dòng điện cao tần $I_{ref,fast}$ từ thuật toán `[Low_Pass_Filter](obsidian://open?file=Low_Pass_Filter)` hoặc `[Fast_Fourier_Transform](obsidian://open?file=Fast_Fourier_Transform)` để xả/sạc cực nhanh, hấp thụ các xung công suất RoCoF.
- **Nhánh Pin điện hóa (`[BESS](obsidian://open?file=BESS)`):** Bộ DC-DC hai chiều nhận tín hiệu đặt dòng điện tần số trung bình/thấp $I_{ref,slow}$, đồng thời giới hạn tốc độ biến thiên dòng điện $\frac{di}{dt}$ nhằm bảo vệ điện cực, giảm ứng suất nhiệt và kéo dài tuổi thọ (`[Storage_Degradation_Lifetime](obsidian://open?file=Storage_Degradation_Lifetime)`).

---

## III. GHI CHÚ THẢO LUẬN & CHUYÊN GIA
- **Đóng góp chuyên môn:** Đồng chí **N.H.Anh k66** chủ trì nghiên cứu thiết bị Điện tử công suất trong hệ thống lưới điện hiện đại.
- **Trạng thái tài liệu:** *(AI Agent dự thảo: `pure_entity_researcher.md` & `grid_interaction_researcher.md` — Chờ đồng chí N.H.Anh k66 hiệu chỉnh và thẩm định công thức)*.
