---
contributors: 
  - Khanh k67
  - Thanh K67
---
Direct Parent Connection: -> [[Grid_Intelligence]]

# Lập lịch và Điều độ Hệ thống Điện (Power System Scheduling & Control)
*(Trạm trung chuyển quản lý dòng thời gian của hệ thống điện, đóng vai trò là "Bộ Não" phân bổ nguồn lực dựa trên ràng buộc của Lưới điện vật lý).*

## 1. Bản chất & Tính chất Chung (Common Properties)
- **Giao diện Không - Thời gian (Space-Time Interface):** Đóng vai trò là cầu nối giữa các phương trình kinh tế vĩ mô (trục thời gian dài) và các giới hạn nhiệt/điện áp/tần số của lưới điện vật lý `Power_Grid_System` (trục thời gian siêu ngắn).
- **Nguyên lý Top-Down (Phân rã Kế hoạch):** Quá trình ra quyết định tuân theo dòng chảy thời gian. Kế hoạch dài hạn (Tertiary) ấn định biên độ đầu tư; Thị trường Ngày tới (Day-Ahead) chốt trạng thái đóng/cắt (Unit Commitment); Điều độ ngắn hạn (Secondary) tinh chỉnh công suất (OPF/Base-points); và Điều khiển Sơ cấp (Primary) hấp thụ biến động vật lý tức thời.
- **Nguyên lý Bottom-Up (Khắc phục Sai số):** Các biến động ngẫu nhiên hoặc sự cố từ lưới điện sẽ được Primary Control đỡ nhịp đầu tiên, sau đó đẩy sai số tĩnh lên Secondary Control để dọn dẹp bằng dự phòng (Reserves) đã được Day-Ahead và Tertiary tính toán từ trước.
- **Co-optimization (Đồng Tối Ưu Hóa):** Bản chất sâu xa của Nút này là tìm cách phá vỡ ranh giới truyền thống (tuần tự) để giải đồng thời các lớp (Ví dụ: Tối ưu Planning + Day-Ahead + Secondary trong cùng một mô hình MILP) để giảm thiểu tổng chi phí vòng đời (CAPEX + OPEX) mà vẫn bảo toàn an ninh năng lượng.

## 2. Sơ đồ Phân loại (Taxonomy / Splitting)
*(Kiến trúc phân tầng thời gian khép kín, được tách bạch rõ ranh giới để giải quyết bài toán Đa khung)*
- **[Capacity Planning](obsidian://open?file=Capacity_Planning):** Quy hoạch Dài hạn (Tháng/Năm) - Gốc rễ của quy hoạch và mở rộng hệ thống.
- **[Day Ahead Market](obsidian://open?file=Day_Ahead_Market):** Thị trường Ngày tới (24h) - Lập lịch, Unit Commitment (UC) và định giá thanh toán.
- **[Real Time Balancing](obsidian://open?file=Real_Time_Balancing):** Vận hành & Cân bằng Thời gian Thực (Ms đến Giờ) - Bao gồm Tertiary, Secondary, và Primary Control để dập tắt sai số.