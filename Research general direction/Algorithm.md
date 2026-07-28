# Algorithm & Math Models (L1)

**Direct Parent Connection:** -> [[Power_Grid_System]]

**Mô tả kết nối hướng tâm:** 
"Thuật toán và Mô hình Toán học" chính là BỘ NÃO điều phối toàn bộ các thành phần (Source, Load, Storage, Communication) của Lưới điện. Nút này được bóc tách làm hai mặt cắt độc lập: Một mặt nhìn thuật toán như công cụ thực thi cho Hệ thống Năng lượng, mặt kia soi xét nó dưới các định lý nghiêm ngặt của Toán học Giải tích và Khoa học Máy tính.

---
## ươm mầm Lớp Level 2 (Bản đồ Phân loại & Đặc tính Thuật toán)

*(Theo quy tắc Hydra: Khi khối lượng nghiên cứu nội tại $W > W_{max}$, các đề mục dưới đây sẽ tự động bóc tách thành các Node file `.md` độc lập có chứa link hướng tâm `[[Algorithm]]`)*

### PHẦN A: THEO GÓC NHÌN LƯỚI ĐIỆN & ỨNG DỤNG (GRID-LEVEL INTERACTION)
*Xem Thuật toán như một "Hộp đen" (Công cụ) để giải quyết các bài toán hệ thống điện, không xét đến độ phức tạp mã hóa bên trong.*

**1. Phân loại vai trò Lưới (Grid Role Classification):**
- *Unit Commitment (UC):* Bài toán tổ hợp máy phát (Day-ahead/Hour-ahead), xuất ra lịch trình đóng/cắt (On/Off) để đảm bảo quỹ quán tính dự trữ.
- *Optimal Power Flow (OPF):* Bài toán trào lưu công suất tối ưu (Real-time). Xuất ra lệnh bơm/rút P/Q cho từng nút, đảm bảo không vi phạm giới hạn nhiệt và điện áp.
- *State Estimation (Đánh giá trạng thái):* Lọc nhiễu dữ liệu SCADA/PMU, xuất ra "Bản sao kỹ thuật số" (Digital Twin) về điện áp và góc pha của toàn lưới.
- *Network Reconfiguration (Tái cấu trúc mạng lưới):* Tham gia tự phục hồi (Self-healing). Xác định chuỗi lệnh đóng/cắt Switch để khôi phục vùng mất điện hoặc giảm thiểu tổn thất truyền tải.

**2. Đặc tính Tương tác Lưới (Grid Interaction Characteristics):**
- *Áp lực Chu kỳ Điều độ (Execution time vs. Dispatch cycles):* Tại thị trường thời gian thực (chu kỳ 5 phút), thuật toán OPF hộp đen bắt buộc phải hội tụ < 2 phút. Nếu giải quá chậm, lưới điện phải dùng số liệu cũ, gây sụp đổ bảo vệ quá dòng.
- *Sự đánh đổi Phi tuyến tính (Non-linear Responsiveness):* Bức tranh lưới điện AC vốn dĩ phi tuyến. Tuy nhiên mô hình Hộp đen AC thường gặp tình trạng "Tắc nghẽn tính toán". Hệ thống buộc phải đánh đổi dùng mô hình DC (Tuyến tính hóa cực nhanh) nhưng phải trả giá bằng rủi ro bỏ lọt hiện tượng sụp áp cục bộ (Voltage Collapse).

---

### PHẦN B: THEO GÓC NHÌN TOÁN HỌC & KHOA HỌC MÁY TÍNH NỘI TẠI (PURE ENTITY DYNAMICS)
*Phá vỡ Hộp đen, bóc tách cấu trúc giải tích, độ phức tạp thuật toán và giới hạn của chính bài toán đó.*

**3. Phân loại Cấu trúc Thuật toán Lõi (Core Topology Classification):**
- *Exact Solvers (Giải chính xác):* Quy hoạch Tuyến tính (LP), Nguyên hỗn hợp (MILP), Quy hoạch Động (DP). Cơ chế lõi dựa trên Simplex, Interior Point, và phân nhánh (Branch & Bound).
- *Heuristics/Meta-heuristics (Xấp xỉ/Cảm tính):* Giải thuật Di truyền (GA), Đàn chim bay (PSO). Rà soát ngẫu nhiên có hướng (Stochastic search) thay vì duyệt toàn bộ không gian nghiệm.
- *Machine Learning / Data-Driven:* Học sâu (ANN), Học tăng cường (MDP/DRL). Dựa trên Gradient Descent và lan truyền ngược (Backpropagation) trên đa tạp không gian tham số.
- *Convex vs Non-convex (Lồi và Không lồi):* Không gian lồi đảm bảo cực tiểu địa phương là cực tiểu toàn cục qua điều kiện KKT. Không gian không lồi có địa hình nhấp nhô (rugged landscape) khiến ma trận Hessian vô cực.

**4. Đặc tính Toán học & KHKT Nội tại (Math & Computational Characteristics):**
- *Độ phức tạp (Time/Space Complexity):* Được định lượng bằng Big-O. Mô hình tuyến tính là $O(n^k)$. Nhưng khi chèn biến nguyên (Integer) như bài toán Switch/UC, bài toán lập tức nhảy lên mức NP-Hard/NP-Complete với độ phức tạp hàm mũ $O(2^n)$. Không gian bộ nhớ cực dễ tràn (OOM) ở quy hoạch động.
- *Tốc độ Hội tụ (Convergence Rate):* Sự hội tụ tuyến tính (Gradient Descent) vs Siêu tuyến tính (Newton-Raphson $O(n^3)$). Gặp rủi ro cực lớn về Triệt tiêu/Nổ đạo hàm (Vanishing/Exploding Gradient) ở các không gian phi tuyến lồi phức tạp.
- *Bùng nổ Tổ hợp (Combinatorial Explosion):* "Lời nguyền số chiều" (Curse of Dimensionality). Khi quy mô (số lượng nút mạng/switch) tăng lên, nhánh tính toán sinh ra vượt quá năng lực phần cứng, khiến các Exact Solvers như Branch & Bound rơi vào trạng thái lặp vô tận (không bao giờ tìm thấy điểm hội tụ).
