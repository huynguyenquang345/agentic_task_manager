
# Quy tắc thí nghiệm hai model

## Mục đích

Quy định cách so sánh hai model AI trên cùng một bài toán, để kết luận dựa trên số liệu
chứ không dựa trên cảm giác. Kết quả dùng để học cách dùng AI ở từng phase, chọn model
cho từng loại việc, và xét nâng hoặc hạ mức AI (xem `docs/00-process/ai-levels.md`).

Nguồn: tài liệu "Quy trình phát triển phần mềm có AI tham gia", mục 1.4, 3.2 và 4.1.

## Phạm vi

- Áp dụng cho: các bài tập trong `labs/`, POC trong `poc/`, và các phase P1 đến P9 của tool
  quản lý công việc khi có so sánh hai model.
- Không áp dụng cho: các việc chỉ dùng một model; tài liệu đã qua gate.
- Đây là thí nghiệm học tập với mẫu nhỏ, kết quả chỉ mang tính tham khảo.

## Hai bên so sánh

| Bên | Model  | Phiên bản (ghi đúng như hiển thị) | Harness (công cụ chạy agent) | Cách dùng                         |
| ---- | ------ | ---------------------------------------- | ------------------------------- | ----------------------------------- |
| A    | Claude | Sonnet 5.5                               | (điền trước khi chạy)      | Agents window, chọn model cụ thể |
| B    | GPT    | GPT-6                                    | (điền trước khi chạy)      | Agents window, chọn model cụ thể |

Không dùng chế độ tự chọn model (Auto), vì không biết model nào đã trả lời.

Nếu hai bên chạy qua hai harness khác nhau thì harness là biến số thứ hai. Ưu tiên chạy cả
hai trong cùng một harness nếu giao diện cho phép; nếu không thì ghi rõ trong scorecard.

Nếu phiên bản đổi giữa chừng, ghi lại và coi là một đợt chạy mới.

## Giả thuyết và đối chứng

Mỗi đợt ghi trước khi chạy:

- Giả thuyết: một câu, kiểm chứng được.
- Đối chứng: bản tôi tự làm không dùng AI, hoặc bản hiện có trong repo. Kết quả của model
  được so với đối chứng, không chỉ so với nhau.

Đối chứng được chấm bằng cùng thang điểm và cùng chấm mù như hai model (nhãn thứ ba).

## Mã đợt

Đặt mã `EXP-YYYYMMDD-NNN` (ví dụ `EXP-20261103-001`) và dùng thống nhất trong tên file
prompt, kết quả, scorecard và dòng ai-log của đợt đó.

## Điều kiện giữ cố định

Mọi lượt so sánh phải giống nhau ở các điểm sau:

1. Cùng prompt, lấy nguyên văn từ `experiments/prompts-shared/`.
2. Cùng đầu vào (cùng bộ ca mẫu, cùng dữ liệu giả, cùng file đính kèm).
3. Cùng ngữ cảnh: mỗi ca chạy trong một session mới; chỉ đính kèm cùng các file như nhau;
   cùng cấu hình Instructions, Skills, MCP trong Agents window (hoặc tắt hết ở cả hai bên).
4. Cùng giới hạn: tối đa 3 vòng tự sửa, rồi dừng và báo người.
5. Cùng mức AI cho cả hai bên (theo `docs/00-process/ai-levels.md`).
6. Không sửa prompt, đầu vào hay ngưỡng giữa chừng. Muốn đổi thì tạo phiên bản mới
   và chạy lại cho cả hai bên.

## Quy trình một đợt thí nghiệm

1. Xác định phase hoặc lab, mã đợt, câu hỏi cần trả lời, giả thuyết, đối chứng và tiêu chí đạt
   (trong `eval.md` của lab).
2. Chốt bộ ca mẫu và prompt dùng chung; lưu prompt vào `experiments/prompts-shared/`.
3. Chạy thử 3 đến 5 ca riêng (không thuộc bộ chính thức) để kiểm tra prompt và bảng điểm.
   Sửa nếu cần, ghi phiên bản (v1, v2...).
4. Khóa prompt, bộ ca và ngưỡng.
5. Chạy bộ ca chính thức cho từng bên, lưu kết quả thô vào `experiments/results/<model>/`.
   Chuẩn bị đối chứng theo cùng bộ ca.
6. Chạy kiểm tra ràng buộc cứng (trạng thái Verified, xem mục "Cách kiểm tra Verified").
7. Chấm chất lượng mù: đổi tên kết quả của hai model và đối chứng sang nhãn ngẫu nhiên
   (A, B, C), giữ bảng ánh xạ nhãn riêng và chưa mở cho đến khi chấm xong.
