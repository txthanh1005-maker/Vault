---
contributors: 
  - D.M.Hai K67
---
Direct Parent Connection: -> [[Algorithm]]

# Model Predictive Control (MPC_Control)
*(Thuật toán Điều khiển Dự báo Mô hình, tối ưu hóa các quyết định điều khiển trong tương lai dựa trên mô hình toán học của hệ thống lưới điện)*

*Implemented by Meta-Agent*

## 1. Đặc tính Nội tại (Pure Entity View)
*(Phần Âm: Cấu trúc toán học của MPC)*
- **Cấu trúc lõi:** Bao gồm mô hình dự báo (Predictive Model), hàm mục tiêu (Objective Function), và bộ giải tối ưu hóa (Optimizer).
- **Phương trình chi phối:** 
  - Giải quyết bài toán tối ưu hóa có ràng buộc (Constrained Optimization) tại mỗi bước thời gian $k$ để tìm ra chuỗi tín hiệu điều khiển tối ưu $u(k), u(k+1)...$
  - Phương trình động học trạng thái lưới điện (State-space model): $x(k+1) = Ax(k) + Bu(k)$.
- **Đặc điểm:** Chỉ áp dụng tín hiệu điều khiển đầu tiên của chuỗi giải pháp, sau đó trượt cửa sổ thời gian (Receding Horizon) và tính toán lại ở bước tiếp theo. Đòi hỏi năng lực tính toán (Computational power) rất lớn.

## 2. Tương tác Cục bộ (Local Interaction View)
*(Phần Dương: Tương tác với lưới điện và luồng dữ liệu)*
- **Hệ tham chiếu (Container):** SCADA/EMS, Microgrid Central Controller.
- **Tương tác Đầu vào:** Phụ thuộc cực kỳ lớn vào tính liên tục và độ chính xác của trạng thái lưới điện $x(k)$ do PMU cung cấp. Nếu tín hiệu bị mất hoặc trễ, hàm dự báo của MPC sẽ giải ra nghiệm sai. Do đó, nó bắt buộc phải liên kết chặt chẽ với module [[Signal_Recovery]].
- **Tương tác Đầu ra:** Phát tín hiệu điều khiển góc pha, công suất tác dụng/phản kháng cho Inverter, BESS hoặc máy phát điện đồng bộ (VSG).
- **Động lực học:** Nhờ khả năng dự báo trước tương lai, MPC có thể xử lý các ràng buộc vật lý của lưới (ví dụ: giới hạn dòng điện của Inverter) tốt hơn rất nhiều so với PI Control, giúp hệ thống không bị vượt ngưỡng an toàn trong quá trình phục hồi.

## 3. Điểm mạnh, Điểm yếu & Ứng dụng (Pros, Cons & Use Cases)
- **Điểm mạnh (Strengths):**
  - Khả năng xử lý trực tiếp các ràng buộc (Constraints) cứng của hệ thống vật lý.
  - Tối ưu hóa đa mục tiêu (Multi-variable) xuất sắc hơn PI Control truyền thống.
- **Điểm yếu (Weaknesses):**
  - Cực kỳ nhạy cảm với lỗi dữ liệu đầu vào (Delay/Loss).
  - Yêu cầu khối lượng tính toán lớn, khó đáp ứng được trong các chu kỳ điều khiển tính bằng mili-giây nếu mô hình quá phức tạp.
- **Khi nào dùng (When to use):** Điều khiển thứ cấp (Secondary Control), cân bằng công suất Microgrid, hoặc phối hợp nhiều BESS.
- **Khi nào KHÔNG dùng (When NOT to use):** Ở các cấp điều khiển sơ cấp (Primary Control) đòi hỏi phản ứng ngay lập tức mà phần cứng xử lý chưa đủ mạnh.