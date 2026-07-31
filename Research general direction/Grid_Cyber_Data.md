---
contributors: 
  - Khanh k67
  - Thanh K67
---
Direct Parent Connection: -> [[Power_Grid_System]]

# Grid Cyber Data (Trạm Thần kinh & Truyền tin)
*(Hub Node đại diện cho hệ thống truyền dẫn tín hiệu, SCADA, IoT và An ninh không gian mạng lưới điện)*

## 1. Bản chất & Tính chất Chung (Common Properties)
Đây là trụ cột "Hệ Thần kinh" (Cyber/Data Layer). Khác với lớp vật lý truyền tải Electron, lớp này truyền tải Bit dữ liệu. Nó thu thập các phép đo (Measurements) từ Thể xác vật lý và truyền về cho Não bộ (Intelligence) xử lý, sau đó truyền tín hiệu điều khiển từ Não bộ ngược xuống Thể xác. Các yếu tố sống còn bao gồm băng thông, độ trễ (latency), mất gói tin, tính toàn vẹn của dữ liệu và đặc biệt là an ninh mạng (Cybersecurity).

## 2. Sơ đồ Phân loại (Taxonomy / Splitting)
- **[Communication](obsidian://open?file=Communication):** Hạ tầng mạng viễn thông, SCADA, IoT, cung cấp đường truyền kết nối vật lý và logic.
- **[Grid_Cyber_Security](obsidian://open?file=Grid_Cyber_Security):** Hệ thống bảo vệ lưới điện khỏi rủi ro không gian mạng (Ví dụ: Tấn công tiêm dữ liệu sai FDI, Denial of Service DoS), đảm bảo an ninh dòng dữ liệu. (Trọng tâm chiến dịch mới)
- **[Grid_Data_Management](obsidian://open?file=Grid_Data_Management):** Xử lý dữ liệu lớn (Big Data) từ lưới điện (như PMU, Smart Meters), tổ chức lưu trữ và phân phối thông tin.