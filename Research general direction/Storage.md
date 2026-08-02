---
weighttree: 0
contributors:
  - H.T.Hải K67
---
Direct Parent Connection: -> [[Grid_Physical_Assets]]

# STORAGE (HỆ THỐNG LƯU TRỮ NĂNG LƯỢNG HỖN HỢP - HESS)
*(Nút Phân loại Lớp 1 đại diện cho tổ hợp các công nghệ lưu trữ năng lượng phối hợp trong hệ thống điện).*

## 1. Bản chất & Tính chất Chung (Common Properties)
*(Dự thảo cấu trúc bởi AI Meta-Agent - Chờ H.T.Hải K67 hiệu chỉnh)*
- Có khả năng hấp thụ, tích trữ và giải phóng năng lượng điện, đóng vai trò hạt nhân trong việc cân bằng công suất thời gian thực (P, Q) và dịch chuyển năng lượng theo thời gian (Energy Shifting).
- **Tuổi thọ & Suy hao ([Storage_Degradation_Lifetime](obsidian://open?file=Storage_Degradation_Lifetime)):** Cả 3 công nghệ đều bị suy hao theo thời gian, số chu kỳ sạc/xả (Cycle Life) và stress nhiệt/điện áp.
- **Chi phí & Kinh tế ([Storage_Economics_Cost](obsidian://open?file=Storage_Economics_Cost)):** Đánh giá chung dựa trên chi phí đầu tư ban đầu (CAPEX), chi phí vận hành (OPEX) và chi phí san bằng LCOS (Levelized Cost of Storage).

## 2. Sơ đồ Phân loại (Taxonomy by Dynamic Response & C-rate)
*(Dự thảo phân loại bởi AI Meta-Agent - Chờ H.T.Hải K67 hiệu chỉnh)*
- **[Supercapacitor](obsidian://open?file=Supercapacitor) (Siêu tụ điện):**
  - *Thời gian phản ứng:* Siêu nhanh (thang đo mili-giây $\text{ms}$).
  - *Tốc độ sạc/xả (C-rate):* Cực đại (Mật độ công suất siêu cao, mật độ năng lượng thấp).
  - *Vai trò phối hợp:* Đảm nhận thành phần tần số cao (High-frequency power fluctuations), dập xung công suất tức thời, bảo vệ pin.
- **[BESS](obsidian://open?file=BESS) (Hệ thống Pin lưu trữ):**
  - *Thời gian phản ứng:* Trung hạn (thang đo giây đến giờ $\text{s}-\text{h}$).
  - *Tốc độ sạc/xả (C-rate):* Trung bình (Cân bằng tối ưu giữa mật độ công suất và mật độ năng lượng).
  - *Vai trò phối hợp:* Đóng vai trò chủ đạo cân bằng lệch công suất dải trung, điều phối P-Q và tham gia đáp ứng tần số nhanh (FFR).
- **[Hydrogen_Storage](obsidian://open?file=Hydrogen_Storage) (Lưu trữ Hydro):**
  - *Thời gian phản ứng:* Dài hạn (thang đo giờ đến mùa $\text{h}-\text{seasonal}$).
  - *Tốc độ sạc/xả (C-rate):* Thấp (Mật độ năng lượng cực lớn, động lực học khởi động chậm).
  - *Vai trò phối hợp:* Hấp thụ lượng điện năng lượng tái tạo dư thừa chu kỳ dài hạn, tích trữ mùa và giảm sa thải công suất (curtailment).
