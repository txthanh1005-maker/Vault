---
weighttree: 1
contributors:
  - H.T.Hải K67
---
Direct Parent Connection: -> [[Storage]]

# STORAGE DEGRADATION & LIFETIME (SUY HAO & TUỔI THỌ HỆ THỐNG LƯU TRỮ HESS)
*(Nút Thực thể Lớp 2 - Đại diện cho thuộc tính chung về cơ chế lão hóa vật lý/hóa học, độ suy giảm dung lượng và bài toán mô hình hóa tuổi thọ của hệ thống lưu trữ hỗn hợp).*

## 1. Đặc tính Nội tại (Pure Entity View)
*(Phần Âm - Bản chất hóa lý, điện hóa và toán học nội tại chi phối sự suy thoái theo chu kỳ và theo thời gian).*
- **Người đảm nhiệm (Assignee):** `pure_entity_researcher.md` *(AI Agent dự thảo - Chờ H.T.Hải K67 hiệu chỉnh)*
- **Phân loại suy hao theo 2 cơ chế độc lập:**
  - **Suy hao theo chu kỳ (Cycle Aging):** Suy thoái xảy ra mỗi khi trạm phóng/nạp dòng công suất, phụ thuộc vào Độ sâu xả (Depth of Discharge - $DoD$), tốc độ $C-rate$ và dòng điện tức thời.
  - **Suy hao thời gian / tĩnh (Calendar Aging):** Suy thoái tự nhiên theo thời gian lưu trữ ngay cả khi ở trạng thái nghỉ, phụ thuộc vào $SOC$ tĩnh và stress nhiệt độ $T$.
- **Mô hình hóa suy hao cho từng công nghệ lõi HESS:**
  - **BESS (Pin hóa học - LFP/NMC):**
    - Lão hóa hóa học phi tuyến do gia tăng lớp màng điện phân rắn (SEI Layer Growth), mất lithium khả dụng (LLLI) và nứt vi mô điện cực:
      $$Q_{loss}(t, DoD, T) = k_{cal}(T, SOC) \cdot t^{0.5} + k_{cyc}(DoD, C_{rate}, T) \cdot N_{cycles}$$
  - **Supercapacitor (Siêu tụ điện):**
    - Lão hóa vật lý bề mặt điện cực do stress quá áp (Overvoltage) và nhiệt làm gia tăng điện trở nối tiếp $ESR$ và suy giảm điện dung $C_{dl}$. Tuổi thọ cực cao ($> 500,000 - 1,000,000$ chu kỳ).
  - **Hydrogen Storage (PEM Electrolyzer & Fuel Cell):**
    - Lão hóa nhiệt động lực học do suy thoái màng trao đổi proton (PEM Degradation), ăn mòn chất xúc tác Platinum và quá tải áp suất cục bộ khi thay đổi tải biến thiên quá gắt.
- **Tuổi thọ sử dụng hữu ích (End of Life - EOL):**
  - Trạm BESS thường được xác định EOL khi dung lượng khả dụng giảm xuống dưới $80\%$ định mức ban đầu ($SOH \le 80\%$), trong khi Supercapacitor là khi $ESR$ tăng $100\%$ hoặc $C_{dl}$ giảm $20\%$.

## 2. Tương tác Cục bộ (Local Interaction View)
*(Phần Dương - Luồng tương tác và tác động của Đặc tính Suy hao/Tuổi thọ lên Container bọc trực tiếp là hệ thống `Storage` - HESS).*
- **Người đảm nhiệm (Assignee):** `grid_interaction_researcher.md` *(AI Agent dự thảo - Chờ H.T.Hải K67 hiệu chỉnh)*
- **Hệ tham chiếu (Container):** `Storage` (Hệ thống Lưu trữ Năng lượng Hỗn hợp - HESS).
- **Tương tác với Thang Thời gian Phản ứng & Tốc độ Sạc/Xả:**
  - Sự thoái hóa của mỗi công nghệ phụ thuộc trực tiếp vào tần số dao động công suất mà nó gánh chịu.
  - Nếu áp đặt dao động tần số cao ($\Delta P_{high}$, chu kỳ mili-giây đến vài giây, dòng C-rate lớn) trực tiếp lên BESS, tốc độ suy hao $k_{cyc}$ sẽ tăng gấp 3 - 5 lần, làm trạm pin suy kiệt nhanh chóng.
- **Hàm mục tiêu Điều phối Kéo dài Tuổi thọ HESS (Lifetime-Aware EMS):**
  - Trong chiến lược điều khiển HESS, thuộc tính `Storage_Degradation_Lifetime` đóng vai trò là **Hàm phạt chi phí suy hao (Degradation Cost Penalty)** trong bài toán tối ưu hóa thời gian thực:
    $$\min \int \left( \lambda_{BESS} \cdot \Delta SOH_{BESS}(t) + \lambda_{SC} \cdot \Delta SOH_{SC}(t) + \lambda_{H_2} \cdot \Delta SOH_{H_2}(t) \right) dt$$
- **Cơ chế phối hợp bảo vệ chéo trong Container `Storage`:**
  - `Supercapacitor` làm "lá chắn xung công suất" (Power Shield), lọc sạch dòng đỉnh và dao động tần số cao, giữ cho dòng qua `BESS` êm ái và đều đặn.
  - `Hydrogen_Storage` hấp thụ phần năng lượng dư thừa quy mô dài hạn, ngăn cản `BESS` phải duy trì trạng thái sạc đầy ($SOC \to 100\%$) hoặc xả cạn ($SOC \to 0\%$) trong thời gian dài, triệt tiêu suy hao tĩnh (Calendar Aging) nguy hiểm cho pin.

## 3. Điểm mạnh, Điểm yếu & Ứng dụng (Pros, Cons & Use Cases)
- **Điểm mạnh (Strengths):**
  - Cho phép lượng hóa chính xác chi phí hao mòn thực tế của từng kWh sạc/xả.
  - Cung cấp cơ sở toán học để tối ưu hóa chiến lược quản lý năng lượng (EMS) và phân chia tần số trong HESS.
- **Điểm yếu (Weaknesses):**
  - Các mô hình suy hao hóa học/điện hóa (đặc biệt là BESS và PEM) có tính phi tuyến cực cao và phụ thuộc nhiều tham số thực nghiệm.
  - Khó khăn trong việc ước lượng chính xác Trạng thái sức khỏe ($SOH$ - State of Health) theo thời gian thực mà không có cảm biến chuyên dụng.
- **Khi nào dùng (When to use):**
  - Lập lịch vận hành tối ưu ngày tới (Day-ahead Scheduling) và điều phối công suất thời gian thực có xét đến tuổi thọ HESS.
  - Đánh giá tính khả thi kinh tế dài hạn và kế hoạch thay thế tài sản (Asset Replacement Planning).
- **Khi nào KHÔNG dùng (When NOT to use):**
  - Các mô phỏng ổn định quá độ siêu ngắn hạn (Vài mili-giây) nơi suy hao chu kỳ không kịp tạo ra tác động đáng kể.
