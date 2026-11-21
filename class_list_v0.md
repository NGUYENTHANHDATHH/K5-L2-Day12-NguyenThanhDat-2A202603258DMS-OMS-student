# Danh sách lớp v0 — Nguyen Thanh Dat · G01

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `eye_left`, `eye_right` | open / closed / unknown | Phát hiện buồn ngủ: tỉ lệ thời gian mắt nhắm (PERCLOS), nhắm lâu liên tục | Có, trừ khi phản xạ LED trên kính hoặc kính râm che |
| 2 | `gaze_zone` | road, mirror_l, mirror_r, mirror_c, cluster, center, lap, other | Phát hiện mất tập trung: nhìn khỏi đường quá N giây | Có (hướng đầu + mắt); kém chính xác khi mắt `unknown` |
| 3 | Tư thế đầu (từ `face13`) | yaw, pitch, roll (độ) | Ước lượng hướng nhìn khi không thấy mắt; phát hiện gục đầu | Có, điểm mặt dựa vào hình dạng |
| 4 | `phone` + `state` | held / mounted / lap | Cảnh báo dùng điện thoại khi lái | Có (hình dạng); màn hình sáng có thể trông khác trên NIR |
| 5 | `hand` + `on_wheel` | yes / no / unknown, theo `side` | Cảnh báo rời tay vô lăng; kiểm tài xế sẵn sàng nhận lại quyền lái từ chế độ tự lái | Có |
| 6 | `glasses` | none / clear / sun / unknown | Biết khi nào `eye_*` không tin được, để hạ độ tin cậy cảnh báo buồn ngủ | Một phần: kính râm có thể trông trong suốt trên NIR |
| 7 | Miệng mở / ngáp (từ điểm miệng `face13`) | open / closed | Dấu hiệu buồn ngủ sớm, trước khi mắt nhắm | Có |
| 8 | `frame_meta.camera_blocked` | yes / no | Hệ DMS tự báo lỗi khi camera bị che, thay vì báo sai "tài xế ổn" | Có |

## OMS

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person` + `seat` | theo ghế: rear_left, rear_middle, rear_right… | Đếm người, biết ghế nào có người để bật nhắc dây an toàn và điều chỉnh túi khí | Có (hình dạng) |
| 2 | `person.seatbelt` | worn_correct / worn_incorrect / not_worn / unknown | Nhắc thắt dây hàng ghế sau; phát hiện thắt sai (dưới nách, sau lưng) | Một phần: dây tối dễ chìm trên áo tối |
| 3 | `child_seat` + `occupied` | no / unknown, theo ghế | Tắt túi khí khi có ghế trẻ em ở ghế trước; nhắc quên trẻ khi rời xe | Có (hình dạng); bên trong ghế khó thấy |
| 4 | `infant_seat` + `facing` | rear / front, theo ghế | Ghế em bé quay sau ở ghế trước phải tắt túi khí | Có (hình dạng) |
| 5 | `object` + `type` | bag / box / cup / other, theo ghế | Nhắc quên đồ; không báo nhầm "người chưa thắt dây" khi chỉ có túi trên ghế | Có hình dạng, nhưng mất màu nên khó phân biệt loại vật |
| 6 | Ghế trống | số ghế trống | Không bật nhắc dây cho ghế trống; biết chỗ còn trống | Có |
| 7 | Trẻ em (người nhỏ trong ghế trẻ em) | có / không, theo ghế | Phát hiện trẻ bị bỏ quên trong xe | Có (hình dạng); SVIRO bộ này không có trẻ em ngồi nên chưa gán được |

## 2 câu hỏi leo thang

1. Câu hỏi: Khi ảnh RGB tối không thấy một vật mà ảnh IR-sim cùng cảnh thấy rõ, có được vẽ box trên RGB dựa vào ảnh IR không? Nếu không, cùng một cảnh sẽ có hai bộ nhãn khác nhau khi huấn luyện model hai luồng RGB + IR.
   Khung ví dụ: G01 S06_rgb / S06_ir (ghế em bé rear_left và túi rear_middle).
   Bạn tạm gán thế nào và vì sao: Chỉ vẽ cái thấy trên từng ảnh, vì guideline §4.4-1 cấm suy từ ảnh kia của cùng cảnh. Ghi khác biệt vào `ir_log`.
2. Câu hỏi: Khi cả hai mắt là `unknown` (phản xạ trên kính), `gaze_zone` có được gán theo hướng đầu không, hay phải ghi `other`?
   Khung ví dụ: G01 D03 (mirror_r) và D05 (road).
   Bạn tạm gán thế nào và vì sao: Gán theo hướng đầu khi hướng đầu rõ, vì §4.5 nói vùng nhìn theo "hướng mắt và đầu". Nếu đầu cũng không rõ thì `other` và ghi lý do.
