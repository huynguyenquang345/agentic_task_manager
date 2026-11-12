# Tầm nhìn và phạm vi sản phẩm (BRD)

## Trạng thái

- Draft v0.1, do AI soạn (mức L2), chưa được tôi duyệt.
- Mục đánh dấu `[Giả định]` hoặc `(bạn điền)` là chỗ chưa có thông tin thật; tôi thay bằng
  thông tin của mình trước khi duyệt.

Nguồn: tài liệu "Quy trình phát triển phần mềm có AI tham gia", mục 2.1 (P2, các bước S06 đến
S09). Phần đánh dấu `[Đề xuất]` là bổ sung cho dự án này, không có nguyên văn trong tài liệu gốc.

## Bối cảnh và vấn đề

- Bối cảnh: dự án học tập do một người làm, nhằm xây một tool quản lý công việc trong đó AI agent
  là một phần của sản phẩm.
- Vấn đề `[Giả định]`: người dùng mất thời gian tách một mục tiêu lớn thành các task, sắp thứ tự
  và đặt hạn chót; việc này lặp lại và dễ bỏ sót.
- Nỗi đau cụ thể: (bạn điền, ba nỗi đau thật của tôi hoặc của người dùng thử).

## Mục tiêu

| Mục tiêu | Nội dung                                                                                                               |
| ---------- | ----------------------------------------------------------------------------------------------------------------------- |
| Sản phẩm | Agent đề xuất cách tách một mục tiêu thành task nháp; người dùng duyệt rồi mới ghi vào dữ liệu thật |
| POC        | Kiểm chứng một điều: agent tách việc có chạy được và đủ tin cậy không                                  |
| Học tập  | Học cách dùng AI có kiểm soát và so sánh hai model (xem `docs/00-process/experiment-protocol.md`)             |

## Người dùng

- Người dùng chính `[Giả định]`: cá nhân hoặc nhóm nhỏ quản lý công việc theo dự án. (bạn điền)
- Vai trò `[Giả định]`: chủ dự án (tạo dự án, chấp nhận task nháp) và thành viên.



## Giá trị và KPI

### Giá trị kỳ vọng

- Với người dùng `[Giả định]`: bớt thời gian nghĩ và nhập danh sách task từ đầu, ít bỏ sót bước,
  vẫn giữ quyền quyết định nhờ task nháp.
- Với dự án: chứng minh hoặc bác bỏ bằng số liệu giả thuyết "agent tách việc đủ tin cậy ở mức
  L2", và học cách dùng AI có kiểm soát.

### KPI

`[Đề xuất]` Ngưỡng do con người chốt (S07, S11). Đặt trước khi chạy, hiệu chỉnh bằng 3 đến 5 ca
thử riêng, rồi khóa; muốn đổi thì tạo phiên bản mới và ghi lý do. Chỉ số của người dùng thật
(quay lại dùng, mức hài lòng) đặt khi chốt scope MVP (M2).

| KPI                                                                 | Cách đo                                                    | Ngưỡng POC                                                                                                      | Mục tiêu MVP                                                               | NFR liên quan      |
| ------------------------------------------------------------------- | ------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- | ------------------- |
| K1. Vi phạm ràng buộc cứng                                      | Số ca vi phạm, mọi ca và mọi lần chạy                 | 0                                                                                                                 | 0 trên bộ ca từ khoảng 30 ca                                             | (ràng buộc cứng) |
| K2. Task nháp được chấp nhận hoặc chỉ sửa nhẹ             | Task điểm 2 chia tổng task nháp                          | từ (bạn điền, gợi ý 60) phần trăm                                                                         | từ (bạn điền, gợi ý 80) phần trăm                                    | NFR-012             |
| K3. Thời gian từ mục tiêu đến đề xuất                      | Trung vị và ca chậm nhất trên bộ ca mẫu               | trung vị không quá (bạn điền, gợi ý 15) giây; ca chậm nhất không quá (bạn điền, gợi ý 60) giây | (đo lại với người dùng thật)                                          | NFR-010             |
| K4. Chi phí model mỗi lần chia việc                             | Từ nhật ký                                                | Chỉ ghi lại                                                                                                     | Ngân sách tháng (bạn điền) chia số lần dùng dự kiến (bạn điền) | NFR-011             |
| K5. Thời gian đến danh sách task chính thức, so với làm tay | Bấm giờ 3 đến 5 mục tiêu, tự làm so với dùng agent | Không dài hơn làm tay                                                                                         | Giảm từ (bạn điền, gợi ý 30) phần trăm                              | (không)            |
| K6. Số vòng tự sửa trung bình                                  | Từ nhật ký                                                | Trung bình không quá 2; không quá 1 trên 16 ca chạm 3 vòng                                                | (xét lại sau 2 đợt)                                                      | (không)            |

Lưu ý mẫu nhỏ: 0 vi phạm trên 16 ca chỉ cho phép kết luận "chưa thấy vi phạm", không kết luận
agent an toàn; MVP cần bộ ca lớn hơn.

## Phạm vi

Trong phạm vi (dự kiến cho MVP):

