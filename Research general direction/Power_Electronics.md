---
contributors: 
  - Khanh k67
  - Thanh K67
---
Direct Parent Connection: -> [[Grid_Physical_Assets]]

# Power Electronics
*(Điện tử công suất, thiết bị biến đổi và cổng giao tiếp phần cứng)*

## 1. Đặc tính Nội tại (Pure Entity View)
- Bao gồm Inverter, Converter, HVDC, FACTS.
- Bản chất là sử dụng linh kiện bán dẫn công suất cao đóng cắt tần số cao để biến đổi điện áp, dòng điện (AC/DC, DC/AC).

## 2. Tương tác Cục bộ (Local Interaction View)
- Hệ tham chiếu (Container): Grid_Physical_Assets
- Đóng vai trò là khớp xương và mạch máu kết nối Nguồn (Source) và Tải (Load) vào lưới điện truyền tải/phân phối. Đóng góp quán tính ảo (Virtual Inertia).