# Kịch bản thuyết trình Đề án tốt nghiệp (15 phút)

**Đề tài:** Nghiên cứu mô hình học tăng cường đa tác tử trong bài toán điều khiển tín hiệu giao thông tại Việt Nam.

---

## 1. Slide tiêu đề (0:00 - 0:35)
**Lời nói:**  
Kính thưa các thầy cô trong Hội đồng, em tên là Nguyễn Thanh Hùng. Hôm nay em xin trình bày đề án tốt nghiệp thạc sĩ với đề tài **“Nghiên cứu mô hình học tăng cường đa tác tử trong bài toán điều khiển tín hiệu giao thông tại Việt Nam”** dưới sự hướng dẫn của TS. Nguyễn Đình Hoá.

---

## 2. Nội dung trình bày (0:35 - 1:00)
**Lời nói:**  
Bài trình bày của em gồm bốn phần chính: bối cảnh nghiên cứu và mục tiêu đề án, mô hình bài toán và các thuật toán được triển khai, thiết lập thực nghiệm và quy trình đánh giá, cuối cùng là kết quả, thảo luận và kết luận.

---

## 3. Bối cảnh và khoảng trống nghiên cứu (1:00 - 2:00)
**Lời nói:**  
Điểm xuất phát của đề án là hạn chế của điều khiển đèn tín hiệu chu kỳ cố định khi lưu lượng giao thông biến động theo thời gian. Trong bối cảnh Việt Nam, giao thông còn có đặc trưng hỗn hợp với tỷ lệ xe máy cao và hành vi di chuyển linh hoạt. Vì vậy, đề án tập trung vào bài toán điều khiển phối hợp nhiều nút giao và đánh giá các thuật toán MARL trên một kịch bản mô phỏng được xây dựng cho khu vực Thanh Xuân, Hà Nội.

---

## 4. Mục tiêu và đóng góp (2:00 - 2:50)
**Lời nói:**  
Mục tiêu của đề án là xây dựng và đánh giá một quy trình điều khiển tín hiệu giao thông đa tác tử trên kịch bản Thanh Xuân UTM. Đóng góp của đề án không nằm ở việc đề xuất một thuật toán mới, mà nằm ở việc xây dựng môi trường mô phỏng, đặc tả bài toán, triển khai bốn biến thể MARL và đánh giá chúng trong cùng điều kiện thực nghiệm.

---

## 5. Kịch bản và dữ liệu thực nghiệm (2:50 - 3:50)
**Lời nói:**  
Kịch bản thực nghiệm được xây dựng cho khu vực Thanh Xuân, Hà Nội. Mạng lưới đường được trích xuất từ OpenStreetMap và chuyển sang SUMO. Nhu cầu giao thông được xây dựng từ dữ liệu UTM-Hanoi và điều chỉnh về mức 5.000 phương tiện mỗi giờ. Sau khi phân tích chương trình đèn tín hiệu, môi trường xác định 41 bộ điều khiển có nhiều hơn một giai đoạn xanh hợp lệ và sử dụng các bộ điều khiển này làm tác tử học.

---

## 6. Mô hình bài toán trong môi trường SUMO (3:50 - 4:50)
**Lời nói:**  
Bài toán được mô hình hóa dưới dạng trò chơi Markov hợp tác. Sau mỗi 5 giây mô phỏng, môi trường đọc trạng thái giao thông từ SUMO, tạo quan sát cục bộ cho từng tác tử, áp dụng bộ lọc hành động, thực thi hành động và tính phần thưởng. Cơ chế lọc hành động bảo đảm tác tử không chọn các lệnh chuyển pha không hợp lệ, còn hàm phần thưởng hướng tới giảm các đại lượng bất lợi trong trạng thái giao thông.

---

## 7. Hàm phần thưởng và ràng buộc hành động (4:50 - 5:40)
**Lời nói:**  
Hàm phần thưởng không nhằm tối ưu một chỉ số đơn lẻ mà phạt các trạng thái bất lợi gồm áp lực cục bộ, hàng đợi dừng, thời gian chờ tăng thêm và dịch chuyển cưỡng bức. Về hành động, tác tử luôn có thể giữ pha hiện tại; yêu cầu chuyển pha chỉ hợp lệ khi pha yêu cầu là pha xanh kế tiếp và thời gian xanh tối thiểu đã được thỏa mãn. Cơ chế này bảo đảm chính sách không tạo lệnh chuyển pha vi phạm chu kỳ đèn.

---

