---
contributors: 
  - N.H.Anh k66
---
Direct Parent Connection: -> [[Power_Electronics]]

# MMC (MODULAR MULTILEVEL CONVERTER)
*(Nút Thực thể Lớp 2: Bộ biến đổi nhiều bậc dạng module chuyên trách truyền tải công suất lớn HVDC và tích hợp lưới điện gió ngoài khơi Offshore Wind).*

---

## I. KHÂU ÂM (PURE ENTITY): CẤU TRÚC VI MÔ SUBMODULE & ĐỘNG HỌC MẠCH VAN NHIỀU BẬC
*(Phân tích độc lập nội tại bởi `pure_entity_researcher.md` — Không tham chiếu môi trường ngoại vi).*

### 1. Cấu trúc Submodule Nửa cầu & Toàn cầu (Half-Bridge & Full-Bridge Submodule)
Bộ biến đổi MMC cấu thành từ các nhánh van nối tiếp chứa nhiều khối Submodule (SM) đồng nhất:
- **Submodule Nửa cầu (Half-Bridge SM):** Gồm 2 van bán dẫn IGBT/SiC ($T_1, T_2$) và tụ điện $C_{sm}$. Điện áp đầu cực của SM tuân theo hàm chọn trạng thái $S_i \in \{0, 1\}$:
  $$v_{sm,i} = S_i v_{c,i} = \begin{cases} v_{c,i}, & S_i = 1 \text{ (Inserted - Chèn tụ)} \\ 0, & S_i = 0 \text{ (Bypassed - Bỏ qua)} \end{cases}$$
- **Submodule Toàn cầu (Full-Bridge SM):** Gồm 4 van bán dẫn IGBT tạo thành cầu H, cho phép tạo ra 3 mức điện áp $v_{sm,i} \in \{-v_{c,i}, 0, +v_{c,i}\}$, có khả năng tự chặn dòng ngắn mạch DC (DC Fault Ride Through).

### 2. Cấu trúc Nhánh van MMC (Multilevel Arm Topology)
Mỗi pha $j \in \{a, b, c\}$ của MMC chia thành hai nhánh: Nhánh trên (Upper Arm) và Nhánh dưới (Lower Arm), mỗi nhánh chứa $N$ submodule nối tiếp với một cuộn kháng nhánh $L_{arm}$ và điện trở nhánh $R_{arm}$.

Phương trình cân bằng điện áp trên nhánh trên ($u$) và nhánh dưới ($l$) của pha $j$:
$$\frac{V_{dc}}{2} - v_{u,j} - L_{arm} \frac{di_{u,j}}{dt} - R_{arm} i_{u,j} = v_{j}$$
$$-\frac{V_{dc}}{2} + v_{l,j} - L_{arm} \frac{di_{l,j}}{dt} - R_{arm} i_{l,j} = v_{j}$$
Trong đó $v_j$ là điện áp pha đầu ra xoay chiều AC, $v_{u,j}$ và $v_{l,j}$ là tổng điện áp của các submodule được chèn vào trong nhánh trên/dưới.

### 3. Động học Dòng vòng & Cân bằng Điện áp Tụ Submodule (Circulating Current & Capacitor Balancing)
Sự sai lệch điện áp tức thời giữa các nhánh van sinh ra dòng vòng nội bộ (Circulating Current $i_{circ,j}$) chủ yếu chứa thành phần hài bậc hai ($2\omega_0$), chạy qua lại giữa các pha và nguồn DC mà không ra tải AC:
$$i_{circ,j} = \frac{i_{u,j} + i_{l,j}}{2} - \frac{I_{dc}}{3}$$
Phương trình vi phân dòng vòng:
$$2 L_{arm} \frac{di_{circ,j}}{dt} + 2 R_{arm} i_{circ,j} = V_{dc} - (v_{u,j} + v_{l,j})$$
Bộ điều khiển triệt tiêu dòng vòng (Circulating Current Suppression Controller - CCSC) thêm một thành phần điện áp nghịch pha để triệt tiêu hài $2\omega_0$, kết hợp cùng Thuật toán Sắp xếp Điện áp Tụ (Voltage Sorting Algorithm) để liên tục luân chuyển trạng thái chèn/bỏ qua giữa các submodule, giữ áp tụ $v_{c,i}$ cân bằng tại $\frac{V_{dc}}{N}$.

---

## II. KHÂU DƯƠNG (LOCAL INTERACTION): TRUYỀN TẢI HVDC & TÍCH HỢP LƯỚI ĐIỆN GIÓ NGOÀI KHƠI
*(Phân tích tương tác cục bộ bởi `grid_interaction_researcher.md` — Môi trường bọc: Lưới điện Không gian - Vật lý `Grid_Physical_Assets`).*

### 1. Cổng Giao tiếp Siêu công suất cho Hệ thống HVDC (VSC-HVDC Transmission)
- **Truyền tải đường dài điện áp cao:** MMC là công nghệ chuẩn mực cho các tuyến đường dây truyền tải điện áp cực cao hàng ngàn Megawatt ($\pm 320 \text{ kV} \dots \pm 800 \text{ kV DC}$), kết nối các hệ thống lưới điện xoay chiều không đồng bộ (Asynchronous AC Grids) qua khoảng cách hàng trăm kilômét.
- **Tách biệt dòng công suất:** Khả năng điều khiển độc lập và tức thời công suất tác dụng ($P$) truyền qua liên kết DC và công suất phản kháng ($Q$) bơm vào 2 đầu trạm biến áp AC.

### 2. Tích hợp Trang trại Điện gió Ngoài khơi (Offshore Wind Farm Integration)
- Khi các trang trại điện gió ngoài khơi (Offshore Wind) có công suất lớn (> 500 MW) cách bờ > 80 km, hệ thống cáp AC truyền thống gặp tổn hao dung kháng khổng lồ.
- Trạm chuyển đổi MMC ngoài khơi (Offshore MMC Converter Station) hoạt động ở chế độ tạo lưới GFM, tạo ra điện áp và tần số AC chuẩn để gom công suất từ các tuabin gió, sau đó biến đổi thành DC truyền tải trực tiếp về trạm MMC trên bờ.

### 3. Chất lượng Điện áp & Loại bỏ Bộ lọc Thụ động (Harmonic Elimination)
- Nhờ số bậc áp rất cao ($N \ge 200 \text{ SM/nhánh}$), dạng sóng điện áp đầu ra của MMC mịn như dạng sóng hình sin hoàn hảo, tổng độ méo hài áp cực thấp ($\text{THD} < 1\%$).
- Hoàn toàn loại bỏ sự cần thiết của các bộ lọc hài thụ động khổng lồ, giảm chi phí đầu tư và tiết kiệm diện tích trạm tại nút `[Power_Quality](obsidian://open?file=Power_Quality)`.

---

## III. GHI CHÚ THẢO LUẬN & CHUYÊN GIA
- **Đóng góp chuyên môn:** Đồng chí **N.H.Anh k66** chủ trì nghiên cứu thiết bị Điện tử công suất trong hệ thống lưới điện hiện đại.
- **Trạng thái tài liệu:** *(AI Agent dự thảo: `pure_entity_researcher.md` & `grid_interaction_researcher.md` — Chờ đồng chí N.H.Anh k66 hiệu chỉnh và thẩm định công thức)*.
