---
weighttree: 0
contributors: 
  - Khanh k67
---
Direct Parent Connection: -> [[Algorithm]]

# GAME THEORY (Lý Thuyết Trò Chơi)
*(Công cụ toán học chuyên mô hình hóa sự tương tác chiến lược giữa nhiều thực thể (agents) ra quyết định độc lập, nơi lợi ích của mỗi bên phụ thuộc vào hành động của các bên còn lại).*

## 1. Đặc tính Nội tại (Phần Âm - Nội sinh)
- **Cấu trúc Lõi:** Một trò chơi (Game) được định nghĩa bởi 3 yếu tố: Tập hợp Người chơi (Players/Agents), Không gian Chiến lược (Strategy/Action Space), và Hàm Lợi ích (Utility/Payoff Function).
- **Cơ chế Ra Quyết định:** Mỗi agent cố gắng tối đa hóa hàm mục tiêu của riêng mình (Self-interested), thay vì một hàm mục tiêu chung của hệ thống (Global Objective).
- **Trạng thái Cân bằng (Equilibrium):** Đích đến của toán học Game Theory là tìm ra "Điểm cân bằng Nash" (Nash Equilibrium) - trạng thái mà tại đó không một người chơi nào có động lực đơn phương thay đổi chiến lược vì làm vậy sẽ làm giảm lợi ích của chính họ.
- **Phân loại Nội sinh:** 
  - *Non-cooperative Game (Trò chơi phi hiệp tác):* Cạnh tranh khốc liệt, giấu thông tin (ví dụ: đấu giá thị trường điện).
  - *Cooperative / Coalitional Game (Trò chơi liên minh):* Liên minh chia sẻ lợi ích cốt lõi (ví dụ: các Microgrid liên kết để tối ưu chi phí rồi chia chác lợi nhuận theo giá trị Shapley).
  - *Stackelberg Game (Trò chơi phân cấp):* Có sự bất đối xứng về quyền lực (Leader-Follower), nơi Leader ra quyết định trước và Follower phản ứng lại.

## 2. Tương tác Cục bộ (Phần Dương - Giao tiếp với Container)
- **Container (Vỏ bọc):** Hệ sinh thái Đa Tác Nhân (Multi-Agent System), Thị trường Điện (Electricity Market) hoặc Mạng P2P.
- **Ranh giới Tương tác:** 
  - *Tín hiệu Nhận:* Trạng thái của môi trường hoặc hành động/giá chào (bidding price) từ các đối thủ cạnh tranh/đối tác.
  - *Tín hiệu Phát:* Chiến lược/Quyết định tối ưu cục bộ của bản thân agent (Ví dụ: Chào bán với giá bao nhiêu? Mua bao nhiêu kW?).
- **Cơ chế Giao tiếp:** Thay vì một bộ giải trung tâm (Centralized Solver) thu thập mọi dữ liệu để tối ưu hóa, Game Theory tương tác theo cơ chế "Phân tán / Cạnh tranh". Lưới điện lúc này không còn là một hệ thống kỹ thuật đơn thuần mà trở thành một môi trường kinh tế (Economic Environment).

## 3. Điểm mạnh, Điểm yếu và Ứng dụng
- **Điểm mạnh:** Mô hình hóa hoàn hảo bản chất "vị kỷ" của con người và tổ chức trong nền kinh tế thị trường tự do. Bảo vệ thông tin riêng tư (Privacy) vì các agent không cần phơi bày hàm chi phí gốc của mình cho bên thứ 3.
- **Điểm yếu:** Điểm cân bằng Nash thường kém hiệu quả hơn so với Tối ưu hóa Toàn cục tập trung (Social Optimum). Sự hao hụt này được giới học thuật gọi là "Cái giá của sự vô chính phủ" (Price of Anarchy). Rất khó chứng minh tính tồn tại (Existence) và tính duy nhất (Uniqueness) của nghiệm nếu hàm lợi ích không lồi.
- **Ứng dụng:**
  - Trò chơi Stackelberg để thiết kế Cấu trúc Định giá động (Dynamic Pricing) giữa Lưới điện/Trạm sạc (Leader) và Xe điện (Follower).
  - Cạnh tranh đấu thầu trong Thị trường điện bán buôn/bán lẻ.
  - Thương mại năng lượng ngang hàng (P2P Energy Trading) giữa các Prosumer.