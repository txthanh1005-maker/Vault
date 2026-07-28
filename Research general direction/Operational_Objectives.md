---
contributors: 
  - Thanh K67
---
Direct Parent Connection: -> [[Power_Grid_System]]

# MỤC TIÊU VẬN HÀNH (OPERATIONAL OBJECTIVES)
Mục tiêu vận hành là các tiêu chuẩn và triết lý tối cao chi phối hành vi của hệ thống điện. Đây là lớp định hướng (Goal-oriented Layer), đóng vai trò "Bên Đặt Hàng" (Client) thao túng toàn bộ lớp Thuật toán (Algorithm) bên dưới.

## 1. Phả hệ Nút Con (Child Nodes)
Các mục tiêu vận hành cụ thể được chia thành các nhánh sau:
- -> [[Economic_Optimization]] (Tối ưu Kinh tế)
- -> [[Reliability_Security]] (Độ tin cậy & An ninh lưới điện)
- -> [[Power_Quality]] (Chất lượng Điện năng)
- -> [[Environmental_Emission]] (Môi trường & Giảm phát thải)
- -> [[Resilience_Short_Term]] (Độ Đàn hồi - Trạng thái Khẩn cấp)

## 2. Bản chất Toán học (Pure Entity View)
- **Người đảm nhiệm (Assignee):** pure_entity_researcher
- Khối này chứa các hàm mục tiêu (Objective Functions) và không gian tìm kiếm, được phân loại như sau:
  - **Tối ưu Kinh tế (Economic/Cost Minimization):** Thường là hàm bậc hai lồi ($F_i(P_i) = a_i + b_i P_i + c_i P_i^2$). Nếu xét đến hiệu ứng điểm van (valve-point effect), hàm trở thành phi tuyến không lồi chứa thành phần hình sin.
  - **Độ tin cậy (Reliability & Security):** Đại diện qua tiêu chuẩn tất định **N-1 Contingency** (ép buộc bài toán phải có nghiệm khả thi khi rớt 1 phần tử) hoặc xác suất mất tải **LOLP** (Loss of Load Probability).
  - **Chất lượng điện năng (Power Quality):** Tối thiểu hóa Độ lệch điện áp (Voltage Deviation) dạng hàm lồi khoảng cách tuyệt đối.
- **Xung đột Đa mục tiêu (Pareto Front):** Không thể đạt cực tiểu của mọi hàm mục tiêu (vd: giảm sụt áp cần bơm Vô công, làm tăng tổn thất $I^2R$ và chi phí Kinh tế). Mặt Pareto đại diện cho ranh giới vật lý tuyệt đối - nơi không thể trích xuất thêm lợi ích cho mục tiêu này mà không phải trả giá bằng mục tiêu khác. Tính chất này biến bài toán thành Tối ưu hóa Đa mục tiêu (Multi-Objective Optimization).

## 3. Tương tác Cục bộ (Local Interaction View)
- **Người đảm nhiệm (Assignee):** grid_interaction_researcher
- **Hệ tham chiếu (Container):** Hệ thống điều độ Trung tâm (ISO/TSO) và Lớp Thuật toán bên dưới (UC, OPF).
- **Vai trò Tương tác:** Hoạt động như một "Bộ điều hướng Hàm mục tiêu" (Objective Navigator). Tiếp nhận áp lực vĩ mô và dịch thành hệ số thao túng không gian nghiệm của Container.
- **Luồng Tín hiệu In/Out:**
  - *Inputs:* Tín hiệu thị trường vĩ mô (giá spot), Tín hiệu pháp lý (Thuế Carbon, Hạn ngạch), Cờ trạng thái khẩn cấp (HILP).
  - *Outputs:* Trọng số Đa mục tiêu ($w_i$) phân rã mức độ ưu tiên; Hệ số Hàm phạt ($\lambda$) dội chi phí ảo vào thuật toán; Cờ đóng/mở biến đổi ràng buộc động (Dynamic Bounds).
- **Động lực học Thao túng Thuật toán (Solution Space Manipulation):** Khi thuế Carbon tăng (Input), khối Mục tiêu lập tức khuếch đại hệ số $\lambda_{carbon}$ (Output). Các Trình giải (Solvers) của lớp Thuật toán bên dưới sẽ bị "lực kéo ảo" này ép phải rời khỏi điểm cực tiểu chi phí nhiên liệu, trượt dọc theo Mặt Pareto để tìm điểm cân bằng mới. Khối Mục tiêu bẻ lái toàn bộ tư duy của mạng điện thông qua những cú shock hàm mục tiêu dạng bậc thang này.

## 4. Vai trò Kiến trúc trong Zettelkasten
Nút "Mục tiêu vận hành" tạo thế chân vạc cân bằng tuyệt đối:
1.  **Lớp Vật lý (Physical/Nguồn Tải):** Nơi chứa cấu trúc phần cứng.
2.  **Lớp Thuật toán (Algorithm):** Trí tuệ nhân tạo (MILP, ADMM, OPF).
3.  **Lớp Mục tiêu (Operational Objectives):** Bộ Não ra quyết định, quyết định Thuật toán phải giải bài toán gì (Quy hoạch tuyến tính, Đa mục tiêu hay Robust Optimization). Nút `[[Resilience_Short_Term]]` chính là một dạng Mục tiêu vận hành khẩn cấp.