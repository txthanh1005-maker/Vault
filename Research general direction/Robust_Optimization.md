---
contributors: 
  - Thanh K67
---
Direct Parent Connection: -> [[Resilience_Short_Term]]

# ROBUST OPTIMIZATION (TỐI ƯU HÓA BỀN VỮNG - RO)
Robust Optimization (RO) là một nhánh toán học giải quyết sự bất định (Uncertainty) không dựa trên xác suất, mà dựa trên giới hạn biên (Boundary). Đây là công cụ phòng thủ tối thượng cho lưới điện trong trạng thái khẩn cấp.

## 1. Bản chất Toán học (Pure Entity View)
- **Cấu trúc Bài toán (Min-Max Problem):** 
  Khác với các bài toán tối ưu thông thường, RO là một cuộc chơi hai chiều (Two-stage Min-Max):
  $$\min_{x} \max_{u \in U} \min_{y \in \Phi(x,u)} f(x, y, u)$$
  Trong đó: Hệ thống cố gắng **Tối thiểu hóa** ($\min$) thiệt hại, trong khi Thiên nhiên/Sự cố cố gắng **Tối đa hóa** ($\max$) thiệt hại trong một Tập bất định (Uncertainty Set $U$).
- **Không gian Bất định (Uncertainty Set $U$):** 
  RO không quan tâm xác suất xảy ra sự cố là 1% hay 99%. Nó chỉ khoanh vùng một không gian ranh giới (ví dụ: Tải có thể dao động $\pm 20\%$, mất ngẫu nhiên 2 đường dây).
- **Phòng thủ Biên (Boundary Defense):** Nghiệm của RO là một nghiệm bảo thủ tuyệt đối. Hệ thống được ép buộc phải có đủ "khoảng đệm vật lý" (physical margin) để tồn tại ngay cả khi kịch bản tồi tệ nhất (Worst-case scenario) ở rìa của tập $U$ xảy ra.

## 2. Ứng dụng trong Resilience Short-Term
- Khi ứng dụng vào khung 15-60 phút, RO sẽ ra lệnh cho lưới điện phải dự trữ một lượng BESS hoặc DR cực lớn. 
- **Ưu điểm:** Đảm bảo 100% không sập lưới (Blackout) miễn là nhiễu loạn nằm trong tập $U$ đã định hình. Khả năng sinh tồn (Survivability) cao nhất.
- **Nhược điểm:** Cực kỳ tốn kém và lãng phí (Bảo thủ - Conservative). Vì hệ thống luôn chuẩn bị cho cái xấu nhất, nó sẽ hi sinh lượng lớn lợi ích kinh tế (Social Welfare) ở trạng thái bình thường.