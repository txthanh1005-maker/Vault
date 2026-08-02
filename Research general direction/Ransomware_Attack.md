---
weighttree: 0
contributors: 
  - D.M.Hai K67
---
Direct Parent Connection: -> [[Outside_Attack]]

# Ransomware Attack
*(Cuộc tấn công cưỡng bức Entropy cục bộ thông qua các biến đổi đại số phi tuyến nhằm tước đoạt Tầm nhìn và Khả năng điều khiển của hệ thống điều hành)*

## 1. Đặc tính Nội tại (Pure Entity View)
- **Người đảm nhiệm (Assignee):** pure_entity_researcher
- **Bản chất Toán học:** Là một sự biến đổi toán học có tính khả nghịch có điều kiện. Sử dụng khối toán học đối xứng (AES trên trường Galois $GF(2^8)$) để sinh hỗn loạn (confusion/diffusion) và khối toán học bất đối xứng (RSA/ECC) để khóa chặt biến $k$.
- **Phương trình chi phối:** $C = E_k(P)$ và $C_k \equiv k^e \pmod{n}$. Việc khôi phục đòi hỏi giải bài toán phân tích thừa số nguyên tố hoặc logarit rời rạc - vượt quá giới hạn vật lý hiện tại.
- **Nhiệt động lực học & Thông tin:** Ransomware cưỡng bức cực đại hóa Entropy Shannon ($H(X) \to 8$ bits/byte). Chiều thời gian là không thuận nghịch trừ khi sở hữu "Quỷ Maxwell" (Khóa giải mã $d$) để giảm Entropy về lại trạng thái trật tự ban đầu.

## 2. Tương tác Cục bộ (Local Interaction View)
- **Người đảm nhiệm (Assignee):** grid_interaction_researcher
- **Hệ tham chiếu (Container):** Outside_Attack (Hệ thống IT văn phòng/Server)
- **Tín hiệu Đầu vào ($u$):** Tín hiệu IOPS ($u_1$) tiêm lệnh Read/Write liên tục, Tín hiệu cấp phát tài nguyên ($u_2$) ép CPU giải bài toán mã hóa, và Tín hiệu lan truyền mạng ($u_3$) quét cổng lân cận.
- **Tín hiệu Đầu ra ($y$):** Độ suy giảm dữ liệu ($y_1$) khi hệ điều hành mất quyền truy cập, Bão hòa tài nguyên Disk/CPU ($y_2$), và Tê liệt dịch vụ hoàn toàn ($y_3$).
- **Hàm truyền (Transfer Function):** Tương tác được mô hình hóa theo dạng Hàm truyền bậc nhất có trễ (FOPDT): $H(s) = \frac{K \cdot e^{-Ls}}{\tau s + 1}$. Với $L$ là Dwell time ngâm bẫy, $\tau$ là tốc độ sụp đổ. Nghịch lý: Server càng mạnh (IOPS cao, $\tau$ nhỏ) thì IT sụp đổ càng nhanh.

## 3. Điểm mạnh, Điểm yếu & Ứng dụng (Pros, Cons & Use Cases)
- **Điểm mạnh (Strengths):** Vũ khí hoàn hảo để phong tỏa (Denial of View/Denial of Control) mà không cần phải hiểu kiến trúc Lưới điện vật lý bên dưới. Toán học bảo vệ là tuyệt đối.
- **Điểm yếu (Weaknesses):** Bị khắc chế hoàn toàn nếu hệ thống IT áp dụng quy tắc Backup 3-2-1 với phân vùng lưu trữ ngoại tuyến (Air-gapped) hoặc Immutable Storage.
- **Khi nào dùng (When to use):** Tấn công vào hạ tầng IT thượng tầng, cắt đứt sự chỉ huy của Dispatcher, tạo môi trường "mù" để yểm trợ cho các cuộc tấn công vật lý bên dưới lưới điện.
- **Khi nào KHÔNG dùng (When NOT to use):** Khi mục tiêu là thao túng dữ liệu lén lút (FDI) để trục lợi thị trường hoặc thay đổi sóng điện áp tinh vi.