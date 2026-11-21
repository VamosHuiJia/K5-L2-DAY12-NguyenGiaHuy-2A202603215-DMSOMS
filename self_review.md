# Tự đánh giá — Nguyễn Gia Huy · G02

**MSSV:** 2A202603215

Mình tổng hợp phần tự đánh giá từ các quyết định review chéo đã thống nhất và những thay đổi có trong `export_final_v2.zip`. Phần rà soát bổ sung cờ điểm mặt có hỗ trợ AI. Các mục ghi đã sửa dưới đây nói đến dữ liệu trong file ZIP v2, không mặc định rằng task trên CVAT đã được đồng bộ với file đó.

## 1. Tự rà soát 4 cảnh

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| `S03_ir.png` | `child_seat` và `person` ở `rear_middle` | `ok` | Thuộc tính ghế của hai đối tượng cùng là `rear_middle`. Ghế trẻ em có `occupied = yes`, phù hợp với cập nhật đã được coach xác nhận cho G02. Nhận xét này chỉ áp dụng cho các thuộc tính vừa nêu, không kết luận rằng mọi đối tượng trong ảnh đã hết bất đồng. | Giữ nguyên các thuộc tính này. |
| `S05_rgb.png` | `person.seatbelt` | `ok` | Mình giữ `unknown` vì phần thân người trong ảnh RGB tối, không đủ rõ để kết luận về dây an toàn. Mình không lấy giá trị `not_worn` ở ảnh IR cùng cảnh để điền sang RGB. Đây vẫn là một điểm khác với bài peer cần tiếp tục đối chiếu nếu hai bên chưa chốt. | Giữ `unknown` trong bản v2. |
| `D01.png` | `face.glasses` | `wrong_class` (thuộc tính kính) | Ban đầu mình để `unknown`, còn bạn review chọn `clear`. Sau khi trao đổi, mình và bạn thống nhất dùng `clear`, nên mình sửa lại thuộc tính này. Trạng thái mắt trái vẫn là `unknown` vì chưa phân biệt được mắt nhắm hay mở. | Đã sửa `glasses = clear`; đã có trong bản v2. |
| `D05.png` | Các cờ của `face13` | `flag` | Trước lần rà soát bổ sung, cả 13 điểm đều có `occluded = 1`. Bản v2 chuyển 8 điểm mắt sang `outside = 1`, `occluded = 0` do không định vị chắc mép mí trong vùng nhòe. Năm điểm mũi và miệng được bỏ cờ che khuất, giữ nguyên tọa độ đã đặt. Đây là phần hiệu chỉnh có hỗ trợ AI, cần đối chiếu lại trên CVAT trước khi xác nhận vị trí và mức nhìn thấy của từng điểm. | Đã cập nhật các cờ trong ZIP v2. |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

**Lỗi:** Mình gán cờ điểm mặt chưa phân biệt rõ từng trường hợp. Ở D05, lỗi lặp trên nhiều điểm vì cả 13 điểm đều được đánh `occluded`, dù vùng mắt nhòe và vùng mũi, miệng cần được xét riêng.

**Vì sao mình mắc:** Cách gán đồng loạt làm mất sự khác nhau giữa điểm bị vật che, điểm không định vị được và điểm vẫn nhìn thấy. Khi thuộc tính mắt là `unknown`, việc đánh cùng một cờ cho toàn bộ khuôn mặt cũng không giải thích đúng nguyên nhân mắt không quan sát rõ.

**Lần sau tránh bằng cách:** Mình sẽ kiểm từng mắt và từng điểm thay vì áp dụng một cờ cho cả skeleton. Với điểm bị che nhưng còn ước lượng được vị trí, mình dùng `occluded`; với điểm không định vị chắc, mình dùng `outside` theo GUIDE §4.6. Điểm nhìn thấy được sẽ không bị đánh cờ che khuất chỉ vì một vùng khác trên mặt khó nhìn. Sau đó mình kiểm lại tính nhất quán giữa trạng thái mắt và các cờ điểm mặt, rồi chạy script để phát hiện lỗi cấu trúc hoặc cảnh báo còn sót.

## 3. Ghi chú về disagreement_log

Một dòng mình sửa theo bạn review là **D01 — `face.glasses`**. Bài ban đầu của mình dùng `unknown`, bài của bạn dùng `clear`; sau khi thống nhất, mình ghi `verdict = peer`, quy tắc `4.5` và đổi giá trị thành `clear`. Việc sửa này đã được ghi trong `disagreement_log.csv` và thể hiện trong bản v2.

Qua dòng này, mình học được rằng cần xét riêng từng thuộc tính. Không nhìn rõ mắt trái không có nghĩa là mọi thông tin về kính đều không xác định được. Vì vậy, ở D01 mình có thể giữ `eye_left = unknown` nhưng dùng `glasses = clear` theo kết quả review.

Sau lần hiệu chỉnh bổ sung, số dòng khác giữa bản v2 của mình và bản peer giảm từ 77 xuống 27. Đây là số bất đồng còn lại, không phải số lỗi đã được xác nhận. Bản v2 của mình có đủ 22 ảnh và được script báo 0 lỗi, 0 cảnh báo; những khác biệt chưa thống nhất vẫn cần đối chiếu bằng guideline và nội dung ảnh.