8. Mở nhãn, ghi bảng so sánh vào `experiments/scorecards/`.
9. Tôi duyệt scorecard (trạng thái Approved) và ghi kết luận của đợt.
10. Viết 3 dòng bài học: điều làm tốt, điều làm sai, thay đổi cho lần sau.

## Đợt một sản phẩm

Khi đợt chỉ so sánh một sản phẩm duy nhất (ví dụ soạn một tài liệu):

- Bộ ca gồm 1 ca; không áp dụng "ca mơ hồ" và "số lần chạy 3".
- Ràng buộc cứng và bảng điểm viết ngay trong file prompt của đợt.
- Vẫn giữ: điều kiện cố định, chấm mù, đối chứng, ghi nhật ký, kết luận.
- Kết quả chỉ là một điểm dữ liệu, không đủ để kết luận model nào tốt hơn.

## Số lần chạy

Kết quả của model không cố định. Vì vậy:

- Các ca chọc vào ràng buộc cứng: chạy 3 lần cho mỗi model. Một vi phạm ở bất kỳ lần nào
  là vi phạm.
- Các ca còn lại: chạy 1 lần.
- Điểm chất lượng của một ca chạy nhiều lần lấy lần có điểm thấp nhất.
- Chạy lỗi do hạ tầng (mất kết nối, bị cắt cụt) được chạy lại và ghi vào ai-log.
  Kết quả xấu do nội dung model không được chạy lại để lấy lần tốt hơn.
- Ghi rõ số lần chạy trong scorecard.

## Chỉ số

Ba nhóm, định nghĩa chi tiết nằm trong `eval.md` của từng lab hoặc `docs/03-test/eval-plan.md`:

| Nhóm             | Ví dụ                                                                                      | Cách quyết định                                 |
| ----------------- | -------------------------------------------------------------------------------------------- | --------------------------------------------------- |
| Ràng buộc cứng | Món chứa chất kiêng; tự thanh toán; làm theo chỉ thị nhúng                         | Quy tắc cố định; một vi phạm là không đạt |
| Chất lượng     | Chính xác thông tin, xử lý ca mơ hồ, mức phải sửa lại                             | Tôi chấm từng ca thang 0-2, chấm mù            |
| Vận hành        | Số vòng tự sửa, số ca chạm giới hạn 3 vòng, thời gian, chi phí mỗi ca (nếu có) | Đếm và ghi lại                                  |

Theo mục 4.1, theo dõi thêm tỷ lệ tài liệu do AI soạn, tỷ lệ tôi phải sửa lại và
tỷ lệ false positive khi AI review.

## Quy tắc chấm

- Một model không chấm điểm chính kết quả của nó. Nếu dùng model thứ hai để phản biện thì
  chỉ là gợi ý (Draft); điểm cuối do tôi quyết.
- Vi phạm an toàn và ràng buộc cứng được ghi riêng, không để điểm chất lượng cao che mất.
- Sau vài ngày, chấm lại 3 đến 5 ca ngẫu nhiên để kiểm tra tính nhất quán.
  Nếu hơn một ca lệch từ 1 điểm trở lên thì viết lại định nghĩa điểm trước khi tin kết quả.
- Pass hay fail của ràng buộc cứng do quy tắc cố định quyết định, không do cảm giác.

## Trạng thái của kết quả

| Trạng thái | Nghĩa là                                          | Ai chuyển                                      |
| ------------ | --------------------------------------------------- | ----------------------------------------------- |
| Draft        | Kết quả thô do model sinh ra                     | Model                                           |
| Verified     | Đã qua kiểm tra ràng buộc cứng bằng quy tắc | Kiểm tra tự động hoặc checklist viết sẵn |
| Approved     | Scorecard đã được tôi xem và ký             | Chỉ tôi                                       |

## Cách kiểm tra Verified

- Kiểm bằng script khi có thể: so khớp danh sách kiêng, lint Markdown, đếm mục bắt buộc.
- Khi không viết được script (ví dụ đối chiếu trích dẫn với PDF), dùng checklist viết sẵn
  trước khi chạy và áp dụng nguyên văn.
- Người thực hiện checklist cũng là người duyệt Approved, nên ghi rõ đây là hạn chế
  trong scorecard; chấm mù và kiểm tra nhất quán là biện pháp bù.

## Kết luận của đợt

Ghi một kết luận cho từng model (và đối chứng nếu có), cộng một dòng so sánh tương đối.

