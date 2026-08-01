---
contributors:
  - H.T.Hải K67
---
Direct Parent Connection: -> [[Storage]]

# SUPERCAPACITOR (SIÊU TỤ ĐIỆN - EDLC / PSEUDOCAPACITOR)
*(Nút Thực thể Lớp 2 - Đại diện cho công nghệ lưu trữ năng lượng tĩnh điện/giả điện dung với khả năng phóng nạp xung công suất mật độ cực cao trong hệ thống HESS).*

## 1. Đặc tính Nội tại (Pure Entity View)
*(Phần Âm - Bản chất vật lý, điện từ và toán học nội tại của siêu tụ điện).*
- **Người đảm nhiệm (Assignee):** `pure_entity_researcher.md` *(AI Agent dự thảo - Chờ H.T.Hải K67 hiệu chỉnh)*
- **Cơ chế tích trữ năng lượng:** Tích trữ điện tích tĩnh điện tại bề mặt lớp kép điện hóa (Electrical Double-Layer Capacitance - EDLC) hoặc thông qua phản ứng redox giả điện dung bề mặt (Pseudocapacitance). Không xảy ra phản ứng chuyển pha hóa học sâu trong lòng điện cực.
- **Mô hình mạch điện tương đương RC:**
  - Cấu trúc mạch nối tiếp gồm điện trở tương đương nội $ESR$ (Equivalent Series Resistance), điện trở tự xả song song $EPR$ (Equivalent Parallel Resistance) và điện dung lớp kép $C_{dl}$.
  - **Phương trình vi phân dòng - áp:**
    $$v(t) = v(t_0) + \frac{1}{C_{dl}} \int_{t_0}^t i(\tau) d\tau + i(t) \cdot ESR$$
  - **Hằng số thời gian động học:** $\tau = ESR \cdot C_{dl}$ đạt giá trị siêu nhỏ, cho phép đáp ứng dòng tức thời cực lớn mà không tụt áp sâu.
- **Cơ chế suy hao nội tại (Internal Degradation):**
  - Suy hao chủ yếu do điện áp hoạt động vượt ngưỡng (Overvoltage) và stress nhiệt làm phân hủy chất điện phân, gia tăng nội trở $ESR$ và suy giảm điện dung $C_{dl}$.
  - Do không có suy thoái cấu trúc mạng tinh thể như pin hóa học, tuổi thọ chu kỳ (Cycle Life) vượt trội: $> 500,000 - 1,000,000$ chu kỳ sạc/xả hoàn chỉnh.

## 2. Tương tác Cục bộ (Local Interaction View)
*(Phần Dương - Luồng tương tác và động lực học của Siêu tụ điện trong Container bọc trực tiếp là hệ thống `Storage` - HESS).*
- **Người đảm nhiệm (Assignee):** `grid_interaction_researcher.md` *(AI Agent dự thảo - Chờ H.T.Hải K67 hiệu chỉnh)*
- **Hệ tham chiếu (Container):** `Storage` (Hệ thống Lưu trữ Năng lượng Hỗn hợp - HESS).
- **Tốc độ sạc/xả (C-rate & Power Density):**
  - **Mật độ công suất siêu cao:** $> 10,000 \text{ W/kg}$, khả năng duy trì dòng phóng/nạp xung đỉnh (Peak Pulse Current) cực lớn gấp hàng chục lần định mức C-rate của pin hóa học.
- **Thời gian phản ứng (Dynamic Response Time):**
  - **Thang đo Mili-giây ($\text{ms}$):** Thời gian đáp ứng từ lúc nhận lệnh đến khi bơm/rút đầy đủ công suất dưới $5 \text{ ms}$, nhanh nhất trong 3 công nghệ của nhánh `Storage`.
- **Hàm truyền & Vai trò Lọc tần số cao (High-Pass Frequency Filtering):**
  - Trong chiến lược quản lý năng lượng phối hợp HESS, tổng yêu cầu bù lệch công suất $\Delta P_{HESS}$ được phân tách tần số (Wavelet / Frequency Decomposition):
    $$\Delta P_{HESS} = \Delta P_{high} + \Delta P_{mid} + \Delta P_{low}$$
  - Siêu tụ điện hoạt động như một **Bộ lọc thông cao (High-Pass Filter)**, chịu trách nhiệm hấp thụ toàn bộ thành phần dao động tần số cao $\Delta P_{high}$ (dao động xung nhọn, nhiễu tần số, RoCoF tức thời).
- **Động lực học tương tác bảo vệ BESS:**
  - Việc Siêu tụ gánh chịu các xung sạc/xả vi mô và dòng đỉnh ngắn hạn giúp giảm thiểu dòng RMS qua pin hóa học (`BESS`), ngăn ngừa hiện tượng quá nhiệt và kéo dài đáng kể tuổi thọ của pin trong Container `Storage`.

## 3. Điểm mạnh, Điểm yếu & Ứng dụng (Pros, Cons & Use Cases)
- **Điểm mạnh (Strengths):**
  - Mật độ công suất cực đại, đáp ứng mili-giây.
  - Tuổi thọ chu kỳ gần như không giới hạn, độ suy hao theo chu kỳ cực thấp.
  - Hiệu suất khứ hồi cao ($\text{RTE} > 95\%$).
- **Điểm yếu (Weaknesses):**
  - Mật độ năng lượng trọng lượng và thể tích rất thấp ($\approx 5 - 10 \text{ Wh/kg}$).
  - Hiện tượng tự xả (Self-discharge) nhanh khi không tải.
  - Chi phí đầu tư ban đầu trên mỗi đơn vị dung lượng năng lượng ($\text{\$/kWh}$) cao.
- **Khi nào dùng (When to use):**
  - Dập tắt xung công suất nhọn trong HESS, hỗ trợ quán tính RoCoF siêu tốc.
  - Bù tải động rải rác có chu kỳ tính bằng mili-giây đến vài giây.
- **Khi nào KHÔNG dùng (When NOT to use):**
  - Cân bằng năng lượng duy trì kéo dài từ vài phút trở lên.
  - Lập lịch phát điện lưu trữ qua đêm hoặc mùa.
