Direct Parent Connection: -> [[Storage]]

# BESS (HỆ THỐNG LƯU TRỮ NĂNG LƯỢNG PIN)
*(Nút Thực thể Vĩ mô - Macro Entity: BESS là một cỗ máy hoàn chỉnh tương tác trực tiếp với Lưới điện, đồng thời làm "Vỏ bọc" cho các linh kiện vi mô bên trong).*

## 1. PHẦN ÂM (Nội tại của BESS - Pure Entity)
*(Đặc tính vật lý tổng quát của cả hệ thống như một "Hộp đen", không bàn tới cấu tạo chi tiết bên trong).*
- **Trạng thái sạc (State of Charge - SOC):** Tham số cốt lõi $SOC(t) = SOC(t-1) + \int (P_{charge}\eta_c - \frac{P_{discharge}}{\eta_d})dt$. Bị giới hạn nghiêm ngặt trong khoảng $[SOC_{min}, SOC_{max}]$ để bảo vệ tuổi thọ.
- **Tổn hao hệ thống (System Efficiency):** Hiệu suất khứ hồi (Round-trip efficiency) bị suy giảm bởi chu trình biến đổi kép DC-AC-DC và tổn thất nhiệt hệ thống.
- **Tuổi thọ vòng lặp (Cycle Life):** Phụ thuộc vào Độ sâu xả (Depth of Discharge - DoD) và C-rate (Tốc độ sạc/xả) của toàn trạm.

## 2. PHẦN DƯƠNG (Tương tác với Lưới - Local Interaction)
*(Luồng giao tiếp và tác động của BESS ra môi trường bọc trực tiếp nó - Lưới Điện).*
- **Động học Công suất (P-Q Control):** Bơm/rút $P$ và $Q$ độc lập, đảo chiều siêu tốc (<20ms).
- **Đáp ứng Tần số (Frequency Response):** 
  - *Primary:* Cung cấp Quán tính ảo (Virtual Inertia) và Fast Frequency Response (FFR).
  - *Secondary:* Tham gia AGC gánh đỉnh.
- **Dịch chuyển năng lượng (Energy Shifting):** Mua điện lúc rẻ/thấp điểm, xả điện lúc đắt/cao điểm (Arbitrage).

## 3. CÁC THỰC THỂ CON (Sub-Entities / Internal Components)
*(Khi "mổ bụng" BESS, ta thấy các linh kiện con. Các linh kiện này cũng là Nút Thực Thể (Template 2), và "Phần Dương" của chúng sẽ trỏ ngược về vỏ bọc là BESS).*
- **[[BESS_Cell_Chemistry]]:** Mạch lõi pin (LFP, NMC) - Quyết định mật độ năng lượng và phương trình lão hóa hóa học.
- **[[BESS_Inverter]]:** Mạch biến đổi điện tử công suất - Quyết định khả năng tạo lưới (Grid-forming) hay bám lưới (Grid-following).
- **[[BESS_Cooling]]:** Hệ thống tản nhiệt - Quyết định độ suy hao Joule và an toàn cháy nổ.