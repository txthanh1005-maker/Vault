---
weighttree: 1
contributors: 
  - Thanh K67
---
Direct Parent Connection: -> [[Demand_Response]]

# PRICE-BASED DEMAND RESPONSE (PBDR)
Đáp ứng nhu cầu dựa trên giá (PBDR) là cơ chế điều chỉnh hành vi tiêu thụ điện năng thông qua các tín hiệu kinh tế gián tiếp, nhắm vào sự nhạy cảm về giá của người dùng.

## 1. Đặc tính Nội tại (Pure Entity View)
- **Người đảm nhiệm (Assignee):** pure_entity_researcher
- **Cấu trúc Vi mô Tín hiệu Giá:**
  - *Khối Tĩnh (Static - TOU):* Bậc thang cố định.
  - *Xung Kích (Impulse - CPP/PTR):* Gai xung biên độ lớn, độ rộng xung hẹp.
  - *Động Liên tục (Continuous Dynamic - RTP):* Chuỗi thời gian ngẫu nhiên (RTP).
  - *Tích lũy (Cumulative - IBR):* Hàm đơn điệu tăng theo tổng sản lượng (Block Rate).
- **Đặc tính Toán học & Vật lý:**
  - *Ma trận Độ đàn hồi (Elasticity Matrix):* Hệ số đàn hồi tự thân ($\epsilon_{i,i} < 0$, lực nén cục bộ) và hệ số đàn hồi chéo ($\epsilon_{i,j} \ge 0$, sự dịch chuyển đỉnh tải).
  - *Phương trình Trạng thái Phụ tải:* Mô hình hóa dưới dạng tuyến tính hoặc lũy thừa (ví dụ: $d(p) = d_0 \prod (p_j/p_{j,0})^{\epsilon_{i,j}}$).
  - *Ràng buộc Bảo toàn Năng lượng:* Đối với các tải có thể dịch chuyển (Shiftable Loads), tổng năng lượng trong chu kỳ $T$ là bất biến: $\sum d_{shift}(t) = \sum d_{shift,0}(t)$.
  - *Mỏi Đàn hồi (Elasticity Fatigue):* Sự bào mòn khả năng đáp ứng. Kích thích tần số cao liên tục làm $\epsilon \to 0$ (phụ tải trở nên trơ với giá).

## 2. Tương tác Cục bộ (Local Interaction View)
- **Người đảm nhiệm (Assignee):** grid_interaction_researcher
- **Hệ tham chiếu (Container):** Aggregator / Lưới phân phối thông minh (Smart Distribution Grid).
- **Vai trò tương tác:** Hoạt động như một "Bộ đệm điều chỉnh phụ tải phi tuyến" (Non-linear load adjustment buffer). Container tận dụng bộ đệm này để định hình lại (shape) biểu đồ phụ tải mà không cần can thiệp cắt điện trực tiếp.
- **Tín hiệu Đầu vào (Inputs):** Tín hiệu giá (Price signal $P_t$) từ Aggregator và (tùy chọn) tín hiệu ràng buộc công suất tại điểm ghép nối chung (PCC).
- **Tín hiệu Đầu ra (Outputs):** Công suất tiêu thụ thực tế đo được qua Smart Meter ($D_t$) và dữ liệu Baseline lịch sử.
- **Hàm truyền tương tác (Transfer Function & Dynamics):**
  - *Độ trễ pha (Dead-time):* Là khâu trễ bậc 1 do quán tính nhận thức và truyền tin.
  - *Đỉnh thứ cấp (Rebound Peak):* Hệ quả của độ co giãn chéo. Lực nén ở vùng giá cao đẩy khối lượng năng lượng sang vùng giá thấp, tạo ra một đỉnh tiêu thụ cục bộ mới gây khó khăn cho Aggregator.
  - *Vùng bão hòa bẩm sinh:* Tăng tín hiệu giá vượt một ngưỡng nhất định không còn mang lại sự suy giảm tải thêm nữa (Saturation limit).
