---
contributors: 
  - N.H.Anh k66
---
Direct Parent Connection: -> [[Inverter]]

# HARMONIC MITIGATION (TRIỆT TIÊU SÓNG HÀI CHỦ ĐỘNG / APF)
*(Nút Hub Phân loại Lớp 2: Kỹ thuật cải thiện chất lượng điện năng, lọc và bù sóng hài chủ động từ phía tải cục bộ và phía lưới điện AC).*

## 1. Bản chất & Tính chất Chung (Common Properties)
- Là năng lực điều khiển nâng cao giúp biến tần (Inverter) vừa phát công suất hữu công ($P$), vừa đóng vai trò như một bộ lọc tích cực (Active Power Filter - APF) triệt tiêu các thành phần méo dạng sóng hài dòng điện và điện áp.
- Ứng dụng các lý thuyết công suất tức thời (p-q Theory / Conservative Power Theory) và bộ điều khiển cộng hưởng đa tần (Multi-Resonant PR Controller) tại các tần số hài bậc lẻ (5, 7, 11, 13, 17, 19).
- Đảm bảo tổng độ méo dạng sóng hài dòng điện ($\text{THD}_i$) và điện áp ($\text{THD}_v$) tại điểm kết nối chung (PCC) tuân thủ tiêu chuẩn nghiêm ngặt IEEE 519 (< 5%).

## 2. Sơ đồ Phân loại (Taxonomy / Splitting)
- **[Local_Load_Harmonic_Mitigation](obsidian://open?file=Local_Load_Harmonic_Mitigation):** Bù sóng hài do tải chỉnh lưu phi tuyến cục bộ (Local Nonlinear Load Harmonic Compensation), bơm dòng hài bù ngược pha để dòng lưới tổng hoàn toàn hình sin.
- **[Grid_Harmonic_Mitigation](obsidian://open?file=Grid_Harmonic_Mitigation):** Triệt tiêu méo hài từ phía điện áp lưới AC (Grid Voltage Harmonic Rejection) và chống cộng hưởng trở kháng (LCL Resonance Damping) trong môi trường lưới điện yếu.
