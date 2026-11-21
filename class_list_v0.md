# Danh sách lớp v0 — Nguyễn Gia Huy · G02

**MSSV:** 2A202603215

Em chọn các lớp và thuộc tính dưới đây dựa trên Guideline Cabin v1.0 và bộ nhãn đang dùng trong lab. Mỗi mục gắn với một chức năng cụ thể của DMS hoặc OMS. Những phần không quan sát rõ được giữ là `unknown`, hoặc `other` đối với vùng nhìn, theo giá trị mà bộ nhãn cho phép.

Với bộ G02, coach đã xác nhận bổ sung `child_seat.occupied = yes`. Vì vậy, danh sách này sử dụng ba giá trị `yes`, `no`, `unknown` cho thuộc tính đó, theo bộ nhãn và script đã cập nhật.

## DMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `face` | Box khuôn mặt khi nhìn thấy mặt | Xác định vùng mặt tài xế để xử lý mắt, kính và hướng nhìn ở các bước sau. | Có thể nhận ra đường nét mặt. Khi mặt quay đi hoặc bị che gần hết thì không đủ căn cứ để vẽ box. |
| 2 | `face13` | 13 điểm mặt, mỗi điểm có cờ `occluded` và `outside` | Làm mốc căn chỉnh khuôn mặt và hỗ trợ ước lượng tư thế đầu. Các cờ giúp phân biệt điểm nhìn thấy với điểm bị che hoặc không xác định được. | Có thể gán nếu mặt đủ rõ và box mặt rộng ít nhất 60 px. Điểm bị kính, tay hoặc vật che phải có cờ phù hợp. |
| 3 | `face.eye_left`, `face.eye_right` | Mỗi mắt gán riêng: `open` / `closed` / `unknown` | Theo dõi trạng thái từng mắt. Khi có chuỗi khung hình, thông tin này có thể hỗ trợ nhận biết nhắm mắt kéo dài; một ảnh mắt nhắm chưa đủ để kết luận buồn ngủ. | Có thể thấy khe mắt và mí mắt. Khi ảnh nhòe, bị che hoặc có phản xạ trên kính thì dùng `unknown` nếu không phân biệt được. |
| 4 | `face.glasses` | `none` / `clear` / `sun` / `unknown` | Nhận biết kính đang đeo để đánh giá mức độ quan sát được vùng mắt và hỗ trợ xử lý các trường hợp mắt khó nhìn. | Có thể thấy gọng và tròng kính, nhưng không phải lúc nào cũng phân biệt chắc kính trong với kính râm trên NIR. Không chắc thì chọn `unknown`. |
| 5 | `face.gaze_zone` | `road` / `mirror_l` / `mirror_r` / `mirror_c` / `cluster` / `center` / `lap` / `other` | Xác định vùng tài xế đang nhìn để hỗ trợ theo dõi sự chú ý. Hướng nhìn cần dựa vào mắt và đầu, không suy chỉ từ việc tài xế cầm điện thoại. | Có thể xác định khi mắt và tư thế đầu đủ rõ. Nếu không chắc vùng nhìn thì dùng `other` theo guideline hiện tại và ghi lý do. |
| 6 | `hand` và các thuộc tính | `whose`: `driver` / `passenger`; `side`: `left` / `right`; `on_wheel`: `yes` / `no` / `unknown` | Phân biệt tay tài xế với tay hành khách và kiểm tra tay có chạm vô lăng hay không. Trái/phải tính theo người; tay rời vô lăng trong bộ xe tự lái chưa phải là lỗi. | Có thể nhận ra bàn tay theo hình dạng. Nếu không thấy rõ vị trí tiếp xúc với vô lăng thì `on_wheel = unknown`. |
| 7 | `phone.state` | `held` / `mounted` / `lap`, gắn với box `phone` | Phân biệt điện thoại đang được cầm, đặt trên giá hay trên đùi để hỗ trợ nhận biết tình huống sử dụng điện thoại. | Có thể nhận ra từ cạnh, góc hoặc màn hình. Chỉ gán `phone` khi thấy đủ dấu hiệu của vật; tay áp tai chưa đủ để xác định có điện thoại. |

## OMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person` và `person.seat` | Box người; ghế sau: `rear_left` / `rear_middle` / `rear_right` | Nhận biết có người và người đó ở chỗ ngồi nào, làm cơ sở theo dõi số người trong cabin. Ghế được gọi theo xe, nên bên trái ảnh OMS là `rear_right`. | Có thể nhận ra cơ thể theo đường nét và vị trí. Nếu bị che một phần thì box chỉ ôm phần cơ thể nhìn thấy. |
| 2 | `child_seat.occupied` | Box `child_seat`, thuộc tính `seat`; `occupied`: `yes` / `no` / `unknown` | Nhận biết ghế trẻ em và tình trạng có người trong ghế theo cập nhật của coach. Cần đối chiếu thêm nhãn `person` khi đánh giá có trẻ trong xe. | Có thể nhận ra hình dạng ghế. Việc xác định `occupied` phụ thuộc vào phần bên trong nhìn thấy được; bị che và không xác định được thì dùng `unknown`. |
| 3 | `infant_seat.facing` | Box `infant_seat`, thuộc tính `seat`; `facing`: `rear` / `front` | Phân biệt nôi/ghế em bé với ghế trẻ em và ghi hướng đặt so với xe. Muốn xác định có người trong nôi cần xét thêm nhãn `person`. | Có thể nhận ra qua hình dạng vỏ ghế, phần tựa và hướng đặt. Không dựa vào màu vải để phân loại trên IR. |
| 4 | `object.type` | Box `object`, thuộc tính `seat`; `type`: `bag` / `box` / `cup` / `other` | Phân biệt đồ vật với người và ghi đồ vật đang chiếm chỗ nào. Nếu vật nằm trong ghế trẻ em thì có box riêng cho vật và cho ghế. | Có thể phân loại bằng hình dạng và đường viền. Không nhận chắc loại vật thì dùng `other`, không suy từ màu của ảnh RGB. |
| 5 | `person.seatbelt` | `worn_correct` / `worn_incorrect` / `not_worn` / `unknown`; trên SVIRO của lab chỉ dùng `not_worn` / `unknown` | Hỗ trợ kiểm tra tình trạng dây an toàn theo người và chỗ ngồi. Bộ SVIRO của lab không có người thắt dây, nên chưa đủ dữ liệu để học hai trạng thái `worn_correct` và `worn_incorrect`. | Chỉ xác định khi quan sát được dải dây hoặc thấy rõ không có dây. Vùng thân quá tối hoặc bị che thì dùng `unknown` cho chính ảnh đang xem. |
| 6 | `frame_meta.camera_blocked` | `yes` / `no`, nằm trong một tag `frame_meta` của mỗi ảnh | Đánh dấu camera bị che để hệ thống biết khi nào việc quan sát cabin bị hạn chế, tránh diễn giải mất hình do che camera thành ghế không có người. | Có thể nhận biết sự che khuất ảnh, nhưng cần phân biệt với ảnh tối hoặc vùng bị che bởi một đối tượng trong cabin. |

## 2 câu hỏi leo thang

1. **Câu hỏi:** Thuộc tính `gaze_zone = other` hiện dùng cho cả trường hợp nhìn ra ngoài các vùng đã định nghĩa và trường hợp không xác định được hướng nhìn. Ở bản guideline tiếp theo, có nên tách hai trường hợp này để model không học lẫn hướng nhìn ngoài vùng với dữ liệu không quan sát được không?

   **Khung ví dụ:** `G02/driver/D02.png`. Sau review, hai bạn thống nhất giữ `gaze_zone = other` vì chưa đủ căn cứ xác định vùng nhìn.

   **Em tạm gán thế nào và vì sao:** Em giữ `other` theo GUIDE §4.5. Việc có điện thoại trong ảnh không đủ để kết luận tài xế đang nhìn vào đâu, nên em không đổi thành `road` hoặc một vùng cụ thể chỉ dựa vào hành động đó.

2. **Câu hỏi:** Sau khi bổ sung `child_seat.occupied = yes`, giá trị `yes` có chỉ dành cho ghế có người hay bao gồm cả trường hợp ghế chỉ chứa đồ vật? Em muốn guideline làm rõ trường hợp chỉ có túi hoặc hộp trong ghế để các bạn áp dụng thống nhất và để model phân biệt có trẻ với có vật.

   **Khung ví dụ:** `G02/oms/S03_ir.png` và `G02/oms/S03_rgb.png`. Ghế trẻ em ở `rear_middle` có người, và bài của em đang dùng `occupied = yes` theo xác nhận của coach. Đây là mốc để phân biệt với trường hợp giả định chỉ có đồ vật trong ghế.

   **Em tạm gán thế nào và vì sao:** Với S03, em gán box `child_seat` có `seat = rear_middle`, `occupied = yes`, đồng thời gán box `person` riêng với cùng `seat`. Em dùng giá trị mới đã được coach xác nhận; cách xử lý ghế chỉ chứa đồ vật cần được làm rõ trong guideline cập nhật.
