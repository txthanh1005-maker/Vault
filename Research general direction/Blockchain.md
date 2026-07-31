---
contributors: 
  - D.M.Hai K67
---
Direct Parent Connection: -> [[Grid_Cyber_Security]]

# Blockchain (Công nghệ Chuỗi Khối trong Lưới Điện)
*Công nghệ sổ cái phân tán phi tập trung, đóng vai trò như một lớp áo giáp mã hóa bất biến để bảo vệ dữ liệu truyền tải, giám sát năng lượng và ngăn chặn các cuộc tấn công mạng (như FDI).*

*Implemented by pure_entity_researcher and grid_interaction_researcher*

## 1. Đặc tính Nội tại (Pure Entity View)
*(Phần Âm: Cấu trúc lõi của Blockchain)*
- **Cấu trúc Dữ liệu:** Sổ cái phân tán (Distributed Ledger), cấu trúc chuỗi khối với mỗi khối (Block) chứa mã băm (Hash) của khối trước đó, tạo thành chuỗi liên kết không thể phá vỡ.
- **Cơ chế Đồng thuận (Consensus Algorithms):** Proof of Work (PoW), Proof of Stake (PoS), hoặc Practical Byzantine Fault Tolerance (PBFT) giúp các nút mạng đạt được thỏa thuận mà không cần tổ chức trung gian.
- **Mật mã học (Cryptography):** Sử dụng mã hóa bất đối xứng (Public/Private Key) và hàm băm mật mã (SHA-256) để bảo mật thông tin.
- **Hợp đồng Thông minh (Smart Contracts):** Mã lập trình tự động thực thi các điều khoản khi thỏa mãn các điều kiện định trước (ví dụ: phát hiện dị thường FDI tự động từ chối bản ghi).

## 2. Tương tác Cục bộ (Local Interaction View)
*(Phần Dương: Tương tác với Hệ thống Điện & Phòng thủ không gian mạng)*
- **Hệ tham chiếu (Container):** SCADA/EMS, Microgrid, PMU Data Network.
- **Bảo mật Dữ liệu PMU/SCADA:** Mọi dữ liệu thu thập từ các cảm biến (Smart Meters, PMUs) được băm (hash) và lưu trữ định kỳ lên Blockchain. Nếu kẻ tấn công (FDI) thay đổi một con số trong trạm biến áp, mã băm sẽ thay đổi, hệ thống đồng thuận sẽ phát hiện và loại bỏ dữ liệu sai lệch ngay lập tức.
- **Ngăn chặn DoS và Single Point of Failure:** Blockchain có kiến trúc mạng lưới ngang hàng (P2P). Không có một máy chủ trung tâm duy nhất để đánh sập, giúp mạng điều khiển chịu lỗi cao hơn trước các cuộc tấn công DDoS hay Ransomware.
- **Quản lý Định danh (Identity Management):** Các thiết bị IED hoặc Inverter cấp cơ sở khi tham gia lưới sẽ được cấp định danh phi tập trung, ngăn chặn các nút giả mạo (Spoofing) gửi tín hiệu độc hại.

## 3. Điểm mạnh, Điểm yếu & Ứng dụng (Pros, Cons & Use Cases)
- **Điểm mạnh (Strengths):** Tính minh bạch, bất biến (Immutable), chống giả mạo tuyệt đối. Chịu lỗi Byzantine (Byzantine Fault Tolerance) xuất sắc.
- **Điểm yếu (Weaknesses):** Độ trễ giao dịch (Latency) cao do cần thời gian đồng thuận; tốn nhiều tài nguyên lưu trữ và băng thông truyền tải mạng.
- **Khi nào dùng (When to use):** 
  - Ghi nhật ký (Data Logging) dữ liệu điều hành không được phép sửa đổi.
  - Quản lý giao dịch mua bán điện ngang hàng (P2P Energy Trading).
  - Làm lớp khiên bảo mật thứ 2 (Trust Layer) để xác minh độ tin cậy của dữ liệu PMU.
- **Khi nào KHÔNG dùng (When NOT to use):** 
  - Các hệ thống điều khiển bảo vệ Rơ-le (Relay Protection) hoặc điều khiển PI/MPC yêu cầu độ trễ cực thấp (chuẩn thời gian thực tính bằng mili-giây) vì thuật toán đồng thuận của Blockchain quá chậm để đáp ứng.