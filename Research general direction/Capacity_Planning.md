---
weighttree: 1
contributors: 
  - Khanh k67
  - Thanh K67
---
Direct Parent Connection: -> [[Power_System_Scheduling]]

# Capacity Planning (Quy hoạch Dài hạn)
*(Bài toán Quy hoạch Mở rộng Nguồn và Lưới điện. Khung thời gian: Tháng/Năm)*

## 1. Bản chất Vật lý & Toán học (Intrinsic Properties)

### 1.1. Bản chất toán học của bài toán Quy hoạch mở rộng (GEP & TEP)
Bài toán Quy hoạch mở rộng nguồn điện (Generation Expansion Planning - GEP) và lưới điện (Transmission Expansion Planning - TEP) bản chất là một mô hình **tối ưu hóa ngẫu nhiên, rời rạc - liên tục hỗn hợp (Stochastic Mixed-Integer Linear/Non-Linear Programming - MILP/MINLP)** quy mô cực lớn. 
- **GEP** giải quyết việc định hình không gian vector công suất phát $\mathbf{P}_G$ trong tương lai (loại nguồn, dung lượng, vị trí, thời điểm).
- **TEP** giải quyết sự tiến hóa về mặt tô-pô học của mạng điện, được biểu diễn qua ma trận dẫn nạp $\mathbf{Y}_{bus}$ theo thời gian.
Bài toán tổng quát phải giải quyết đồng thời hai lớp biến: Quyết định đầu tư (Investment variables - tĩnh, rời rạc/nhị phân) và Quyết định vận hành (Operational variables - động, liên tục).

### 1.2. Mô hình tối ưu hóa Chi phí đầu tư (CAPEX) và Chi phí vận hành kỳ vọng (Expected OPEX)
Hàm mục tiêu cốt lõi của bài toán là cực tiểu hóa Tổng chi phí hiện tại thuần (Net Present Value - NPV) xuyên suốt vòng đời dự án, bao gồm CAPEX và OPEX.

**Hàm mục tiêu tổng quát:**
$$ \min \sum_{t \in T} \frac{1}{(1+r)^t} \left[ CAPEX_t + \mathbb{E}_{\omega}[OPEX_{t, \omega}] \right] $$

Trong đó:
- **$T$**: Chân trời quy hoạch (Planning horizon, tính bằng năm).
- **$r$**: Tỷ suất chiết khấu (Discount rate).
- **$\omega \in \Omega$**: Tập hợp các kịch bản bất định.

**Cấu trúc CAPEX (Chi phí vốn):**
$$ CAPEX_t = \sum_{i \in \mathcal{G}} IC_{G,i} \cdot x_{G,i,t} + \sum_{(i,j) \in \mathcal{L}} IC_{T,ij} \cdot x_{T,ij,t} $$
- $x_{G,i,t}$: Biến quyết định xây dựng nhà máy $i$ tại năm $t$ (liên tục hoặc nguyên).
- $x_{T,ij,t}$: Biến nhị phân $\{0,1\}$ quyết định xây dựng đường dây $(i,j)$ tại năm $t$.
- $IC$: Chi phí đầu tư quy chuẩn (Investment Cost).

**Cấu trúc Expected OPEX (Chi phí vận hành kỳ vọng):**
$$ \mathbb{E}_{\omega}[OPEX_{t, \omega}] = \sum_{\omega \in \Omega} \pi_{\omega} \sum_{h \in \mathcal{H}} \left[ \sum_{i \in \mathcal{G}} VC_{i} \cdot P_{i,t,h,\omega} + VOLL \cdot ENS_{t,h,\omega} \right] $$
- $\pi_{\omega}$: Xác suất xảy ra kịch bản $\omega$.
- $P_{i,t,h,\omega}$: Công suất phát của tổ máy $i$ tại giờ $h$, năm $t$, kịch bản $\omega$.
- $VC_i$: Chi phí biến đổi (Variable Cost).
- $ENS_{t,h,\omega}$: Lượng điện năng không cung cấp được (Energy Not Supplied).
- $VOLL$: Giá trị của phần tải bị mất (Value of Lost Load).

