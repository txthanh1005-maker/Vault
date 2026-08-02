---
weighttree: 0
contributors: 
  - Thanh K67
---
Direct Parent Connection: -> [[P2P_Energy_Trading]]

# MESH P2P (MẠNG LƯỚI GIAO DỊCH NĂNG LƯỢNG)
Mesh P2P là cấu trúc mạng giao dịch năng lượng đa liên kết (nhiều đường dẫn song song), nơi các nút tham gia (Prosumers) được kết nối đan chéo với nhau. Cấu trúc này tối ưu hóa độ đàn hồi (resilience) và bảo mật nhưng đặt ra thách thức lớn về định tuyến công suất và điều khiển truyền tin.

## 1. Đặc tính Nội tại (Pure Entity View)
- **Người đảm nhiệm (Assignee):** pure_entity_researcher
- **Cấu trúc Đồ thị (Meshed Graph Topology):** Đồ thị vô hướng/có hướng $G=(V, E)$. Ở mức cực đoan là đồ thị đầy đủ (Complete Graph) với $N(N-1)/2$ cạnh, hoặc dạng đa liên kết (Sparse Mesh) chứa nhiều chu trình (cycles).
- **Ma trận Không gian:**
  - *Ma trận kề (Adjacency Matrix $A$):* Xác định liên kết không gian.
  - *Ma trận Laplacian ($L = D - A$):* Đặc trưng cho sự khuếch tán. Giá trị riêng $\lambda_2$ (Algebraic connectivity / Fiedler value) quyết định tốc độ hội tụ toán học và độ bền vững trước sự phân mảnh.
- **Đặc tính Vật lý & Toán học (Kirchhoff's Flow):**
  - *Định luật KVL & Dòng vòng (Loop Flows):* Do tồn tại các chu trình, tổng sụt áp trên một vòng kín bằng 0. Năng lượng giao dịch sẽ phân tán qua mọi nhánh kề, tạo thành dòng công suất song song (parallel flows) tỷ lệ nghịch với trở kháng.
  - *Khuếch tán PTDF:* Luồng công suất bị định hướng toán học qua Power Transfer Distribution Factors (dựa trên nghịch đảo giả của Laplacian $L^{\dagger}$). Mọi giao dịch P2P đơn lẻ đều phụ thuộc vào toàn cục cấu trúc Mesh.

## 2. Tương tác Cục bộ (Local Interaction View)
- **Người đảm nhiệm (Assignee):** grid_interaction_researcher
- **Hệ tham chiếu (Container):** Lưới Điện Phân Phối / Hệ thống cáp truyền tải cục bộ.
- **Vai trò Tương tác:** Mesh P2P là "Hộp đen Điều phối Động". Nó nhận tín hiệu trạng thái tuyến cáp và xuất ra vector định tuyến công suất nhằm duy trì cân bằng và tránh đứt gãy.
- **Luồng Giao tiếp Cục bộ (In/Out Signals):**
  - *Input:* Dung lượng $C_{max}$ của cáp, tín hiệu đứt cáp (topology changes), giới hạn nhiệt.
  - *Output:* Tín hiệu định tuyến đa đường (multipath routing), các gói tin trạng thái mạng ngang hàng.
- **Hàm truyền tương tác (Transfer Function & Dynamics):**
  - *Độ Đàn hồi (Resilience):* Khi một nhánh bị đứt ($C_{max}=0$), Mesh P2P loại bỏ nhánh lỗi và chuyển hướng dòng chảy bao quanh sự cố, giúp Lưới Phân Phối tự phục hồi (Self-healing).
  - *Tránh Nghẽn mạch (Congestion Avoidance):* Phân tán luồng năng lượng ra nhiều đường vòng tỷ lệ nghịch với mức độ tắc nghẽn hiện thời, loại bỏ triệt để nút thắt cổ chai (bottleneck) cục bộ.
  - *Ngập lụt Truyền tin (Data Flooding):* Các nút Mesh P2P có xu hướng quảng bá dữ liệu đa hướng. Nếu tần suất phát Output không được kiểm soát biên (TTL, băng thông rảnh rỗi), nó sẽ làm bão hòa hệ thống viễn thông của Container.