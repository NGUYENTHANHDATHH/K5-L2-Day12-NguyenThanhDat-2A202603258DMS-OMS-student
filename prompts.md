# Khai báo dùng AI — Nguyen Thanh Dat

Công cụ: **Claude Code** (trong VS Code), model Claude Haiku 5.5, sau đó Claude Opus 5.5.

Không đưa ảnh cabin hay ảnh tài xế (kể cả ảnh chụp màn hình) cho AI. AI chỉ đọc tài liệu trong repo, `labels_cabin_v1.json`, kết quả `cabin_check` và `annotations.xml` trong file export (toạ độ + thuộc tính, không có ảnh). AI không gán nhãn thay mình.

| #   | Công cụ     | Bạn hỏi gì (tóm tắt)                                           | Bạn dùng kết quả thế nào                                                                         | Bạn có kiểm lại không? Bằng gì?         |
| --- | ----------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ | --------------------------------------- |
| 1   | Claude Code | "Đọc repo rồi cho tôi biết tôi cần làm gì"                     | Lập thứ tự làm: Task 0–4, Checkpoint A/B, kiểm cuối, nộp bài                                     | Có, đối chiếu README, GUIDE, SUBMISSION |
| 2   | Claude Code | "Vậy là tôi tạo 2 task riêng"                                  | Tạo task chính `Day12-G01` 22 ảnh; S09–S10 (tùy chọn) để task riêng                              | Có, đối chiếu GUIDE Task 0 và §5        |
| 3   | Claude Code | "Thế sao để tạo check_cpA.txt"                                 | Làm theo các bước export CVAT for images 1.1, bỏ "Save images", chạy `cabin_check` với `--out`   | Có, đối chiếu CHECKPOINTS.md            |
| 4   | Claude Code | "Trong task có 22 ảnh, tôi export lẻ 4 ảnh đầu được không"     | Export cả task, dùng `--only S01,S02` để chỉ kiểm 4 ảnh                                          | Có, đối chiếu CHECKPOINTS.md            |
| 5   | Claude Code | "Ảnh S01 thì tôi cần gán gì" (không gửi ảnh)                   | Gán theo thứ tự `frame_meta` → box theo ghế; ghế gọi theo xe                                     | Có, đối chiếu GUIDE §4.2, §4.8          |
| 6   | Claude Code | "Ảnh D01 cần gán gì" (không gửi ảnh)                           | Gán `person` → `face` → `face13` → `hand` → `phone`, không xoay ảnh                              | Có, đối chiếu GUIDE §4.3, §4.5–4.7      |
| 7   | Claude Code | Dán nội dung schema nhãn, hỏi "file này có thêm thuộc tính gì" | Biết các giá trị mặc định cần chọn lại (`seat`, `modality`, `side`); không dùng `occupied = yes` | Có, đối chiếu GUIDE §4.1                |
| 8   | Claude Code | "Đã export 4 ảnh đầu" — nhờ chạy Checkpoint A                  | Lần đầu ra 4 lỗi thiếu `frame_meta` (export trước khi gán); gán lại rồi export                   | Có, xem lại trên CVAT                   |
| 9   | Claude Code | "Tôi đang gán D01–D04"                                         | Nhắc kiểm Checkpoint B với `--only D01,D02` và quay lại OMS                                      | Có, đối chiếu CHECKPOINTS.md            |
| 10  | Claude Code | "Ảnh S01 có person thì có gán face13 không"                    | Không vẽ `face13` trên S##, chỉ box `face`                                                       | Có, đối chiếu GUIDE §4.6                |
| 11  | Claude Code | "Tôi cần làm gì tiếp"                                          | Danh sách bước còn lại đến lúc nộp                                                               | Có, đối chiếu SUBMISSION.md             |
