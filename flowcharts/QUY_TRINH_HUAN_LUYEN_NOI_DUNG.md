# Nội dung Flowchart Vòng Lặp Huấn Luyện

Mỗi mục `[]` là một khối; mỗi mục `<>` là một nút điều kiện. Nội dung này dùng để vẽ lại flowchart trong diagrams.net.

## Tiêu đề

**Vòng lặp huấn luyện**

Phụ đề: **Mỗi episode kéo dài 3.600 giây; mỗi bước quyết định kéo dài 5 giây**

## Nội dung các khối

```text
[Bắt đầu episode]
3.600 giây mô phỏng
        |
        v
[Tạo quan sát cho tác tử]
Và lọc hành động
        |
        v
[Actor và critic]
Chọn hành động, ước lượng giá trị
        |
        v
[Thực thi trong SUMO]
Mô phỏng thêm 5 giây
        |
        v
[Tính phần thưởng]
Và lưu rollout
        |
        v
<Rollout đủ 128 bước hoặc episode kết thúc?>
     | Chưa
     +--------------------------------> quay lại [Tạo quan sát cho tác tử và lọc hành động]
     |
     | Có
     v
[GAE và PPO update]
Cập nhật actor, critic
        |
        v
<Episode kết thúc?>
     | Chưa
     +--------------------------------> quay lại [Tạo quan sát cho tác tử và lọc hành động]
     |
     | Có
     v
<Đủ 500 episode?>
     | Chưa                              | Đủ
     v                                   v
Quay lại bắt đầu episode             [Lưu mô hình cuối cùng]
```

## Nhãn mũi tên vòng

- Từ “Rollout đủ 128 bước hoặc episode kết thúc?” quay về “Tạo quan sát cho tác tử và lọc hành động”: **Chưa**.
- Từ “Episode kết thúc?” quay về “Tạo quan sát cho tác tử và lọc hành động”: **Chưa**.
- Từ “Đủ 500 episode?” quay về “Bắt đầu episode”: **Chưa**.

## Câu nói tương ứng

“Mỗi episode kéo dài 3.600 giây. Ở mỗi bước quyết định, môi trường đọc trạng thái từ SUMO, tạo quan sát cho từng tác tử và lọc các hành động không hợp lệ. Actor chọn hành động, còn critic ước lượng giá trị. SUMO mô phỏng thêm 5 giây; sau đó môi trường tính phần thưởng và lưu dữ liệu vào rollout. Khi rollout đủ 128 bước hoặc episode kết thúc, GAE và PPO cập nhật actor cùng critic. Nếu episode chưa kết thúc, quy trình quay lại bước tạo quan sát. Sau 500 episode, mô hình cuối cùng được lưu để đánh giá.”
