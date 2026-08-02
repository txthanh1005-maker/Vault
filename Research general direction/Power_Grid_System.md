---
weighttree: 0
contributors:
  - Khanh k67
  - Thanh K67
  - D.M.Hai K67
  - H.T.Hải K67
  - N.H.Anh k66
color: red
---
# POWER GRID SYSTEM (Gốc Tọa Độ)
*(Gốc tọa độ của toàn bộ hệ sinh thái nghiên cứu lưới điện. Theo kiến trúc Cyber-Physical System, Nút gốc này phân rã thành 3 Trụ cột cốt lõi: Não bộ, Thể xác, và Thần kinh).*

## 1. Bản chất & Tính chất Chung (Common Properties)
- Là môi trường lớn nhất (Ultimate Container) bao bọc mọi thành phần phát, truyền tải, lưu trữ, tiêu thụ, viễn thông và điều khiển.
- Cấu trúc hệ thống tuân theo mô hình **Không gian Mạng - Vật lý (Cyber-Physical System)**, nơi các quyết định toán học (Não bộ) được truyền qua mạng viễn thông (Thần kinh) để điều khiển các thiết bị công suất thực (Thể xác).
- Mọi Nút Thực Thể (Entity Nodes) khi tương tác vào môi trường này đều phải đồng bộ về Tần số (Frequency) và tuân thủ các dải Điện áp (Voltage) quy định, bảo toàn định luật năng lượng.

## 2. Sơ đồ Phân loại (Taxonomy / Splitting)
Hệ thống điện được phân rã thành **3 Trạm Trung chuyển Tối cao (Hub Nodes)**. Mọi thực thể vật lý, thuật toán, hay thời gian đều phải móc link kế thừa hướng tâm (Bottom-Up) về một trong ba trụ cột này.

### 2.1. Trụ cột Không gian Mạng - Vật lý (Cyber-Physical Pillars)
- **[Grid Intelligence](obsidian://open?file=Grid_Intelligence):** Trạm Điều khiển & Trí tuệ (Não bộ) - Bao bọc Trục thời gian (Power System Scheduling), Thuật toán tối ưu (Algorithm) và Mục tiêu vận hành (Operational Objectives).
- **[Grid Physical Assets](obsidian://open?file=Grid_Physical_Assets):** Trạm Tài sản Vật lý (Thể xác) - Bao bọc các thiết bị công suất (Source, Load, Storage) và cổng giao tiếp phần cứng (Power Electronics).
- **[Grid Cyber Data](obsidian://open?file=Grid_Cyber_Data):** Trạm Thần kinh & Truyền tin (Mạng dữ liệu) - Bao bọc hệ thống Viễn thông, SCADA, IoT và An ninh mạng (Communication).

*(Zettelkasten Note: Đã áp dụng luật Giấu Mắt Đồ Thị. Sử dụng liên kết ẩn `[Tên](obsidian://open?file=Tên)` tại Nút Phân Loại này để duy trì kiến trúc Bottom-Up thuần túy. Khi thiết lập Nút Kịch Bản Template 3, bắt buộc móc tới ít nhất 1 nhánh Não bộ và 1 nhánh Thể xác).*