### 1.3. Bản chất vật lý nội tại (Intrinsic Physical Properties)
Bài toán bị ràng buộc nghiêm ngặt bởi các định luật vật lý và giới hạn kỹ thuật phần cứng:
- **Định luật Kirchhoff & Trào lưu công suất (Power Flow):** 
  Sự truyền tải năng lượng trên lưới tuân theo phương trình phi tuyến AC. Để đảm bảo tính toán khả thi trong dài hạn, TEP thường sử dụng tuyến tính hóa **DC Power Flow**:
  $$ P_{ij,t,h,\omega} = B_{ij} (\theta_{i,t,h,\omega} - \theta_{j,t,h,\omega}) \cdot x_{T,ij,t} $$
  Biểu thức này chứa phép nhân giữa biến nhị phân $x_{T,ij,t}$ và biến liên tục $\theta$ (góc pha), tạo ra tính phi tuyến. Yêu cầu sử dụng phương pháp Big-M (hoặc Disjunctive Model) để tuyến tính hóa.
- **Ràng buộc tiến hóa trạng thái (State Evolution):** Một khi thiết bị (nguồn/lưới) được quyết định đầu tư tại năm $t$, biến trạng thái khả dụng của nó bằng 1 cho tất cả các năm $t' > t$ trong giới hạn tuổi thọ kỹ thuật.
- **Quán tính và ổn định nội tại:** Với sự gia tăng của VRE (Năng lượng tái tạo biến đổi), bài toán GEP thế hệ mới phải đưa thêm các ràng buộc về **Khối lượng quán tính tới hạn (Critical Inertia)** và dự phòng động lực học.

### 1.4. Xử lý rủi ro và Kịch bản bất định (Stochastic Scenarios)
Tính bất định của hệ thống đến từ sự phân phối ngẫu nhiên theo không gian - thời gian của Tốc độ gió, Bức xạ mặt trời, và Phụ tải. Các phương pháp toán học nội tại xử lý vấn đề này gồm:
- **Tối ưu hóa ngẫu nhiên hai giai đoạn (Two-Stage Stochastic Programming):**
  - *Giai đoạn 1 (Here-and-Now):* Ra quyết định CAPEX ($x_G, x_T$) trước khi kịch bản $\omega$ được hiện thực hóa.
  - *Giai đoạn 2 (Wait-and-See):* Ra quyết định OPEX ($P_{i}, ENS$) để phản ứng lại kịch bản $\omega$ cụ thể.
- **Khử chiều Không gian kịch bản (Scenario Reduction):** Sử dụng K-means Clustering hoặc Fast Forward Selection để cô lập ra một tập $\Omega$ hữu hạn (các ngày/tuần đại diện) mà vẫn duy trì khoảng cách xác suất.
- **Tối ưu hóa bền vững (Robust Optimization - RO):** Tối ưu hóa dựa trên kịch bản tồi tệ nhất thuộc một không gian bất định liên tục (Uncertainty Set $\mathcal{U}$). Bản chất là mô hình "Max-Min-Max" trong Lý thuyết Trò chơi, đảm bảo hệ thống vật lý có khả năng chống chịu cực đoan.

---

## 2. Tương tác Cục bộ (Local Interaction)

**Nút mục tiêu:** `Capacity_Planning`
**Container bọc trực tiếp:** `Power_System_Scheduling`

### 2.1. Vai trò Kiến tạo Không gian Trạng thái (State-Space Definition)
Trong môi trường `Power_System_Scheduling`, nút `Capacity_Planning` đóng vai trò là cơ chế thiết lập biên giới vật lý và kinh tế. Nó không tham gia vào các quyết định điều độ từng giờ, mà tạo ra một "chiếc lồng ràng buộc" (constraint bounding box). Mọi hoạt động của hệ thống lập lịch chỉ được phép dao động và tối ưu hóa bên trong chiếc lồng này. 

Sự tương tác này mang tính chất **trên-xuống (top-down)** một chiều trong ngắn hạn: `Capacity_Planning` áp đặt các thông số tĩnh, định hình toàn bộ không gian nghiệm khả thi (feasible region) cho các phân hệ chức năng con bên trong Container.

