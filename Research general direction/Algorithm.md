---
weighttree: 0
contributors: 
  - Thanh K67
  - Khanh k67
  - D.M.Hai K67
---
Direct Parent Connection: -> [[Grid_Intelligence]]

# ALGORITHM (THUẬT TOÁN & TOÁN HỌC)
*(Nút Không gian Lớp 1: Bộ NÃO điều phối toàn bộ các thành phần của lưới điện bằng sức mạnh tính toán).*

## 1. Bản chất & Tính chất Chung (Common Properties)
- Thuật toán hoạt động như một công cụ ra quyết định để điều khiển dòng công suất, đóng cắt thiết bị và bảo vệ hệ thống.
- Bị chi phối bởi **Độ phức tạp tính toán (Big-O)** và **Thời gian thực thi**. Nếu thuật toán giải quá chậm so với chu kỳ vật lý của lưới, hệ thống sẽ sụp đổ.
- Phải đối mặt với sự đánh đổi liên tục giữa Độ chính xác (Mô hình AC phi tuyến, dễ bùng nổ tổ hợp) và Tốc độ (Mô hình DC tuyến tính, kém chính xác điện áp).

## 2. Sơ đồ Phân loại (Taxonomy / Splitting)
### 2.1. Phân loại theo Ứng dụng Lưới điện (Grid Applications)
- **[[Unit_Commitment]]:** Bài toán tổ hợp máy phát, tối ưu hóa trạng thái BẬT/TẮT.
- **[[Optimal_Power_Flow]]:** Bài toán trào lưu công suất tối ưu thời gian thực (OPF).
- **[[State_Estimation]]:** Đánh giá trạng thái, lọc nhiễu SCADA tạo Digital Twin.
- **[[Network_Reconfiguration]]:** Tái cấu trúc mạng lưới, tìm kịch bản đóng/cắt công tắc tối ưu (Switching).
- **[[P2P_Energy_Trading]]:** Bài toán giao dịch năng lượng ngang hàng (Peer-to-Peer), tối ưu hóa lợi ích kinh tế cục bộ giữa các Prosumer.
- **[[MPC]]:** (Model Predictive Control) Điều khiển dự báo mô hình vòng kín, ứng dụng cực mạnh trong bám đuổi quỹ đạo, bù trừ nhiễu (Microgrid) và độ đàn hồi ngắn hạn (Resilience).

### 2.2. Phân loại theo Bản chất Toán học (Mathematical Core)
- **[[Exact_Solvers]]:** Giải chính xác. Bao gồm Quy hoạch Tuyến tính (LP), Nguyên hỗn hợp (MILP), Quy hoạch Động (DP).
- **[[Heuristics_Metaheuristics]]:** Xấp xỉ/Cảm tính. Bao gồm GA, PSO (Stochastic search). -> thuần "bao cát"
- **[[Machine_Learning_Grid]]:** Dữ liệu hóa. Học sâu (ANN), Học tăng cường (DRL).
- **[[Convex_Optimization]]:** Lý thuyết tối ưu lồi (KKT conditions) và không lồi.
- **[[ADMM]]:** Phương pháp phân rã, xử lý đồng thuận phân tán (Distributed consensus).
- **[Game_Theory](obsidian://open?file=Game_Theory):** Lý thuyết trò chơi. Mô hình hóa sự tương tác, cạnh tranh (Stackelberg) và hợp tác giữa các thực thể ra quyết định độc lập (Multi-Agent).

### 2.3. Phân loại theo Xử lý Tín hiệu & Lọc Tần số (Signal Processing & Frequency Filtering - HESS)
- **[Low_Pass_Filter](obsidian://open?file=Low_Pass_Filter):** Bộ lọc thông thấp miền thời gian ($H(s) = \frac{1}{1 + \tau s}$), tách thành phần dao động tần số thấp cho thiết bị lưu trữ chậm/dung lượng lớn (Hydro, BESS).
- **[Fast_Fourier_Transform](obsidian://open?file=Fast_Fourier_Transform):** Biến đổi Fourier nhanh ($O(N \log N)$), phân tích phổ miền tần số để cắt và phân rã tín hiệu dao động công suất tải/tái tạo thành 3 dải chuyên biệt trong Hệ thống Lưu trữ Năng lượng Hỗn hợp (HESS).
