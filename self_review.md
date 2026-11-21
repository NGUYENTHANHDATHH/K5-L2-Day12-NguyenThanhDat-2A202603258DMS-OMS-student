# Tự đánh giá — Nguyen Thanh Dat · G01

## 1. Tự rà soát 4 cảnh

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S01_rgb | `frame_meta` | guess | `light = day` trên ảnh dựng SVIRO; phải là `unknown` (lặp lại ở cả 8 ảnh S##_rgb) | chưa |
| S06_rgb | ghế rear_left, rear_middle | missing | RGB chỉ có `child_seat`, IR cùng cảnh có thêm `infant_seat` và `object` (bag); kiểm lại RGB với brightness | chưa |
| D03 | `face13` điểm mắt | flag | Mắt `unknown` nhưng 8 điểm mắt không có cờ `occluded`/`outside` | chưa |
| D04 | `face13` | missing | Mặt rộng khoảng 210 px (≥ 60 px) nhưng chưa vẽ `face13` | chưa |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: Không kiểm lại tag `frame_meta`. `light` ghi `day`/`night` trên cả 8 ảnh S##_rgb (phải là `unknown`), và `modality` ghi `rgb` trên cả 6 ảnh D## (phải là `ir`).

Vì sao mình mắc: Đặt `frame_meta` theo cảm nhận độ sáng của ảnh và theo giá trị đã chọn ở ảnh trước, không đối chiếu lại quy tắc của từng bộ dữ liệu. `cabin_check` không báo lỗi `modality` trên D## nên mình không phát hiện sớm.

Lần sau tránh bằng cách: Trước khi vẽ box, đọc lại 3 dòng: S## → `light = unknown`; `_ir` và D## → `modality = ir`; `_rgb` → `modality = rgb`. Sau mỗi checkpoint, sửa ngay loại lỗi đã thấy trên **tất cả** ảnh còn lại, không chỉ ảnh đã kiểm.

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình đúng và mình sai. Mình học được gì từ dòng đó: … (điền sau review chéo)
