---
contributors: 
  - Khanh k67
  - Thanh K67
---
Direct Parent Connection: -> [[Real_Time_Balancing]]

# LAYER: SECONDARY CONTROL (TẦNG ĐIỀU ĐỘ NGẮN HẠN / THỜI GIAN THỰC)
*(Trục Thời gian L2: Nút giao hòa giữa bài toán Tối ưu hóa Kinh tế và Lý thuyết Điều khiển tự động, giữ trọng trách triệt tiêu hoàn toàn sai số).*

## 1. ĐẶC TÍNH NỘI TẠI (PURE ENTITY VIEW)
*(Bản chất Toán học và Vật lý nội tại của AGC và OPF)*

### 1.1. Bản chất Vật lý Nội tại
- **Mục tiêu cốt lõi:** Khôi phục trạng thái cân bằng năng lượng động học về chuẩn danh định (50Hz/60Hz) sau khi hệ thống đã đi qua giai đoạn hãm suy giảm (Primary Control). 
- **Cơ chế can thiệp vật lý:** Tác động trực tiếp vào bộ điều tốc (Governor) của các tổ máy phát thông qua tín hiệu thay đổi setpoint công suất đầu vào. Bù đắp hoàn toàn năng lượng thiếu hụt tĩnh.
- **Thuộc tính thời gian (Time-scale):** Quá trình quá độ tuyến tính diễn ra từ 10 giây đến 15 phút.

### 1.2. Bản chất Toán học Nội tại
Sự kết hợp giữa **lý thuyết điều khiển phản hồi (Feedback Control)** và **tối ưu hóa phi tuyến có ràng buộc (Nonlinear Constrained Optimization)**.

#### A. Thuật toán tính toán ACE (Area Control Error)
Thước đo định lượng công suất mất cân bằng tại một vùng cục bộ:
$$ACE = \Delta P_{tie} + B \cdot \Delta f$$
- $\Delta P_{tie}$: Sai số công suất qua biên giới.
- $\Delta f$: Sai số tần số hệ thống.
- $B$: Hằng số khuynh hướng tần số (MW/Hz), là hồi tiếp âm kết hợp Droop của máy phát và cản của tải.

#### B. Thuật toán PI trong AGC (Automatic Generation Control)
AGC xuất lệnh bù công suất $\Delta P_c(t)$ bằng thuật toán Tỷ lệ - Tích phân:
$$\Delta P_c(t) = - \left( K_p \cdot ACE(t) + K_i \cdot \int_{0}^{t} ACE(\tau) d\tau \right)$$
- **Khâu Tích phân ($K_i$):** Là bản chất cốt lõi để đưa $f$ về đúng 50/60Hz. Toán học của tích phân ép **sai số xác lập (steady-state error) về tuyệt đối 0**. 

#### C. Bản chất thuật toán OPF (Optimal Power Flow) trong phân bổ
Phân bổ tổng $\Delta P_c$ cho các máy phát để cực tiểu hóa bề mặt chi phí:
$$Min \ C = \sum_{i=1}^{N} (a_i + b_i P_{Gi} + c_i P_{Gi}^2)$$
Tuân thủ đẳng thức Kirchhoff và các bất đẳng thức vật lý, thuật toán xuất ra **hệ số tham gia phân bổ (Participation Factors - $\alpha_i$)** để đảm bảo quá trình triệt tiêu sai số trượt trên viền giới hạn an toàn với tổng năng lượng tiêu tốn cực tiểu.

---

## 2. TƯƠNG TÁC CỤC BỘ (LOCAL INTERACTION VIEW)
*(Cách Layer Secondary đóng vai trò cơ chế chấp hành bị giam lỏng bên trong Container: `Real_Time_Balancing`).*

### 2.1. Phân tích Tín hiệu Đầu vào (Nhận từ Container / Cấp trên)
1. **Lịch chạy máy (Unit Commitment - UC):** Tín hiệu nhị phân (On/Off). Secondary Control chỉ được kích hoạt điều tần cho các tổ máy có trạng thái "On".
2. **Điểm đặt Cơ sở và Dải công suất Điều tần (Base-point & Regulation Capacity):** Nhận $P_{base}$ và dải biên độ $\pm \Delta P_{reg}$ từ DAM. Nó **bị cấm** điều chỉnh vượt quá biên độ $\pm \Delta P_{reg}$ để không phá vỡ giới hạn nhiệt và tính tối ưu kinh tế.

### 2.2. Phân tích Tín hiệu Đầu ra (Truyền xuống `Layer_Primary_Control`)
1. **Tạo Lệnh Bù trừ Sai số (AGC Set-point):** Tín hiệu liên tục (vài giây - 1 phút) cộng dồn vào $P_{base}$ thành điểm đặt mới $P_{ref} = P_{base} + \Delta P_{AGC, i}$.
2. **Truyền Tín hiệu Set-point xuống Bộ Điều tốc (Governor):** Primary Control bản chất là tỷ lệ (Droop), luôn để lại **sai số tĩnh (steady-state error)**. Tín hiệu Set-point từ Secondary đóng vai trò như một khâu tích phân chủ động thay đổi gốc tham chiếu của Primary, ép hệ thống kéo tần số về đúng giá trị danh định. 

**Tổng kết luồng:** Điều độ (Balancing) $\rightarrow$ Điều khiển Tối ưu hóa (AGC) $\rightarrow$ Lệnh thay đổi tham chiếu (Reference Change). Hoàn toàn cô lập khỏi động lực học cơ điện quay của Lưới điện vật lý.