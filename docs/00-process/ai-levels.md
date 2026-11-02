
# Mức tự chủ của AI (L0–L4)

## Mục đích

Quy định AI được làm gì, đến đâu, ở từng phần của dự án. Nguyên tắc chung:
việc càng rủi ro thì AI càng ít được tự quyết.

Nguồn: tài liệu "Quy trình phát triển phần mềm có AI tham gia", mục 1.4 và 4.1.

## Năm mức

| Mức | Tên            | AI làm gì                                                    | Ví dụ trong dự án này                                   |
| ---- | --------------- | -------------------------------------------------------------- | ------------------------------------------------------------ |
| L0   | Human only      | Không dùng AI                                                | Chốt ý định và phạm vi, ký các gate                  |
| L1   | Suggest         | Gợi ý, tra cứu, so sánh; con người tự làm              | Gợi ý câu hỏi làm rõ, so sánh phương án thiết kế |
| L2   | Draft           | Soạn nháp; con người đọc và sửa trước khi dùng      | Nháp BRD, SRS, AC, release note                             |
| L3   | Gated execution | Tự chạy một chuỗi việc, dừng ở gate để người duyệt | Sinh và chạy test, viết code cho task được phép       |
| L4   | Autonomous      | Chỉ việc rủi ro thấp mà máy kiểm chứng được         | Build, lint, secret scan trong CI                            |

## Ba trạng thái của sản phẩm do AI tạo

| Trạng thái | Nghĩa là                                              | Ai chuyển sang                 |
| ------------ | ------------------------------------------------------- | ------------------------------- |
| Draft        | Vừa tạo, chưa ai kiểm                               | Người hoặc AI tạo ra        |
| Verified     | Kiểm tra tự động theo quy tắc cố định đã pass | Hệ thống kiểm tra tự động |
| Approved     | Người có thẩm quyền đã xem và ký               | Chỉ con người                |

AI chỉ tạo ra Draft. Không có đường tắt từ Draft sang Approved.

## Ba nguyên tắc nền

1. AI không bao giờ là người chịu trách nhiệm cuối cùng (Accountable).
2. Quyết định về ưu tiên, phạm vi, chấp nhận rủi ro và nghiệm thu thuộc về con người.
3. Pass hay fail do kiểm tra tự động theo quy tắc cố định, không do AI tự thấy đạt.

## Quy tắc kiểm soát

- Maker không phải checker: AI tạo xong không tự chuyển sang Verified.
- Giới hạn theo vùng: vùng nhạy cảm (đăng nhập, dữ liệu cá nhân, thanh toán) tối đa L2.
- Giới hạn vòng tự sửa: AI sửa tối đa N vòng rồi báo con người. N của dự án này: (bạn điền).
- Không đưa dữ liệu cá nhân, khóa bí mật, dữ liệu khách hàng chưa che vào AI.
- Ghi nhật ký mọi lần dùng AI: model, prompt, đầu vào, thời điểm (xem docs/ai-log).

## Áp dụng trong dự án này

| Khu vực                                      | Mức tối đa | Lý do                                                                                                                                                                                             |
| --------------------------------------------- | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| docs/ chưa qua G1                            | L2            | Tài liệu gốc cho L1–L2 ở P1–P3; tôi viết bản thô trước, AI phản biện (L1) rồi soạn nháp phần tôi đã mô tả (L2), tôi duyệt; G1 là quyết định của tôi (L0)            |
| labs/ và poc/ (code bỏ đi, dữ liệu giả) | L3            | Tài liệu cho phép L3–L4 ở POC; chọn L3 vì viết code cần người duyệt (L4 chỉ cho việc máy kiểm chứng được); test trước, tối đa 3 vòng tự sửa, tôi đọc mọi thay đổi |
| Mã sản phẩm chính, vùng nhạy cảm       | L2            | Theo mục 4.1                                                                                                                                                                                      |
| Mã sản phẩm chính, task ai-eligible       | L3            | Sau G2, có test và review                                                                                                                                                                        |
| CI (build, lint, quét bí mật)              | L4            | Máy kiểm chứng được                                                                                                                                                                          |

## Mức tự chủ của agent trong sản phẩm

Chi tiết nằm ở docs/02-design/agent-autonomy.md. Nguyên tắc khởi đầu:
agent trong tool chỉ đề xuất và tạo nháp (L2), người dùng duyệt mới ghi vào dữ liệu thật.
Mọi lần nâng mức tự chủ là một yêu cầu thay đổi (CR) riêng, có kiểm thử và duyệt.
