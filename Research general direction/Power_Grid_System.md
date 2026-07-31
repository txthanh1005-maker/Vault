---
contributors:
  - Khanh k67
  - Thanh K67
color: red
---
# POWER GRID SYSTEM (Gốc Tọa Độ)
*(Gốc tọa độ của toàn bộ hệ sinh thái nghiên cứu lưới điện. Nơi đây đóng vai trò là "Ma trận Không - Thời gian" chứa đựng và chi phối mọi thực thể bên trong nó).*

## 1. Bản chất & Tính chất Chung (Common Properties)
- Là môi trường lớn nhất (Ultimate Container) bao bọc mọi thành phần phát, truyền tải, lưu trữ và tiêu thụ năng lượng.
- Bị chi phối tuyệt đối bởi định luật bảo toàn năng lượng (Cân bằng P - Q tức thời).
- Mọi Nút Thực Thể (Entity Nodes) khi tương tác vào môi trường này đều phải đồng bộ về Tần số (Frequency) và tuân thủ các dải Điện áp (Voltage) quy định.

## 2. Sơ đồ Phân loại (Taxonomy / Splitting)
Hệ thống điện được phân rã thành **Cấu trúc 5 Thực thể Không gian** (Hydra Entities) và **Trục Thời gian & Vận hành** (Time-Scale Scheduling). Mọi Nút con bên dưới sẽ móc link kế thừa hướng tâm về đây.

### 2.1. Phân loại theo Trục Không Gian (Physical Entities L1)
*(Mọi nghiên cứu sâu hơn về máy móc, thuật toán, dây dẫn đều phải trỏ link hướng tâm về 1 trong 5 nút này)*
- **[Source of System](obsidian://open?file=Source_of_System):** Nguồn phát - Cơ bắp của hệ thống (Máy phát đồng bộ, Năng lượng tái tạo).
- **[Storage](obsidian://open?file=Storage):** Lưu trữ - Trái tim tuần hoàn (BESS, H2, Siêu tụ...).
- **[Load](obsidian://open?file=Load):** Phụ tải - Điểm tiêu thụ năng lượng.
- **[Communication](obsidian://open?file=Communication):** Viễn thông & Dữ liệu - Hệ thần kinh truyền dẫn tín hiệu SCADA/IoT.
- **[Algorithm](obsidian://open?file=Algorithm):** Thuật toán - Bộ não điều phối, phân tích và tối ưu hóa hệ thống.

### 2.2. Phân loại theo Trục Thời Gian & Chế Độ Vận Hành (Time-Scale & Operation)
*(Thay vì phân tán thành các layer riêng lẻ, toàn bộ trục thời gian và chế độ vận hành nay được quản lý tập trung qua một Nút Đầu Não duy nhất)*
- **[Power System Scheduling](obsidian://open?file=Power_System_Scheduling):** Trung tâm Lập lịch và Chế độ Vận Hành đa khung thời gian (Bao bọc tất cả bài toán từ Quy hoạch dài hạn, Thị trường Ngày tới, Điều độ thời gian thực đến Điều khiển vật lý AGC).

*(Zettelkasten Note: Đã áp dụng luật Giấu Mắt Đồ Thị. Sử dụng liên kết ẩn `[Tên](obsidian://open?file=Tên)` tại Nút Phân Loại này để duy trì kiến trúc Bottom-Up thuần túy. Khi thiết lập Nút Kịch Bản Template 3, bắt buộc móc tới ít nhất 1 nhánh Không gian và nhánh Thời gian).*