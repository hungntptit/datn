# Pseudocode Huấn Luyện Và Đánh Giá

Tài liệu này mô tả logic chính của hệ thống bằng pseudocode. `N` là số lần mô phỏng huấn luyện; trong thực nghiệm chính, `N = 500`.

## 1. Huấn luyện một mô hình MARL

```text
KHỞI TẠO môi trường trên SUMO
KHỞI TẠO agent theo thuật toán đã chọn
    MAPPO, HAPPO, GAT-MAPPO hoặc GAT-HAPPO

CHO mỗi lần mô phỏng episode từ 1 đến N:
    obs, info <- environment.reset(seed)
    khởi tạo rollout rỗng
    khởi tạo tổng reward của các tác tử

    LẶP cho đến khi episode kết thúc:
        # 1. Tạo đầu vào từ quan sát hiện có
        node_features <- tạo đặc trưng nút từ obs
        global_state <- ghép node_features của tất cả tác tử
        action_masks <- lấy tập hành động hợp lệ từ info

        # 2. Actor và critic suy luận
        actions, log_probs, values <- agent.act(
            node_features, global_state, action_masks
        )
        action_dict <- gán mỗi action cho đúng ID tác tử

        # 3. Lớp môi trường điều khiển tương tác với SUMO
        next_obs, reward_dict, terminated, truncated, info <-
            environment.step(action_dict)
        rewards <- đổi reward_dict thành mảng theo thứ tự tác tử
        done <- terminated hoặc truncated

        # environment.step thực hiện:
        # - kiểm tra hành động có hợp lệ không;
        # - gửi yêu cầu điều khiển đèn sang SUMO;
        # - chạy SUMO thêm 5 giây;
        # - lấy dữ liệu thô từ SUMO;
        # - tạo next_obs và tính reward_dict.

        # 4. Lưu dữ liệu của bước vừa chạy
        rollout.add(
            node_features, global_state, action_masks,
            actions, log_probs, values, rewards, done
        )
        obs <- next_obs

        # 5. Cập nhật khi đủ lô dữ liệu hoặc episode kết thúc
        NẾU rollout đủ 128 bước HOẶC done:
            NẾU done:
                last_values <- vector 0
            NGƯỢC LẠI:
                last_values <- critic ước lượng giá trị của obs hiện có

            rollout.tính lợi thế bằng GAE(last_values)
            agent.cập nhật PPO(rollout)
            xóa rollout

    ghi metrics cuối episode vào CSV lịch sử huấn luyện
    lưu bản lưu mô hình mới nhất và metadata kèm theo
    lưu thêm bản lưu có tên theo chu kỳ đã cấu hình

ĐÓNG môi trường SUMO
```

## 2. Đánh giá một bản lưu mô hình

```text
NẠP checkpoint và kiểm tra checkpoint có khớp kịch bản không
KHỞI TẠO môi trường SUMO với một seed đánh giá
obs, info <- environment.reset(seed)

LẶP cho đến khi episode kết thúc:
    node_features <- tạo từ obs
    global_state <- ghép đặc trưng của tất cả tác tử
    action_masks <- lấy từ info

    actions <- agent.act(..., deterministic = True)
    action_dict <- gán mỗi action cho đúng ID tác tử
    next_obs, _, terminated, truncated, info <- environment.step(action_dict)
    done <- terminated hoặc truncated
    obs <- next_obs

metrics <- info["metrics"]
ghi metrics theo seed vào kết quả đánh giá

# Không tạo rollout, không tính GAE và không cập nhật trọng số.
```

## 3. Đánh giá đa hạt giống

```text
CHO mỗi seed trong danh sách seed:
    chạy điều khiển cố định và Max-Pressure với seed đó
    chạy đánh giá cho từng mô hình MARL với cùng seed đó
    ghi một dòng kết quả cho mỗi phương pháp--seed

tính trung bình, độ lệch chuẩn và khoảng tin cậy 95% theo từng phương pháp
ghi raw CSV và summary CSV
```

## 4. Cách đọc nhanh

```text
reset tạo obs ban đầu
    -> actor/critic dùng obs hiện có để chọn hành động và ước lượng giá trị
    -> environment.step chạy SUMO 5 giây
    -> môi trường trả next_obs và reward
    -> lưu rollout
    -> đủ 128 bước hoặc done thì GAE + PPO update
    -> next_obs trở thành obs của bước sau
```
