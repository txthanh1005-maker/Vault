---
contributors:
  - Thanh K67
---
Direct Parent Connection: -> [[Algorithm]]

# BỘ GIẢI CHÍNH XÁC (EXACT SOLVERS)
Nhóm các thuật toán toán học dựa trên lý thuyết tối ưu hóa chặt chẽ, được thiết kế để tìm ra nghiệm tối ưu toàn cục (global optimum) cho bài toán, trái ngược với các phương pháp xấp xỉ hay phỏng đoán (Heuristics).

## 1. Bản chất & Tính chất Chung (Common Properties)
- Đảm bảo tính hội tụ về nghiệm tối ưu toàn cục (nếu bài toán khả thi và được mô hình hóa đúng chuẩn).
- Độ phức tạp tính toán rất cao (đặc biệt khi vướng vào các biến rời rạc, dẫn đến NP-hard).
- Cần công thức toán học tường minh tuyệt đối (hàm mục tiêu, tập ràng buộc) và thường phụ thuộc vào các công cụ giải mạnh mẽ (Gurobi, CPLEX, MOSEK).

## 2. Sơ đồ Phân loại (Taxonomy / Splitting)
- **[[MILP]]:** (Quy hoạch Tuyến tính Nguyên hỗn hợp) Thuật toán cốt lõi cho các bài toán lưới điện chứa quyết định Bật/Tắt máy cắt hoặc Xây/Không xây trạm.
- **[[LP]]:** (Quy hoạch Tuyến tính) Bài toán thuần tuyến tính liên tục, giải cực nhanh.
- **[[NLP]]:** (Quy hoạch Phi tuyến) Xử lý trực tiếp các phương trình AC Power Flow nhưng đối diện rủi ro rơi vào nghiệm tối ưu cục bộ (local optimum).
- **[[DP]]:** (Quy hoạch Động) Giải quyết bài toán theo trục thời gian bằng cách bẻ nhỏ thành từng chu kỳ.
