# POWER GRID SYSTEM (Gốc Tọa Độ)
*(Gốc tọa độ của toàn bộ hệ sinh thái nghiên cứu lưới điện. Nơi đây đóng vai trò là "Ma trận Không - Thời gian" chứa đựng và chi phối mọi thực thể bên trong nó).*

## 1. Bản chất & Tính chất Chung (Common Properties)
- Là môi trường lớn nhất (Ultimate Container) bao bọc mọi thành phần phát, truyền tải, lưu trữ và tiêu thụ năng lượng.
- Bị chi phối tuyệt đối bởi định luật bảo toàn năng lượng (Cân bằng P - Q tức thời).
- Mọi Nút Thực Thể (Entity Nodes) khi tương tác vào môi trường này đều phải đồng bộ về Tần số (Frequency) và tuân thủ các dải Điện áp (Voltage) quy định.

## 2. Sơ đồ Phân loại (Taxonomy / Splitting)
Hệ thống điện được phân rã thành **Cấu trúc 5 Thực thể Không gian** (Hydra Entities) và **3 Trục Thời gian Điều khiển**. Mọi Nút con bên dưới sẽ kế thừa từ các nhánh này.

### 2.1. Phân loại theo Trục Không Gian (Physical Entities L1)
*(Mọi nghiên cứu sâu hơn về máy móc, thuật toán, dây dẫn đều phải trỏ link hướng tâm về 1 trong 5 nút này)*
- **[[Source_of_System]]:** Nguồn phát - Cơ bắp của hệ thống (Máy phát đồng bộ, Năng lượng tái tạo).
- **[[Storage]]:** Lưu trữ - Trái tim tuần hoàn (BESS, H2, Siêu tụ...).
- **[[Load]]:** Phụ tải - Điểm tiêu thụ năng lượng.
- **[[Communication]]:** Viễn thông & Dữ liệu - Hệ thần kinh truyền dẫn tín hiệu SCADA/IoT.
- **[[Algorithm]]:** Thuật toán - Bộ não điều phối, phân tích và tối ưu hóa hệ thống.

### 2.2. Phân loại theo Trục Thời Gian (Time-Scale Control Layers)
*(Mọi bài toán vận hành trên 5 nút Không gian bên trên đều bắt buộc phải gióng ngang sang 1 trong 3 Trục Thời gian này)*
- **[[Layer_Primary_Control]]:** Tầng Vật lý Tức thời (Micro-giây tới vài giây). Giao điểm của Quán tính, Dao động góc Rotor và Droop Control.
- **[[Layer_Secondary_Control]]:** Tầng Điều độ Ngắn hạn (Vài giây đến 1 giờ). Giao điểm của Toán tối ưu hóa (OPF, AGC) nhằm triệt tiêu sai số.
- **[[Layer_Tertiary_Control]]:** Tầng Lập lịch Dài hạn (24h trở lên). Giao điểm của Thị trường điện (Market Bidding) và Bài toán tổ hợp máy phát (Unit Commitment).

*(Zettelkasten Note: Khi thiết lập một Nút Kịch Bản (Template 3), bắt buộc phải móc link tới ít nhất 1 Nút Không gian và 1 Nút Thời gian để định vị chính xác bài toán).*