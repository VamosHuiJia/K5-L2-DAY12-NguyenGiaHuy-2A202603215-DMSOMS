# Khai báo dùng AI — Nguyễn Gia Huy

**MSSV:** 2A202603215 · **Mã bộ ảnh:** G02

Mình dùng ChatGPT/Codex để tìm hiểu yêu cầu, hỗ trợ thao tác CVAT, xử lý script, ghi kết quả review và soạn báo cáo. Bảng dưới tổng hợp các lượt hỏi nối tiếp về cùng một nội dung.

Mình tự gán lại bộ ảnh và xuất bài trước review. Sau đó, mình có nhờ AI trực tiếp rà soát và hiệu chỉnh hai file ZIP; bản `export_final_v2.zip` hiện tại chứa các sửa đổi đó. Vì vậy, phần hỗ trợ ở lần hiệu chỉnh sau review không chỉ là góp ý câu chữ.

| # | Công cụ | Bạn hỏi gì (tóm tắt) | Bạn dùng kết quả thế nào | Bạn có kiểm lại không? Bằng gì? |
|---|---|---|---|---|
| 1 | ChatGPT/Codex | Đọc đề và giải thích cần làm gì, nộp gì; cách tạo project, task chính và task extra. | Mình dùng hướng dẫn để chia task chính 22 ảnh và task extra 4 ảnh, chọn định dạng export và xác định các file cần nộp. | Đối chiếu với `GUIDE.md`, `SUBMISSION.md`, `RULES.md` và `CHECKPOINTS.md`. |
| 2 | ChatGPT/Codex | Hỏi cách gán nhãn qua ảnh chụp CVAT ở G01; sau khi chuyển sang G02, nhờ xem ảnh và tạo hai ZIP annotation tham khảo. | AI đã góp ý nhãn tham khảo. Sau đó mình tự gán lại toàn bộ ảnh, thuộc tính và tự export bài G02 trước bước review, như mình đã xác nhận trong chat. | Mình thao tác trên CVAT và chạy checker cho các bản tự export. Kết quả checker kiểm được cấu trúc nhãn, chưa đủ để xác nhận mọi quyết định thị giác. |
| 3 | ChatGPT/Codex | Giải thích lỗi đường dẫn manifest G01/G02 và sửa checker để nhận `child_seat.occupied = yes` theo xác nhận của coach. | Mình dùng manifest G02. Codex bổ sung `yes` trong `scripts/cabin_schema.py` và kiểm tra tính thống nhất với bộ nhãn CVAT. | Căn cứ xác nhận của coach; chạy lại checker và kiểm tra các giá trị hợp lệ trong schema. |
| 4 | ChatGPT/Codex | Giải thích kết quả diff, cách điền log để bạn review hiểu chiều `mine`/`peer`. | Sau khi trao đổi với Đặng Đình Thái, mình cung cấp 5 quyết định đã thống nhất. AI giúp ghi đúng các cột `verdict`, `guideline_rule`, `action` trong `disagreement_log.csv`. | Đối chiếu với 5 quyết định mình đã xác nhận trong chat và các quy tắc GUIDE §4.5, §4.7. |
| 5 | ChatGPT/Codex | Nhờ chỉnh sửa lại cách trình bày `ir_log.csv`, `class_list_v0.md`, `self_review.md` để đạt yêu cầu. | AI sửa lại các bảng và nội dung từ guideline, nhãn v2, quyết định review và thông tin mình cung cấp. | Codex đối chiếu cấu trúc với mẫu và các giá trị được nêu với bản v2. Phần mô tả quan sát và tự đánh giá cần được mình đọc lại để bảo đảm phản ánh đúng quá trình làm. |
