
# Truy vết (traceability)

## Mục đích

Liên kết hai chiều giữa yêu cầu, thiết kế, code, test và release, để biết mỗi thứ phục vụ yêu cầu
nào và mỗi yêu cầu đã được thiết kế, làm và kiểm thử chưa. File này là bằng chứng (ma trận truy
vết) cho người ký ở G1, G2 và G5.

Nguồn: tài liệu "Quy trình phát triển phần mềm có AI tham gia", bảng thuật ngữ (Traceability),
mục 1.3, 2.1 (S13), 2.2 (S19), 2.3 (V-model, S27), 2.5 (G5), 3.2, 4.2. Phần đánh dấu `[Đề xuất]` là
bổ sung cho dự án này, không có nguyên văn trong tài liệu gốc.

## Trạng thái

- Draft v0.1. Bảng 1 lập từ bản nháp `srs.md` và `acceptance-criteria.md` do AI soạn; mã FR và AC
  là dự kiến cho đến khi tôi duyệt SRS.
- Bảng 2 được điền dần theo phase (xem "Ai lập cột nào, khi nào").
- Chỉ chuyển sang Verified khi các kiểm tra ở mục "Kiểm tra" đạt. Chỉ tôi chuyển sang Approved.

## Chuỗi truy vết

Chuỗi liên kết: CR, rồi FR hoặc NFR, rồi AC, rồi thiết kế, rồi task, rồi code, rồi test case, rồi
kết quả test, rồi release.

| Liên kết            | Quy tắc (nguồn)                                                                  | Gate hoặc bước kiểm |
| --------------------- | ---------------------------------------------------------------------------------- | ----------------------- |
| FR truy về CR        | Mọi FR truy về một CR (mục 2.1)                                                | G1                      |
| FR có AC             | Mọi FR có AC (mục 2.1)                                                          | G1                      |
| FR có thiết kế     | Mọi FR có thiết kế; thiết kế không thuộc yêu cầu nào là finding (S19)  | S19, G2                 |
| Task truy về FR      | Mọi task trong backlog truy về một FR (mục 2.2)                                | G2                      |
| AC có test case      | Mỗi AC có ít nhất một test case (mục 2.3)                                    | S27, P6                 |
| Test case truy về AC | Mỗi test case truy về một AC (mục 2.3)                                         | S27, P6                 |
| AC mức must pass     | 100% AC mức must có test case pass (mục 2.5)                                    | G5                      |
| Release               | Bản phát hành truy về CR đã đóng và gate đã qua (bảng thuật ngữ, G8) | G7, G8                  |

Nguyên tắc V-model (mục 2.3): mỗi mức đặc tả có một mức kiểm thử tương ứng. MVP cắt scope,
không cắt chuẩn: thiếu traceability là không được (mục 3.2).

## Quy ước

- Mỗi thứ có một nơi gốc; file này chỉ chứa mã và liên kết, không chép nội dung. `[Đề xuất]`
  - CR: GitHub Issue (mã CR-001).
  - FR, NFR: `docs/01-requirements/srs.md`.
  - AC: `docs/01-requirements/acceptance-criteria.md`, mã `AC-<FR hoặc NFR>-<số>`.
  - Thiết kế: mục trong `docs/02-design/` hoặc mã quyết định (ví dụ ADR-0001 trong
    `docs/02-design/decisions/`).
  - Task: GitHub Issue (`#<số>`), ghi mã FR trong mô tả, nhãn `ai-eligible` hoặc `human-only`.
  - Code: pull request (`PR #<số>`), ghi `Closes #<số>` và FR, AC liên quan (theo mẫu PR).
  - Test case: mã `TC-<số>` và đường dẫn file chứa test. `[Đề xuất]`
  - Kết quả test: số lượt chạy CI hoặc đường dẫn báo cáo; thí nghiệm thì mã `EXP-...`.
  - Release: tag (ví dụ `v0.1.0`).
- Ô chưa có ghi dấu gạch ngang (`-`), không để trống, để script kiểm tra được.
- Bảng này cho tool chính. `labs/` và `poc/` là POC, có AC và ca kiểm thử riêng trong từng thư mục.

## Bảng 1: CR, yêu cầu, AC (lập ở P3, bằng chứng cho G1)

