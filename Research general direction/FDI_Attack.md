---
weighttree: 0
contributors: 
  - D.M.Hai K67
---
Direct Parent Connection: -> [[Inside_Attack]]

# False Data Injection Attack (FDI)
*(Tấn công bơm dữ liệu sai: Dạng tấn công tinh vi nhắm vào luồng dữ liệu đo lường, thao túng các con số mà không làm kích hoạt hệ thống cảnh báo lỗi)*

## 1. Đặc tính Nội tại (Pure Entity View)
- **Người đảm nhiệm (Assignee):** pure_entity_researcher
- **Bản chất Toán học:** Thực thể FDI bản chất là một phép biến đổi tuyến tính có chủ đích trên không gian đo lường. Lõi toán học của FDI dựa trên sự thao túng không gian cột (column space / Range) của hệ thống quan sát $z = Hx + e$.
- **Phương trình chi phối (Stealth Condition):** FDI tiêm véc-tơ nhiễu $a \in \mathbb{R}^m$ sao cho $a = Hc$ (với $c$ là sự dịch chuyển trạng thái). Khi đó véc-tơ phần dư $r_a = z_a - H\hat{x}_a = r$ bất biến hoàn toàn về mặt đại số.
- **Bản chất Vật lý:** FDI không phá vỡ đa tạp khả dĩ (manifold). Tín hiệu $a$ là một véc-tơ trượt hoàn toàn dọc theo không gian tiếp tuyến của đa tạp. Đây là sự "Ký sinh đẳng cấu" (Isomorphic Parasitism), sao chép cấu trúc toán học của hệ thống (ma trận $H$) để tạo ra một bóng ma tuyến tính.

## 2. Tương tác Cục bộ (Local Interaction View)
- **Người đảm nhiệm (Assignee):** grid_interaction_researcher
- **Hệ tham chiếu (Container):** Inside_Attack (Mạng liên lạc/dữ liệu bên trong)
- **Hàm truyền nội tại (H(s)):** Quá trình xử lý tín hiệu hợp lệ của Inside_Attack, mang tín hiệu $Y_{attack}(s) = H(s) \cdot [X(s) + A(s)]$.
- **Tín hiệu In/Out:** Vector $A(s)$ chịu sự chi phối trực tiếp của đặc tính băng thông và bộ lọc mạng nội bộ. FDI phải ép thiết kế sao cho dư lượng $r(s) = || Z_{out}(s) - G(s) \cdot X_{attack}(s) || \leq \tau$ (ngưỡng cảnh báo BDD cục bộ).
- **Động lực học vòng lặp phản hồi:** Kẻ tấn công phải liên tục thay đổi $A(s)$ theo thời gian thực (Time-varying Attack) để bù trừ lại hàm phản hồi sửa lỗi $F(s)$ của mạng truyền thông cục bộ. Nếu thất bại, gói tin sẽ bị cô lập (Dropped/Timeout) ngay từ mức mạng.

## 3. Điểm mạnh, Điểm yếu & Ứng dụng (Pros, Cons & Use Cases)
- **Điểm mạnh (Strengths):** Khả năng ngụy trang (Stealth) tuyệt đối trước các bộ lọc nhiễu dựa trên chuẩn hình học (norm-based) truyền thống.
- **Điểm yếu (Weaknesses):** Yêu cầu thông tin cấu trúc hình học $H$ gần như hoàn hảo. Phải thao túng đồng bộ (coordination) nhiều điểm đo lường cùng lúc.
- **Khi nào dùng (When to use):** Thao túng thị trường điện, đánh lừa State Estimator để EMS ra quyết định cắt tải sai lầm.
- **Khi nào KHÔNG dùng (When NOT to use):** Khi hệ thống có kiến trúc Zero-Trust cao, hoặc khi hacker không có dữ liệu tô-pô lưới (Black-box).