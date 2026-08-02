---
weighttree: 1
contributors:
  - H.T.Hải K67
---
Direct Parent Connection: -> [[Algorithm]]

# FAST FOURIER TRANSFORM (BIẾN ĐỔI FOURIER NHANH — FFT)
*(Nút Thực thể Lớp 2: Thuật toán phân tích phổ miền tần số chuyên trách cắt và phân rã tín hiệu dao động công suất thành 3 dải chuyên biệt trong Hệ thống Lưu trữ Năng lượng Hỗn hợp - HESS).*

---

## I. KHÂU ÂM (PURE ENTITY): BẢN CHẤT TOÁN HỌC & LÝ THUYẾT PHÂN GIẢI PHỔ THUẦN TÚY
*(Phân tích độc lập nội tại bởi `pure_entity_researcher.md` — Không tham chiếu môi trường ngoại vi).*

### 1. Biến đổi Fourier Rời rạc (Discrete Fourier Transform — DFT)
Cho một chuỗi tín hiệu rời rạc theo thời gian độ dài N điểm $x[0], x[1], \dots, x[N-1]$, phép biến đổi DFT chuyển đổi tín hiệu từ miền thời gian sang miền tần số:
$$X[k] = \sum_{n=0}^{N-1} x[n] e^{-j \frac{2\pi}{N} k n}, \quad k = 0, 1, \dots, N-1$$
Trong đó:
- $X[k]$: Số phức biểu thị biên độ và pha của thành phần tần số thứ $k$.
- Phép biến đổi ngược (Inverse DFT — IDFT) tái tạo tín hiệu miền thời gian:
  $$x[n] = \frac{1}{N} \sum_{k=0}^{N-1} X[k] e^{j \frac{2\pi}{N} k n}$$

### 2. Thuật toán Cooley-Tukey Radix-2 FFT & Độ phức tạp Tính toán
Việc tính trực tiếp DFT đòi hỏi độ phức tạp Big-O là $O(N^2)$ phép nhân phức, gây nghẽn cổ chai cho hệ thống điều khiển thời gian thực. Thuật toán **Cooley-Tukey Radix-2 FFT** áp dụng chiến lược Chia để Trị (Divide-and-Conquer):
- Tách chuỗi tín hiệu N điểm thành chuỗi chỉ số chẵn (Even indices) và chỉ số lẻ (Odd indices):
  $$X[k] = \text{DFT}_{\text{even}}[k] + W_N^k \cdot \text{DFT}_{\text{odd}}[k]$$
  với nhân số xoay (Twiddle Factor) $W_N^k = e^{-j \frac{2\pi}{N} k}$.
- Giảm đột phá số phép toán từ $O(N^2)$ xuống **$O(N \log_2 N)$**, giúp tốc độ thực thi tăng hàng nghìn lần khi N lớn.

### 3. Định lý Nyquist-Shannon & Bảo toàn Năng lượng Parseval
- **Định lý Lấy mẫu Nyquist-Shannon:** Tần số lấy mẫu $f_s$ bắt buộc phải thỏa mãn $f_s \ge 2 f_{\max}$ để tránh hiện tượng chồng phổ (Aliasing/Folding).
- **Định lý Parseval:** Tổng năng lượng của tín hiệu trong miền thời gian bằng đúng tổng năng lượng trong miền tần số:
  $$\sum_{n=0}^{N-1} |x[n]|^2 = \frac{1}{N} \sum_{k=0}^{N-1} |X[k]|^2$$
- *Xử lý Rò rỉ phổ (Spectral Leakage):* Sử dụng các hàm cửa sổ tín hiệu (Hanning, Hamming, Blackman Windows) trước khi thực thi FFT để triệt tiêu hiện tượng gián đoạn biên tại hai đầu chuỗi trượt.

---

## II. KHÂU DƯƠNG (LOCAL INTERACTION): CƠ CHẾ CẮT & PHÂN DẢI TẦN SỐ TRONG HỆ THỐNG HESS
*(Phân tích tương tác cục bộ bởi `grid_interaction_researcher.md` — Môi trường bọc: Hệ thống Điều khiển Lưu trữ Hỗn hợp HESS).*

### 1. Phân rã Phổ Công suất 3 Dải (3-Band Power Spectrum Decomposition)
Trong kiến trúc điều khiển HESS do đồng chí **D.M.Hai K67** phát triển, chuỗi tín hiệu sai lệch công suất tải/tái tạo $\Delta P_{req}[n]$ được đưa qua bộ FFT trượt trong cửa sổ thời gian thời gian thực.ổ phổ tần số $P(f)$ nhận được được cắt thành 3 dải trực giao dựa trên 2 tần số cắt $f_{c1}$ và $f_{c2}$ ($f_{c1} < f_{c2}$):
1. **Dải Tần số Thấp (Low-Frequency Band: $0 \le f \le f_{c1}$):**
   - Chứa năng lượng nền dài hạn (biến thiên theo giờ, ngày, mùa).
   - Thực hiện biến đổi ngược (IFFT) cho phổ $P_{low}(f)$ để tạo ra tín hiệu đặt công suất $P_{ref,H_2}(t)$ giao chuyên trách cho `[Hydrogen_Storage](obsidian://open?file=Hydrogen_Storage)`.
2. **Dải Tần số Trung bình (Medium-Frequency Band: $f_{c1} < f \le f_{c2}$):**
   - Chứa dao động tải trung hạn và nhiệm vụ điều tần AGC (biến thiên theo giây đến phút).
   - Thực hiện IFFT cho phổ $P_{med}(f)$ để tạo tín hiệu đặt công suất $P_{ref,BESS}(t)$ giao chuyên trách cho `[BESS](obsidian://open?file=BESS)`.
3. **Dải Tần số Cao (High-Frequency Band: $f > f_{c2}$):**
   - Chứa các xung đột biến công suất, đáp ứng quán tính ảo và RoCoF (biến thiên mili-giây đến giây).
   - Thực hiện IFFT cho phổ $P_{high}(f)$ để tạo tín hiệu đặt công suất $P_{ref,SC}(t)$ giao chuyên trách cho `[Supercapacitor](obsidian://open?file=Supercapacitor)`.

### 2. Ưu thế Vượt trội của FFT trong Điều phối 3 Công nghệ Storage
- **Độc lập Trực giao (Zero Cross-Band Interference):** Khác với bộ lọc thông thấp (LPF) bị chồng chéo biên độ tại vùng chuyển tiếp (Transition Band), FFT phân dải dứt khoát trong miền tần số, đảm bảo Siêu tụ, Pin BESS và Hydro không bị tranh chấp lệnh xả/sạc cùng lúc.
- **Không suy hao Pha (Zero Phase Distortion):** Biến đổi IFFT từ các dải phổ được chọn lọc giúp tái tạo tín hiệu đặt công suất mà không gây trễ pha thời gian như các bộ lọc miền thời gian bậc cao.

---

## III. GHI CHÚ THẢO LUẬN & CHUYÊN GIA
- **Đóng góp chuyên môn:** Đồng chí **D.M.Hai K67** chủ trì phát triển thuật toán phân rã phổ tần số FFT cho HESS.
- **Trạng thái tài liệu:** *(AI Agent dự thảo: `pure_entity_researcher.md` & `grid_interaction_researcher.md` — Chờ đồng chí D.M.Hai K67 hiệu chỉnh và thẩm định công thức)*.
