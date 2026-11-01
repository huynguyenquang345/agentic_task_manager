
# Hướng dẫn đóng góp

Tài liệu này mô tả cách làm việc trên repo `agentic_task_manager`. Dự án đi theo quy trình phát triển phần mềm có AI tham gia (12 phase, 8 gate), nên mọi thay đổi cần có dấu vết, có người review và không bỏ qua gate.

Trạng thái: Draft, v0.1.

## Nguyên tắc chung

- AI chỉ tạo Draft. Kiểm tra tự động chuyển Draft sang Verified. Chỉ con người mới chuyển sang Approved.
- Người tạo ra sản phẩm không tự duyệt chính sản phẩm đó (maker ≠ checker).
- Không commit dữ liệu thật, secret, token, khóa API hay file `.env`.
- Không commit tài liệu nội bộ gốc. Chỉ tóm tắt bằng lời của mình và ghi nguồn tham chiếu.
- Nội dung do AI soạn phải được người đọc lại và sửa trước khi commit.

## Quy trình làm việc

1. Tạo branch từ `main`.
2. Thay đổi nhỏ, mỗi branch một mục đích.
3. Chạy lint cho file Markdown.
4. Commit theo quy ước bên dưới.
5. Push branch và mở Pull Request.
6. Người review duyệt. Sau khi duyệt mới merge vào `main`.

## Đặt tên branch

Dùng dạng `loại/mô-tả-ngắn`, chữ thường, nối bằng dấu gạch ngang.

| Loại     | Dùng cho                         | Ví dụ                     |
| --------- | --------------------------------- | --------------------------- |
| `docs`  | Tài liệu, quy trình, yêu cầu | `docs/srs-draft`          |
| `feat`  | Tính năng mới                  | `feat/task-decomposition` |
| `fix`   | Sửa lỗi                         | `fix/prompt-parsing`      |
| `chore` | Cấu hình, CI, việc lặt vặt   | `chore/add-markdown-lint` |
| `exp`   | Thử nghiệm, POC                 | `exp/compare-models`      |

## Quy ước commit message

Dạng: `loại: mô tả ngắn` (dòng đầu dưới khoảng 72 ký tự). Phần chi tiết, nếu cần, viết sau một dòng trống.

Các loại dùng chung với tên branch: `docs`, `feat`, `fix`, `chore`, `exp`. Có thể thêm `test` cho thay đổi chỉ liên quan test.

Ví dụ:

```text
docs: thêm danh mục yêu cầu ứng viên cho POC
```

Mỗi commit nên liên quan đến một mã CR hoặc FR khi có (ví dụ `docs: thêm AC cho FR-001 (CR-001)`).

## Pull Request

Mô tả PR cần có:

- Thay đổi gì và vì sao.
- Mã CR, FR, AC liên quan (nếu có).
- Trạng thái tài liệu sau khi merge: Draft, Verified hoặc Approved.
- Xác nhận không có dữ liệu thật hoặc secret.
- Phần nào do AI soạn và đã được người đọc lại.

Mỗi PR do người khác review. Chỉ ghi Approved cho một tài liệu khi người review đã duyệt PR đó.

## Tài liệu và trạng thái

- Tài liệu yêu cầu và quy trình nằm trong `docs/`.
- Mỗi tài liệu có đầu file ghi trạng thái, phiên bản, tác giả, người review và ngày.
- Mọi yêu cầu (FR) có mã, có AC dạng Given-When-Then và truy về một CR.
- Khi yêu cầu đã qua G1, mọi thay đổi tiếp theo đi qua CR mới.
- Bảng trạng thái tài liệu trong `README.md` được cập nhật cùng PR.

## Quy tắc cho code

- `poc/`, `labs/`, `experiments/` là nơi thử nghiệm. Code ở đây là code bỏ đi, không đưa thẳng vào sản phẩm mà không viết lại và review.
- Chỉ dùng dữ liệu synthetic khi thử nghiệm.
- Vùng nhạy cảm (dữ liệu người dùng, xác thực, tích hợp hệ thống nội bộ): AI chỉ được tạo draft, người viết và review lại.
- Ghi lại mỗi lần AI chạy ở mức cần thiết: model, prompt, input, thời điểm.

## Lint và kiểm tra

- File Markdown được kiểm bằng `markdownlint` theo cấu hình trong `.markdownlint.json`.
- Chạy cả repo: `npx markdownlint-cli2 "**/*.md"`.
- Sửa hết cảnh báo trước khi mở PR.

## Review và sign-off

Danh sách người duyệt theo thư mục nằm trong `CODEOWNERS`. Người review kiểm tra:

- Nội dung đúng và đủ so với yêu cầu liên quan.
- Có truy vết (CR, FR, AC) khi áp dụng.
- Không có thông tin nhạy cảm hoặc nội bộ.
- Trạng thái tài liệu được ghi trung thực.
