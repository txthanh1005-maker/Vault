# Power Grid System (Hub / Central Node)

**Mô tả:** 
Đây là Gốc tọa độ của toàn bộ hệ sinh thái nghiên cứu lưới điện. Nơi đây đóng vai trò là "Ma trận Không - Thời gian" (Spacetime Fabric) chứa đựng và chi phối mọi thực thể bên trong nó.

---
## 1. TRỤC KHÔNG GIAN: CẤU TRÚC 5 THỰC THỂ LÕI (HYDRA ENTITIES)
Hệ thống điện được phân rã thành 5 nút Thực thể Lớp 1 (L1). Mọi nghiên cứu sâu hơn về máy móc, thuật toán, dây dẫn đều phải kế thừa trực tiếp từ 1 trong 5 nút này:
- [[Source_of_System]] (Nguồn phát - Cơ bắp của hệ thống)
- [[Storage]] (Lưu trữ - Trái tim tuần hoàn)
- [[Load]] (Phụ tải - Điểm tiêu thụ)
- [[Communication]] (Viễn thông & Dữ liệu - Hệ thần kinh)
- [[Algorithm]] (Thuật toán - Bộ não điều phối)

---
## 2. TRỤC THỜI GIAN: CHIỀU ĐIỀU KHIỂN & VẬN HÀNH (TIME-SCALE DIMENSIONS)
Mọi bài toán vận hành trên 5 nút Không gian bên trên (dù là sạc pin BESS, lập lịch máy phát, hay chạy thuật toán tái cấu trúc) đều bắt buộc phải "Gióng ngang" sang Trục Thời Gian này. Trục này được bóc tách thành 3 Node Bao trùm (Overarching Nodes):

- **[[Layer_Primary_Control]]** *(Tầng Vật lý Tức thời)*: Giao điểm của các bài toán diễn ra trong chớp mắt (Micro-giây tới vài giây). Nơi các định luật vật lý như Quán tính, Dao động góc Rotor, và Droop Control ngự trị.
- **[[Layer_Secondary_Control]]** *(Tầng Điều độ Ngắn hạn)*: Giao điểm của các bài toán diễn ra từ vài giây đến 1 giờ. Nơi Toán học Tối ưu hóa (OPF, AGC) can thiệp để triệt tiêu sai số và điều tiết trào lưu công suất thời gian thực.
- **[[Layer_Tertiary_Control]]** *(Tầng Lập lịch Dài hạn)*: Giao điểm của các bài toán vĩ mô từ 24h trở lên. Nơi thị trường điện (Market Bidding) và Bài toán tổ hợp máy phát (Unit Commitment) thống trị.

*(Zettelkasten Note: Khi bạn viết 1 bài báo nghiên cứu, hãy liên kết (Link) nó tới ít nhất 1 Nút Không gian và 1 Nút Thời gian. Sự giao thoa của 2 Link này sẽ định vị chính xác vị trí bài báo của bạn trên toàn bộ Bản đồ Lưới điện).*