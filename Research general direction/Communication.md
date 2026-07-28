# Communication (L1)

**Direct Parent Connection:** -> [[Power_Grid_System]]

**Mô tả kết nối hướng tâm:** 
Nếu Source (Nguồn) là cơ bắp, Storage (Lưu trữ) là trái tim, thì Communication (Viễn thông & Dữ liệu) chính là "Hệ thần kinh" của Lưới điện Hiện đại. Nút này được phân tách làm hai góc nhìn hoàn toàn độc lập: Ứng dụng điều độ lưới điện (Phần A) và Ranh giới vật lý/toán học của kỹ thuật truyền tin (Phần B).

---
## ươm mầm Lớp Level 2 (Bản đồ Phân loại & Đặc tính Viễn thông Lưới điện)

*(Theo quy tắc Hydra: Khi khối lượng nghiên cứu nội tại $W > W_{max}$, các đề mục dưới đây sẽ tự động bóc tách thành các Node file `.md` độc lập có chứa link hướng tâm `[[Communication]]`)*

### PHẦN A: THEO GÓC NHÌN LƯỚI ĐIỆN & TƯƠNG TÁC (GRID-LEVEL INTERACTION)
*Xem hạ tầng viễn thông như "Hệ thống thần kinh" để truyền lệnh cho lưới điện, không bàn đến phương thức mã hóa tín hiệu cấp thấp.*

**1. Phân loại Vai trò Lưới điện (Grid Role Classification):**
- *Đo lường Diện rộng (WAMS) qua SCADA/PMU:* Thu thập điện áp, dòng điện, góc pha đồng bộ thời gian thực để trung tâm điều độ (Control Center) có cái nhìn toàn cảnh về lưới.
- *Điều khiển Bảo vệ Rơ-le (Relay Protection):* Truyền tin (GOOSE/SV tiêu chuẩn IEC 61850) giữa các rơ-le để cô lập sự cố cắt dòng trong thời gian tính bằng mili-giây.
- *An ninh mạng (Cybersecurity):* Đảm bảo tính vẹn toàn cho các tín hiệu Điều khiển tự động phát điện (AGC) và dập tắt các lệnh điều khiển mã độc nhắm vào máy cắt.

**2. Đặc tính Tương tác Lưới (Interaction Characteristics):**
- *Hiệu ứng Độ trễ tới Ổn định (Latency Impact on Stability):* Hạ tầng viễn thông chèn độ trễ (delay) vào các vòng điều khiển nhạy cảm (Primary Frequency Control, PSS). Trễ quá hạn mức tới hạn sẽ làm âm biên dự trữ pha (phase margin), biến hệ thống điều khiển thành máy phát dao động và làm mất ổn định góc Rotor.
- *Méo mó State Estimation do Lỗi Dữ liệu:* Nhiễu viễn thông hoặc Tấn công tiêm dữ liệu sai (FDIA) làm lệch ma trận quan sát, khiến bài toán Đánh giá trạng thái (State Estimator) giải sai, dẫn tới điều độ sai lệch dòng công suất tối ưu (OPF).
- *Sụp đổ VPP do rớt mạng (ICT Failure):* Rớt mạng diện rộng tại các Node phân tán khiến Nhà máy điện ảo (VPP) mất ngay khả năng đáp ứng công suất (Spinning Reserve/Peak Shaving) đã cam kết trên thị trường điện.

---

### PHẦN B: THEO GÓC NHÌN VẬT LÝ & DỮ LIỆU NỘI TẠI (PURE ENTITY DYNAMICS)
*Phá vỡ Hộp đen, đi sâu vào cấu trúc vật lý sóng vô tuyến, quang học và xử lý tín hiệu.*

**3. Phân loại Cấu trúc Mạng & Giao thức Nội bộ (Topologies & Protocols):**
- *Môi trường truyền dẫn (Physical Medium):* Cáp đồng (bị giới hạn bởi trở kháng và hiệu ứng bề mặt), Cáp quang (bị giới hạn bởi tán sắc ánh sáng), Không dây RF (bị giới hạn bởi tổn hao suy giảm tự do không gian).
- *Cấu trúc liên kết vật lý (Network Topologies):* Mesh (đa đường định tuyến dự phòng), Star (tập trung, nghẽn bộ đệm trung tâm), Ring/Bus (truyền token giải quyết xung đột dòng).
- *Điều chế & Mã hóa Lớp dưới (Modulation & MAC):* Ghép kênh (TDMA, FDMA, CSMA/CD), Điều chế QAM/OFDM dồn kênh, mã hóa đường dây Manchester/NRZ.

**4. Đặc tính Vật lý & Dung lượng (Physical & Data Characteristics):**
- *Giới hạn Băng thông & Dung lượng:* Chịu sự chi phối tuyệt đối của Định lý Shannon-Hartley: $C = B \log_2(1 + SNR)$.
- *Phân rã Độ trễ (Latency Breakdown):* Trễ lan truyền (Propagation) do chiết suất môi trường vật lý, Trễ xử lý (Processing) do xung nhịp chip mạch (SRAM/DRAM), và Biến động trễ (Jitter) do luồng gói tin ngẫu nhiên trong hàng đợi (Queuing).
- *Tổn hao Tín hiệu (Attenuation & Interference):* Suy hao biên độ (hấp thụ quang/tỏa nhiệt điện), giao thoa đa đường tự triệt tiêu (Fading/Multipath), và Nhiễu xuyên âm (Crosstalk) giữa các lõi cáp từ trường kề nhau.
- *Giới hạn Lỗi Bit (Bit Error Rate - BER):* Xảy ra ở cấp độ nguyên tử do nhiễu nhiệt (Johnson-Nyquist). Mạch vật lý phải bù đắp bằng các đa thức khôi phục Forward Error Correction (FEC) hoặc CRC.
