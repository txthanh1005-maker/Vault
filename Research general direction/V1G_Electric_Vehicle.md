---
weighttree: 1
contributors: 
  - Khanh k67
---
Direct Parent Connection: -> [[Electric_Vehicle]]

# V1G ELECTRIC VEHICLE (XE ĐIỆN SẠC MỘT CHIỀU)
*(Thực thể xe điện chỉ có khả năng sạc (G2V - Grid to Vehicle), đóng vai trò là phụ tải thuần túy nhưng có thể điều chỉnh thời gian và công suất sạc - Smart Charging).*

## 1. Đặc tính Nội tại (Pure Entity View)
*(Phần Âm: Không liên quan đến môi trường bên ngoài)*
- **Người đảm nhiệm (Assignee):** pure_entity_researcher
- **Cấu trúc Phần cứng:** Tối ưu hóa cho luồng một chiều. Bộ sạc tích hợp (OBC) sử dụng cấu trúc Boost PFC một chiều tiêu chuẩn và tầng DC-DC cách ly (mạch cộng hưởng LLC hoặc Phase-Shifted Full-Bridge). Giao tiếp truyền thông cơ bản (Analog PWM). Mạch PDU bất đối xứng chỉ bảo vệ dòng điện đi vào pin.
- **Phản ứng Hóa học:** Dịch chuyển ion Lithium đơn hướng từ Cathode sang Anode trong suốt quá trình sạc, chu kỳ diễn ra liên tục và ổn định.
- **Nhiệt động lực học:** Nhiệt sinh ra (tổn hao $I^2R$) theo hàm tuyến tính/mũ. Hệ thống làm mát bơm tuần hoàn duy trì vận hành ổn định.
- **Hao mòn Vật lý (Degradation):** Lão hóa hóa học và theo chu kỳ chỉ chi phối bởi chu trình lái xe (Driving cycle). Lớp màng SEI phát triển chậm, ổn định do không bị ép thực hiện sạc/xả liên tục (Micro-cycling), giúp bảo toàn tuổi thọ vật lý của cell pin.

## 2. Tương tác Cục bộ (Local Interaction View)
*(Phần Dương: Chỉ phân tích tương tác với HỆ THỐNG CẤP CAO HƠN BỌC TRỰC TIẾP LẤY NÓ)*
- **Người đảm nhiệm (Assignee):** grid_interaction_researcher
- **Hệ tham chiếu (Container):** [[Electric_Vehicle]] (Đóng vai trò Trạm sạc cục bộ / Điểm kết nối tải).
- **Vai trò Tương tác:** Unidirectional Controllable Load (Tải có khả năng điều khiển một chiều). Về bản chất, hình thành cơ chế dịch chuyển tải (load shifting) nội bộ mà không làm đảo chiều dòng điện.
- **Luồng Giao tiếp & Tín hiệu:** Tương tác vật lý một chiều (Container -> Node), tín hiệu hai chiều.
  - Inputs (từ Container): `P_charge_limit_t` (Giới hạn sạc tối đa), `Charge_Status_Cmd`.
  - Outputs (tới Container): `Current_SoC`, `Connection_Flag`, `P_actual`.
- **Hàm truyền & Giới hạn:** Biến thiên năng lượng bị kẹp chặt trong dải $[0, P_{max\_charge}]$. Tuyệt đối không có luồng năng lượng âm (reverse flow) chạy ngược về Container. Khả năng đáp ứng (Responsiveness) theo hàm step có độ trễ ngắn khi Container thay đổi `P_charge_limit_t`.

## 3. Điểm mạnh, Điểm yếu & Ứng dụng (Pros, Cons & Use Cases)
- **Điểm mạnh (Strengths):** Bảo vệ tuyệt đối tuổi thọ pin, dễ dàng triển khai do phần cứng sạc 1 chiều rẻ và phổ biến. Đơn giản trong tối ưu hóa.
- **Điểm yếu (Weaknesses):** Không thể cung cấp năng lượng ngược lại lưới khi lưới xảy ra sự cố (Blackout) hoặc cao điểm cực hạn.
- **Khi nào dùng (When to use):** Ứng dụng trong Demand Response dựa trên giá (Price-based DR), sạc chậm tại nhà hoặc nơi làm việc.
- **Khi nào KHÔNG dùng (When NOT to use):** Không dùng trong các dịch vụ phụ trợ như ổn định tần số sơ cấp vốn đòi hỏi nguồn bơm ngược.