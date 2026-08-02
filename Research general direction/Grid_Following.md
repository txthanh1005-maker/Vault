---
weighttree: 2
contributors: 
  - N.H.Anh k66
---
Direct Parent Connection: -> [[Inverter]]

# GRID FOLLOWING (NGHỊCH LƯU BÁM LƯỚI / GFL)
*(Nút Hub Phân loại Lớp 2: Chế độ điều khiển bám lưới truyền thống sử dụng bộ khóa pha SRF-PLL và điều khiển bơm công suất tác dụng - phản kháng).*

## 1. Bản chất & Tính chất Chung (Common Properties)
- Là chế độ vận hành chủ đạo của hầu hết biến tần nguồn điện mặt trời (PV) và điện gió nối lưới hiện nay, hoạt động như một **nguồn dòng được điều khiển (Controlled Current Source)** mắc song song vào một lưới điện áp nền vững chắc.
- Sử dụng bộ khóa pha trong hệ tọa độ đồng bộ quay (**SRF-PLL** - Synchronous Reference Frame Phase-Locked Loop) để đo lường góc pha $\theta_{grid}$ và tần số lưới, từ đó đồng bộ vectơ dòng điện phát ra.
- Thực hiện điều khiển dòng điện công suất tác dụng và phản kháng ($P-Q$ Control / Current Loop) trong hệ tọa độ $dq0$ theo tham chiếu từ nhà điều độ, không có khả năng tự lập điện áp hoặc tần số khi rã lưới.

## 2. Sơ đồ Phân loại (Taxonomy / Splitting)
- **[Power_Sharing](obsidian://open?file=Power_Sharing):** Cơ chế chia sẻ công suất song song giữa các nghịch lưu bám lưới ($P-Q$ Proportional Current Sharing / Master-Slave Coordination), chia tải tác dụng và phản kháng theo tỷ lệ công suất định mức ($P_i = k_i P_{total}$) mà không gây xung đột điện áp thanh cái.
