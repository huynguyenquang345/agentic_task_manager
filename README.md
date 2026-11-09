
# agentic_task_manager

Tool quản lý công việc theo hướng agentic AI, phát triển theo quy trình có AI tham gia (12 phase, 8 gate).

## Trạng thái hiện tại

| Mục                | Giá trị                                    |
| ------------------- | -------------------------------------------- |
| Product stage       | POC                                          |
| Phase hiện tại    | P1–P3 (Tiếp nhận, Phân tích, Đặc tả) |
| Gate kế tiếp      | G1 (Duyệt yêu cầu)                        |
| Code                | Chưa có code sản phẩm                    |
| Trạng thái README | Draft, v0.1                                  |

## Mục tiêu

Xây dựng một tool quản lý công việc trong đó AI Agent là một phần của sản phẩm: nhận yêu cầu công việc, tách thành module/task, đề xuất ưu tiên và theo dõi tiến độ. Ở giai đoạn POC, mục tiêu chỉ là kiểm chứng một điều: agent tách việc có chạy được và đủ tin cậy không. Dự án cũng so sánh kết quả của hai model (GPT-6 và Claude Sonnet 5.5) trên cùng một bài toán.

## Quy trình áp dụng

Mỗi thay đổi đi qua 12 phase (P1–P12) và 8 gate (G1–G8). Chưa có sign-off của người có thẩm quyền thì chưa đi tiếp.

Các nguyên tắc giữ nguyên trong mọi giai đoạn:

- AI chỉ tạo Draft. Kiểm tra tự động chuyển Draft sang Verified. Chỉ con người mới chuyển sang Approved.
- AI không bao giờ là Accountable (người sign-off).
- Maker ≠ checker: người tạo ra sản phẩm không tự duyệt chính sản phẩm đó.
- Mỗi yêu cầu (FR) có tiêu chí nghiệm thu (AC) và truy về một CR. Mỗi AC có ít nhất một test case.
- Không đưa dữ liệu thật hoặc secret vào repo hay vào prompt. POC và Demo chỉ dùng dữ liệu synthetic.
- Ghi lại mọi lần AI chạy (model, prompt, input, thời điểm).

## Trạng thái tài liệu

Cập nhật bảng này mỗi khi tài liệu đổi trạng thái. Chỉ ghi Approved khi đã có người khác review và duyệt.

| Tài liệu                      | Phase      | Trạng thái | Gate | Người review |
| ------------------------------- | ---------- | ------------ | ---- | -------------- |
| Tổng quan quy trình áp dụng | Nền tảng | Chưa tạo   |      |                |
| Danh mục yêu cầu ứng viên  | P1–P3     | Chưa tạo   |      |                |
| CR-001                          | P1         | Chưa tạo   |      |                |
| SRS (FR, NFR, AC)               | P3         | Chưa tạo   | G1   |                |
| Bảng traceability              | P3–P6     | Chưa tạo   |      |                |

Trạng thái dùng một trong: Chưa tạo, Draft, Verified, Approved.

## Cấu trúc repo

| Thư mục / file    | Mục đích                                                                   |
| ------------------- | ----------------------------------------------------------------------------- |
| `docs/`           | Tài liệu quy trình và yêu cầu                                           |
| `poc/`            | (Ghi rõ mục đích) Code POC là code bỏ đi, không đưa thẳng vào MVP |
| `labs/`           | (Ghi rõ mục đích)                                                         |
| `experiments/`    | (Ghi rõ mục đích)                                                         |
| `.github/`        | Cấu hình repo, PR template, CI                                              |
| `CODEOWNERS`      | Người duyệt các thay đổi theo thư mục                                 |
| `CONTRIBUTING.md` | Cách đóng góp và quy ước làm việc                                    |

## Đang chờ review

- [ ] Phạm vi POC và các mục Out of scope có hợp lý không
- [ ] Danh mục yêu cầu ứng viên: giữ, bỏ hoặc hoãn mục nào
- [ ] Ngưỡng NFR đề xuất (đang để TBD)
- [ ] Những gate nào cần người ngoài duyệt, những gate nào tự kiểm
