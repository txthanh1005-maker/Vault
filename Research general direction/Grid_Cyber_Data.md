---
contributors: 
  - Khanh k67
  - Thanh K67
---
Direct Parent Connection: -> [[Power_Grid_System]]

# Grid Cyber Data (Trạm Thần kinh & Truyền tin)
*(Hub Node đại diện cho hệ thống truyền dẫn tín hiệu, SCADA, IoT và Không gian mạng)*

## 1. Bản chất & Tính chất Chung (Common Properties)
Đây là trụ cột "Hệ Thần kinh" (Cyber/Data Layer). Khác với lớp vật lý truyền tải Electron, lớp này truyền tải Bit dữ liệu. Nó thu thập các phép đo (Measurements) từ Thể xác vật lý và truyền về cho Não bộ (Intelligence) xử lý, sau đó truyền tín hiệu điều khiển từ Não bộ ngược xuống Thể xác. Các yếu tố sống còn bao gồm băng thông, độ trễ (latency), mất gói tin, và an ninh mạng (Cybersecurity).

## 2. Sơ đồ Phân loại (Taxonomy / Splitting)
- [Communication](obsidian://open?file=Communication): Mạng viễn thông, SCADA, IoT