---
weighttree: 1
contributors: 
  - N.H.Anh k66
---
Direct Parent Connection: -> [[Power_Electronics]]

# STATCOM (STATIC SYNCHRONOUS COMPENSATOR)
*(Nút Thực thể Lớp 2: Thiết bị bù đồng bộ tĩnh FACTS sử dụng bộ biến đổi điện áp VSC để bù công suất phản kháng động cực nhanh, hỗ trợ ổn định điện áp AC).*

---

## I. KHÂU ÂM (PURE ENTITY): BẢN CHẤT BIẾN ĐỔI ĐIỆN ÁP VSC & PHƯƠNG TRÌNH TRAO ĐỔI CÔNG SUẤT
*(Phân tích độc lập nội tại bởi `pure_entity_researcher.md` — Không tham chiếu môi trường ngoại vi).*

### 1. Cấu trúc Phần cứng Bộ biến đổi Nguồn áp (Voltage Source Converter - VSC)
STATCOM được xây dựng trên nền tảng bộ biến đổi nguồn áp ba pha (VSC), kết nối song song với nút qua một cuộn kháng ghép nối nối tiếp (Coupled Inductor / Transformer Impedance $X_s = \omega L_s$) và một tụ điện tĩnh điện áp cao một chiều $C_{dc}$ ở phía DC.

Trong điều kiện bỏ qua điện trở tổn hao mạch van ($R_s \approx 0$), phương trình vi phân điện áp - dòng điện của STATCOM tại đầu cực pha là:
$$L_s \frac{di_x(t)}{dt} = v_{sx}(t) - v_{cx}(t)$$
Trong đó $v_{sx}(t)$ là điện áp đầu cực nút, $v_{cx}(t)$ là điện áp đầu ra do mạch van VSC tổng hợp qua kỹ thuật điều chế PWM.

### 2. Phương trình Trao đổi Công suất Phản kháng & Tác dụng
Biểu diễn điện áp đầu cực là vectơ gốc $V_s \angle 0^\circ$ và điện áp đầu ra STATCOM là $V_c \angle \delta$ (với $\delta$ là góc lệch pha vi phân do bộ điều khiển áp tụ DC thiết lập):
$$P = \frac{V_s V_c}{X_s} \sin \delta$$
$$Q = \frac{V_s(V_c \cos \delta - V_s)}{X_s}$$
Vì STATCOM là thiết bị bù công suất phản kháng thuần túy (không nối với nguồn phát năng lượng DC ngoài), góc $\delta \approx 0^\circ$ (chỉ nhận một lượng $P$ cực nhỏ để bù tổn hao chuyển mạch):
$$Q \approx \frac{V_s(V_c - V_s)}{X_s}$$
- **Chế độ phát dung kháng (Capacitive Mode):** Khi điều khiển biên độ $V_c > V_s \implies Q > 0$, STATCOM phát công suất phản kháng vào nút.
- **Chế độ tiêu thụ cảm kháng (Inductive Mode):** Khi điều khiển biên độ $V_c < V_s \implies Q < 0$, STATCOM tiêu thụ công suất phản kháng từ nút.

### 3. Động học Vòng điều khiển Điện áp Tụ DC ($V_{dc}$ Regulation Loop)
Năng lượng tĩnh điện lưu trữ trên tụ $C_{dc}$ được mô tả bởi phương trình cân bằng công suất:
$$\frac{1}{2} C_{dc} \frac{d(V_{dc}^2)}{dt} = P_{in} - P_{loss}$$
Bộ điều khiển PI vòng ngoài tính toán góc $\delta_{ref}$ để tinh chỉnh trễ/sớm pha cực nhỏ của vectơ $V_c$, giữ cho điện áp tụ $V_{dc}$ luôn không đổi ở mức danh định trong mọi điều kiện đóng cắt van.

---

## II. KHÂU DƯƠNG (LOCAL INTERACTION): BÙ ĐỒNG BỘ TĨNH & ỔN ĐỊNH ĐIỆN ÁP LƯỚI AC
*(Phân tích tương tác cục bộ bởi `grid_interaction_researcher.md` — Môi trường bọc: Lưới điện Không gian - Vật lý `Grid_Physical_Assets`).*

### 1. Điều chỉnh Điện áp Động Nhanh (Dynamic Voltage Support & Flicker Mitigation)
- **Tốc độ đáp ứng mili-giây:** STATCOM có khả năng thay đổi toàn dải công suất phản kháng từ $+Q_{max}$ sang $-Q_{max}$ chỉ trong dưới $10 \text{ ms}$ ($\frac{1}{2}$ chu kỳ lưới), vượt trội so với tụ bù cơ khí hoặc SVC (Static Var Compensator) truyền thống.
- **Triệt tiêu nhấp nháy điện áp (Flicker Mitigation):** Bù nhanh sự biến thiên công suất vô công gây ra bởi tải công nghiệp lớn (lò hồ quang) hoặc sự suy giảm đột ngột bức xạ điện mặt trời/gió, giữ điện áp nút AC ổn định trong ngưỡng $\pm 5\%$ danh định.

### 2. Hỗ trợ Khả năng Đi qua Sự cố Điện áp Thấp (Low Voltage Ride Through - LVRT)
- Khi xảy ra ngắn mạch lưới điện truyền tải/phân phối làm điện áp nút sụt sâu ($V_s < 0.8 \text{ p.u.}$), STATCOM lập tức kích hoạt chế độ LVRT: bơm tối đa dòng điện phản kháng $I_q = 1.0 \text{ p.u.}$ để chống sụt áp dây chuyền (Voltage Collapse).
- Phối hợp trực tiếp với bộ đổi nấc máy biến áp dưới tải (OLTC) và hệ thống FACTS thuộc Lớp điều khiển sơ cấp `[Layer_Primary_Control](obsidian://open?file=Layer_Primary_Control)`.

---

## III. GHI CHÚ THẢO LUẬN & CHUYÊN GIA
- **Đóng góp chuyên môn:** Đồng chí **N.H.Anh k66** chủ trì nghiên cứu thiết bị Điện tử công suất trong hệ thống lưới điện hiện đại.
- **Trạng thái tài liệu:** *(AI Agent dự thảo: `pure_entity_researcher.md` & `grid_interaction_researcher.md` — Chờ đồng chí N.H.Anh k66 hiệu chỉnh và thẩm định công thức)*.
