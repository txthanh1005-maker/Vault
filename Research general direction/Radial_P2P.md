---
weighttree: 1
contributors: 
  - Thanh K67
---
Direct Parent Connection: -> [[P2P_Energy_Trading]]

# RADIAL P2P (MẠNG HÌNH TIA GIAO DỊCH NĂNG LƯỢNG)
Radial P2P là cấu trúc mạng giao dịch năng lượng hình tia, điển hình trong các Lưới điện Phân phối truyền thống. Khác biệt cốt lõi là cấu trúc này không có mạch vòng (acyclic), dẫn đến đường đi của luồng năng lượng là duy nhất nhưng đi kèm với nhiều điểm nghẽn vật lý (bottlenecks).

## 1. Đặc tính Nội tại (Pure Entity View)
- **Người đảm nhiệm (Assignee):** pure_entity_researcher
- **Nền tảng Đồ thị Dạng cây (Tree Graph Topology):** Đồ thị có hướng $\mathcal{G} = (\mathcal{N}, \mathcal{E})$ không có chu trình. Tồn tại một đường đi duy nhất giữa hai nút bất kỳ. Điều kiện giới hạn tuyệt đối: Số cạnh $|\mathcal{E}| = |\mathcal{N}| - 1$.
- **Đặc tính Ma trận Liên kết (Incidence Matrix $A$):**
  - *Tính thưa (Sparsity):* Ma trận cực kỳ thưa thớt (chỉ có một phần tử 1 và -1 mỗi cột).
  - *Hạng ma trận:* $\text{Rank}(A) = N - 1$, cho phép giải nghiệm tuyến tính và nghịch đảo một phần (pseudo-inverse) hiệu quả cao về mặt thời gian thuật toán.
- **Đặc tính Vật lý & Toán học (Branch Flow Model - DistFlow):**
  - *Phương trình trạng thái:* Dòng năng lượng vật lý được mô tả qua phương trình cân bằng công suất tác dụng ($P$), phản kháng ($Q$) và sụt áp dọc tuyến $v_j = v_i - 2(r_{ij}P_{ij} + x_{ij}Q_{ij}) + (r^2 + x^2)l_{ij}$.
  - *Nền tảng Lồi hóa (Convex Relaxation):* Tính phi tuyến của dòng nhánh $l_{ij} = (P_{ij}^2 + Q_{ij}^2)/v_i$ khiến bài toán NP-hard. Tuy nhiên, tính chất đồ thị cây (Radial) là điều kiện toán học bắt buộc để nới lỏng cấu trúc nón bậc hai (SOCP) với độ chênh lệch (relaxation gap) tiến về 0, đảm bảo nghiệm khả thi vật lý.

## 2. Tương tác Cục bộ (Local Interaction View)
- **Người đảm nhiệm (Assignee):** grid_interaction_researcher
- **Hệ tham chiếu (Container):** Lưới Điện Phân Phối Truyền Thống (Traditional Distribution Grid).
- **Vai trò Tương tác:** Radial P2P tạo ra các tín hiệu bơm/rút công suất dựa trên hợp đồng tài chính, còn Lưới Phân Phối làm môi trường ràng buộc vật lý, phản hồi lại sự thay đổi biến thiên điện áp.
- **Hàm truyền tương tác (Transfer Function & Dynamics):**
  - *Động lực học Thắt nút cổ chai (Bottleneck Dynamics):* Do đồ thị hình tia, luồng công suất không thể đi đường chim bay mà bắt buộc phải chảy qua điểm giao cắt (junction). Ngã rẽ trở thành điểm nghẽn duy nhất. Khi dòng giao dịch qua ngã rẽ đạt $I_{max}$, Lưới Phân Phối sẽ bão hòa cục bộ và từ chối các giao dịch xuyên nhánh dù tổng dung lượng mạng còn dư.
  - *Lan truyền Sụt áp (Voltage Drop Propagation):* Tín hiệu công suất P2P khuếch đại dòng năng lượng dọc theo tia. Node P2P bơm/rút năng lượng sẽ gửi xung dao động điện áp ($\pm \Delta V$) lên Container, lan truyền ngược dòng lên tận máy biến áp gốc, nguy cơ vi phạm điện áp của người dùng khác.
  - *Độ nhạy Bất đối xứng (Asymmetric Sensitivity):* Container phản ứng hoàn toàn khác nhau dựa trên tọa độ topology. Một giao dịch đặt gần gốc (Root) ít gây nhiễu, nhưng đặt ở cuối tia (Leaf) sẽ khuếch đại sụt áp cực mạnh ($\frac{\partial V}{\partial P}$ cao). Hệ thống P2P tài chính thường bị "mù" trước độ nhạy vật lý này.