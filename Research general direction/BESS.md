---
weighttree: 0
contributors:
  - H.T.Hải K67
---
Direct Parent Connection: -> [[Storage]]

# BESS (HỆ THỐNG LƯU TRỮ NĂNG LƯỢNG PIN - BATTERY ENERGY STORAGE SYSTEM)
*(Nút Thực thể Lớp 2 - Đại diện cho công nghệ lưu trữ năng lượng hóa học bằng pin, đóng vai trò cân bằng trung hạn về công suất và năng lượng trong hệ thống HESS).*

## 1. Đặc tính Nội tại (Pure Entity View)
*(Phần Âm - Bản chất hóa lý, điện hóa và động lực học nội tại của hệ thống BESS).*
- **Người đảm nhiệm (Assignee):** `pure_entity_researcher.md` *(AI Agent dự thảo - Chờ H.T.Hải K67 hiệu chỉnh)*
- **Cơ chế điện hóa:** Phản ứng chuyển dịch ion lithium thuận nghịch giữa điện cực dương (LFP - Lithium Iron Phosphate hoặc NMC - Nickel Manganese Cobalt) và điện cực âm (Graphite).
- **Trạng thái sạc (State of Charge - SOC) & Điện áp mạch hở (OCV):**
  - **Phương trình vi phân SOC:**
    $$SOC(t) = SOC(t_0) + \frac{1}{Q_{nom}} \int_{t_0}^t \left( \eta_{c} I_{charge}(\tau) - \frac{I_{discharge}(\tau)}{\eta_{d}} \right) d\tau$$
  - Ràng buộc an toàn nội tại: $SOC(t) \in [SOC_{min}, SOC_{max}]$ nhằm ngăn chặn suy thoái quá mức.
  - Điện áp mạch hở $OCV(SOC, T)$ là hàm phi tuyến của trạng thái sạc và nhiệt độ trạm pin.
- **Hiệu suất khứ hồi & Tổn hao nội (Round-Trip Efficiency & Internal Losses):**
  - Hiệu suất khứ hồi cao ($\text{RTE} \approx 85\% - 92\%$).
  - Tổn hao bao gồm nhiệt lượng Joule tạo ra trên điện trở nội $R_i$ và tổn hao biến đổi kép qua hệ thống điện tử công suất.
- **Cơ chế lão hóa hóa học (Chemical Degradation Mechanism):**
  - Suy hao chủ yếu do mất lithium khả dụng (Loss of Lithium Inventory - LLLI), tăng trưởng lớp màng điện phân rắn (SEI Layer Growth) và rạn nứt vật liệu cấu trúc điện cực khi làm việc ở C-rate cao hoặc nhiệt độ lớn.

## 2. Tương tác Cục bộ (Local Interaction View)
*(Phần Dương - Luồng tương tác và động lực học của BESS trong Container bọc trực tiếp là hệ thống `Storage` - HESS).*
- **Người đảm nhiệm (Assignee):** `grid_interaction_researcher.md` *(AI Agent dự thảo - Chờ H.T.Hải K67 hiệu chỉnh)*
- **Hệ tham chiếu (Container):** `Storage` (Hệ thống Lưu trữ Năng lượng Hỗn hợp - HESS).
- **Tốc độ sạc/xả (C-rate & Power Density):**
  - **Tốc độ trung bình tối ưu:** Định mức phổ biến từ $0.5\text{C} - 2\text{C}$, cho phép cân bằng hài hòa giữa mật độ công suất ($\text{W/kg}$) và mật độ năng lượng ($\text{Wh/kg}$).
- **Thời gian phản ứng (Dynamic Response Time):**
  - **Thang đo Giây đến Giờ ($\text{s} - \text{h}$):** Thời gian đáp ứng từ lúc nhận lệnh đủ nhanh để tham gia ổn định công suất dải trung hạn (từ vài trăm mili-giây đến vài giây), và duy trì bơm/rút năng lượng liên tục trong nhiều giờ.
- **Hàm truyền & Vai trò Lọc tần số trung bình (Band-Pass / Mid-Frequency Filtering):**
  - Trong sự phân tách tần số công suất HESS ($\Delta P_{HESS} = \Delta P_{high} + \Delta P_{mid} + \Delta P_{low}$), `BESS` hoạt động như một **Bộ lọc thông dải / tần số trung bình (Band-Pass Filter)**.
  - Hấp thụ thành phần dao động trung hạn $\Delta P_{mid}$, thực hiện điều phối P-Q, đáp ứng tần số nhanh (FFR - Fast Frequency Response), điều tần thứ cấp (AGC) và dịch chuyển phụ tải trong ngày (Daily Peak Shaving / Load Shifting).
- **Sự phối hợp nội bộ trong Container `Storage`:**
  - Nhờ có `Supercapacitor` dập xung tần số cao và `Hydrogen_Storage` gánh phần năng lượng chu kỳ dài, `BESS` được tối ưu hóa chế độ sạc/xả trong dải SOC an toàn, hạn chế các chu kỳ xả sâu (DoD lớn), tối đa hóa tuổi thọ toàn trạm.

## 3. Điểm mạnh, Điểm yếu & Ứng dụng (Pros, Cons & Use Cases)
- **Điểm mạnh (Strengths):**
  - Hiệu suất chuyển đổi khứ hồi cao (85% - 92%).
  - Mật độ năng lượng tốt, công nghệ chín muồi, chuỗi cung ứng phổ biến.
  - Khả năng kiểm soát linh hoạt dòng công suất tác dụng và phản kháng (P-Q).
- **Điểm yếu (Weaknesses):**
  - Tuổi thọ chu kỳ (Cycle Life) bị hao mòn mạnh nếu xả sâu liên tục hoặc dòng C-rate vượt định mức ($\approx 4,000 - 8,000$ chu kỳ).
  - Nhạy cảm với stress nhiệt độ, tiềm ẩn nguy cơ suy thoái nhiệt và cháy nổ (Thermal Runaway).
- **Khi nào dùng (When to use):**
  - Cân bằng cung cầu năng lượng trong ngày (1 - 4 giờ), kinh doanh chênh lệch giá (Arbitrage).
  - Điều chỉnh tần số lưới điện, tích hợp nguồn năng lượng tái tạo có tính chu kỳ ngày/đêm.
- **Khi nào KHÔNG dùng (When NOT to use):**
  - Lọc dao động xung tần số mili-giây liên tục (gây suy hao cell pin nhanh chóng).
  - Lưu trữ năng lượng dài hạn qua nhiều ngày hoặc theo mùa (chi phí tự xả và tổn thất dung lượng lớn).

## 4. Các Thực thể Con (Sub-Entities)
*(Các cấu phần kỹ thuật vi mô tạo nên thực thể vĩ mô BESS - Template 2):*
- **[BESS_Cell_Chemistry](obsidian://open?file=BESS_Cell_Chemistry):** Mạch lõi pin hóa học (LFP, NMC) - Quyết định mật độ năng lượng và phương trình suy hao.
- **[BESS_Inverter](obsidian://open?file=BESS_Inverter):** Bộ nghịch lưu lưu trữ - Quyết định chế độ điều khiển tạo lưới (Grid-forming) hay bám lưới (Grid-following).
- **[BESS_Cooling](obsidian://open?file=BESS_Cooling):** Hệ thống quản lý nhiệt (TMS - Thermal Management System) - Quyết định tổn thất nhiệt Joule và an toàn trạm.
