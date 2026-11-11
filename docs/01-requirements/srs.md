
# Đặc tả yêu cầu phần mềm (SRS)

## Trạng thái

- Draft v0.1, do AI soạn (mức L2), chưa được tôi duyệt, chưa phải baseline.
- Sau G1, SRS thành baseline; mọi thay đổi sau đó phải đi qua CR mới
  (xem `docs/00-process/gates.md`).

Nguồn: tài liệu "Quy trình phát triển phần mềm có AI tham gia", mục 2.1 (P3, bước S10 đến S14).
Phần đánh dấu `[Đề xuất]` là bổ sung cho dự án này.

## Mục đích và phạm vi

Đặc tả yêu cầu của tool quản lý công việc có agent đề xuất task nháp. Bối cảnh, mục tiêu và
phạm vi: xem `docs/01-requirements/vision-brd.md`. Chưa chốt công nghệ ở SRS; việc đó thuộc P4.

## Vai trò

| Vai trò                       | Mô tả                                                                |
| ------------------------------ | ---------------------------------------------------------------------- |
| Chủ dự án `[Giả định]` | Tạo dự án, nhập mục tiêu, chấp nhận hoặc từ chối task nháp |
| Thành viên `[Giả định]` | Làm việc trong dự án mình thuộc về                              |
| Agent                          | Đề xuất task nháp; không tự ghi dữ liệu thật                  |

## Giả định và ràng buộc

- Chỉ dùng dữ liệu giả trong POC.
- Agent ở mức L2; tối đa 3 vòng tự chạy rồi dừng và báo người dùng.
- Vùng nhạy cảm (phân quyền, dữ liệu cá nhân): AI tối đa L2 khi viết code.
- Mọi yêu cầu có AC trong `docs/01-requirements/acceptance-criteria.md`.

## Yêu cầu chức năng (FR)

Mỗi FR một mã, một ý. Mức `must` hoặc `should`. Mọi FR truy về CR-001.

| Mã    | Yêu cầu                                                                                                                        | Mức | AC                                                                  |
| ------ | -------------------------------------------------------------------------------------------------------------------------------- | ---- | ------------------------------------------------------------------- |
| FR-010 | Hệ thống cho phép tạo task với tiêu đề bắt buộc                                                                        | must | AC-FR-010-1 đến 3                                                 |
| FR-011 | Hệ thống cho phép đổi trạng thái task giữa "Chưa làm", "Đang làm", "Xong" và ghi thời điểm đổi                 | must | AC-FR-011-1                                                         |
| FR-012 | Hệ thống gắn nhãn "Quá hạn" cho task có hạn chót đã qua và chưa "Xong"                                              | must | AC-FR-012-2 đến 5                                                 |
| FR-013 | Hệ thống cho phép đặt hạn chót cho task                                                                                   | must | AC-FR-012-1 (đổi mã thành AC-FR-013-1)                          |
| FR-020 | Agent đề xuất task nháp từ một mục tiêu của người dùng                                                               | must | AC-FR-020-1                                                         |
| FR-021 | Khi đầu vào mơ hồ hoặc không hợp lệ, agent hỏi lại hoặc hệ thống báo lỗi, không tạo task nháp                 | must | AC-FR-021-1 đến 3                                                 |
| FR-022 | Agent chỉ dùng các công cụ trong danh sách được phép                                                                   | must | AC-FR-022-1                                                         |
| FR-023 | Mọi thay đổi dữ liệu do agent đề xuất chỉ có hiệu lực sau khi người dùng xác nhận (gồm chấp nhận task nháp) | must | AC-FR-023-1; AC-FR-020-2 và 3 (đổi mã thành AC-FR-023-2 và 3) |
| FR-024 | Agent không làm theo chỉ thị nằm trong nội dung người dùng nhập                                                        | must | AC-FR-022-2 (đổi mã thành AC-FR-024-1)                          |
| FR-025 | Agent dừng và báo người dùng sau 3 vòng tự chạy mà chưa xong                                                          | must | AC-FR-022-3 (đổi mã thành AC-FR-025-1)                          |
| FR-030 | Hệ thống ghi nhật ký mỗi lần agent chạy: model và phiên bản, prompt, đầu vào, hành động, thời điểm            | must | AC-FR-030-1                                                         |
| FR-040 | Agent không đọc dữ liệu của dự án mà người dùng không thuộc về                                                    | must | AC-FR-040-1                                                         |

Sau khi duyệt SRS, đồng bộ lại `acceptance-criteria.md` theo cột AC (đổi mã AC nói trên và đổi
tên FR-010 từ "Tạo và sửa task" thành "Tạo task").

## Yêu cầu phi chức năng (NFR)

Ngưỡng do con người chốt (PO); các ô ngưỡng để trống.

| Mã     | Yêu cầu                                                                                | Cách đo                   | Ngưỡng                                                       | AC           |
| ------- | ---------------------------------------------------------------------------------------- | --------------------------- | -------------------------------------------------------------- | ------------ |
| NFR-010 | Thời gian từ lúc người dùng bấm "Nhờ agent chia việc" đến lúc có đề xuất | Đo trên bộ ca mẫu       | dưới (bạn điền) giây ở (bạn điền) phần trăm số ca | AC-NFR-010-1 |
| NFR-011 | Chi phí model mỗi lần chia việc                                                      | Từ nhật ký               | không quá (bạn điền)                                      | AC-NFR-011-1 |
| NFR-012 | Tỷ lệ task nháp người dùng chấp nhận hoặc chỉ sửa nhẹ                        | Từ nhật ký và scorecard | từ (bạn điền) phần trăm                                  | AC-NFR-012-1 |

## Truy vết

| FR hoặc NFR          | CR     | AC                       | Test case    |
| --------------------- | ------ | ------------------------ | ------------ |
| FR-010 đến FR-013   | CR-001 | Xem cột AC ở bảng FR  | (lập ở P6) |
| FR-020 đến FR-025   | CR-001 | Xem cột AC ở bảng FR  | (lập ở P6) |
| FR-030, FR-040        | CR-001 | Xem cột AC ở bảng FR  | (lập ở P6) |
| NFR-010 đến NFR-012 | CR-001 | Xem cột AC ở bảng NFR | (lập ở P6) |

Bảng truy vết đầy đủ: `docs/01-requirements/traceability.md`.

## Câu hỏi mở

- MVP có cần đăng nhập không? Nếu có, thêm FR và AC (vùng nhạy cảm, AI tối đa L2).
- Có cần sửa hoặc xóa task thủ công trong MVP không?
- Có cho mở lại task đã "Xong" không?
- Ngưỡng các NFR là bao nhiêu?
- Ai đóng vai khách hàng ở G1 và G6?

## Phiên bản

- v0.1, 11/11/2026: bản nháp do Claude và tôi soạn với chỉnh sửa, chưa duyệt.