## 8. Các phương pháp được so sánh (5:40 - 6:40)
**Lời nói:**  
Đề án so sánh sáu phương pháp. Hai phương pháp đối chứng là điều khiển cố định và Max-Pressure, trong đó Max-Pressure là một đối chứng thích ứng mạnh. Bốn phương pháp học tăng cường là MAPPO, HAPPO, GAT-MAPPO và GAT-HAPPO. Các phương pháp này khác nhau ở hai điểm chính: actor dùng chung hay actor riêng, và có hay không sử dụng bộ mã hóa đồ thị GAT.

---

## 9. Quy trình huấn luyện và đánh giá (6:40 - 7:40)
**Lời nói:**  
Mỗi mô hình được huấn luyện trong 500 lần mô phỏng. Mỗi lần mô phỏng kéo dài 3.600 giây; với bước quyết định 5 giây, mỗi lần mô phỏng gồm 720 bước quyết định. Như vậy, mỗi mô hình học tăng cường được huấn luyện trên 360.000 bước quyết định. Sau huấn luyện, bản lưu mô hình cuối cùng được dùng để đánh giá trên 10 hạt giống ngẫu nhiên.

---

## 10. Luồng huấn luyện và đánh giá (7:40 - 8:30)
**Lời nói:**  
Trong huấn luyện, tại mỗi bước quyết định, SUMO cung cấp trạng thái giao thông; môi trường tạo quan sát, áp dụng bộ lọc hành động và thực thi hành động do chính sách chọn. Dữ liệu tương tác theo thời gian được sử dụng để tính GAE và cập nhật chính sách theo PPO. Sau 500 lần mô phỏng, bản lưu mô hình cuối cùng được đánh giá trên 10 hạt giống; các chỉ số được tổng hợp bằng trung bình cùng độ lệch chuẩn.

---

## 11. Chỉ số đánh giá (8:30 - 9:15)
**Lời nói:**  
Đề án dùng bốn chỉ số. Ba chỉ số vận hành chính là số phương tiện hoàn thành hành trình, thời gian chờ trung bình và tốc độ trung bình. Dịch chuyển cưỡng bức chỉ là chỉ số hỗ trợ, vì nó còn chịu ảnh hưởng của hình học mạng đường, cấu hình mô phỏng và dao động giữa các hạt giống.

---

## 12. Kết quả đánh giá (9:15 - 10:30)
**Lời nói:**  
Bảng này tóm tắt kết quả trung bình và độ lệch chuẩn trên 10 hạt giống đánh giá. MAPPO đạt thời gian chờ trung bình thấp nhất và tốc độ trung bình cao nhất. GAT-MAPPO đạt số phương tiện hoàn thành hành trình cao nhất, nhưng mức chênh lệch so với MAPPO nhỏ. Số sự kiện dịch chuyển cưỡng bức không cho thấy xu hướng cải thiện rõ ràng dưới các chính sách học tăng cường.

---

## 13. Thảo luận kết quả (10:30 - 11:30)
**Lời nói:**  
Kết quả cho thấy MAPPO và GAT-MAPPO là hai phương pháp nổi bật nhất trong kịch bản Thanh Xuân UTM. Tuy nhiên, mỗi phương pháp mạnh ở một chỉ số khác nhau: MAPPO tốt hơn về thời gian chờ và tốc độ trung bình, trong khi GAT-MAPPO cao nhất về số phương tiện hoàn thành hành trình. Max-Pressure vẫn là một đối chứng thích ứng mạnh. Đồng thời, việc bổ sung GAT hoặc dùng actor riêng không tự động bảo đảm cải thiện trên mọi chỉ số.

---

## 14. Hạn chế và hướng phát triển (11:30 - 12:30)
**Lời nói:**  
Đề án hiện mới được kiểm chứng trong môi trường mô phỏng SUMO. Kết quả còn phụ thuộc vào chất lượng mạng đường, dữ liệu nhu cầu và các giả định khi xây dựng kịch bản. Ngoài ra, thực nghiệm mới tập trung vào một mức nhu cầu chính. Trong tương lai, nghiên cứu có thể mở rộng sang nhiều mức nhu cầu, tăng số hạt giống đánh giá và cải thiện dữ liệu đầu vào để tăng độ tin cậy của kết quả.

---

## 15. Kết luận (12:30 - 13:30)
**Lời nói:**  
Tóm lại, đề án đã xây dựng được một quy trình mô phỏng, huấn luyện và đánh giá các phương pháp điều khiển tín hiệu giao thông đa tác tử trên kịch bản Thanh Xuân UTM. Kết quả cho thấy MAPPO và GAT-MAPPO là hai phương pháp có kết quả tốt nhất ở các chỉ số vận hành chính. Tuy nhiên, không có phương pháp nào vượt trội trên toàn bộ tiêu chí, và các biến thể GAT không luôn cải thiện so với biến thể không dùng GAT. Vì vậy, kết quả cần được diễn giải thận trọng theo từng chỉ số.