- Quản lý task cơ bản: tạo task, đổi trạng thái, đặt hạn chót, nhãn quá hạn.
- Agent đề xuất task nháp từ một mục tiêu; hỏi lại khi đầu vào mơ hồ hoặc không hợp lệ.
- Agent chỉ dùng công cụ được phép, không làm theo chỉ thị nằm trong nội dung, tự chạy tối đa 3 vòng.
- Mọi thay đổi do agent đề xuất chỉ có hiệu lực sau khi người dùng xác nhận.
- Nhật ký hành động của agent; phân quyền theo dự án.

Ngoài phạm vi `[Đề xuất]`:

- Agent đề xuất ưu tiên và theo dõi tiến độ (hoãn sau POC, sẽ là CR riêng).
- Nâng mức tự chủ của agent lên L3.
- Thông báo email, lịch, tích hợp dịch vụ ngoài, báo cáo, thanh toán, ứng dụng di động.

## Quy trình nghiệp vụ

Hiện tại `[Giả định]`: người dùng tự nghĩ danh sách việc, nhập từng task, tự đặt hạn chót và
theo dõi bằng tay.

Mong muốn:

1. Người dùng nhập một mục tiêu cho một dự án.
2. Nếu mục tiêu mơ hồ hoặc thiếu, agent hỏi lại.
3. Agent đề xuất các task nháp.
4. Người dùng sửa và chấp nhận từng task nháp; task được chấp nhận thành task chính thức.
5. Người dùng theo dõi trạng thái và hạn chót; task quá hạn được gắn nhãn.
6. Mọi hành động của agent được ghi nhật ký để xem lại.

## User story

- Là người dùng, tôi muốn nhập một mục tiêu và nhận danh sách task nháp để không phải nghĩ từ đầu.
- Là người dùng, tôi muốn agent hỏi lại khi mục tiêu mơ hồ để đề xuất không sai hướng.
- Là người dùng, tôi muốn chỉ task tôi chấp nhận mới vào danh sách chính thức để tôi kiểm soát.
- Là người dùng, tôi muốn đặt hạn chót và thấy task quá hạn để không bỏ sót việc.
- Là người dùng, tôi muốn xem agent đã làm gì để tin cậy và kiểm tra được.
- Là người dùng, tôi muốn dữ liệu dự án của mình không bị lộ sang dự án khác.

## Ràng buộc mức nghiệp vụ

- Dữ liệu: POC chỉ dùng dữ liệu giả; không đưa dữ liệu cá nhân thật vào AI (xem
  `docs/00-process/safety-rules.md`).
- Vùng nhạy cảm (phân quyền, dữ liệu cá nhân): AI tối đa L2 khi viết code.
- Agent: mức L2, chỉ đề xuất và tạo nháp; tối đa 3 vòng tự chạy.
- Pháp lý `[Giả định]`: chưa có yêu cầu pháp lý cụ thể cho POC. Nếu MVP lưu dữ liệu người dùng
  thật thì cần xem lại; việc diễn giải pháp lý do con người làm (S09).

## Rủi ro và giả định

| Rủi ro hoặc giả định                                     | Ảnh hưởng                     | Cách giảm                                                                           |
| ------------------------------------------------------------- | -------------------------------- | ------------------------------------------------------------------------------------- |
| Agent tách việc sai hoặc bịa                              | Người dùng mất tin tưởng   | Task nháp, người dùng duyệt, hỏi lại khi mơ hồ                               |
| Chỉ thị nằm trong nội dung người dùng nhập            | Agent làm việc ngoài ý muốn | Coi nội dung là dữ liệu; danh sách công cụ được phép; ca kiểm thử riêng |
| Agent đọc dữ liệu dự án không được phép            | Lộ dữ liệu                    | Phân quyền theo dự án; vùng nhạy cảm tối đa L2                               |
| Chi phí model vượt dự kiến                               | Không duy trì được          | Đo chi phí mỗi lần; đặt ngưỡng                                                |
| Mẫu thử nhỏ                                                | Kết luận chưa chắc           | Ghi rõ giới hạn trong scorecard                                                    |
| `[Giả định]` Người dùng chịu duyệt từng task nháp | Nếu sai, giá trị thấp        | Đo tỷ lệ chấp nhận trong POC                                                     |

## Stage và lộ trình

POC, rồi Demo, rồi MVP, rồi Production (xem `docs/00-process/gates.md`). MVP bắt buộc UAT và
cắt scope, không cắt chuẩn. Kế hoạch theo tuần: `docs/06-roadmap/plan.md`.

## Câu hỏi mở

- Người dùng thật là ai, và ai nghiệm thu ở G6?
- Ba nỗi đau thật là gì?
- Ngưỡng KPI nào đủ để coi POC thành công?
- MVP có cần đăng nhập không?
- Có cần sửa hoặc xóa task thủ công trong MVP không?

## Truy vết

- CR gốc: CR-001. BRD này là đầu vào của `docs/01-requirements/srs.md`.
- Yêu cầu và AC: `docs/01-requirements/acceptance-criteria.md`.

## Phiên bản

- v0.1, 11/11/2026: bản nháp do Claude soạn và có chỉnh sửa bởi tôi, chưa duyệt.
- v0.2, 12/11/2026: Chỉnh phần giá trị và KPI.
