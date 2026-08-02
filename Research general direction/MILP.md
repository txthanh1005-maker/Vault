---
weighttree: 3
contributors:
  - Thanh K67
---
Direct Parent Connection: -> [[Exact_Solvers]]

# MILP (Mixed-Integer Linear Programming)
Thuật toán tối ưu hóa tuyến tính nguyên hỗn hợp, được sử dụng làm bộ giải toán học cốt lõi để tìm nghiệm tối ưu toàn cục cho bài toán quy hoạch và vận hành lưới điện.

## 1. Bản chất & Tính chất Chung (Common Properties)
- Là thuật toán tối ưu xác định (deterministic), không dùng Heuristic/AI, đảm bảo tìm được nghiệm tối ưu toàn cục nếu hội tụ.
- Cho phép mô hình hóa các quyết định rời rạc (Bật/Tắt máy cắt, Xây dựng trạm) thông qua biến nguyên (Integer/Binary variables).
- Mọi hàm mục tiêu và ràng buộc đều phải được tuyến tính hóa.

## 2. Sơ đồ Phân loại (Taxonomy / Splitting)
- **[[MILP_Single_Node]]:** Bài toán xét 1 nút.
- **[[MILP_Multi_Node]]:** Bài toán xét nhiều nút.
