---
weighttree: 0
contributors:
  - N.H.Anh k66
---
Direct Parent Connection: -> [[Grid_Physical_Assets]]

# POWER ELECTRONICS (ĐIỆN TỬ CÔNG SUẤT)
*(Nút Không gian Lớp 1: Cổng giao tiếp phần cứng, biến đổi điện áp và dòng điện kết nối Nguồn, Lưu trữ và Tải với Lưới điện).*

## 1. Bản chất & Tính chất Chung (Common Properties)
- Là công nghệ cốt lõi sử dụng linh kiện bán dẫn công suất cao (IGBT, MOSFET, SiC, GaN) đóng cắt ở tần số cao để biến đổi dạng điện áp, tần số và dòng điện (AC/DC, DC/AC, DC/DC, AC/AC).
- Đóng vai trò là "mạch máu và khớp xương" kết nối mọi nguồn tài nguyên phân tán (DERs), pin lưu trữ (BESS), xe điện (EV) và năng lượng tái tạo vào lưới xoay chiều AC.
- Quyết định đến đặc tính động học tức thời của lưới điện hiện đại: thay thế quán tính vật lý của máy phát truyền thống bằng **Quán tính ảo (Virtual Inertia / Grid-forming)**, đồng thời đưa ra thách thức về độ méo dạng sóng hài và ổn định nhanh.

## 2. Sơ đồ Phân loại (Taxonomy / Splitting)
- **[Inverter](obsidian://open?file=Inverter):** (Nghịch lưu / Bi-directional Grid-tied Inverter) Bộ biến đổi DC/AC hai chiều, cổng giao tiếp chủ chốt cho điện mặt trời (PV), hệ thống pin lưu trữ (BESS) và xe điện (V2G) kết nối với lưới AC; chế độ Grid-following và Grid-forming.
- **[DC_DC_Converter](obsidian://open?file=DC_DC_Converter):** Bộ biến đổi một chiều - một chiều (Buck, Boost, Dual Active Bridge), điều chỉnh mức áp DC, phối hợp trở kháng và điều phối dòng năng lượng giữa pin, siêu tụ (`Supercapacitor`) với Bus DC.
- **[STATCOM](obsidian://open?file=STATCOM):** (Static Synchronous Compensator) Thiết bị bù đồng bộ tĩnh thuộc họ FACTS sử dụng bộ biến đổi VSC để bù công suất phản kháng ($Q$) nhanh trong thời gian thực, ổn định điện áp lưới AC động và giảm LVRT.
- **[MMC](obsidian://open?file=MMC):** (Modular Multilevel Converter) Bộ biến đổi nhiều bậc dạng module chuyên trách truyền tải điện áp cao dòng một chiều (HVDC), kết nối các trang trại điện gió ngoài khơi (Offshore Wind Farm) công suất hàng trăm MW với sóng hài cực thấp.