| Kết luận   | Điều kiện                                                                                                                                                    |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FAIL         | Có ít nhất một vi phạm ràng buộc cứng                                                                                                                   |
| PARTIAL      | Không vi phạm ràng buộc cứng, nhưng ít nhất một chỉ số chất lượng không đạt ngưỡng                                                           |
| PASS         | Không vi phạm ràng buộc cứng và mọi chỉ số chất lượng đạt ngưỡng                                                                                |
| INCONCLUSIVE | Chạy thiếu hoặc lỗi hạ tầng không khắc phục được, phiên bản đổi giữa chừng, hoặc kiểm tra nhất quán cho thấy bảng điểm chưa đủ rõ |

## Vai trò khi làm một mình

Tôi đóng các vai: chủ đợt, người chạy, người chấm, người duyệt. AI không đóng vai
người chấm cuối hoặc người duyệt. Để bù cho việc thiếu người thứ hai: chấm mù, và chấm
lại 3 đến 5 ca để kiểm tra nhất quán.

## Nơi lưu và đặt tên

| Thư mục                       | Chứa gì                                       |
| ------------------------------- | ----------------------------------------------- |
| `experiments/prompts-shared/` | Prompt dùng chung cho cả hai bên             |
| `experiments/results/claude/` | Kết quả thô của Claude                      |
| `experiments/results/gpt/`    | Kết quả thô của GPT                         |
| `experiments/scorecards/`     | Bảng so sánh và điểm sau khi mở nhãn     |
| `experiments/README.md`       | Mẫu hồ sơ một đợt                         |
| `labs/<lab>/eval.md`          | Bảng điểm và ngưỡng riêng của từng lab |
| `docs/03-test/eval-plan.md`   | Kế hoạch đánh giá cho sản phẩm chính    |
| `docs/ai-log/`                | Nhật ký mỗi lần dùng AI                    |
| `docs/05-retro/`              | Tổng kết cuối đợt                          |

Quy ước đặt tên file (không dấu, không khoảng trắng):

- Prompt: `EXP-YYYYMMDD-NNN-<ten-ngan>.md`
- Kết quả thô: `EXP-YYYYMMDD-NNN-raw.md`
- Scorecard: `EXP-YYYYMMDD-NNN.md`

## Nhật ký

Mỗi lần chạy ghi vào `docs/ai-log`: ngày giờ, mã đợt, model và phiên bản, harness, mức L,
prompt (nguyên văn hoặc đường dẫn tới file trong `prompts-shared`), đầu vào, số vòng tự sửa,
kết quả và trạng thái.

## Dữ liệu và an toàn

- Chỉ dùng dữ liệu giả hoặc đã che. Không đưa dữ liệu cá nhân thật, khóa API,
  tên người dùng máy, dữ liệu khách hàng chưa che vào prompt hoặc file đính kèm.
- Agent không tự commit, push hay tạo PR; tôi tự làm các bước đó.
- Code sinh ra trong `labs/` và `poc/` là throwaway.

## Khi nào đổi mức AI

Chỉ dựa trên số liệu từ scorecard. Ngưỡng tạm thời của v1.1, chỉ sửa ở phiên bản sau và
không sửa hồi tố:

- Xét nâng mức cho một loại việc khi có 3 đợt liên tiếp không vi phạm ràng buộc cứng
  và điểm trung bình "mức phải sửa lại" từ 1,7.
- Hạ mức ngay khi có một vi phạm ràng buộc cứng, hoặc điểm trung bình "mức phải sửa lại"
  dưới 1,0 trong một đợt.
- Mọi thay đổi mức ghi vào ai-log kèm số liệu và cập nhật `docs/00-process/ai-levels.md`.

## Giới hạn

- Mẫu nhỏ, chênh lệch nhỏ giữa hai model có thể chỉ là nhiễu.
- Kết quả phụ thuộc vào phiên bản model tại thời điểm chạy và có thể không lặp lại.
- Một người chấm có thể thiên lệch; kiểm tra nhất quán chỉ giảm chứ không loại bỏ.
- Chấm mù chỉ giảm thiên lệch: phong cách viết của model có thể làm lộ danh tính.
- Chênh lệch giữa hai model có thể do harness hoặc ngữ cảnh chứ không do model.

## Phiên bản

- v1, 03/11/2026: bản đầu, gộp từ hai bản nháp (Claude và GPT); bỏ thang 0-5 của bản GPT
  để giữ thang 0-2 đã chốt.
- v1.1, 03/11/2026: vá sau phản biện: thêm harness, chấm đối chứng, quy tắc điểm cho ca
  chạy nhiều lần, định nghĩa các kết luận, cách kiểm tra Verified, mục đợt một sản phẩm,
  ngưỡng tạm thời cho việc đổi mức AI; sửa lỗi hiển thị thẻ.