### 2.2. Chiều truyền tín hiệu Đầu ra (Output Signals) thành Ràng buộc Cứng
Các quyết định từ `Capacity_Planning` được mã hóa thành các thông số cấu trúc và giáng xuống hai phân hệ chính của Container là **Day-Ahead Market (DAM)** và **Real-Time Balancing (RTB)** dưới dạng các ràng buộc cứng không thể vi phạm:

#### A. Giới hạn trần công suất lắp đặt tối đa (Maximum Installed Capacity Ceiling)
- **Bản chất:** Tổng công suất khả dụng thiết kế ($P_{max}$) của các thiết bị.
- **Tác động lên Day-Ahead Market (DAM):** Đóng vai trò là cận trên tuyệt đối trong Unit Commitment (UC) và Economic Dispatch (ED). Biến quyết định công suất $P_{g,t}$ bị khóa chặt bởi bất phương trình: $P_{min} \leq P_{g,t} \leq \mathbf{P_{max\_installed}}$. Ngăn chặn lập lịch ảo phi vật lý.
- **Tác động lên Real-Time Balancing (RTB):** Chặn đứng dư địa điều chỉnh tăng (Upward Regulation Limit). Tổng công suất cộng Spinning Reserve không bao giờ được phép xuyên thủng trần công suất này.

#### B. Định cỡ thiết bị (Equipment Sizing)
- **Bản chất:** Các giới hạn vật lý nội tại (Dung lượng lưu trữ $E_{max}$ MWh, tốc độ biến thiên Ramp-rate $R_{up}/R_{down}$, công suất Inverter).
- **Tác động lên Day-Ahead Market (DAM):** Tạo ra các **ràng buộc liên thời gian (Inter-temporal constraints)** khắt khe. Biến Trạng thái sạc (SoC) bị khóa bởi định cỡ dung lượng: $0 \leq SoC_t \leq \mathbf{E_{max}}$. DAM bị giới hạn triệt để khả năng kinh doanh chênh lệch giá (arbitrage).
- **Tác động lên Real-Time Balancing (RTB):** Kích cỡ Inverter thiết lập giới hạn dòng ngắn mạch và khả năng cung cấp công suất phản kháng (Q) tức thời. RTB phụ thuộc hoàn toàn vào Sizing này để định cỡ Dịch vụ phụ trợ.

#### C. Ngân sách CAPEX (Capital Expenditure Budget)
- **Bản chất:** Nguồn vốn đầu tư ban đầu tạo ra danh mục công nghệ (Technology Portfolio Mix).
- **Tác động lên Day-Ahead Market (DAM):** CAPEX định hình cấu trúc OPEX. CAPEX nghiêng về Năng lượng tái tạo buộc DAM phải giải bài toán tối ưu trong môi trường Zero marginal cost nhưng bất định. DAM bị ép phải tối đa hóa tỷ lệ hấp thụ năng lượng tái tạo để bù đắp chi phí chìm.
- **Tác động lên Real-Time Balancing (RTB):** Sự thắt chặt CAPEX cản trở đầu tư vào tài nguyên đắt đỏ có độ linh hoạt cao (lưu trữ nhanh). Hệ quả là RTB kế thừa một hệ sinh thái nghèo nàn, tăng mức độ chênh lệch cung-cầu và tần suất Cắt giảm phụ tải (Load Shedding).

### 2.3. Hệ quả Tương tác: Cơ chế Khóa (Lock-in Effect) & Tín hiệu Đối ngẫu
Sự tương tác một chiều từ `Capacity_Planning` giam lỏng `Power_System_Scheduling` trong các giới hạn vật lý định sẵn. Khi Container chạm đến các giới hạn này, nó không thể tự phá vỡ ranh giới. 
Thông qua bộ giải tối ưu, Container sẽ sinh ra các **Giá trị bóng (Shadow Prices / Lagragian Multipliers)** tại các điểm thắt cổ chai. Các giá trị đối ngẫu này đại diện cho "nỗi đau kinh tế" của hệ thống điều độ, trỏ ngược lên như một tín hiệu phản hồi vô hình (economic feedback) bắt buộc chu kỳ `Capacity_Planning` ở tương lai phải mở rộng quy mô.