| CR     | FR hoặc NFR | Mức   | AC                                                 |
| ------ | ------------ | ------ | -------------------------------------------------- |
| CR-001 | FR-010       | must   | AC-FR-010-1; AC-FR-010-2; AC-FR-010-3              |
| CR-001 | FR-011       | must   | AC-FR-011-1                                        |
| CR-001 | FR-012       | must   | AC-FR-012-1; AC-FR-012-2; AC-FR-012-3; AC-FR-012-4 |
| CR-001 | FR-013       | must   | AC-FR-013-1                                        |
| CR-001 | FR-020       | must   | AC-FR-020-1                                        |
| CR-001 | FR-021       | must   | AC-FR-021-1; AC-FR-021-2; AC-FR-021-3              |
| CR-001 | FR-022       | must   | AC-FR-022-1                                        |
| CR-001 | FR-023       | must   | AC-FR-023-1; AC-FR-023-2; AC-FR-023-3              |
| CR-001 | FR-024       | must   | AC-FR-024-1                                        |
| CR-001 | FR-025       | must   | AC-FR-025-1                                        |
| CR-001 | FR-030       | must   | AC-FR-030-1                                        |
| CR-001 | FR-040       | must   | AC-FR-040-1                                        |
| CR-001 | NFR-010      | should | AC-NFR-010-1                                       |
| CR-001 | NFR-011      | should | AC-NFR-011-1                                       |
| CR-001 | NFR-012      | should | AC-NFR-012-1                                       |

## Bảng 2: AC, thiết kế, task, test case, code, kết quả

Điền dần. Dấu `-` nghĩa là chưa có. Mỗi AC một dòng; thêm dòng khi thêm AC.

| AC           | Thiết kế (P4) | Task (P5) | Test case (P6) | Code (P7 đến P8) | Kết quả (P9) | Ghi chú |
| ------------ | --------------- | --------- | -------------- | ------------------ | -------------- | -------- |
| AC-FR-010-1  | -               | -         | -              | -                  | -              |          |
| AC-FR-010-2  | -               | -         | -              | -                  | -              |          |
| AC-FR-010-3  | -               | -         | -              | -                  | -              |          |
| AC-FR-011-1  | -               | -         | -              | -                  | -              |          |
| AC-FR-012-1  | -               | -         | -              | -                  | -              |          |
| AC-FR-012-2  | -               | -         | -              | -                  | -              |          |
| AC-FR-012-3  | -               | -         | -              | -                  | -              |          |
| AC-FR-012-4  | -               | -         | -              | -                  | -              |          |
| AC-FR-013-1  | -               | -         | -              | -                  | -              |          |
| AC-FR-020-1  | -               | -         | -              | -                  | -              |          |
| AC-FR-021-1  | -               | -         | -              | -                  | -              |          |
| AC-FR-021-2  | -               | -         | -              | -                  | -              |          |
| AC-FR-021-3  | -               | -         | -              | -                  | -              |          |
| AC-FR-022-1  | -               | -         | -              | -                  | -              |          |
| AC-FR-023-1  | -               | -         | -              | -                  | -              |          |
| AC-FR-023-2  | -               | -         | -              | -                  | -              |          |
| AC-FR-023-3  | -               | -         | -              | -                  | -              |          |
| AC-FR-024-1  | -               | -         | -              | -                  | -              |          |
| AC-FR-025-1  | -               | -         | -              | -                  | -              |          |
| AC-FR-030-1  | -               | -         | -              | -                  | -              |          |
| AC-FR-040-1  | -               | -         | -              | -                  | -              |          |
| AC-NFR-010-1 | -               | -         | -              | -                  | -              |          |
| AC-NFR-011-1 | -               | -         | -              | -                  | -              |          |
| AC-NFR-012-1 | -               | -         | -              | -                  | -              |          |

## Bảng 3: Release

Lập ở P11 và P12.

| Release     | CR đã đóng | Gate đã qua | UAT | Ghi chú |
| ----------- | -------------- | ------------- | --- | -------- |
| (chưa có) | -              | -             | -   |          |

## Ai lập cột nào, khi nào

| Phase         | Việc                                                                      | Mức AI theo tài liệu gốc                                                     | Gate   |
| ------------- | -------------------------------------------------------------------------- | -------------------------------------------------------------------------------- | ------ |
| P3            | Lập Bảng 1 (CR, FR, AC)                                                  | L2 soạn nháp; kiểm tra yêu cầu tự động (S13)                             | G1     |
| P4            | Điền cột thiết kế; kiểm tra truy vết thiết kế và yêu cầu (S19) | L3: tìm yêu cầu chưa có thiết kế, thiết kế không thuộc yêu cầu nào | G2     |
| P5            | Điền cột task                                                           | L1 đến L2                                                                      | G2     |
| P6            | Điền cột test case; kiểm tra coverage (S27)                            | L3: tự động tính coverage matrix                                             |        |
| P7 đến P8   | Điền cột code (PR)                                                      | L2 đến L3                                                                      | G3     |
| P9            | Điền cột kết quả                                                      | L3                                                                               | G5     |
| P11 đến P12 | Điền Bảng 3                                                             | L2 đến L3                                                                      | G7, G8 |

