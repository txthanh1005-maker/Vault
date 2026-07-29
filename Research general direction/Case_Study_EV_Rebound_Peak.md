---
contributors: [Khanh k67]
color: red
related: 
  - "[[Price_Responsive_EV]]"
  - "[[Price_Based_DR]]"
---
# Case_Study_EV_Rebound_Peak
*(Nút giao thoa: Sự cộng hưởng hành vi săn giá rẻ của quần thể EV tạo ra Rebound Peak, dẫn đến xung đột vật lý giữa tối ưu lưới điện và giới hạn lão hóa của ắc quy).*

## 1. Các Nút Nền Tảng (Cross-Linked Nodes)
- **Thực thể A:** [[Price_Responsive_EV]]
- **Thực thể B:** [[Price_Based_DR]]

## 2. Điểm Giao Thoa & Cơ Chế Cắt Ngang (Intersection Mechanism)
- **Cơ chế hình thành Rebound Peak thảm họa (Cross-elasticity Effect):** Khi tín hiệu giá từ `Price_Based_DR` chạm đáy ở khung giờ thấp điểm, nó vô tình đồng bộ hóa hành vi "săn giá rẻ" của toàn bộ quần thể `Price_Responsive_EV`. Sự dịch chuyển nhu cầu đồng loạt này tạo ra một dòng sạc nhồi ồ ạt, biến thung lũng phụ tải thành một Đỉnh thứ cấp (Rebound Peak) khổng lồ, làm quá tải máy biến áp phân phối và đe dọa an ninh lưới điện.
- **Xung đột đàn hồi và "Mỏi đàn hồi" (Elasticity Fatigue vs Peak-Shaving):** Lưới điện luôn kỳ vọng EV sẽ linh hoạt phản ứng để cắt đỉnh. Tuy nhiên, sự co giãn của EV bị giới hạn vật lý cứng bởi SoC. Khi SoC đạt bão hòa do đã sạc nhồi ở khung giờ giá rẻ, EV mất hoàn toàn khả năng đáp ứng với các tín hiệu DR tiếp theo. Sự bất đối xứng này làm phá vỡ ma trận độ đàn hồi truyền thống.
- **Sự đánh đổi khốc liệt giữa Lưới và Pin (Grid Rebound vs BMS Degradation):** Để tận dụng tối đa chênh lệch giá, các EV có xu hướng sạc với dòng lớn (C-rate cao) trong thời gian ngắn. Tuy nhiên, hành vi này đẩy nhanh tốc độ lão hóa pin (Degradation). Lợi ích kinh tế thu được từ giá điện DR thấp có nguy cơ bị triệt tiêu hoàn toàn bởi chi phí suy giảm tuổi thọ của hệ thống quản lý pin (BMS).

## 3. Khoảng Trống Nghiên Cứu (Research Gap & Novelty)
- **Khoảng trống hiện tại:** Phần lớn các nghiên cứu hiện tại giả định độ đàn hồi của EV là hằng số hoặc tuyến tính và tách rời bài toán tối ưu DR (của Lưới) khỏi mô hình hóa vật lý lão hóa pin (của BMS). Việc bỏ qua độ bão hòa SoC và sự phi tuyến của Degradation Cost khiến các cơ chế định giá DR cổ điển liên tục gây ra Rebound Peak.
- **Tính mới (Novelty) cho bài báo Q1:**
  1. **Thuật toán định giá phân tán dập tắt Rebound Peak (Distributed Anti-Rebound Pricing Algorithm):** Đề xuất cơ chế định giá động, cá nhân hóa (Non-homogeneous Pricing) dựa trên trạng thái SoC thực thời và độ mỏi đàn hồi của từng cụm EV, nhằm phân rã đám đông săn giá và triệt tiêu Đỉnh thứ cấp.
  2. **Mô hình toán học cân bằng Grid - BMS (Co-optimization Model):** Xây dựng một bài toán tối ưu (hoặc Stackelberg Game) đa mục tiêu, nơi hàm mục tiêu cân bằng trực tiếp giữa (1) Việc làm phẳng Rebound Peak của Grid và (2) Tối thiểu hóa phi tuyến Degradation Cost của BMS dưới tác động của dòng sạc nhồi (C-rate cao).

## 4. Tài liệu Tham chiếu (Literature)
- *(Cần cập nhật các nghiên cứu Q1 mới nhất về: Stackelberg game in EV charging, Rebound peak mitigation strategies, và Nonlinear battery degradation modeling in Demand Response).*