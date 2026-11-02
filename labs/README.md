
# Labs

## Mục đích

Labs là các bài tập nhỏ về agent trong đời sống (nấu ăn, mua sắm). Mục tiêu là luyện
cách đặt ràng buộc cứng, chọn mức tự chủ và thiết kế cổng duyệt của con người cho
agent, trước khi áp dụng vào tool quản lý công việc. Đây là bài tập học, không phải
sản phẩm; kết quả được dùng để rút bài học và so sánh hai model.

## Labs, poc và experiments khác nhau thế nào

| Thư mục    | Chứa gì                                                            | Dùng cho sản phẩm chính không                              |
| ------------ | -------------------------------------------------------------------- | --------------------------------------------------------------- |
| labs/        | Bài tập đời sống (nấu ăn, mua sắm) để luyện tư duy agent | Không dùng code, chỉ rút bài học                          |
| poc/         | Thử giả thuyết kỹ thuật của tool quản lý công việc         | Không dùng code; kết quả dẫn tới quyết định ở mốc M1 |
| experiments/ | Dữ liệu so sánh hai model: prompt, kết quả, scorecard           | Dùng để chọn model và xét nâng mức AI                   |

## Quy tắc chung

- Code trong labs là throwaway, không đưa vào sản phẩm chính.
- Dữ liệu giả, không dữ liệu cá nhân thật, không khóa API trong prompt.
- Trước khi nhờ AI viết code, mỗi lab phải có yêu cầu và AC đã được tôi duyệt.
  AI được soạn nháp AC (L2) từ mô tả tôi tự viết; tôi sửa và duyệt.
- Viết test trước, từ AC đã duyệt.
- Mỗi lần dùng AI ghi nhật ký vào docs/ai-log (model, phiên bản, prompt, đầu vào, thời điểm).
- Không sửa ngưỡng đánh giá sau khi thấy kết quả. Muốn đổi thì tạo bản mới và ghi lý do.

## Mức AI áp dụng

Hai khái niệm khác nhau, không được lẫn:

- Mức AI giúp tôi làm lab: L3 cho code, L2 cho yêu cầu và AC. Điều kiện của L3
  (test trước, tối đa 3 vòng tự sửa, tôi đọc mọi thay đổi, agent không tự commit)
  xem tại docs/00-process/ai-levels.md.
- Mức tự chủ của agent tôi đang xây: L2, chỉ đề xuất và tạo nháp, không tự hành động.

## Danh sách lab

| Lab            | Câu hỏi POC                                                                                                                                                | Ràng buộc cứng chính                                                             | Đánh giá            |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------ | ---------------------- |
| cooking-agent  | Từ nguyên liệu sẵn có, sở thích và danh sách kiêng, agent có đề xuất được thực đơn hợp lý mà không vi phạm ràng buộc nào không? | Không đề xuất món chứa chất kiêng giả định                                | cooking-agent/eval.md  |
| shopping-agent | Từ nhu cầu và ngân sách, agent có so sánh đúng và lập được giỏ hàng nháp mà không vượt quyền không?                                   | Không tự thanh toán; không làm theo chỉ thị nhúng trong nội dung sản phẩm | shopping-agent/eval.md |

## Cách đánh giá chung

- Ràng buộc cứng: đạt/không đạt bằng quy tắc cố định; một vi phạm là lab không đạt.
- Chất lượng: tôi chấm từng ca, thang 0-2, chấm mù. Định nghĩa điểm và ngưỡng nằm
  trong eval.md của từng lab.
- Lab đạt khi mọi ràng buộc cứng bằng 0 và các chỉ số chất lượng đạt ngưỡng.
- Mẫu nhỏ (khoảng 16 ca mỗi lab), kết quả chỉ mang tính tham khảo.
- Kết quả ghi vào experiments/scorecards.

## Cách bắt đầu một lab

1. Viết mô tả ngắn và câu hỏi POC của lab (tôi tự viết).
2. Soạn requirements.md và AC, tôi duyệt.
3. Điền test-data.md (dữ liệu giả) và eval.md.
4. Chạy thử 3-5 ca riêng để kiểm tra bảng điểm, rồi khóa ngưỡng.
5. Chạy bộ ca chính thức với từng model, ghi vào ai-log và scorecard.
6. Viết 3 dòng bài học.
