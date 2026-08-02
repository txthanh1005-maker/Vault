---
weighttree: 0
contributors: 
  - Khanh k67
---
Direct Parent Connection: -> [[Electric_Vehicle]]

# EV SIZING (QUY HOẠCH KÍCH CỠ & DUNG LƯỢNG)
*(Thực thể tĩnh định hình giới hạn công suất phần cứng (kW) và dung lượng năng lượng (kWh) của Trạm sạc. Đây là bài toán Lập kế hoạch (Planning), tạo rào cản vật lý trước khi hệ thống bước vào giai đoạn vận hành thời gian thực).*

## 1. Bản chất & Tính chất Chung (Common Properties)
- Bài toán tìm điểm rơi cực tiểu hóa Tổng chi phí vòng đời: Min(TCO) = CAPEX + Present_Value(OPEX).
- Phải thỏa mãn điều kiện chất lượng phục vụ thông qua Lý thuyết hàng đợi bất định (Queueing Theory - M/M/c/K).

## 2. Sơ đồ Phân loại (Taxonomy / Splitting)
*(Bài toán được phân nhánh dựa trên môi trường đấu nối vật lý và sức chứa của lưới điện).*

- **[Grid_Connected_EV_Sizing](obsidian://open?file=Grid_Connected_EV_Sizing):** Định cỡ Trạm sạc Nối lưới. Tùy chọn dung lượng BESS để cắt đỉnh, chịu sự kìm hãm tuyệt đối của rào cản Hosting Capacity tại điểm PCC.
- **[Standalone_EV_Sizing](obsidian://open?file=Standalone_EV_Sizing):** Định cỡ Trạm sạc Độc lập (Off-grid). Bắt buộc phải tích hợp Nguồn tái tạo & BESS, bị trói buộc sinh tử bởi Xác suất mất điện (LPSP) thay vì giới hạn của lưới.