AI giúp lập và kiểm tra, nhưng người ký chỉ approve khi đã thấy bằng chứng (mục 1.3); chỉ tôi ký.

## Bảng đổi mã AC (chờ tôi duyệt SRS)

`[Đề xuất]` `acceptance-criteria.md` bản nháp dùng mã cũ. Khi SRS được duyệt, đổi mã trong
`acceptance-criteria.md` theo bảng dưới, theo đúng thứ tự từ trên xuống (đổi AC-FR-012-1 trước, rồi
mới đánh số lại các AC còn lại), sau đó đổi tên FR-010 từ "Tạo và sửa task" thành "Tạo task".

| Mã cũ     | Mã mới    | Lý do                                       |
| ----------- | ----------- | -------------------------------------------- |
| AC-FR-012-1 | AC-FR-013-1 | Đặt hạn chót tách thành FR-013         |
| AC-FR-012-2 | AC-FR-012-1 | Đánh số lại                              |
| AC-FR-012-3 | AC-FR-012-2 | Đánh số lại                              |
| AC-FR-012-4 | AC-FR-012-3 | Đánh số lại                              |
| AC-FR-012-5 | AC-FR-012-4 | Đánh số lại                              |
| AC-FR-020-2 | AC-FR-023-2 | Chấp nhận task nháp thuộc FR-023         |
| AC-FR-020-3 | AC-FR-023-3 | Chấp nhận task nháp thuộc FR-023         |
| AC-FR-022-2 | AC-FR-024-1 | Chỉ thị nằm trong nội dung thuộc FR-024 |
| AC-FR-022-3 | AC-FR-025-1 | Giới hạn 3 vòng thuộc FR-025             |

## Kiểm tra

Điều kiện kiểm được của G1 (mục 2.1): 100% FR có AC, 100% FR truy về CR, không còn finding mức
blocker.

- [ ] G1: mọi FR và NFR trong `srs.md` có ít nhất một AC; Bảng 1 không có ô CR hoặc AC trống.
- [ ] G2: mọi FR có thiết kế (cột thiết kế ở Bảng 2 không còn `-`); mọi task truy về một FR.
- [ ] P6: mỗi AC có ít nhất một test case; mỗi test case truy về một AC.
- [ ] G5: mọi AC mức must có test case pass.

`[Đề xuất]` Kiểm bằng script (chạy ở thư mục gốc repo). Script thứ nhất liệt kê FR hoặc NFR
chưa có AC:

```powershell
$srs = Get-Content docs\01-requirements\srs.md -Raw
$ac = Get-Content docs\01-requirements\acceptance-criteria.md -Raw
[regex]::Matches($srs, '(N?FR)-\d+') | ForEach-Object { $_.Value } | Sort-Object -Unique |
  Where-Object { $ac -notmatch ('AC-' + [regex]::Escape($_) + '-') }
```

Script thứ hai liệt kê AC chưa có test case (cột Test case của Bảng 2 còn `-`):

```powershell
Get-Content docs\01-requirements\traceability.md | Where-Object { $_ -match '^\| AC-' } |
  ForEach-Object {
    $c = $_ -split '\|' | ForEach-Object { $_.Trim() }
    if ($c[4] -eq '-') { "$($c[1]): chưa có test case" }
  }
```

Kết quả rỗng nghĩa là đạt. Đây là kiểm tra tự động đơn giản (trạng thái Verified); nó không thay thế
việc tôi đọc và ký.

## Khi yêu cầu đổi

Theo mục 4.2: tạo CR mới; làm impact analysis (dùng Bảng 2 để tìm thiết kế, task và test bị ảnh
hưởng); cập nhật SRS lên phiên bản mới; ghi "cũ (stale)" vào cột Ghi chú của các test bị ảnh
hưởng; quay lại gate tương ứng.

## Phiên bản

- v0.1, 12/11/2026: bản nháp do Claude và tôi soạn theo tài liệu gốc, dựa trên srs.md và acceptance-criteria.md nháp.
