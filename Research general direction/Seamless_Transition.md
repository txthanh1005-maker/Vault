---
weighttree: 3
contributors: 
  - N.H.Anh k66
---
Direct Parent Connection: -> [[Inverter]]

# SEAMLESS TRANSITION (CHUYỂN ĐỔI CHẾ ĐỘ LIỀN MẠCH)
*(Nút Hub Phân loại Lớp 2: Các cơ quan kỹ thuật chuyển đổi chế độ làm việc giữa Nối lưới - Grid Connected và Độc lập - Islanded trong Inverter & Microgrid).*

## 1. Bản chất & Tính chất Chung (Common Properties)
- Là năng lực điều khiển chiến lược cho phép biến đổi trạng thái làm việc của bộ nghịch lưu (Inverter) từ chế độ Nối lưới (Grid-connected / Grid-following) sang chế độ Độc lập (Islanded / Grid-forming) và ngược lại.
- Đảm bảo tính linh hoạt, phục hồi tải quan trọng (Critical Load Recovery) khi lưới điện rã lưới hoặc mất điện đột ngột, tuân thủ nghiêm ngặt các tiêu chuẩn IEEE 1547 về thời gian cho phép chuyển tiếp (< 20 ms).
- Giải quyết hai thách thức cơ bản của quá trình chuyển mạch: (1) Tránh quá áp/dòng đột biến khi cắt mạch; (2) Đồng bộ hoàn hảo pha, tần số và biên độ điện áp trước khi hòa lưới.

## 2. Sơ đồ Phân loại (Taxonomy / Splitting)
- **[Open_Transition](obsidian://open?file=Open_Transition):** Chuyển đổi hở (Break-before-make / Hard switching), cắt cầu dao nối lưới trước khi tái lập điện áp tự trị bằng Grid-Forming, kỹ thuật giảm dốc sụt điện áp và phục hồi tải ưu tiên.
- **[Close_Transition](obsidian://open?file=Close_Transition):** Chuyển đổi kín (Make-before-break / Soft switching / Synchronized Reconnection), bộ khóa pha và đồng bộ trước (Pre-synchronization loop) khớp góc pha $\Delta \theta$, áp $\Delta V$, tần số $\Delta f$ trước khi đóng MCCB hòa lưới không xung dòng.
