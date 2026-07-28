---
contributors: 
  - Thanh K67
---
Direct Parent Connection: -> [[Resilience_Short_Term]]

# STOCHASTIC OPTIMIZATION (TỐI ƯU HÓA NGẪU NHIÊN - SO)
Stochastic Optimization (SO) là nhánh toán học giải quyết sự bất định dựa trên Hàm mật độ xác suất (Probability Density Function - PDF). Trái ngược với Robust Optimization (RO) cứng nhắc, SO tìm kiếm một sự cân bằng (Trade-off) giữa rủi ro và chi phí.

## 1. Bản chất Toán học (Pure Entity View)
- **Cấu trúc Bài toán (Two-Stage Objective):** 
  SO thường được mô hình hóa dưới dạng bài toán 2 giai đoạn (Here-and-Now và Wait-and-See):
  $$\min_{x} \left( c^T x + \mathbb{E}_{\omega \in \Omega}[Q(x, \omega)] \right)$$
  Trong đó:
  - $x$: Các quyết định phải đưa ra ngay lập tức (Day-ahead hoặc Pre-dispatch).
  - $\omega$: Kịch bản sự cố ngẫu nhiên (Scenario) trích xuất từ không gian mẫu $\Omega$.
  - $\mathbb{E}$: Giá trị kỳ vọng toán học dựa trên xác suất xảy ra của $\omega$.
  - $Q(x, \omega)$: Hàm chi phí khắc phục hậu quả (Recourse cost) hoặc thiệt hại mất tải (Loss of Load) khi kịch bản $\omega$ xảy ra.
- **Tính toán theo Kịch bản (Scenario-based):** Không gian bất định được rời rạc hóa thành hàng ngàn kịch bản (Monte Carlo Simulation hoặc Scenario Tree). SO tối ưu hóa giá trị trung bình trên toàn bộ cây kịch bản này.

## 2. Ứng dụng trong Resilience Short-Term
- **Chấp nhận Rủi ro (Risk-aware):** Trong khung 15-60 phút, SO không cố gắng cứu lưới bằng mọi giá như RO. Nếu một kịch bản sự cố có xác suất xảy ra quá thấp (HILP cực đoan), SO sẽ "chấp nhận buông" (cho phép sa thải một phần phụ tải - Loss of Load) thay vì tốn hàng triệu đô la để duy trì dự phòng pin BESS khổng lồ.
- **Ưu điểm:** Cân bằng kinh tế xuất sắc. Tổng chi phí kỳ vọng vận hành rẻ hơn rất nhiều so với RO, phù hợp với thị trường điện cạnh tranh.
- **Nhược điểm:** Đòi hỏi dữ liệu lịch sử khổng lồ để vẽ được đường cong phân bố xác suất chính xác. Nếu hàm xác suất bị sai lệch, hoặc xảy ra sự kiện "Thiên nga đen" (Black Swan - nằm ngoài dự báo), hệ thống áp dụng SO sẽ sụp đổ. Tính toán nặng nề do sự bùng nổ tổ hợp của cây kịch bản (Curse of Dimensionality).